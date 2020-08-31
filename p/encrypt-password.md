---
layout: default
---

# Encrypt a password for application.properties file

## Table of contents

- [Definition](#definition)

- [to Index](/index)

## Definition
Metaheuristic is using password with BCrypt encryption. For encrypting a password use the following command:

```text
java -jar metaheuristic/apps/encrypt-password/target/encrypt-password.jar <password>
```

&lt;password> - password for encrypting   


Resulting password will look like 
```text
$2a$10$jaQkP.gqwgenn.xKtjWIbeP4X.LDJx92FKaQ9VfrN2jgdOUTPTMIu
```

This is an encrypted form of password 123 