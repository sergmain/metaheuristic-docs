---
layout: default
---

# Case: Batch example

## Table of contents

- [Description](#Description)
- [Implementation](#Implementation)

- [to Index](/index)
- [Back to Cases](/p/c/cases)


### Description

This is an example of batch processing implemented with Metaheuristic.

### Implementation

Metaheuristic has simple application called simple-batch-app. 
You can find a code of this application here - [https://github.com/sergmain/metaheuristic/tree/master/apps/simple-batch-app]()

To implement this example with Metaheuristic you need to do the following steps:

1. Add to [env.yaml](/p/description-of-env-yaml.md) a mapping for simple-batch-application:

````yaml
envs:
  batch-cmd: java -jar /mh-root/apps/simple-batch-app/target/simple-batch-app.jar
````

We suppose that you have installed Metaheuristic locally. 
If you want to try batch example with quick-start 
you have to package simple-batch-app as [Function](/p/function.md) and upload it with web interface.

2. At [http://localhost:8080/dispatcher/source-code/source-code-add]() create new [Source code](/p/source-code.md) with following content:

```yaml
source:
  uid: source-code-for-batch-processing-1.6
  variables:
    startInputAs: input-data
  metas:
    - mh.result-file-extension: .zip
  processes:
    - code: mh.batch-splitter
      name: mh.batch-splitter
      function:
        code: mh.batch-splitter
        context: internal
      metas:
        - variable-for-splitting: input-data
        - output-is-dynamic: true
        - output-variable: var-batch-item
      subProcesses:
        logic: sequential
        processes:
          - code: batch-1
            name: batch 1
            function:
              code: batch-function:1.0
            inputs:
              - name: var-batch-item
                array: true
            outputs:
              - name: var-processed-file-1
                type: processed-file-type-1
              - name: var-processed-file-2
                type: processed-file-type-2
              - name: var-processing-status
                type: processing-status-type
              - name: var-item-mapping
                type: mapping-type
    - code: mh.batch-result-processor
      name: batch result processor
      function:
        code: mh.batch-result-processor
        context: internal
      outputs:
        - name: var-batch-result
          type: batch-result
        - name: var-batch-status
          type: batch-status
      metas:
        - batch-type-processed-file: processed-file-type-1, processed-file-type-2
        - batch-type-processing-status: processing-status-type
        - batch-type-mapping: mapping-type
version: 1
```  

2. Package 'simple-batch-app' [Function]() - [Packaging of Function](/p/function#packaging)

```yaml
functions:
  - code: batch-function:1.0
    type: batch-processing
    sourcing: processor
    env: batch-cmd
``` 

Or you can use a pre-packed [functions.zip](/bin/functions.zip) 
if you turned off a signature checking in [dispatcher.yaml](/p/description-of-dispatcher-yaml)

```yaml
 - signatureRequired: false
``` 

3. Upload the packaged Function at [http://localhost:8080/dispatcher/function/functions]()

3. At [http://localhost:8080/dispatcher/batch/batch-add]() create a new Batch. 
As a file you can select any file which contains at least one byte. 
The processing of newly created Batch will be started automatically. 

4. You can see the progress of execution of Batch at [http://localhost:8080/dispatcher/source-code/source-codes]():
- press 'Exec Contexts' button
- press 'Task states' button

5. When a status of batch has changed to 'Finished', 
you will be able to download result archive and see status.txt t [http://localhost:8080/dispatcher/batch/batches]().
