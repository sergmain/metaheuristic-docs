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

```yaml
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