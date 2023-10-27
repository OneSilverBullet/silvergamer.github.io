---
layout: post
title:  "[Network]Communication Networks and Services"
date:   2023-10-25
excerpt: "The introduction to some basic conceptions."
tag:
- Network
comments: false
blog: true
feature: https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/blogHead/directX12partI.jpg
---


## 1.Overview

A communication network, in its simplest form, is a set of equipment and facilities that provides a service: **the transfer of information between users located at various geographical points**.


Networks include telephone networks:
* telephone networks
* computer networks 
* television broadcast networks
* cellular telephone networks
* Internet

* The ability of communication networks to transfer communication at extremely high speeds allow users to **gather information** in large volumes nearly instantaneously.
* With the aid of computers, to almost immediately exercise **action at a distance**.

these two unique capabilities form the basis for many existing services and an unlimited number of future network-based services.

Several services that are examined from the point of view of user requirements: quality of service,features,capabilities.

* Radio and television broadcasting: the most common communication services.

## 2. Approaches to network design

Different user applications impose different requirements on services provided by the network in terms of:
* transfer delay
* reliability of service
* accuracy of transmission
* volume of information that can be transferred
* cost and convenience

In point of view of network designer, the task of the designer is to develop an overall network design that meets the requirements of the users in a cost-effective manner.


### 2.1 Network Functions and Network Topology

**The essential function of a network is to transfer information between a source and a destination**.
* the source and destination typically comprise terminal equipment that attaches to the network.
* This process may involve **the transfer of a single block of information** or **the transfer of a stream of information**.



<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/1network.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/1network.png" align="center"></a>
    <figcaption>The transmission of a single block of information or a stream of information.</figcaption>
</figure>



The network must be able to provide **connectivity** in the sense of providing a means for information to flow among users.
* this basic capability is provided by **transmission systems** that **transmit information by using various media sucha as wires, cables, radio, and optical fiber**.
* Networks are typically designed to carry **specific types of information representation**.

Some analogy:
* roads and high-ways: transmission lines.
* **A specific segment of road** corresponds to a **point-to-point transmission line**.
* The intersections: switches.
* routing: decide which exit corresponding to the destination.
* forwarding: move through the exit.


##### Basic components

We refer to **the access transmission lines and the first switch** as **an access network**. These access networks **concentrate the information flows prior to entering a backbone network**.

**Multiplexers** are used to **concentrate the traffic between the communities into the trunks** that connect the switches.

The associated demultiplexer and witch then direct each information flow to the appropriate destination.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/1switch.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/1switch.png" align="center"></a>
    <figcaption>The siwtch and multiplexer.</figcaption>
</figure>


##### Topology

**hierarchical network topology**: consider of geography, cost considerations, communities of interests among users.

Multi-plexers and trunks are used to interconnect a number of access networks to form a **metropolitan network**. 
* A metropolitan network might correspond to **a transportation system associated with a county or large city**.
* metropolitan subnetworks are interconnected into a **regional network**(province).
* **national network** can be viewed **as a network of regional subnetworks** hat interconnect **metropolitan networks**.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/1topology.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/1topology.png" align="center"></a>
    <figcaption>The topology of the networks.</figcaption>
</figure>


community of interest: a set of users who have a need to communicate with each other.

switches: are placed to interconnect a cluster of users where it makes economic sense.

**Traffic to more distant users is aggregated into multi-plexers that connect to more distant switches through a backbone network**.

These two trends: distance-independent communities of interest and lower communications costs, will in time lead to very different network topologies.

##### Addressing

Addressing is required to identify which network input is to be connected to which network output.

**The use of hierarchical addresses facilitates the task of routing**.


In networks, there two types of addressing:
* hierarchical addressing in **wide area networks**.
* flat addressing in **local area networks**.


##### Traffic control

Traffic controls are necessary to ensure **the smooth flow of information through the network**.


When **congestion occurs inside the network** as a result of **a surge in traffic or a fault in equipment**, the network should react by **applying congestion or overload control mechanisms** to ensure **a degree of continued operation in the network**.
* Information may be rerouted or prevented from entering the network.


##### Switching

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/1switchingType.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/1switchingType.png" align="center"></a>
    <figcaption>Switching Type.</figcaption>
</figure>


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/1compare.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/1compare.png" align="center"></a>
    <figcaption>Comparision of packet-switching and circuit-swicthing.</figcaption>
</figure>



##### Network Management

these functions fall under the category of network management:
* monitoring the performance of the network.
* detecting and recovering from faults
* configuring the network resources
* maintaining accounting information for cost and bill puirposes
* providing security by controlling access to the information flows in the network.

##### Summary

* Basic user service: the primary service or services that the network provides to its user.
* Swicthing approaches: the means of **transferring information flows between communication lines**.
* Terminal: **the end system** that connects to the network.
* Information representation: **the formate of the information** handled by the network.
* Transmission System: the means for **transmitting information across a physical medium**.
* Addressing: the means for **identifying points of connection to the network**.
* Routing: the means for **determining the path across the network**.
* Multiplexing: the means for **connecting multiple information flows into shared connection lines**.








































 