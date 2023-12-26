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

## Client Server Web Example

- Client sends a request to the server.
- Server sends a response to the client.
