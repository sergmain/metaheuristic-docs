---
layout: default
---

# Concept, architecture, roadmap

## Table of contents

- [Concept](#concept)
- [Architecture](#architecture)
- [Roadmap](#roadmap)

- [to Index](/index)

## Concept
Metaheuristic is an application which implements (or intended to) a Turing complete machine. 
The main use of MH is a management of distributed task. Right now there two main areas where MH is being used:
 - AI model's hyper-parameter optimization purpose. \
    Each optimization is presented as Experiment. An Experiment consists of some Tasks.
    Tasks are created at Dispatcher and distributed to Processor. For evaluating a performance of models, 
    metrics and other data are collected and evaluated later by Metaheuristic.  
 
 - Batch processing. \
     Common usage of batch processing - split data, create tasks for processing each part of data, 
     process Tasks, aggregate results 


## Architecture
Metaheuristic has two main modules - Dispatcher and Processor
The purpose of Dispatcher is to manage tasks which will be processed at Processor side.
Processor is a module of Metaheuristic which is responsible for receiving tasks, 
preparing assets, processing task, and delivering results of processing of task to the Dispatcher.

The typical configuration is one Dispatcher and N Processor. Processor could be configured to host 
tasks from unlimited number of Dispatchers. The current algorithm to determine the order of receiving 
tasks from Processors is round-robin.

A processing entity which is configured at Dispatcher and is being run at Processor is a Function.
Function can be executable application, .jar file, python notebook, 
file in any scripting language. It can be 'curl' command to request external url, send e-mail and so on.
Instance of Function is Task. \
For full description of Function see [documentation on Functions](function)  

From delivery point of view Function can be provisioned from Dispatcher or from git repository.

For full description how to use Function for hyper-parameter optimization see [documentation on Experiment](experiment) 

For creating set of tasks, Dispatcher is using a dynamic directed acyclic graph which defines the order 
in which Processors have to process tasks.

Tasks can have any number of Variables for its execution purpose. 
The most common case for hyper-parameter optimization is when Variables are dataset, model fitting/predicting, metrics.
For full description of Variables see [documentation on Variables](variable)

## Roadmap

- Series of Experiments.   
- Implementation the different strategies for Series - Genetic algorithm, Bayesian optimization, GRASP, ILS. 
 The concrete algo which will be implemented in first place will be chosen later.
- Prototype 2-side platform for optimization based on Metaheuristic.
- TBD   

 