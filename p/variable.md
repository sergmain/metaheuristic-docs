---
layout: default
---

# Variable

## Table of contents

- [Definition](#definition)
- [Creation](#creation)

- [to Index](/index)

### Definition

Variable in Metaheuristic is any information which stored in Metaheuristic's database and has its name and type (optionally) .

Variables can be just Variable or Global variable:
- the scope of Variable is concrete Execution Context
- Global variables are being shared between all Execution Contexts   

### Creation

Global variable can be created at web console of Dispatcher - [http://localhost:8080/dispatcher/variable/variables]()

There are two options how to create Global variable:
- by uploading file and assigning name
- by uploading descriptor which defines Global variable on external storage. 
  Metaheuristic doesn't control content of such Global variables. 
  
  
Global variable and Variable can be configured with external sourcing:
- git, variable will be initialized by check outing and Processor side 

 ```yaml
sourcing: git
git:
  repo: https://github.com/sergmain/metaheuristic.git
  branch: master
  commit: b25331edba72a1a901634212ac55752238fd2dd5
 ```

- disk, content of variable is supposed to be provided as file and must be accessible for Processor 
```yaml
sourcing: disk
disk:
  code: storage-code
  mask: '*'
```  

Field disk/code must have the value which is specified as metadata disk/code in [env.yaml file](/p/description-of-env-yaml#structure).
For supporting the definition of variable which is showed above, env.yaml must be like this:

```yaml
disk:
- code: storage-code
  path: /dataset/input-dir
envs:
  python-3: python3
  java-11: java -jar
mirrors:
  https://github.com/sergmain/metaheuristic.git: /git-mirror/mh/metaheuristic.git
```



   

