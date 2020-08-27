---
layout: default
---

# Description of application.properties

## Table of contents

- [Definition](#definition)
- [Structure](#structure)

- [to Index](/index)


### Definition

When Processor is being invoked it will be supplied with an absolute path to parameter file. 
This parameter will always be the last parameter in the list of parameters which snippet will receive.

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

Top-level fields in params.yaml:   
- version - The current version of format of params.yaml file.   
- task - The holder of all fields, except field 'version'
      
Fields in taskYaml:      