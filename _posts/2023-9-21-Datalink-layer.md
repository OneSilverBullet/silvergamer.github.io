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

* P_f: the probability that a frame has errors.

The Stop-and-wait ARQ on average time cost of transferring a frame:

$$t_{SW} = t_0 / (1 - P_f)$$

The transmission efficiency with the error probability:

$$η_{SW} = (n_f - n_o) / t_{SW} / R$$

$$η_{SW} = ((1 - n_o / n_f) / (1 +  n_a / n_f  + 2(t_{prop} + t_{proc})R / n_f))(1-P_f)$$


The probability that a single bit gets through without error:

$$1- p$$

The probability that all nf get through assuming random bit errors:

$$1-P_f = (1 - p) ^{n_f}$$



### 2.3 GO-BACK-N ARQ

#### 2.3.1 Interaction Process of GO-BACK-N ARQ

The go-back-n ARQ forms the basis of HDLC Data Link Protocol.

The transmitter has a limit on the number of frames Ws.

Window size Ws is choosen larger than the delay-bandwidth product to ensure that the channel or pipe is kept full.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/Goback4.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/Goback4.png" align="center"></a>
    <figcaption>The GO-BACK-N ARQ.</figcaption>
</figure>



Pipelined: A procedure where the processing of a new task is begun before the completion of the previous task.

Go-back-N ARQ base on pipelined mechanism. If there is a frame in error, the transmitter will reaches the maximum number of outstanding frames. Then it is **forced to go back N(Ws) frames**.

* Go-back-N ARQ as stated above depends on the transmitter exhausting its maximum number of oustanding frames to trigger the retransmission of a frame.


Q: If packets arrive sporadically, there may not be Ws - 1 subsequent transmissions.

A: a timer is associated with each transmitted frame.
* the transmitter maintain a list of the frames that it is processing.
* S_last is the number of last transmitted frame that remain unacknowledged.
* S_recent is the number of the most recently transmitted frame.
* The lower end of the window is given by S_last, the upper limit of the transmitter window is S_last + Ws - 1.

If S_recent reaches the upper limit of the window, the transmitter is not allowed to transmit further new frames until the send window "slides" foward with the receipt of a new acknowledgement.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/TrueARQ.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/TrueARQ.png" align="center"></a>
    <figcaption>The true mechanism of ARQ: every frame associate with a timer.</figcaption>
</figure>


Go-back-n ARQ is an example of **sliding-window protocol**.

**Attention**: When the transmitter receives an ACK with a given value R_next, it can assume that all the prior frames have been received correctly. **Even if the sender has not received ACK for those frames, either because they were lost or the receiver chose not to send them**.

$$S_{last} <= R_{next} <= S_{recent}$$


The process of transmission:

(1) Transmitter

**ready state**
* The transmitter is in **ready state**.
* Hight layer request the service of current layer, a packet from higher layer is received.
* The transmitter prepare a **frame**: a header with sequence number S_recent, the packet, a CRC. **A timer is started**.
* The transmitter starts a timer and transmitted the frame using the services of the layer below.
* If S_recent = S_last + Ws - 1, the transmitter enters a **blocking state**.
* Otherwise The transmitter enters a **ready state**.

**blocking state**
* the transmitter wait until **an acknowledgement is received** or the **time-out period expires**.
    * time-out: retransmitted S_last and all subsequence frames, all frame timers are restarted.
    * outside ACK: ignored ACK and stay in blocking state.
    * correct ACK: S_last = R_next, set maximum send window number to S_last + Ws - 1. **update S and enter ready-state**.

(2) Receiver

The receiver always in **ready-state** waiting for a notification of an arriving frame from the layer below.

* a frame arrive, the receiver accepts the frame and check errors.
    * no error: update R_next and accept the frame, transmitted a ACK. package is deliveried to the higher layer.
    * wrong sequence number: the frame is discarded and send a ACK with old R_next.
    * error frame: the frame is discarded and no further action is taken.


Q: the window size of duplicate transmission

In general, the window size Ws is 2^m - 1 or less and assume that the current sent window is 0 up to Ws - 1.

**Curcially, the receiver will not receive frame Ws until the acknowledgement for frame 0 has been received at the transmitter**.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/windowSize.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/windowSize.png" align="center"></a>
    <figcaption>The correct window size.</figcaption>
</figure>


When the information flow is bidirection, the transmitter and receiver functions of the modified GO-BACK-N protocol are implemented in both A and B process.
* Many control frames can be eliminated by **piggybacking** the acknowledgments.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/bidirection.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/bidirection.png" align="center"></a>
    <figcaption>The bidirection ARQ .</figcaption>
</figure>


**ACK Timer**: The receiver can set a ACK timer that defines the maximum time it will wait for the availability for an information frame. When the time expires a control frame will be used to convey the acknowledgement.


#### 2.3.2 Performance Issues

Conceptions:

* t_GBN： the time takes to get a frame through.
* P_f: the error frame probability. If P_f is 0.9, which means that 1 in 10 frames get through without error.
* t_f: the time takes to transmit a frame.

The average number of transmissions:

$$ 1/ (1 - P_f)$$

The total average time required to transmit a frame in GO-BACK-N:


$$t_f = n_f / R$$

$$t_{GBN} = t_f +  P_f W_S t_f / (1 - P_f)$$

The efficiency of GO-BACK-N:

$$η_{GBN} = (n_f - n_o) / t_{GBN} / R$$


$$η_{GBN} = ((1 - n_o / n_f) / (1 +  (W_S - 1) P_f))(1-P_f)$$



### 2.4 Selective Repeat ARQ

#### 2.4.1 Interaction Process of GO-BACK-N ARQ

**Conception**

The Go-Back-N ARQ's protocol is inefficient because of the need to **retransmit the frame in error and all the subsequent frames**.

The Selective Rpeat ARQ:
* the receive window is made larger than one frame so that the receiver can **accept frames that are out of order but error free**.
* the retransmission mechanism is modified that **only individual frames are retransmitted**.
 
 When an out-of-sequence frame is observed at the receiver, a NAK frame is sent with sequence number R_next. When the transmitter receive the NAK with sequence number R_next, the transmitter retransmits the R_next frame. **the piggybacked acknowledgement in the information frame continues to carry R_next**.

**The Interaction Process**

(1) Initialize

The transmitter send window set to {0, 1, 2, ..., Ws - 1} and S_last = 0. The receiver window set to {0, 1, ..., Wr - 1} and R_next = 0.

(2) Transmitter

Sending Packages:
* When the send window is nonempty, the transmitter is in **ready state** waiting for a request from higher layers.
* When a request occurs, the transmitter accept a packet from higher layers and prepare a frame(header, packet, CRC). Update **the lowest available number** of send window to S_recent and start a timer.
    * If S_recent = S_last + Ws - 1, the send window has been empty and transmitter enter **blocking state**.

Ready State Receiving ACK:
* If an error-free ACK frame is received with R_next in range of S_last and S_recent, the send window slides forward by setting S_last = R_next, and maximum of slide window is S_last + Ws - 1. 
* If an error-free NAK frame with R_next(between S_last and S_recent) is received, the frame with sequence number R_next is retransmitted. Update S_last to R_next (the NAK means that the fames before R_next has been received) and maximum of slide window is S_last + Ws - 1.
* If a timer expires, then the transmitter **resends the corresponding frame and resets the timer**.

Blocking State Receiving ACK:
* Reject to accept packet transferred from above layers. If timer is expires, the transmitter resends the corresponding frame and reset S_recent.
* If an error-free ACK is received with R_next in range of S_last and S_recent, the send window slides forward by setting S_last = R_next, and maximum of slide window is S_last + Ws - 1. The transmitter enter **ready state**.
* If an error-free NAK frame with R_next(between S_last and S_recent) is received, the frame with sequence number R_next is retransmitted. Update slide window's range and the transmitter enter **ready state**.
* If an error-free ACK is received with sequence number outside the range of S_last and S_recent, the frame is discarded and no further action is taken.

(3) Receiver

When a frame arrive, the receiver checks the frame for errors.

* If no errors are detected and the sequence number in the range of R_next to R_next + Wr - 1. Then the frame is accepted and buffered. **An acknowledgement frame is transmitted**.
* If the received frame's sequence number is R_next, and if R_next + 1 up to  R_next + k - 1 has been received, then the receive sequence number is incremented to R_next + k, **the reciever window slides forward** and the packets are delivered to the higher layer.
* If an arriving frame has no errors but the sequence number is outside the receive window, then the frame is **discarded** and an acknowledgement with sequence number R_next is sent to transmitter.
* If an arriving frame has errors, then the frame is discarded and no further action is taken.


**Maximum Window Size**

The maximum window size is Ws = 2^(m-1).


**Application**

Transmission Control Protocol(TCP): uses Selective Repeat ARQ to provide a end-to-end error control across a network. TCP is used over internets that use IP to transfer packets in connectionless mode.

Service Specific Connection Oriented Protocol(SSCOP): provides error control for signaling message in ATM networks.


#### 2.4.2 Performance Issues

Given frame transmission is received correctly with probability:

$$1-P_{f}$$

The average transmission time:

$$t_{SR} = t_f / (1 - P_f)$$

The efficiency of Selective Repeat ARQ:

$$η_{SR} = (n_f - n_o) / t_{SR} / R$$

$$η_{SR} = (1 - n_o/n_f)(1-P_f)$$



### 2.5 Comparison

The parameters that affect efficiency:
* header and CRC overhead
* delay-bandwidth product.
* frame size.
* frame error rate.

We can **neglect the header and CRC overhead's negative influence** in efficiency of transmission when the frame is not too short.

$$η_{SR} = (1-P_f)$$

$$η_{GBN} = (1-P_f) / (1 +    L P_f)$$


$$η_{SW} = (1-P_f)/ (1 + L)$$

$$L = 2(t_{prop} + t_{proc})R / n_f$$

The L is the size of the pipe in multiples of frames, and we assume the window size is Ws = L - 1.

Adaptivity and Robustness are equally important attributes of ARQ.



Error free frames out of expected sequence number are accepted by receiver.

Only individual frames are retransmitted. Not all of the buffer is retransmitted.

The maximum window size 2^m - 1 is not enough for Selective Repeat ARQ. The window size should be 2^(m-1).

The performance of Selective ARQ: in textbook and ppt.

Framing is essential in Data Link Layers. A Frame:
* Start Bit 
* Information 
* Stop Bit

Framing example:

(1) T1 Frame Length: 192 bit, 24 channel, each channel have 8 bit.

(2) Sonet Frame: 2bits beginning.

fixed length block transfer.

STX: 02, beginning of frame.

ETX: 03, end of frame.

DLE: 10, data link escaped character.

**A framing method is not considered to be transparent, if any sequence of bits cannot be transmitted.**

* bit stuffering

Fig5.34: method of transfering numbers bits.

HDLC Flag: 01111110 7E. Start with flag, end with flag.

For transparency perform bit staffering, insert a 0 after 5 ones in the data.

PPP: point-to-point protocol.
* Control escape character: 7D. 7D->7D5D; 7E->7D5E. 7E never occur in the information.

GFP: Generic Framing Procedure.

ATM: Aynschronous Transfer Method.

GFP payload: User Information.

Fig 5.39:

PLI: the next two bytes contains the CRC of this two bytes.

Fig 5.43: How ARQ implement.

HDLC Protocol: High Level Data-link Control.

HDLC can operate two configuration:
* unbalanced configuration.
* balanced configuration.

Mode of transparency:
* Normal response mode
* Used with unbalanced (NRM)
* Asynchronised Mode (ABM) used with balanced configuration.

FCS: frame check sequence.
* 0: information frame.
* 1: supervisory frame.
* 11: unnumbered frame

Supervisor Frame: Control Frame.

SS = 00: receive ready.
SS = 01: reject control frame used in Go-Back-N ARQ.
SS = 10: receive not ready or it is RNR. flow control.
SS = 11: for SR ARQ.



## 3. Data Link Controls

The main purpose of Data Link Controls is to enable the transfer of frames of information over the digital bits or octet stream provided by physical layer.

### 3.1  Framing

Framing: involves **identifying the begining and end of a block of information within digital stream**.
* Presuppose: There is enough synchronization at the physical layer.

Framing may involve **delineating the boundaries between frames that are of fixed length** or it may involve **delineating between frames that are of variable length**.

The information is contrained to be 
* an integer number of characters of a certain length.
* any number of bits.


T-1 carrier system: a frame consists of a single framing bit followed by 24 octets. Thereafter, only bit errors, loss of bits, or loss of signal will cause a loss of frame synchronization.

SNET frames: fixed length framing in the physical layer.

ATM cell delineation: is a fixed length packet that consists of 53 bytes. ATM framing can be viewed as a data link layer function and is based on the use of CRC checking.

The essence of framing in these three examples is character counting.

The purpose of the framing bit or character is merely to **confirm that the frame position has not changed or been lost**.

Variable-length frames need more information to delineate.
* special characters to identify begining and end of frame.
* special bit patterns "flags" to identify the beginning and end of frames
* character counts and CRC checking methods

### 3.1 Beginning and End of frame charcters and byte stuffing

**Character-based frame synchronization methods** are used when the information in a frame consists of **an integer number of characters**.

To delineate a frame of characters, special eight-bit codes that do not correspond to printable characters are used as **control characters**.
* STX: start of text control character. 02
* ETX: end of text character. 03

Byte Stuffing: to solve the transparent problem.
* DLE: data link escape control with HEX value 10. 

DLE STX indicate the beginning of the frame and DLE ETX denotes the end. Extra DLE is inserted or stuffed before **the occurence of a DLE inside the frame**.


### 3.2 Flags, Bit Stuffing and Byte Stuffing

**Flag-based frame synchronization** was developed to transfer **an arbitrary number of bits** within a frame.

The beginning and end of an HDLC frame is indicated by the presence of an eight-bit flag.

The beginning and end of an HDLC frame is indicated by the presence of an eight-bit flag HEX 7E.

**Bit stuffing** prevents the occurrence of the flag inside the frame. The transmitter examines the content of the frame and insert an extra 0 after each instance of 5 consecutive 1s. The transmitter then attaches the flag at the beginning and end of the resulting bit-stuffed frame.

**The receiver looks for the 5 consecutive 1s in the received sequence.**
* 0 after 5 1s: stuffing bit.
* 10 after 5 1s: a flag.
* 11 after 5 1s: an error.

**Application: PPP**

The HDLC flag is used in PPP data data link control to provide framing.

In PPP the data in a frame is constrained to be an integer number of octets. 
* 0x7E: indicate the beginning and end of a frame.

Byte stuffing is used to deal with the occurrence of the flag inside the frame. 
* Control Escape Character: 0x7D.
* Any occurence of the flag or the Control Escape character inside the frame is replaced by a two-character sequence consisting of **Control Escape character followed by the original octet exlusive-ORed with 0x20**.
    * 0x7E -> 0x7D 0x5E
    * 0x7D -> 0x7D 0x5D

The receiver must **remove the inserted Control Escape characters** prior to computing the CRC checksum. Each Control Escape octet is removed and following octet is exclusive-ORed with 0x20, unless the Octet is the Flag.

PPP framing can be used over **asynchronous**, **bit-synchronous**, or **octet-synchronous** transmission systems.
Also provide the framing in Packet-over-SONET.


### 3.3 CRC-BASED FRAMING

Generic Framing Procedure(GFP): a new standard for framing that is intended to address some shortcomings of PPP framing.

PPP shotcoming:
* the size of transmitted frame that contains a given number of characters cannot be predicted ahead of time.
* privide an opportunity for malicious users to inflate the bandwidth consumed by inserting flag pattern within a frame.

GFP combines a frame length indication field with the Head Error Control(HEC) method used to delineate ATM cells.
* looking at the **length indication field** to find the length of the frame.
* Attention: the occurrence of an error in the count field leads to a situation where the begining of the next frame cannot be found.

The GFP Frame Structure:
* Payload Length Indicator(PLI): gives the **size in bytes of the GFP payload area** and so indicate the begining of next GFP frame.
* cHEC field: contains **the CRC-16 redundancy check bits** for PLI and cHEC fields. cHEC can be used to correct single errors and to detect multiple errors.

**Receiver Process**
* **hunt state**: it examines 4 bytes at a time to see if **the CRC computed over the first two bytes equals the contents of the next two bytes**. If there is no match, the receiver moves forward by one byte as GFP assumes octet synchronous transmission given by the physical layer. If match, receiver enter pre-sync state.
* **pre-sync state**: the receiver uses the tentative PLI field to **determine the location of the next frame boundary**. If a target number N of successful frame detections has been achieved, then the receiver moves to **the sync state**. 
* **sync state**: receiver examines each PLI, validates it using the cHEC, extracts the payload, and proceeds to the next frame.


GFP is designed to operate over octet-synchronous physical layers. The frames in GPF can be of **variable length** or of **fixed length**.


**Application**

In the frame-mapped mode, GFP can be used to carry variable length payloads, such as Ethernet frames, PPP/IP packets, or any HDLC-framed PDU.

In the transparent-mapped mode, GFP carries synchronous information streams with low delay in fixed length frames.




## 4. HDLC Data Link Control

High-Level Data Link Control(HDLC) provides a rich set of standards for operating a data link over bit synchronous physical layers.


### 4.1 Data Link Services

The **data link control** as a set of functions whose role is  to provide a communication service to the network layer.

The network layer entity is involved in an exchange of packets with a peer network layer entity located at a neighbor packet-switching node. **To exchange these packets the network layer must rely on the service that is provided by the data link layer**. The data link layer itself **transmit frames and make use of the bit transport service** that is provided by the physical layer, that is, the actual digital transmission system.

**Transmission**

The network layer passes **its packets(network layer PDU / NLPDU)** to the data link layer in the form of **a data link SDU**. The data link layers adds **a header and CRC check bits** to the SDU to form **a data link PDU**. The frame is transmitted using the physical layer.

**Receiver**

The data link layer at the other end recovers the frame from the physical layer, **performs the error checking, and when appropriate delivers the SDU(packet) to its network layer**.

Data link layers can be configured to provide several types of services to the network layer.
* connection-oriented service: provide error-free, ordered delivery of packets. involves three phase:
    * setting up the connection: service access point(SAP).
    * the actual transfer of packet encapsulated in data link frames.
    * release the connection and free up the variable and buffers. 

* connectionless service: there is no connection setup, and the network layer is allowed to pass a packet across **its local SAP together with the address of the destination SAP** to witch packet is being sent.
    * acknowledged service.
    * unacknowledged service.


### 4.2 HDLC Configurations and Transfer Modes

HDLC provides for a variety of data transfer modes that can be used in a number of different configurations. 

The **normal response mode(NRM)** of HDLC defiens the set of procedures that are to be used with **the unbalanced configurations**.

the unbalanced configurations: use **a command/response interaction** whereby the primary station sends command frames to secondary stations and interrogates or polls the secondaries to provide them with transmission opportunities. The secondary stations reply using **response frame**.

the balanced point-to-point link configuration: two stations implement the data link control, acting as peers. 
* application: asynchronous balanced mode(ABM)
* information frames can be transmitted in full-duplex manner.


### 4.3 HDLC Frame Format

The HDLC Frame Format:
* HDLC frame is delineated by two 8-bit flags. 
* The frame has a field for only one address. 
* 8- or 16- bit control field.

The control fields type:
* 0 in the first bit: an information frame.(I-frame)
* 10 in the first two bits : supervisory frame.
* 11 in the first twor bits: unnumbered frame.

the information frame and supervisory frame **implement the main functions of data link control, which is to provide error and flow control**.

Poll/Final bit(P/F): In unbalanced mode this bit indicates a poll when being sent from a primary to a secondary. The bit indicates a final frame when being sent from a secondary to a primary. 
* To **poll a given secondary**, a host sends a frame to the secondary, indicated by the address field with the P/F bit set to 1. Only the last frame transmitted from **the secondary has the P/F bit set to 1 to indicate that it is the final frame**.

N(S) field in I-frame provides **the send sequence number** of the I-frame. 

N(R) field is used to **piggyback acknowledgements and to indicate the next frame that is expected at the given station**.

Supervisory frames have 4 values if the S bits in the control field:
* SS = 00: **receive ready frame(RR)**. RR frames are used to acknowledge frames when no I-frames are available to piggyback the acknowledgment
* SS = 01: **reject(REJ) frame**, which used by reciever to send a negative acknowledgement. REJ frame indicates that an error has been detected and that the transmitter should go back and retransmit frames from N(R) onwards(Go-Back-N ARQ).
* SS = 10: indicates a **receive not ready frame(RNR)**. The RNR frame acknowledges all frames up to N(R)-1 and inform the transmitter that receiver not accept any more frames with temporary problem. 
* SS = 11: indicates a **selective reject frame(SREJ)**. The transmitter should retransmit the frame indicated in the N(R) subfield(Selective Repeat ARQ).

**The combination of I-frame and supervisory frames allow HDLC to implement Stop-and-Wait, Go-back-N and Selective Repeat ARQ.**

HDLC has two options for sequence numbering. In default HDLC uses a three-bit sequence numbering.
* default 3 bits. window size is 7 for Stop-and-Wait and Go-Back-N ARQ.
* In the extended sequence numbering option, the control field is increased to 16 bits. The sequence numbers are increased to 7 bits. The maximum send window size is 127 for Stop-and-Wait and Go-Back-N ARQ. For Selective Repeat ARQ, the maximum send and receive window sizes are 4 and 64 respectively.

The unnumbered frames implement a number of control functions. Each type of unnumbered frame is identified by a specific set of M bits. During call setuop ir rekease

During call setup or release, specific unnumbered frames are used to set the **data transfer mode**.
* The **Set Asynchronous Balanced Mode(SABM)** frame indicates that the sender wishes to set up an asynchronous balanced mode connections.
* The **Set Normal Response Mode(SNRM)** frame indicates a desire to set up a normal response mode connection.
* The **disconnect(DISC)** frame indicates that a station wishes to terminate a connection.
* The **Unnumbered Acknowledgement(UA)** frame acknowledges frames during call setup and call release.
* The **Frame reject(FRMR)** unnumbered frame reports receipt of an unacceptable frame.

### 4.4 Typical Frame Exchanges

We use the following convention to specifiy the frame types:
* The first entry indicates the **contents of the address field**;
* The second entry specifies the **type of frame**, I for information, RR for receive ready, etc.
* The third entry is N(S) in the case of I frame only, **the send sequence number**. 
* The following entry is N(R), **the recieve sequence number**.
* A P or F at the end of an entry indicates **that Poll or Final bit** is set.


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/5ed.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/5ed.png" align="center"></a>
    <figcaption>Exchange of frames for connection establishment and release.</figcaption>
</figure>


The above figure shows the exchange of frames that occurs for connection establishment and release. 
* Station A send SAMB frame to indicate that it wishes to setup an **ABM mode connection**.
* Station B send a Unnumbered Acknowledgement to indicate its readiness to proceed with the connection.
* A bidirectional flow of information and supervisory frames then take place.
* The station with to disconnect, and it sends a DISC frame.


#### 4.4.1 Normal Response Mode

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/5ed2.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/5ed2.png" align="center"></a>
    <figcaption>Exchange of frames using normal response mode.</figcaption>
</figure>

The primary station A is communicating with the secondary stations B and C.
* Station A begins by sending a frame to station B with **poll** bit set and the sequence number N(R) = 0 (**the next frame it is expecting from B is frame 0**).
* Error occurs, station A sends an SREJ frame with N(R) = 1 without poll bit. This frame indicates that **Station B should be prepared to retransmit frame 1**.


#### 4.4.2 Asynchronous Balanced Mode 

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/5ed3.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/Telecommunication/5ed3.png" align="center"></a>
    <figcaption>Exchange of frames using asynchronous balanced mode.</figcaption>
</figure>


The address field always contains the address of the secondary station. There are some conventions: 
* If a frame is **command** then the address field contains **the address of receiving station**.
* If a frame is a **response**, the address field contains **the address of the sending station**.
* Information frames are always commands, RR and RNR frames can be either command or response frame, REJ frames are always response frames.

Important Process Stage:
* Frame 1 from station A has undergone transmission errors, and when station B receives a frame with N(S)_A = 2, it finds that the frame is out of sequence.
* Station B then sends a negative acknowledgement by transmitting an REJ frame with N(R)_B = 1.
* After receiving the REJ frame, station A goes back and begins retransmitting from frame 1 onwards.
* Station B does not have **additional I-frames for transmission**, it sends **acknowledgements by using RR frame in response form**.
