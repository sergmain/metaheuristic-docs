---
layout: default
---

# Description of params.yaml for Task

## Table of contents

- [Definition](#definition)
- [Structure](#structure)

- [to Index](/index)


### Definition

When Function is being invoked it will be supplied with an absolute path to parameter file. 
This parameter will always be the last parameter in the list of parameters which Function will receive.

### Structure

This parameter file has the follow structure:   

```yaml
task:
  clean: false
  execContextId: 1373
  inline: {
  }
  inputs:
    - dataType: variable
      id: 5700
      name: var-batch-item
      sourcing: dispatcher
  outputs:
    - dataType: variable
      id: 5701
      name: var-processed-file-1
      sourcing: dispatcher
      type: batch-item-processed-file
    - dataType: variable
      id: 5702
      name: var-processing-status-1
      sourcing: dispatcher
      type: batch-item-processing-status
    - dataType: variable
      id: 5703
      name: var-item-mapping-1
      sourcing: dispatcher
      type: batch-item-mapping
  workingPath: /sandbox/git/mh/mh-processor/task/localhost-8080/11/6691
version: 1
```

Top-level fields in params-v3.yaml:   
- version - The current version of format of params.yaml file.   
- task - The holder of all fields, except field 'version'
      
Fields in task:      
- clean - delete all files after finishing processing of Task . 
- execContextId - id of current execContext. 
- inline - key-value dictionary which is configured in SourceCode. 
- inputs - A list of input Variables. 
- outputs - A list of input Variables. 
- realNames - A dictionary of mapping resouce code to the real file name. 
   In some cases you need to know the actual name of file and you can find it in this dictionary.
- sourcing - type of Variables' sourcing, possible values are dispatcher, git, disk. 
   - dispatcher - Variable will be provisioned from Dispatcher by MH
   - git - Variable will be provisioned from git repository by MH
   - disk - Variable already has been deployed locally or on shared resource and is accessible at Processor

- workingPath - A current working dir. A processing of Task will be started in this directory.
     
