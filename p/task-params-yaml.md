---
layout: default
---

- [Index](/index)

## Table of contents

- [Definition](#definition)
- [Structure](#structure)



### Definition

When snippet is being invoked it will be supplied with an absolute path to parameter file. 
This parameter will always be the last parameter in the list of parameters which snippet will receive.

### Structure

This parameter file has the follow structure:   

```markdown
taskYaml:
  inputResourceAbsolutePaths:
    mainDocument:
    - /sandbox/git/mh/mh-station/task/localhost-8080/11/6691/DATA/uploaded-file-17064922919341969220-264882960313000.bin
  inputResourceCodes:
    mainDocument:
    - uploaded-file-17064922919341969220-264882960313000.bin
  outputResourceAbsolutePath: /sandbox/git/mh/mh-station/task/localhost-8080/11/6691/artifacts/1449-1-fake-snippet-1_1-simple-app-with-timeout-0
  outputResourceCode: 1449-1-fake-snippet-1_1-simple-app-with-timeout-0
  realNames:
    uploaded-file-17064922919341969220-264882960313000.bin: 81509 some file name.xml
  resourceStorageUrls:
    uploaded-file-17064922919341969220-264882960313000.bin:
      sourcing: launchpad
    1449-1-fake-snippet-1_1-simple-app-with-timeout-0:
      sourcing: launchpad
  snippet:
    checksum: '{"checksums":{"SHA256":"6e8ac7a2d3c6c6ea5ac01cf9ca678106f6fc61b045fd0a045580b54908c1d731"}}'
    code: fake-snippet:1.1
    env: fake
    info:
      length: 0
      signed: false
    metas:
    - key: task-params-version
      value: '3'
    metrics: false
    skipParams: false
    sourcing: station
    type: fileless
  timeoutBeforeTerminate: 2
  workingPath: /sandbox/git/mh/mh-station/task/localhost-8080/11/6691
version: 3
```

Top-level fields in params-v3.yaml:   
- version - The current version of format of params.yaml file.   
- taskYaml - The holder of all fields, except field 'version'
      
Fields in taskYaml:      
- inputResourceAbsolutePaths - A dictionary of absolute paths to resources. 
   Key is name group, value - absolute path.
- inputResourceCodes - A dictionary of resource files. The key is name group, the value - name of resource file. 
   Names of file is auto-generated and mean nothing. 
   You should use the key of this dictionary to understand which file is which.
- outputResourceAbsolutePath - An absolute path to result file. 
   Whatever your snippet is doing the result of snippet must be written to this file.    
- outputResourceCode - The name of result file. 
- realNames - A dictionary of mapping resouce code to the real file name. 
   In some cases you need to know the actual name of file and you can find it in this dictionary.
- resourceStorageUrls - A dictionary of types of resources' sourcing. 
   The key is resource code, the value - sourcing type. Right now there are three type of sourcing:   
   - launchpad - resources will be provisioned from launchpad by MH
   - git - resources will be provisioned from git repository by MH
   - disk - resources already has been deployed locally at station

- snippet - A config of current snippet. In most cases isn't using. 
  The fields is the same as in config of snippet - see [documentation on snippet's config](snippet.md)  
- timeoutBeforeTerminate - A timeout before the execution of snippet will be terminated. Value in seconds. 
  If this field is omitted or value is 0, then execution will last as long as snippet needs to finish its work.
- workingPath - A current working dir. A snippet will be started in this directory.
     
