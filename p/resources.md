---
layout: default
---

- [Index](/index)

## Table of contents

- [Definition](#definition)
- [Creation](#creation)

### Definition

Resource in Metaheuristic is any information which stored in Metaheuristic's database and has its own code and pool code.

Pool code is using to group resources in pool. When [Snippet](/p/snippet) is executed, 
pool code will be used to decide which resources need to be provided to snippet.

For example, we have historical data in number of files, i.e. year-by-year. 
While uploading those files we will specify pool code, i.e. 'historical-dataset'
Then we can transform all files which are loaded with this pool code into one dataset file 
and then this result of transforming will eb sended to other snippet as input resource.  

### Creation

There are [two forms of uploading resources](http://localhost:8080/launchpad/resource/resources)

The first one is for uploading files and assign pool code (required) and code (optional) for uploaded resource.

The second one is for defining resources which isn't controlled by Metaheuristic and 
stored somewhere else.
Right now two type of remote sourcing of resources are supported - 'git' and 'disk'
 -- git - resources is in a git repository and Station has to clone repository before accessing that resource
 
 ```yaml
sourcing: git
git:
  repo: https://github.com/sergmain/metaheuristic.git
  branch: master
  commit: b25331edba72a1a901634212ac55752238fd2dd5
 ```

  -- disk - resources are stored on shared devices (i.e. shared dir)
```yaml
sourcing: disk
disk:
  code: storage-code
  mask: '*'
```  

Field disk/code must have the value which is specified as metadata disk/code in [env.yaml file](/p/description-of-env-yaml#structure).
For supporting the yaml config which is showed above, env.yaml must be like this:

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



   

