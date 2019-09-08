---
layout: default
---

- [Index](/index)

## Table of contents

- [Prerequisites](#prerequisites)
- [On-premises installation](#on-premises-installation)

>This documentation is about how to install Metaheuristic locally. For cloud installation see [Clouds](clouds.md) 

## Prerequisites: 
#### Java Development Kit (JDK) 11

To run Metaheuristic you have to have jdk 11  
Right now there isn't any known bug which restricts to use certain JDK.

[Amazon Corretto 11](https://docs.aws.amazon.com/corretto/latest/corretto-11-ug/downloads-list.html)  
[AdoptOpenJDK (AKA OpenJDK) 11](https://adoptopenjdk.net/?variant=openjdk11&jvmVariant=hotspot)  
[Zulu JDK 11](https://www.azul.com/downloads/zulu-community/?&version=java-11-lts)  

#### Database
At this moment we are supporting two databases - mysql and postgresql.    
By supporting we means that there is sql scripts for initialization of db. 
Quick start is using H2 db, so this db is fine too. 
Sql files for H2 [is there](https://github.com/sergmain/metaheuristic/blob/master/apps/metaheuristic/src/main/resources/schema-h2.sql)

In this documentation we expect that you'll create db's scheme 'mh', db's user will be 'mh' and password will be 'qwe321'

## On-premises installation

- Create dir for Metaheuristic, i.e. /mh-root 
It'll be /mh-root in the follow text. 

- Change dir to /mh-root and the follow dirs:
   
```
/mh-root/config
/mh-root/logs
/mh-root/mh-launchpad
/mh-root/mh-station
```

- From /mh-root run the git cloning command:   
```
git clone https://github.com/sergmain/metaheuristic.git
```

- Change dir to /mh-root/metaheuristic and run the command:   
```
mvnw clean install -f pom.xml -Dmaven.test.skip=true
```

- Change dir to /mh-root/config and create file application.properties with follow content:   

```properties
server.address=127.0.0.1
spring.profiles.active=launchpad, station

# ------------- Logging -----------------
logging.file=logs/logs/mh.log
logging.level.root = warn
logging.level.ai.metaheuristic.ai.Monitoring=error
logging.level.ai.metaheuristic.ai.*=warn

# ===============================
# = DATA SOURCE
# ===============================

# postgresql db @localhost
#spring.datasource.url=jdbc:postgresql://localhost:5432/mh?user=mh&password=qwe321&sslmode=require
#spring.datasource.username=mh
#spring.datasource.password=qwe321
#spring.datasource.driver-class-name=org.postgresql.Driver
#spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQL95Dialect

# mysql db @localhost
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url = jdbc:mysql://localhost:3306/mh?useUnicode=true&characterEncoding=utf8&zeroDateTimeBehavior=CONVERT_TO_NULL&autoReconnect=true&failOverReadOnly=false&maxReconnects=10&useJDBCCompliantTimezoneShift=true&useLegacyDatetimeCode=false&serverTimezone=America/Los_Angeles&sslMode=DISABLED&allowPublicKeyRetrieval=true
spring.datasource.username = mh
spring.datasource.password = qwe321

spring.jpa.properties.hibernate.temp.use_jdbc_metadata_defaults = false
spring.jpa.properties.hibernate.dialect = org.hibernate.dialect.MySQL57Dialect


# Keep the connection alive if idle for a long time (needed in production)
spring.datasource.testWhileIdle = true
spring.datasource.validationQuery = SELECT 1

# ============== ai.metaheuristic ==================
# ------------- Launchpad -----------------
mh.launchpad.is-ssl-required=false
# password is 123
mh.launchpad.master-password=$2a$10$jaQkP.gqwgenn.xKtjWIbeP4X.LDJx92FKaQ9VfrN2jgdOUTPTMIu
mh.launchpad.master-username=q
mh.launchpad.enabled=true
mh.launchpad.dir=./mh-launchpad
mh.launchpad.is-replace-snapshot=true

# ------------- Station -----------------
mh.station.enabled=true
mh.station.dir=./mh-station
```

This config file is for installation with mysql. 
If you are using postgresql, you need to uncomment section for postgresql and comment out for mysql one.

Current connection parameters to database are:   
    -- spring.datasource.username = mh  - defines db's username   
    -- spring.datasource.password = qwe321 - defines db's username   
    -- spring.datasource.url = jdbc:mysql://localhost:3306/mh - defines IP address (localhost) and port (3306) of db,
        and db scheme (mh). If you want to use the different values, you need to change them accordingly   

Descriptions of all mh.* parameters can be found in [mh.* parameters](mh-application-properties) 

- Create file /mh-root/mh-station/launchpad.yaml with follow content:
   
```yaml
launchpads:   
  - signatureRequired: false   
    securityEnabled: true   
    url: http://localhost:8080   
    lookupType: direct   
    authType: basic   
    restPassword: 123   
    restUsername: q   
    taskProcessingTime: |   
      workingDay: 0:00-23:59   
      weekend: 0:00-23:59   
    acceptOnlySignedSnippets: false   
```

- Create file /mh-root/mh-station/env.yaml with follow content:
   
```yaml
envs:
  python-3: python.exe
  java-11: java -jar
```







