---
layout: post
title:  "[Software Engineering]Testing and Evaluation"
date:   2023-12-7
excerpt: "The Software testing and evaluation."
tag:
- Software Engineering
comments: false
blog: true
feature: https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/blogHead/directX12partI.jpg
---

## 1. Overviews

Testing is intended to show that a program does what it is intended to do and **to discover program defects before it is put into use**. 


Testing is part of **a more general verification and validation process**, which also includes static validation techniques.

Goals:
* Validation Testing: To demonstrate to the developer and the customer that the software **meets its requirements**. 
    * For **custom software**, this means that there should be at least one test for every requirement in the requirements document. 
    * For **generic software products**, it means that there should be tests for all of the system features, plus combinations of these features, that will be incorporated in the product release.  
    * A successful test shows that the system operates as intended.
* Defect Testing: To discover situations in which **the behavior of the software is incorrect, undesirable or does not conform to its specification**.
    * **Defect testing** is concerned with rooting out undesirable system behavior such as system crashes, unwanted interactions with other systems, **incorrect computations and data corruption**.
    * A successful test is a test that makes the system perform incorrectly and so exposes a defect in the system

An input-output model of program testing:

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/test_io.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/test_io.png" align="center"></a>
    <figcaption>The input-output testing.</figcaption>
</figure>

### 1.1 V & V

Verification: "Are we building the product right”.
* The software should conform to its specification.

Validation: "Are we building the right product”.
* The software should do what the user really requires.

**Aim of V & V** is to **establish confidence that the system is ‘fit for purpose’**.
Depends on system’s purpose, user expectations and marketing environment.
* Software Purpose: The level of confidence depends on **how critical the software is** to an organisation.
* User expectations: Users may have low expectations of certain kinds of software.
* Marketing environment： Getting a product to **market early may be more important** than **finding defects in the program**.


### 1.2 Insepctions and Testing

Software Inspections: Concerned with analysis of the static system representation to discover problems  (static verification).
* tool-based document 
* code analysis

Software Testing: Concerned with exercising and observing product behaviour (dynamic verification).
* The system is executed with test data and its operational behaviour is observed.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/test_it.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/test_it.png" align="center"></a>
    <figcaption>The inspections and testing.</figcaption>
</figure>


Inspections Properties:
* involve people examining the source representation **with the aim of discovering anomalies and defects**.
* Inspections not require execution of a system so **may be used before implementation**.
* They may be applied to any representation of the system (requirements, design,configuration data, test data, etc.).

Inspections advantages:
* inspection is a static process, you don’t **have to be concerned with interactions between errors**. While testing, errors mask other errors.
* **Incomplete versions of a system can be inspected without additional costs**.   If a program is incomplete, then you need to develop specialized test harnesses to test the parts that are available.
* As well as searching for program defects, **an inspection can also consider broader quality attributes of a program**, such as compliance with standards, portability and maintainability. 

Inspections Drawback:
* Inspections can check conformance with a specification but not conformance with the customer’s real requirements.
* Inspections cannot check non-functional characteristics such as performance, usability, etc.



Inspections and testing are **complementary and not opposing verification techniques**.
* should be used during the **V & V process**


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/test_pro.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/test_pro.png" align="center"></a>
    <figcaption>The testing process.</figcaption>
</figure>


### 1.3 Testing Stages

(1) Development Testing: where the system is tested during development to discover bugs and defects. 
 
(2) Release Testing: a separate testing team test a complete version of the system **before it is released to users**. 

(3) User Testing: users or potential users of a system test the system in their own environment.


## 2. Development Testing

### 2.1 Conception

Development testing includes **all testing activities** that are carried out by the team developing the system. 
* Unit Testing:  **individual program units or object classes** are tested. Unit testing should focus on testing **the functionality of objects or methods**.
* Component Testing: **several individual units** are integrated to create composite components. Component testing should focus on **testing component interfaces**.
* System Testing: some or all of the components in a system are integrated and the system is **tested as a whole**. System testing should focus on **testing component interactions**.

### 2.2 Unit Testing

Unit testing is the process of testing individual components in isolation.
* it is a defect testing process

Units may be:
* **Individual functions or methods** within an object 
* **Object classes** with several attributes and methods 
* **Composite components** with defined interfaces used to access their functionality.

### 2.2.1 Object Class Testing

Complete Test coverage of a class involves:
* Testing all operations associated with an object.
* Setting and interrogating all object attributes 
* Exercising the object in all possible states.

Inheritance makes it difficult to design object class tests as the information to be tested is not localised.

### 2.2.2 Automated Testing

Whenever possible, **unit testing** should be **automated** so that tests are run and checked without manual intervention.
* you make use of a test automation framework (such as JUnit) to write and run your program tests. 

Automated Test Components:
* A setup part:  you initialize the system with the test case, **namely the inputs and expected outputs**.
* A call part: call the object or method to be tested.
* An assertion part: where you compare the result of the call with the expected result. If the assertion evaluates to true, the test has been successful  if false, then it has failed.


### 2.2.3 Choosing Unit Test Cases

When used as expected, **the component that you are testing does what it is supposed to do**.
If there are **defects** in the component, these should be **revealed by test cases**.

2 types of unit test case:
* reflect normal operation of a program and should show that the component works as expected
* The other kind of test case should be based on testing experience of where common problems arise. It should use **abnormal inputs** to check that these are properly processed and do not crash the component. 

### 2.2.4 Testing Strategies

(1) Partition Testing: you **identify groups of inputs that have common characteristics** and should be **processed in the same way**. 
* **Input data and output results** often fall into different classes where all members of a class are related.
* **Each of these classes** is an **equivalence partition or domain** where the program behaves in an equivalent way for each class member.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/test_pt.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/test_pt.png" align="center"></a>
    <figcaption>The partition testing process.</figcaption>
</figure>

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/test_ep.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/test_ep.png" align="center"></a>
    <figcaption>The equivalence partition testing process.</figcaption>
</figure>

(2) Guideline-based Testing: you use **testing guidelines** to choose test cases. 


Test software with sequences which have only a single value.
* **Use sequences of different sizes in different tests**.
* Derive tests so that the first, middle and last elements of the sequence are accessed.
* Test with sequences of zero length.
* Choose inputs that **force the system to generate all error messages**
* Design inputs that **cause input buffers to overflow** 
* Repeat the same input or series of inputs **numerous times** 
* **Force invalid outputs** to be generated 
* Force computation results to be **too large or too small**.

### 2.3  Component Testing

Software components are often **composite components** that are **made up of several interacting objects**. 
* You access **the functionality of these objects** through **the defined component interface**. 
* Testing composite components should therefore focus on **showing that the component interface behaves according to its specification**.

#### 2.3.1 Interface Testing

Objectives are to **detect faults** due to **interface errors or invalid assumptions** about **interfaces**.

Interface Types:
* **Parameter interfaces**: Data passed from one method or procedure to another.
* **Shared memory interfaces**: Block of memory is shared **between procedures or functions**.
* **Procedural interfaces**: Sub-system encapsulates **a set of procedures to be called by other sub-systems**.
* **Message passing interfaces**: Sub-systems request services from other sub-systems

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/test_interface.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/test_interface.png" align="center"></a>
    <figcaption>The Interface Testing.</figcaption>
</figure>

Interface Errors:
* Interface Misuse: A calling component calls another component and **makes an error in its use of its interface** e.g. parameters in the **wrong order**.
* Interface Misunderstanding: A calling component embeds assumptions about **the behaviour of the called component which are incorrect**.
* Timing errors: The called and the calling component operate at different speeds and **out-of-date information is accessed**.

Interface Testing Guidelines
* Design tests so that **parameters to a called procedure are at the extreme ends of their ranges**.
* Always **test pointer parameters with null pointers**.
* Design tests which **cause the component to fail**.
* Use **stress testing in message passing systems**.
* In shared memory systems, **vary the order in which components are activated**.

### 2.4  System Testing

System testing during development involves **integrating components to create a version of the system** and then **testing the integrated system**.

The focus in system testing is **testing the interactions between components**. 
* System testing checks that **components are compatible, interact correctly and transfer the right data at the right time across their interfaces**.
* System testing tests **the emergent behavior of a system**.

During system testing, **reusable components** that have been separately developed and off-the-shelf systems may be integrated with newly developed components. The complete system is then tested.
* Components developed by different team members or sub-teams may be integrated at this stage.
* System testing is a collective rather than an individual process. 

In some companies, system testing may involve **a separate testing team with no involvement from designers and programmers**. 


#### 2.4.1 Use-case Testing

The use-cases developed to **identify system interactions** can be used as **a basis for system testing**.
* Each use case usually involves **several system components so testing the use case forces these interactions to occur**.
* The **sequence diagrams** associated with **the use case documents the components and interactions** that are being tested.

**An input of a request** for a report should have **an associated acknowledgement**. A report should ultimately **be returned from the request**. 
* You should create **summarized data** that can be used to **check that the report is correctly organized**.


#### 2.4.2 Testing Policies

**Exhaustive system testing** is **impossible**: so testing policies which define the **required system test coverage** may be developed.
* **All system functions** that are **accessed through menus** should be tested.
* **Combinations of functions** (e.g. text formatting) that are accessed through the same menu must be tested.
* Where user input is provided, **all functions** must be **tested** with **both correct and incorrect input**.


## 3. Test-driven development

Test-driven development (TDD) is an approach to **program development in which you inter-leave testing and code development**.
* **Tests are written before code** and **‘passing’ the tests** is the critical driver of development.
* You **develop code incrementally**, along with a test for that increment. You **don’t move on to the next increment** until **the code that you have developed passes its test**. 
* **TDD** was introduced **as part of agile methods** such as **Extreme Programming**. However, it can also be used in **plan-driven development processe**s. 


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/test_tdd.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/test_tdd.png" align="center"></a>
    <figcaption>The test-driven development.</figcaption>
</figure>


### 3.1 TDD Process Activities

(1) **Identifying the increment of functionality** is required.
* This should normally be small and implementable in a few lines of code.

(2) **Write a test for this functionality** and implement this as **an automated test**.

(3) Run the test, **along with all other tests** that have been implemented. Initially, you have not **implemented the functionality** so the new test will fail. 

(4) Implement the functionality and re-run the test.

(5) Once all **tests run successfully**, you move on to **implementing the next chunk of functionality**.


### 3.2 Benefits

(1) Code Coverage

Every code segment that you write has **at least one associated test** so **all code written has at least one test**.

(2) Regression Testing

**A regression test suite is developed incrementally** as a program is developed. 


(3) Simplified Debugging

When a test fails, it should be **obvious** where the problem lies. The newly written code needs to be checked and modified. 

(4) System Document

**The tests themselves are a form of documentation** that describe what **the code should be doing**. 


### 3.3 Regression Testing

**Regression testing** is testing the system to **check that changes have not ‘broken’ previously working code**.

In a **manual testing process**, regression testing is expensive but, with automated testing, it is simple and straightforward. **All tests are rerun every time a change is made to the program.**
* Tests must run ‘successfully’ before the change is committed.


## 4. Releasing Testing

**Release testing** is the process of **testing a particular release of a system** that is intended for **use outside of the development team**. 

The primary goal of the release testing process is to **convince the supplier of the system that it is good enough for use**.
* Release testing, therefore, has to show that the system delivers its **specified functionality**, **performance and dependability**, and that it **does not fail during normal use**. 

Release testing is **usually a black-box testing process** where tests are only **derived from the system specification.** 
* is a form of system testing

The difference between releasing testing and system testing:
* **A separate team** that has not been involved in the system development, should be **responsible for release testing**.
* **System testing by the development team** should focus on **discovering bugs in the system (defect testing)**. The **objective of release testing** is to check that **the system meets its requirements and is good enough for external use (validation testing)**.


#### 3.3.1 Requirements based Testing

Requirements-based testing involves examining each requirement and developing a test or tests for it.

#### 3.3.2 Performance Testing

**Part of release testing** may involve testing **the emergent properties of a system**, such as performance and reliability.

Tests should **reflect the profile of use of the system**.

**Performance tests** usually involve planning a series of tests where the load is steadily increased until the system performance becomes unacceptable.

**Stress testing** is a form of performance testing where the system is deliberately overloaded to test its failure behavior.


## 5. User Testing

**User or customer testing** is a stage in the testing process in which **users or customers provide input and advice on system testing**.

**User testing is essential,** even when comprehensive system and release testing have been carried out. 

The reason for this is that **influences from the user’s working environment** have a major effect on the reliability, performance, usability and robustness of a system. **These cannot be replicated in a testing environment.**

Use Testing Types:
* Alpha Testing: Users of the software work with the development team to test the software at the developer’s site.
* Beta Testing: **A release of the software** is made available to users to allow them to experiment and to raise problems that they discover with the system developers.
* Acceptance testing: Customers test a system to **decide whether or not it is ready to be accepted from the system developers and deployed in the customer environment**. Primarily for custom systems.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/test_atp.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/test_atp.png" align="center"></a>
    <figcaption>The acceptance testing process.</figcaption>
</figure>

Stages in the acceptance testing process:
* Define acceptance criteria
* Plan acceptance testing
* Derive acceptance tests
* Run acceptance tests
* Negotiate test results
* Reject/accept system

### 5.1 Agile Methods and Acceptance Testing

In agile methods, the **user/customer is part of the development team** and is responsible for **making decisions on the acceptability of the system**.

**Tests are defined by the user/customer** and are **integrated with other tests** in that they are run automatically when changes are made.

**There is no separate acceptance testing process**.

Main problem here is whether or not the embedded user is **‘typical’** and can **represent the interests of all system stakeholders**.




## 6. Software Evolution

Software Change is inevitable:
* New requirements emerge when the software is used;
* The business environment changes;
* Errors must be repaired;
* New computers and equipment is added to the system;
* The performance or reliability of the system may have to be improved.

A key problem for all organizations is **implementing and managing change to their existing software systems**.

Organizations have huge investments in their software systems - they are critical business assets.
* To **maintain the value of these assets to the business**, they must **be changed and updated**.
* The **majority of the software budget** in large companies is devoted to **changing and evolving existing software rather than developing new software**.




<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/ev_spi.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/ev_spi.png" align="center"></a>
    <figcaption>The spiral model of development and evolution.</figcaption>
</figure>


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/ev_pr.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/ev_pr.png" align="center"></a>
    <figcaption>The evolution and Servicing.</figcaption>
</figure>

(1) Evolution

The stage in a software system’s life cycle where it is in **operational use** and is **evolving as new requirements are proposed and implemented in the system**.

(2) Servicing

At this stage, the software remains **useful** but the only changes made are those required to keep it operational i.e. **bug fixes and changes to reflect changes in the software’s environment**. No new functionality is added.

(3) Phase-out

The software may still be used but no further changes are made to it.


### 6.1 Evolution Process

Software evolution processes depend on
* The **type of software** being maintained;
* The development processes used;
* The **skills and experience of the people involved**.

**Proposals for change** are the driver for system evolution.
* Should be linked with components that are affected by the change, thus allowing the cost and impact of the change to be estimated.

**Change identification and evolution** continues **throughout the system lifetime**.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/ev_ci.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/ev_ci.png" align="center"></a>
    <figcaption>The change identification and evolution processes.</figcaption>
</figure>


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/ev_ep.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/ev_ep.png" align="center"></a>
    <figcaption>The software evolution process.</figcaption>
</figure>

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/ev_cim.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/ev_cim.png" align="center"></a>
    <figcaption>Change Implementation.</figcaption>
</figure>

About Change Implementation

* **Iteration of the development process** where the revisions to the system are designed, implemented and tested.
* A **critical difference** is that **the first stage of change implementation may involve program understanding**, especially if the original system developers are not responsible for  the **change implementation**. 
* During the **program understanding phase**, you have to understand how the program is structured, how it delivers functionality and how the proposed change might affect the program. 


About Urgent Change Requests

Urgent changes may have to be **implemented without going through all stages of the software engineering process**
* If a serious system fault has to be repaired to allow normal operation to continue;
* If changes to the system’s environment (e.g. an OS upgrade) have unexpected effects;
* If there are business changes that require a very rapid response (e.g. the release of a competing product).


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/ev_rp.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/software/ev_rp.png" align="center"></a>
    <figcaption>The emergency repair process.</figcaption>
</figure>

About Methods and Evolution

(1) Agile methods are based on **incremental development** so the **transition from development to evolution is a seamless one**.

(2) Evolution is simply **a continuation of the development process** based on frequent system releases.

(3) **Automated regression testing** is particularly valuable when changes are made to a system.

(4) Changes may be expressed as **additional user stories**.

**Handover Problems**:
* Where the development team have used **an agile approach**, but **the evolution team** is unfamiliar with **agile methods and prefer a plan-based approach**.
    * The evolution team may expect detailed documentation to support evolution, and this is **not produced in agile processes**. 
* Where a plan-based approach has been used for development, but the evolution team prefer to use agile methods. 
    * The evolution team may have to start from scratch developing automated tests and the code in the system **may not have been refactored and simplified as is expected in agile development**. 

### 6.2 Software Maintenance

Software Maintenance
* Modify a program after it has been put into use.
* **The term is mostly used for changing custom software**. **Generic software products** are said to **evolve to create new versions**.
* Maintenance does not normally **involve major changes to the system’s architecture**.
* **Changes are implemented by modifying existing components** and **adding new components to the system**.



Maintenance Types:

(1) Fault repairs

Changing a system to **fix bugs/vulnerabilities** and **correct deficiencies** in the way meets its requirements.

(2) Environmental adaptation

Maintenance to adapt software to **a different operating environment**

Changing a system so that it operates in a different environment (computer, OS, etc.) from its initial implementation.

(3)Functionality addition and modification 

Modifying the system to **satisfy new requirements**.
