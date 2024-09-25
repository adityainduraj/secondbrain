---
time created: 2024-09-17 11:57
tags:
  - academics
  - os
references:
  - "[[Exam Schedules and Syllabus]]"
follows: "[[OS - Chapter 6 - Process Synchronization]]"
---
---
## 1. System Model
The system model provides a framework for understanding how deadlocks can occur in a system where multiple processes compete for resources. Each process interacts with system resources in 3 stages: **request**, **use** and **release**. These stages describe how processes acquire and release resources, which is crucial for understanding potential deadlock scenarios. 
1. **Processes:** Active programs that require resources to function. They typically follow this cycle when interacting with a resource:
	- **Request:** The process asks for a resource. If the resource is available, the system allocates it to the process. If not, the process must wait until the resource becomes available. 
	- **Use:** Once the process has the resource, it uses it for the required operation (example -> writing to a file or printing). 
	- **Release:** After completing its task, the process releases the resource, making it available for other process to **request**. 
2. **Resources:** These can be:
	- **Preemptable Resources:** These can be taken away from a process without any negative impact (example -> CPU).
	- **Non-Preemptable Resources:** These cannot be forcibly taken away (example -> printers, files), which can lead to deadlocks if not managed properly. 
3. **[[#Resource Allocation Graph]]:** This graph visually represents the relationships between processes and resources:
	- **Nodes** represent processes and resources.
	- **Edges** represent the allocation and requests:
		- A directed edge from a process to a resource indicates a **request**. 
		- A directed edge from a resource to a process represents an **allocation**. 

---
## 2. Deadlock Characterization
A deadlock occurs when a set of processes are blocked, each waiting for a process held by another process in the set. 
Four conditions must hold simultaneously for deadlocks to happen:
- **Mutual Exclusion:** Only one process can use a resource at a time. 
- **Hold & Wait:** Process holding resources can request more resources without releasing their current ones. If the requested resource is held by another process, it will wait for its completion.
- **No preemption:** Resources cannot be forcibly taken away from processes. 
- **Circular Wait:** A closed chain of processes exists, where each process holds a resource that the next process in the chain is waiting for. For example, consider that P1 is waiting on a resource held by P2, who is waiting on P3, and P3 is waiting on P1. 
### Resource Allocation Graph
A visual representation, consisting of a set of vertices $V$ and a set of edges $E$. 
$V$ is partitioned into 2 types, a set $P$ that consists of all processes in the system and a set $R$ which consists of all **resource types** in the system. 

An edge from a process to a resource is a **request** edge, whereas an edge from a resource to a process is an **allocation** or **assignment** edge.
An edge that represents a *request for resources* is indicated by a dotted line, and is called a **claim edge**. 
#### RAG - Syntax #delveDeeper 
1. **Process:** Represented with a circle. 
2. **Resource Type with 4 Instances:** A square containing 4 smaller squares/dots. 
3. **P<sub>i</sub> requests instance of R<sub>j</sub>:** A circle P<sub>i</sub> pointing to a resource square R<sub>j</sub> with 4 smaller squares/dots within it. 
4. **P<sub>i</sub> is holding an instance of R<sub>j</sub>:** One of the 4 smaller squares/dots within R<sub>j</sub> is pointing to the circle P<sub>i</sub>. 

> [!NOTE] Revision Tip
> Check the PPT and properly practise deadlock identification on example graphs. 
### Basic Facts:
1. If a graph contains no cycles -> **no deadlock**. 
2. If a graph contains a cycle:
	1. If only one instance per resource type -> **deadlock**. 
	2. If several instances per resource type -> **possibility of deadlock**. 
---
## 3. Methods for Handling Deadlocks
There are several strategies for handling deadlocks:
- **Deadlock Ignorance:** In many systems (including UNIX), deadlocks are simply ignored, assuming that they are rare and resolved manually. 
- **Deadlock Detection and Recovery:** The system monitors for deadlocks, and once detected, uses strategies to recover (example -> aborting process or preempting resources). 
- **Deadlock Prevention:** Proactively ensure that at least one of the four necessary conditions for deadlock **cannot occur**. 
- **Deadlock Avoidance:** Dynamically ensure that a system will never enter a deadlock state. 

---
## 4. Deadlock Prevention
Deadlock prevention ensures that a system never enters a deadlock state by violating one of the necessary conditions:
- **Mutual Exclusion:** Cannot be avoided for non-shareable resources, but it can be reduced by limiting mutual exclusion to only non-preemptable resources. 
- **Hold & Wait:** Must guarantee that whenever a process requests a resource, it does not hold any other resources. This can be done by requiring processes to request all required resources at one or by requiring processes to release all held resource before requesting new ones. 
- **No Preemption:** If a process that is holding some resources requests another resource that cannot be immediately allocated to it, then all resources currently being held are released. Preempted resources are added to the list of resources for which the process is waiting and the process will be restarted **only when** it can regain its old resources **as well** as the new ones that it is requesting.  
- **Circular Wait:** Impose a total ordering on all resources and force processes to request resources in a specific order.

---
## 5. Deadlock Avoidance
In deadlock avoidance, the system makes decisions dynamically, based on the current resource allocation and future resource requests. The idea is the ensure that the system never enters an **unsafe state**, where deadlocks can potentially occur.

The most well-known deadlock avoidance algorithm is:
- **Bankers Algorithm:** Based on resource allocation and requests, it checks if grating a process's resource request will leave the system in a safe state. If not, the request is denied.

Deadlock avoidance requires that the system has some additional *a priori* information available:
- The simplest and most useful model requires that each process declare the **maximum number of resources** of each type that it might need. 
- The deadlock-avoidance algorithm dynamically examines the resource-allocation state to ensure that there can never be a circular-wait condition. 
- The resource-allocation **state** is defined by the number of available and allocated resources, and the maximum demands of the processes. 

### What is a **Safe State**?
A system is considered to be in a **safe state** if there exists a specific order in which all processes can complete without causing a deadlock. This sequence guarantees that, even if some processes have to wait for resources, they will eventually get the resources they need and terminate. 
**Key Points:**
1. **Safe Sequence:** There must exists a sequence of processes `<P1, P2, ... ,Pn>` (where `n` is the total number of processes) such that each process `Pi` in the sequence can finish its execution by acquiring the resources it needs. 
2. **Resource Availability:** 
	- For each process **P<sub>i</sub>** in the sequence, the system must have enough **currently available resources** or be able to gather enough resources after other processes finish. 
	- Specifically, the resources **P<sub>i<//sub>** needs can be provided by:
		- The **currently available resources** in the system. 
		- Resources held by processes **P<sub>j</sub>** (with `j<i`), meaning those processes before **P<sub>i</sub>** in the sequence have already finished and released their resources.

**Detailed Scenario:**
1. **P<sub>j</sub> (j < i)** is executing and has locked some resources. 
2. The system is checking if the next process, **P<sub>i</sub>** , can execute in the future. To do this, it considers: 
	1. **What's currrently available** (resources not being used by any process). 
	2. **What resources will become available** after **P<sub>j</sub>** finishes and releases its resources. 
3. **P<sub>j</sub>** will finish **before P<sub>i</sub>** (because it's earlier in the sequence), so the resource that **P<sub>j</sub>** is currently holding will be released only **after** it completes its task. 
4. Once **P<sub>j</sub>** finishes and releases its resources, those resources will **then** become available for **P<sub>i</sub>** to use. However, until then, those resources are still "held" and not part of the **currently available resources**. 

---
## 6. Avoidance Algorithms
Avoidance algorithms are used to ensure that a system never enters a deadlock state by carefully managing the allocation of resources. These algorithms dynamically allocate resources to processes in a way that avoids potential deadlocks. 

**Key Concepts:**
- **Resource Allocation:** Resources are allocated to processes in a way that ensures system safety. 
- **Safe State:** A state where there is a sequence of processes that can complete without causing deadlock. 
- **Deadlock Avoidance:** Ensures the system stays in a safe state by considering resource requests and allocations. 

### Banker's Algorithm
The Banker's Algorithm is used to allocate resources to processes in a way that avoids deadlock. It checks if resource allocation will keep the system in a safe state or not. 

**Data Structures for Banker's Algorithm:**
- **Available:** A narray representing the number of available units of each resource type. 
- **Max:** A matrix where `Max[i][j]` indicates the maximum number of resources of type `j` that process `i` **might need**. 
- **Allocation:** A matrix where `Allocation[i][j]` represents the number of resources of type `j` that is **currently allocated** to `i`. 
- **Need:** A matrix where `Need[i][j]` represents the remaining resource of type `j` that process `i` still needs to complete.

### Safety Algorithm #mustRevise
The Safety Algorithm determines whether a system is in a safe state by checkign if there exists a safe sequence of processes. 
**Steps:**
1. **Initialize:** 
	- Set `Work` to the number of currently available resources. 
	- Set `Finish` for each process to `false`. 
2. **Find a Process:**
	- Find a process `Pi` that can finish with `Work`. 
	- If such a process is found, update `Work` by adding the resources allocated to `Pi`. 
	- Set `Finish[i]` to true. 
3. **Repeat:**
	- Repeat the process until all processes are marked as `Finish` or no suc hprocess can be found. 
4. **Check Safe State:**
	- If all processes can be marked as `Finish`, the system is in a safe state. 
	- If not all processes can be marked as `Finish`, the system is not in a safe state.

### Resource-Request Algorithm #mustRevise 
The Resource-Request Algorithm is used to determine whether a process's request for resources can be granted without leading the system to an unsafe state.
#### **Steps:**
1. **Request Evaluation**:
    - When a process makes a request, check if the request is less than or equal to the `Need` matrix and less than or equal to the `Available` resources.
2. **Simulate Allocation**:
    - Pretend to allocate the requested resources and update the `Available`, `Allocation`, and `Need` matrices.
3. **Safety Check**:
    - Use the Safety Algorithm to determine if the system remains in a safe state after the simulated allocation.
4. **Grant or Deny**:
    - If the system is in a safe state, grant the request.
    - If not, deny the request and leave the system in its original state.