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

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/MultiMedia.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/MultiMedia.png" align="center"></a>
    <figcaption>Multi Media.</figcaption>
</figure>


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

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/Satellite.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/Satellite.png" align="center"></a>
    <figcaption>Statellite Communications.</figcaption>
</figure>

* Wired communications.
    * Multidrop lines were used in early data networks to connect a number of terminals to a central host computer.
* Ring networks.
    * A Mac protocol is required to orchestrate the insertion and removal of frames from the shared ring, since obviously only one signal can occupy a particular part of the ring.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/Ring.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/Ring.png" align="center"></a>
    <figcaption>Ring.</figcaption>
</figure>

    
* Shared buses.
    * In coaxial cable transmission systems users can inject a signal that propagates in both directions along the medium, eventually reaching all stations connected to the cable.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/Bus.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/Bus.png" align="center"></a>
    <figcaption>Bus.</figcaption>
</figure>
    
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

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/ChannelCapture.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/ChannelCapture.png" align="center"></a>
    <figcaption>The channel capture and delay-bandwidth product.</figcaption>
</figure>


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


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/load.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/load.png" align="center"></a>
    <figcaption>The transfer delay versus load curve.</figcaption>
</figure>


The abscissa (x-axis) ρ represents the **load** generated by all the stations. The ordinate (y-axis) gives the **average frame transfer delay E[T]** in **multiples of a single average frame transmission time X = L/R seconds**.

Finnaly, the rate of frame arrivals from all the stations reaches a point that exceeds the rate that the channel can sustain. At this point  the buffers in the stations build up with  a backlog of frames, and if arrivals remain unchecked, the transfer delays grow without bound as shown in the figure.

The **transfer delay versus load curve** varies with parameter a, which defined as **the ratio of the one-way delay-bandwidth product to the average frame length**. When a is **small**, the relative cost of coordination is low and ρ_max can be close to 1.



## 3. Random Access

### 3.1 ALOHA


Difference between noraml transmission errors and frame collision:
* Transmission Error: noise affect only a single station.
* Frame Collision: more than one retransmission.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/6aloharandom.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/6aloharandom.png" align="center"></a>
    <figcaption>The aloha.</figcaption>
</figure>



Aloha scheme requires stations to use a **backoff algorithm**, which typically chooses a **random number in a certain retransmission time interval**. This randomization is intended to 
* spread out the retransmissions.
* reduce the likelihood of additional collisions between the stations.

ALOHA protocol: If no acknowledgment is received after a time-out period, a backoff algorithm is used to **select a random retransmission time**. In the ALOHA scheme the network can swing between two modes:
* The first mode: frame transmissions from the station **traverse the network successfully** on the first try and collide only from time to time.
* The second mode: a snowball effects when there is a surge of collisions. The increaed number of backlogged stations, stations waiting to retransmit a message, increase the likelihood of additional collisions.

Assume that a frame has a constant length L and constant transmission time:
$$X = L/R$$

This frame will be transmitted successfully at time t0 and completed at time t0+X.

Vulnerable period: t0-X to t0+X. Any frames begin its transmission in the period of vulnerable period will be collided with reference frame.

**arrival rate of new frames(S)**: in units of frames/X seconds.

**total arrival rate(G)**: in units of frames/X seconds. also called total laod.


The key simplifying assumption: the backoff algorithm spreads the retransmissions so that frame transmissions, new and repeated, are **equally likely to occur at any instant in time**.


The number of frames transmitted in a time interval has a poisson distribution:

$$P[k  transmissions in 2X seconds] = ((2G)^k/k!)e^{-2G}$$

The theoughput S is equal to the total arrival rate G times the probability of a successful transmission.

$$S = GP[NoCollision] = GP[0transmissionin2Xseconds]$$
$$S = Ge^{-2G}$$



### 3.2 Slotted ALOHA

The performance of the ALOHA scheme can be improved by reducing the probability of collisions.

Slotted ALOHA: reduces collisions by **constraining the stations to transmit in synchronized fashion**. 
* All the transmission keep track of transmission time slots.
* are allowed to initiate transmissions only at the beginning of a time slot.
* Frasmes are assumed to be constant and to occupy one time slot.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/SlottedALOHA.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/SlottedALOHA.png" align="center"></a>
    <figcaption>The slotted aloha.</figcaption>
</figure>



Vulnerable period: from t0-X to t0.

$$S = GP[no collision] = GP[0transmissionsinXseconds]$$

$$S = Ge^{-G}$$

Aloha and Slotted Aloha show how low-delay frame transmission is possible using essentially uncoordinated access to a media.

### 3.3 Carrier Sense Multiple Access

Carrier Scense Multiple Access(CSMA) MAC: By sensing the meduim for the **presence of a carrier signal** from other stations, a station can **determine whether there is an ongoing transmission**.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/6CSMA.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/6CSMA.png" align="center"></a>
    <figcaption>The CSMA.</figcaption>
</figure>


CSMA basic process; How the **vulnerable period** is determined in a CSMA.

(1) At time t = 0, station A begins transmission at one extreme end of a broadcast medium.

(2) As the signal propagates through the medium, stations become aware of the transmission from station A.

(3) At time t = t_prop, transmission from station A reaches to the other end of the medium.

The vulnerable period consists of **one propagation delay**. If no other station initiates a transmission during this period, station A will in effect capture the channel.

### 3.3.1 1-Persistent CSMA

In 1-Persistent CSMA, stations with a frame to transmit sense the channel. If the channel is busy, **they sense the channel continuously**, waiting until the channel becomes idle. As soon as the channel is sensed idle, they transmit their frames. 




Collisions Occur:
* **If more than one stations is waiting, a collision will ocur.**
* stations that have a frame arrive within t_prop of the preceding transmission will also transmit and possibly be involved in a collision.


 1-Persistent CSMA act in a  greedy fashion, attemping to access the medium as soon as possible. As a result, 1-Persistent CSMA has a relatively **high collision rate**.

 ### 3.3.2 Non-Persistent CSMA

If the channel is busy, the stations immediately **run the backoff algorithm and reschedule a future resensing time**. If the channel is idle, the stations transmit. 

By immediately rescheduling a resensing time and not persisting, the incidence of collisions is reduced.


 ### 3.3.3 p-Persistent CSMA

Stations with a frame to transmit sense the channel. If the channel is busy, they **persist with sensing until the channel becomes idle**. If the channel is idle:
* with probability p, the station transmits its frame.
* with probability 1-p, the station decides to **wait an additional propagation delay t_prop before again sensing the channel**.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/6ppCSMA.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/6ppCSMA.png" align="center"></a>
    <figcaption>The pp CSMA.</figcaption>
</figure>


This behavior is intended to **spread out the transmission attempts** by the stations that have been waiting for a transmission to be completed and hence to **increase the likelihood that a waiting station successfully seizes the medium**.

 ### 3.3.4 Conclusion

All the variations of CSMA are sensitive to the end-to-end propagation delay of the medium that constitues the vulnerable period.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/6CSMAComp.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/6CSMAComp.png" align="center"></a>
    <figcaption>Comparison.</figcaption>
</figure>




It can also be seen that the **normalized propagation delay a = t_prop / X has a significant impact on the maximum achievable throughput**.

The CSMA schemes improve over the ALOHA schemes by reducing the vulnerable period from one- or two-frame transmission times to a single propagation delay t_prop.

### 3.4 Carrier Sense Multiple Access with Collision Detection

The carrier sensing multiple access with collision detection(CSMA-CD): a station can determine whether a collision is taking place, then **the amount of wasted bandwidth can be reduced by aborting the transmission when a collision is taking place**.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/6CSMACD.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/6CSMACD.png" align="center"></a>
    <figcaption>CSMA-CD.</figcaption>
</figure>


If a collision is detected during transmission, then **a short jamming signal is transmitted** to ensure that other stations know that collision has occurred before aborting the transmission, and **the backoff algorithm is used to schedule a future resensing time**.


The channel can be in three states: 
* busy transmitting a frame.
* idle.
* in a contention period where stations attempt to capture the channel. 

The **throughput performance of 1-persistent CSMA-CD** can be analyzed by assuming that time is divide into **minislots of length 2t_prop seconds** to ensure that stations can always detect a collision. 

**Stations content for the channel by transmitting and listening to the channel to see if they have successfully captured the channel.**


Each station transmits during a contention minislot with probability p. The probability of a successful transmission is given by the probability that only one station transmits:

$$P_{success} = np(1-p)^{n-1}$$

To find the maximum achieveable throughput, we find the probability is maximized when p=1/n. The maximum probability of success is:

$$P_{success}^{max}=(1-1/n)^{n-1}=1/e$$

If the probability of success in one minislot is P {max}{success}, the **average number of minislots that elapse until a station successfully captures the cahnnel** is 1/P{max}{success}. Assuming a large value of n, and so the average number of minislots until a station successfully captures the channel  is:

$$1/P_{success}^{max}=e= 2.718minislots$$

The maximum throughput in the CSMA-CD system occurs when all of the channel time is spent in frame transmissions followed by contention intervals. Each frame transmission time X is followed by a period t_prop during which stations find out that the frame transmission is completed and then a contention interval of average duration 2et_prop, the maximum throughput:

$$ρ_{max}=X/(X+t_{prop}+2et_{prop})=1/(1+(2e+1)a)=1/(1+(2e+1)Rd/vL)$$

$$a = t_prop/X$$

* a is the propagation delay normalized to the frame transmission time.
* R: bit rate of the medium.
* d: diameter of the medium.
* v: propagation speed over the medium.
* L: frame length.

CSMA-CD can achieve throughputs that are close to 1 when a is much smaller than 1. 

CSMA-CA provide the basis of **Ethernet LAN** protocol.

It should be emphasized that CSMA-CD does not provide an orderly transfer of frames. The random backoff mechanism and the random occurrence of collisions imply that frames need not be transmitted in the order that they arrived.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/6CSMAaCurve.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/6CSMAaCurve.png" align="center"></a>
    <figcaption>All comparision.</figcaption>
</figure>




ALOHA and slotted ALOHA are not sensitive to a since their operation does not depend on the
reaction time.

## 4 Scheduling Approaches To Medium Access Control

The random access approaches:
* Advantages: The random access approaches provide **low-delay frame transfer** in broadcast networks.
* Disadvantages: The random access approaches **limit the maximum achievable throughput** and can **result in large variability in frame delays** under traffic loads.

**The scheduling approaches to medium access control** attempt to produce **an orderly access** to the transmission medium.

### 4.1 Reservation System

The stations take turns transmitting a single frame at the full rate R bps, and the transmissions from the stations are organised into cycles that are variable in length.

* Each cycle begins with **a reservation interval**.

In the simplest case, the reservation interval **consists of M minislots, one minislot per station**. Stations use their corresponding minislot to indicate that **they have a frame to transmit in  a corresponding cycle**. The stations announce their intention to transmit a frame by broadcasting their reservation bit during the appropriate minislot.

**By listening to the reservation interval, the stations can determine the order of frame transmissions in the corresponding cycle**. The length of the cycle will then correspond to **the number of stations that have a frame to transmit**.
* Variable-length frames can be handled if the reservation message includes frame-length information.

About Efficiency:
* The frame transmission time: X = 1.
* Each frame transmission requires 1+v time units.

Suppose the propagation time is negligible, The maximum throughput occurs when all stations are busy, and hence the maximum thoughput is:
$$ρ_{max} = 1/(1 + v)$$


Suppose that the propagation delay is not negligible. If the stations transmit their reservations in the same way as before, but the reservations do not take effect until some fixed number of cycles later.

The basic reservation system can be modified so that **stations can reserve more than one slot per frame transmission per minislot**. Suppose that a minislot can reserve up to k frames. The maximum  cycle size occurs when all stations are busy. Given by Mk + Mv. So the maximum achievable throughput is:
$$ρ_{max} = Mk / (Mk + Mv) = 1/(1 + v/k)$$



##### Question: The effect of the stations number

If M becomes very large, this overhead can become significant. This situation becomes a serious problem when a very large number of stations transmit frames infrequently. **The reservation minislots are incurred every cycle, even though most stations donot transmit**.

Solution: Not allocating  a minislots for each stations, and instead making stations contend for a reservation minislot by using **a random access technique such as Slotted ALOHA**.
$$ρ_{max} = 1/ (1 + v/0.368) = 1/(1 + 2.71v)$$

### 4.2 Polling

Polling System:m stations make turns accessing the medium. **At any given time only one of the stations has the right to transmit into the medium**.

#### 4.2.1 Centralized with a host computer

The system consists of an **outbound line** in which information is transmitted from the host computer to the stations.

The system consists of an **inbound line** in which must shared with M stations. **The inbound line that must be shared with the M stations**. The inbound line is a shared medium that **requires a medium access control to coordinate the transmission from stations to the host computer**.

The host computer acting as a centrol controller that issues control messages to coordinate the transmission from the stations. 
* The central controller sends a **polling message** to a particular station.
* When polled, the stations sends **its inbound frames** and indicates the completion of its transmission through **a go-ahead message**.
* The central controller polls stations in round-robin fashion, or some pre-detected order.

#### 4.2.2 Central controller use radio transmission

Frequency-division duplex(FDD): The central controller may use radio **transmissions in a certain frequency band to transmit out-bound frames**, and stations may **share a different frequency band to transmit inbound frames**.


Time-division duplex(TDD): The variation of FDD, having inbound and outbound transmission share one frequency band. show as 6.21.B

#### 4.2.3 Polling without central controller

Stations have developed **a polling order list under some protocol**. All stations can receive the transmissions from all other stations. After a station is done transmitting, it is responsible for sending a polling message to the next station in the polling order list.

* walk time: elapse while the polling message propagates and is received.
* frame transmission time.

In some system, the transmission time is allowed to transmit as long as it has information in its buffers. In other systems, the transmission time for each station is limited to some maximum duration.

The total walk time τ' is the sum of the walk times in one cycle, represents the minimum time for one round of polling of all the stations. **The walk time between consecutive stations t' is determined by several factors**:
* the propagation time required for a signal to propagate from one station to another.
* the time required for a station to begin transmitting after it has been polled.
* the time required to transmit the polling messages.

The cycle time Tc: the total time that elapses **between the start of two consecutive polls of the same station**. **The cycle time is the sum of the M walk times and the M station transmission times**

Suppose **λ/M frames/second is the average arrival rate of frames for transmission from a station**. E[Nc] is the average number of message arriavls to a station in one cycle time.

$$E[N_c] = (λ/M)E(T_c) $$

The time spent at each station is E[Nc]X + t', where t' is the walk time.

The average cycle time is then M times the average time spent at each station:

$$E[T_c] = M{E[N_c]X + t'} = M{(λ/M)E[T_c]X + t'}$$

$$E[T_c] = Mt'/(1 -  λX) = τ'/(1-ρ)$$

The behavior of the mean cycle time as a function of load:
$$ρ = λX$$

Under light load the cycle time is simply required to poll the full set of stations, and the mean cycle time is approximately τ'.

The walk times required to pass control of the access right to the medium can be viewed as a form of overhead. The **normalized overhead per cycle** is then given by the ratio of **the total walk time to the cycle time**.



### 4.3 Token-Passing Rings

Polling can be implemented in a distributed fashion on networks with a ring topology. Such networks consist of station interfaces that are connected by point-to-point digital transmission lines. 

**An interface in the listen mode** reproduces **each bit that is received from its input to its output after some constant delay**, ideally in the order of one bit time. This delay allow the interface to monitor the passing bit stream for certain patterns.
* address
* the pattern corresponding to a free token.

Idle Token: 11111111

Busy Token: 01111111

When **a free token is received and the attached station has information to send**, the interface changes the passing token to **busy** by **changing a particular bit in the passing stream**.
* In effect, receiving a free token corresponding to  receiving a polling message.
* **The station interface then changes to the transmit mode** where it proceeds to transmit frames of information from the attached station. These frames circulate around the ring and are copied at the destination station interfaces.


##### The relationship between ring circulation time and transmission time

(1) ring circulation time < transmission time

The arriving information corresponds to bits of **the same frame that the station is transmitting**.

(2) ring circulation time > transmission time

More than one frames may be present in the ring at any given time. 

In such cases the arriving information could correspond to bits of a frame from a different station, **so, the station must buffer these bits for later transmission**.


##### Token Ring Type

A frame that is inserted into the ring must be removed.  The frame removal approach:

(1) have the destination station remove the frame from the ring.

(2) allow the frame to travel back to the transmitting station. 
* This approach is preferred because the transmitting station interface can then forward the arriving frame to its attached station, thus providing a form of acknowledgment.

The ring latency is defined as **the number of bits that can be simutaneously in transit around the ring**.

(1) Multitoken approach. The free token is transmitted immediately **after the last bit of the data frame**. It allows several frames to be in transmit in different part of ring.

(2) Single-token Operation. Involves inserting the free token after **the last bit of the busy token is recieved back** and **the last bit of the frame is transmitted**.

* If the frame length greater than the ring latency, the operation is equivalent to multitoken.

(3) Single-frame Operation. The free token is inserted after **the transmitting station has received the last bit of its frame**.

* If the frame length greater than ring latency, this approach corresponds to multitoken operation.

##### About the limitation

The token-ring operation usually also specifies **a limit on the time that a station can transmit**. One approach is to allow a station to transmit  an unlimited number of frames each time a token is received.
* This approach **minimizes the delay experienced by frames**.
* But the time that can elapse between consecutive arrivals of a free token to a station to be unbounded.

A limit is usually placed:
* on the number of frames that can be transmitted each time a token is received.
* on the total time that a station may transmit information into the ring.


##### Token Ring Performance

The introduction of limits on the number of frames that can be transmitted per token affects the maximum achievable throughput.

Suppose that a maximum of one frame can be transmitted per token. 

* Ring Latency: τ' seconds.
* The Ring Latency normalized to the frame transmission time: a'.
* The total propagation delay around the ring: τ
* The number of bit delays in an interface: b.
* The total delay introduced by the M station interfaces: Mb.
* The speed of the transmission lines: R.

$$τ' = τ + (Mb)/R$$
$$a' = τ' / X$$

**For system using multitoken operation**, the total time taken to transmit the frames from the M stations is MX + τ'. Because MX of this time is spent transmitting information, **the maximum normalized throughput** is then

$$ρ_{max} = MX / (MX + τ') = 1/(1 + τ'/MX) = 1/(1 + a'/M)$$

**For Single-token system**, we can see that the effective frame duration is the maximum of X and τ'. Therefore, the maximum normalized throughput is:

$$ρ_{max} = MX/(Mmax(X,τ') + τ') = 1/(max{1, a'}+τ'/MX)$$
$$ρ_{max} = 1/(max{1, a'}+a'/M)$$

a' < 1: the single-token operation is the same maximum throughput as multitoken operation.

a' > 1: the maximum throughput is less than that of multitoken operation.

**For single-frame system**, the effective frame transmission time is always X+τ'. Therefore, the maximum throughput is:

$$ρ_{max} = MX/(M(X+ τ') +τ') = 1/(1 + a'(1+1/M))$$

The maximum throughput for single-frame operation is the lowest of the three approaches.

### 4.4 Comparision of Scheduling and Random Access Systems

(1) In scheduling reservation interval, polling and token transmission constitue overhead and in random access systems collisions are overhead.

(2) In sheduling overhead is proportional to the number of stations, in random access systems overhead is proportional to the load.

(3) Scheduling systems are more efficient under heavy load and random access system are more efficient under light load.

(4) Scheduling systems provide orderly access to the medium, random access systems show great delay variability in access to the medium.




















































 