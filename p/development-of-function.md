---
layout: default
---

# Development of Function 

- [to Index](/index)

## Table of contents

- [Definition](#definition)


### Definition

[Function](/p/function) is an executable code which can be run from command-line. It can be python script, 
Spring Boot based application, curl to invoke specific url, and others.

Function has the following life-cycle stages:
- Coded, [packaged](/p/package-a-function) for uploading to Dispatcher
- stored at Dispather
- provisioned to Processor
- executed by Processor


Before execute a Function, Processor is preparing directory structure in which a Function will be executed.

Execution of Function is a Task in terminology of Metaheuristic.

Task has its own directory with following structure:

Root director for Task:
\<directory specified in property ''mh.processor.dir'\>/task/\<code of dispatcher's url\>/0/0001


inside the Task directory there are the following directories:
- artifacts - dir where user has to create any assets while Function is being executed.
- system - dir where Metaheuristic is storing output to console.
- global_variable - dir where Global variabled are stored before executing a Function.
- variable - input variables for this FUnction
-    
  