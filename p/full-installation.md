---
layout: default
---

# Full installation

- [to Index](/index)

## Table of contents

- [Prerequisites](#prerequisites)
- [On-premises installation](#on-premises-installation)

>This documentation is about how to install Metaheuristic locally. For cloud installation see [Clouds](clouds.md) 

## Prerequisites: 
#### Java Development Kit (JDK) 11

To run Metaheuristic you have to have jdk 11  
Right now, there isn't any known bug which restricts to use certain JDK.

[Amazon Corretto 11](https://docs.aws.amazon.com/corretto/latest/corretto-11-ug/downloads-list.html)  
[AdoptOpenJDK (AKA OpenJDK) 11](https://adoptopenjdk.net/?variant=openjdk11&jvmVariant=hotspot)  
[Zulu JDK 11](https://www.azul.com/downloads/zulu-community/?&version=java-11-lts)  

#### Database
At this moment we are supporting two databases for production environment - mysql and postgresql.    
By supporting we mean that there is sql scripts for initialization of db. 
Quick start is using H2 db, so this db is fine too but only for testing environment.
 
Sql files for db's scheme:   
 -- [Postgresql](https://github.com/sergmain/metaheuristic/blob/master/sql/schema-postgresql.sql)   
 -- [Mysql](https://github.com/sergmain/metaheuristic/blob/master/sql/schema-mysql.sql)   
 -- [H2](https://github.com/sergmain/metaheuristic/blob/master/apps/metaheuristic/src/main/resources/schema-h2.sql)   

In this documentation we expect that you'll create db's scheme 'mh', db's user will be 'mh' and password will be 'qwe321'

## On-premises installation

- Create dir for Metaheuristic, i.e. /mh-root 
It'll be /mh-root in the following text. 

- Change dir to /mh-root and create the following dirs:
   
```text
/mh-root/config
/mh-root/logs
/mh-root/mh-dispatcher
/mh-root/mh-processor
```

- From /mh-root run the git cloning command:   
```text
git clone https://github.com/sergmain/metaheuristic.git
```

- Change dir to /mh-root/metaheuristic and run the command:   
```text
mvnw clean install -f pom.xml -Dmaven.test.skip=true
```

- Change dir to /mh-root/config and create file application.properties with the following content:   

```properties
server.address=127.0.0.1
spring.profiles.active=dispatcher, processor

# ------------- Logging -----------------
logging.file=logs/logs/mh.log
logging.level.root = warn
logging.level.ai.metaheuristic.ai.Monitoring=error
logging.level.ai.metaheuristic.ai.*=warn

# ===============================
# = DATA SOURCE
# ===============================

# start of postgresql section db @localhost
#spring.datasource.url=jdbc:postgresql://localhost:5432/mh?user=mh&password=qwe321&sslmode=require
#spring.datasource.username=mh
#spring.datasource.password=qwe321
#spring.datasource.driver-class-name=org.postgresql.Driver
#spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQL95Dialect
# end of postgresql section

# # start of mysql section db @localhost
spring.datasource.url = jdbc:mysql://localhost:3306/mh?useUnicode=true&characterEncoding=utf8&zeroDateTimeBehavior=CONVERT_TO_NULL&autoReconnect=true&failOverReadOnly=false&maxReconnects=10&useJDBCCompliantTimezoneShift=true&useLegacyDatetimeCode=false&serverTimezone=America/Los_Angeles&sslMode=DISABLED&allowPublicKeyRetrieval=true
spring.datasource.username = mh
spring.datasource.password = qwe321
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.jpa.properties.hibernate.dialect = org.hibernate.dialect.MySQL57Dialect
# end of mysql section

spring.jpa.properties.hibernate.temp.use_jdbc_metadata_defaults = false


# Keep the connection alive if idle for a long time (needed in production)
spring.datasource.testWhileIdle = true
spring.datasource.validationQuery = SELECT 1

# ============== ai.metaheuristic ==================
# ------------- Dispatcher -----------------
mh.dispatcher.is-ssl-required=false
# password is 123
mh.dispatcher.master-password=$2a$10$jaQkP.gqwgenn.xKtjWIbeP4X.LDJx92FKaQ9VfrN2jgdOUTPTMIu
mh.dispatcher.master-username=q
mh.dispatcher.enabled=true
mh.dispatcher.dir=./mh-dispatcher

# ------------- Processor -----------------
mh.processor.enabled=true
mh.processor.dir=./mh-processor
```

> - For mysql Connect/J 5.7 the value zeroDateTimeBehavior must be zeroDateTimeBehavior=convertToNull
> - for version 8.x correct value is zeroDateTimeBehavior=CONVERT_TO_NULL

This config file is for installation with mysql. 
If you are using postgresql, you need to uncomment section for postgresql and comment out for mysql one.

Current connection parameters to database are:   
    -- spring.datasource.username = mh  - defines username in db   
    -- spring.datasource.password = qwe321 - defines password in db   
    -- spring.datasource.url = jdbc:mysql://localhost:3306/mh - defines IP address (localhost) and port (3306) of db,
        and db scheme (mh). If you want to use the different values, you need to change them accordingly.   

Descriptions of all mh.* parameters can be found in [mh.* parameters](description-of-mh-application-properties) 

#### Configure Processor

Now you need to configure Processor. You can skip this part for now and launch 
Metaheuristic with predefined files by going to [Launching of Metaheuristic]() section.

- Create file /mh-root/mh-processor/dispatcher.yaml with following content:
   
```yaml
dispatchers:   
  - signatureRequired: false   
    url: http://localhost:8080   
    lookupType: direct   
    authType: basic   
    restPassword: 123   
    restUsername: q   
    taskProcessingTime: |   
      workingDay: 0:00-23:59   
      weekend: 0:00-23:59   
```
Descriptions of all parameters can be found in [description of dispatcher.yaml](description-of-dispatcher-yaml) 


- Create file /mh-root/mh-processor/env.yaml with the following content:
   
```yaml
envs:
  python-3: python
  java-11: java -jar
```
Descriptions of all parameters can be found in [description of env.yaml](description-of-env-yaml) 


#### Launching of Metaheuristic

That's all. Now you can launch Metaheuristic. Change dir to /mh-root and run Metaheuristic:

- If you skipped the creation of dispatcher.yaml and env.yaml, the command for launching is:
```text
java -jar metaheuristic/apps/metaheuristic/target/metaheuristic.jar --mh.processor.default-dispatcher-yaml-file=metaheuristic/docs-dev/cfg/default-cfg/dispatcher.yaml --mh.processor.default-env-yaml-file=metaheuristic/docs-dev/cfg/default-cfg/env.yaml 
```

- If you created a custom version of dispatcher.yaml and env.yaml, the command for launching is:
```text
java -jar metaheuristic/apps/metaheuristic/target/metaheuristic.jar 
```




