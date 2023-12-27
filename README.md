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
