---
time created: 2024-09-18 09:10
speaker: Amit Gurkha, Sr Director, Cisco Systems, Data Centre Business Group
tags:
  - "#academics"
  - "#interviews"
references:
  - "[[01 - Acads]]"
  - "[[Exam Schedules and Syllabus]]"
follows: "[[CN - Guest Lecture - Day 1]]"
---
# Principles of Reliable Data Transfer
#incompleteNote ...
## Getting Started
We will:
- incrementally develop sender, receiver sides of **reliable data transfer** protocol **(rdt)**. 
- consider only unidirectional data transfer (but control info will flow in both directions). 
- use finite state machines (FSM) to specify sender, receiver. 

> [!NOTE] FSM Example
> The switch on, off analogy. Flipping the switch is the *event*. 

## rdt1.0: reliable transfer over a reliable channel
- underlying channel perfectly reliable
	- no bit errors
	- no loss of packets
- separate FSMs for sender, receiver
	- sender sends data into underlying channel
	- receiver reads data from underlying channel

## rdt2.0: channel with bit errors
- underlying channel may flip bits in packet
	- checksum to detect bit errors. 
- the question: how to recover from errors? 
	- **acknowledgements(ACKs):** receiever explicitly tells sender that pkt received OK
	- **negative acknowledgments(NAKs):** reciever explicitly tells the sender that pkt had errors. 
	- sender **retransmits** packet on receiving NAKs. 
- adds a *checksum* while creating the packet for error determination
### rdt2.0: fatal flaw
What happens if the ACK/NAK gets corrupted? The sender doesn't know what happened at receiver, cna' tjust retransmit: possible duplicate. 
**Handling Duplicates:** 
- Sender retransmits current pkt if ACK/NAK gets corrupted
- sender adds *sequence number* to each pkt. 
- reciever discards (doesn't deliver up) duplicate pkt.

### rdt2.2: NAK free protocol
If you get a duplicate ACK, you retransmit the current packet (just like NAK). 
### rdt3.0: channels with errors *and* loss
**Approach:** the sender waits for a "reasonable" amount of time for ACK.
- retransmits the packet with lossy ACK, if no ACK received in this time
- if the ACK is sent after timeout, it resends ACK, detects duplicate (the one that arrived late) and ignores one of the ACKs and proceeds as normal (refer to the FSM diagram in the PPT) #delveDeeper 
#### Pipelined Protocols Operation
**Pipelining:** sender allows multiple, "in-flight", yet-to-be-acknowledges packets
It starts sending all the packets, and say in the middle one packet is lost it sends a request to resend. Meanwhile hte following packets that have arrived are placed in a buffer. Once the issue is resolved, they are picked up from the buffer and made whole with the rest of the received packets. 

With this approach, the delay is a lot better. 

### Go-Back-N: 
When it encounters a lossy packet, it requests for it to be resent, and discards all subsequent packets it receives in the meantime.  #delveDeeper 

---
## TCP: Connection-Oriented Transport

