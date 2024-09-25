---
time created: 2024-09-21 19:26
tags:
  - "#academics"
  - "#os"
references:
  - "[[Exam Schedules and Syllabus]]"
  - "[[OS - 02 - Processes]]"
follows:
---
> Some notes available over at [[OS - 02 - Processes]] that I made during Quiz prep. 

---

# Concept of a Process
A **process** is a program in execution, which forms the basis of all computation. Think of a process as a moving car on a highway, where the program is the car itself, but the execution (process) is the car in motion. Also, just like cars can switch lanes, processes can change states during execution. 

## Key Elements of a Process
- **Text Section**: The program code.
- **Program Counter**: Keeps track of which instruction to execute next
- **Stack**: Stores temporary data such as function parameters, return addresses and local variables. 
- **Data Section**: Contains global variables. 
- **Heap**: Memory dynamically allocated during runtime (like objects created-on-the-fly). 

> [!NOTE] Program vs Process
> A program is a **passive** entity, stored on disk in the form of an executable file. 
> A process is an **active** entity. A program becomes a process when the executable file is loaded into the memory. 

> One program can be several processes. Consider the case of multiple users executing the same program.
---
# Process State
As a process runs, it can be in one of several states:
- **New**: The process is being created. 
- **Running**: Instructions are being executed. 
- **Waiting**: The process is waiting for some event (example -> I/O completion).
- **Ready**: The process is waiting for CPU time. 
- **Terminated**: The process has finished execution. 

---
# Process Control Block (PCB)
The **Process Control Block** contains all the information the operating system needs to manage the process, including: 
- Process State
- Process Number
- Program Counter
- CPU Registers
- CPU Scheduling Information
- Memory Management Information
- Accounting Information
- I/O Status

> Think of the PCB as a detailed report card for the process. It records what the process has done so far, what it is currently doing and what it needs to do next. 

---
# Threads
A **thread** is the smallest unit of execution within a process. Threads allow multiple tasks to be performed simultaneously within the same process. 
For example, a word processor may have one thread for typing, one for spell-check and another for auto-saving. 

> **Analogy**: If a process is a factory, then the threads are the assembly lines. Each thread focuses on a specific task, but together, they all contribute to making the final product. 

---
# Process Scheduling
Scheduling determines which process gets the CPU and when. There are three types of schedulers. 
1. **Long-term scheduler**: Selects which processes should be brought into the **ready queue**. Long-term scheduler is invoked infrequently. It controls the degree of multiprogramming. It is slow and infrequent. 
2. **Short-term scheduler**: Selects which process should be executed next and allocates CPU. It is sometimes the only scheduler in a system. It is fast and frequent. 
3. **Medium-term scheduler**: Removes processes from memory (swapping) to reduce the degree of multiprogramming. 
4. **I/O-bound process**: Spends more time doing I/O than computations; many short CPU bursts. 
5. **CPU-bound process**: Spends more time doing computations. Few very long CPU bursts.

## Scheduling Queues
1. **Job Queue**: All processes in the system. 
2. **Ready Queue**: Processes in memory, ready and waiting to run. 
3. **Device Queue**: Processes waiting for I/O devices. 

> **Analogy**: Imagine a restaurant where the scheduler is the manager deciding which orders (processes) are prepared first. The long-term scheduler manages which customers (jobs) are seated, the short-term scheduler decides which dish (process) is prepared next, and the medium-term scheuler moves customers to the waiting area 

---
# Context Switch
When the CPU switches from one process to another, the OS saves the state of the old process and loads the state of the new process. This switch is called a **context switch**. 

The **context** of a process is represented in the PCB. The context-switch time is an overhead as the system does no useful work during switching.

---
# Process Creation and Termination
## Process Creation
Parent processes create child processes who have their own children and so until, until a *tree* of processes is formed. 
Generally, processes are identified and managed used a **process identifier** or `pid`. 

Some options for resource sharing are:
1. Parent and children share all resources.
2. The children share a subset of the parent's resources.
3. Parent and children share no resources.

Some options for execution are:
1. Parents and children execute concurrently. 
2. Parent waits until the children terminate.
### UNIX Examples:
1. `fork()`: This system call creates a new process. 
2. `exec()`: This system call is used after a `fork()` to replace the process's memory space with a new program. 
