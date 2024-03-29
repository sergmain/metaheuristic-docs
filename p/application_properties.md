---
layout: default
---

- [Index](/index)

## Table of contents

- [application.properties](#application.properties)
- [Metaheuristic properties](#Metaheuristic properties)



## application.properties: 
application.properties file is the main config file which controls how Metaheuristic behave.
Usually, its location is a dir 'config'. Beside this option there are some other options how to specify this file -  
[Spring Externalized Configuration](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html)  

Common properties and values of application.properties are: 

```properties
spring.jmx.enabled=false
server.address=127.0.0.1
server.http2.enabled=true
server.forward-headers-strategy=native
spring.profiles.active=dispatcher, processor

# ------------- Logging -----------------
logging.file.name=logs/logs/mh.log
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

spring.datasource.maxActive=100
spring.datasource.maxIdle=50
spring.datasource.minIdle=50
spring.datasource.initialSize=50
spring.datasource.removeAbandoned=true

spring.jpa.show-sql = false

# Keep the connection alive if idle for a long time (needed in production)
spring.datasource.testWhileIdle = true
spring.datasource.validationQuery = SELECT 1

# ============== ai.metaheuristic ==================
# ------------- common -----------------
mh.thread-number=4
mh.branding=Metaheuristic
mh.cors-allowed-origins=http://localhost:4200, http://localhost:8888
mh.is-event-enabled=false

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


The full list of Spring's properties you can find here:
[list of Spring's properties](https://docs.spring.io/spring-boot/docs/current/reference/html/common-application-properties.html)

## Metaheuristic properties

Metaheuristic's properties are divided in 3 big groups - globals, related to dispatcher, and related to processor

Globals properties   
- mh.thread-number - integer value in range from 1 to 16, default value is 4. This property defines how many threads will be used.
- mh.is-testing - boolean, default value is false. Its purpose is only for testing.
- mh.system-owner - string, no default value. Isn't used right now. 
- mh.branding - string, default value is 'Metaheuristic project'. Branding title for web interface. 
- mh.is-event-enabled - boolean, default value is false. When true MH's internal events will be stored in db. 
- mh.cors-allowed-origins - string, comma separated list of site urls which will be allowed by CORS, 
    i.e. http://acme.com . There isn't default value for this property.
 
    
Dispatcher related properties:   
```properties
mh.dispatcher.is-ssl-required=false

# password - 123
mh.dispatcher.master-password=$2a$10$jaQkP.gqwgenn.xKtjWIbeP4X.LDJx92FKaQ9VfrN2jgdOUTPTMIu
mh.dispatcher.master-username=q

mh.dispatcher.public-key= MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAtS3jRjE1wlHcxiqn6fCRvTahRt6LBvhrqxzgo1FcpJ9uZvRUmf3KwszwQoL+Ypw7aM9oxmg15Q+pssKcrulS/ofDfbuusiYdny7wMlil1H11svQM3yGwMl9gjZ2FupaRwpyZkIMj1ILaDhylTudQCBoJgJ/BWyMCDn2kzh5EpV7hkhhfjZ/2/NRIcayQVmMKOikCXR8q1bb3QNQ2HiMyUsBUGzeO2DuvX4n375+SaFIDrse4eGNVbR/ImWw7TeD4wk0h5kJ2VTdgl2J7gVS7gCCMwBN9TVxPErRDxg/OtXreS8VRUd0hOZiadX12KjwI4mjhC4q+geXAq2sC1DOV8wIDAQAB

mh.dispatcher.enabled=true
mh.dispatcher.dir=./mh-dispatcher
mh.dispatcher.chunk-size=10m
mh.dispatcher.default-result-file-extension=.bin

#mh.dispatcher.asset.source-url = http://localhost:8080
#mh.dispatcher.asset.password=123
#mh.dispatcher.asset.username=rest_user
#mh.dispatcher.asset.mode = source
#mh.dispatcher.asset.mode = replicated
mh.dispatcher.asset.mode = local
```

Processor related properties:  
 
```properties
mh.processor.enabled=true
mh.processor.dir=./mh-processor
    
#mh.processor.default-dispatcher-yaml-file=...
#mh.processor.default-env-yaml-file=...
```

In case when dispatcher is disabled and there is only processor profile the following exclusion must be inserted in application.properties
```properties
spring.autoconfigure.exclude=org.springframework.boot.autoconfigure.orm.jpa.HibernateJpaAutoConfiguration,\
    org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration,\
    org.springframework.boot.autoconfigure.jdbc.DataSourceTransactionManagerAutoConfiguration,\
    org.springframework.boot.autoconfigure.data.web.SpringDataWebAutoConfiguration
```

