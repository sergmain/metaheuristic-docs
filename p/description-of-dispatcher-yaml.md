---
layout: default
---

# Description of dispatcher.yaml config file

## Table of contents

- [Definition](#definition)
- [Structure](#structure)
- [Schedule](#Schedule)
- [WeekTimePeriod](#WeekTimePeriod)
- [Schedule samples](#Schedule samples)

- [to Index](/index)


### Definition

When Function is being invoked at Processor side it will be supplied with an absolute path to parameter file. 
This parameter will always be the last parameter in the list of parameters which Function will receive.

### Structure

This parameter file has the following structure:   

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
    disabled: false
```

Top-level fields in dispatcher.yaml:   
- dispatchers - The list of configuration of Dispatchers
      
Fields in dispatchers:
- signatureRequired: false - does Function have to be signed with signature  
- url: http://localhost:8080 <string> - url of Dispatcher  
- lookupType: direct - right now there is only one value which is 'direct'  
- authType: basic - right now there is only one value which is 'basic'  
- restPassword: <string> - password for connecting to Dispatcher  
- restUsername: <String> - login for connecting to Dispatcher  
- taskProcessingTime <String> - schedule when tasks have to processed at Processor side. 
    Full description see [below](#schedule)   
- disabled: false - is this Dispatcher disabled or not
      
      
### Schedule   
      
For purpose of controlling when Processor can process the Task, there is taskProcessingTime parameter:

- dayMask - mask which defines the format of date for fields workingDay and workingDay, i.e dd/MM/yyyy   
- workingDay - comma separated list of time periods when Processor is active while working day (mon-fri), i.e.:
    -- 0:00-23:59 
    -- 0:00-8:45, 19:00-23:59 
- weekend - comma separated list of time periods when Processor is active while working day (sat, sun), i.e.:
    -- 0:00-23:59 
    -- 0:00-8:45, 19:00-23:59 
- holiday - comma separated list of days which must be evaluated as holidays, i.e. 15/01/2019, 16/01/2019
- exceptionWorkingDay - comma separated list of days which must be evaluated as working day, 
    i.e. 15/01/2019, 16/01/2019
- week <WeekTimePeriod> - schedule for every day of week. See [WeekTimePeriod](#weektimeperiod) below.
- policy strict/normal - should the task be terminated immediately if working time is over,
             with 'strict' value, the task will be terminated immediately, 
             with 'normal' value, the task can finish its execution in normal way
             default value is normal      

### WeekTimePeriod
Structure WeekTimePeriod has 7 fields - mon, tue, wed, thu, fri, sat, sun. 
Each field has value of comma separated list of time periods when Processor is active while working day.

#### Schedule samples

Sample \#1, Processor is working without interruption

```yaml
dispatchers:   
  - url: http://localhost:8080   
    lookupType: direct   
    authType: basic   
    restPassword: 123   
    restUsername: q   
    taskProcessingTime: |   
      workingDay: 0:00-23:59   
      weekend: 0:00-23:59   
    signatureRequired: false   
```   

or just omit taskProcessingTime completely.


Sample \#2, Processor is working with interruption on mon, the, and sat. 
There are two holidays (15/01/2019 and 16/01/2019) and one working day which is on Saturday (19/01/2019)  

```yaml
dispatchers:   
  - url: http://localhost:8080   
    lookupType: direct   
    authType: basic   
    restPassword: 123   
    restUsername: q   
    taskProcessingTime: |   
      week:
        mon: 0:00-8:45, 19:00-23:59
        thu: 0:00-8:45, 19:00-23:59
        sat: 0:00-8:45, 19:00-23:59
      dayMask: dd/MM/yyyy
      holiday: 15/01/2019,16/01/2019
      exceptionWorkingDay: 19/01/2019
    signatureRequired: false   
```


Sample \#3, Processor is working at night only (from 19:00 till 8:45) and full day at holidays (sat, sun)  
There are two holidays (15/01/2019 and 16/01/2019) and one working day which is on Saturday (19/01/2019)

```yaml
dispatchers:   
  - url: http://localhost:8080   
    lookupType: direct   
    authType: basic   
    restPassword: 123   
    restUsername: q   
    taskProcessingTime: |   
      workingDay: 0:00-8:45, 19:00-23:59
      weekend: 0:00-23:59
      dayMask: dd/MM/yyyy
      holiday: 15/01/2019,16/01/2019
      exceptionWorkingDay: 19/01/2019
    signatureRequired: false   
```


Sample \#4, Processor is working at weekend only (from 0:00 till 23:59).
Policy is strict.  

```yaml
dispatchers:   
  - url: http://localhost:8080   
    lookupType: direct   
    authType: basic   
    restPassword: 123   
    restUsername: q   
    taskProcessingTime: |   
      workingDay: 0:00-0:00
      weekend: 0:00-23:59
      policy: strict
    signatureRequired: false   
```


