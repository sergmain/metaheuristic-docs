---
layout: default
---

- [Index](/index)

## Table of contents

- [Concept](#concept)
- [Architecture](#architecture)
- [Roadmap](#roadmap)

## Concept
Metaheuristic is an application for an AI model's hyper-parameter optimization purpose. 
Each step of optimization is presented as Experiment. An Experiment consists of some Tasks.
Tasks are created at Launchpad and distributed to Stations. For evaluating a performance of models, 
metrics and other data are collected and are evaluated later by Metaheuristic.  

## Architecture
Metaheuristic has two main modules - Launchpad and Station
The purpose of Launchpad is to manage tasks which will be processed at Station side.
Station is an module of Metaheuristic which is responsible for receiving tasks, 
preparing assets, processing task, and delivering results of processing of task to the Launchpad.

The typical configuration is one Launchpad and N Station. But Station could be configured to host 
tasks from unlimited number of Launchpads. The current algorithm to determine the order of receiving 
tasks from Launchpads is round-robin.

A processing module which is configured at Launchpad and is being run at Station is a Snippet.
Snippet can be executable application, .jar file, python notebook, 
file in any scripting language. It can be 'curl' command to request external url, send e-mail and so on
For full description of Snippet see [documentation on Snippets](snippet)  

From delivery point of view Snippet can be provisioned from Lauchpad or from git repository.

There are two special type of Snippets for fitting and predicting with AI models. 
Those Snippets are configured while configuring an Experiment. An Experiment is 
special set of tasks which is processing hyper-parameter optimization. For full 
description of Experiment see [documentation on Experiment](experiment) 

For creating set of tasks Launchpad is using a directed acyclic graph which define the order 
in which Stations have to process tasks.

Tasks can have any kind of resources for its execution purpose. 
The most common case is when resource is dataset for model fitting/predicting.
For full description of Resource see [documentation on Resource](resource)

## Roadmap

- Overfitting detector in Experiment.   
- Series of Experiments.   
- Implementation the different strategies for Series - Genetic algorithm, Bayesian optimization, GRASP, ILS. 
 The concrete algo which will be implemented in first place will be chosen later.
- DSL for fitness function (in term of GA) or adapt a standard language for this purpose   
- Prototype 2-side platform for optimization based on Metaheuristic.
- TBD   

 