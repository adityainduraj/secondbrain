---
time created: 2024-09-18 18:32
tags: 
references: 
source:
---
>[[CN - Guest Lecture - Day 1]] for all topics before this. 

---
# Principles of Reliable Data Transfer
> Refer to [[CN - Guest Lecture - Day 2]] to build the notes here upon. 

Reliable Data Transfer (RDT) protocols are essential for ensuring that data is transferred correctly and efficiently over a network, particularly when the underlying transport medium can introduce errors. Here’s a structured overview of the various versions of RDT protocols:

---

## 1. **RDT 1.0: Reliable Transfer over a Perfect Channel**
- **Assumption**: The underlying channel is completely reliable, i.e., there are no bit errors or packet loss.
- **Working**: In this version, the sender simply sends data, and the receiver receives it without the need for any additional error-checking mechanisms.
- **Mechanism**: 
  - Simple unidirectional data transmission (from sender to receiver).
  - No need for acknowledgments (ACKs) or retransmissions, as errors are assumed not to occur.
  
Since this is a perfect-world scenario, there are no concerns for error control, but this is an unrealistic assumption for real networks.

---

## 2. **RDT 2.0: Reliable Transfer over a Channel with Bit Errors**
- **Assumption**: The channel may introduce bit errors during transmission.
- **Working**: To ensure reliability, the protocol must detect errors and recover from them.
- **Mechanism**:
  - **Error Detection**: Uses checksums to detect errors in the received packets.
  - **Feedback Mechanism**: 
    - **Acknowledgments (ACK)**: Sent by the receiver to confirm correct receipt of the packet.
    - **Negative Acknowledgments (NAK)**: Sent by the receiver when a packet is received with errors.
  - **Retransmission**: When a NAK is received, the sender retransmits the packet.

**Limitation**: Duplicate packets can occur if ACKs or NAKs themselves are corrupted or lost.

---

## 3. **RDT 2.1: Handling Duplicates in RDT 2.0**
- **Assumption**: Same as RDT 2.0 (channel introduces bit errors).
- **Working**: Extends RDT 2.0 to handle possible duplicate packets caused by corrupted ACKs or NAKs.
- **Mechanism**:
  - **Sequence Numbers**: 
    - Introduced to distinguish between new packets and retransmitted duplicates.
    - Typically uses a 1-bit sequence number (`0` or `1`) for alternating packets.
  - **Error Detection**: Same as RDT 2.0, using checksums.
  - **Retransmission**: If an ACK/NAK is corrupted, the sender will retransmit the packet. The sequence number allows the receiver to differentiate between retransmissions and new data.

---

## 4. **RDT 2.2: NAK-Free Protocol**
- **Assumption**: Same as RDT 2.0/2.1.
- **Working**: A variant of RDT 2.1 that eliminates the use of explicit NAKs to reduce overhead.
- **Mechanism**:
  - Instead of using NAKs, the receiver sends an ACK for the last correctly received packet, even if the current packet is corrupt.
  - The sender infers a problem if it receives multiple ACKs for the same sequence number and retransmits the missing packet.
  - **Sequence Numbers**: Still used to distinguish between new data and retransmissions.
  
This version improves efficiency by removing the need for NAKs.

---

## 5. **RDT 3.0: Reliable Transfer over a Lossy Channel with Errors**
- **Assumption**: The channel may introduce both bit errors and packet loss.
- **Working**: This version adds mechanisms to handle packet loss in addition to error detection and recovery.
- **Mechanism**:
  - **Error Detection**: Same as previous versions, using checksums.
  - **Acknowledgments (ACKs)**: Same as before, but now ACKs may also be lost.
  - **Timeouts and Retransmissions**: 
    - The sender sets a timer after sending each packet.
    - If an ACK is not received within the timeout period, the sender assumes the packet (or its ACK) was lost and retransmits the packet.
  - **Sequence Numbers**: Continue to be used to manage duplicates caused by retransmissions.
  
RDT 3.0 can handle both errors and packet loss, making it suitable for unreliable networks like the Internet.

---

## Summary of RDT Versions

| **RDT Version** | **Errors** | **Loss** | **Mechanism** |
|-----------------|------------|----------|---------------|
| **RDT 1.0**     | No         | No       | Simple transmission, no error handling needed. |
| **RDT 2.0**     | Yes        | No       | Checksums for error detection, ACK/NAK for feedback, retransmissions for errors. |
| **RDT 2.1**     | Yes        | No       | Adds sequence numbers to handle duplicate packets. |
| **RDT 2.2**     | Yes        | No       | NAK-free version, uses duplicate ACKs for retransmissions. |
| **RDT 3.0**     | Yes        | Yes      | Adds timeouts and retransmissions to handle packet loss, in addition to error handling. |

--- 

Each version of RDT builds on the previous one, adding mechanisms to address more complex network conditions, such as errors and packet loss, making the protocol more robust and suitable for real-world usage.

---
