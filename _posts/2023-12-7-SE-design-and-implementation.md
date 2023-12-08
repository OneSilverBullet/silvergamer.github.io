---
layout: post
title:  "[Software Engineering]Design and Implementation"
date:   2023-12-7
excerpt: "The Software Design and Implementation."
tag:
- Software Engineering
comments: false
blog: true
feature: https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/blogHead/directX12partI.jpg
---

## Overviews

**Software design and implementation** is the stage in the software engineering process at which **an executable software system is developed**. 

Software **design** and **implementation** activities are **invariably inter-leaved**. 
* identify software components and their relationships, based on a customer's requirements.
* Implementation is the process of realizing the design as a program.

**Question: Build or Buy?**

In a wide range of domains, it is now possible to buy off-the-shelf systems (COTS) that can be adapted and tailored to the users’ requirements. When you develop an application in this way, the design process becomes concerned with **how to use the configuration features of that system to deliver the system requirements.** 


## 1. Object-oriented design using the UML

### 1.1 Object-oriented Design Process

**Structured object-oriented design processes** involve **developing a number of different system models**.

They require a lot of effort for **development and maintenance** of these models and, for small systems, this may not be **cost-effective**. However, for large systems developed by different groups design models are **an important communication mechanism**.

### 1.2 Process Stages

There are **variety of different object-oriented design processes** that depend on the organization using the process. Common activities in these processes include:
* define the context and modes of use of the system
* design the system architecture
* identify the principal system objects
* develop design models
* specify object interfaces

#### 1.2.1 System Context and Interactions

**Understanding the relationships between the software that is being designed and its external environment** is essential:
* for deciding how to provide the required system functionality and how to structure the system to communicate with its environment. 
* establish the boundaries of the system. **Setting the system boundaries** helps you decide what features are implemented in the system being designed and what features are in other associated systems. 

**A system context model** is **a structural model** that demonstrates the other systems in the environment of the system being developed.

**An interaction model** is **a dynamic model** that shows how the system interacts with its environment as it is used.


#### 1.2.2 Architectural Design

**Once interactions between the system and its environment have been understood**, you use this information for designing the system architecture.
* identify the major components that make up the system and their interactions.
* organize the components using an architectural pattern such as a layered or client-server mode.

#### 1.2.3 Object class Identification

Identifying object classes is often a difficult part of object-oriented design.
* no magic-formula for object identification
* an iterative process

Approaches to Identification:

(1) Use **a grammatical approach** based on a natural language description of the system.

(2) Base **the identification on tangible things** in the application domain.

(3) Use a **behavioural approach** and identify objects based on what participates in what behaviour.

(4) Use a **scenario-based analysis**. The objects, attributes and methods in each scenario are identified.

#### 1.2.4 Design Models

**Design models** show the objects and object classes and r**elationships between these entities**.

(1) Structural models describe the **static structure of the system** in terms of object classes and relationships.

(2) Dynamic models describe **the dynamic interactions** between objects.

The Design Model Examples:

* Subsystem Model: show how the design is organised into logically related groups of objects.
    * an encapsulation construct is a logical model. **packages**.
* Sequence Models: show the sequence of object interactions.
    * objects are arranged horizontally across the top
    * Time is represented vertically so models are read top to bottom
* State Machine Models: show how individual objects change their state in response to events.
    *  useful high-level models of a system or an object’s run-time behavior. 
    *  don’t usually need a state diagram for all of the objects in the system. 
* Use-case Models, Aggregation Models, Generalisation Models.

#### 1.2.5 Interface  Specification

**Object interfaces** have to be specified so that the **objects and other components can be designed in parallel.**

Designers should avoid **designing the interface representation** but should hide this in the object itself.

Objects may have several interfaces which are viewpoints on the methods provided.

The UML uses class diagrams  for interface specification but Java may also be used.








