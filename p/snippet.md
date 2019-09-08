---
layout: default
---

- [Index](/index)

## Table of contents

- [Definition](#definition)
- [Configuration](#configuration)
- [Metadata](#metadata)
- [Packaging](#packaging)
- [Uploading](#uploading)


### Definition

A processing module which is configured, packaged, uploaded to the Launchpad, and is being run at Station is a Snippet.
Snippet can be executable application, .jar file, python notebook, 
file in any scripting language. It can be 'curl' command to request external url, send e-mail and so on.
  

From a delivery point of view, the Snippet can be provisioned from Launchpad or from git repository.

### Configuration

To upload snippet to Launchpad, the Snippet have to configured with snippet.yaml file


snippet.yaml for the case with external .py file:
```yaml
snippets:
- code: simple-snippet-1.2
  env: python-3
  file: some-python-file.py
  params: param1 param2 param3
  metas:
  - key: mh.task-params-version
    value: '3'
  metrics: false
  skipParams: false
  sourcing: launchpad
  type: simple-snippet
```

So, how does it work?
python3 is a string reference to environment which is configured at Station side. Such configuration is done in env.yaml file 
I.e.
```yaml
envs:
  python-3: /anaconda3/envs/python-3.6/python
```

And final command to execute snippet at Station side will be 
```text
  /anaconda3/envs/python-3.6/python some-python-file.py param1 param2 param3
```

Fields in snippet.yaml:   
- code - string representation of code of concrete snippet must be unique within Launchpad   
- env - string reference to environment   
- file - name of file which must be provisioned along side with snippet.yaml   
- params - parameters for file, which is specified in field 'file'   
- metas - dictionary for key-value based configuration parameters   
-- mh.task-params-version - the version of format of params.yaml file which 
 is the last parameter for executable file (skipParams must be false)   
 
> - mh.task-params-version defines which format must be used for params.yaml file which is provided to snippet 
 as the last parameter in command line (skipParams is false). For details see [description of task's params.yaml config](description-of-task-params-yaml.md)   
   
 
- metrics - boolean, true/false, should we collect metrics after executing this snippet?   
- skipParams - boolean, true/false, should params.yaml file be omitted?   
- sourcing - enum, possible values - launchpad, station, git
--        launchpad - snippet will be downloaded from launchpad   
--        station - snippet already has been deployed locally at station   
--        git - snippet will be downloaded from git   

- type - a freely defined field. There are only two cases when this field must be defined in certain way:   
-- fit - this snippet is for fitting   
-- predict - this snippet is for predicting   


snippet.yaml for the case when scripting file is embedded in config:
```yaml
snippets:
  - code: simple-metrics.fit:1.2
    type: fit
    env: python-3
    sourcing: station
    metas:
      - key: mh.task-params-version
        value: 3
      - key: mh.snippet-params-as-file
        value: true
      - key: mh.snippet-params-file-ext
        value: .py
    params: |+
      import sys
      from datetime import datetime

      print(sys.argv)
      print(str(datetime.now()))
```

Fields in this snippet.yaml almost the same as the previous one, except the follow:      
- sourcing - station . This value tells Station that it doesn't need to download Snippet from Launchpad   
- params - contains the code for executing. Will be stored in file and 
 this file will be executed in defined environment(env field)    
- metas - for this case two additional metas must be configured   
-- mh.snippet-params-as-file - true, tells that the params field must be evaluated as scripting file   
-- mh.snippet-params-file-ext - extension for scripting file which will be created at Station side   



snippet.yaml for the case when snippet will be provisioned from git repository:
```yaml
snippets:
  - code: simple-metrics.fit:1.2
    type: fit
    file: examples/simple-metrics/snippets/simple-metrics-fit.py
    env: python-3
    sourcing: git
    metas:
      - key: mh.task-params-version
        value: 3
    git:
      repo: https://github.com/sergmain/metaheuristic-assets.git
      branch: master
      commit: 5d809f50e3ccb45688deefd9e1e70f6202d8d18a
```
 
For using git as source of snippet two fields must be defined:            
- sourcing - git . This value tells Station that it snippet is stored in git repository   
- git - definition of git, the follow additional fields must be specified:     
-- repo - url to git repo   
-- branch - name of branch which will be used   
-- commit - sha256 representation of commit in current branch    


### Metadata
Metadata is using for configuring a snippet. 
Metadata as solution for flexible configuration of snippet was chosen because in this case we can upgrade 
the code of Metaheuristic without loosing backward compatibility. 

Declaration of metadata in snippet's config is follow:
```yaml
snippets:
  - code: simple-metrics.fit:1.2
    metas:
      - key: mh.task-params-version
        value: 3
      - key: mh.snippet-supported-os
        value: linux, macos
      - key: mh.snippet-params-as-file
        value: true
      - key: mh.snippet-params-file-ext
        value: .py
```

Current list of metadatas includes: 
- mh.task-params-version -  defines which version of params.yaml must be used for invoking snippet. 
    If requested version is below of current default version, a downgrade will be tried
- mh.snippet-params-as-file - defines that the field 'params' contains actual code of script and 
    must be stored in temporary file. 
- mh.snippet-params-file-ext - if meta 'mh.snippet-params-as-file' is defined, extension of temporary file can be specified if needed.
- mh.snippet-supported-os - if Metaheuristic's stations installed in heterogeneous environment (i.e. on windows OS and on Linux ), target OS can be specified.
 Possible values are - windows, linux, macos. Values can be combined in comma-separated list. 
  

### Packaging

In some cases snippet has to be packaged before uploading to Launchpad:   
- snippet is an external file (like .py, .jar, and so on)    
- snippet has to be signed so Launchpad can verify legibility of snippet's source 

An application 'package-snippet' is used for packaging purpose. For details see [Package snippet](package-snippet) page.

Default steps for packaging snippet are:   
- create temporary folder   
- in that temp folder, create file snippets.yaml and configure desired parameters   
- from this temp folder, run the follow command  

java -jar /mh/git/apps/package-snippet/target/package-snippet.jar snippet.zip \[path to private key file\]   

- first parameter is the name of resulted .zip archive (in this example it's snippet.zip)   
- second parameter is optional and is used only when the snippet has to be signed.   
\[path to private key file\] must point to a valid private key, i.e. /mh/git/private-key.txt

For details how to generate public and privat keys, see [Gen keys](gen-keys) page   


### Uploading
After snippet is prepared in form of snippet.yaml or snippet.zip it has to be uploaded to Launchpad.  

This can be done via web interface at url [http://localhost:8080/launchpad/snippet/snippets]()

Other way to upload snippet is using REST-based url:      
```text   
curl -u q:123 -F "file=@simple-metrics-snippets-as-one-file.yaml"  http://localhost:8080/rest/v1/launchpad/snippet/snippet-upload-from-file
```
