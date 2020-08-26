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

```
MAVEN_CONFIG = -f pom-heroku.xml

SPRING_APPLICATION_JSON =   
{  
  "spring.jpa.properties.hibernate.dialect": "org.hibernate.dialect.PostgreSQL95Dialect",    
  "spring.jpa.hibernate.ddl-auto": "none",  
  "spring.datasource.driver-class-name": "org.postgresql.Driver",  
  "spring.datasource.platform": "postgresql",  
  "spring.datasource.continue-on-error": false,  
  
  "mh.thread-number": 4,  
  "mh.dispatcher.is-ssl-required": true,  
  "mh.dispatcher.master-password": "$2a$10$jaQkP.gqwgenn.xKtjWIbeP4X.LDJx92FKaQ9VfrN2jgdOUTPTMIu",  
  "mh.dispatcher.master-username": "q",  
  "mh.dispatcher.public-key": "<public-key>",  
  "mh.dispatcher.enabled": true,  
  "mh.dispatcher.dir": "./mh-launchpad",  
  "mh.dispatcher.chunk-size": "900k",  
  "mh.processor.enabled": false,  
  "mh.branding": "<desired-branding-name>"
}  
```

> - Heroku has limitation that output of server can't be more that 1Mb. 
So you must keep the value of mh.dispatcher.chunk-size below 1Mb. From experience, the best value is 900k.
[See on Heroku 'HTTP response buffering'](https://devcenter.heroku.com/articles/http-routing#response-buffering)   


## Jelastic
Create an environment topology:

Load balanser =========
- NGiNX 1.14.x

Optional step if your cloud provider doesn't support :
- change redirection rule by adding to nginx-jelastic.conf the following line in the HTTP (80) server block:
```
return 301 https://$host$request_uri;
```

Application server =====  
- SpringBoot, jdk - OpenJDK 11.0.7  
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
   

Building MH from source at jelastic side isn't supported. 
You have to package MH locally and then upload fat-jar to jelastic manually. 


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

