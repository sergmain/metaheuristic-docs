---
layout: default
---

# Description of env.yaml config file, version 2

## Table of contents

- [Schemas](#schemas)
- [Definition](#definition)
- [Structure](#structure)

- [to description of env.yaml, version 1](/p/description-of-env-yaml-v1)
- [to Index](/index)


### Schemas

Env schema v1 - [https://docs.metaheuristic.ai/schemas/env-schema-v1.json](https://docs.metaheuristic.ai/schemas/env-schema-v1.json)  
Env schema v2 - [https://docs.metaheuristic.ai/schemas/env-schema-v2.json](https://docs.metaheuristic.ai/schemas/env-schema-v2.json)
Env schema v3 - [https://docs.metaheuristic.ai/schemas/env-schema-v3.json](https://docs.metaheuristic.ai/schemas/env-schema-v3.json)

### Definition

When Function is being invoked at Processor side, it will be supplied with an absolute path to parameter file. 
This parameter file will always be the last parameter in the list of parameters which Function will receive.

### Structure

This parameter file has the following structure:   

```yaml
version: 2
disk:
- code: input-dir
  path: /dataset/input-dir
envs:
  python-3: python3
  java-11: java -jar
mirrors:
  https://github.com/sergmain/metaheuristic.git: /git-mirror/mh/metaheuristic.git
processors:
  - code: processor-code-1
    tags: tag1, tag2
  - code: processor-code-2
    tags: tag1
  - code: processor-code-3
```

Top-level fields in env.yaml:   
- disk \<meta\> - Meta object has two fields - code and path. Code defines the code of metadata 
    and path is path to dir where variable are stored.    
- envs \<key-value\> - defines pair of key-value for code of env (key) and executable (value)
- mirrors \<key-value\> - defines a pair of key-value for real url of git's repository (key) and 
    local dir which represents the mirror of remote git's repository. So in this string:
```text
https://github.com/sergmain/metaheuristic.git: /git-mirror/mh/metaheuristic.git
```
    https://github.com/sergmain/metaheuristic.git - remote git's url
    /git-mirror/mh/metaheuristic.git - local directory of git's repository

- processors - List of definitions of processors.
  code -  code of processor. it'll be used as name of directory for this Processor.
  tags - comma-separated list of tags for this Processor    
      
 