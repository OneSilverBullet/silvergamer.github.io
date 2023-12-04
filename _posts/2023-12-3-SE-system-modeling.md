---
layout: post
title:  "[Software Engineering]System Modeling"
date:   2023-12-3
excerpt: "The Software Development Models."
tag:
- Software Engineering
comments: false
blog: true
feature: https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/blogHead/directX12partI.jpg
---

## Overviews

**System Modeling** is the process of **developiong abstract models of a system**, with each model presenting a different view or perspective of that system.

**Models of the existing system** are used during **requirements engineering**.
* They help clarify what the existing system does and can be used as a basis for discussing its strengths and weaknesses. 
* These then lead to requirements for the new system.

**Models of the new system** are used during **requirements engineering** to help explain the proposed requirements to other system stakeholders. 
* Engineers use these models to discuss design proposals and to **document the system for implementation**. 

In **a model-driven engineering process**, it is possible to **generate a complete or partial system implementation** from the **system model**.


System Perspectives:
* **External/Context Model**: External Perspective: where you model **the context or environment of the system**.
    * Block Diagram
    * User Case Diagram
* **Process Model**: the whole bussiness process.
    * Activity Diagram
* **Interaction Model**: Interaction Perspective: where you model **the interactions between a system** and its environment, or between the components of a system.
    * User Case Diagram
    * Sequence Diagram
* **Structural Model**: Structural Perspective: where you model **the organization of a system** or the structure of the data that is processed by the system.
    * Class Diagram
    * Activity Diagram
* **Behavioral Model**: Behavioral Perspective: where you model the **dynamic behavior of the system** and how it responds to events.
    * State Diagram
    * Activity Diagram

Usage of Graphical Models:
* As a means of **facilitating discussion about an existing or proposed system**
    * **Incomplete and incorrect models are OK** as their role is to support discussion.
* As a way of **documenting an existing system**
    * Models should be **an accurate representation of the system** but need not be complete.
* As **a detailed system description** that can be used to generate a system implementation.
    * Models have to be both correct and complete.


## 1. Context Models

### 1.1 Conceptions

**Context models** are used to illustrate **the operational context of a system** - they show what lies **outside the system boundaries**.
* **Social and organisational concerns** may affect the decision on where to **position system boundaries**.
* **Architectural models** show the system and **its relationship with other systems**.
* Context Diagram

### 1.2 System Boundaries

System boundaries are established to **define what is inside and what is outside the system**.
* They show other systems that are **used or depended on the system being developed**.

The **position of the system boundary** has **a profound effect** on the system requirements.
* Defining a system boundary is **a political judgement**.


### 1.3 Process Perspective

Question: **Context Models** simply show the other systems in the environment, **not how the system being developed** is used **in that environment**.

Solution: **Process Models** reveal **how the system being developed** is used in broader business processes.
* Activity Diagram

## 2. Interaction Models

**Modeling user interaction** is important as it helps to **identify user requirements**. 

**Modeling system-to-system interaction** highlights the **communication problems** that may arise. 

**Modeling component interaction** helps us understand **if a proposed system structure is likely to deliver the required system performance and dependability**. 
 
Diagram:
* User Case Diagrams
* Sequence Diagrams

### 2.1 User Case Modeling

**User cases** were developed originally to **Support Requirements Elicitation** and now incorporated into the UML.

* Each **use case** represents **a discrete task** that involves **external interaction with a system**.
* Actors in a use case may be **people or other systems**.
* Represented diagrammatically to provide an overview of the use case and in a **more detailed textual form**.

### 2.2 Sequence Diagrams

**Sequence diagrams** are part of the UML and are used to **model the interactions** between **the actors and the objects within a system**.

A sequence diagram shows the **sequence of interactions** that take place during a particular use case or use case instance.
* The **objects and actors** involved are listed along the top of the diagram, with a dotted line drawn vertically from these. 
* Interactions between objects are **indicated by annotated arrows**.  

## 3. Structural Models

### 3.1 Structural Models

**Structural models** of software display **the organization of a system** in terms of the components that **make up that system and their relationships**. 

Structural models may be:
* **static models**, which **show the structure of the system design**
* **dynamic models**, which show **the organization of the system when it is executing**. 

You create **structural models of a system** when you are discussing and designing the system architecture. 

### 3.2 Class Diagram

Class diagrams are used **when developing an object-oriented system model** to show the classes in a system and the associations between these classes. 

**An object class** can be thought of as **a general definition of one kind of system object**. 

**An association** is **a link between classes** that indicates that there is some relationship between these classes. 

When you are developing models during the early stages of the software engineering process, objects represent something in the real world, such as a patient, a prescription, doctor, etc. 

#### Generalization

This allows us to infer that **different members of these classes** have some **common characteristics** e.g. squirrels and rats are rodents.

In a generalization:
* **the attributes and operations associated with higher-level classes are also associated with the lower-level classes**.
* **The lower-level classes** are subclasses **inherit** the attributes and operations from their superclasses. 
* These **lower-level classes** then **add more specific attributes and operations**. 

#### Aggregation

An aggregation model shows how classes that are **collections are composed of other classes**.

**Aggregation models** are similar to **the part-of relationship in semantic data models**. 

## 4. Behavioral Models

Behavioral models are **models of the dynamic behavior of a system** as it is executing. 
* They show what happens or what is supposed to happen when a system **responds** to **a stimulus from its environment**. Stimuli Types:

(1) Data: some data arrives that has to be processed by the system

(2) Events: some event happens that triggers system processing.

### 4.1 Data-driven Modeling

Many business systems are **data-processing systems** that are primarily **driven by data**.

They are controlled by **the data input to the system**, with relatively little external event processing. 

**Data-driven models** show **the sequence of actions involved in processing input data and generating an associated output**. 

They are particularly useful during **the analysis of requirements** as they can be used to show **end-to-end processing in a system**. 

Diagram: 
* Activity
* Sequence

### 4.2 Event-driven Modeling

Event-driven modeling shows **how a system responds to external and internal events**. 

It is based on the **assumption**:
* that a system **has a finite number of states** 
* that events (stimuli) may **cause a transition from one state to another**

#### State Machine Models

**These model the behaviour of the system** in response to external and internal events.

They show **the system’s responses to stimuli** so are often used for modelling real-time systems.

**State machine models** show **system states as nodes** and **events as arcs** between these nodes. When an event occurs, the system moves from one state to another.

Statecharts are an integral part of the UML and are used to represent state machine models.



## 5. Model-driven Engineering (Optional)

**Model-driven engineering (MDE)** is an approach to software development where **models rather than programs are the principal outputs of the development process**. 

The programs that **execute on a hardware/software platform** are **then generated automatically from the models**. 

Proponents of MDE argue that this raises the level of abstraction in software engineering so that engineers no longer have to be concerned with **programming language details** or **the specifics of execution platforms**. 

Usage:

**Model-driven engineering** is still at an early stage of development, and it is unclear whether or not it will have a significant effect on software engineering practice. 

Pros:
* Allows systems to be considered at **higher levels of abstraction**.
* **Generating code automatically** means that it is cheaper to **adapt systems to new platforms**.

Cons:
* Models for abstraction and **not necessarily right for implementation**.
* Saving from generating code may be **outweighed** by the costs of developing translators for new platforms.


Model-driven Architecture

* **Model-driven Architecture(MDA)** was the **precursor** of more general model-driven engineering.
* MDA is **a model-focused approach** to software design and implementation that uses a subset of UML models to describe a system. 
* **Models at different levels of abstraction** are created. From a high-level, platform independent model, it is possible, in principle, to generate a working program without manual intervention. 





