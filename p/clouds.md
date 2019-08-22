---
layout: default
---

- [Index](/index)

## Table of contents

- [Heroku](#heroku)
- [Jelastic](#jelastic)
- [GCP](#gcp)
- [AWS](#aws)
- [Docker](#docker)
- [K8s](#k8s)

## Heroku
set environment variable:

```/
MAVEN_CONFIG = -f pom-heroku.xml

SPRING_APPLICATION_JSON =   
{  
  "spring.jpa.properties.hibernate.dialect": "org.hibernate.dialect.PostgreSQL95Dialect",    
  "spring.jpa.hibernate.ddl-auto": "none",  
  "spring.datasource.driver-class-name": "org.postgresql.Driver",  
  "spring.datasource.platform": "postgresql",  
  "spring.datasource.continue-on-error": false,  
  
  "mh.thread-number": 4,  
  "mh.launchpad.is-ssl-required": true,  
  "mh.launchpad.master-password": "$2a$10$jaQkP.gqwgenn.xKtjWIbeP4X.LDJx92FKaQ9VfrN2jgdOUTPTMIu",  
  "mh.launchpad.master-username": "q",  
  "mh.launchpad.public-key": "<public-key>",  
  "mh.launchpad.enabled": true,  
  "mh.launchpad.dir": "./mh-launchpad",  
  "mh.launchpad.is-replace-snapshot": false,  
  "mh.launchpad.chunk-size": "900k",  
  "mh.station.enabled": false,  
  "mh.branding": "<desired-branding-name>"
}  
```

## Jelastic
Create an environment topology:

Load balanser =========
- NGiNX 1.14.x

Optional step if your cloud provider doesn't support :
- change redirection rule by adding to nginx-jelastic.conf the following line in the HTTP (80) server block:
return 301 https://$host$request_uri;


Application server =====  
- SpringBoot, jdk - OpenJDK 11.0.2  
  min 2, max 4 cloudlets, Horizontal scaling - 1 node, statefull  
  Variables:  
```
  JDBC_DATABASE_PASSWORD = <pass-for-db>  
  JDBC_DATABASE_URL = jdbc:mysql://<ip-address>:3306/mh?useUnicode=true&characterEncoding=utf8&zeroDateTimeBehavior=CONVERT_TO_NULL&autoReconnect=true&failOverReadOnly=false&maxReconnects=10&useJDBCCompliantTimezoneShift=true&useLegacyDatetimeCode=false&serverTimezone=America/Los_Angeles&allowPublicKeyRetrieval=true    
  JDBC_DATABASE_USERNAME = mh  
  MH_JDBC_DRIVER_CLASS_NAME = com.mysql.cj.jdbc.Driver    
  MH_HIBERNATE_DIALECT = org.hibernate.dialect.MySQL57Dialect    
  MH_MASTER_PASSWORD = $2a$10$jaQkP.gqwgenn.xKtjWIbeP4X.LDJx92FKaQ9VfrN2jgdOUTPTMIu    
  MH_MASTER_USERNAME = q  
  MH_IS_SSL_REQUIRED = true  
  MH_DEFAULT_RESULT_FILE_EXTENSION = <desired-default-extension>  
  MH_BRANDING = <desired-branding-name>  
  MH_LAUNCHPAD_DIR = target/launchpad  
```


SQL DB =================
- Mysql CE 8.0.16,  
  min 3, max 4 cloudlets, Horizontal scaling - 1 node, stateless  

Preparing:
- create database mh   
- run scrip https://github.com/sergmain/metaheuristic/blob/master/sql/schema-mysql.sql in mh database   
- create user mh, grant all to it, use SSL required   
- replace <pass-for-db> in app server's variable JDBC_DATABASE_PASSWORD  
- replace <ip-address>: in app server's variable JDBC_DATABASE_URL   
   


Build node =============  
- Maven 3.6.1, jdk - OpenJDK 11.0.2  
  min 1, max 4 cloudlets  
  variables:  
```
  MAVEN_DEPLOY_ARTIFACT = metaheuristic.jar  
  MAVEN_OPTS = -Dmaven.test.skip=true  
  MAVEN_RUN_ARGS = -T2 -f pom-heroku.xml  
```


Link github repo =======  
- after creating a topology add a link to git repository - https://github.com/sergmain/metaheuristic.git   
  branch - release  



=====================  
login into Metaheuristic :  
login: q  
pass: 123  


## GCP
TBD

## AWS
TBD

## Docker
TBD

## K8s
TBD
