---
layout: post
title:  "[Software Engineering]Requirements Engineering"
date:   2023-9-26
excerpt: "How to make specification in software processing."
tag:
- Software Engineering
comments: false
blog: true
feature: https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/blogHead/directX12partI.jpg
---

## 1. Conceptions

The **process of establishing the services** that a customer requires from a system and the constraints under which it operates and is developed.

The system requirements are **the descriptions of the system services and constraints** that are generated during the requirements engineering process.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/4_requirements.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/4_requirements.png" align="center"></a>
    <figcaption>The requirements engineering process.</figcaption>
</figure>

It may range from **a high-level abstract statement of a service** or of a system constraint to **a detailed mathematical functional specification**.

This is inevitable **as requirements may serve a dual function**:
* May be **the basis for a bid for a contract** - therefore must be **open to interpretation**;
* May be **the basis for the contract itself** - therefore must be **defined in detail**;
* Both these statements may be called requirements.

### 1.1 Types of requirements

(1) User Requirements: **Statements in natural language** plus **diagrams of the services** the system provides and **its operational constraints**. Written for customers.


(2) System Requirements: **A structured document setting out detailed descriptions of the system’s functions**, **services** and **operational constraints**. Defines what should be implemented so may be part of a contract between client and contractor.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/4_user_and_system_requirements.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/4_user_and_system_requirements.png" align="center"></a>
    <figcaption>The user and system process.</figcaption>
</figure>

The requirements must be written so that **several contractors** can **bid for the contract**, **offering**, perhaps, different ways of meeting the client organization’s needs. 

User Requirement: 
* client managers
* system end-users
* client engineers
* contractor managers
* system architects

System Requirements:
* system end-users
* client engineers
* system architects
* software developers

### 1.2 System Stakeholders

**Any person or organization** who is **affected** by **the system in some way and so who has a legitimate interest**. The stakeholder types:
* end users
* system managers
* system owners
* external stakeholders

### 1.3 Agile Methods and Requirements

(1) **Many agile methods** argue that **producing detailed system requirements** is **a waste** of time as **requirements change so quickly**.

(2) The requirements document is therefore always **out of date**.

(3) **Agile methods** usually use **incremental requirements engineering** and may **express requirements as ‘user stories’**.

(4) This is practical for business systems but **problematic for systems** that **require pre-delivery analysis** (e.g. critical systems) or **systems developed by several teams**.

## 2. Functional and non-functional requirements

### 2.1 Functional and non-functional requirements

(1) Functional Requirements

* **Statements of services** the system should provide, how **the system should react to particular inputs** and how **the system should behave in particular situations**.

* May state what the system should not do.

The functional requirements:

* Describe functionality or system services.
* Depend on the type of software, expected users and the type of system where the software is used.
* Functional user requirements may be high-level statements of what the system should do.
* Functional system requirements should describe the system services in detail.


(2) Non-functional Requirements

* **Constraints on the services** or **functions offered by the system** such as **timing constraints**, **constraints on the development process**, **standards**, etc.
* Often apply to the system as a whole rather than **individual features or services**.

These define **system properties and constraints** e.g. reliability, response time and storage requirements, Constraints are I/O device capability, system representations, etc.

**Process Requirements** may also be specified mandating a **particular IDE**, **programming language** or **development method**.

**Non-functional requirements** may be **more critical** than functional requirements. If these are not met, the system may be **useless**.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/4_non_func_reqs.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/4_non_func_reqs.png" align="center"></a>
    <figcaption>The Non-functional Requirements.</figcaption>
</figure>


(3) Domain Requirements

Constraints on the system from the domain of operation


### 2.2 Requirements Imprecision

(1) **Problems arise** when functional requirements are not precisely stated.

(2) **Ambiguous requirements** may be **interpreted** in different ways by developers and users.

### 2.3 Requirements Completeness and Consistency

In principle, requirements should be both complete and consistent.
* Complete: They should include **descriptions of all facilities** required.
* Consistent: There should be **no conflicts or contradictions** in **the descriptions of the system facilities**.

Because of **system and environmental complexity**, it is **impossible** to produce a **complete and consistent requirements** document.


### 2.4 Non-functional Requirements Implementation

* **Non-functional requirements** may **affect** the **overall architecture** of a system rather than **the individual components**. 
    * For example, to ensure that performance requirements are met, you may have to **organize the system to minimize communications between components**.
* A single non-functional requirement, such as a security requirement, may generate a number of **related functional requirements** that define system services that are required. 
    * It may also generate requirements that restrict existing requirements. 

Non-functional classifications
* **Product Requirements**: Requirements which specify that the delivered product must behave in a particular way, execution speed, reliability, etc.
* **Organisational Requirements**: Requirements which are **a consequence of organisational policies and procedures**.  process standard used, implementation requirements.
* **External Requirements**: Requirements which arise from factors which are external to the system and its development process e.g. interoperability requirements, legislative requirements.

### 2.5 Goals and Requirements

Non-functional requirements may be very **difficult to state precisely** and **imprecise requirements may be difficult to verify**. 

(1) Goal: A **general intention of the user** such as ease of use.

(2) Verifiable non-functional Requirement: A **statement** using some **measure that can be objectively tested**.

**Goals are helpful to developers** as **they convey the intentions of the system users**.

### 2.6 Metrics for Specifying non-functional requirements

**Speed**: Processed transactions/second; User/event response time; Screen refresh time.

**Size**: Mbytes; Number of ROM chips.

**Ease of Use**: Training time; Number of help frames.

**Reliability**: Mean time to failure; Probability of unavailability; Rate of failure occurrence; Availability.

**Robustness**: Time to restart after failure; Percentage of events causing failure; Probability of data corruption on failure;

**Portability**: Percentage of target dependent statements; Number of target systems;

## 3. Requirements engineering processes

### 3.1 Requirements Engineering Processes

**The processes used for RE** vary widely depending on the **application domain**, the people involved and the organisation developing the requirements.

Generic Activities:
* Requirements Elicitation.
* Requirements Analysis.
* Requirements Validation.
* Requirements Management.

In practice, RE is **an iterative activity** in which these processes are **interleaved**.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/4_spiral_view.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/4_spiral_view.png" align="center"></a>
    <figcaption>The spiral view of the requrements engineering process.</figcaption>
</figure>

#### 3.1.1 Requirements Elicitation and Analysis

(1) Sometimes called **requirements elicitation** or **requirements discovery**.

(2) Involves technical staff working with customers to find out about the application domain, **the services that the system should provide and the system’s operational constraints**.

(3) May involve **end-users, managers, engineers involved in maintenance, domain experts, trade unions**, etc. These are called **stakeholders**.

##### A. Requirements Elicitation

**Software engineers work with a range of system stakeholders** to find out about:
* the application domain
* the services that the system should provide
* the required system performance
* hardware constraints
* other systems

Processing Stages:
* Requirements discovery: **Interacting with stakeholders** to discover their requirements. **Domain requirements** are also discovered at this stage
* Requirements classification and organization: **Groups related requirements** and organises them into coherent clusters.
* Requirements priorization and negotiation: **Prioritising requirements and resolving requirements conflicts**.
* Requirements specification: Requirements are **documented** and **input into the next round of the spiral**.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/4_req_elicitation.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/4_req_elicitation.png" align="center"></a>
    <figcaption>The requirements elicitation and analysis process.</figcaption>
</figure>

Problems:

(1) Stakeholders dont know what they really want.

(2) Stakeholders **express requirements in their own terms**.

(3) **Different stakeholders** may have **conflicting requirements**.

(4) **Organisational and political factors** may influence **the system requirements**.

(5) **The requirements change** during **the analysis process**. New stakeholders may emerge and the business environment may change.

##### B. Requirements Discovery

The process of :
* **gathering information** about **the required and existing systems** 
* **distilling the user and system requirements from this information**.

**Interaction is with system stakeholders** from managers to external regulators.

Systems normally **have a range of stakeholders**.

##### C. Interviewing

Formal or informal interviews with stakeholders are part of most RE processes.

Types of interview:
* **Closed Interviews** based on **pre-determined list of questions**.
* **Open Interviews** where various issues are explored with stakeholders.

Effective Interviewing
* **Be open-minded**, **avoid pre-conceived ideas about the requirements** and **are willing to** listen to stakeholder.
* **Prompt the interviewee** to get discussions going using a springboard question, a requirements proposal, or by **working together on a prototype system**. 

Interviews in Practice
* A mix of closed and open-ended interviewing
* Interviews are good for getting an overall understanding of what stakeholders do and how they might interact with the system.
* Interviewers need to be **open-minded without pre-conceived ideas** of what the system should do.
* prompt the use to talk about the system by **suggesting requirements** rather than simply asking them what they want.

Problems
* Application specialists may use language to describe their work that **isn't easy for the requirements engineer to understand**.
* Interviews are not good for understanding domain requirements


##### D. Stories and Scenarios

**Scenarios and user stories** are **real-life examples of how a system can be used**.

**Stories and scenarios** are a **description** of **how a system may be used for a particular task**.

Because they are based on a practical situation, **stakeholders can relate to them** and can **comment on their situation with respect to the story**.

(1) Scenarios

A structured form of user story. Scenarios should include:
* A description of **the starting situation**;
* A description of **the normal flow of events**;
* A description of **what can go wrong**;
* Information about **other concurrent activities**;
* A description of **the state when the scenario finishes**.


## 4. Requirements Specification

### 4.1 Requirements Specification

The process of writing down **the user and system requirements in a requirements document**.
* **User requirements** have to be **understandable** by **end-users and customers who do not have a technical background**.
* **System requirements** are more detailed requirements and may **include more technical information**.
* The requirements may be part of a contract for the system development

Ways of writing a system requirements specification:
* Natural language
* Structured natural language
* Design description languages
* Graphics notations
* Mathematical specifications

Guidelines for writing requirements
* Invent a standard format and use it for all requirements.
* Use language in a consistent way. Use shall for mandatory requirements, should for desirable requirements.
* Use text highlighting to identify key parts of the requirement.
* Avoid the use of computer jargon.
* Include an explanation (rationale) of why a requirement is necessary.

### 4.2 Structured Specifications

An approach to **writing requirements where the freedom of the requirements writer is limited** and **requirements are written in a standard way**.

This works well for some types of requirements e.g. requirements for embedded control system but is sometimes **too rigid for writing business system requirements.**


### 4.3 Form-based Specifications

* Definition of the **function or entity**.
* Description of **inputs and where they come from**.
* Description of **outputs and where they go to**.
* Information about the information needed for the computation and other entities used.
* Description of **the action to be taken**.
* Pre and post **conditions** (if appropriate).
* The **side effects (if any) of the function.**

### 4.4 Tabular Specifications
* used to supplement natural language
* particularly useful when you have to **define a number of possible alternative courses of action.**


### 4.5 Requirements document variability

Information in requirements document depends on type of system and the approach to development used.

Systems developed incrementally will, typically, have less detail **in the requirements document**.

Requirements documents standards have been designed e.g. IEEE standard. These are mostly applicable to the requirements for large systems engineering projects.

## 5. Requirements Validation

### 5.1 Requirements Validation

Concerned with demonstrating that **the requirements define the system that the customer really wants**.

**Requirements error costs** are high so validation is very important
* Fixing a requirements error after delivery may cost up to 100 times the cost of fixing an implementation error

### 5.2 Requirements checking

Validity: Does the system provide the functions which best support the customer's needs?

Consistency: Are there any requirements conflicts?

Completeness: Are all functiosn required by the customer?

Realism: Can the requirements be implemented given available budget and technology?

Verifiability: Can the requirements be checked?

### 5.3 Requirements validation techniques

(1) Requirements Reviews

**Regular reviews** should be held **while the requirements definition is being formulated**.

Both client and contractor staff should be **involved in reviews**.

Reviews may be formal (with completed documents) or informal. Good communications between developers, customers and users can resolve problems at an early stage.

(2) Prototyping

(3) Test-case Generation

### 5.4 Review Checks

* Verifiability
* Comprehensibility
* Traceability
* Adaptability


## 6. Requirements Change

### 6.1 Changing Requirements

Reasons:
* The business and technical environment of the system always **changes after installation**.
* The people who pay for a system and the users of that system are rarely the same people.
* Large systems usually have **a diverse user community, with many users having different requirements and priorities** that may be conflicting or contradictory. 

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/4_req_evo.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/4_req_evo.png" align="center"></a>
    <figcaption>The requirements evolution.</figcaption>
</figure>

### 6.2 Requirements Management

Requirements management is the process of **managing changing requirements during the requirements engineering process and system development.**

Reason:
New requirements emerge as a system is being developed and after it has gone into use.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/4_change_managementy.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/DirectXP1Fig/4_change_managementy.png" align="center"></a>
    <figcaption>The requirements change management.</figcaption>
</figure>






