---
layout: post
title:  "[Distributed System]Advanced Distributed System Design"
date:   2024-3-12
excerpt: "Discuss the advanced conceptions about DSD."
tag:
- Distribute System Design
comments: false
blog: true
feature: https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/blogHead/directX12partI.jpg
---


## 1. Fundamentals of Replication

### 1.1 Conceptions

Failure Meanings:
* **Halting/crash failures**: component simply stops
* **Fail-stop**: halting failures with notifications
* **Omission failures**: failure to send/receive message
* **Network failures**: network link breaks
* **Network partition**: network fragments into two or more disjoint subnetworks
* **Timing failures**: action early/late; clock fails, etc.
* **Byzantine failures**: arbitrary malicious/nonmalicious behavior

Solutions:
* Replace critical components with group of components that can each act on behalf of the original one.
* Develop a technology by which **states can be kept consistent and processes** in system can
agree on status (operational/failure) of components
* Separate handling of partitioning from **handling of isolated component failures** if possible.

### 1.2 Basic Architecture Model

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/bam.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/bam.png" align="center"></a>
    <figcaption>The basic architectural model.</figcaption>
</figure>

Replication:
* Performance Enhancement
    * Several web servers can have the same DNS name and the servers are selected in turn. To share the load.
    * Replication of read-only data is simple, but replication of changing data has overheads.
* High Availability
    * Server Failures
        * Replicate data at failure-independent servers and when one
            fails, client may use another.
    * Network partitions and disconnected operation
        * Users of mobile computers deliberately disconnect, and then on re-connection, resolve conflicts.

Replica Manager(RM): An RM contains replicas on a computer and access them directly.
* RMs apply operations to replicas recoverably.
* Objects are copied at all RMs unless we state otherwise.
* Static systems are based on a fixed set of RMs.
* Dynamic system: RMs may join or leave.
* RM can be a state machine.




### 1.3 Detecting Failure

There are ways to overcome failures that don't explicityly detect them

But situation is much easier with detectable faults:
* Process does something to say “I am still alive”
* Absence of proof of liveness taken as evidence of a failure

Consistent detection is impossible “in practice”.

### 1.4 Fault Tolerant Services

Provision of a service that is correct even if f processes fail.
* By replicating data and functionality at RMs
* Assume reliable communication and no partitions
* a service is correct if it responds despite failures and clients can’t tell the difference between replicated data and a single copy.
* Care is needed to ensure that a set of replicas produce the same result as a single one would.

We can achieve fault tolerance through replication.
* If f of f +1 servers crash then 1 remains to supply the service.
* If f of 2f +1 servers have **non-malicious Byzantine faults** then they can supply a
correct service.
* If f of 3f +1 servers have **malicious Byzantine faults** then they can supply a correct service.

Requirements for replicated data:
* Replication Transparency: Clients see logical objects (not several physical
copies) They access one logical item and receive a single result.
* Consistency: Specified to suit the application. When a user of a diary disconnects, their local copy may be inconsistent with the others and will need to be reconciled when they connect again. But connected clients using different copies should get consistent results

### 1.5 The passive(Primary-Backup) Model for Fault Tolerance


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/pb.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/pb.png" align="center"></a>
    <figcaption>The primary-backup model.</figcaption>
</figure>

There is at any time a single primary RM and one or more secondary (backup, slave) RMs.

FEs communicate with the primary which executes the operation and sends copies of the updated data to the result to backups.

If the primary fails, one of the backups is promoted to act as the primary.

Frive Phases:
* **Request**: A FE issues the request, containing a unique identifier, to the
primary RM.
* **Coordination**: The primary performs each request atomically, in the order in
which it receives it relative to other requests. It checks the unique id; if it has already done the request it resends the response.
* **Execution**: The primary executes the request and stores the response.
* **Agreement**: If the request is an update the primary sends the updated state, the response and the unique identifier to all the backups. The backups send an acknowledgement.
* **Response**: The primary responds to the FE, which hands the response back to the client.

### 1.6 Linearizability of a Replication System

The correctness criteria for replicated objects are defined by referring to **a virtual interleaving** which would be correct.

A **replicated object service** is **linearizable** if for any execution there is some interleaving of clients’ operations such that
* The interleaved sequence of operations meets the specification of a (single) correct copy of the objects.
* The order of operations in the interleaving is consistent with the real time at which they occurred.

**Sequential Consistency**

A replicated shared object service is sequentially consistent if for any execution there is some interleaving of clients’ operations such that:
* The **interleaved sequence of operations** meets the specification of a (single) correct copy of the objects.
* The **order of operations in the interleaving** is consistent with the program order in which each client executed them.

### 1.7 Passive Replication(Primary-Backup)

This system implements **linearizability**, since **the primary sequences all the operations** on the shared objects.

If the primary fails, the system is linearizable, if a single backup takes over exactly where the primary left off
* The primary is replaced by a unique backup
* Surviving RMs agree which operations had been performed at take over

To survive f process crashes, f+1 RMs are required.
* Cannot deal with Byzantine failures because the client cannot get replies from the backup RMs

To design **passive replication** that is **linearizable**. Challenges:
* **View synchronous communication** has relatively **large overheads**
* Several rounds of messages per multicast
* After failure of primary, there is **latency** due to **delivery of group view**

Variant in which **clients can read from backups**
* reduces the work fro the primary
* get sequential consistency but not linearizability

### 1.8 Active Replication

(1) The RMs are **state machines** all playing the **same role** and **organised as a group**.
* All start in the **same state** and perform the **same operations in the same order** so that their state remains **identical**.

(2) If an RM crashes it has **no effect on performance of the service** because the others continue as normal.

(3) It can **tolerate Byzantine failures** because the FE can collect and compare the replies it receives.

Five Phases:
* **Request**: FE attaches **a unique id** and uses **totally ordered reliable multicast** to send request to RMs. FE can at worst, crash. It does not issue requests in parallel

* **Coordination**: The multicast delivers requests to all the RMs in
the same (total) order.

* **Execution**: Every RM executes the request. They are state machines and **receive requests in the same order**, so the effects are identical. The id is put in the response

* **Agreement**: No agreement is required because **all RMs execute the same operations in the same order**, due to **the properties of the totally ordered multicast**.

* **Response**: FEs collect responses from RMs. FE may just use one or more responses. If it is only trying to **tolerate crash failures**, it gives the client the first response.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/ar.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/ar.png" align="center"></a>
    <figcaption>The active replication for fault tolerance.</figcaption>
</figure>

As RMs are **state machines** we have **sequential consistency**
* Due to reliable totally ordered multicast, the RMs collectively do the same as a single copy would do
* It works in a synchronous system
* In an asynchronous system reliable totally ordered multicast is impossible – but failure detectors can be used to work around this problem. 

This **replication scheme** is **not linearizable**
* Because total order is not necessarily the same as real-time order

To deal with Byzantine failures
* For up to **f Byzantine failures**, use **2f +1 RMs**
* FE collects **f +1 identical responses**

To improve performance,
* FEs send read-only requests to just one RM



## 2. Group Communication

### 2.1 Conceptions 


**(1) Multicast**

Multicast Communication: requires coordination and agreements. The aim is for members of a group to receive copies of messages sent to the group.

A process can multicast by the use of a single operation instead of a send to each member.
* IP multicast: aSocket.send
* The single operation makes sure the efficiency; delivery guarantees.

**(2) System Model**

The system consists of a collection of procersses which can communicate **reliably** over 1-1 channels.
* processes fail only by crash
* processes are members of groups(destinations of multicast messages)
* one process can belong to multiple groups

Some basic operations:
* multicast(g, m): sends message m to all members of process group g.
* deliver(m): get a multicast message delivered. (may be delayed to allow for ordering or reliability).

Be attention, the parameter m contains:
* id of sending process
* id of destination group 

**(3) Group Types**

Closed Group: Only members can send to group, a member delivers to itself.
* useful for coordination of groups of cooperating servers

Open Group: They are useful for notification of events to groups of
interested processes.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/gt.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/gt.png" align="center"></a>
    <figcaption>The group types.</figcaption>
</figure>

### 2.2 one-to-one communication

The term **reliable 1-1 communication** is defined in terms of validity and integrity as
follows:

* Validity: Any message in **the outgoing message buffer** is eventually delivered to the **incoming message buffer**, by use of **acknowledgements and retries**.

* Integrity:
    * The message received is **identical to the one sent**, by use of checksums;
    * No messages are delivered twice, by **rejecting duplicates** (e.g. due to retries).

### 2.3 Basic Multicast(B-Multicast)

A correct process will eventually deliver the message
provided **the multicaster does not crash**.

Drawback: If the number of processes is large, the protocol will suffer
from ack-implosion.

Implmentation:

(1) Each process p maintains:
* A sequence number S_p for each group it belongs to.
* A sequence number R_q for the latest message it has delivered from process q.

(2) For process p to B-multicast a message m to group g.
* piggybacks S on the message m, using IP multicast to send it.
* piggy backs sequence number allow recipients to learn **about message they have not received**.

(3) On recieve(g,m,S) ar p:
* S = R + 1, B-deliver and increment R_q by 1.
* S < R + 1, reject information because it has been delivered before.
* S > R + 1, note that a message is missing, request missing message from sender.

If the sender crashes, then a message may be delivered to some members of the group but not others.


### 2.4 Reliable Multicast(R-Multicast)

The protocol is correct even if the multicaster crashes.

Operations:
* R-multicast
* R-deliver

Satisfies criteria:
* **Integrity**: a correct process, p delivered m at most once.
* **Validity**: if a correct process multicasts m, it will eventually
deliver m.
* **Agreement**: if a correct process delivers m then all correct
processes in group(m) will eventually deliver m.


**(1) Reliable multicast algorithm over B-Multicast**

Processes can belong to several closed groups.

```
On Initialization:
    Received:={}

For process p to R-multicast message m to group g:
    B-multicast(g, m);

On B-deliver(m) at process q with g = group(m):
    if(m not belong Received)
        Received := Received ∪ {m}
        if(q ≠ p) 
            B-multicast(g, m)
        R-deliver m.
```

**(2) Reliable multicast over IP multicast**

This protocol assumes **groups are closed**. It uses:
* Piggybacked acknowledgement messages.
* Negative acknowledgements when messages are missed

Process p maintains:
* S_p is a message sequence number for each group it belongs to.
* R_q is a message sequence number of latest message received from process q to g.

For process p to R-multicast message m to group g.
* Piggyback S and acks for messages received in the form [q, R].
* IP multicasts the message to g, increments S_g by 1.

Properties:
* **Integrity**: 
    * duplicate messages detected and rejected.
    * IP multicast uses checksums to reject corrupt messages
* **Validity**:
    * due to IP multicast in which sender delivers to itself
* **Agreement**:
    * processes can detect missing messages.
    * They must keep copies of messages they have delivered so that they can re-transmit them to others.

**Discarding of copies of messages** that are no longer needed.
* When piggybacked acknowledgements arrive, note which
processes have received messages. When all processes in g
have the message, discard it.
* Problem of a process that stops sending - use ‘heartbeat’
messages.


### 2.5 Hold-back Queue

The hold-back queue is not necessary for reliability as in the
implementation using IP muilticast, but it simplifies the protocol,
allowing sequence numbers to **represent sets of messages**.

Hold-back queues are also used for ordering protocols. 

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/hq.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/hq.png" align="center"></a>
    <figcaption>The hold back queue for arriving multicast messages.</figcaption>
</figure>




### 2.6 Ordered Multicast

The basic multicast algorithm delivers messages to
processes in an arbitrary order. A variety of
orderings may be implemented.


* Causal Ordering: If multicast (g, m) **happened** before multicast (g,m’ ), then
any correct process that delivers m’ will deliver m before m’

* Total ordering:  If a correct process **delivers** message m before it delivers m’,
then any other correct process that delivers m’ will deliver m before m’.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/timeline.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/timeline.png" align="center"></a>
    <figcaption>The Total, FIFO and causal ordering of multicast messages.</figcaption>
</figure>


**Reliable and totally ordered multicast**
* Can be implemented in a synchronous system
* But is impossible in an asynchronous system


**(1) FIFO ordering**

Messages sent by the **same node** must be delivered in the order they were sent. When a node sent  a message, it will recieve the information itselt.

Possible Order:
* (m2, m1, m3) 
* (m1, m2, m3)
* (m1, m3, m2)

**Implementation of FIFO ordering over basic multicast**

We discuss FIFO ordered multicast with operations
FO-multicast and FO-deliver for non-overlapping groups.
It can be implemented on top of any basic multicast.

Each process p holds:
* S_p a count of messages sent by p to g.
* R_q the seqyebce bynver if the latest message to g that p delivered from q.

Process p FO-multicast a message to group g, it piggybacks S_p on the message.
* B-multicasts it and increments S_p by 1.

Process p received the message from q with sequence number S.
* p checks whether S = R_q + 1, if so, FO-deliver it.
* if S > R_q + 1, process p place the message in hold-back queue until intervening message has been delivered.




**(2) Causal ordering**

If a message's broadcast happens before another message's broadcast, all the nodes must deliver the two messages in order. If the two messages braodcasts at the same time, the other nodes can order them in any order.

Causally related messages must be delivered in causal order.
Concurrent messages can be delivered in any order.

Possible Order:
* (m1, m2, m3)
* (m1, m3, m2)

If C received m2 before m1, the m2 will be hold back in the buffer, until m1 is delivered.


**Causal Ordering using vector timestamps**

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/ts.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/ts.png" align="center"></a>
    <figcaption>The algorithm for causal ordering.</figcaption>
</figure>

After delivering a message from p_j , process p_i updates its vector timestamp 
* By adding 1 to the j th element of its timestamp.

Compare the vector clock rule where  V_i[ j]:=max(V_i[ j], t[ j]) for j=1,2,...,N.
* In this algorithm we know that only the j th element will increase.

If we use **R-multicast instead of B-multicast** then the protocol is reliable as well as **causally ordered**.

 If we combine it with the sequencer algorithm we get **total and causal ordering**.




**(3) Total ordering(Atomic broadcast)**

All nodes must deliver messages in the same order, this includes a node's deliveries to itself.

In FIFO ordering and Causal Ordering, when a node deliver message to itself, it can deliver the message directly to itself and have no need to wait for others' communication. However, in total ordering, this issue is not set up.

**Implementation of Totally Ordered Multicast**

Attack **Totally Ordered Identifiers** to multicast messages.
* Each receiving process makes ordering decisions based on the identifiers 
*  TO-multicast and TO-deliver

There are two methods to implement total ordered multicast over basic multicast
* Using a sequencer (only for non-overlapping groups)
* The processes in a group collectively agree on a sequence number for each message

**Sequencer**

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/sequencer.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/sequencer.png" align="center"></a>
    <figcaption>The implementation using a sequencer.</figcaption>
</figure>

**The sequence numbers are defined by a sequencer, we have total ordering.**

Like B-multicast, if the sender does not crash, all members receive the message.

Kaashoek’s protocol uses hardware-based multicast
* The sender transmits one message to **sequencer**
* The sequencer multicasts the sequence number and the message
* But IP multicast is not as reliable as B-multicast so the sequencer stores messages in its history buffer for retransmission on request
* Members notice messages are missing by inspecting sequence numbers

**Group collectively agree on a sequence number**

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/da.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/da.png" align="center"></a>
    <figcaption>The distributed algorithm for total ordering.</figcaption>
</figure>

Each Process, q keeps:
* A_q the largest agreed sequence number it has seen
* P_q its own largest proposed sequence number

a. Process p B-multicasts [m, i] to g, where i is a unique identifier for m.

b. Each process q replies to the sender p with a proposal for the message's agreed sequence number of
* P_q := Max(A_q, P_q) + 1
* assigns the proposed sequence number to the message and places it in its hold-back queue.

c. p **collects all the proposed sequence numbers** and selects the **largest** as the next agreed sequence number a.
* It B-multicasts [i, a] to g.
* Recipients set A_q := Max(A_q, a) and **attach a to the message and re-order hold-back queue**.


Hold-back queue ordered with the message with the smallest sequence number at the front of the queue.

When the agreed number is added to a message, the queue is re-ordered.

When the message at the front has an agreed id, it is transferred to the delivery queue
* Even if agreed, those not at the front of the queue are not transferred.

**Every process agrees on the same order and delivers messages in that order**, therefore we have total ordering. 

It has higher latency than the sequencer method.


## Conclusion:

(1) In a replicated server system, the communications among **the front end (FE)** and the **replicas are implemented using UDP/IP (instead of TCP/IP)** in order to

Avoid unnecessary marshalling and unmarshalling

(2) Since all the replicas in an actively replicated server system execute a set of client requests in total order,

A is strongest.

1.the data in every replica will be identical after **every client request is processed**

2.the data in every replica will be identical after all the client requests is processed

3.the data in every replica will be identical after a specific client request is processed

4.the data in every replica will be identical after none of the client requests is processed

(3) The replicas in an **actively replicated server system** can only guarantee sequentially consistent data because

The **replicas** may **execute client request** at **different times**.

(4) In a **passively replicated primary-backup server system**, the backups should perform the data updates send by the primary in: FIFO order.

(5) In a replicated server system, a software failure cannot be detected using a heart beat signal because: 

the heart beat does not indicate **the result of a client request**.

(6) While an **actively replicated server system** is recovering from a software failure
* another software failure cannot be detected
* another crash failure can be tolerated with reliable failure detection

(7) In a replicated server system, each replica maintains a local copy of the application data instead of a single global copy to achieve
* concurrent data access in multiple copies
* correct data access
* consistent data access

(8) In an actively replicated server system, a client request should be sent to the replica using reliable multicast to faciliate
* software failure detection

(9) A passively replicated primary-backup server system cannot detect a software failure in one of the replicas because
* multiple results for a request are required




## 3. Time and Global States

### 3.1 Conceptions

Leslie Lamport suggested that we should reduce time to its basics.
* If A happened before B, TIME(A) < TIME(B)
* If TIME(A) < TIME(B), A happened before B

“A happens before B”, written A→B
* A→PB according to the local ordering
* A is a send and B is a receive and A→MB
* A and B are related under the transitive closure of rules (1) and (2)

### 3.2 Clocks

#### 3.2.1 Logical Clocks

A simple tool that can capture parts of **the happens before relation**.
* Designed for big (64-bit or more) counters
* Each process p maintains LT_p, a local counter
* A message m will carry LT_m

When an event happens at a process p it increments LT_p. Any event that matters to p. Normally, also send and receive events.

When p sends m, set LT_m = LT_p.

When q receives m, set LT_q = max(LT_q, LT_m)+1.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/lc.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/lc.png" align="center"></a>
    <figcaption>The logical time and logical clocks.</figcaption>
</figure>

If A happens before B, A→B, then LT(A)< LT(B).

But converse might not be true: If LT(A)< LT(B) can’t be sure that A→ B.
* This is because processes that don’t communicate still assign timestamps and hence events will “seem” to have an order.

#### 3.2.2 Vector Clocks

Clock is a vector: e.g. VT(A)=[1, 0]
* Vector clocks require either agreement on the numbering, or that the actual process id’s be included with the vector.
* **When event happens at p, increment VT_p[ index_p]**. also increment for send and receive events.
* When sending a message, set VT(m)=VT_p.
* When receiving, set VT_q=max(VT_q, VT(m)).



<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/vc.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/vc.png" align="center"></a>
    <figcaption>The vector clocks.</figcaption>
</figure>


We’ll say that VT_A ≤ VT_B if
* ∀I, VT_A[i] ≤ VT_B[i]
* [2, 4] ≤ [2, 4]

And we’ll say that VT_A < VT_B if
* VT_A ≤ VT_B but VT_A ≠ VT_B
* That is, for some i, VT_A[i] < VT_B[i]
* [1, 3] < [7, 3]

**[1, 3] is “incomparable” to [3, 1]**.

If A→B, then VT(A)< VT(B).

If VT(A)< VT(B) then A→B.

Otherwise A and B happened concurrently.


### 3.3 Detecting Global Properties


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/gp.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/gp.png" align="center"></a>
    <figcaption>The global properties.</figcaption>
</figure>



### 3.4 Simultaneous Actions

#### 3.4.1 Simultaneous Actions

Think about updating replicated data
* Perhaps we have multiple conflicting updates
* The need is to ensure that they will happen in the same order at all copies
* This “looks” like a kind of simultaneous action

#### 3.4.2 Temporal Distortions

Things can be complicated because we can’t predict.
* Message delays (they vary constantly)
* Execution speeds (often a process shares a machine with many other tasks)
* Timing of external events


<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/TD.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/TD.png" align="center"></a>
    <figcaption>The temporal distortions.</figcaption>
</figure>


#### 3.4.3 Consistent Cuts and Snapshots

Idea is to identify system states that “might” have occurred in real-life
* Need to avoid **capturing states** in which a message is received but **nobody is shown as having sent it**.
* This the problem with the gray cuts.

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/CC.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/CC.png" align="center"></a>
    <figcaption>The consistent cuts.</figcaption>
</figure>


### 3.5 The Snapshot Algorithm


**Records a set of process and channel states** such that the combination is a consistent GS.

Assumptions:
* No failure, all messages arrive intact, exactly once
* Communication channels are **unidirectional and FIFO-ordered**
* There is a **comm. path between any two processes**
* Any process may initiate the snapshot (sends Marker)
* Snapshot does not interfere with normal execution
* Each process records its state and the state of its incoming channels (no central collection)

<figure>
    <a href="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/sa.png"><img src="https://raw.githubusercontent.com/OneSilverBullet/SilverGamer.GitHub.io/gh-pages/_img/ds/sa.png" align="center"></a>
    <figcaption>The snapshot algorithm.</figcaption>
</figure>


In practice we only record things important
to the application running the algorithm, not
the “whole” state. E.g. “locks currently held”, “lock release
messages”.

Idea is that the snapshot will be
* Easy to analyze, letting us build a picture of the
system state
* And will have everything that matters for our real
purpose, like deadlock detection




