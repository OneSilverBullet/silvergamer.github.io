---
layout: post
title:  "[Software Engineering]Agile Software Development"
date:   2023-12-1
excerpt: "The Agile Software Development."
tag:
- Software Engineering
comments: false
blog: true
feature: https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/blogHead/directX12partI.jpg
---

## Overview

Agile development methods is aiming to radically **reduce the delivery time for working software systems**.

Plan-driven development is essential for some types of system but does not meet these business needs.


Agile Development key points:
* Program specification, design and implementation are **inter-leaved**.
* The system is developed as a **series of versions or increments with stakeholders involved in version specification and evaluation**.
* Frequent delivery of new versions for evaluation.
* **Extensive tool support** used to support development.
* **Minimal documentation**: focus on working code.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/3_com.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/3_com.png" align="center"></a>
    <figcaption>Plan-driven and agile development comparision.</figcaption>
</figure>

(1) Plan-driven development
* A plan-driven approach to software engineering is based around **separate development stages with the outputs to be produced at each of these stages planned in advance**.
* Not necessarily waterfall model, plan-driven incremental development is possible.
* Iteration occurs within activities.

(2) Agile Development
* Specification, Deign Implementation, Testing are inter-leaved.
* The outputs from the development process are decided through a process of negotiation during the software development process.


## 1. Agile Methods

### 1.1 Agile Conceptions
Agile methods:
* Focus on the code rather than the design
* Are based on an iterative approach to software development
* Are intended to deliver working software quickly and **evolve this quickly to meet changing requirements**

The aim of agile methods:
* reduce overheads in the software process
* respond quickly to changing requirements without excessive rework

### 1.2 Agile Manifesto
* Individuals and interactions over processes and tools
* Working software over comprehensive documentation 
* Customer collaboration over contract negotiation 
* Responding to change over following a plan 

### 1.3 The principles of agile methods:

(1) Customer Involvement

Customers should be closely involved throughout **the development process**. Their role is provide and **prioritize new system requirements** and to evaluate the iterations of the system.

(2) Incremental delivery

The software is developed in increments with the customer specifying the requirements to be included in each increment.

(3) People not process

The skills of **the development team should be recognized and exploited**. Team members should be left to develop their own ways of working without prescriptive processes.

(4) Embrace change

Expect the system requirements to change and so **design the system to accommodate these changes**.

(5) Maintain simplicity

Focus on simplicity in both the software being developed and in the development process. Wherever possible, actively work to eliminate complexity from the system.

Applicability:

(1) Product development where a software company is developing a small or medium-sized product for sale. 

(2) Custom system development within an organization, where **there is a clear commitment from the customer to become involved in the development process and where there are few external rules and regulations that affect the software**.

## 2. Agile Development Techiniques

### 2.1 Extreme Programming

Extreme Programming (XP) takes an ‘extreme’ approach to iterative development. 
* New versions may be built **several times per day**;
* Increments are delivered to customers **every 2 weeks**;
* All tests must be run for **every build and the build is only accepted if tests run successfully**.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/3_extreme.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/3_extreme.png" align="center"></a>
    <figcaption>The extreme programming release cycle.</figcaption>
</figure>

Extreme Programming Practices:

(1) Incremental Planning

**Requirements are recorded on story cards** and **the stories to be included in a release** are determined by the time available and their relative priority. The developers break these stories into development ‘Tasks’.

(2) Small Release

The **minimal useful set of functionality** that provides business value is developed first. Releases of the system are **frequent and incrementally add functionality** to the first release.

(3) Simple Design

**Enough design** is carried out to meet the current requirements and **no more**.

(4) Test-first Development

An **automated unit test framework** is used to write tests for a new piece of functionality before that functionality itself is implemented.

(5) Refactoring

All developers are expected to **refactor the code continuously** as soon as possible code improvements are found. This keeps the **code simple** and **maintainable**.

(6) Pair Programming

**Developers work in pairs**, checking each other's work and providing the support to always **do a good job**.

(7) Collective Ownership

The pairs of developers work on all areas of the system, so that no islands of expertise develop and all the developers take responsibility for all of the code. Anyone can change anything.

(8) Continuous Integration

As soon as the work on a task is complete, it is integrated into the whole system. After any such integration, **all the unit tests in the system must pass**.

(9) Sustainable Pace

Large amounts of overtime are not considered acceptable as the net effect is often to **reduce code quality and medium term productivity**

(10) On-site Customer

**A representative of the end-user of the system (the customer)** should be available full time for the use of the XP team. In an extreme programming process, **the customer is a member of the development team** and **is responsible for bringing system requirements to the team for implementation**.

### 2.2 Extreme Programming & Agile Princiles

(1) Incremental development is supported through small frequent system releases.

(2) Customer involvement means full-time customer engagement with the team.

(3) People not process through pair programming, collective ownership and a process that avoids long working hours.

(4) Change supported through regular system releases.

(5) Maintaining simplicity through constant refactoring of code.

Extreme programming **has a technical focus** and is **not easy to integrate with management practice in most organizations**.

Consequently, while **agile development** uses practices from XP, **the method as originally defined is not widely used**. Key practices:
* User Stories for specification
* Refactoring
* Test-first Development
* Paire Programming

#### 2.2.1 User Stories for Requirements

**A customer or user is part of the XP team** and is **responsible for making decisions on requirements**.

User requirements are expressed as **user stories** or **scenarios**.

These are written on **cards** and the development team break them down into implementation tasks. These tasks are **the basis of schedule and cost estimates**.

#### 2.2.2 Refactoring

Conventional wisdom in software engineering is to **design for change**. It is **worth spending time and effort anticipating changes** as this **reduces costs later in the life cycle**.
 
XP maintains that **this is not worthwhile as changes cannot be reliably anticipated**. Rather, it proposes constant code improvement (refactoring) to make changes easier when they have to be implemented.

* Programming team look for possible software improvements and **make these improvements even where there is no immediate need for them.**
* This **improves the understandability of the software** and **so reduces the need for documentation**.
* Changes are easier to make because the **code is well-structured and clear**.
* Some changes **requires architecture refactoring** and this is much more **expensive**.

Examples:
* Re-organization of a class hierarchy to **remove duplicate code**.
* Tidying up and renaming attributes and methods to **make them easier to understand**.
* The replacement of inline code with calls to methods that **have been included in a program library**.

#### 2.2.3 Test-first Development

**Testing is central to XP** and XP has developed an approach where **the program is tested after every change has been made.**

XP testing features:
* Test-first development.
* Incremental test development from scenarios.
* User involvement in **test development and validation**.
* **Automated test harnesses** are used to **run all component tests** each time that a new release is built.

Test-Driven Development:

* Writing tests before **code clarifies the requirements to be implemented**.
* Tests are written as **programs** rather than data so that they can be executed automatically. The test includes a check that it has executed correctly.
    * Usually relies on a **testing framework** such as Junit.
* **All previous and new tests are run automatically** when new functionality is added, thus checking that the new functionality has not introduced errors.

##### Customer Involvment

The role of the customer in the testing process is **to help develop acceptance tests for the stories** that are to be implemented in the next release of the system. 

The customer who is part of the team **writes tests as development proceeds**. All new code is therefore validated to ensure that it is what the customer needs. 

People adopting the customer role have limited time available and so cannot work full-time with the development team. **They may feel that providing the requirements was enough of a contribution and so may be reluctant to get involved in the testing process.**

##### Test Automation

(1) Test automation means that tests are written as executable components before the task is implemented.

(2) As testing is automated, there is always a set of tests that can be quickly and easily executed

##### Problems

Programmers prefer programming to testing and sometimes they take short cuts when writing tests. For example, they may write **incomplete tests that do not check for all possible exceptions that may occur**.

**Some tests can be very difficult to write incrementally**. For example, in a complex user interface, it is often difficult to write unit tests for the code that implements the ‘display logic’ and workflow between screens. 

**It difficult to judge the completeness of a set of tests**. Although you may have a lot of system tests, your test set may not provide complete coverage.  

#### 2.2.4 Pair Programming

Pair programming involves **programmers working in pairs,  developing code together**.

This helps develop **common ownership of code and spreads knowledge across the team**.

It serves as **an informal review process** as each line of code is looked at by more than 1 person.

It **encourages refactoring** as the whole team can benefit from **improving the system code**.

Pair programming is not necessarily inefficient and **there is some evidence that suggests that a pair working together is more efficient than 2 programmers working separately.**

## 3. Agile Project Management

The principal responsibility of software project managers is to **manage the project so that the software is delivered on time and within the planned budget for the project.**

**The standard approach** to project management is **plan-driven**. Managers draw up a plan for the project showing what should be delivered, when it should be delivered and who will work on the development of the project deliverables. 

**Agile project management** requires a different approach, which is adapted to incremental development and the practices used in agile methods. 


### 3.1 Scrum

Scrum is an agile method that focuses on managing iterative development rather than specific agile practices.
* **The initial phase is an outline planning phase where you establish the general objectives for the project and design the software architecture.** 
* This is followed by **a series of sprint cycles**, where each cycle develops an increment of the system. 
* **The project closure phase wraps up the project**, completes required documentation such as system help frames and user manuals and assesses the lessons learned from the project.

#### 3.1.1 Scrum Terminology

(1) Development team

A self-organizing group of software developers, which should be no more than 7 people. They are responsible for developing the software and other essential project documents.

(2) Potentially shippable product increment

The software increment that is delivered from a sprint.

(3) Product backlog

**This is a list of ‘to do’ items which the Scrum team must tackle.** They may be feature definitions for the software, software requirements, user stories or descriptions of supplementary tasks that are needed, such as architecture definition or user documentation.

(4) Product owner

An individual (or possibly a small group) whose job is to identify product features or requirements, prioritize these for development and continuously review the product backlog to ensure that the project continues to meet critical business needs. **The Product Owner can be a customer but might also be a product manager in a software company or other stakeholder representative.**

(5) Scrum

A daily meeting of the Scrum team that reviews progress and prioritizes work to be done that day. Ideally, this should be a short face-to-face meeting that includes the whole team.

(6) Scrum Master

The ScrumMaster is responsible for ensuring that the Scrum process is followed and guides the team in the effective use of Scrum. He or she is responsible for interfacing with the rest of the company and for ensuring that the Scrum team is not diverted by outside interference. The Scrum developers are adamant that the ScrumMaster should not be thought of as a project manager. Others, however, may not always find it easy to see the difference.

**protect the development team from external distractions.**


The ‘Scrum master’ is a facilitator who **arranges daily meetings, tracks the backlog of work to be done, records decisions, measures progress against the backlog and communicates with customers and management outside of the team**.

The whole team attends short daily meetings (Scrums) where **all team members share information**, describe their progress since the last meeting, problems that have arisen and what is planned for the following day. 


(7) Sprint

A development iteration. Sprints are usually 2-4 weeks long.


(8) Velocity

**An estimate of how much product backlog effort that a team can cover in a single sprint.**  Understanding a team’s **velocity** helps them estimate what can be covered in a sprint and **provides a basis for measuring improving performance**.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/3_scrum.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/3_scrum.png" align="center"></a>
    <figcaption>The scrum sprint cycle.</figcaption>
</figure>

* **Sprints** are fixed length, normally 2–4 weeks.  
* **The selection phase** involves all of the project team who work with the customer to select the features and functionality from the product backlog to be developed during the sprint. 
* **The sprint cycle**: During this stage the team is isolated from the customer and the organization, with all **communications channelled through the so-called ‘Scrum master’**.

#### Benefits

(1) The product is broken down into a set of manageable and understandable chunks.

(2) Unstable requirements do not hold up progress.

(3) **The whole team have visibility of everything** and consequently team communication is improved.

(4) **Customers see on-time delivery of increments and gain feedback** on how the product works.

(5) **Trust between customers and developers is established** and a positive culture is created in which everyone expects the project to succeed.


### 3.2 Scaling Agile Methods

Scaling up agile methods involves changing these to **cope with larger, longer projects** where there are multiple development teams, perhaps working in different locations.

**‘Scaling up’**: is concerned with using agile methods for **developing large software systems that cannot be developed by a small team**.

**‘Scaling out’** is concerned with how agile methods can be **introduced across a large organization** with many years of software development experience.

When scaling agile methods it is important to maintain agile fundamentals:
* Flexible planning
* Frequent system releases
* Continuous integration,
* Test-driven development and good team communications.

Practical Problems with agile methods:

(1) The informality of agile development **is incompatible with the legal approach to contract definition** that is commonly used in large companies.

(2) **Agile methods are most appropriate for new software development** rather than software maintenance. Yet **the majority of software costs in large companies** come from **maintaining their existing software systems**.

(3) Agile methods are designed for **small co-located teams yet much software development now involves worldwide distributed teams**.  


#### 3.2.1 Agile maintance

Key problems:
* Lack of product documentation
* Keeping customers involved in the development process
* Maintaining the continuity of the development team: For long-lifetime systems, this is a real problem as the original developers will not always work on the system.

### 3.3 Compairision

A plan-driven approach: have a very detailed specification and design before moving to implementation.

Agile methods: where you deliver the software to customers and get rapid feedback from them.

Agile methods are most effective when **the system can be developed with a small co-located team who can communicate informally**. This may not be possible for large systems that require larger development teams so **a plan-driven approach** may have to be used. 


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/3_fact.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/3_fact.png" align="center"></a>
    <figcaption>The agile and plan-based factors.</figcaption>
</figure>
 
Q: How large is the system is being developed?

A: **Agile methods are most effective** a **relatively small co-located team** who can communicate informally. 

Q: What type of system is being developed?

A: **Systems that require a lot of analysis before implementation need a fairly detailed design to carry out this analysis.** 

Q: What is the expected system lifetime?

A: Long-lifetime systems require documentation to communicate the intentions of the system developers to the support team. 

Q: Is the system subject to external regulation?

A: If a system is regulated you will probably be required to produce detailed documentation as part of the system safety case. 

About People and Team:

It is sometimes argued that **agile methods require higher skill levels** than plan-based approaches in which programmers simply translate a detailed design into code.

Design documents may be required if the **team is dsitributed**.

**IDE support for visualisation and program analysis** is **essential** if design documentation is not available.


### 3.4 Agile for Large System

Large systems are **usually collections of separate, communicating systems, where separate teams develop each system**. Frequently, these teams are working in different places, sometimes in different time zones. 

Large systems are ‘brownfield systems’, that is they include and interact with a number of existing systems. **Many of the system requirements are concerned with this interaction and so don’t really lend themselves to flexibility and incremental development.**

Where **several systems are integrated to create a system**, a significant fraction of the development is concerned with system configuration rather than original code development.

Large systems and their development processes are often **constrained by external rules and regulations limiting the way that they can be developed.**

Large systems have a long procurement and development time. It is **difficult to maintain coherent teams** who know about the system over that period as, inevitably, people move on to other jobs and projects.

Large systems usually have a diverse set of stakeholders. **It is practically impossible to involve all of these different stakeholders in the development process**. 


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/3_largface.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/3_largface.png" align="center"></a>
    <figcaption>The factors for large system.</figcaption>
</figure>


 
### 3.5 Scaling up to large system

* A **completely incremental approach** to requirements engineering is impossible.
* There cannot be a **single product owner or customer representative**.
* For large systems development, it is not possible to focus only on the code of the system.  
* **Cross-team communication mechanisms** have to be designed and used. 
* **Continuous integration** is practically impossible. However, it is essential to maintain frequent system builds and regular releases of the system. 


### 3.6  Multi-team Scrum

(1) Role replication 

Each team has a Product Owner for their work component and ScrumMaster. 

(2) Product architects 

Each team chooses a product architect and these architects collaborate to design and evolve the overall system architecture.

(3) Release alignment 

The dates of product releases from each team are aligned so that a demonstrable and complete system is produced.

(4)Scrum of Scrums 

There is a daily Scrum of Scrums where representatives from each team **meet to discuss progressand plan work to be done**.





