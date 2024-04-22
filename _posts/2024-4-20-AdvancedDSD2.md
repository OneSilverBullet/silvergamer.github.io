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

## Part1 

(1) Distributed Transparency:
For a client to communicate with a server using the server's name, the communication needs to have:
* location transparency

(2) RPC Execution Semantics:
Client-server communication middleware supports **at-most-once RPC semantics** because it simplifies:
* server implementation

(3) Inter Process Communication(IPC): In inter process communication(IPC), the communicating processes:
* should run on the same networked host.
* can run on different networked hosts.

(4) Inter Process Communication(IPC): Implementation of inter process communication requires:
* sockets
* protocol

Points:

IPC mechanism:
* Shared Memory
* Socket
* Semaphore
* Protocol


(5) Request-Reply Protocol: A remote procedure call(RPC) between a client and a server is typically implemented using a request-reply protocol because:
* The client does not know whether the server is running
* The server may not be running

(6) Remote Procedure Call(RPC): For a client process to call a local server(running in the same host) the same way as a remote server(running in a different networked host), we need:
* access transparency
* location transparency

(7) Local Procedure Call(LPC): In a local procedure call(LPC), the communicating(caller and called) processes:
* can run on the same networked host
* can run on the same standalone host.

(8) Data marshalling and unmarshalling are required in the implementation of
* both LPC and RPC

(9) Network Communication: The **IP routing protocol** to route a packet/message between a source and a destination:
* is a peer-to-peer distributed algorithm
* uses **local information** about the network

(10) Distributed Applications: An application is designed as a distributed application in order to achieve:
* transparency
* scalability

Points: Why Distributed Systems?
* Easily connect user to remote resources
* Share resources with remote users in a controlled way
    * Transparency: hide the fact that the resources are physically distributed over a network
    * Open System.
    * Scalable: Size; Geography and Administration.



## Part2 

(1) Middleware like RMI and Corba for distributed system achieve access transparency using 
* Server interface definitions 
* Stubs and skeletons

(2) A corba applicatiuon deployed on a  LAN may not use
* implementation reposity
* corba naming service

A corba application in LAN use:
* object adapter
* interface definition

Points: 

**Corba Naming Service**: is a core component in CORBA, it provides services of object naming and finding.

An **implementation repository** is a central location or system for **storing and managing service implementation code**

**Object Adapter**: play a role between the client and server, responsible for the initialization, delete, invoke and binding, encode, decode, transmission and respose.

(3) Corba application use Corba Data Representation CDR because they:
* can be implemented in different programming languages
* can execute on different hosts

Points:
Common Data Representation: a coding scheme for marshalling and unmarshalling data of each IDL data type.
* functioins:
    * how to encode, marshalling and transmission.
    * transmission between different hosts, operator systems, and languages.
    * cross-platform, cross-language, cross internet.

(4) Implementation repository in RMI, a RMI distributed application does not require an implementation repository because RMI applications:
* do not use multiple language implementation
* know the location of server implementations

(5) When a distributed application is implemented in a same/single programming language and deployed on a LAN using JAVA RMI and Corba, the Corba Implementation will run
* **slower** due to **middleware overhead**.

Points: 

**RMI** provides a simple, direct, and consistent API, which is integrated in JVM and Java Language.

**Corba** provides a cross-language, cross platform dirstibuted communication mechanism, which supports multiple programming languages and operating systems.

(6) A Java RMI application cannot be deployed on a WAN because of
* RMI Registry
* RMI Middleware

Points:

RMI Registry(Remote Method Invocation Registry): it provides a simple, light **object registration services**, which used for manage and maintain the remote object reference and address. RMI Registry is responsible for **registing, finding, binding, and unbinding the remote objects**.
* simple flat table that cannot be used on a WAN, since it’s not hierarchical

RMI Middleware: it is designed for the LAN, not for the WAN. WAN requires message exchange between networks, so it is not supported

(7) Location Transparency in Distributed Systems: In distributed systems, location transparency is typically achieved by:
* by the programmer using central directory

Points:

**Location transparency**: The client can access the server, **without knowing where it is**. Its difficult to do this because we always needed the know where the server is. So, its acieved by the programmer to **add a central directory to the main server for the client to access by name**.

(8) Corba Applications achieve platform independence:
* IOR and IIOP
* object adapters

Points: 

Platform Independence: can execute in any hardware/operating system

General Inter-ORB Protocol(GIOP): a specification which provides a general framework for protocols to be built on top of specific transport layers.

Internet Inter-ORB Protocol(IIOP): is a special case of GIOP, which is the GIOP applied to the TCP/IP transport layer. The IIOP specification includes:
* Transport Management Requirements
* Definition of Common Data Representation
* Message Formats

The Object Request Broker(ORB): mediates the interaction between client and server objects. 

As in Java RMI, **a corba distributed object** is located using an **object reference**. Since CORBA is **language-independent**, a CORBA object reference is an abstract entity mapped to **a language-specific object reference by an ORB**, in a representation chosen by the developer of the ORB.

Interoperable Object Reference(IOR): An ORB compatible with the IOR protocol will allow an object reference to be **registered with and retrieved from any IOR-compliant directory service**. 
* **CORBA object references** represented in this protocol are called: "**Interoperable Object References(IORs)**".
* IOR is a string that contains encoding for the following information: 
    * The type of the object
    * The host where the object can be found
    * The port number of the server for that object

(9) Java RMI applications achieve platform independence using:
* Java Virtual Machines
* RMI middleware

Points:

Java RMI run based on JVM, which provides a abstract calculation environment and hides operating system and hardwares for users.

(10) A corba application implementation in a single programming language may not require the:
* implementation repository
* naming service

Points: 

CORBA Naming Service: CORBA specifies a generic directory service, which serves as a **directory for CORBA objects**, which is **platform independent** and **programming language independent**. 
* Name Resolving: The naming service permits ORB-based clients to obtain references to objects they wish to use. **Name asscociate with the object reference**.
* The API for the Naming Service is specified in interfaces defined in **IDL**


## Part3 Review

(1) In a distributed application based on web services, it is desirable to implement the client: 
* **synchronously** as the server **response time is high**.


(2) In web service technology, clients and servers communicate: 
* **synchronously by sending at the request** and **reply message asynchronously**.

Points: 

Web Services are software components **described via WSDL** which are capable of being accessed via standard network protocols such as **SOAP over HTTP**.
* SOAP provides rules for encoding the request and its arguments.
* WSDL documents are used to drive object assembly, code generation, and development tools.

(3) Unlike CORBA, a distributed application implemented in web service technology does not use **an implemnation repository** because:
* servers may be implemented in any programming languages
* requests and reply are encoded in XML.

Points: An implementation repository is a central location or system for **storing and managing service implementation code**.


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

Points: 

XML works as the encoding format.
* SOAP message use XML as its default encoding format. 
* SOAP message contains Envelope, Header and Body. 
* Each elements in SOAP message are encoded as XML.


(6) Unlike CORBA, **a client and client-side middleware** need not be in the **same programming language** in web services because:
* the client and the middleware communicate using XML
* the client and the middleware communicate using SOAP

(7) In web service technology, **platform-independence** is achieved by(application can perform on any software or hardware):
* using a web service middleware.
* encoding the requests and replies in XML.


(8) In web service technology, a client needs to **discover the service** because:
* the available service changes frequently.

Points: 

The web services working process:
* First the client discover the service
* client binds to the server(binding not always need)
    * Setting up TCP connection to the discovered address
* Build the SOAP request(Marshalling)
    * Fill in what service is needed, the arguments, send it to server side.
    * XML
* SOAP router routes the request to the appropriate server
* Server unpacks the request, handles it, computes results
* result sent back in the reverse direction: from the server to the SOAP router back to the client.

Reposity:
* a database listing servers
* Each is described using the UDDI language, which is defined over XML.
    * can be searched with XML queries.

UDDI is used to write down the information that became a "row" in the repository.

WSDL documents the interfaces and data types used by the service.




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



## Part5 Review

(1) **Concurrent snapshots** can be taken in a distributed system using Chandy and Lamport's algorithm becasue:
* the markers can differentiate the concurrent snapshots
* the algorithm is non-blocking


(2) The Chandy and Lamport's distributed snapshot algorithm makes a cut consistent by:
* including **all the messages in the network** at that **instance of time in the channel state of a process**.

Points:

instance of time in the channel state of a process: in **a specific time slot**, We want to know **what messages** are **being delivered or waiting** to be **received** in a **particular process's message channel**.

(3) In a distributed system, a consistent cut represents the system states:
* that can happen during some execution

Points:

A consistent cut: something that can happen when the system executes.

(4) In Chandy and Lamport’s distributed snapshot algorithm, a cut is: 
* the set of time points (one per process) at which **the processes receive the marker for the first time**.

(5) In distributed system, a cut is
* a collection of time points, one per process during an execution
* the current time in the distributed system

(6) If there is no path between some pair of process, the Chandy and Lamport’s distributed snapshot algorithm will: 
* produce incomplete state 
* not terminate

(7) If the communication channels are not FIFO, the **set of states** collected by **Chandy and Lamport’s snapshot algorithm** will be **inconsistent**  because: 

* a marker sent by a process may **overtake a message** 
* a marker sent by a process may **arrive before a message already in the network**. 

Points:
* Overtaking is corrupting message and overwriting in arriving earlier. (Check could be opposite. 

(8) **Global States** in a distributed snapshot contain the appropriate information about 

All the processes and the messages among them **relevant to the snapshot**.

(9) Distributed System cannot reliably use physical time because:
* It is not possible to make the times at all the hosts always the same.

(10) in distributed system, the vector clock maintained by a process corresponds to its knowledge about:
* the relevant events in all the processes from which it has received messages






