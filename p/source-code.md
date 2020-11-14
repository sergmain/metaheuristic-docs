---
layout: default
---

# Source code

## Table of contents

- [General Source code](#general-source-code)
- [Batch specific Source code](#batch-specific-source-code)
- [Experiment specific Source code](#experiment-specific-source-code)

- [to Index](/index)

## General Source code


## Batch specific Source code

````yaml
source:
  uid: source-code-for-batch-processing-1.4
  variables:
    startInputAs: input-data
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
      cache:
        enabled: true
      tags: tag1, tag2        
      priority: -1
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
                type: batch-item-processed-file
              - name: var-processing-status-1
                type: batch-item-processing-status
              - name: var-item-mapping-1
                type: batch-item-mapping
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
        - result-file-extension: .zip
version: 2
````

For batch processing you have to use two specific internal functions:
 - mh.batch-splitter 
 - mh.batch-result-processor


## Experiment specific Source code
