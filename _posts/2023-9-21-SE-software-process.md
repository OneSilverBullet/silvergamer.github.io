---
layout: post
title:  "[Software Engineering]Software Process"
date:   2023-9-26
excerpt: "The Software Development Models."
tag:
- Software Engineering
comments: false
blog: true
feature: https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/blogHead/directX12partI.jpg
---

## 1. Conceptions

### 1.1 The Software Process

Software Process: **A structured set of activities required** to develop a software system. Many different software processes but all involve:
* **Specifications**: defining what the system should do.
* **Design and Implementation**: defining the organization of the system and implementing the system.
* **Validation**: checking that it does what the customer wants.
* **Evaluation**: changing the system in response to changing customer needs.

**Software Process Model**: an abstract representation of a process.

### 1.2 Software Process Descriptions

Process Descriptions: we usually talk about the activities in these processes such as:
* **specifying a data model**
* **designing a user interface**
* **the ordering of these activities**

Process Description includes:
* Products: outcomes of a process activity.
* Roles: reflect the responsibilities of the people involved in the process.
* Pre- and post-conditions: statements that are true before and after a process activity has been enacted or a product produced.

### 1.3 Plan-driven & Agile Processes

**Plan-driven processes** are processes where **all of the process activities are planned in advance** and progress is measured against this plan.

**Agile Processes**: **planning is incremental** and it is easier to change the process to reflect **changing customer requirements**.

In practice, most practical processes include elements of both plan-driven and agile approaches. There are no right or wrong software processes.


## 2. Software Process Models

### 2.1 The Waterfall Model

Conception: **Plan-driven model**. Separate and **distinct phases of specificatio**n and development.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/waterfall.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/waterfall.png" align="center"></a>
    <figcaption>Waterfall Model Process.</figcaption>
</figure>

There are separate identified phases in the waterfall model:
* Requirements Analysis and Defination
* System and Software Design
* Implementation and Unit Testing
* Integration and System Testing
* Operation and Maintenance

The main drawback:

(1) The **main drawback** of the waterfall model is **the difficulty of accommodating change after the process is underway**. In principle, a phase has to be complete before moving onto the next phase.

(2) Inflexible partitioning of the project into distinct stages makes it difficult to respond to changing customer requirements.
* This model is only appropriate when the requirements are well-understood and changes will be fairly limited during the design process. 
* Few business systems have stable requirements.

The waterfall model is mostly used for large systems engineering projects where a system is developed at several sites.


### 2.2 Incremental Development

Incremental development is based on **the idea of developing an initial implementation**, exposing this to user comment and **evolving it through several versions until an adequate system has been developed**

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/incremental.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/incremental.png" align="center"></a>
    <figcaption>Incremental Model Process.</figcaption>
</figure>

Benefits:

(1) **The cost of accommodating changing customer requirements is reduced**.

(2) It is easier to **get customer feedback** on the development work that has been done. 

(3) **More rapid delivery and deployment of useful software to the customer** is possible.

Problems:

(1) **The process is not visible.** If systems are developed quickly, it is not cost-effective to produce documents that reflect every version of the system. 

(2) System structure tends to **degrade** as new increments are added. 

### 2.3 Integration and configuration

Conception: Based on **software reuse** where **systems are integrated from existing components or application systems** (sometimes called COTS -Commercial-off-the-shelf systems).
* Reused elements may be configured to **adapt their behaviour and functionality** to a user’s requirements.
* Reuse is now the standard approach for building many types of business system.

Types of reusable software:
* Stand-alone application systems
* Collections of objects that are developed as a package.
* Web service.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/reuse.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/reuse.png" align="center"></a>
    <figcaption>Reuse-oriented software engineering.</figcaption>
</figure>

The Key Process Stages:
* Requirements Specification
* Software Discovery and Evaluation
* Requirements Refinement
* Application System Configuration
* Component Adaptation and Integration

Benefits:

(1) **Reduced costs and risks** as less software is developed from scratch.

(2) Faster delivery and deployment of system.

Problems:

(1) But **requirements compromises are inevitable** so system may **not meet real needs of users**.

(2) Loss of control over evolution of reused system elements.


## 3. Process Activities

Real software processes are **inter-leaved sequences** of:
* technical
* collaborative
* managerial activities

with the overall goal of 
* specifying, 
* designing, 
* implementing 
* testing a software system. 

The **four basic process activities** of **specification, development, validation and evolution** are organized differently in different development processes. 


### 3.1 Software Specification

The **process of establishing what services are required** and the constraints on the system's operation and development.

Requirements engineering process:
* Requirements Elicitation and Analysis
* Requirements Specification
* Requirements Validation


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/requires.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/requires.png" align="center"></a>
    <figcaption>The requirements engineering process.</figcaption>
</figure>

### 3.2 Software Design and Implementation

Conception: The process of converting the system specification into an executable system.
* Software Design: design a software structure that realises the specification.
* Implementation: Translate the structure into an executable program.


The activities of design and implementation are closely related and may be inter-leaved.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/design.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/design.png" align="center"></a>
    <figcaption>The general model of the design process.</figcaption>
</figure>

Design Activities:
* Architectural Design: Identify the **overall structure of the system**, **the principal components** (subsystems or modules), their **relationships** and **how they are distributed**.
* Database Design: Design the system data structures and how these are to be represented in a database. 
* Interface Design: Define the interfaces between system components.
* Component Selection and Design: search for reusable components.

 
System Implementation:

(1) The software is implemented either by:
* developing a program 
* configuring an application system.

(2) **Programming is an individual activity** with no standard process.

(3) **Debugging is the activity of finding program faults and correcting these faults.**

**Design and implementation** are **interleaved activities for most types of software system**.

### 3.3 Software Validation

Verification: do the right product.

Validation: do the product right.

Verification and Validation is intended to show that **a system conforms to its specification** and **meets the requirements of the system customer**.
* Involves **checking and review processes** and **system testing**.

System testing involves executing the system with test cases that are derived from the specification of the real data to be processed by the system.
* Testing is the most commonly used V&V activity.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/testing.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/testing.png" align="center"></a>
    <figcaption>The stages of testing.</figcaption>
</figure>

Testing Stages:

(1) Component Testing

Components may be functions or objects or coherent groupings of these entities.

(2) System Testing

Testing of the system as a whole. Testing of emergent properties is particularly important.

(3) Customer Testing

Testing with customer data to check that the system meets the customer’s needs.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/V.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/V.png" align="center"></a>
    <figcaption>Testing phases in a plan-driven software process V-model.</figcaption>
</figure>


### 3.4 Software Evolution

Software is inherently flexible and can change.

As requirements change through changing business circumstances, the software that supports the business must also **evolve and change**.



<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/evolution.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/evolution.png" align="center"></a>
    <figcaption>The evaluation process.</figcaption>
</figure>



## 4. Coping with change

Change is inevitable in all large software projects.
* Business Changes
* New technologies
* Platforms Changes

Change leads to rework so **the costs of change** include both **rework** (e.g. re-analysing requirements) as well as **the costs of implementing new functionality**.


### 4.1 Reducing the costs of rework

Change anticipation, where the software process includes activities that can anticipate possible changes before significant rework is required.

Change tolerance, where the process is designed so that changes can be accommodated at relatively low cost.

### 4.2 Coping with changing reuqirements

#### 4.2.1 Software Prototyping

**System prototyping**, where a version of the system or part of the system is developed quickly to check the customer’s requirements and the feasibility of design decisions. This approach supports change anticipation. 

A prototype is an initial version of a system used to demonstrate concepts and try out design options. Can be used in:
* The requirements engineering process to help with requirements elicitation and validation;
* In design processes to explore options and develop a UI design;
* In the testing process to run back-to-back tests.

Benefits:

* Improved system usability.
* A closer match to users’ real needs.
* Improved design quality.
* Improved maintainability.
* Reduced development effort.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/prototype.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/prototype.png" align="center"></a>
    <figcaption>The process of prototype development.</figcaption>
</figure>

Prototype Development:
* rapid prototyping languages or tools
* involve leaving out functionality
    * **focus on areas of the product that are not well-understood;**
    * Error checking and recovery may not be included in the prototype;
    * **Focus on functional rather than non-functional requirements such as reliability and security**

Prototypes should be discarded after development as they are **not a good basis for a production system.**
* be impossible to tune the system to meet **non-functional requirements**
* prototypes are normally **undocumented**
* the prototype structure is usually **degraded through rapid change**.
* the prototype probably will **not meet normal organisational quality standards**.


#### 4.2.2 Incremental Delivery

**Incremental delivery**, where system increments are delivered to the customer for comment and experimentation. This supports both change avoidance and change tolerance. 

**The development and delivery** is broken down into **increments** with each **increment delivering part** of the **required functionality**.
* User requirements are prioritised and the highest priority requirements are included in early increments.
* Once the development of an increment is started, the requirements are frozen though requirements for later increments can continue to evolve.

**The Incremental Development & Incremental Delivery**

Incremental Development:
* Develop the system in **increments** and evaluate each increment before proceeding to the development of the next increment;
* **Normal approach used in agile methods**;
* **Evaluation** done by user/customer proxy.

Incremental Delivery:
* Deploy an increment for use by end-users;
* More realistic evaluation about practical use of software;
* Difficult to implement for replacement systems as increments have less functionality than the system being replaced.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/incrementaldelivery.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/incrementaldelivery.png" align="center"></a>
    <figcaption>The process of incremental delivery.</figcaption>
</figure>

Benefits:

(1) Customer value can be delivered with each increment so system functionality is availiable earlier.

(2) Early increments act as a prototype to help elicit requirements for later increments.

(3) Lower risk of overall project failure.

(4) The highest priority system services tend to receive the most testing.

Problems:

(1) Most systems **require a set of basic facilities** that are used by different parts of the system.

(2) **The essence of iterative processes** is that the specification is developed in **conjunction with the software.** 


## 5. Process Improvement

Process improvement as a way of enhancing the quality of their software, reducing costs or accelerating their development processes. 

Process improvement means **understanding existing processes** and **changing these processes to increase product quality and/or reduce costs and development time**. 

### 5.1 The process maturity approach

Conception: focuses on **improving process and project management** and **introducing good software engineering practice**. 

The level of process maturity reflects the extent to which **good technical and management practice has been adopted in organizational software development processes**. 


### 5.2 The agile approach

Conception: focuses on **iterative development and the reduction of overheads** in the software process. 

The primary characteristics of agile methods are **rapid delivery of functionality** and **responsiveness to changing customer requirements.**


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/improve.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/improve.png" align="center"></a>
    <figcaption>The process of improvement cycle.</figcaption>
</figure>



