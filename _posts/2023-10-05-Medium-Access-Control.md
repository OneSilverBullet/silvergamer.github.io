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


## 1.Overview

**Switched networks**: provide interconnection between users by means of transmission lines, multiplexers and switchers. The transfer of information across such networks requires **routing tables** to direct the information from source to destination.

**Broadcast networks**: All information is received by all users, routing is not necessary. A **nonhieracrchical addressing scheme** is sufficient to indicate which user the information is destined to.

**Local area networks(LANs)**: emphasis on low cost and simplicity, have been traditionally based on the broadcast approach.

**Multiple Access Networks**: a single transmission medium is shared by a community of users.

**Medium Access Control(MAC)**: to **coordinate the access to the channel(avoid colliding problem)** so that information gets through from a source to a destination in **the same broadcast network**.


## 2. Multi Access Communications

### 2.1 Conceptions

**Multi Access Communications**: The transmission medium is broadcast in nature, and so all the other stations that are attached to the medium can hear the transmission from other stations.

**Questions**：the signals will collide and interfere with each other, when stations transmit simultaneously.

Two broad categories of schemes for sharing a transmission medium.:
* Channelization Schemes: a static and collision-free sharing of the medium. They involve the partitioning of the medium into separate channels that are then decided to particular users.
    * steady stream of information
* MAC schemes: a **dynamic sharing of the meduim on a per frame basis** that is better matched to situations in which **the user traffic is bursty**.
    * random access
    * scheduling


Channel sharing techniques used in:
* networks based on radio communication.
    * Satellite communications
* Wired communications.
    * Multidrop lines were used in early data networks to connect a number of terminals to a central host computer.
* Ring networks.
    * A Mac protocol is required to orchestrate the insertion and removal of frames from the shared ring, since obviously only one signal can occupy a particular part of the ring.
* Shared buses.
    * In coaxial cable transmission systems users can inject a signal that propagates in both directions along the medium, eventually reaching all stations connected to the cable.
* Wireless communications    
    * also provide some degree of quality-of-service(QoS) guarantees.

Two basic approaches:
* Centralized.
    * Communications between M stations makes use of two channels.
    * A base station or controller communicates to all the other stations by **broadcasting** over the "outbound", "forward", "downstream" channel. In turn, the stations in turn share an "inbound", "reverse" or "upstream" channel in the direction of the base station.
    * examples: satellite system, cellular telephone systems, multidrop line, cable modem access systems and certain types of wireless data access systems.
* Distributed
    * providing communications uses **direct communications between all M stations**.
    * examples: ad hoc wireless networks, multitapped bus.

Ring Network has features of both approaches.

### 2.2 Delay-bandwidth Product and MAC Performance.


A collision of frame transmissions take place. In the case station B must have begun its transmission somtime between times t=0 and t=t_prop. 
* The stations began transmitting earlier (greater than t_prop/2) is declared to be the winner. The winner proceeds to retransmit its frames as soon as the channel goes quiet.
* The losing station defers and remains quiet until the frame transmission from the other stations is completed.
* the winning station is compelled to **remain quiet for 2t_prop** after it has completed its frame transmission.

Assume L is **the frame length**, R is **the transmission bit rate**. The seconds required to transmit a frame:

$$X = L/R$$

A frame transmission time requires:

$$L/R + 2t_prop$$

The throughput of a system is defined as the actual rate at which information is sent over the channel. The **maximum throughput**:

$$R_{eff} = L / (L/R + 2t_{prop}) =  R / (1 + 2t_{prop}R / L) = R/(1 + 2a) $$

**Normalized maximum throughput or efficiency**:

$$ρ_{max} = R_{eff} / R = 1 / (1 + 2a)$$

The **normalized delay-bandwidth product a**: 

$$a = t_{prop}R / L = t_{prop}/(L/R) = t_{prop}/X$$

When a is much smaller than 1, the medium can be used very efficiently by using the above protocol. When a becomes larger, the channel becomes more inefficient.


The selection of a MAC protocol for a given situation depends on **delay-bandwidth product** and **throughput efficiency**. 

The **frame transfer delay T**: the time that elapses from when the first bit of the frame arrives at the source MAC to when the last bit of the frame is delivered to the destination MAC.

The properties of MAC:
* Fairness: giving stations equitable treatment.
* Reliability: respect to failure and faults in equipment is improtant as it affects the availability of service.
* Different types of traffic: stream versus bursty traffic as well as the capability  to  provide quality-of-service.
* Scalability: respect to number of stations and the user bandwidth requirements.


Suppose the M stations in the system generate frames at **an aggregate rate of λ frames/second**.

The **average bit rate generated by all the stations is λL bits/secon**.

The **normalized throughput or load** is defined as:

$$ρ = λL / R < 1$$

Every MAC protocol has **a maximum throughput less than R/L frames/second**, while some channel time is waste in collision or in sending coordination information.


The abscissa (x-axis) ρ represents the **load** generated by all the stations. The ordinate (y-axis) gives the **average frame transfer delay E[T]** in **multiples of a single average frame transmission time X = L/R seconds**.

Finnaly, the rate of frame arrivals from all the stations reaches a point that exceeds the rate that the channel can sustain. At this point  the buffers in the stations build up with  a backlog of frames, and if arrivals remain unchecked, the transfer delays grow without bound as shown in the figure.

The **transfer delay versus load curve** varies with parameter a, which defined as **the ratio of the one-way delay-bandwidth product to the average frame length**. When a is **small**, the relative cost of coordination is low and ρ_max can be close to 1.





















