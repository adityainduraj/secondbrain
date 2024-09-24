---
time created: 2024-09-18 15:40
tags:
  - academics
  - examNotes
references:
  - "[[Exam Schedules and Syllabus]]"
  - "[[01 - Acads]]"
source:
---
> [!NOTE] What's a protocol? 
> **Protocols** define the **format** and **order** of messages sent and received among network entities, and the **actions taken** on message transmission and receipt. 

---
# 1. Network Edge
The network edge refers to the devices and systems that sit at the outermost part of the network, primarily the **hosts**, **access networks** and **physical media**. 
## Hosts
- **Definition**: Hosts are the end devices that users interact with (example -> computers, smartphones). 
- **Role:** They generate and consume data, and are often referred to as *end systems* because they sit at the edge of the network. 
- **Client-Server Model:** Common network architecture where clients request services (example -> web browsers) from servers (example -> web servers). 
- **Peer-to-Peer Model:** In contrast, hosts in this model both request and provide services (example -> file-sharing networks like BitTorrent). 

## Access Networks
Access networks connect individual hosts to the first router (often called the edge router) on the network. There are several types of access networks:

### 1. Cable-based Access
- **Description:** Uses coaxial cables to deliver broadband internet. 
- **Technology:** Data Over Cable Service Interface Specification (DOCSIS) is used for data transfer. 
- **Bandwidth:** Speeds range from 10 Mbps to 1Gbps. 
- **Example:** Commonly used in residential areas for Internet access. 

### 2. Digital Subscriber Line (DSL)
- **Description:** Delivers Internet over traditional telephone lines. 
- **Technology:** Modems convert the digital data to frequencies that can travel over phone lines. 
- **Bandwidth:** Usually offers asymmetrical speeds (example -> 24Mbps download, 1Mbps upload). 
- **Example:** Widely used in rural and suburban areas. 

### 3. Wireless Access Networks
- **Wi-Fi:** Provides short-range wireless connectivity, typically used in homes, offices and public spaces. 
- **Cellular Networks:** Longer-range access networks like 4G, 5G; used for mobile Internet. 
- **Bandwidth:** Depends on the wireless standard and coverage (example -> 4G offers up to 100Mbps speeds, whereas 5G offers upto 1Gbps). 

### 4. Enterprise Networks
- **Description:** Used within large organizations to interconnect computers, servers and other devices. 
- **Components:** Typically includes switches, routers and firewalls. 
- **Access:** Often a combination of wired (Ethernet) and wireless (Wi-Fi) connectivity. 

### 5. Data Center Networks
- **Description:** Specialized networks designed to connect thousands of servers in a data center. 
- **Role:** High bandwidth, low latency networks that support cloud services and applications like Google Cloud, AWS, Azure. 

## Physical Media
The physical media is the atual path or medium over which the data travels between devices. It can be: 
1. **Guided Media** -> twisted pair cables, fiber optics. 
2. **Unguided Media** -> radio waves for wireless communication. 

---
# 2. Network Core
The network core is responsible for the transportation of data between the network edge devices, enabling data communication. 
## Core Functions
- **Routing:** Determines the path data packets will take through the network to reach their destination. It kinda decides on the entire source -> destination path beforehand. 
- **Forwarding:** Transfers packets from the router's input link to the appropriate output link based on the destination address. This doesn't have a set path, at each step it transfers the packets to the most appropriate output link. 

> [!NOTE] Routing vs Forwarding
> - **Routing:** The process of planning the entire trip (global). 
> - **Forwarding:** The process of moving through each intersection (local). 
## Packet Switching
- **Definition:** Data is broken into packets and each packet travels independently over the network. 
- **Advantage:** Efficient bandwidth usage since multiple packets can share the same transmission link. Packet switching allows multiple users to send packets over the same communication links. This efficient use of bandwidth ensures that resources aren't tied up when a user is idle on their assigned transmission link. Also, since the data is broken into packets that are sent independently, the network dynamically allocates resources on an as-needed basis. This allows the network to handle bursts of data from various sources, adjusting to fluctuating traffic more flexibly. 
- **Disadvantage:** Possible packet loss, delay, and out-of-order delivery due to congestion. 

> [!NOTE] **Packet Queuing & Loss**
At the routers, the packets are stored in **queues**. The problem with this is that when the router's queue is full, any incoming packets sent to the router may be dropped, causing packet loss. 

## Circuit Switching
- **Definition:** A dedicated communication path ("call") is established between two devices for the duration of the connection. 
- **Advantage:** Guaranteed bandwidth and zero queuing delay. 
- **Disadvantage:** Inefficient bandwidth usage as the dedicated path remains reserved even when no data is being transmitted. 

## Comparison: Circuit Switching vs Packet Switching

| **Criteria**            | **Packet Switching**                                 | **Circuit Switching**                                       |
| ----------------------- | ---------------------------------------------------- | ----------------------------------------------------------- |
| *Bandwidth Utilization* | More efficient, since it shares bandwidth.           | Less efficient, since bandwidth is reserved even when idle. |
| *Delay*                 | Variable, due to queuing and congestion.             | Fixed and predictable delay.                                |
| *Use Case*              | Best for bursty data (example -> Internet browsing). | Best for real-time data (example -> phone calls).           |
| *Cost*                  | Generally lower.                                     | Generally higher due to reserved resources.                 |
| *Scalability*           | Highly scalable.                                     | Limited scalability.                                        |
> With 35 users, the probability that more than 10 will be active at the same time in packet switching is less that $0.004$ (assume each user is active only 10% of the time). As a result, a packet-switching link can support upto 35 users simultaneously as opposed to circuit-switching which is capped at 10 simultaneous users. 

### Is Packet Switching a "Slam-Dunk" Winner? 
Well, before we answer this, we have a few things to consider. 
- Packet switching is great for 'bursty' data (inconsistent rates) because it is **simpler** (no call setup) and there is **resource sharing**. 
- However, **excessive congestion is possible** which leads directly to [[#Packet Delay and Loss]] due to buffer overflow. To combat this, we need protocols for reliable data transfer.
  ([[CN - Chapter 3 - Transport Layer#Principles of Reliable Data Transfer]]).

**How do we provide circuit-like behavior with packet switching?**
Well, it's complicated. We'll study various techniques that try to make packet switching as 'circuit-like' as possible to try and get the best of both worlds. 

## FDM vs TDM
- **Frequency Division Multiplexing (FDM)**: Different signals are transmitted simultaenously on different frequency channels. 
- **Time Division Multiplexing (TDM)**: Each signal is assigned a specific time slot for transmission over the same channel. 

## Internet Structure and Tiers
- **Tier-1 Commercial ISPs**: The top-level ISPs that can reach every other network on the Internet without paying for transit. Examples are Sprint, AT&T etc. that provide national and international coverage. 
- **Tier-2 Regional ISPs**: Regional ISPs that connect to Tier-1 ISPs and provide transit for end-users. 
- **Tier-3 ISPs**: Access ISPs that connect directly to end-users (example -> homes and businesses). 
- **IXPs (Internet Exchange Points)**: Physical infrastructure that allows different ISPs to exchange traffic. 
- **Content Provider Networks**: Companies like Google or Netflix that bypass ISPs directly to connect with end-users for improved performance. 
---
# 3. Performance
## Packet Delay and Loss
**Packet Delay**: The time taken for a packet to travel from source to destination, inclusive of all other delays. 
**Packet Loss** occurs when the queue is full and incoming packets have to be dropped.
There are 4 main sources of delay:
1. **Processing Delay**: The time taken to examine packet headers and figure out where to forward it. 
2. **Queuing Delay**: Time spent waiting in router queues due to congestion. 
3. **Transmission Delay**: Time taken to push all the bits of the packet onto the transmission link. 
4. **Propagation Delay**: Time taken for the signal to travel through the medium to its destination.
### Formulae:

$$
\text{d}_{nodal} = \text{d}_{proc} + \text{d}_{queue} + \text{d}_{trans} + \text{d}_{prop}
$$
Where, $\text{d}_{trans} = L/R$, and $\text{d}_{prop} = d/s$

### Caravan Analogy
Let's picture 3 toll booths, each a 100km away from each other. We have a caravan of 10 cars trying to cover the entire distance. In this analogy:
- Cars = Bits
- Caravan = A Packet
- Toll Booth = Link Transmission

Now, consider that a toll booth takes 12s to service a car (bit transmission time). The speed of each individual car is 100kmph (the *propagation speed*). 
So, the time taken to 'push' the entire caravan through a toll booth onto the highway will be $12*10=120s$. 

Thus, the we can calculate that the time taken for the last car to propagate from 1st to 2nd toll booth is 1hr, since it is travelling at 100kmph. 

Our question is, **how long until the caravan is lined up before the second toll booth?** 
Since it takes 120s or 2mins for the last car to reach the first toll booth, and an hour of travel, the time will be $60+2=62mins$ for the entire caravan to line up before the second toll booth. 

Suppose now, that the cars 'propagate' at 1000kmph, and it takes a minute to service each car. 
Our question is, **will cars arrive at the second booth before all cars are serviced at the first booth?**
Yes. Since it takes a minute to service each car, and the car can cover the distance to the second booth in a tenth-of-an-hour (i.e. 6 mins), in 7 minutes, the first car will arrive at the second booth. At this time, there will still be 3 cars waiting to be serviced at the first booth. 

### How do we determine when the packet queuing delay is permissible and when its getting out of hand? 
We use the following formula:
$$
(L.a)/R : (\text{arrival rate of bits}) / (\text{service rate of bits}) 
$$
Here,
- $a$ -> average packet arrival rate
- $L$ -> the packet length in bits
- $R$ -> the link bandwidth (bit transmission rate) or the rate at which bits are pushed onto the queue. 

If $(L.a)/R$ is:
1. Close to Zero -> our avg. queuing delay is small
2. Close to One -> our avg. queuing delay is large
3. More than One -> more 'work' is arriving faster than it can eb serviced, which means that the average delay is ∞. 

## Real Internet Delay & End-to-End Delay
The delay across the Internet is a combination of all four sources. 
$$
\text{d}_{end-end} = N(\text{d}_{proc} + \text{d}_{trans} + \text{d}_{prop})
$$
Where once again, $\text{d}_{trans} = L/R$ where $L$ is the packet size. 

### Traceroute
In Traceroute, the source sends packets to the destination, passing through multiple routers. Here's a simplified breakdown:

1. The source sends a series of packets, each labeled with a number from 1 to $N$ ($N$ is the number of routers plus the destination).
2. Each packet stops at a different router along the way. When a router receives a packet meant for it (packet 1 for the first router, packet 2 for the second, etc.), it sends a response back to the source instead of forwarding the packet. 
3. When the destination receives the final packet, it also responds to the source.
4. The source records the time it takes to get a response from each router and the destination, noting their addresses and response times.
5. This process is repeated three times to ensure accuracy, with the source sending 3 times the number of routers $3.n$ packets.
This helps the source trace the path and measure round-trip delays between itself and all routers along the route to the destination.

### Throughput
The rate at which data is successfully transferred from one place to another. #incompleteNote 

---
# 4. Security
### Packet Interception

- **Packet Sniffing**: Monitoring traffic for sensitive information (e.g., passwords).
- **IP Spoofing**: The attacker sends packets appearing to come from a trusted IP address.

### Denial of Service (DoS)

- **DoS Attack**: Flooding a target with traffic to exhaust resources and make the service unavailable to legitimate users.

### Lines of Defense

1. **Authentication**: Verifying the identity of a user or device.
2. **Confidentiality**: Ensuring data is accessible only to authorized entities (e.g., encryption).
3. **Integrity Checks**: Ensuring data has not been altered in transit (e.g., hash functions).
4. **Access Restrictions**: Limiting access to certain users or devices (e.g., access control lists).
5. **Firewalls**: Devices or software that filter traffic between trusted and untrusted networks.

---
## 5. Protocol Layers and Reference Models

## Layering

- **Definition**: Network functionality is divided into layers, where each layer provides services to the layer above and uses services of the layer below.
### Why Layering? 
It's a good approach to designing/dicussing complex systems:
- Explicit structure allows identification, relationship of system's pieces. 
- Modularization eases maintenance, updation of system. 
## Internet Protocol Stack
**5 Layers** (Read bottom-to-top):
1. **Physical**: Transmits raw bits over a medium.
2. **Data Link**: Establishes links and transfers frames. Data transfer between neighboring network elements, such as tranferring the network layer datagram from the *host* to a neighboring host. Adds its own header $\text{H}_l$.  
3. **Network**: Routes packets/datagrams across networks (IP protocol). This transfers the transport-layer segment from one *host* to another, using link-layer services. Adds its own header $\text{H}_n$. 
4. **Transport**: Ensures reliable data transfer (TCP/UDP). Process-process data transfer. This transfers M reliably over the network and adds its own header $\text{H}_t$ 
5. **Application**: Supports network applications (HTTP, SMTP). This contains the message $M$ that we are trying to send over the network.

## Layering and Encapsulation
- **Encapsulation**: As data moves down the layers, each layer adds its own header (and sometimes a trailer) to the data.
Here's what each layer actually sends:
1. **Application**: Message 
2. **Transport**: Segment
3. **Network**: Datagram
4. **Link**: Frame
5. **Physical**: The actual bits.

> A **switch** has just link and physical layers. 
> A **router** has just network, link and physical layers. 

---
