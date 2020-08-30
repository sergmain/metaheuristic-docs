---
layout: default
---
# Asset server 

## Table of contents

- [Concept](#concept)
- [Description](#description)

- [to Index](/index)


## Concept
When an installation of Metaheuristic contains several Processors and some Dispatchers 
you can move administrating of assets (Companies, Accounts, Source codes, Functions) to dedicated Asset server.

Technically, Asset server is a [Dispatcher](p/concept-architecture-roadmap#Architecture).

That Asset server will deliver assets to Dispatchers and then Dispatcher will provide Processors with those assets.
Also, Asset server serves all requests from Processors 
to check checksums of assets which were downloaded from Dispatchers to Processors.    

## Description

### Configuring Asset server

To enable Assert server functionality you have to set the following properties 
in [application.properties](/p/application_properties) file 

```properties
spring.profiles.active = dispatcher
mh.launchpad.asset.mode = source
```

Create new user in Company #1 and add a role ASSET_REST_ACCESS to this account


### Configuring Dispatcher with 'replication' mode
To enable communication of Dispather with Assert server the following properties must be set: 
in [application.properties](/p/application_properties) file 

```properties
spring.profiles.active = dispatcher

mh.dispatcher.asset.source-url = http://localhost:8080
mh.dispatcher.asset.password=123
mh.dispatcher.asset.username=rest_user
mh.dispatcher.asset.mode = replicated
```

properties mh.dispatcher.asset.password and mh.dispatcher.asset.username 
must contain credential of user which was created in the previous step.

### Configuring Processors
Configuration of Processor is done by modifying [dispatcher.yaml](/p/description-of-dispatcher-yaml) file

```yaml
    asset:
      url: 
      username: 
      password: 
```

- url - url of Asset server
- username - username of user which will be used to make rest-based query to Asset server
- username - password of user

 