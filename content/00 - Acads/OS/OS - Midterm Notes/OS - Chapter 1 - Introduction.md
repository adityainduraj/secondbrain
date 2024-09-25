---
time created: 2024-09-25 18:49
tags:
  - academics
  - os
references:
  - "[[Exam Schedules and Syllabus]]"
follows:
---
# What is an Operating System? 
An **Operating System** (henceforth shortened to **OS**) is a program that **manages a computer's hardware**. It acts as an intermediary between a user of a computer and the computer hardware. 

Some operating systems are designed to be convenient while others are designed to be efficient, and others are a combination of both. 

## Goals of an Operating System
1. Execute the user's prgorams and make solving user problems easier. 
2. Make the computer system easier and more convenient to use. 
3. Use the computer hardware and resources in an efficient manner. 

## Operating Systems - Definition
The **OS** is a **resource allocator**. It manges all resources and decides between confilctign requests for efficient and fair resource use. 

The **OS** is a **control program**. It controls the execution of programs to prevent errors and improper use of the computer. 

This is not a universally accepted definition however. A more appropriate definition is:
> Everything that a vendor ships when you order an operating system. 

The one program that runs on your system the entire time is the [[Kernel]]. Everything else is either a **system program** (ships with the OS) or an **application program**. 

## Structure of a Computer System
A computer system can be broadly divided into four components. 
1. **Hardware**: This provides the basic computing resources suc has CPU, memory, I/O. 
2. **Operating System**: Controls and coordinates the use of hardware among various applications and users. 
3. **Application Programs**: Define the ways in which the system resources are being used to solve the user's computing problems. 
4. **Users**: Peoples, machines, other computers utilising the system. 


> [!info]- Computer Startup
> The **bootstrap program** is loaded at power-up or reboot. It is typically stored in ROM or EPROM, generally known as **firmware**. It initializes all the aspects of the system. 
> It **loads the operating system kernel** and starts the execution

---
# What do Operating Systems do? 
Well, it depends on the point of view, namely, the **User View** or the **System View**. 
- Users want convenience, ease of use and good performance. They generally don't care about resource utilization. But a shared computer such as a **mainframe** or a **mini computer** must keep all users happy. 
- Users of dedicated systems such as **workstations** have dedicated resources but frequently use shared resources from **servers**. 
- *Handheld computers* are resource poor, optimized for usability and battery life. Some computers like *embedded computers* have little to no user interface, like the computers in devices and automobiles. 

---
# Structure of an Operating System
## Multiprogramming (Batch System)
This is requried for efficiency. A single user cannot keep the CPU and I/O busy at all times. Multiprogramming organizes jobs so that the CPU always has one to execute. 

A subset of total jobs in the system is kept in memory. One job is selected and run via **job scheduling**. When it has to wait, the OS switches to another job. 

## Timesharing (Multitasking)
**Timesharing** is a logical extension in which the CPU switches jobs so frequently that users can interact with each job whiel it is running, creating **interacting computing**. Some more points about interactive computing:
- The response time *has* to be less than a second. 
- Each user has at least one program executing in memory -> process.
- If several jobs are ready at the same time -> [[OS - Chapter 5 - CPU Scheduling]]. 
- If processes don't fit in memory, **swapping** moves them in and out to run. 
- **Virtual Memory** allows for execution of processes not completely in memory. 

---
# Operating Systems - Operations
OS Operations are **interrupt driven**. These interrupts can be both hardware (think of a hardware interrupt by one of the devices) or software (think of a code error, a request for an OS service or other process problems like infinite loops etc).

**Dual-Mode Operations** allows the OS to protect itself and other system components. This is done by having a **User Mode** and a **Kernel Mode**. The mode bit is provided by hardware. 
- It provides the ability to distinguish when the system is running user code or kernel code. 
- Some instructions are designated as **privileged**, so that they're only executed in kernel mode. 
- A system call changes the mode to kernel, a return from call resets it to user. 
- Increasingly, most CPUs support multi-mode operations. 

---
# Process Management
First, we need to understand the distinction between a **program** and a **process**. 
It's quite simple. A **program** is a *passive entity*, while a process is an *active entity*. 

> A **process** is a program in execution. 

A process requires resources such as CPU memory, I/O, initialization data etc to accomplish its task. **Process Termination** requires a reclaiming of any reusable resources.

A single-threaded process has one **program counter** specifying the location of the next instruction to execute. The process executes instructions sequentially, one at a time, until completion. 

Multi-threaded processes have one program counter *per thread*. Typically, a system has many processes, some user process, some OS processes, all running concurrently on one or more of the CPUs. We achieve **concurrency** by multiplexing the CPUs among the processes. 

## Process Managment Activities
The operating system is responsible for the following activities in connection with process management:
1. Creating and deleting both user and system processes. 
2. Suspending and resuming processes. 
3. Providing [[OS - Chapter 6 - Process Synchronization]] mechanisms. 
4. Providing [[OS - Chapter 7 - Deadlocks]] handling mechanisms. 
5. Providing mechanisms for process communication. 

![[os-process-management-flowchart.png]]

---
# Memory Management
## Mass Storage Management
- Usually consists of disks that store data that does not fit into the main memory or data that needs to be preserved for a 'long' period of time.
- Proper management is of central importance. 
- The entire speed of the computer operation hinges on the disk subsystem and its algorithms. 
---



