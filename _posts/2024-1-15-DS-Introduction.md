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


### Resource Sharing



The only access we have to the service is via the set of operations that it exports.



### Challenges

#### 1.5.1 Heterogeneity

 Heterogeneity (that is, variety and difference)  applies to all of the following:
 * Networks
 * Computer Hardware
 * Operation Systems
 * Programming Language
 * Implementation by different developers

 

## 3. Networking and Internetworking


### 3.1 Interprocess Communications

Distributed computing requires information to be exchanged among **independent processes**. Operating systems provide facilities for interprocess communications (IPC)
* message queues
* semaphores
* shared memory

Distributed computing systems make use of these facilities to provide application programming interface which allows IPC to be programmed at a higher level of abstraction.


#### (1)Conceptions

* Message: communicate between processes. Arbitrary size byte stream.
* Packet: a fragment of a message that might travel on the wire. Variable size but limited, usually to 1400 bytes or less.
* Protocol: an algorithm by which processes cooperate to do something using message exchanges.
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

The network is not reliable

A. **Different packets/messages** between a **sender** and a **receiver** may take different paths/time and **arrive out of order**.

B. Link can corrupt message
* Rare in high quality ones on the internet backbone
* More common with wireless connections, cable modems, ADSL

C. Routers can get overloaded
* When this happens they drop messages
* This is very common

Solution: Protocols retransmit lost packets increase reliable.

#### (3) Internet Protocol

Three IP Services:
* Unicast: transmits a packet to specific host.
* Multicast: transmits a packet to a group of hosts.
* Anycast: transmits a packet to one of a group of hosts.(nearest)

Destination and source identified by the **IP address** 
* 32 bits for IPv4
* 128 bits for IPv6

Packet may be **dropped**, **duplicated**, and **received in a different order**.


#### (4) IP v4 addresses

In binary, a 32-bit integer. 

In text, this: “128.52.7.243”:
* Each decimal digit represents 8 bits (0 – 255)

“Private” addresses are not globally unique:
* Used behind NAT boxes
* 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16

Multicast addresses start with 1110 as the first 4 bits (Class D address):
* 224.0.0.0/4.
* Unicast and anycast addresses come from the same space.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/port.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/port.png" align="center"></a>
    <figcaption>Logical Ports: Process and Port.</figcaption>
</figure>

### 3.2 UDP & TCP

#### 3.2.1 UDP: User Datagram Protocol

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

#### 3.2.2 TCP: Transmission Control Protocol

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

#### 3.2.3 UDP vs. TCP

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

### 3.3 OSI

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


### 3.4 Sockets and Port

#### 3.4.1 UDP Client and Server

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

#### 3.4.2 Stream-mode Socket API(Connection-oriented)

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


### 3.5 RPC: Remote Procedure Call using Sockets

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/RPC.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/RPC.png" align="center"></a>
    <figcaption>RPC.</figcaption>
</figure>

* Server program is shared by many clients.
* **RPC protocol typically used to issue requests**.
* **Server may manage special data, run on an especially fast platform, or have an especially large disk**.
* Client systems handle “front-end” processing and interaction with the human user.

#### 3.5.1 Binding Stage

* Occurs when client and server program first start execution
* Server registers its network address with **name directory**, perhaps with other information.
* Client scans directory to find appropriate server.
* Depending on how RPC protocol is implemented, may make a “connection” to the server, but this is not mandatory

#### 3.5.2 Marshalled & Unmarshalled
We say that data is “marshalled” into a message and “unmarshalled” from it
* Representation needs to deal with **byte ordering issues** (big-endian versus littleendian), **strings** (some CPUs require padding), alignment, etc
* Goal is to be as fast as possible on the most common architectures, yet must also be very general.

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

Data transmitted on the network is a binary stream.
* An **interprocess communication system** may provide the capability to allow data representation to be imposed on the raw data.
* Because different computers may have different internal storage format for the same data type, an external representation of data may be necessary.
* Data marshalling is the process of 
    * (i) flattening a data structure.
    * (ii) converting the data to an external representation. 

Some well known **external data representation schemes** are: Sun XDR, ASN.1 (Abstract Syntax Notation), XML (Extensible Markup Language).

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/dec.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/dec.png" align="center"></a>
    <figcaption>Data Encoding Protocols.</figcaption>
</figure>

#### 3.5.3 RPC Semantics

* At most once: request is processed 0 or 1 times
* Exactly once: request is always processed 1 time
* At least once: request processed 1 or more times

#### 3.5.4 RPC versus local procedure call

* Restrictions on argument sizes and types
* New error cases
    * Bind operation failed
    * Request timed out
    * Argument “too large” can occur if, e.g., a table grows
* Costs may be very high

#### 3.5.5 RPC costs in case of local destination process

Often, the destination is right on the caller’s
machine!

* Caller builds message
* Issues send system call, blocks, context switch
* Message copied into kernel, then out to destination
* Destination is blocked... wake it up, context switch
* Destination computes result
* Entire sequence repeated in reverse direction
* If scheduler is a process, context switch 6 times!

#### 3.5.6 Synchronous vs. Asynchronous Communication

* The IPC operations may provide the synchronization necessary using blocking. A blocking operation issued by a process will block further processing of the process until the operation is fulfilled.

* Alternatively, IPC operations may be asynchronous or nonblocking. An asynchronous operation issued by a process will not block further processing of the process. Instead, the process is free to proceed with its processing, and may optionally be notified by the system when the operation is
fulfilled.


When using an IPC programming interface, it is important to
note whether the operations are synchronous or asynchronous.

 If only blocking operation is provided for send and/or receive,
then it is the programmer’s responsibility to using child
processes or threads if asynchronous operations are desired.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/t.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/t.png" align="center"></a>
    <figcaption>Threads asynchronous.</figcaption>
</figure>