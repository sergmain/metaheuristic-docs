---
layout: default
---

# Security

## Table of contents

- [Intro](#intro)
- ['Master admin' account configuration](#master-admin-account-configuration)
- [Concept of 'main company'](#concept-of-main-company)
- [Configuring access for rest-request from Processor](#configuring-access-for-rest-request-from-processor)
- [Configuring rest-access on Processor side](#configuring-rest-access-on-processor-side)
- [Configuring 'Admin' account for a new company](#configuring-admin-account-for-a-new-company)
- [Verifying rest-access](#verifying-rest-access)
- [Asset server](#asset-server)
- [The current role matrix](#the-current-role-matrix)
- [General information about securing a Function](#general-information-about-securing-a-function)
- [Configuring for using a signed Function](#configuring-for-using-a-signed-function)
- [Configuring Public key](#configuring-public-key)
- [Enabling CORS protection](#enabling-cors-protection)

- [to Index](/index)

## Intro

As a distributed application Metaheuristic has a complex security model which protects itself from different vectors.



### 'Master admin' account configuration

For configuring the main properties of Metaheuristic, such as rest access for Processors, 
the 'Master admin' account must be used. Master admin account is configured in 
[application.properties](application_properties) file and can't be changed dynamically via web-interface.
Properties for configuring:
```properties
# password - 123
mh.dispatcher.master-password=$2a$10$jaQkP.gqwgenn.xKtjWIbeP4X.LDJx92FKaQ9VfrN2jgdOUTPTMIu
mh.dispatcher.master-username=q
```

For storing/verifying a password, Metaheuristic is using bcrypt method. 
You need to use an application [encript-password](encrypt-password)  for producing a new encrypted password.

After configuring master admin account you should be able to log in Metaheuristic the first time.

### Concept of 'main company'
Metaheuristic is using multi-tenant architecture based on Companies. 
There is predefined 'Main company' which has an uniqueCompanyId==1. 
'Master admin' and all rest accounts belong to this company.
 

### Configuring access for rest-request from Processor
All communications between Dispatcher and Processors are a rest-requests. 
Metaheuristic is using pull-model of communication in which Processor is asking for new task via rest-based request.

As a result Processor can be deployed in private environment but Dispatcher can be deployed on public hosting.

For configuring rest accounts, 'master admin' account has to be used:
 - Log in Metaheuristic
 - navigate to 'Control panel'/'Company' 
 - select 'Main company'
 - create a new account and select the role 'ROLE_SERVER_REST_ACCESS'


### Configuring rest-access on Processor side
After configuring a rest account in Dispatcher,
this account has to be registered in [dispatcher.yaml](description-of-dispatcher-yaml)
Properties for configuring:
```yaml
dispatchers:   
  - url: http://localhost:8080   
    lookupType: direct   
    authType: basic   
    restUsername: q
    restPassword: 123   
```

url is the address at which Dispatcher will response to requests.
lookupType has only one value as 'direct' right now
authType has only one value as 'basic' right now

> as a password you have to specify a plain password, not encrypted form. 


### Configuring 'Admin' account for a new company
For interaction with Metaheuristic user has to have its own account.
Let's create 'Admin' account:
 - Log in Metaheuristing with 'Master admin' account
 - navigate to 'Control panel'/'Company' 
 - create a new Company, ie 'Company #1'
 - navigate to 'Company #1'/'Accounts'
 - create a new account and select the role 'ROLE_ADMIN'
 

### Verifying rest-access
If a rest-acceess was configured correctly, you will see a new Processor in the list of Processors:
 - Log in Metaheuristing with 'Admin' account
 - navigate to 'Control panel'/'Processors' 
 
if there isn't any Processor, you have to check mh.log for troubleshooting. 
Common errors are usually about wrong url of Dispatcher, wrong credential of rest account, 
the role 'ROLE_SERVER_REST_ACCESS' wasn't selected. 


### Asset server
Configuring of assert server is describing in a dedicated page about [Asset server](asset-server)  
  
### The current role matrix
The current role martix is following:

```text
'AI' - 'ADMIN', 'DATA', 'MANAGER'
all sub-menu - the same roles

'Batch' = 'ADMIN', 'DATA', 'MANAGER', 'OPERATOR'
all sub-menu - the same roles

'Control panel' - 'MASTER_ADMIN', 'MASTER_OPERATOR', 'MASTER_SUPPORT', 'ADMIN', 'DATA', 'MANAGER'
sub-menu:
    'Source codes' - 'ADMIN', 'DATA', 'MANAGER'
    'Variables' - 'ADMIN', 'DATA'
    'Functions' - 'ADMIN', 'DATA'
    'Processors' - 'ADMIN', 'DATA'
    'Accounts' - 'ADMIN'
    'Company' - 'MASTER_ADMIN', 'MASTER_OPERATOR', 'MASTER_SUPPORT'
```

### General information about securing a Function
One of the main feature of Metaheuristic is a built-in provisioning of [Functions](function).
Because a Function is an executable code, Metaheuristic has internal mechanism for verifying consistency of Functions 
and can verify authority of deploying of new Function.


### Configuring for using a signed Function
Metaheuristic can operate in two modes:
- there isn't any requirements for singing of Function
- all Function must be signed 

This behaviour is controlled by: 
  -  property mh.dispatcher.function-signature-required in [application.properties](application_properties) file.
  -  property signatureRequired in [dispatcher.yaml](description-of-dispatcher-yaml)
> There is [Internal function](internal-function) which is executed at Dispatcher side but right now 
> there isn't any possibility to dynamically upload Internal function to Dispatcher and execute it. 
> All Internal functions are part of Metaheuristic code-base.  

If installation has mixed rules about signing Functions and mh.dispatcher.function-signature-required=false, 
a Processor with signatureRequired=true will receive only signed Functions, 
and a Processor with signatureRequired=false will receive all Functions.     
  

For operating without signing there isn't any required special steps.


If Processor configured with signatureRequired=true, all Functions which are intended to be executed 
at this Processor have to be signed with [Package a Function](package-a-function).

### Configuring Public key
For verifying that Function is signed with correct private key, the Public key must be configured in two config files:
 - application.properties - the property for specifying Public key is 'mh.dispatcher.public-key'
 - dispatcher.yaml - the property for specifying Public key is 'publicKey'
 
> Current implementation of Metaheuristic is using RSA-2048 for generating a pair of keys. 
> If you want to use different an algo please fill in a new issue at [https://github.com/sergmain/metaheuristic/issues]() 

Common problem about configuring public keys is:
- generated public and/or private keys contain an information text. Remove the information text before using keys - 
'Private key in base64 format:' and 'Public key in base64 format'. 

> 'Private key in base64 format:' and 'Public key in base64 format' 
>aren't parts of keys and must not be used or stored in config file. 
>This information text is output of [Generate keys](gen-keys) application for information only. 
>
>

### Enabling CORS protection
CORS can be enabled for protecting rest end-points. By default, the value of allowed-origin is '*'.    
Config file [application.properties](application_properties) has a property 'mh.cors-allowed-origins' 
which allows to make a fine-grained restriction. 
For example, allowing localhost only:
```properties
mh.cors-allowed-origins=http://localhost:4200, http://localhost:8888
```
     