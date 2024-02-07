---
layout: post
title:  "[Distributed System]Introduction"
date:   2024-1-15
excerpt: "How to make specification in software processing."
tag:
- Distribute System Design
comments: false
blog: true
feature: https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/blogHead/directX12partI.jpg
---


## 1. Introduction of Distributed System

### 1.1 Conception

A  distributed system is **a collection of autonomous hosts** that are connected through a computer **network**. Each host executes **components** and operates a distribution **middleware**, which enables the components to coordinate their activities in such a way that users perceive the system as a single, integrated computing  facility.


Transparency: Distributed systems should be perceived by users and application programmers
as a whole rather than as a collection of cooperating components.
* Access Transparency: A components should be able to communicate with **any other component the same way**.
* Location Transparency: Components should be able to communicate using their **names** only **without knowing their location**.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/tds.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/tds.png" align="center"></a>
    <figcaption>Transparency in a distributed system.</figcaption>
</figure>

### 1.2 Common Requirements

#### 1.2.1 Concurrency

* **Components** in distributed systems are executed in **concurrent processes**.
* Components **access and update shared resources** (e.g. variables, databases, device drivers).
* Integrity of the system may be violated if concurrent updates are not coordinated.
    * Lost updates
    * Inconsistent analysis

#### 1.2.2 Resource Sharing

Ability to use any hardware, software or data anywhere in the system

Resource manager controls:
* access
* provides naming scheme 
* controls concurrency

**Resource sharing model** (e.g. client/server or object-based) describing how
* resources are provided
* they are used 
* provider and user interact with each other.

#### 1.2.3 Openness

Openness is concerned with **extensions and improvements of distributed systems**.
* **Detailed interfaces of components need to be published**.
* New components have to be **integrated with existing components**.
* Differences in data representation of interface types on different processors (of different vendors) have to be resolved.


#### 1.2.4 Scalability

Adaption of distributed systems to
* accommodate **more users**
* **respond faster** (this is the hard one)

Components should not need to be changed
when scale of a system increases.
* Usually done by adding more and/or faster
processors.
* Design components to be scalable!

#### 1.2.5 Fault Tolerance

Hardware, software and networks fail.

Distributed systems must maintain availability even at low levels of hardware/software/network reliability.

Fault tolerance is achieved by
* **recovery**
* **redundancy**

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/ft.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/ft.png" align="center"></a>
    <figcaption>Failure Tolerance.</figcaption>
</figure>


(1) There is at any time **a single primary process and one or more secondary** (backup, slave) processes.

(2) FEs communicate with the primary which **executes the operation and sends copies of the updated data to the result to backups**

(3) If the primary fails, one of **the backups is promoted to act as the primary**

#### 1.2.6 High Availability

* All the processes **perform the same computation**

* A single result is **produced from the results of all the processes**

* Correct result can be produced even **when one of the processes produces incorrect result**

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/ha.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/ha.png" align="center"></a>
    <figcaption>High Availability.</figcaption>
</figure>

### 1.3 Distributed System Overview

#### 1.3.1 Usage

(1) Easily connect users to **remote resources**

(2) **Share resources with remote users** in a controlled way

* Hide the fact that the resources are physically distributed over a network -- **transparency**
* Should be an **open** system
* Should be **scalable**

An application is designed as a distributed application to achieve scalability, failure tolerance and availability.

#### 1.3.2 Characteristics

In **distributed system**:

* Multiple autonomous components
* Components are not shared by all users
* Resources may not be accessible
* Software runs in concurrent processes on different processors
* Multiple Points of control
* Multiple Points of failure

In **centralized system**:
* One component with non-autonomous parts
* Component shared by users all the time
* All resources accessible
* Software runs in a single process
* Single Point of control
* Single Point of failure

#### 1.3.3 Major Issues in Destributed System Design

(1) Independent Failure

(2) Unreliable and Insecure Communication

(3) Costly Communication

(4) High Availability, Fault-Tolerance

(5) Atomicity is difficult

#### 1.3.4 Distributed Computing Paradigms

(1) Client-Server

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/cs.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/cs.png" align="center"></a>
    <figcaption>Client and Server.</figcaption>
</figure>

Client access to server should be **independent** of
* Local or remote server
    * Access Transparency
* Location of the server
    * Location Transparency
* Implementation of the server
    * Implementation Transparency
    * Porgramming Language Transparency
    * Replication Transparency

(2) Peer-to-Peer

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/pp.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/pp.png" align="center"></a>
    <figcaption>Peer to peer.</figcaption>
</figure>

(3) Distributed-Agents

(4) Distributed-Object-Spaces


### 1.4 Remote Procedure Call RPC

#### 1.4.1 Conception

Pre-RPC: Most applications were **built directly over the Internet primitives**.

Mask distributed computing system using a **“transparent” abstraction**:
* Looks like **normal procedure call**
* Hides **all aspects of distributed interaction**
* Supports an **easy programming model**

RPC often used from object-oriented systems employing **CORBA or COM standards**.

RPC is the core of many distributed systems.

#### 1.4.2 RPC Types

* RMI: Object Oriented RPC
* Network Services: RPC among mostly homogeneous processes (over LANs)
* CORBA: RPC among heterogeneous processes (over WANs using IIOP)
    * The **Common Object Request Broker Architecture (CORBA)** is a standard architecture for a distributed objects system.
    * CORBA is designed to allow **distributed objects** to **interoperate in a heterogenous environment**, where objects can be implemented in different programming language and/or deployed on different platforms.

* Web Services: RPC over the web using HTTP.
    * Web Services **generalize this model** so that computers can talk to computers

#### 1.4.3 Server Design

Many of the server characteristics are achieved through process replication.

* Scalable: Automatically create **server processes to distribute client requests**.
* Fault Tolerant: Maintain multiple server processes so that **a faulty server can be taken over by a good one**.
* Highly Available: Maintain multiple server processes so that **a client request can be handled without delay**.



## 2. Networking and Internetworking


### 2.1 Interprocess Communications

**Distributed computing** requires information to be exchanged among **independent processes**. 

**Operating systems** provide **facilities** for interprocess communications (IPC)
* message queues
* semaphores
* shared memory

Distributed computing systems make use of these facilities to provide application programming interface which **allows IPC to be programmed at a higher level of abstraction**.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/dsn.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/dsn.png" align="center"></a>
    <figcaption>Distributed System Networks.</figcaption>
</figure>



#### (1)Conceptions

* Message: communicate **between processes**. Arbitrary size byte stream.
    * Even if they are on the same host OS will not allow independent processes to access shared memory.
    * A message is must.
* Packet: a fragment of a message that might **travel on the wire**. Variable size but limited, usually to 1400 bytes or less.
* Protocol: an algorithm by which **processes cooperate to do something using message exchanges**.
    * A protocol is a way of performing operations using messages. 
    * **It's meaningful only when there are multiple messages**.

* Network:  the infrastructure that links the computers, workstations, terminals, servers.
    * Routers
    * Communication Links


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/3NC.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/3NC.png" align="center"></a>
    <figcaption>Network Components.</figcaption>
</figure>

#### (2) Routing in a wide area network

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/RT.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/RT.png" align="center"></a>
    <figcaption>Routing Tables.</figcaption>
</figure>

**The network is not reliable**

A. **Different packets/messages** between a **sender** and a **receiver** may take different paths/time and **arrive out of order**.

B. Link can corrupt message
* Rare in high quality ones on the internet backbone
* More common with wireless connections, cable modems, ADSL

C. Routers can get overloaded
* When this happens they drop messages
* This is very common

Solution: Protocols retransmit lost packets increase reliable.

#### (3) IP: Internet Protocol

Three IP Services:
* Unicast: transmits a packet to specific host.
* Multicast: transmits a packet to a group of hosts.
* Anycast: transmits a packet to one of a group of hosts.(nearest)


Destination and source identified by the **IP address** 
* 32 bits for IPv4
* 128 bits for IPv6

All services are unreliable: Packet may be **dropped**, **duplicated**, and **received in a different order**.

Each router/gateway has the routing table and these router work together to route messages. Each router has its own router table, which means the peer-to-peer architecture.


#### (4) IP v4 addresses

In binary, a 32-bit integer. 

In text, this: “128.52.7.243”:
* Each decimal digit represents 8 bits (0 – 255)

**“Private” addresses** are not **globally unique**:
* Used behind NAT boxes
* 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16

**Multicast addresses** start with 1110 as the first 4 bits (Class D address):
* 224.0.0.0/4.
* Unicast and anycast addresses come from the same space.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/port.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/port.png" align="center"></a>
    <figcaption>Logical Ports: Process and Port.</figcaption>
</figure>

### 2.2 UDP & TCP

#### 2.2.1 UDP: User Datagram Protocol

(1) Runs above IP

(2) Same unreliable service as IP
* Packets can get lost anywhere:
    * Outgoing buffer at source
    * Router or link
    * Incoming buffer at destination
* Messages may be delivered out of send order

(3) But adds port numbers
* Used to identify “application layer” protocols or
processes

(4) Also a checksum, optional

#### 2.2.2 TCP: Transmission Control Protocol

(1) Runs above IP
* Port number and checksum like UDP

(2) Service is in-order byte stream
* Application does not absolutely know how the bytes are
packaged in packets

(3) Flow control and conjestion control

(4) Connection setup and teardown phases

(5) Can be considerable delay between bytes out at source and bytes in at destination
* Because of **timeouts** and **retransmissions**

(6) Works only with unicast (not multicast or anycast)

#### 2.2.3 UDP vs. TCP

(1) UDP is more real-time
* packet is sent or dropped, but is not delayed

(2) UDP has more of a message flavor
* One packet = one message
* But must add reliability mechanisms over it

(3) TCP is great for transferring a file or a bunch of email,
but kind-of frustrating for messaging.
* Interrupts to application don’t conform to message boundaries
* No “Application Layer Framing”

(4) TCP is **vulnerable** to DoS (Denial of Service) attacks,
because initial packet consumes resources at the
receiver

### 2.3 OSI

(1) Application 

Protocols that are designed to meet the communication requirements of specific applications, often defining the interface to a service.
* HTTP, FTP, SMTP, CORBA, IIOP

(2) Presentation

Protocols at this level **transmit data in a network representation** that is **independent of the representations used in individual computers**, which may differ. Encryption is also performed in this layer, if required.
* Secure Sockets(SSL),CORBA Data Representation

(3) Session

At this level reliability and adaptation are performed, such as detection of failures and automatic recovery.

(4) Transport

This is the lowest level at which messages (rather than packets) are handled. 

Messages are addressed to communication ports attached to processes.

Protocols in this layer may be connection-oriented or connectionless

* TCP, UDP

(5) Network

Transfers data packets between computers in a specific network. 

In a WAN or an internetwork this involves the generation of a route passing through routers. 

In a single LAN no routing is required.
* IP, ATM virtual circuits

(6) Data link

Responsible for **transmission of packets between nodes** that are directly connected by a physical link.

 In a WAN transmission is between **pairs of routers** or **between routers and hosts**. 
 
 In a LAN it is between any pair of hosts.

(7) Physical 

The circuits and hardware that drive the network. 

It transmits sequences of binary data by analogue signalling, using amplitude or frequency modulation of electrical signals (on cable circuits), light signals (on fibre optic circuits) or other electromagnetic signals (on radio and microwave circuits).

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/TL.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/TL.png" align="center"></a>
    <figcaption>TCP/IP layers.</figcaption>
</figure>


### 2.4 Sockets and Port

#### 2.4.1 UDP Client and Server

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/SR.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/SR.png" align="center"></a>
    <figcaption>sender and receiver programs.</figcaption>
</figure>

(1) Send Program

* create a **datagram socket** and bind it to any **local port**;
* place data in a byte array;
* create a **datagram packet**, specifying the data array and the receiver's address;
* invoke the **send method of the socket** with a reference to the datagram packet;

(2) Receive Program
* create a **datagram socket** and bind it to a specific **local port**;
* create a byte array for receiving the data;
* create a **datagram packet**, specifying the data array;
* invoke the **receive method of the socket with a reference** to the datagram packet;

#### 2.4.2 Stream-mode Socket API(Connection-oriented)

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/SS.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/SS.png" align="center"></a>
    <figcaption>Stream-mode Socket API.</figcaption>
</figure>

(1) Connection Listener(Server)
* **create a connection socket** and listen for connection requests;
* accept a connection;
* **creates a data socket for reading from or writing** to the socket stream;
    * get an input stream for reading to the socket;
    * read from the stream;
    * get an output stream for writing to the socket;
    * write to the stream;
* close the data socket
* close the connection socket

(2) Connection Requester(Server)
* create a data socket and request for a connection;
    * get an output stream for writing to the socket;
    * write to the stream;
    * get an input stream for reading to the socket;
    * read from the stream;
* close the data socket.


### 2.5 RPC: Remote Procedure Call using Sockets

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/RPC.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/RPC.png" align="center"></a>
    <figcaption>RPC.</figcaption>
</figure>

* Server program is **shared by many clients**.
* **RPC protocol typically used to issue requests**.
* **Server may manage special data, run on an especially fast platform, or have an especially large disk**.
* Client systems handle “front-end” processing and interaction with the human user.

#### 2.5.1 Binding Stage

* Occurs when client and server program first start execution
* **Server registers its network address** with **name directory**, perhaps with other information.
* **Client scans directory to find appropriate server.**
* Depending on how RPC protocol is implemented, may **make a “connection” to the server**, but this is not mandatory.

#### 2.5.2 Marshalled & Unmarshalled
We say that data is “marshalled” into a message and “unmarshalled” from it
* Representation needs to deal with 
    * **byte ordering issues** (big-endian versus littleendian)
    * **strings** (some CPUs require padding), 
    * **alignment**, etc
* Goal is to be as fast as possible on the most common architectures, yet must also be very general.
* In RPC, the middleware does the marshalling automatically.
* In socket programming, the programmer should do marshalling by self.



<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/ms.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/ms.png" align="center"></a>
    <figcaption>Marshalling & Unmarshalling.</figcaption>
</figure>

(1) Request Marshalling

Client builds a message containing arguments, indicates what procedure to invoke.
* Due to the need for generality, data presentation a potentially costly issue.

Performs a send I/O operation to send the message.

Performs a receive I/O operation to accept the reply.

Unpacks the reply from the reply message.

Returns result to the client program.

(2) Data Representation

Data transmitted on the network is a **binary stream**.
* An **interprocess communication system** may provide the capability to allow data representation to be imposed on the raw data.
* Because different computers may have different internal storage format for the same data type, an **external representation** of data may be necessary.
* Data marshalling is the process of 
    * (i) **flattening a data structure**.
    * (ii) **converting the data to an external representation**. 

Some well known **external data representation schemes** are: Sun XDR, ASN.1 (Abstract Syntax Notation), XML (Extensible Markup Language).

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/dec.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/dec.png" align="center"></a>
    <figcaption>Data Encoding Protocols.</figcaption>
</figure>

#### 2.5.3 RPC Semantics

* At most once: request is processed 0 or 1 times
* Exactly once: request is always processed 1 time
* At least once: request processed 1 or more times

#### 2.5.4 RPC versus local procedure call

* Restrictions on argument sizes and types
* New error cases
    * Bind operation failed
    * Request timed out
    * Argument “too large” can occur if, e.g., a table grows
* Costs may be very high

#### 2.5.5 RPC costs in case of local destination process

Often, the destination is right on the caller’s
machine!

* Caller builds message
* Issues send system call, blocks, context switch
* Message copied into kernel, then out to destination
* Destination is blocked... wake it up, context switch
* Destination computes result
* Entire sequence repeated in reverse direction
* If scheduler is a process, context switch 6 times!

#### 2.5.6 Synchronous vs. Asynchronous Communication

* The IPC operations may provide the synchronization necessary using **blocking**. A blocking operation issued by a process will block further processing of the process until the operation is fulfilled.

* Alternatively, IPC operations may be asynchronous or nonblocking. An asynchronous operation issued by a process will not block further processing of the process. Instead, the process is free to proceed with its processing, and may optionally be notified by the system when the operation is fulfilled.


When using an IPC programming interface, it is important to
note whether the operations are synchronous or asynchronous.

 If only blocking operation is provided for send and/or receive,
then it is the programmer’s responsibility to using child
processes or threads if asynchronous operations are desired.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/t.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/t.png" align="center"></a>
    <figcaption>Threads asynchronous.</figcaption>
</figure>



## 3. Remote Invocation and RMI

### 3.1 Conception


Steps of a remote procedure call:
* Client procedure calls **client stub** in normal way
* **Client stub builds message**, calls **local OS**
* Client's OS **sends message to remote OS**
* **Remote OS** gives message to **server stub**
* Server stub unpacks parameters, calls server
* Server does work, returns result to the stub
* Server stub **packs** it in message, calls local OS
* **Server's OS** sends message to client's OS
* **Client's OS** gives message to **client stub**
* **Stub** unpacks result, returns to **client**

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/PC.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/PC.png" align="center"></a>
    <figcaption>Remote Procedure Call.</figcaption>
</figure>

#### 3.1.1 Remote and Local method Invocations

Each process contains **objects**:
* **remote objects**: can receive **remote invocations**
* others can receive only **local invocations**
* **remote interface** specifies which methods can be invoked remotely

#### 3.1.2 Representation of a remote object reference

A remote object reference must be **unique** in the distributed system and over time. It **should not be reused** after **the object is deleted**. 


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/ror.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/ror.png" align="center"></a>
    <figcaption>Remote object reference.</figcaption>
</figure>

(1) **The first two fields locate the object** unless **migration** or **re-activation**
in a new process can happen.

(2) The fourth field identifies the object within the process whose
interface tells the receiver what methods it has (e.g. class Method)

(3) A **remote object reference** is created by a **remote reference module**
when a reference is passed as **argument or result** to another process
* It will be stored in the **corresponding proxy**
* It will be passed in request messages to **identify the remote object** whose
method is to be **invoked**

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/rm.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/rm.png" align="center"></a>
    <figcaption>Remote object reference.</figcaption>
</figure>

#### 3.1.3 Poxy, Skeleton in RMI

(1) Stubs

* **Creating code** for **marshalling** and **unmarshalling** is tedious and error-prone.
* Code can be generated fully automatically from **interface definition**.
* Code is embedded in stubs for client and server.
* **Client stub represents server** for client, **Server stub represents client** for server.
* Stubs achieve type safety.
* Stubs also perform **synchronization**.

(2) Synchronization

* Goal:  achieve similar synchronization to local method invocation
* Achieved by stubs:
    * **Client stub sends request** and **waits until server finishes**
    * **Server stub waits for requests** and **calls server when request arrives**.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/javarmi.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/javarmi.png" align="center"></a>
    <figcaption>Java RMI.</figcaption>
</figure>

### 3.2 Comparison of the RMI and the socket APIs

The **remote method invocation** API is an efficient tool for building network applications. It can be used in lieu of **the socket API in a network application**.
 
The tradeoffs between the RMI API and the socket API:
* The socket API is **closely related to the operating system**, and hence has less execution overhead. For applications which require high performance, this may be a consideration.
* The RMI API provides **the abstraction which eases the task of software development**. Programs developed with a higher level of abstraction are more comprehensible and hence easier to debug.

### 3.3 Multithreading

#### 3.3.1 Conceptions

Three Major Options:
* Single-threaded server: only does one thing at a time, uses send/receive system calls and
blocks while waiting (iterative server).
    * Applications can **deadlock** if a request cycle forms
    * Much of system may be idle **waiting for replies to pending requests**
    * **Harder to implement RPC protocol itself** (need to use a timer interrupt to trigger acks, retransmission, which is awkward)
* Multi-threaded server: internally concurrent, **each request spawns a new thread** to handle it (concurrent server).
    * must implement appropriate mutual exclusion to **guard against “race conditions”** and other **concurrency problems**
    * server is more active because it can **process new requests while waiting for its own RPC’s to complete on other pending requests**
* Upcalss: **event dispatch loop does a procedure call for each incoming event**, like for X11 or PC’s running Windows.

#### 3.3.2 Multi-threading Server

Drawbacks:
* Users may have **little experience with concurrency** and will then make mistakes
* Concurrency bugs are very hard to **find due to nonreproducible scheduling orders**
* **Reentrancy** can come as an undesired surprise
* Threads need stacks hence **consumption of memory can be very high**
* **Deadlock** remains a risk, now associated with concurrency control
* Stacks for threads must be finite and can **overflow, corrupting the address space**


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/sta.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/sta.png" align="center"></a>
    <figcaption>Server threading architectures.</figcaption>
</figure>

Implemented by the server-side ORB in CORBA
* (a) Would be useful for **UDP-based service**, e.g. NTP
* (b) is the most commonly used - matches the TCP connection model
* (c) is used where the service is encapsulated as an object. Each object has only one thread, **avoiding the need for thread synchronization within objects**.


#### 3.3.3 Support for Communication and Invocation

**The performance of RPC and RMI mechanisms** is critical for **effective distributed systems**. 

Typical times for 'null procedure call':
* **Local** procedure call < 1 microseconds
* **Remote** procedure call ~ 10 milliseconds
* network time' (involving about 100 bytes transferred, at 100 megabits/sec.) accounts for only .01 millisecond; the remaining delays must be in OS and middleware - latency, not communication time.

**Factors affecting RPC/RMI performance**:
* **marshalling/unmarshalling** + operation despatch at the server
* **data copying**: application → kernel space → communication buffers
* **thread scheduling and context switching**: including kernel entry
* **protocol processing**: for each protocol layer
* **network access delays**: connection setup, network latency

#### 3.3.4 Implementation of invocation mechanism

**Most invocation middleware** (Corba, Java RMI, HTTP) is implemented over **TCP**
* For universal availability, **unlimited message size** and **reliable transfer**.
* Sun RPC (used in NFS) is implemented over both UDP and TCP and **generally works faster over UDP**.

**Research-based systems** have implemented much **more efficient invocation protocols**.

Concurrent and asynchronous invocations.
* middleware or application **doesn't block** waiting for **reply to each invocation**.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/ias.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/ias.png" align="center"></a>
    <figcaption>Invocations between address spaces.</figcaption>
</figure>

The above figure shows the particular cases of a **system call**, a **remote invocation between processes hosted at the same computer**, and a **remote invocation between processes at different nodes** in the distributed system. 


The above figure suggests that a **cross-address-space invocation** is implemented within
a computer exactly as it is between computers, except that the underlying message
passing happens to be local. Indeed, this has often been the model implemented.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/ci.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/ci.png" align="center"></a>
    <figcaption>Times for serialized and concurrent invocation.</figcaption>
</figure>

Concurrent invocation is more effective than the Serialized Invocation.


### 3.4 LRPC

#### 3.4.1 Performance

Often, the hidden but huge issue is that we want high performance

#### 3.4.2 Important optimizations: LRPC

Lightweight RPC(LRPC): for case of sender, destination on same machine.
* Uses memory mapping to pass data
* Reuses same kernel thread to reduce context switching costs
* Single system call: send_rcv or rcv_send

Bershad's LRPC: 
* Uses **shared memory** for **interprocess communication**.
    * while **maintaining protection of the two processes**
    * arguments copied only once (versus four times for convenitional RPC)
* Client threads can execute server code
    * via protected entry points only
* Up to 3 x faster for local invocations


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/lrpc.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/lrpc.png" align="center"></a>
    <figcaption>A lightweight remote procedure call.</figcaption>
</figure>

### 3.5 Callback

#### 3.5.1 Conception

In the client server model, the server is passive: 
* the IPC is initiated by the client; 
* the server waits for the arrival of requests and provides responses.

In the absence of callback, a client will have to poll a
passive server repeatedly if it needs to be notified
that an event has occurred at the server end.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/pvc.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/pvc.png" align="center"></a>
    <figcaption>Polling vs Callback.</figcaption>
</figure>

#### 3.5.2 Two-way Communications

* Some applications require that both sides may initiate IPC.
* Using sockets, duplex communication can be achieved by **using two sockets on either side**.
* With connection-oriented sockets, **each side acts as both a client and a server**.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/twc.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/twc.png" align="center"></a>
    <figcaption>Two-way communications.</figcaption>
</figure>


#### 3.5.3 RMI Callbacks

A callback client registers itself with an **RMI server**.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/rmic.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/rmic.png" align="center"></a>
    <figcaption>RMI callback.</figcaption>
</figure>


The server makes a callback to **each registered client** upon **the occurrence of a certain event**.



<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/ccsi.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/ccsi.png" align="center"></a>
    <figcaption>Callback client server interaction.</figcaption>
</figure>


1. Client looks up the interface object in the RMIregistry on the server host.

2. The RMIRegistry returns a remote reference to the interface object.

3. Via the server stub, the client process invokes a remote method to register itself for callback, passing a remote reference to itself to the server. The server saves the reference in its callback list.

4. Via the server stub, the client process interacts with the skeleton of the interface object
 to access the methods in the interface object.

5. When the anticipated event takes place, the server makes a callback to each registered
 client via the callback interface stub on the server side and the callback interface skeleton on the
 client side.



