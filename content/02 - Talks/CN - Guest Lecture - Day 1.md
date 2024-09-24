---
time created: 2024-09-11 09:11
speaker: Amit Gurkha, Sr Director, Cisco Systems, Data Centre Business Group
tags:
  - "#academics"
  - "#interviews"
references:
  - "[[01 - Acads]]"
  - "[[Exam Schedules and Syllabus]]"
source:
---
# Transport Layer: Roadmap
1. [[#Transport Services and Protocols]]
2. [[#Multiplexing and Demultiplexing]]
3. [[#UDP User Datagram Protocol]]
4. [[CN - Guest Lecture - Day 2#Principles of Reliable Data Transfer]]
5. Connection-Oriented Transport: TCP
6. Principles of Congestion COntrol
7. TCP Congestion Control
8. Evolution of Transport-Layer Functionality

# Transport Services and Protocols
Provide **logical communication** between application processes running on different hosts.
Transport protocols actions in end systems:
- **Sender** - Breaks application messages into ==*segments*==, passes to network layer
- **Receiver** - Reassembles segments into messages, passes to application layer. 

> [!NOTE] Interview Info #interviews 
> 1. Segment -> Transport Layer
> 2. Packet -> Network Layer
> 3. Frames -> Data Link Layer


### Analogy - Household
There are 12 kids in Ann's house who are sending letters to 12 kids in Bill's house. In this case, the:
1. Hosts = Houses
2. Processes = Kids
3. App Messages = Letters in Envelopes
4. **Transport Protocol** = Ann and Bill who demux to in-house siblings.

## Transport Layer Actions
Steps that the Sender and Receiver follow for transport:
1. The sender takes the application and forms application message. 
2. The sender then determines segment header fields values. (Relate to a postal service analogy, how the postal service will put your envelope in their own envelope and send it from IPS Blr to IPS Delhi).
3. The **segment** (segment header + application message) is pushed to the next layer, **Network (IP)**. 
4. The reciever receives the segment from the IP, checks the header values and extracts the application-layer message. 
5. It then **demultiplexes** the message up to application via sockets.

## 2. Principal Internet Transport Protocols
### **TCP** - Transmission Control Protocol
- Reliable, in-order delivery. 
- Congestion Control
- Flow Control
- Connection Setup
### **UDP** - User Datagram Protocol
- Unreliable, unordered delivery.
- No-frills extension of "best-effort" IP. 
### Services *Not* Guarantees
1. Delay Guarantees
2. Bandwidth Guarantees

> [!NOTE] Interview Question - Why would you use an unreliable protocol? #interviewQuestion
> It's the convenience vs reliability debate, if you consider the Uber vs rentals analogy for the same.
> In video streaming, speed is paramount, and giving a late frame after the fact wouldn't add to the experience in any way, which is why UDP works perfectly.

# Multiplexing and Demultiplexing
**Multiplexing** is like a bunch of service roads **merging** onto the main road. **Demultiplexing** is the converse.
An example, again in Uber, Lyft and other ride-hailing apps is **ride-sharing** or **carpooling.**

A **socket** can be understood as a particular space in memory specially allocated for access. #delveDeeper

In **Demultiplexing**, the reciever checks the headers to properly allocate the messages to their intended sockets.
### How Demultiplexing Works:
The host receives the IP Datagrams. 
- Each datagram has source IP and destination IP. 
- Each datagram carries one transport-layer segment
- Each segment has source, destination port number. #incompleteNote
### Connection Oriented Demultiplexing
UDP just uses desitnation port no. A TCP Socket is identified by 4-tuple:
1. Source IP Address
2. Source Port No.
3. Destination IP Address
4. Destination Port No.

> [!NOTE] Interview Question - Standard Port Numbers #interviewQuestion 
> Basic questions like these are asked so that the interview can decide whether to go into more detail or just skip it entirely if the candidate can't answer the basic one.

> Pick a sub-specialty and go very deep into that specialty. #interviewAdvice

# UDP: User Datagram Protocol
This is a **no frills**, **bare-bones** Internet transport protocol. 
It's a **best-effort** service. The UDP segments may be: 
1. Lost
2. Delivered out of order 
### Connectionless:
- No handshaking between UDP sender and receiver
- Each UDP segment handles independently of others
### Why is there a UDP? 
- No connection establishment (which can add RTT delay)
- Its simple, no connection *state* at sender, receiver
- Small header size
- No congestion control
	- UDP can blast away as fast as desired
	- Can function in the face of congestion
### UDP Use:
1. Streaming multimedia apps (loss tolerant, rate sensitive)
2. DNS
3. SNMP
4. HTTP/3
### However, if reliable tansport is needed over UDP...
- Add needed reliability at transport layer #incompleteNote 

> For UDP Transport Layer actions refer to PPT #incompleteNote 

## Internet Checksum
**Goal:** Detect errors (such as flipped bits) in a transmitted segment. 
We add both numbers and taking the 1's complement of the sum. 

