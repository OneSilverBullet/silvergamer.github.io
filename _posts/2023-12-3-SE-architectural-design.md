---
layout: post
title:  "[Software Engineering]Architectural Design"
date:   2023-12-3
excerpt: "The architectural design."
tag:
- Software Engineering
comments: false
blog: true
feature: https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/blogHead/directX12partI.jpg
---

## Overviews


**Architectural design** is concerned with understanding how a software system should be **organized and designing the overall structure of that system**.

**Architectural design** is the critical link between design and requirements engineering, as it identifies the main structural components in a system and the relationships between them. 

**The output of the architectural design process** is **an architectural model** that describes how the system is organized as a set of communicating components. 


Agile: It is generally accepted that **an early stage of agile processes** is to **design an overall systems architecture**.

**Refactoring the system architecture** is usually **expensive** because it affects so many components in the system
 
### Architectural Abstraction

**Architecture in the small** is concerned with **the architecture of individual programs**. At this level, we are concerned with the way that an individual program is decomposed into components.  

**Architecture in the large** is concerned with **the architecture of complex enterprise systems** that include other systems, programs, and program components. These enterprise systems are distributed over different computers, which may be owned and managed by different companies.  

Advantage of explicit architecture:
* Stakeholder communication
* System Analysis
* Large-scale reuse

Architecture Representation:
* **Simple, informal block diagrams** showing entities and relationships are the most frequently used method for **documenting software architectures**.
* These have been **criticized** because they lack semantics, do not show **the types of relationships between entities** nor **the visible properties of entities in the architecture**.
* Depends on the **use of architectural models**. The requirements for model semantics 
depends on how the models are used.


Usage of Architectural Models:
* As a way of **facilitating discussion** about the system design 
* As a way of **documenting an architecture** that has been designed 




## 1. Architectural Design Descision

### 1.1 Architectural Design Descisions

Architectural design is a creative process, so the **process differs depending on the type of system being developed**.
* a number of common decisions **span all design processes** 
*  these decisions affect the **non-functional characteristics of the system**.

The decisions:
* Is there a generic application architecture that can act as a template for the system that is being designed?
* How will the system be distributed across hardware cores or processors?
* What architectural patterns or styles might be used?
* What Strategy will be used to control the operation of the components in the system?
* How should the architecture of the system be documented?
* What architectural organization is best for delivering the non-functional requirements of the system?
* How will the structural components in the system be decomposed into sub-components?
* What will be the fundamental approach used to structure the system?

### 1.2 Architecture Reuse

(1) **Systems in the same domain** often have **similar architectures** that reflect domain concepts.

(2) **Application Product Lines** are built around **a core architecture with variants** that satisfy particular customer requirements.

(3) The architecture of a system may be designed around **one of more architectural patterns** or 'styles'.

### 1.3 Architectural and System Characteristics

* Performance: minimize communication, use large rather than fine-grain components.
* Security: layered architecture with critical assets in the inner layers.
* Safety: Localize safety-critical features in a small number of sub-systems.
* Availability: Include redundant components and mechanisms for fault tolerence.
* Maintainability: Fine-grain, replaceable components.

## 2. Architectural Views

Each architectural model **only shows one view or perspective of the system**:
* Logical View: shows the key abstractions in the system as objects or object classes. 
* Physical View: shows the system hardware and how software components are distributed across the processors in the system.
* Development View: which shows how the software is decomposed for development.
* Process View: shows how, at run-time, the system is composed of interacting processes

Related using use cases or scenarios

**Architectural description languages (ADLs)** have been developed but are not widely used

## 3. Architectural Patterns

### 3.1 Conceptions

Patterns are a **means of representing, sharing and reusing knowledge**.

An architectural pattern is **a stylized description** of good design practice, which has been **tried and tested in different environments**.

### 3.2 The model-view-controller pattern MVC


Separates presentation and interaction from the system data. The system is structured into three **logical components** that interact with each other. 
* The Model component manages the system data and associated operations on that data. 
* The View component defines and manages how the data is presented to the user.
* The Controller component manages user interaction (e.g., key presses, mouse clicks, etc.) and passes these interactions to the View and the Model. See Figure 6.3.

Used: Used when there are **multiple ways to view and interact with data**. Also used when the future requirements for interaction and presentation of data are unknown.

Advantages: 
* Allows the data to change independently of its representation and vice versa. 
* Supports presentation of the same data in different ways with changes made in one representation shown in all of them. 

Disadvantages:
* Can involve additional code and code complexity when the data model and interactions are simple.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/a_mvc.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/a_mvc.png" align="center"></a>
    <figcaption>The architecture of MVC.</figcaption>
</figure>

### 3.3 The Layered Architecture

* Used to model the interfacing of sub-systems.
* Organises the system into **a set of layers** (or abstract machines) each of which provide a set of services.
* Supports the **incremental development of sub-systems** in different layers. 
* When a **layer interface changes**, only the **adjacent layer** is affected.
* However, often artificial to structure systems in this way.


**Organizes the system** into **layers with related functionality associated with each layer**. **A layer provides services to the layer above it** so **the lowest-level layers represent core services** that are likely to be used throughout the system.

Usage: 
* Used when **building new facilities** on **top of existing systems**; 
* when the development is **spread across several teams** with each **team responsibility for a layer of functionality**; 
* when there is **a requirement for multi-level security**.

Pros:
* Allows replacement of entire layers so long as the interface is maintained.
* Redundant facilities (e.g., authentication) can be provided in each layer to increase the dependability of the system.

Cons:
* In practice, providing a clean separation between layers is often difficult and a high-level layer may have to interact directly with lower-level layers rather than through the layer immediately below it. 
* Performance can be a problem because of multiple levels of interpretation of a service request as it is processed at each layer.


### 3.4 The Repository Architecture

Sub-systems must **exchange data**. This may be done in two ways:
* Shared data is held in a **central database** or **repository** and may be accessed by all sub-systems;
* Each sub-system maintains its own database and **passes data explicitly to other sub-systems**.


All data in a system is managed in **a central repository** that is **accessible to all system components**. Components do not **interact directly, only through the repository**. 

Used: 
* When large amounts of data are to be shared, the **repository model of sharing is most commonly used a this is an efficient data sharing mechanism.**
* You may also use it in data-driven systems where the inclusion of data in the repository triggers an action or tool.


Pros:
* Components can be independent—they do not need to know of the existence of other components. 
* **Changes made by one component can be propagated to all components**. All data can be managed consistently (e.g., backups done at the same time) as it is all in one place. 

Cons:
* The repository is a single point of failure so **problems in the repository affect the whole system.** 
* May be **inefficiencies** in organizing all communication through the repository. 
* Distributing the repository across several computers may be difficult.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/a_raide.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/a_raide.png" align="center"></a>
    <figcaption>The architecture of repository.</figcaption>
</figure>



### 3.5 The Client-server Architecture

**Distributed system model** which shows how data and processing is distributed across a range of components.
* Can be implemented on a single computer.
* Set of **stand-alone servers** which provide specific services such as printing, data management, etc.
* Set of clients which **call on these services**.
* Network which **allows clients to access servers**.

In a client–server architecture, the functionality of the system is organized into services, **with each service delivered from a separate server**. Clients are users of these services and access servers to make use of them.

Usage: Used when data in **a shared database has to be accessed from a range of locations**. Because servers can be replicated, may also be used when the load on a system is variable.

Pros:
* The principal advantage of this model is that servers can be distributed across a network. * General functionality (e.g., a printing service) can be available to all clients and does not need to be implemented by all services. 

Cons:
* Each service is a single point of failure so susceptible to denial of service attacks or server failure.
* Performance may be unpredictable because it depends on the network as well as the system. 
* May be management problems if servers are owned by different organizations.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/a_cs.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/a_cs.png" align="center"></a>
    <figcaption>The architecture of client and server.</figcaption>
</figure>

### 3.6 The Pipe and filter architecture

Functional transformations process their inputs to produce outputs.
* Not really suitable for interactive systems.

Description: **The processing of the data in a system** is organized so that **each processing component (filter)** is discrete and **carries out one type of data transformation**. The data flows (as in a pipe) from one component to another for processing. 


Usage: Commonly used in data processing applications (both batch- and transaction-based) where inputs are processed in separate stages to generate related outputs.

Prons:
* Easy to understand and supports transformation reuse. 
* Workflow style matches the structure of many business processes. 
* Evolution by adding transformations is straightforward. 
* Can be implemented as either a sequential or concurrent system.

Cons:
* The format for data transfer has to be agreed upon between communicating transformations.
* Each transformation must parse its input and unparsed its output to the agreed form. This increases system overhead and may mean that it is impossible to reuse functional transformations that use incompatible data structures.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/a_pf.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/a_pf.png" align="center"></a>
    <figcaption>The architecture of pipeline filter.</figcaption>
</figure>


## 4. Application Architectures

### 4.1 Application Architectures

**Application systems** are designed to **meet an organizational need**.

As businesses have much in common, their application systems also tend to have **a common architecture** that **reflects the application requirements**.

A **generic application architecture** is an architecture for a type of software system that may be configured and adapted to **create a system that meets specific requirements**.

Use of application architecture:
* as a **starting point** for architectural design
* as a **design checklist**
* as a **way of organising the work** of the development team
* as a **means of assessing** components for reuse
* as a **vocabulary** for talking about application types

Example:
* data processing application
* transaction processing applications(widely used)
    * e-commerce systems
    * reservation systems
* event processing systems
* language processing systems(widely used)
    * compilers
    * command interpreters


### 4.2 Transaction Processing Systems

**Process user requests for information from a database or requests** to **update the database**.
* Users make asynchronous requests for service which are then processed by a transaction manager.

From a user perspective, a **transaction**:
* any **coherent sequence of operations that satisfies a goal**.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/a_tpa.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/a_tpa.png" align="center"></a>
    <figcaption>The application architecture of transaction process application.</figcaption>
</figure>

### 4.3 Information Systems Architecture

Information systems have **a generic architecture** that can be **organized as a layered architecture**.

These are transaction-based systems as interaction with these systems generally involves database transactions.

Layers include:
* user interface
* user communications
* information retrieval
* system database

These systems are often implemented as **multi-tier client server architectures**.



### 4.3 Language Processing Systems

(1) Accept **a natural or artificial language as input** and generate some other representation of that language. 

(2) May include an interpreter to **act on the instructions in the language that is being processed**.

(3) Used in situations where the easiest way to solve a problem is to **describe an algorithm or describe the system data**.
* Meta-case tools process tool descriptions, method rules, etc and generate tools.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/a_lps.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/a_lps.png" align="center"></a>
    <figcaption>The application architecture of language processing system.</figcaption>
</figure>

#### 4.3.1 Compiler Component

**A lexical analyzer**: takes input languages tokens and **converts them to an internal form**.
* **A symbol table**, which holds information about the names of entities (variables, class names, object names, etc.) used in the text that is being translated.
* A syntax analyzer, which checks the syntax of the language being translated. 
* A syntax tree, which is an internal structure representing the program being compiled.


A semantic analyzer that uses information from the syntax tree and the symbol table to check the semantic correctness of the input language text. 
A code generator that ‘walks’ the syntax tree and generates abstract machine code.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/a_ra.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/a_ra.png" align="center"></a>
    <figcaption>The repository compiler architecture of language processing system.</figcaption>
</figure>



<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/a_ca.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/a_ca.png" align="center"></a>
    <figcaption>The pipe and filter compiler architecture of language processing system.</figcaption>
</figure>



