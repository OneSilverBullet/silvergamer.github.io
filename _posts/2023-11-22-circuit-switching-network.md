---
layout: post
title:  "[Network]Circuit Switching Networks"
date:   2023-11-22
excerpt: "The mechanism of Switching."
tag:
- Network
comments: false
blog: true
feature: https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/blogHead/directX12partI.jpg
---


## Overview

**Circuit-switching networks** provide dedicated circuits that enable the flow of information between users.

example: telephone network, which provides **64 kbps circuits** for the transfer of voice signals between users.


The purpose of a transport network is to **provide high bandwidth circuits**, that is, large pipes typically in the range of 50 Mbps to 10 Gbps, between clients such as **telephone switches and large routers**.

* Transport networks provide the backbone of large pipes that interconnect telephone switches to form the telephone network. 
* Transport networks also form the backbone that interconnects large routers to form the Internet. 
* Transport networks can also be used to provide the backbone for large corporate networks.

## 1. Multiplexing

**Multiplexing** in the physical layer involves the **sharing of transmission systems by several connections or information flows**.

The capacity of a transmission system is given by its bandwidth, which is measured:
* **Hertz** for **analog transmission systems**
* **bits/second** for **digital transmission systems**

Multiplexing is desirable when **the bandwidth of individual connections** is much **smaller** than **the bandwidth of the available transmission system**.



<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_4multiplexing.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_4multiplexing.png" align="center"></a>
    <figcaption>Multiplexing.</figcaption>
</figure>


This approach a quickly becomes unwieldy, inefficient and expensive as the number of users increases.
Multiplexing is introduced when **a transmission line has sufficient bandwidth to carry several connections**.

### 1.1 Frequency-Division Multiplexing

In **frequency-division multiplexing(FDM)**, the bandwidth is divided into a number of **frequency slots**, each of which can **accommodate the signal of an individual connection**.

* The multiplexer assigns a frequency slot to each connection and uses modulation to place the signal of the connection in the appropriate slot.

* The **combined signal** is transmitted, and the demultiplexer recovers the signals corresponding to each connection and delivers the signal to the appropriate user. 
 


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_4circuitswitching.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_4circuitswitching.png" align="center"></a>
    <figcaption>Frequency division multiplexing.</figcaption>
</figure>

 Reducing the number of wires that need to be handled **reduces the overall cost of the system**.

The basic analog multiplexer:

* The **basic analog multiplexer** combines 12 voice channels in one line. 

* Each voice signal occupies about 3.4 kHz but the channels are **assigned 4 kHz of bandwidth to provide guard bands** between channels. 

A hierarchy of analog multiplexers has been defined:

(1) The multiplexer modulates each voice signal so that it **occupies a 4 kHz slot in the band between 60 and 108 kHz**.  The combined signal is called a **group**.

(2) a **supergroup (that carries 60 voice signals)** is formed by **multiplexing five groups**, each of bandwidth 48 kHz, into the frequency band from 312 to 552 kHz. 
* Note that for the purposes of multiplexing, **each group is treated as an individual signal**. 

(3) Ten **supergroups** can then be multiplexed to form a **mastergroup** of 600 voice signals that occupies the band 564 to 3084 kHz.

* Stations in AM, FM, and television are assigned frequency bands of 10 kHz, 200 kHz, and 6 MHz, respectively.  
* FDM is also used in cellular telephony where a pool of frequency slots, typically of 25 to 30 kHz each, are shared by the users within a geographic cell. 
* **Each user is assigned a frequency slot for each direction**. Note that in FDM the user information can be in **analog or digital form** and that the information from all the users flows simultaneously.

### 1.2 Time-Division Multiplexing


In time-division multiplexing (TDM), the multiplexed connections share a single **high-speed digital transmission line**.

Each connection produces **a digital information flow** that is then inserted into the **high-speed line using temporal interleaving**.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_4timediv.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_4timediv.png" align="center"></a>
    <figcaption> Time-division multiplexing.</figcaption>
</figure>

Each connection generates a signal that produces one unit of information every 3T seconds. This unit of information could be **a bit, a byte, or a fixed-size block of bits**.


**A digital telephone speech signal** is obtained by **sampling a speech waveform 8000 times/second** and by representing **each sample with eight bits**. 
* The T-1 system uses **a transmission frame** that consists of **24 slots of eight bits each**. Each slot carries one PCM sample for a single connection. 
* The beginning of each frame is indicated by a single “framing bit.”

The resulting transmission line has a speed of:

$$(1+24*8)bits/frame*8000frames/second = 1.544Mbps$$

 The value of the framing bit alternates in value, so the receiver can determine whether a target bit is the framing bit if the target bit follows the alternation.

 The growth of telephone network traffic and the advances in digital transmission led to the development of a standard digital multiplexing hierarchy.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_4T1.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_4T1.png" align="center"></a>
    <figcaption> T1 system.</figcaption>
</figure>

In T1 carrier system, **the growth of telephone network traffic and the advances in digital transmission** led to the development of a standard **digital multiplexing hierarchy**.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_4ha.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_4ha.png" align="center"></a>
    <figcaption> Basic digital hierarchies.</figcaption>
</figure>

In north america and japan:

(1) the **Digital Signal 1(DS1)**:
* corresponds to the output of a T-1 multiplexer
* the **basic building block**.

(2) The **DS2 signal**
* obtained by combining four DS1 signals 
* adding 136 kilobits of synchronization information.

(3) The **DS3 Signal**
* combining 7 DS2 signals 
* adding 552 kilobits of synchronization information.
* extensive use in providing high-speed communications to **large users such as corporations**.

In Europe:

(1) CEPT-1 (E-1):
* consisting of thirty-two 64-kilobit channels forms the basic building block.
* 30 of the 32 channels are used for voice channels
*  one of the other channels is used for **signaling**, and the other channel is used for **frame alignment and link maintenance**.

(2) The second, third, and fourth levels of the hierarchy are obtained by grouping **four of the signals in the lower level**.



#### Time-Division Multiplexing Problem

The operation of **a time-division multiplexer** involves tricky problems with **the synchronization of the input streams**.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_4basicHierarchies.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_4basicHierarchies.png" align="center"></a>
    <figcaption> Slower Bit Problem.</figcaption>
</figure>

 Two streams each with a nominal rate of **one bit every T seconds**, that are combined into **a stream that sends two bits every T seconds**. 

Problem: if one of the streams is slightly slower than 1/T bps
* **Bit Slip**: Every T seconds, the multiplexer expects each input to provide a one-bit input; **at some point the slow input will fail to produce its input bit**. Thus the slow stream will alternate between being late, undergoing a bit slip, and then being early.
 
Result: Because bits are arriving faster than they can be sent out, bits will accumulate at the multiplexer and eventually be dropped.

Solution: To deal with the preceding synchronization problems, time-division multiplexers
have traditionally been designed to **operate at a speed slightly higher than the combined speed of the inputs**.

* The frame structure of the multiplexer output signal contains bits that are used to indicate to **the receiving multiplexer that a slip has occurred**.
* extra bits to deal with slips implies that the frame structure of the output stream is
**not exactly synchronized to the frame structure of all the input streams**.

### 1.3 Wavelength-Division Multiplexing

The information carried by **a single optical fiber** can be increased through the use of
**wavelength-division multiplexing (WDM)**.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_4wavelength.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_4wavelength.png" align="center"></a>
    <figcaption> Wavelength-division multiplexing.</figcaption>
</figure>

WDM can be viewed as **an optical-domain version** of FDM in which **multiple information signals modulate optical signals** at different optical wavelengths (colors).

The attraction of WDM is that a huge increase in available bandwidth is obtained **without**
the huge investment **associated with deploying additional optical fiber**.
* Early WDM systems combined 16 wavelengths at 2.5 Gbps to provide an aggregate signal of 16×2.5 Gbps = 40 Gbps.
* WDM systems with 32 wavelengths at 10 Gbps have a total bit rate of 320 Gbps and are widely deployed.
* Systems that can carry 160 wavelengths at 10 Gbps are also available and achieve an amazing bit rate of 1.6 terabits/second.


The additional bandwidth can be used to carry more traffic and can also provide the **additional protection bandwidth** required by **self-healing network topologies**.

Early **WDM systems** differ in **substantial ways** from **electronic FDM systems**. In
FDM the channel slots are separated by guard bands that are narrow relative to the
bandwidth of each channel slot. 

These narrow guard bands are possible because the devices for carrying out the required modulation, filtering, and demodulation are available. Narrow spacing is not the case for WDM systems. Consequently, the spacing between wavelengths in WDM systems tends to be large compared to the bandwidth of the information carried by each wavelength.

## 2. SONET

Question: The deregulation of telecommunications in the United States led to a situation in which the long-distance carriers were expected to provide the interconnection between local telephone service providers.

Solution: To meet the urgent need for **standards to interconnect optical transmission systems**:
* the **Synchronous Optical Network (SONET) standard** was developed in **North America**.
* The CCITT later developed a corresponding set of standards called **Synchronous Digital Hierarchy (SDH)**.

**Current backbone networks** in **North America are based on SONET**, while in **Europe and many other parts of the world they are based on SDH systems**.

The SONET/SDH standards introduced several significant concepts for transmission networks.
* S: synchronous format that greatly simplifies the handling of lower-level digital signals and reduces the overall cost of multiplexing in the network.
* **An additional feature of the SONET standards** is the **incorporation of overhead bytes in the frame structure for use in monitoring the signal quality, detecting faults, and signaling among SONET equipment to orchestrate rapid recovery from faults**.

### 2.1 SONET Multiplexing

#### STS

The **synchronous transport signal level-1 (STS-1)** as a building block to **extend the digital transmission hierarchy** into the multigigabit/second range.

A **higher-level STS-n electrical signal** in the hierarchy is obtained through the **interleaving** of bytes from the lower-level component signals. 
* Each **STS-n electrical signal** has a **corresponding optical carrier level-n (OC-n) signal** that is obtained by modulating a laser source. 
* The bit formats of **STS-n and OC-n signals** are the same except for **the use of scrambling in the optical signal**.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_4STS_synchronous.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_4STS_synchronous.png" align="center"></a>
    <figcaption> SONET digital hierarchy.</figcaption>
</figure>

**Notice that the rate of an STS-n signal is simply n times the rate of an STS-1 signal.**


#### SDH

The SDH standard refers to **synchronous transfer module-n (STM-n) signals**. The SDH STM-1 has a bit rate of **155.52 Mbps** and is equivalent to **the SONET STS-3 signal**.

* The STS-1 signal accommodates the **DS3** signal from the existing digital transmission hierarchy in North America. 
* The STM-1 signal accommodates the **CEPT-4** signal in the CCITT digital hierarchy. 
* The **STS-48/STM-16 signal** is widely deployed in **the backbone of modern communication networks**.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_4SonetMultiplexing.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_4SonetMultiplexing.png" align="center"></a>
    <figcaption> Sonet Multiplexing.</figcaption>
</figure>

SONET uses **a frame structure that has the same 8 kHz repetition rate** as traditional telephone TDM systems.

SONET uses the term **tributary** to refer to the **component streams** that are **multiplexed** together.


* A **slow-speed mapping function** allows DS1, DS2, and CEPT-1 signals to be combined into an STS-1 signal. 
* A DS3 signal can be mapped into an STS-1 signal
* A CEPT-4 signal can be mapped into an STS-3 signal. 


**Mappings** have also been defined for **mapping streams of packet information into SONET**.

* **ATM** streams can be mapped into an **STS-3c signal**
* **Packet-over-SONET (POS)** allows packet streams also to be mapped into **STS-3c signals**.

A SONET multiplexer can then combine STS input signals into **a higher-order STS-n signal**.



### 2.2 SONET Frame Structure

A SONET system is divided into three layers: 
* sections: refers to **the span of fiber** between two adjacent devices, such as two regenerators.
    * deals with **the transmission of an STS-n signal** across the physical medium.
* lines: refers to the span between two adjacent multiplexers and therefore in general encompasses several sections.
    * deals with **the transport of an aggregate multiplexed stream** and the associated overhead.
* paths: refers to the span between the two **SONET terminals** at the endpoints of the system and in general encompasses one or more lines.
    * User equipment, for example, large routers, can act as SONET terminals and be connected by SONET paths.

In general **the bit rates of the SONET signals** increase as we move from **terminal equipment** on to **multiplexers deeper** in the network.
* The reason is that a typical information flow begins at some bit rate at the edge of the network, which is then combined into higher-level aggregate flows inside the network, and finally delivered back at the original lower bit rate at the outside edge of the network.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_4sonetlayer.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_4sonetlayer.png" align="center"></a>
    <figcaption> Sonet Layers.</figcaption>
</figure>


4.11a: **The multiplexers associated with the path level** typically handle signals **lower** in the hierarchy than **the multiplexers in the line level**. 

4.11b: all SONET equipment implement the optical and section functions
* the section layer deals with **the signals in their electrical form**.
* the optical layer deals with **the transmission of optical pulses**.

About the functions each equipment focus on:

* It can be seen that every regenerator **involves converting the optical signal to electrical form** to **carry out the regeneration function** and then **back to optical form**.
* Line functions are found only in **the multiplexers and end terminal equipment**. 
* The path function occurs only at **the end terminal equipment**.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_4STS1frameFormat.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_4STS1frameFormat.png" align="center"></a>
    <figcaption> Sonet STS-1 frame format.</figcaption>
</figure>

**Adjacent multiplexers exchange information** using frames.

* The **structure of the SONET STS-1 frame** that is defined at **the line level**.
* A frame consisting of a **rectangular** array of bytes arranged in **9 rows by 90 columns** is **repeated 8000 times a second**.

$$8*9*90*8000=51.84Mbps$$

The first three columns of the array are allocated to **section** and **line overhead**.
* The **section overhead** is interpreted and modified at every section termination and is used to **provide framing, error monitoring, and other section-related management functions**.
    * A1 and A2 of the section overhead is used to  indicate the beginning of a frame.
    * B1 carries **parity checks of the transmitted signal** and is used to **monitor the bit error rate in a section**.
    * last three bytes of the section overhead are used to provide **a data communications channel between regenerators that can be used to exchange alarms, control, monitoring and other administrative messages**.
* The line overhead is interpreted and modified at **every line termination** and is used to provide **synchronization and multiplexing, performance monitoring, line maintenance, as well as protection-switching capability in case of faults**. 
    * H1, H2, H3: play a crucial role in how **multiplexing is carried out**.
    * B2: is used to **monitor the bit error rate in a line**.
    * K1, K2: **trigger recovery procedures in case of faults**.
* The remaining 87 columns of the frame constitute the information payload that **carries the path layer or “user” information**.

The bit rate of the information payload is:

$$8*9*87*8000=50.122Mbps$$

(1) The information payload includes **one column of path overhead information** but the column is not necessarily aligned to the frame.

(2) The **path overhead** includes bytes for **monitoring the performance of a path** as well as for **indicating the content and status of the end-to-end transfer**.


#### How the end-to-end user information is organized at the path level?

The SONET terminal equipment takes **the user data and the path overhead** and maps
it into **a synchronous payload envelope (SPE)**, which consists of **a byte array** of nine rows by 87 columns.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_4sp.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_4sp.png" align="center"></a>
    <figcaption> The synchronous payload envelope can span two consecutive frames. </figcaption>
</figure>

The **path overhead** uses **the first column of this array**. This SPE is then inserted into the STS-1 frame. 
* The SPE is **not necessarily aligned to** the information payload of an STS-1 frame.
* The **first two bytes of the line overhead** are used as **a pointer that indicates the byte within the information payload where the SPE begins**.
* the SPE can be spread over two consecutive frames.

The use of the pointer makes it possible to **extract a tributary signal** from the **multiplexed signal**. This feature gives **SONET multiplexers** an **add-drop capability**, which means that **individual tributaries can be dropped** and **individual tributaries can be added without having to demultiplex the entire signal**.

The **pointer structure** consisting of **the H1, H2, and H3 bytes**, maintains **synchronization of frames and SPEs** in situations where their clock frequencies differ slightly.

(1) Questions: If **the payload stream is faster** than the frame rate, then a buffer is required to hold payload bits as the frame stream falls behind the payload stream. To allow the frame to catch up, an extra SPE byte is transmitted in a frame from time to time.

Solution: Negative Byte Stuffing
* This extra byte, which is carried by H3 within the line overhead, clears the backlog that has built up. 
* Whenever this byte is inserted, the pointer is moved forward by one byte to indicate that the **SPE starting point has been moved one byte forward**. 

(2) Question: When the payload stream is slower than the frame stream

Solution: Positive Byte Stuffing
* **The number of SPE bytes transmitted in a frame** needs to be reduced by one byte from time to time. 
* This correction is done by **stuffing an SPE byte with dummy information** and then **adjusting the pointer to indicate that the SPE now starts one byte later**.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_41.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_41.png" align="center"></a>
    <figcaption> Positive byte stuffing and negative byte stuffing. </figcaption>
</figure>

SPE rate >  STS-1 rate, negative byte stuffing. H3 carries a byte of information, decrement the pointer by one.

SPE rate < STS-1 rate, positive byte stuffing. byte next to H3 carries dummy information, increment the pointer by one.

(3) Question: how n STS-1 signals are multiplexed into an STS-n signal?

Solution: **Each incoming STS-1 signal** is first synchronized to the local STS-1 clock of the multiplexer as follows.

* The **section and line overhead** of the incoming STS-1 signal are terminated,
and its payload (SPE) is mapped into a new STS-1 frame that is **synchronized to the local clock**.
* The pointer in the new STS-1 frame is adjusted as necessary, and the mapping is done **on the fly**.
* This procedure ensures that **all the incoming STS-1 frames** are mapped into STS-1 frames that are **synchronized with respect to each other**. 
* The STS-n frame is produced by interleaving the bytes of the n synchronized STS-1 frames, in effect **producing a frame that has nine rows, 3n section and line overhead columns, and 87n payload columns**. 
* To multiplex k STS-n signals into an STS-kn signal, the incoming signals are first de-interleaved into STS-1 signals and then the above procedure is applied to all kn STS-1 signals.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_42.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_42.png" align="center"></a>
    <figcaption> Synchronous multiplexing in SONET. </figcaption>
</figure>


A SONET STS-1 signal can be divided into **virtual tributary signals** that accommodate lower-bit-rate streams. 

In each SPE, 84 columns are set aside and divided into **seven groups of 12 columns**. Each group constitutes a virtual tributary and has a bit rate of
12 × 9 × 8 × 8000 = 6.912 Mbps. 

Alternatively, each virtual tributary can be viewed as 12 × 9 = 108 voice channels. T

hus mappings have been developed so that **a virtual tributary can accommodate four T-1 carrier signals** (4 × 24 = 96 < 108), or three CEPT-1 signals (3 × 32 = 96 < 108). 

The SPE can then handle **any mix of T-1 and CEPT-1 signals that can be accommodated in its virtual tributaries**. 

In particular **the SPE can handle a maximum of 7 × 4 = 28 T-1 carrier signals or 3 × 7 = 21 CEPT-1 signals**. A mapping has also been developed so that a single SPE signal can handle one DS3 signal.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_43.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_43.png" align="center"></a>
    <figcaption> The virtual group. </figcaption>
</figure>

### 2.3 Transport Networks

A **transport network** provides **high bit rate connections** to clients at different locations much like a telephone network provides voice circuits to users.

The connections provided by a transport network can form the **backbone of multiple**, **independent networks**.

SONET produced significant reduction in cost by enabling **add-drop multiplexers**
(ADM) that can insert and extract tributary streams without disturbing tributary streams
that are in transit as shown in Figure 4.17b. The overall cost of the network can then be
reduced by replacing back-to-back multiplexers with SONET ADMs.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_44.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_44.png" align="center"></a>
    <figcaption> The SONET add-drop multiplexing. </figcaption>
</figure>


## 4.4  CIRCUIT SWITCHES

A network is frequently represented as a cloud that connects multiple users(as 4.33a). 

A **circuit-switched network** is **a generalization of a physical cable** in the sense that it provides **connectivity that allows information to flow between inputs and outputs** to the network.


The function of a circuit switch is to **transfer the signal that arrives at a given input to an appropriate output**. The interconnection of a sequence of **transmission links** and **circuit switches** enables the flow of information between inputs and outputs in the network.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_433.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_433.png" align="center"></a>
    <figcaption> A network consists of links and switches. </figcaption>
</figure>

### 4.4.1 Space-Division Switches

Space-division switches: they provide a **separate physical connection** between inputs and outputs so **the different signals are separated in space**.

#### (1) Crossbar Switch

The crossbar switch consists of an N × N array of **crosspoints** that can connect any input to any available output.
* When a request comes in from an
incoming line for an outgoing line, **the corresponding crosspoint is closed to enable information to flow from the input to the output**.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_434.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_434.png" align="center"></a>
    <figcaption> Crossbar switch. </figcaption>
</figure>

The **crossbar switch** is said to be a
**nonblocking switch**; in other words, connection requests are never denied because of **lack of connectivity resources**, that is, **crosspoints**. Connection requests are denied only
when the requested outgoing line is **already engaged in another connection**.


The complexity of the crossbar switch as measured by the number of crosspoints
is $N^2$.


#### (2) Mulistage Switches

a multistage switch that consists of **three stages** of smaller space division switches.
* The **N inputs** are grouped into **N/n groups of n input lines.**
* Each group of **n input lines** enters a small switch in the first stage that consists of an n×k array
of crosspoints.
* Each **input switch** has one line connecting it to each of **k intermediate stage N/n × N/n switches**.
* Each intermediate switch in turn has one line connecting it to each of **the N/n switches in the third stage**.
* The latter switches are k × n.

**In effect each set of n input lines shares k possible paths to any one of the switches at the last stage**.
* the first path goes through the first intermediate switch
* the second path goes through the second intermediate switch

The resulting multistage
switch is **not necessarily nonblocking**. For example, if k < n, then as soon as a switch
in the first stage has k connections, all other connections will be blocked.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_435.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_435.png" align="center"></a>
    <figcaption> Multistage switch (with three smaller space-division switches). </figcaption>
</figure>

#### (3) CLOS NONBLOCKING SWITCHING FABRIC

The set of routes that **maximize the number of intermediate switches** already in use by the given input and output groups is shown in the figure. That is, **each existing connection uses a different intermediate switch**. Therefore, the maximum number of intermediate switches not available to **connect the desired input to the desired output is 2(n − 1)**. 
* Now suppose that k = 2n − 1; then k paths are available from any input group to any output group.


Thus **the multistage switch with k = 2n − 1** is **nonblocking**.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_436.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_436.png" align="center"></a>
    <figcaption> A multistage switch is nonblocking if k = 2n − 1 </figcaption>
</figure>

The number of crosspoints required in a three-stage switch is the sum of the
following components:

* N/n input switches × nk crosspoints/input switch.
* k intermediate switches × (N/n)2 crosspoints/intermediate switch.
* N/n output switches × nk crosspoints/output switch.

The total number of crosspoints is $2Nk + k(N/n)^2$.

The number of
crosspoints required to make the switch nonblocking is $2N(2n −1)+(2n −1)(N/n)^2$.

**The number of crosspoints can be minimized through the choice of group size n**. 
* By differentiating the above expression with respect to n, we find that the number of crosspoints is minimized if $n ≈ (N/2)^{1/2}$. The minimum number of crosspoints is then
$4N((2N)^{1/2} − 1)$.

We then see that the minimum number of crosspoints using a Clos nonblocking three-stage switch grows at a rate proportional to $N^{1.5}$.


*When k < 2n − 1, there is a nonzero probability that a connection request will be blocked*.


### 4.4.2 Time Division Switches

The **time-slot interchange (TSI)** technique replaces the crosspoints in a **space switch with the reading and writing of a slot into a memory**. 


The interchange technique: 
* The **octets in each incoming frame** are written into a **register**. 
* The call setup procedure has **set a permutation table that controls the order in which the contents of the register are read out**. 
* Thus the outgoing frame begins by reading the contents of slot 23, followed by slot 24, and so on until
slots 1 and 2 are read, as shown in the figure. This procedure can connect any input to any available output.

Frames come in at a **rate of 8000 times** a second and **the time-slot interchange** requires **one memory write and one memory read** operation per
slot, the maximum number of slots per frame that can be handled is

$$MaximumNumOfSlots=125μsec/(2*MemoryCycleTime)$$


The development of the TSI technique was **crucial in completing the digitization of the telephone network**.

The introduction of TSI in digital time-division switches led to significant reductions in cost and to improvements in performance by **obviating the need to convert back to analog form**.



<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_437.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_437.png" align="center"></a>
    <figcaption> Time-slot interchange technique </figcaption>
</figure>

#### (1) Time-Space-Time Switches

a **hybrid switch design** in which **TSI switches are used at the input and output stages** and **a crossbar space switch is used at the intermediate stage**. These switches are called **time-space-time switches**.
 
The design approach is to establish an **exact correspondence** between the input lines in a space-division switch in the first stage and time slots in a TSI switch.

Each input line to the switch corresponds to a slot, so the **TSI switch has input frames of size n slots**.

Similarly, the **output frame from the TSI switch** has **k slots**. Thus the operation of the TSI switch involves taking the n slots from the incoming frame and reading them out in a frame of size k, according to some preset permutation table.

Note that for the system to operate in synchronous fashion, the **transmission time of an input frame** must be equal to the **transmission time of an output frame**.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_438.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_438.png" align="center"></a>
    <figcaption> Hybrid switches </figcaption>
</figure>

The **flow of slots** between **the switches in the first stage** and **the switches in the intermediate stage**.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_439.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_439.png" align="center"></a>
    <figcaption> Flow of slots between switches in a hybrid switch. </figcaption>
</figure>

(1) what happens as **the first slot in a frame** comes out of **the first stage**

This first slot corresponds to the first output line
out of each of the first stage switches.

The first line out of each first stage switch is connected to the first intermediate switch.

The first slot in each intermediate frame will be directed to intermediate switch 1.

this switch is a crossbar switch, and so it will transfer the N/n input slots into N/n output slots according to the crosspoint settings. 

Note that all the other intermediate switches are idle during the first time slot.

(2) what happens with the second slot in a frame

These slots are now directed to crossbar switch 2, and all other intermediate switches are idle.

It thus becomes apparent that **only one of the crossbar switches is active** during any given time slot. 

This situation makes it possible to replace the k intermediate crossbar switches with a single crossbar switch that is time-shared among the k slots in a frame.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_440.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_440.png" align="center"></a>
    <figcaption> Time-space-time switches </figcaption>
</figure>

To **replace the k intermediate original crossbar switches**, the **time-shared crossbar switch** must be reconfigured to the **interconnection pattern of the corresponding original switch** at every time slot. This approach to sharing a space switch is called **time-division switching**.

#### (2) Example: 4X4 Time-Space-Time Switch

A simple 4 × 4 switch example that is configured for the connection pattern that takes inputs (A, B, C, D) and outputs (C, A, D, B). 

Part (a) of the figure shows **a configuration of a three-stage space switch** that implements this **permutation**.

Part (b) shows the **TST implementation** where **the inputs arrive in frames of size 2** that are mapped into **frames of size 3** by **the first-stage TSI switches**.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_441.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_441.png" align="center"></a>
    <figcaption> Example of a time-space-time switch </figcaption>
</figure>

Note that parts (a) and (b) have the same configurations.

#### (3) Example: Time-Space-Time switch design

Question: consider the design of a nonblocking 4096 × 4096 time-space-time switch that has input frames with 128 slots. 

Solution:
* Because N = 4096 and n = 128, we see that N/n = 32 input TSI switches are required in the first stage. 
* The nonblocking requirement implies that the frames between the input stage and the intermediate stage must be of size k = 2n − 1 = 255 slots. 
* The internal speed of the system is then approximately double that of the input lines. 
* The time-shared crossbar switch is of size 32 × 32.

 The resulting switch can be seen to be quite compact.



## 4.7 Traffic and Overload control in Telephone Networks

The **dynamic aspects of multiplexing** the information flows **from various users into shared high-speed digital transmission lines**.

### 4.7.1 Concentration

**Concentration** addresses the situation where **a large number of users** alternate
between periods when they need connections and periods when they are idle.
* Concentration involves **the dynamic sharing of a number of communication channels** among a larger community of users.




<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_455.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_455.png" align="center"></a>
    <figcaption> Concentration. </figcaption>
</figure>

numerous users at a given site, **each connected to a local concentrator**,
**share expensive trunks provided by a high-speed digital transmission line** to connect to
another location.

* When a user on one end wishes to communicate with a user at the other end, the concentrator
assigns **a communication line or trunk** for **the duration of the call**. 
* When the call is completed, **the transmission line is returned to the pool** that is available to meet new connection requests.
* Note that **signaling between concentrators** is required to set up and terminate each call.

 We say that a connection request is **blocked** when **no trunks are available**. 
 * The objective here is to minimize the number of trunks, while **keeping the probability of blocking to some specified level**.



<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_456.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_456.png" align="center"></a>
    <figcaption> Number of trunks in use as a function of time. </figcaption>
</figure>


the statistical behavior of the users can be characterized. In particular it has been
found that the **users as a group** make requests for connections according to **a Poisson process with connection request or arrival rate λ calls/second**. A Poisson process is
characterized by the following two properties:
* In a very small time interval of duration △ seconds, only two things can happen:
    * There is a request for one call, with probability λ△
    * there are no requests for calls, with probability 1 − λ△.
* The arrivals of connection requests **in different intervals** are **statistically independent**.

**The holding time**： The time that a user maintains a connection.
* the holding time X is a random variable.
*  The average holding time E[X] can be viewed as **the amount of “work” that the transmission system has to do for a typical user**.

The **offered load a** is defined as **the total rate at which work is offered by the community of users to the multiplexing system**, as measured in Erlangs

$$a = λ × E[X](Erlangs)$$

One Erlang corresponds to **an offered load that would occupy a single trunk 100 percent of the time**.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_457.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_457.png" align="center"></a>
    <figcaption> Blocking probability for offered load versus number of trunks: blocking probability descreases with the number of trunks. </figcaption>
</figure>

The **blocking probability $P_b$** for a system with **c trunks** and **offered load a** is given by the Erlang B formula:

$$P_b = B(c, a) = (a^c/c!) /  (\sum_{k=0}^{c} a^k / k!)$$

where $k!=1*2*3*...*k$. The figure shows the blocking probability for various offered loads as **the number of trunks c is increased**.
* As expected, the blocking probability decreases with the number of trunks.

 **A 1% blocking probability is typical in the design of trunk systems**.

 This result shows that the system becomes **more efficient as the size of the system increases, in terms of offered load**. The efficiency can be measured by **trunk utilization** that is defined as the average number of trunks in use divided by the total number of trunks.

$$Utilization=λ(1 − P_b)E[X]/c = (1 − P_b)a/c$$


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_4t2.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_4t2.png" align="center"></a>
    <figcaption> The trunk utilization. </figcaption>
</figure>

 The **improvement in system performance** that results from **aggregating traffic flow** is called **multiplexing gain**.


### 4.7.2 Routing Control

**Routing control** refers to **the procedures for assigning paths in a network to connections**.  Clearly, connections should follow **the most direct route**, since this approach uses **the fewest network resources**.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_458.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_458.png" align="center"></a>
    <figcaption> Hierarchical Routing Control. </figcaption>
</figure>

Economic considerations lead to an approach that **provides direct trunks** between **switches** that have **large traffic flows** between them and that **provide indirect paths through tandem switches for smaller flows**.
* Cost: Each pair of switches requires 18 long distance trunks to handle the 10 Erlangs of traffic at 1% blocking probability. Thus the approach in Figure 4.58a requires 9 × 18 = 162 trunks.

**A hierarchical approach** to routing is desirable when the volume of traffic between
switches is **small**. This approach **entails aggregating traffic flows onto paths that are shared by multiple switches**.
* Cost: **Concentrating the traffic flows through the tandems** reduces to 106 the number of trunks required to handle the combined 90 Erlangs of traffic. 


The second approach does require the use of **local trunks to the tandem switch**, and so the choice depends on **the relative costs of local and long-distance trunks**.


Question: The higher efficiency implies that a smaller
number of spare circuits is required to meet the 1% blocking probability. However, **the smaller number of spare circuits** makes the system **more sensitive** to **traffic overload conditions**. the blocking probability for large systems is **quite sensitive to traffic overloads**, and therefore the selection of the trunk groups must **provide a margin for some percentage of overload**.

Solution: 
* A request for a connection between the two switches first attempts to engage a trunk in the direct path. 
* If **no trunk is available in the direct path**, then an attempt is made to **secure an alternative path** through **the tandem switch**.

Note that because **only 10% of the traffic between the switches attempts the alternative route**, a **10% blocking probability on the alternative path** is sufficient to
bring the overall blocking probability to 1%.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_459.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_459.png" align="center"></a>
    <figcaption> Alternative Routing. </figcaption>
</figure>

It should be noted also that the Erlang formula cannot be applied directly in the calculation of blocking probability on the alternative route. The reason is that the requests for routes to the tandem switch arrive **only during periods when the high-usage route is unavailable**.



<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/460.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/460.png" align="center"></a>
    <figcaption> Typical routing scenario. </figcaption>
</figure>

a more realistic scenario where **the tandem switch handles overflow traffic from some high-usage trunk groups and direct traffic between switches** that have small volumes of traffic between them.

The traffic between switches A and D must be provided with a 1% blocking probability, while a
10% blocking probability is sufficient for the other pairs of switches. To achieve this
blocking probability **the traffic from A to D must receive a certain degree of preferential access to the trunks between the tandem switches**.  


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_461.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_461.png" align="center"></a>
    <figcaption> Dynamic nonhierarchical routing. </figcaption>
</figure>


Traffic flows vary according to the time of day, the day of the week, and even the
time of year. The ability to **determine the state of network links and switches provides an opportunity to assign routes in more dynamic fashion**.

time/day differences between the East Coast and the West Coast in North America allow the network
resources at one coast to **provide alternative routes for traffic** on the other coast during
certain times of the day. **Dynamic nonhierarchical routing (DNHR)** is an example of
this type of dynamic approach to routing calls.

A certain number of tandem switches is capable of **providing a two-hop alternative route**. The order in which tandem switches are attempted as alternative routes is determined dynamically according to the state of the network.


### 4.7.3 Overload Controls

Traffic and routing control are concerned with the handling of traffic flows during
normal predictable network conditions. **Overload Control** addresses **the handling of traffic flows** during unexpected or unusual conditions, such as occur **during holidays**.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_462.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_462.png" align="center"></a>
    <figcaption> Traffic overload. </figcaption>
</figure>

As shown in figure, Overload conditions result in traffic levels that the network equipment has not been provisioned for and if not handled properly can result in **a degradation in the level of service offered to all network customers**.

As the **offered traffic** approaches **network capacity**, the **carried traffic** may begin to fall. **network resources become scarce**, many call attempts manage to seize only some of
the resources they need and **ultimately end up uncompleted**.


One purpose of overload control is to ensure that **a maximum number of calls** are completed so that **the carried load can approach the network capacity under overload conditions**.


To identify overload conditions:
*  the **traffic loads** at various links and switches need to be measured and tracked
    * The increased traffic load indicates a problem condition but is not sufficient to identify the problem.
* the **success ratio of call attempts to a given destination** also needs to be monitored.
    *  The answer/bid ratio provides the information that identifies switch A as the location of the problem.

The **traffic load measurement** in combination with the **answer/bid ratio** is useful in diagnosing fault conditions.


Once an overload condition has been identified, several types of actions can be taken：
* One type of overload control addresses problems by **allocating additional resources**
    *  Many transmission systems include **backup redundant capacity** that can be activated in response to failures
* Dynamic alternative routing provides another approach for allocating resources between areas experiencing high levels of traffic.

Question: Certain overload conditions cannot be addressed by **allocation of additional resources**. 

Solution: The overload controls in this case act to **maximize the efficiency with which the available resources are utilized**.

Procedure: 
* In the case of networkwide congestion the routing procedures could be modified so that all call attempts, if accepted, are met using direct routes. 
* Alternative routes are disallowed because they require **more resources to complete calls**.

As a result, the **traffic carried by the network is maximized**.


Question:  a certain area experiences extreme levels of inbound and outbound traffic as may result from the occurrence of a natural disaster

Solution:
* One approach involves **allowing only outbound traffic to seize the available trunks**. This
approach relieves the switches in the affected area from having to process incoming requests for calls while allowing a maximum of outbound calls to be completed.
*  A complementary control involves **code blocking**, where distant switches are instructed to **block calls destined for the affected area**.
* A less extreme measure is to **pace the rate at which call requests from distant switches to the affected area** are allowed to proceed.

## 4.8 Cellular Tellephone Network

In cellular telephony the telephone number **specifies a specific subscriber’s mobile station** (telephone). Much of the complexity in cellular telephony results from
the **need to track the location of the mobile station**.


Radio telephony: an analog voice signal was modulated onto a radio wave.
* **Radio transmission** makes communications possible to mobile users

Because electromagnetic waves propagate over a wide geographical area, they are ideally suited for a **radio broadcasting service** where **information from a source or station** is **transmitted to a community of receivers that is within range of the signal**.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_463.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_463.png" align="center"></a>
    <figcaption> Celullar Network. </figcaption>
</figure>

Early mobile radio telephone systems used a **radio antenna** installed on a hill and equipped with a high-power multichannel transmitter. 
* Transmission from the mobile users to the antenna made use of the power supplied by the car battery.


**frequency-reuse principle**: By reducing the power level, the **coverage area can be reduced** and **the frequency band can then be reused in nearby adjacent areas**. This frequency-reuse principle forms the basis for cellular radio communications.

In **cellular radio communications**, a region, for example, a city, is divided into a
number of geographical areas called **cells** and **users within a cell communicate using a band of frequencies**.
*  Cell areas are established based on the density of subscribers. Large cells are used in rural areas, and small cells are used in urban areas

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_464.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_464.png" align="center"></a>
    <figcaption> Components of a cellular Network. </figcaption>
</figure>

**A base station** is placed near **the center of each cell**. 
* The base station has an **antenna** that is used to **communicate with mobile users in its vicinity**.
* Each base station has **a number of forward channels** available to transmit to its mobile
users and **an equal number of reverse channels** to receive from its mobile users.

The mobile switching center (MSC)(mobile telephone switching office (MTSO)): **Base stations are connected by a wireline transmission link** or by **point-to-point microwave radio** to **a telephone switch**.
**The MSC handles connections between cells** as well as to **the public switched telephone network**. 
* As **a mobile user** moves from one cell to another, a handoff procedure is carried out that **transfers the connection from one base station to the other**, allowing the
call to continue **without interruption**.

**Immediately adjacent cells cannot use the same set of frequency channels** because doing so may result in interference in transmissions to users near their
boundary.
* The **set of available radio channels** are **reused** following the frequency reuse pattern.

a seven-cell reuse pattern: 4 or 12 are also ok.
* seven disjoint sets of frequency channels are reused.
*  introduces a minimum distance of one cell between cells using the same frequency channels.
* As traffic demand grows, additional capacity can be provided by splitting a cell into several smaller cells


Advanced Mobile Phone Service (AMPS): an analog cellular system still in use in North America.
* **the frequency band 824 to 849 MHz** is allocated to transmissions **from the mobile station to the base station**.
* **the band 869 to 894 MHz** is allocated to transmissions from **the base station to the mobile station**.
* uses a 30 kHz channel to carry one voice signal, so the total number of channels available in each direction is 25 MHz/30 kHz = 832 channels.
* The bands are divided equally between **two independent service providers**, so
each cellular network has 416 bidirectional channels.
* Each forward and reverse channel pair has frequency assignments that are separated by 45 MHz.
* This separation between transmit and receive channels reduces the interference between the transmitted signal and the received signal.

A small number of channels within each cell have been designated to function as
**setup channels**.
*  the AMPS system allocates 21 channels for this purpose.

When a mobile user turns on his or her unit, the unit scans the setup channels and **selects the one with the strongest signal**. 
* Henceforth it **monitors this setup channel** as long as the **signal remains above a certain threshold**.


### 4.8.1 Establish a call

(1)  To establish a call from the public telephone network or from another mobile user to a mobile user, **the MSC sends the call request to all of its base stations**, which in turn **broadcast the request in all the forward setup channels**, specifying the mobile user’s telephone number. 

(2) When the **desired mobile station** receives the request message, it replies by **identifying itself on a reverse setup channel**. 

(3) The corresponding base station forwards the reply to the MSC and **assigns a forward and reverse voice channel**. 

(4) The base station instructs the mobile station to **begin using these channels**, and the mobile telephone is rung.
    

### 4.8.2 Init a call

(1) the mobile station sends a request in **the reverse setup channel**.

In addition to its **phone number** and **the destination phone number**, the mobile station
also transmits **a serial number** and possible **password** information that is used by the
MSC to validate the request

This call setup involves **consulting the home location register**, which is a database that contains information about subscribers for which this is the home area.

The validation involves **the authentication center**, which contains authentication information about subscribers.


(2)  The MSC then **establishes the call** to the **public telephone network** by using **conventional telephone signaling**, and **the base station** and **mobile station** are moved to the **assigned forward and reverse voice channels**.


### 4.8.3 the Call proceeds

(1) The signal level is **monitored** by the base station. If **the signal level** falls **below a specified threshold**, the MSC is notified and the mobile station is instructed to transmit on the **setup channel**.

(2) All base stations in the vicinity are instructed to **monitor the strength of the signal level** in the **prescribed setup channel**. The MSC uses this information to **determine the best cell** to which the call should be handed off. The
current base station and the mobile station are instructed to prepare for a handoff.

(3) The MSC then **releases its connection to the first base station** and **establishes a connection to the new base station**. The mobile station changes its channels to those selected in the new cell. The connection is interrupted for the brief period that is required to execute the handoff.


### 4.8.4 Roaming

Roaming users enter an area **outside their home region**.


First, **business arrangements must be in place between the home and visited cellular service providers**. 

(1) When the roamer enters a new area, the **roamer registers in the area** by using **the setup channels**. 

(2) The MSC in the new area uses the information provided by the roamer to **request authorization from the roamer’s home location register**. 

(3) The visitor location register contains information about visiting subscribers. After registering, the roamer can **receive and place calls inside the new area**.


### 4.8.5 Standards

The **Global System for Mobile Communications (GSM)** signaling was developed as part of a standard for **pan-European public land mobile system**.

The **Interim Standard 41 (IS-41)** was developed later in North America, using much of the GSM framework.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_465.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_465.png" align="center"></a>
    <figcaption> Protocol stacks in the cellular network. </figcaption>
</figure>

**In the GSM system the base station subsystem (BSS)** consists of **the base transceiver station (BTS)** and **the base station controller (BSC)**. 
* The BTS consists of the antenna and transceiver to communicate with the mobile telephone.
* The BTS is also concerned with the measurement of signal strength.
* The BSC manages **the radio resources of one or more BTSs**.
* The BSC is concerned with **the setup of frequency channels** as well as with **the handling of handoffs**.

Each BTS communicates with **the mobile switching center** through the **BSC**, which provides an interface between **the radio segment and the switching segment**.


The GSM signaling protocol stack has three layers:
* Layer 1 corresponds to **the physical layer**
* layer 2 to the **data link layer**. 
* layer 3 corresponds to **the application layer** 
    * radio resources management (RRM)
    * mobility management (MM)
    * call management (CM)


The **radio air interface** between the mobile station and the BTS is denoted as
Um.
* The **physical layer** across the **Um interface** is provided by the **radio transmission system**.
* The LAPD protocol is **a data link protocol** that is part of **the ISDN protocol stack**.
* LAPDm denotes **a “mobile” version of LAPD**. 
* The **radio resources management sublayer between the mobile station and the BTS** deals with **setting up the radio channels** and with handover (the GSM term for handoff).


The interface between the BTS and its BSC is denoted as the Abis interface.
* The physical layer consists of **a 64 kbps link with LAPD providing the data link layer**.
* A BSC can handle a handover if the handover involves two cells under its control.
This approach relieves the MSC of some processing load. 

The interface between the BSC and the MSC is denoted as the A interface that uses the protocol stack of SS7.

The RRM sublayer in the MSC is involved in handovers between cells that belong to
different BSCs but that are under the control of the MSC.

Only the **mobile station** and the **MSC** are involved in the **mobility management**
and **call management sublayers**. 
* Mobility management deals with the procedures to **locate mobile stations** so that calls can be completed. 

In GSM, cells are grouped into location areas. A mobile station **is required to send update messages** to notify the system when it is moving between location areas.


In GSM, unlike other standards, the mobile station includes a smart card, called the **Subscriber Identity Module (SIM)**, which **identifies the subscriber**, independently of the specific terminal device, and provides a secret authorization key. 

The **mobility sublayer** also deals with the authentication of users.

The **call management sublayer** deals with the establishment and release of calls.


## 4.9 Channelization in Telephone Cellular Networks

### ADVANCED MOBILE PHONE SYSTEM 

The Advanced Mobile Phone System (AMPS)  had an initial allocation of 40 MHz that was divided between two service providers (A and B).

AMPS uses **FDMA operation** for transmission between a base station and mobile stations.
* One band is used for **forward channels from the base station to the mobile stations**
* The other band is used for **reverse channels from the mobile stations to the base station**.

Each channel pair is separated by 45 MHz.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_635.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_635.png" align="center"></a>
    <figcaption> AMPS frequency allocation and channel structure. </figcaption>
</figure>

 AMPS uses analog frequency modulation to **send a single voice signal** over a **30 kHz** transmission channel.

Thus the 50 MHz allocation provides two-way channels number:

$$(50 × 10^6)/(2 × 30 × 10^3) = 832$$

Of these channels 42 are set aside for control purposes


AMPS uses **a seven-cell frequency reuse pattern** so only **one-seventh of the channels** is available **in a given cell**. 

A **measure of the spectrum efficiency** in a cellular system is the number of calls/MHz/cell that can be supported. For AMPS **each service provider** has 416 − 21 traffic channels
that are divided over **seven cells** and 25 MHz:

$$Spectrum Efficiency For AMPS = 395/(7 × 25) = 2.26 calls/cell/MHz$$


### IS-54/IS-136

IS-54 uses a hybrid channelization technique that **retains the 30 kHz** structure of AMPS but **divides each 30 kHz channel into several digital TDMA channels**.

This approach allows **cellular systems to be operated in dual mode**, **AMPS and TDMA**. Each 30 kHz channel carries a 48.6 kbps digital signal organized into six-slot cycles as shown.
* Each cycle has a duration of 40 ms
* Each slot contains 324 bits
* Each slot corresponds to a bit rate of 324 bits/40 ms = 8.1 kbps
* A full-rate channel consisting of **two slots** per cycle, and hence **16.2 kbps**, is used to **carry a voice call**.

Thus IS-54 supports three digtal voice channels in one analog AMPS channel. 
* Half-rate channels (8.1 kbps) 
* Double full-rate channels (32.4 kbps)
* Triple full-rate channels (48.6 kbps)

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_636.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_636.png" align="center"></a>
    <figcaption> IS-54 TDMA structure. </figcaption>
</figure>

We note that the 416 analog channels available provide 3 × 416 = 1248 digital channels
* 21 of these channels are used for control purposes
*  the frequency reuse factor is 7

$$SpectrumEfficiencyOfIS-54 = 1227/(7 × 25) = 7 calls/cell/MHz$$



### GSM

The **Global System for Mobile Communications (GSM)** is a European standard for cellular telephony that has gained wide acceptance.

GSM was designed to operate in
* the band 890 to 915 MHz for the **reverse channels**
* the band 935 to 960 MHz for the **forward channel**

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_637.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/_637.png" align="center"></a>
    <figcaption> (a) GSM channel structure; (b) GSM TDMA structure. </figcaption>
</figure>

The available frequency band is divided into carrier signals that are spaced 200 kHz apart. 

Thus the 25 MHz bandwidth can support 124 one-way carriers

The carrier signal is divided into **120 ms multiframes**, where each multiframe consists of **26 frames**, and each frame has **eight slots**.

Two frames in a multiframe are used for **control purposes**, and the remaining 24 frames carry traffic.


A **full-rate traffic channel** uses one slot in every traffic frame in a multiframe.
Therefore, the bit rate of a full-rate channel is

$$Traffic channel bit rate = 24 slots/multiframe × 114 bits/slot× (1 multiframe/120 ms) = 22,800 bps$$


If we assume 124 carriers in the 50 MHz band, then we obtain a total of 124 × 8 = 992 traffic channels. Assuming a frequency reuse factor of 3:

$$Spectrum efficiency of GSM = 992/(3 × 50) = 6.61 calls/cell/MHz$$
