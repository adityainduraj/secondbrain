---
time created: 2024-08-22 09:23
tags:
  - academics
  - os
references: 
source:
---
## Process Concept
An OS executes a variety of programs: 
- Batch system - **jobs**
- Time-shared systems - **user programs** or **tasks**

**Process** - a program in execution, process execution must progress in sequential fashion. 

It has multiple parts: 
- **The Program Code** - also called the *text section*
- Current activity including **program counter**, processor registers
- **Stack**
- **Data Section**
- **Heap** containing memory dynamically allocated during runtime


> [!NOTE] Program vs Process
> A program is a **passive** entity, stored on disk in the form of an executable file.
> A process is an **active** entity. A program becomes a process when the executable file is loaded into the memory. 

One program can be several processes. (Consider the case of multiple users executing the same program). 

## Process State
As a process executes, it changes its **state**. Below are some of the states: 
1. **new** - the process being created
2. **running** - instructions are being executed
3. **waiting** - the process is waiting for some event to occur
4. **ready** - the process is waiting to be assigned to a processor
5. **terminated** - the process has finished execution

## Process Control Block (PCB)
Information associated with each process (also called **task control block**)
- Process State
- Process Number
- Program Counter
- CPU Registers
- CPU Scheduling Information
- Memory Management Information
- Accouting Information
- I/O Status Information

## Schedulers
1. **Short-Term Scheduler** - Selects which process should be executed next and allocates CPU. It is sometimes the only scheduler in a system.
2. **Long-Term Scheduler** - Selects which processes should be brought into the **ready queue**. Long-term scheduler is invoked infrequently. It controls the degree of multiprogramming. 
Processes can be described as either: 
1. **I/O-bound process** - spends more time doing I/0 than computations, many short CPU bursts
2. **CPU-bound process** - spends more time doign computations, few very long CPU bursts. 

The long-term scheduler strives for a good **process mix**. 

### Medium Term Scheduler
A medium-term schedular can be added if degree of multiple programming needs to decrease. 
Remove process from memory, store on disk, bring back in from disk to continue execution. 

## Context Switch
When CPU switches to another process, the system must **save the state** of the old process and load the **saved state** for the new process via a **context switch**. 

The **context** of a process is represented in the PCB. The context-switch time is an overhead, as the system does no useful work during switching. 

## Process Creation
Parent processes create children processes who have their own children and so on, until a **tree** of processes is formed. 
Generally, processes are identified and managed using a **process identifier** or **pid**. 

Some options for resource sharing: 
1. Parent and children share all resources.
2. Children share a subset of parents resources.
3. Parent and children share no resources. 

Some options for execution: 
1. Parent and chidlren execute concurrently. 
2. Parent waits until children terminate. 

### UNIX Examples: 
1. `fork()` - this system call creates a new process
2. `exec()` - this system call is used after a `fork()` to replace the process' memory space with a new program

## Process Termination
Process executes the alst statement and then asks the operation system to delete it using the `exit()` system call. 
1. Returns status data from child to parent (via `wait()`). 
2. Process' resources are deallocated by the operating system. 

Parents may terminate the execution of children processes using the `abort()` system call. Some reasons for doing so: 
1. Child has executed allocated resources. 
2. Task assigned to child is no longer required. 
3. The parent is exiting and the operating systems does not allow a child to continue if parent terminates. 

**Cascading Termination** - If a parent process is terminated, all of its children and grandchildren etc processes are terminated as well. 

The parent process may awit for termination of a child process by using the `wait()` system call. This call returns status information and the pid of the terminated process. 
```c
pid = wait(&status)
```

- If no parent is waiting (did not invoke `wait()`), the process is a **zombie**. 
- If the parent terminated without invoking `wait()`, the process is an **orphan**. 


> [!NOTE] Cooperating Processes - Advantages
> 1. Information Sharing
> 2. Computation speed-up
> 3. Modularity
> 4. Convenience

## Message System
Mechanisms for processes to communicate with each other without resorting to shared variables. 

The IPC facility provides two operations: 
1. `send(message)`
2. `receive(message)`

We can use **direct communication** for this to handle these messages **explicitly**: 
1. `send(p, message)` - send a message to process P
2. `receive(q, message)` - receive a message from process Q

**Indirect Communication** utilizes **mailboxes**. It sends a message to a mailbox, from which another process can collect/receive it. 
If mailboxes are being shared by more than process, how do we decide who gets the message? 
1. Allow a link to be associated with at most two processes
2. Allow only one process at a time to execute a receive operation
3. Allow the system to arbitrarily select the receiver. Sender is notified who the receiver was. 

### Synchronization:
Message passing may be either blocking or non-blocking. 
**Blocking** is considered *synchronous*. 
**Non-Blocking** is considered *asynchronous*. 

If both send and receive are blocking, we have a **rendezvous**. 


> [!NOTE] Buffering
> A queue of messages attached to the link. Can be of zero, bounded or unbounded capacity


