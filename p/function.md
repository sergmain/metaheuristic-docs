---
layout: default
---

# Function

- [to Index](/index)

## Table of contents

- [Schemas](#schemas)
- [Definition](#definition)
- [Configuration](#configuration)
- [Metadata](#metadata)
- [Packaging](#packaging)
- [Uploading](#uploading)


### Schemas

Function schema v1 - [https://docs.metaheuristic.ai/schemas/function-schema-v1.json](https://docs.metaheuristic.ai/schemas/function-schema-v1.json)  
Function schema v2 - [https://docs.metaheuristic.ai/schemas/function-schema-v2.json](https://docs.metaheuristic.ai/schemas/function-schema-v2.json)  


### Definition

A processing entity which is configured, packaged, and uploaded to the Dispatcher, and is being run at Processor is a Function.
Function can be executable application, .jar file, python notebook, 
file in any scripting language. It can be 'curl' command to request external url, send e-mail and so on.
  

From a delivery point of view, the Function can be provisioned from Dispatcher or from git repository.

### Configuration

Before uploading of Function to Dispatcher, the Function has to be configured with function.yaml file


sunction.yaml for the case with external .py file:
```yaml
functions:
- code: simple-function-1.2
  env: python-3
  file: some-python-file.py
  params: param1 param2 param3
  metas:
  - mh.task-params-version: 1
  skipParams: false
  sourcing: dispatcher
  type: simple-function
```

How does it work?
python3 is a string reference to environment which is configured at Processor side. Such a configuration is done in [env.yaml file](/p/description-of-env-yaml) 
I.e.
```yaml
envs:
  python-3: /anaconda3/envs/python-3.6/python
```

The final command to execute Function at Processor side will be 
```text
  /anaconda3/envs/python-3.6/python some-python-file.py param1 param2 param3 <full-path-to-file>/params.yaml
```

Fields in function.yaml:   
- code - string representation of code of concrete Function must be unique within Dispatcher   
- env - string reference to environment   
- file - name of file which must be provisioned alongside with function.yaml   
- params - parameters for a file, which is specified in the field 'file'   
- metas - dictionary for key-value based configuration parameters   
-- mh.task-params-version - the version of format of params.yaml file which 
 is the last parameter for executable file (skipParams must be false)   
 
> - mh.task-params-version defines which format must be used for params.yaml file which is provided to snippet 
 as the last parameter in command line (skipParams is false). For details see [description of task's params.yaml config](description-of-task-params-yaml)   
   
 
- skipParams - boolean, true/false, should params.yaml file be omitted?   
- sourcing - enum, possible values - dispatcher, processor, git
   -  dispatcher - Function will be downloaded from Dispatcher   
   - processor - Function already has been deployed locally at Processor   
   - git - snippet will be downloaded from git   
- type - a freely defined field   


function.yaml for the case when scripting file is embedded in a config:
```yaml
functions:
  - code: simple-metrics.fit:1.2
    type: fit
    env: python-3
    sourcing: processor
    metas:
      - mh.task-params-version: 1
      - mh.function-params-as-file: true
      - mh.function-params-file-ext: .py
    params: |+
      import sys
      from datetime import datetime

      print(sys.argv)
      print(str(datetime.now()))
```

Fields in this function.yaml almost the same as the previous one, except the follow:      
- sourcing - processor . This value tells Processor that it doesn't need to download Function from Dispatcher   
- params - contains the code for executing. Will be stored in file and 
 this file will be executed in defined environment(env field)    
- metas - for this case two additional metas must be configured   
-- mh.function-params-as-file - true, tells that the params field must be evaluated as scripting file   
-- mh.function-params-file-ext - extension for scripting file which will be created at Processor side   



function.yaml for the case when Function will be provisioned from git repository:
```yaml
functions:
  - code: simple-metrics.fit:1.2
    type: fit
    file: examples/simple-metrics/functions/simple-metrics-fit.py
    env: python-3
    sourcing: git
    metas:
      - mh.task-params-version: 1
    git:
      repo: https://github.com/sergmain/metaheuristic-assets.git
      branch: master
      commit: 5d809f50e3ccb45688deefd9e1e70f6202d8d18a
```
 
For using git as source of Function, two fields must be defined:            
- sourcing - git . This value tells Processor that Function is stored in git repository   
- git - definition of git, the following additional fields must be specified:     
-- repo - url to git repo   
-- branch - name of branch which will be used   
-- commit - sha256 representation of commit in current branch    


### Metadata
Metadata is being used for configuring a Function. 
Metadata as a solution for a flexible configuration of Function was chosen because in this case we can upgrade 
the code of Metaheuristic without loosing backward compatibility. 

Declaration of metadata in Function's config is as follows:
```yaml
functions:
  - code: simple-metrics.fit:1.2
    metas:
      - mh.task-params-version: 1
      - mh.snippet-supported-os: linux, macos
      - mh.snippet-params-as-file: true
      - mh.snippet-params-file-ext: .py
```

Current list of metadatas includes: 
- mh.task-params-version -  defines which version of params.yaml must be used for invoking Function. 
    If requested version is below of current default version, a downgrade will be tried
- mh.function-params-as-file - defines that the field 'params' contains actual code of script and 
    must be stored in temporary file. 
- mh.function-params-file-ext - if meta 'mh.function-params-as-file' is defined, extension of temporary file can be specified if needed.
- mh.function-supported-os - if Metaheuristic's Processors are installed in heterogeneous environment (i.e. on windows OS and on Linux ), target OS can be specified.
 Possible values are - windows, linux, macos. Values can be combined in comma-separated list. 
  

### Packaging

In some cases a Function has to be packaged before uploading to Dispatcher:   
- Function is an external file (like .py, .jar, and so on)    
- Function has to be signed so Dispatcher can verify legibility of Function's source 

An application 'package-function' is used for packaging purpose. For details see [Package a function](package-a-function) page.

Default steps for packaging Functions are:   
- create temporary folder   
- in that temp folder, create file functions.yaml and configure desired parameters   
- from this temp folder, run the following command  

```
java -jar /mh-root/metaheuristic/apps/package-function/target/package-function.jar function.zip <path to private key file>   
```
> the path '/mh-root/metaheuristic' is related to directory structure as described on [Full installation](full-installation) page. 

- first parameter is the name of resulted .zip archive (in this example it's function.zip)   
- second parameter is optional and is used only when the Function has to be signed.   
\<path to private key file\> must point to a valid private key, i.e. /mh-root/metaheuristic/private-key.txt

> the path '/mh-root/metaheuristic' is related to directory structure as described on [Full installation](full-installation) page. 

For details how to generate public and private keys, see [Gen keys](gen-keys) page   


### Uploading
After a Function is prepared in form of function.yaml or function.zip, it has to be uploaded to Dispatcher.  

This can be done via web interface at url [http://localhost:8080/dispatcher/function/functions]()

Other way to upload a Function is to use a REST-based url:      
```text
curl -u q:123 -F "file=@simple-metrics-functions-as-one-file.yaml"  http://localhost:8080/rest/v1/dispatcher/function/function-upload-from-file
```
