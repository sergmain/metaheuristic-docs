---
layout: default
---

- [Index](/index)

## Table of contents

- [Definition](#definition)
- [Structure](#structure)
- [Schedule](#Schedule)
- [WeekTimePeriod](#WeekTimePeriod)
- [Schedule samples](#Schedule samples)


### Definition

When snippet is being invoked it will be supplied with an absolute path to parameter file. 
This parameter will always be the last parameter in the list of parameters which snippet will receive.

### Structure

This parameter file has the follow structure:   

```yaml
launchpads:   
  - securityEnabled: true   
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

Top-level fields in launchpad.yaml:   
- launchpads - The list of configuration of launchpad
      
Fields in launchpads:
- securityEnabled: <boolean> - does Launchpad required to provide authentication data to connect to Launchpad   
- url: http://localhost:8080 <string> - url of Launchpad  
- lookupType: direct - right now there is only one value which is 'direct'  
- authType: basic - right now there is only one value which is 'basic'  
- restPassword: <string> - password for connecting to Launchpad  
- restUsername: <String> - login for connecting to Launchpad  
- taskProcessingTime <String> - schedule when tasks have to processed at Station side. 
    Full description see [below](#schedule)   
- acceptOnlySignedSnippets: <boolean> - should Snippet be signed or not   
      
      
### Schedule   
      
For purpose of controlling when Station can process the task, there is taskProcessingTime parameter:

- dayMask - mask which defines the format of date for fields workingDay and workingDay, i.e dd/MM/yyyy   
- workingDay - comma separated list of time periods when Station is active while working day (mon-fri), i.e.:
    -- 0:00-23:59 
    -- 0:00-8:45, 19:00-23:59 
- weekend - comma separated list of time periods when Station is active while working day (sat, sun), i.e.:
    -- 0:00-23:59 
    -- 0:00-8:45, 19:00-23:59 
- holiday - comma separated list of days which must be evaluated as holidays, i.e. 15/01/2019, 16/01/2019
- exceptionWorkingDay - comma separated list of days which must be evaluated as working day, 
    i.e. 15/01/2019, 16/01/2019
- week <WeekTimePeriod> - scherule for every day of week. See [WeekTimePeriod](#WeekTimePeriod) below
- policy strict/normal - should task be terminated immediately if working time is over,
             with strict task will be terminated immediately, with normal task can finish its execution in normal way      

#### WeekTimePeriod
Structure WeekTimePeriod has 7 fields - mon, tue, wed, thu, fri, sat, sun. 
Each field has value of comma separated list of time periods when Station is active while working day.

#### Schedule samples

Sample \#1, Station is working without interruption

```yaml
launchpads:   
  - securityEnabled: true   
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

Sample \#2, Station is working with interruption on mon, the, and sat. 
There are two holidays (15/01/2019 and 16/01/2019) and one working day which is on Saturday (19/01/2019)  

```yaml
launchpads:   
  - securityEnabled: true   
    url: http://localhost:8080   
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
    acceptOnlySignedSnippets: false   
```

Sample \#3, Station is working at night only (from 19:00 till 8:45) and full day at holidays (sat, sun)  
There are two holidays (15/01/2019 and 16/01/2019) and one working day which is on Saturday (19/01/2019)  

```yaml
launchpads:   
  - securityEnabled: true   
    url: http://localhost:8080   
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
    acceptOnlySignedSnippets: false   
```


