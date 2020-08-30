---
layout: default
---

# Quick start

## Table of contents

- [Quick start](#quick-start)
- [Quick start for evaluating UI only](#quick-start-for-evaluating-ui-only)
- [Quick start with running the actual tasks](#quick-start-with-running-the-actual-tasks)

- [to Index](/index)

## Prerequisites: Java Development Kit (JDK) 11

To run Metaheuristic you have to have jdk 11  
Right now there isn't any known bug which restricts to use certain JDK.

[Amazon Corretto 11](https://docs.aws.amazon.com/corretto/latest/corretto-11-ug/downloads-list.html)  
[AdoptOpenJDK (AKA OpenJDK) 11](https://adoptopenjdk.net/?variant=openjdk11&jvmVariant=hotspot)  
[Zulu JDK 11](https://www.azul.com/downloads/zulu-community/?&version=java-11-lts)  


## Quick start
>**Attention**. The 'Quick start' mode is working with embedded db which is being hosted in memory. 
As a result after stopping Metaheuristic all data will be lost.

##### Quick start for evaluating UI only

1. Create a temporary dir for Metaheuristic, i.e. /mh-root 
It'll be /mh-root in following text. 

1. from /mh-root run git cloning command:
    ```text
    git clone https://github.com/sergmain/metaheuristic.git
    git clone https://github.com/sergmain/metaheuristic-assets.git
    ```

1. Change dir to /mh-root/metaheuristis and run command:
    ```text
    mvnw clean install -f pom.xml -Dmaven.test.skip=true
    ```

1. Change dir to /mh-root and run command:
```text
java -Dspring.profiles.active=quickstart,dispatcher,processor -jar metaheuristic/apps/metaheuristic/target/metaheuristic.jar --mh.processor.default-dispatcher-yaml-file=metaheuristic/docs-dev/cfg/default-cfg/dispatcher.yaml --mh.processor.default-env-yaml-file=metaheuristic/docs-dev/cfg/default-cfg/env.yaml 
```

1. Change dir to /mh-root/metaheuristic-assets/examples/simple-metrics and run scripts:
    ```text
    curl-upload-simple-metrics-to-atlas
    ```

1. Now you can find our experiment data at http://localhost:8080/dispatcher/atlas/atlas-experiments
login - q, password - 123

1. Press 'Details' for experiment info (there should be only one record in atlas)

1. Press 'Info' button and on the next page 'Info' button at the bottom of page.

1. Select 2 axes, (i.e. RNN and batches) and press 'Draw plot' 


##### Quick start with running the actual tasks
>To run actual tasks in Metaheuristic you have to have python 3.x.  
Be sure to add the python bin dir to your **$PATH**  
Also, PyYAML 5.1 package must [be installed](https://pyyaml.org/wiki/PyYAMLDocumentation) 

1. Change dir to /mh-root/metaheuristic-assets/examples/simple-metrics and run scripts:
    ```text
    curl-upload-functions-as-one-file
    curl-add-variable-stub
    curl-add-experiment
    curl-add-source-code
    curl-bind-experiment-to-source-code--with-varaible
    curl-create-exec-context
    curl-produce-experiment-tasks
    ```

1. At this point Metaheuristic started to produce tasks 
and you have to wait until status will be changed to 'PRODUCED'. You can check current status by running script
    ```text
    curl-get-experiment-processing-status
    ```

1. After being changed to 'PRODUCED' run the command:
    ```text
    curl-start-processing-of-experiment-tasks
    ```

1. All tasks will be completed in 10 minutes approximately. You can get the current status of processing by command:
    ```text
    curl-get-experiment-processing-status
    ```

    there are 3 possible statuses at this point:  
    STARTED - processing of tasks was started  
    STOPPED - processing of tasks was stopped  
    FINISHED - processing of tasks was finished  

1. After status will change to FINISHED you can find our experiment at http://localhost:8080/dispatcher/experiment/experiments  
login - q, password - 123

1. Press 'Info' button and on the next page 'Info' button at the bottom of page.

1. Select 2 axes, (i.e. RNN and batches) and press 'Draw plot' 

