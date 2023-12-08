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

The **pointer structure** consisting of **the H1, H2, and H3 bytes**, 








