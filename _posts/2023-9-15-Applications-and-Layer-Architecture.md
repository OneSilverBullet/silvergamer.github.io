---
layout: post
title:  "The Application and Layer Archinecture of Communication Network"
date:   2023-9-17
excerpt: "The basic conceptions of network."
tag:
- Network
comments: false
blog: true
feature: https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/blogHead/directX12partI.jpg
---

## 1.Overview

### 1.1 Examples of Protocols, Services and Laying

Network Architecture: a set of protocols that specify how every layer is to function.

Protocol: **a set of rules that governs how two or more communicating parties are to interact**. 
* The purpose of protocol is to provide some type of communication service.
* HTTP: enable the retrieval of web pages.
* TCP protocol: provides a reliable byte-stream transfer service that can be used to transmit files across the Internet.

A protocol is solely concerned with the interaction between the two peer processes(client & server).
* The protocal assumes that the message exchange between peer processes occurs directly.


#### 1.1.1 HTTP, DNS and SMTP

Client/Server Application:
* A server process in a computer waits for incoming requests by **listening to a port**.


Port: an address that **identifies which process is to receive a message that is delivered to a given machine**.

Ephemeral Port Number: the client-side port number.

Globally Unique Addresses: 



Widely used applications have **well-known port numbers** assigned to their services.
* The client process in other computer can readily **make requests** as required.
* The servers provide responses to those requests. The server runs in the background as daemon.

The client and server machines are not usually connected directly, **a connection needs  to be set up between them**.

**Example: HTTP**

* In HTTP, we require a two way connection that **transfers a stream of bytes in correct sequential order and without errors**.
* The TCP protocol provides this type of **communication service** between two processes in two machines connected to a network.
* Each HTTP process **inserts its message into a buffer**, and TCP transmits **the contents of the buffer to the other TCP in blocks of information called segments**. 

Attention:**HTTP is said to use the service provided by TCP in the layer below**.
* The transfer of messages between HTTP clients and server in fact is **virtual** and occurs **indirectly** via TCP connection.


**Implementation: Socket**

When the HTTP client software first needs to set up the TCP connection, the client does so by making a series of socket system calls. 
* These calls are similar to function calls except **that control is passed to the operating system kernel when a socket system call is made**. A socket system call specifies a certain action and may contain parameters such as socket type, for example, TCP or UDP, and address
information. 
* Thus the interaction between the HTTP layer and the TCP layer takes place through these socket system calls.


#### 1.1.2 TCP and UDP Transport Layer Services

Bothe TCP and UDP protocols operate by **using the connectionless packet network service provided by IP**.

* UDP: provides connectionless transfer of **datagrams** between processes in hosts attached to the Internet. 
    * provides port numbering to identify the source and destination process in each host.
	* no guarantees in terms of delivery or sequence addressing.
	* simple and fast
* TCP: provides for reliable transfer of **a byte stream** between processes in hosts attached to the Internet.
    *  The processes write bytes into a buffer for transfer across the Internet by TCP.
    * TCP involves the establishment of a connection between the two processes.
	* TCP entites implement error detection, retransmission, flow control algorithm and congestion control.


Datagrams: IP packets.


Some examples:

Domain Name System(DNS) query: determine the IP address corresponding to the host name. DNS is based on UDP.

IP Address: identify the host's network interface. It contains two parts:
* network id.
* host id.

Globally Unique IP Address: each host is identified by it.

Router: a node that is attached to two or more physical networks.

HyperText Transfer Protocal(HTTP): specifies rules by which the client and server interact to retrieve a document.

Simple Mail Transfer Protocal(SMTP):



## 2.the OSI Reference Model

### 2.1 the OSI Reference Model


The OSI Reference Model:
* Patitioned the communication process into seven layers.
* Provide a framework for talking about the overall communications process.

The top four layers are **end to end** and involve **the interaction of peer processes across the network**. In contrast the lower two layers of the OSI
reference model involve **interaction of peer-to-peer processes across a single hop**.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/OSI.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/OSI.png" align="center"></a>
    <figcaption>OSI Reference Model.</figcaption>
</figure>

(1) **Physical Layer**: deals with the transfer of bits over a communication channel.
* is concerned with the particular choice of system parameters such as voltage levels and signal durations.
* is conceredn with the procedures to set up and release the physical connection.

example: **the digital transmission system and the transmission media** such as copper
wire pairs, coaxial cable, radio, or optical fiber.

 Ethernet physical layer standard.

(2) Data Link Layer: provides for **the transfer of frames(blocks of information) across a transmission link** that directly connects two nodes.

*  The data link layer **inserts framing information** in the sequence of transmitted bits to **indicate the boundaries of the frames**.
*  It also **inserts control and address information** in the header and check bits to enable **recovery from transmission errors, as well as flow control**.

Important when the transmission link is prone to transmission errors.

example: High-Level Data Link Control(HDLC), Point-To-Point (PPP).

The OSI data link layer was defined so that it included the functions of LANs,
which are characterized by **the use of broadcast transmissions**. The notion of a “link,”
then, includes the case **where multiple nodes are connected to a broadcast medium**. 
* A **flat addressing space** is used to enable machines to **listen and recognize frames that are destined to them**.


(3)Network Layer: provides for the transfer of data **in the form of packets** across a communication network.

One key aspect of the network layer is **the use of a hierarchical addressing scheme** that identifies the point of attachment to the network and that can **accommodate a large number of network users**.

Key Aspect of the packet transfer service:
* Routing Protocl: **is used to select paths across a network**. The nodes in the network must work together to perform the routing effectively. 
* The network layer is also responsible for **dealing with the congestion** that occurs
from time to time due to **temporary surges in packet traffic**.

**Eaxmple: Packet-switching & Internetwork**

In Packet-swicthing network:
* A single address space.
* A uniform routing procedure.

In internetwork:
* Internetworking protocols are necessary to **route data between gateways/routers** that connect the inermediate networks.
* **deal with differences in addressing and differences in the size of the packets** that are handled within each network
* This internet sublayer of the network layer assumes **the responsibility for hiding the details of the underlying network(s) from the upper layers**.


* **Each intermediate node in the network must implement the lower three layers**. 
* Thus **one pair of network layer entities exists** for each hop of the path required through the network. 
* Note that the network layer entities in the source and destination machines **are not peer processes**. 


(4) Transport Layer: is responsible for **the end-to-end transfer of messages** from a process in the source machine to  a process in the destination machine.
* The transport layer protocol **accepts messages from its higher layers** and **prepares blocks of information called segments or datagrams** for transfer between end machines.

The transport layer uses the services offered by the underlying network or internetwork to provide the session layer with **a transfer of messages that meets a certain quality of service**.

The transport layer can be responsible for **setting up and releasing connections across the network**. 

To optimize the use of network services, the transport layer may **multiplex several transport layer connections onto a single network layer connection**. On
the other hand, to meet the requirements of a high throughput transport layer connection,
the transport layer may **use splitting to support its connection over several network layer connections**.

**Example:**

At on extrem the transport layer may provide a **connection-oriented** service that involves the error-free transfer of a sequence of bytes or messages.
* error-detection
* recovery

At another extrem the transport layer instead provide an unconfirmed connectionless service that involves the transfer of individual messages. 
* the transfer layer is to provide the appropriate address information.
* The transport layer may be called upon to **segment messages** that are too long into shorter blocks of information for **transfer across a network** and to **reassemble there messages at the destination**.

In TCP/IP networks, **processes typically access the transport layer through socket interfaces that are identified by a port number**.


(5) Session Layer: control the manner in which data are exchanged.

* certain applications require a half-duplex dialog where the two parties
take turns transmitting information.

*  Other applications require the introduction of synchronization points that can be used to mark the progress of an interaction and can serve as points from which error recovery can be initiated.

(6) Presentation Layer: provide the application layer with **independence from differences in the representation of data**.

The presentation layer should first convert the machine-dependent information provided by application A into a machine-independent form, and later convert the machine-independent form into a machine-dependent form suitable for application B.

(7) Application Layer: provide **services that are frequently required by applications that involve communications**.

Application layer protocols have been developed for file transfer, virtual terminal (remote log-in), electronic mail, name service, network management, and other applications.

In general **each layer adds a header, and possibly a trailer**, to the block of information
it accepts from the layer above.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/OSI2.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/OSI2.png" align="center"></a>
    <figcaption>OSI Reference Model.</figcaption>
</figure>


### 2.2 Unified View of Layers, Protocols, Services


In each layer, a process on one machine carries out a conversation with a peer process on the other machine across a **peer interface**.

In OSI terminology **the processes at layer n are referred to as layer n entities**.

* Layer n entities communicate by exchanging protocol data unit(PDU).
* Each PDU contains a header, contains protocol control information and user information.
* The behavior of the layer n entities is **governed by a set of rules or conventions called the layer n protocol**.

Communication between the entities is usually virtual in the scense that no direct communication links exists between them.

For communication to take place, **the layer n+1 entities make use of the service provided by layer n.**

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/Peer2Peer.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/Peer2Peer.png" align="center"></a>
    <figcaption>Peer to Peer Connection.</figcaption>
</figure>



Q: How to transmit the layer n+1 PDU?

A: pass a block of information from layer n+1 through **the layer n services access point(SAP)** across a service interface.
* SAP is a software port.
* Each SAP is identified by a unique identifier.

Q: What is the block of information?

A: The block of information passed between layer n and layer n+1 entities consists of **control information** and **a layer n service data unit(SDU)**.
* The layer n SDU is the layer n+1 PDU, and the layer n SDU is **encapsulated** in the layer n PDU.
* The layer n entity uses the control information to form a header that is attached to the SDU to produce **the layer n PDU**.
* The communication process is completed when the SDU(layer n+1 PDU) is passed to the layer n+1 peer process.

**SDU are exchanged between layers, while PDU are exchanged within a layer.**

**Layer n+1 as a user of the service provided by layer n**, is only interested in the correct execution of the service required to transfer its PDUs. **The implement details below layer n+1 are irrelevant**.

The service provided by layer n typically involves accepting a block of information
from layer n + 1, transferring the information to its peer process, which in turn delivers
the block to the user at layer n + 1.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/EntitiesService.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/EntitiesService.png" align="center"></a>
    <figcaption>Entities Connection.</figcaption>
</figure>

The service provided by the layer can be **connection oriented** or **connectionless**.

**Connection-Oriented Service** Phases:

(1) Establishing  a connection between two layer n SAPs.
* intializing state information: the sequence number, flow control variables, buffer allocation.

(2) Transferring layer n-SDU using the layer n protocol.

(3) Tearing down the connection and releasing the resources.
Example: the HTTP and the TCP.

**Connectionless service does not require a connection setup**, and each SDU is
transmitted directly through the SAP

(1) Each SDU transmitted directly through the SAP.

(2) The control information that is passed from layer n+1 to layer n must contain all the address information required to transfer the SDU.

Example: the DNS and the UDP.

**In general, it is not necessary for the layers to operate in the same connection mode**. 


The service provided by the layer n can be confirmed or unconfirmed depending on **whether the sender must enventually be informed of the outcome**.
* The connection setup is usually a confirmed service.
* The connectionless service can  be confirmed or unconfirmed depending on **whether the sending entity needs to receive an acknowledgement**.





#### Segmentation & Reassembly

If the layer n SDU is too large:

Segmentation: **The layer n SDU is segmented into multi layer n PDUs** that are then transmitted using the service of layer n-1.

Reassembly: **The layer n entity must reassemble the original layer n SDU from the sequence of layer n PDUs** it receives.


If the layer n SDU is too small:

Blocking: the layer n entity block several layer n SDUs int oa single layer n PDU.

Unblocking: the other layer n entity must unblock the received PDU into the individual layer n SDUs.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/Seg.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/Seg.png" align="center"></a>
    <figcaption>Segmentation & Reassembly, Blocking & Unblocking.</figcaption>
</figure>



####  Multiplexing

**One service** to **Multi users**:

Multiplexing involvs **the sharing of a layer n service** by multiple layer n+1 users.

Demultiplexing: when the layer n PDUs arrive at the other connection end, the layer n SDU is recovered and must then be delivered to the appropriated layer n+1 user.
* a  multiplexing tage is needed in each PDU.

Example: Several application layers share the datagrams service provided by UDP.

Usage: Increase efficiency.

**Multi services** to **One user**:

Splitting: the use of several layer n services to support a single layer n+1 user. The SDUs from the single user are **redirected to one of several layer n entities**, which in turn transfer the given SDU to a peer entity at the destination end.

Recombination: take place at the destination where the SDUs recovered from each of the layer n entities are passed to the layer n+1 user.

Usage: Increase reliability.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/Multiplexing.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/Multiplexing.png" align="center"></a>
    <figcaption>Multiplexing Situation.</figcaption>
</figure>


The server-side port number is awell-known port numberthat unambiguously identifies the process that is to receive the SDU at the server end.

The client-side port number is an ephemeral number that is selected when the socket for the application is established.



## 4. TCP/IP Architecture

## 4.1 TCP/IP Conceptions

**TCP/IP Network Architecture**: a set of protocols that allows communication across multiple diverse networks. 

The TCP/IP model does not require strict layering, and it contains four layers:
(1) Application Layer: provide services that can be used by other applications.
* incorporate the function of the top three layer of OSI Layers.
* application layer programs are intended to run directly over the transport layer.

EXAMPLE: protocols have been developed for remote login, for e-mail, for file transfer, and for network management.

(2) Transport Layer:
* have two types of services:
	* Transmission Control Protocol(TCP): consists of reliable connection-oriented transfer of a byte stream.
	* User Datagram Protocol(UDP): best-effort connectionless transfer of individual message.

(3) Internet Layer: handles the transfer of information across multiple networks through the use of routers; **deal with the routing of packets**.

* The connect provide single service: best-effort connectionless packet transfer.
* IP packets are exchanged between routers without a connection setup.

(4) the network interface layer: is connected with the network-specific aspects of the transfer of packets.
* the network interface layer is particularly concerned with the protocols that access the intermediate networks.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/TCPIP.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/TCPIP.png" align="center"></a>
    <figcaption>TCP/IP Model.</figcaption>
</figure>

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/TCPIP2.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/TCPIP2.png" align="center"></a>
    <figcaption>TCP/IP Model.</figcaption>
</figure>




## 4.2 TCP/IP Protocol: The process of layers working together

On a LAN the attachment of a device to the network is often identified by a phisical address.

Each **Ethernet network interface card(NIC)** is issued a **globally unique medium access control(MAC)** or physical address.
* the machines connected to the NIC have unique address.


Sender information transfer between layers:
(1) The TCP segment is passed to the IP Layer, which in turn encapsulates the segment into Internet Packet.
* IP packet header:
	* protocol field: designate the layer that is operating above IP.
	* IP Address of sender.
	* IP Address of destination.

(2) The IP datagram is then encapsulated using PPP and then sent to the server.

Receiver information transfer between layers:
(1) The server NIC captures the Ethernet frame and extracts the IP datagram and passes it to IP entity. 

(2) The protocal field in IP header indicates that a TCP segment is to be extracted and passed on to the TCP layer.

(3) The TCP layer uses the port number to find out that the message is likely to be passed to the HTTP server process.

End-to-end process-to-process connection: let the receiver know which connection the message correspond to.
* socket address: the port number, the IP address, the protocol type.
* the source socket address and the destination socket address together uniquely specify the connection between the server process and client process.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/EthernetPDU.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/EthernetPDU.png" align="center"></a>
    <figcaption>TCP/IP Model.</figcaption>
</figure>



## 4.3 TCP/IP Protocol: Sending and Receiving IP Datagrams

**Statement1: the client know the server IP address**

(1) The IP entity of client looks at its routing table to see whether it has an entry for the compelete IP address.
* find the server is connected to the same Ethernet.
* find the physical address of the server

(2) The IP datagram is passed to the Ethernet Device Driver, and prepare the Ethernet Frame.
* source physical address w.
* destination physical address s.
* protocol control field

(3) The Ethernet frame is broadcast over the LAN.

(4) The server's NIC recognizes that the frame is intended for its host, so the card captures the frame and examines it.


**Statement2: the client does not know the server IP address**
(1) Check the IP:
* The IP entity of client looks at its routing table to see whether it has an entry for the complete IP address of the PC.
* The IP entity then checks to see whether it has a routing table entry that matches the network id portion of the IP portion of the IP address of the PC.
* The IP entity then checks to see whether it has an entry that specifies a default router.

(2) The IP datagram is passed to the Ethernet device driver, and prepares an Ethernet Frame, and broadcast the LAN.
* source physical address
* destination physical address

(3) The router's NIC captures the Ethernet frame and examines it.

(4) The router finds out the routing table, ecapsulates the examined IP datagram into Ethernet frame and sends it directly to the target.





