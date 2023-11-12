---
layout: post
title:  "[Network]Medium Access Control Proptocol"
date:   2023-10-5
excerpt: "The mechanism of MAC."
tag:
- Network
comments: false
blog: true
feature: https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/blogHead/directX12partI.jpg
---


## 1.The TCP/IP Architecture


* The PDUs exchanged by the peer TCP protocols are called **TCP segments or segments**
* The PDUs exchanged by UDP protocols are called **UDP datagrams or datagrams**. 
* The PDUs exchanged by IP protocols are called **IP packets or packets**.

IP multiplexes TCP segments and UDP datagrams and performs **fragmentation**. IP packets are sent to the **network interface** for delivery across the physical network.

At the receiver:
(1) packets passed up by the network interface are d**emultiplexed to the appropriate protocol(IP, ARP, RARP)**. 

(2) The receiving **IP entity** determines whether a packet should be sent to **TCP or UDP**.

(3) The TCP(UDP) sends each segment(datagram) to the appropriate application based on the **port number**. such as Ethernet, Token Ring, ATM, PPP.

The logic addresses need to be converted into specific physical addresses to carry out the transfer of bits from one device to the other. This conversion is done by **an address resolution protocol**.

Each host in the Internet is identified by a **globally unique IP address**. 

An IP address is divided into two parts:
* a network ID 
* a host ID

The **Internet layer** provides for the transfer of information across multiple networks through the **routers**.
* At each router the network interface layer is used to **encapsulate the IP packet** into a packet or frame of the underlying network or link. 
* The IP packet is **recovered at an exit router of the given network**. This router must determine the next hop in the route to the destination and then encapsulate the IP packet into the frame of the type of the next network or link. 


To enhance the **scalability** of the routing algorithms and to control the size of a routing tables, **additional levels of hierarchy are introduced in the IP addresses**. Within a domain the host address is further subdivided into a subnetwork part and an associated host part.

Addresses of multiple domains can be aggregated to create **supernets**.



<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/MultiMedia.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/MultiMedia.png" align="center"></a>
    <figcaption>Multi Media.</figcaption>
</figure>

## 2.The Internet Protocol

**IP corresponds to the network layer** in the OSI reference model and provides **a connectionless best-effort delivery service** to the **transport layer**.
* best-effort: IP will try its best to forward packets to the destination, but does not guarantee that a packet will be delivered to the destination.

### 2.1 IP Packet

The header has **a fixed-length component of 20 bytes** plus a variable-length component consisting of options that can be up to **40 bytes**. IP packets are transmitted according to network byte order in the following groups: bits 0–7, bits 8–15, bits 16–23, and finally bits 24–31 for each row


* **Version**: The version field indicates **the version number used by the IP packet** so that revisions can be distinguished from each other.
* **Internet Header Length**: The Internet Header Length(IHL) specifies the length of the header in 32-bit words.  If no options are present, IHL will have a value of 5
* **Type of Service**: The type of service (TOS) field traditionally specifies the priority
of the packet based on delay, throughput, reliability, and cost requirements.
    * **Three bits** are assigned for priority levels (called “precedence”) 
    * **Four bits** for the specific requirement (i.e., **delay, throughput, reliability, and cost**).
    * delay bit to one to get the high priority.
* **Total Length**: The total length specifies the number of bytes of the IP packet including header and data. with 16 bits assigned, the maximum packet length is 65535 bytes. However, the Ethernet limits(Physical Networks) has limitation of 1500 bytes.
* **Identification, Flags, and Fragment Offset**: These fields are used for fragmentation
and reassembly.
* **Time to live**: The time-to-live (TTL) field is defined to indicate **the amount of time in seconds the packet is allowed to remain in the network.**  However, most routers interpret this field to indicate **the number of hops the packet** is allowed to traverse in the network.
    * this field prevents packets from wandering aimlessly in the Internet
* **Protocol**: The protocol field specifies **the upper-layer protocol** that is to receive
the **IP data at the destination host**. Examples of the protocols include TCP (protocol = 6), UDP (protocol = 17), and ICMP (protocol = 1).
* **Header checksum**: The header checksum field verifies the **integrity of the header of the IP packet**.
* **Source IP address and destination IP address**: These fields contain the addresses
of the source and destination hosts. 
* **Options**: The options field, which is of variable length, allows the packet to request
special features such as **security level**, **route to be taken by the packet**, and
**timestamp** at each router.
* **Padding**: This field is used to make the header a multiple of 32-bit words.

The IP packet is passed to the Router, the following process take place:

(1) The header checksum is computed and the fields in the headerr (version and total length)  are checked to see if they contain valid values.

(2) the router identifies the next hop for the IP packet by **consulting its routing table**.

(3) Update the TTL and header checksum fields. The IP packet is then forwarded along the next hop.

### 2.2 IP Addressing

**A node (such as a router or a multihomed host)** may have **multiple network interfaces** with each interface connected to a different network.

* An IP address has a **fixed length of 32 bits**.
* The **network ID identifies the network the host** is connected to. , all hosts connected to the same network have the same network ID.
*  The **host ID** identifies the network connection to the host rather than the actual host.

An implication of this powerful **aggregation** concept is that a router can **forward packets based on the network ID only**, thereby **shortening the size of the routing table significantly**.

* Class A addresses have seven bits for network IDs and 24 bits, for host IDs, allowing up to 126 networks and about 16 million hosts per network.
* Class B  addresses have 14 bits for network IDs and 16 bits for host IDs, allowing about 16,000 networks and about 64,000 hosts for each network.
* Class C addresses have 21 bits for network IDs and 8 bits for host IDs, allowing about 2 million networks and 254 hosts per network.
* Class D addresses are used for multicast services that allow a host to send information to a group of hosts simultaneously. 
* Class E addresses are reserved for experiments.




