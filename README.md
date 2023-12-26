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
