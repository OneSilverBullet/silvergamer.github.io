---
layout: post
title:  "[Distributed System]Advanced Distributed System Conception"
date:   2024-3-12
excerpt: "Discuss the advanced conceptions about DSD."
tag:
- Distribute System Design
comments: false
blog: true
feature: https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/blogHead/directX12partI.jpg
---


## Part3 Review

(1) In a distributed application based on web services, it is desirable to implement the client: 
* **synchronously** as the server **response time is high**.


(2) In web service technology, clients and servers communicate: 
* **synchronously by sending at the request** and **reply message asynchronously**.


(3) Unlike CORBA, a distributed application implemented in web service technology does not use **an implemnation repository** because:
* servers may be implemented in any programming languages
* requests and reply are encoded in XML.


(4) Unlike CORBA, **interface definitions** are not written by the server developer in **web services** because:
* they are generated from **server end point**.
* they are included in **the service description**.

Points:

Server Endpoints:
* clients can communicate with server by this **Server Endpoints**. 

WSDL(Web Services Description Language)
* WSDL is a XML format language, used for descripe web services interface, operation, message and protocle detail.

(5) In web services, **programming language independence** is achieved by using a client and server implemented in any programming language, only when they can **communicate irrespective** of the language they are implemented in:
* SOAP message
* XML encoding

(6) Unlike CORBA, **a client and client-side middleware** need not be in the **same programming language** in web services because:
* the client and the middleware communicate using XML
* the client and the middleware communicate using SOAP

(7) In web service technology, **platform-independence** is achieved by(application can perform on any software or hardware):
* using a web service middleware.
* encoding the requests and replies in XML.


(8) In web service technology, a client needs to **discover the service** because:
* the available service changes frequently.

(9) Unlike CORBA, stub and skeleton codes are not used in web services because:
* their functionalities are included in **WSDL description**.
* SOAP is used in web services.

stub and skeleton generate request and reply messages, so SOAP more relevant than 
XML

(10) The description of a web service in WSDL contains the service:
* interface
* reference


## Part4 Review

(1) In an actively replicated system, it crash failures are detected using the absence of a result like in the project, the server:
* cannot detect without a client request
* cannot tolerate a software failure


(2) When all the replicas in an **actively replicated server system** execute a set of requests in **total order**. The local copy of the data in every replica will be identical after:
* each client request is processed.

Points:


Data in all nodes are actively updated and motified. **When a node is received a write operation: insert, update, delete, it will update the operation to its copy immediately.**


(3) In order to ensure **data consistency** in a passively replicaed server systemm the backups should perform the data updates send from the primary in: 
* FIFO order.

Points:

In Passively Replicated Server, data  only need to be copied to other nodes in specific time slot, it should not be copied each write operation.

If a primary recieved a write operation, it will reserve the operation, in some specific time slot, it will update the data to its replications. **Asynchronous**.


(4) In distributed application implemented using **active replication** in general, the server replicas should execute a set of a client requests in：
* total order
* casual order

Points:

Total Order: every events have a global, linear execute order. In a total order system, all nodes agree the event order in the time line.

Causal Order: If event A happens before B and influences B, all nodes should agree A is before B.

(5) In replicated Server System, each replica maintains a local copy of application data in order to:
* execute client operation faster
* access data in parallel

Points:

Actively Replicated:

Benifits:
* Performance Enhancement
* High Availability

Shortage:
* network cost
* consistent cost

(6) In an actively server system if the FE does not repliably multicast a client request to the server replica:
* the client request may not executed by all replicas.
* a software failure may not be detected

(7) In replicated server systems, the FE typically invokes a server method by sending UDP message because the FE: 

* invoke the method in multiple process
* minimize invocation overload

(8) In an actively replicated server system, replicas should send the result of a client operation to the front end:
* reliable unicast
* UDP message

Points:

About reliable unicats:
* ACK
* Overtime and retransmission
* Guarantee sequence
* Error Check and Fix

(9) In an **actively replicated server system**, the replicas should be implemented as:
* iterative server

Points:

Iterative Server: when process the task, it will not start a new thread, but process the tasks in sequence.
* No concurrent
* Single task processing
* Simple and predictable

(10) When a software failure happens in an application implemented using passive replication:
* the primary replica produce incorrect result(because of the passive copy property)
* the local data in all replicas will be incorrect

Points: 

**Software Failure**: refers to the abnormal or incorrect behavior of the system caused by software errors or defects.
* may lead to the incorrect of the data
* influence the primary and all other replicas
* need to fix errors

**Crash Failure**:  refers to the sudden stop or crash of an application or system component due to a hardware failure, operating system error, or other system-level error.
* unpredictable
* influence single replica
* trigger the recover mechanism










