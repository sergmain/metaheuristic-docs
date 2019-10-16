---
layout: default
---

- [Index](/index)

## Table of contents

- [application.properties](#application.properties)
- [Metaheuristic properties](#Metaheuristic properties)



## application.properties: 
application.properties file is the main config file which is control how Metaheuristic behave.
Usually, its location is dir 'config'. But there are some other options how to specify this file -  
[Spring Externalized Configuration](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html)  

Common properties and values of application.properties are: 

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
spring.datasource.url=jdbc:postgresql://localhost:5432/mh?user=mh&password=qwe321&sslmode=require
spring.datasource.username=mh
spring.datasource.password=qwe321
spring.datasource.driver-class-name=org.postgresql.Driver
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQL95Dialect
/#
spring.jpa.properties.hibernate.temp.use_jdbc_metadata_defaults = false


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

The full list of Spring's properties you can find here:
[list of Spring's properties](https://docs.spring.io/spring-boot/docs/current/reference/html/common-application-properties.html)

## Metaheuristic properties

Metaheuristic's properties divided in 3 big groups - globals, launchpad related, and station related

Globals properties   
- mh.thread-number - integer value in range from 1 to 16, default value is 4. This property defines how many threads will be used.
- mh.is-testing - boolean, default value is false. Its purpose is only for testing.
- mh.system-owner - string, default value is not specified. Isn't used right now. 
- mh.branding - string, default value is 'Metaheuristic project'. Branding title for web interface. 
- mh.is-event-enabled - boolean, default value is false. Specifies will MH's internal events be stored in db or not. 

in development state:       
- mh.cors-allowed-origins - string, url of site which will be allowed by CORS, i.e. http://acme.com, default value not specified
 
    
Launchpad related properties:   
    @Value("${mh.launchpad.is-security-enabled:#{true}}")
    @Value("${mh.launchpad.is-ssl-required:#{true}}")
    @Value("${mh.launchpad.snippet-checksum-required:#{true}}")
    @Value("${mh.launchpad.snippet-signature-required:#{true}}")
    @Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).strIfNotBlankElseNull( environment.getProperty('mh.launchpad.master-username')) }")
    @Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).strIfNotBlankElseNull( environment.getProperty('mh.launchpad.master-password')) }")
    @Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).strIfNotBlankElseNull( environment.getProperty('mh.launchpad.public-key')) }")
    @Value("${mh.launchpad.enabled:#{false}}")
    @Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).toFile( environment.getProperty('mh.launchpad.dir' )) }")

    @Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).minMax( environment.getProperty('mh.launchpad.max-tasks-per-workbook'), 1, 100000, 5000) }")
    @Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).minMax( environment.getProperty('mh.launchpad.resource-table-rows-limit'), 5, 100, 20) }")
    @Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).minMax( environment.getProperty('mh.launchpad.experiment-table-rows-limit'), 5, 30, 10) }")
    @Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).minMax( environment.getProperty('mh.launchpad.atlas-experiment-table-rows-limit'), 5, 100, 20) }")
    @Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).minMax( environment.getProperty('mh.launchpad.plan-table-rows-limit'), 5, 50, 10) }")
    @Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).minMax( environment.getProperty('mh.launchpad.workbook-table-rows-limit'), 5, 50, 20) }")
    @Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).minMax( environment.getProperty('mh.launchpad.station-table-rows-limit'), 5, 100, 50) }")
    @Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).minMax( environment.getProperty('mh.launchpad.account-table-rows-limit'), 5, 100, 20) }")

    @Value("${mh.launchpad.is-replace-snapshot:#{true}}")
    @Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).strIfNotBlankElseNull( environment.getProperty('mh.launchpad.default-result-file-extension')) }")
    @Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).strIfNotBlankElseNull( environment.getProperty('mh.launchpad.chunk-size')) }")


Station related properties:   
    @Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).toFile( environment.getProperty('mh.logging.file' )) }")
    @Value("${mh.station.enabled:#{false}}")
    @Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).toFile( environment.getProperty('mh.station.dir' )) }")
    @Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).toFile( environment.getProperty('mh.station.default-launchpad-yaml-file' )) }")
    @Value("#{ T(ai.metaheuristic.ai.utils.EnvProperty).toFile( environment.getProperty('mh.station.default-env-yaml-file' )) }")