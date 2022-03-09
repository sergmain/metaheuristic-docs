---
layout: default
---

# Concept, architecture, roadmap

## Table of contents

- [Concept](#concept)
- [Architecture](#architecture)

- [to Index](/index)

## Concept
Metaheuristic is an application which implements (or intended to) a Turing complete machine. The main use of MH is a management of distributed tasks.
Common cases of usage are:

 - Batch processing. \
     Common usage of batch processing - split data, create tasks for processing each part of data, 
     process Tasks, aggregate results 
 - Bulk processing of images. 
 - Bulk processing of text. 

All calcualtions are performed by distributed functions.

Metaheuristic can be deployed on-premiceses or in cloud. 
On-premises deployment allows to utilize internal computationa resource in more efficient way, i.e. running tasks at night. 
Also it helps to protect companies' intellectual property.    
     

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
file in any scripting language. It can be 'curl' command to request external url, send e-mail and so on. \ 
For full description of Function see [documentation on Functions](function)  

From delivery point of view Function can be provisioned from Dispatcher or from git repository.

For creating set of tasks, Dispatcher is using a dynamic directed acyclic graph (DAG) which defines the order 
in which Processors have to process tasks.

Tasks can have any number of Variables for its execution purpose. 
The most common case for hyper-parameter optimization is when Variables are dataset, model fitting/predicting, metrics.
For full description of Variables see [documentation on Variables](variable)

Communications between Processors and Dispatcher base on pull mode - Processors constantly start communiccation with Dispatcher. 
Push mode isn't supported.


The current implementation of Metaheuristic handles tens of thousands Tasks simultaneously in DAG. 
Actual number of active tasks is limited by the number of Processors only.