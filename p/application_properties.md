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
#server.connection-timeout=-1
server.forward-headers-strategy=native

#spring.profiles.active=dispatcher
spring.profiles.active=dispatcher, processor

# ------------- Logging -----------------
logging.file=logs/logs/mh.log
logging.level.root = warn
logging.level.ai.metaheuristic.ai.Monitoring=error
logging.level.ai.metaheuristic.ai.*=warn

# ===============================
# = DATA SOURCE
# ===============================

# postgresql db @localhost
spring.datasource.url=jdbc:postgresql://localhost:5432/mh?user=mh&password=qwe321&sslmode=require
spring.datasource.username=mh
spring.datasource.password=qwe321
spring.datasource.driver-class-name=org.postgresql.Driver
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQL95Dialect
#
spring.jpa.properties.hibernate.temp.use_jdbc_metadata_defaults = false


# Keep the connection alive if idle for a long time (needed in production)
spring.datasource.testWhileIdle = true
spring.datasource.validationQuery = SELECT 1

# ============== ai.metaheuristic ==================
# ------------- common -----------------
mh.thread-number=4
mh.branding=Metaheuristic
mh.cors-allowed-origins=http://localhost:4200, http://localhost:8888
mh.is-event-enabled=true

# --- Launchpad ---
mh.dispatcher.is-ssl-required=false


# password - 123
mh.dispatcher.master-password=$2a$10$jaQkP.gqwgenn.xKtjWIbeP4X.LDJx92FKaQ9VfrN2jgdOUTPTMIu
mh.dispatcher.master-username=q

mh.dispatcher.public-key= MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAtS3jRjE1wlHcxiqn6fCRvTahRt6LBvhrqxzgo1FcpJ9uZvRUmf3KwszwQoL+Ypw7aM9oxmg15Q+pssKcrulS/ofDfbuusiYdny7wMlil1H11svQM3yGwMl9gjZ2FupaRwpyZkIMj1ILaDhylTudQCBoJgJ/BWyMCDn2kzh5EpV7hkhhfjZ/2/NRIcayQVmMKOikCXR8q1bb3QNQ2HiMyUsBUGzeO2DuvX4n375+SaFIDrse4eGNVbR/ImWw7TeD4wk0h5kJ2VTdgl2J7gVS7gCCMwBN9TVxPErRDxg/OtXreS8VRUd0hOZiadX12KjwI4mjhC4q+geXAq2sC1DOV8wIDAQAB

mh.dispatcher.enabled=true
mh.dispatcher.dir=./mh-dispatcher
#mh.dispatcher.chunk-size=500k

#mh.dispatcher.asset.source-url = http://localhost:8080
#mh.dispatcher.asset.password=123
#mh.dispatcher.asset.username=rest_user
#mh.dispatcher.asset.mode = source
#mh.dispatcher.asset.mode = replicated
mh.dispatcher.asset.mode = local

# ------------- processor -----------------

mh.processor.enabled=true
mh.processor.dir=./mh-processor
```

The full list of Spring's properties you can find here:
[list of Spring's properties](https://docs.spring.io/spring-boot/docs/current/reference/html/common-application-properties.html)

## Metaheuristic properties

Metaheuristic's properties divided in 3 big groups - globals, dispatcher related, and processor related

Globals properties   
- mh.thread-number - integer value in range from 1 to 16, default value is 4. This property defines how many threads will be used.
- mh.is-testing - boolean, default value is false. Its purpose is only for testing.
- mh.system-owner - string, no default value. Isn't used right now. 
- mh.branding - string, default value is 'Metaheuristic project'. Branding title for web interface. 
- mh.is-event-enabled - boolean, default value is false. When true MH's internal events will be stored in db. 
- mh.cors-allowed-origins - string, comma separated list of site urls which will be allowed by CORS, 
    i.e. http://acme.com . There isn't default value for this property
 
    
Dispatcher related properties:   
```
mh.dispatcher.is-ssl-required=false

# password - 123
mh.dispatcher.master-password=$2a$10$jaQkP.gqwgenn.xKtjWIbeP4X.LDJx92FKaQ9VfrN2jgdOUTPTMIu
mh.dispatcher.master-username=q

mh.dispatcher.public-key= MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAtS3jRjE1wlHcxiqn6fCRvTahRt6LBvhrqxzgo1FcpJ9uZvRUmf3KwszwQoL+Ypw7aM9oxmg15Q+pssKcrulS/ofDfbuusiYdny7wMlil1H11svQM3yGwMl9gjZ2FupaRwpyZkIMj1ILaDhylTudQCBoJgJ/BWyMCDn2kzh5EpV7hkhhfjZ/2/NRIcayQVmMKOikCXR8q1bb3QNQ2HiMyUsBUGzeO2DuvX4n375+SaFIDrse4eGNVbR/ImWw7TeD4wk0h5kJ2VTdgl2J7gVS7gCCMwBN9TVxPErRDxg/OtXreS8VRUd0hOZiadX12KjwI4mjhC4q+geXAq2sC1DOV8wIDAQAB

mh.dispatcher.enabled=true
mh.dispatcher.dir=./mh-dispatcher
#mh.dispatcher.chunk-size=500k

#mh.dispatcher.asset.source-url = http://localhost:8080
#mh.dispatcher.asset.password=123
#mh.dispatcher.asset.username=rest_user
#mh.dispatcher.asset.mode = source
#mh.dispatcher.asset.mode = replicated
mh.dispatcher.asset.mode = local

    @Value("${mh.dispatcher.is-security-enabled:#{true}}")
    @Value("${mh.dispatcher.is-ssl-required:#{true}}")
    @Value("${mh.dispatcher.snippet-checksum-required:#{true}}")
    @Value("${mh.dispatcher.snippet-signature-required:#{true}}")
    @Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).strIfNotBlankElseNull( environment.getProperty('mh.dispatcher.master-username')) }")
    @Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).strIfNotBlankElseNull( environment.getProperty('mh.dispatcher.master-password')) }")
    @Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).strIfNotBlankElseNull( environment.getProperty('mh.dispatcher.public-key')) }")
    @Value("${mh.dispatcher.enabled:#{false}}")
    @Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).toFile( environment.getProperty('mh.dispatcher.dir' )) }")

    @Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).minMax( environment.getProperty('mh.dispatcher.max-tasks-per-workbook'), 1, 100000, 5000) }")
    @Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).minMax( environment.getProperty('mh.dispatcher.resource-table-rows-limit'), 5, 100, 20) }")
    @Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).minMax( environment.getProperty('mh.dispatcher.experiment-table-rows-limit'), 5, 30, 10) }")
    @Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).minMax( environment.getProperty('mh.dispatcher.atlas-experiment-table-rows-limit'), 5, 100, 20) }")
    @Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).minMax( environment.getProperty('mh.dispatcher.plan-table-rows-limit'), 5, 50, 10) }")
    @Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).minMax( environment.getProperty('mh.dispatcher.workbook-table-rows-limit'), 5, 50, 20) }")
    @Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).minMax( environment.getProperty('mh.dispatcher.station-table-rows-limit'), 5, 100, 50) }")
    @Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).minMax( environment.getProperty('mh.dispatcher.account-table-rows-limit'), 5, 100, 20) }")

    @Value("${mh.dispatcher.is-replace-snapshot:#{true}}")
    @Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).strIfNotBlankElseNull( environment.getProperty('mh.dispatcher.default-result-file-extension')) }")
    @Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).strIfNotBlankElseNull( environment.getProperty('mh.dispatcher.chunk-size')) }")
```

Processor related properties:  
 
```
mh.processor.enabled=true
mh.processor.dir=./mh-processor
    
@Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).toFile( environment.getProperty('mh.logging.file' )) }")
@Value("${mh.station.enabled:#{false}}")
@Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).toFile( environment.getProperty('mh.station.dir' )) }")
@Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).toFile( environment.getProperty('mh.station.default-launchpad-yaml-file' )) }")
@Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).toFile( environment.getProperty('mh.station.default-env-yaml-file' )) }")
```