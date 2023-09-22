---
layout: post
title:  "[Network]Peer-to-Peer Protocols and Data Link Layer"
date:   2023-9-21
excerpt: "The mechanism of ARQ."
tag:
- Network
comments: false
blog: true
feature: https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/blogHead/directX12partI.jpg
---


## 1.Overview

* Layer n peer processes carry out a protocol to provide servcie to layer n+1.
* Layer n protocol uses the services of layer n-1.

A Peer-to-Peer Model: involves the interaction of two or more processes or entities through the exchange of **Protocol Data Units(PDU)**.



### 1.1 Service Model

Service Model: specifies the manner in which information is transferred.
(1)Connection-oriented services

Def: a connection **setup** procedure precedes the transfer of  information.

Process:
* Connection Setup: initializes state information and establishes a "pipe" for the transfer of information.
* Data Transfer: this state information provides a context that the layer-n peer processes use to track the exchange of PDUs. The context's value-added services:
    * in-order delivery of SDUs.
* Connection Release: remove the state information & release the resources.

(2) Connectionless services
Def: Individual self-contained blocks of information are transfered and delivered using the appropriate address.

Property:
* The service model specify a type of transfer capability.
* The service model can also include a quality-of-service(QoS) requirement(a level of performance).

best-effort service: describes a service in which **every effort is made to deliver information** but without any guarantees.


### 1.2 Services

**Service model differ according to the features they provide.**

Features:
* Arbitrary message size or structure.
* Sequencing.
* Reliability.
* Timing.
* Pacing.
* Flow control.
* Multiplexing
* Privacy, Integrity, Authentication.




## 2. ARQ Protocols and Reliable Data Transfer Service


### 2.1 ARQ Overview

ARQ: Automatic Repeat Request. The set of rules that **govern the operation of the transmitter and receiver**.
* provide reliable data transfer.
    * error detection
    * retransmission
* peer-to-peer protocols.


packets: Layer n + 1 PDUs.

frames: Layer n PDUs.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/ARQBase.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/ARQBase.png" align="center"></a>
    <figcaption>The basis of ARQ.</figcaption>
</figure>

The ARQ mechanism requires the frame to contain a header with control information.

Basic Elements:

CRC check bits: enable the receiver to determine whether errors have occured during transimission.

Information frames(I-frames): user packets, control frames and time-out mechanisms.
* control frames: short binary block that consist of a header that provides the control information followed by the CRC.
    * ACKs: acknowledge the correct receipt of a given frame or a group of frames.
    * NAKs: indicate that a frame has been received in error.
* fields: identify the type of frame.
* time-out mechanisms: we will see that the time-out mechanisms are required to prompt certain actions to maintain the flow of frames. 


Usage:
* operate over a single noisy communication channel in data link controls.
* ARQ  has been implemented more often at the edge of network in the transport layer to provide **end-to-end reliability** in the transmission of packets over **multiple hops in a network**.


The Multi-layers interactions:

(1) Each time there is information to send, the layer n + 1 process **makes a call to the layer n service** and passes the layer n + 1 PDU down. 

(2) Layer n takes the layer n SDU (layer n + 1 PDU), prepares a layer n PDU according to the **layer n protocol**, and then **makes a call to the service of layer n − 1**. 

(3) At the destination, layer n − 1 notifies the layer n process when a layer n − 1 SDU (layer n PDU) has arrived. 

(4) The layer n process accepts the PDU, executes the layer n protocol, and if appropriate, recovers the layer n SDU. 

(5) Layer n then notifies layer n + 1 that a layer n SDU has arrived.

### 2.2 Stop-and-Wait ARQ


#### 2.2.1 Interaction Process of Stop-and-Wait ARQ

Stop-and-Wait ARQ: the transmitter and receiver work on the delivery of one frame at a time.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/sawARQ.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/sawARQ.png" align="center"></a>
    <figcaption>The interaction of ARQ.</figcaption>
</figure>

The need for Sequence Number:
* the loss of an ACK can result in the delivery of a **duplicate packet**.
* premature time-outs (or delayed ACKs) combined with loss of I-frames can result in **gaps in the delivery packet sequence**.

Sequence Number: 1bit is enough to remove ambiguities in the Stop-and-Wait protocols.

The global state: $$(S_{last}, R_{next})$$
* S : the sequence number of the frame being sent in transmitter.
* R : the sequence number of the frame expecting to receive in receiver.

The ready state: (0, 0).

The process of transmission:

(1) Transmitter

**ready state**
* The transmitter is in **ready state**.
* Hight layer request the service of current layer, a packet from higher layer is received.
* The transmitter prepare a **frame**: a header with sequence number $$S_{last}$$, the packet, a CRC.
* The transmitter starts a timer and transmitted the frame using the services of the layer below.
* The transmitter enter a **wait state**.

**wait state**
* the transmitter wait until **an acknowledgement is received** or the **time-out period expires**.
    * time-out: retransmitted, timer is restarted.
    * detected error or incorrect sequence number: ignored ACK and stay in wait state.
    * correct ACK: get  $$R_{next} = (S_{last} + 1) \% 2 $$ **update S and enter ready-state**.

(2) Receiver

The receiver always in **ready-state** waiting for a notification of an arriving frame from the layer below.

* a frame arrive, the receiver accepts the frame and check errors.
    * no error: update R and accept the frame, transmitted a ACK. package is deliveried to the higher layer.
    * wrong sequence number: the frame is discarded and send a ACK with old R.
    * error frame: the frame is discarded and no further action is taken.





#### 2.2.2 Performance Issues

The delay-bandwidth product: the product of the **bit rate** and **the delay** that elapses before an action can take place.

The core conceptions:
* t_prop: the propagation time of the first bit that is input into the channel appears at the output of the channel.
* t_f: the time of receiving a total frame from the first bit.
* t_proc: the process time.
* t_ack: the time of receiving a total ACK from the first bit.
* t0: the basic delay.
* n_f: the number of bits in the information frame.
* n_a: the number of bits in the ack frame.
* n_o: the number of overhead bits
* R: the bit rate of the transmission channel.

$$t_0 = 2t_{prop} + 2 t_{proc} +  t_{f} + t_{ack} $$

$$t_0  = 2t_{prop} + 2 t_{proc} + n_f / R + n_a / R$$

The effective information transmission rate of the protocol:

$$R_{eff}^0 = (n_f - n_o) / t_o$$

The transmission efficiency of the Stop-and-Wait ARQ:

$$η_0 = R_{eff}^0 / R$$

$$η_0 = (1 - n_o / n_f) / (1 + n_a/n_f + 2(t_{prop} + t_{proc})R / n_f)$$

The delay-bandwidth product or reaction time:

$$2(t_{prop} + t_{proc})R$$

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/performanceARQ.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/performanceARQ.png" align="center"></a>
    <figcaption>The interaction of ARQ.</figcaption>
</figure>


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/performanceARQ2.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/performanceARQ2.png" align="center"></a>
    <figcaption>The performance of ARQ.</figcaption>
</figure>