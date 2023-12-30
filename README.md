# Distributed Systems Notes

Martin Kleppman's distributed systems lecture.

# Introduction

- Multiple computers working together to achieve a common goal.
- Distributed systems are everywhere.
- Distributed systems are hard to get right.

## Why make a distributed system?

- Scalability: handle more load by adding more machines.
- Fault tolerance: continue working even if some nodes fail.
- Latency: do a computation closer to the data, or closer to the user who requested it.
- Huge amounts of data can not fit one machine

## Challenges

- Partial failure: some nodes fail, but others keep working.
- Concurrency: multiple clients may access the same data at the same time.
- Unreliable networks: messages may be delayed, lost, or delivered more than once.

# Computer Networking

- Fundamental abstraction: Sending one message from one node to another.
- Latency: time it takes for a message to travel from one node to another.
- Bandwidth: maximum amount of data that can be sent in a fixed amount of time. Time until message arrives.
- Throughput: actual amount of data that is sent in a fixed amount of time. Data volume per unit time.

## Packet explained

- Packet: a chunk of data sent over the network.
- A message consists of one or more packets.

## Client Server Web Example

- Client sends a request to the server.
- Server sends a response to the client.

# Remote Procedure Calls

- Remote Procedure Call (RPC): a client calls a function on a remote server.
- RPC is a way to make a network call look like a local call.
- The implementation of RPC function is on another node.
- **Marshalling:** converting data structures or objects in one programming language into another. This is done by the RPC library. The RPC library on the client converts the parameters into a byte sequence, and the RPC library on the server converts the byte sequence back into parameters.

# The two generals problem

- Two generals are trying to coordinate an attack on a city.
- They are camped on opposite sides of the city.
- They can only communicate by sending messages through the city.
- They must attack at the same time, or they will lose.
- They must agree on a time to attack.
- If one general sends a message to the other general, he does not know if the message was received. Except if the other general sends a message back. But the message back could also be lost.
- More messengers increase the probability of success, but does not guarantee it.

- General 1 always attacks -> loses if message does not go through.
- General 1 only attacks if he receives a message from general 2 -> However, now general 2 does not know if the message was received and is in the same situation as general 1.

## Distributed systems

This is a problem in distributed systems. The generals are nodes in a distributed system. There is no common knowledge.

# The Byzantine Generals Problem

- Problem: Some of the generals may be traitors.
- The traitors may try to prevent the loyal generals from reaching agreement.
- For example: general 1 sends a message to general 2 to attack, but general 2 tells general 3 to retreat.
- We want all honest generals to agree on the same plan of action.
- We want to tolerate up to `f` traitors.
- We need `3f + 1` generals to tolerate `f` traitors. This means that we need more than 2/3 of the generals to be honest.
- Another approach would be to use cryptography to sign messages. Cryptography means that the traitors can not forge messages.

# System Models

- In real systems, both nodes and network may fail.
- Capture assumptions in a system model consisting of:
  - Network Behaviour (e.g. message loss)
  - Node behaviour (e.g. crashes)
  - Timing behaviour (e.g. message delay)

## Network behaviour

- Assume bidirectional point to point communication between nodes

We can assume reliable links, fair loss links and arbitrary loss links.

Reliable links -> strong assumption.

Fair loss links -> messages are not corrupted, but may be lost.

Arbitrary loss links -> messages may be corrupted or lost. Malicious behaviour.

Network Partition: a set of nodes is partitioned from the rest of the network. This means that the nodes in the partition can not communicate with the nodes outside the partition. A partition in human language is a network split.

## Node behaviour

Each node executes a specific algorithm, assume one of following:

- Crash-stop: node may crash and may recover, but does not send or receive messages after recovery.
- Crash-recovery: node may crash and may recover, and may send or receive messages after recovery.
- Byzantine: node may behave arbitrarily, including sending conflicting messages to different nodes.

A node that is not faulty is called correct.

## Synchrony (timing) assumptions

Assume one of the following:

- Synchronous: Messages are delivered within a known bounded time. This means that we know how long it takes for a message to be delivered.
- Partially synchronous: Messages are delivered within an unknown but bounded time. This means that we do not know how long it takes for a message to be delivered, but we know that it will be delivered within a certain time.
- Asynchronous: Messages are delivered after an unknown time, and there is no bound on how long it may take. This means that we do not know how long it takes for a message to be delivered, and we do not know if it will be delivered at all.

Note: sync and async are not the same as synchronous and asynchronous programming.

### Violations of synchrony

Networks usually have quite predictable latency, which can occasionally increase:

- Message loss requiring retry
- Congestion/Contention causing queuing, this means that messages are waiting in a queue to be sent.
- Network/route reconfiguration

Nodes usually execute at a predictable speed, which occasionally slows down:

- Operating system scheduling issues, e.g. a process is waiting for a resource.
- Stop the world garbage collection pauses, this means that the garbage collector pauses the program to clean up memory.
- Page faults, this means that the program is waiting for data to be loaded from disk into memory.

# Fault Tolerance

## Availability

- Availability: the proportion of time that a system is in a functioning condition.
- Availability = uptime = fraction of time that the system is working.
- "Two nines" availability: 99% availability = 3.65 days downtime per year.
- "Three nines" availability: 99.9% availability = 8.76 hours downtime per year.
- "Four nines" availability: 99.99% availability = 52.56 minutes downtime per year.
- "Five nines" availability: 99.999% availability = 5.26 minutes downtime per year.

**Service Level Agreement (SLA):** a contract between a service provider and a customer that specifies the availability of the service. This means that the service provider guarantees a certain availability.

Service Level Objective (SLO): a target value or range of values for a service level that is measured by a service level indicator. This means that the service provider aims for a certain availability. For example, 99.9% of requests in a day get a response in 200ms.

## Achieving high availability: fault tolerance

- Failure: System as a whole isn't working.
- Fault: Some part of the system isn't working. But the system as a whole is still working.
- Single point of failure: a fault in a single component can cause the entire system to fail.

## Failure detectors

- Failure detector: a process that monitors the availability of a node.
- Perfect failure detector: a failure detector that always detects a node that has crashed.

Problem: cannot tell the difference between a node that has crashed and a node that is slow.

## Failure detection in partially synchronous systems

Perfect timeout based failure detector exists only in synchronous systems. It's because we know how long it takes for a message to be delivered. If we don't know how long it takes for a message to be delivered, we can't know if a node has crashed or is just slow.

This leads to eventually perfect failure detector: a failure detector that eventually detects a node that has crashed:

- May temporarily label a node as crashed, but will eventually correct itself.
- May temporarily label a node as working, but will eventually correct itself.

# Physical time

## Clock and time in distributed systems

Time is measured:

- Schedulers, timeouts, failure detectors, etc.
- Performance metrics, e.g. latency, throughput, etc.
- Logging: record the time when an event happened.
- Data with time limited validity, e.g. certificates, cookies, etc.

We distinguish between physical time and logical time:

- Physical clock: count number of seconds since some fixed point in the past.
- Logical clock: count number of events since some fixed point in the past.

## Quartz clocks

- Quartz clocks: use a quartz crystal to keep time.
- Quartz crystal is a piezoelectric material: it vibrates at a fixed frequency when an electric current is applied.
- The nice thing about quartz clocks is that they are cheap and accurate.
- How it works: the crystal vibrates at a fixed frequency. The frequency is divided down to 1Hz. The 1Hz signal is used to increment a counter. The counter is used to increment a counter for seconds, minutes, hours, etc.

### Quartz clock drift

- Quartz clocks are not perfect.
- They drift: they run slightly faster or slower than the correct time.
- Drift measured in parts per million (ppm).
- 1ppm = 1 microsecond per second. = 86.4ms per day. = 31.6 seconds per year.
- Most computers have a quartz clock that drifts by 50ppm.

## Drift definition

- Drift: the difference between the actual time and the time measured by the clock. In simple language: how much the clock is off or wrong.

## Atomic clocks

- Atomic clocks: use the oscillations of atoms to keep time. Oscillations of atoms are very stable, because they are not affected by temperature, pressure, etc.
- Atomic clocks are very expensive, but very accurate.

## GPS as time source

- GPS satellites have atomic clocks.
- GPS receivers use the time signal from the satellites to calculate their position.

## Coordinated Universal Time (UTC)

- UTC: the international standard for time.
- Greenwich Mean Time (GMT): the time at the Greenwich meridian (0 degrees longitude).
- International Atomic Time (TAI): the time measured by atomic clocks.
- GMT is based on the rotation of the earth, and is not very accurate.
- TAI is based on atomic clocks, and is very accurate. It's more accurate than the rotation of the earth, because the rotation of the earth is slowing down.
- The compromise is UTC: UTC is based on TAI, but has leap seconds to keep it in sync with GMT. Leap seconds mean e.g. that 23:59:60 is a valid time.

## Leap seconds

- Leap seconds are added to UTC to keep it in sync with GMT.
- Leap seconds are added at the end of June or December.
- Leap seconds are announced 6 months in advance.

## How computers represent time

Two most common representations:

- Unix time: number of seconds since 1970-01-01 00:00:00 UTC.
- ISO 8601: 2018-09-23T10:00:00Z, ISO 8601 is a standard for representing dates and times.

# Clock synchronization

- Computers track physical time using quartz clocks.
- Due to clock drift, clocks on different computers will be out of sync. This means that the clocks will show different times.
- Clock skew: the difference between the clocks on two computers.

With clock sync, we want to minimize clock skew.

Solution: Periodically get the time from a server that has a more accurate clock, e.g. atomic clock or GPS receiver.

## Network Time Protocol (NTP)

NTP is a protocol for synchronizing clocks on computers over a network.

**How it works in short:** periodically ask a server for the time, and adjust the local clock to match the server's clock.

Hierarchy of clock servers arranged in strata:

- Stratum 0: atomic clocks, GPS receivers, etc.
- Stratum 1: servers that get time from stratum 0 servers.
- Stratum 2: servers that get time from stratum 1 servers.

Strata means layers.

**Estimating time over a network in short:** NTP client sends a request to the NTP server. The NTP server sends a response to the NTP client. The NTP client calculates the time it took for the request to be sent and the response to be received. The NTP client adjusts the local clock to match the server's clock.

Once the client has estimated the clock skew, it needs to apply the skew gradually. This is called clock slewing, when the clock is adjusted gradually.

## Precision Time Protocol (PTP)

PTP is a protocol for synchronizing clocks on computers over a network.

How it works in short: periodically ask a server for the time, and adjust the local clock to match the server's clock. PTP is more accurate than NTP, because it uses hardware timestamps.

## Monotonic and time of day clocks

- Time of day: Time since a fixed date e.g. 1970-01-01 00:00:00 UTC. May suddenly move forwards or backwards. Used when you care about the actual time of day.

- Monotonic: Time since an arbitrary point in the past. Monotonic means that the time always increases. Good for measuring elapsed time on a single node. Used when you care about the duration of an event.

# Causality and happens-before

In a distributed system, ordering of messages is not guaranteed. This means that we can not assume that messages are received in the same order that they are sent.

Even if we use timestamps, timestamps may not be accurate. This means that we can not assume that messages are received in the same order that they are sent.

## Happens before relation

An event is something happening at one node. An event can be sending a message, receiving a message, or a local event.

We say event a happens before event b if:

- a and b are on the same node, and a is before b in the program order.
- a is sending a message and b is receiving the same message.
- a happens before b and b happens before c.

However, it is possible that a and b are concurrent. This means that a and b are not ordered.

## Causality

Causality is a partial order. This means that some events are ordered, and some events are concurrent.

# Logical time

Timestamp may be inconsistent with the order of messages.

Logical time is a way to assign a timestamp to an event. The key is they capture causality. The happens before relation.

## Lamport clocks

Every node has a counter initialized to 0. Every time an event happens, the counter is incremented. Every time a message is sent, the counter is sent with the message. Every time a message is received, the counter is updated to the maximum of the counter and the counter in the message + 1.

To uniquely identify events, we can combine the counter with the node id. This means that we can have multiple events with the same counter.

## Vector clocks

The limitation of lamport clocks is that they can not tell the difference between two events that are concurrent. Because they only have one counter.

- **Structure**: Each node in a distributed system maintains a vector (an array) of counters, one for each node in the system.
- **Updating Counters**: When a node performs an action, it increments its own counter in the vector. When it sends a message, it includes its entire vector. Upon receiving a message, a node updates each element in its vector to be the maximum of the received vector and its own.

- **Event Ordering**: Vector clocks provide a partial ordering of events as opposed to a total ordering provided by Lamport clocks. This means they can distinguish not only if one event causally affects another but also if events are independent and occurred in parallel.

# Broadcast ordering

Broadcast ordering in distributed systems refers to the methods used to ensure a consistent order of message delivery across all nodes when messages are broadcasted.

This is crucial for maintaining data consistency and coordinated behavior in the system.

1. **FIFO (First In, First Out) Order**:

   - Messages from a single sender are delivered in the order they were sent.
   - Ensures that if one node sends multiple messages, they won't be received out of sequence.

2. **Causal Order**:

   - Messages are delivered in a way that respects the causal relationships between them. Causal relationship refers to the idea that one message must be delivered before another if the first message causally influences the second.
   - If one message causally influences another (e.g., a reply to a previous message), the second message is not delivered before the first.

3. **Total Order (or Global Order)**:
   - All nodes receive all messages in the same order.
   - It’s the most stringent form of ordering, ensuring complete consistency across the system but often at the cost of higher overhead and reduced performance.

Different distributed systems may require different types of broadcast ordering depending on their specific needs for consistency and performance.

# Broadcast algorithms

Broadcast algorithms in distributed systems are methods for sending messages to many computers or nodes at once.

1. **Basic Broadcast**: Send a message to each node one by one. Easy but can be slow and cause a lot of network traffic.

2. **Flooding**: When a node gets a message, it sends it to all its neighbors. This spreads the message widely but can lead to too many copies of the same message.

3. **Gossip Protocol**: Nodes randomly pick other nodes to send the message to. It's like spreading a rumor - eventually, many nodes get the message without overloading the network.

4. **Multicast Broadcasting**: Send messages only to a specific group of nodes. It's like sending a group email only to certain people.

5. **Reliable Broadcast**: Make sure if one node gets a message, all nodes will eventually get it. This is important for not losing any messages.

6. **Ordered Broadcast**: Make sure all nodes get messages in the same order. This helps keep everything organized and consistent.

Each method has its own benefits and is chosen based on what the network needs, like speed, reliability or keeping messages in order.

# Replication

Replication is the process of storing the same data on multiple machines. It is used to increase data availability and fault tolerance.

A node that stores a copy of the data is called a replica.

1. **Making Copies**: Replication involves creating exact copies of data or software services. This means having the same information stored on more than one computer.

2. **Improving Reliability**: If one machine fails, the others can take over, because they have the same data or service. This is like having backup singers ready in case the lead singer loses their voice.

3. **Faster Access**: Having copies in different locations means users can access data from the nearest source, making things faster and reducing delays.

4. **Balancing Load**: When many users are accessing the same service, replication allows the load to be spread across multiple machines, preventing any single one from becoming overloaded.

5. **Synchronization**: It's important to keep all the copies updated and consistent. When data changes in one place, it needs to be updated in all the other places too.

# Quorums
