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

##  Process Termination

When a process completes execution, it must be terminated properly to free up system resources like memory and file descriptors. Termination can occur either because the process has finished its execution or because it has been terminated by another process (typically its parent).

**Normal Process Termination**:
- A process calls the `exit()` system call to indicate that it has finished execution.
- The operating system deallocates the resources the process was using, such as CPU, memory, and open files.
- **Status Data**: If the process is a child process, it can return status data (like an exit code) to its parent process via the `wait()` system call. This exit status is important as it tells the parent whether the child process finished successfully or encountered an error.

```c
pid_t pid;
int status;
pid = wait(&status);  // Parent waits for the child process to finish and retrieves status information.
```

**Forced Termination by Parent (abort)**:
A parent process can forcefully terminate a child process using the `abort()` system call. This might happen under several circumstances:
- The child process exceeds its allocated resources (e.g., memory or CPU time).
- The task assigned to the child is no longer required.
- The parent process itself is terminating, and the operating system does not allow orphaned child processes (i.e., children cannot outlive their parent).

**Cascading Termination**:
In some systems, if a parent process terminates, all of its child processes must also terminate. This is called **cascading termination**, where the termination of the parent leads to the termination of all its descendants (children, grandchildren, etc.). This ensures system stability by preventing orphaned processes from continuing without supervision.

- If a process is terminated without invoking the `wait()` system call, the child process enters a **zombie** state. A zombie process is one that has completed execution but still has an entry in the process table so the parent can read its exit status.
- If the parent process terminates before the child, the child becomes an **orphan**. The operating system assigns orphans to the `init` process, which handles them.

---

# Interprocess Communication (IPC)

**Process Types**:
- **Independent processes**: Do not share data or communicate with other processes. They function autonomously without any interference.
- **Cooperating processes**: Can interact and affect each other’s execution. These processes are essential for tasks that require collaboration between multiple processes, such as sharing information, speeding up computations, modularity, and convenience.

**IPC Models**:
Two main models allow processes to communicate with each other:
1. **Shared Memory**: 
   - In shared memory, processes share a section of memory. The operating system facilitates the creation of this shared space, but the processes are responsible for managing access (ensuring synchronization) to avoid race conditions.
   - It is faster than message passing since data doesn't need to be copied between processes; instead, they all access the same memory location.
   - However, synchronization mechanisms (like semaphores or mutexes) are necessary to ensure that multiple processes don’t modify the same data concurrently, which could lead to inconsistent states.

2. **Message Passing**:
   - This mechanism involves the direct exchange of messages between processes, without the need for shared variables.
   - Two key operations are involved: `send(message)` and `receive(message)`. Depending on implementation, messages may have fixed or variable sizes.
   - Message passing can be **synchronous** (blocking) or **asynchronous** (non-blocking). Synchronous communication requires both sender and receiver to be ready at the same time, while asynchronous communication allows the sender to continue without waiting for the receiver.
   
**Advantages and Disadvantages of IPC Models**:
- **Shared Memory**: 
  - **Advantages**: Fast as there is no need for system calls once the memory is shared.
  - **Disadvantages**: Complexity in synchronization and ensuring that processes access memory in a controlled manner.
  
- **Message Passing**:
  - **Advantages**: Simpler to implement and manage, especially for communication between processes on different machines.
  - **Disadvantages**: Slower, as messages must be sent and copied through the kernel.

## Cooperating Processes

Processes that interact with one another can provide several benefits:
- **Information Sharing**: Allows processes to share data and work collaboratively.
- **Computation Speedup**: By dividing tasks among multiple processes, the overall execution time can be reduced (parallelism).
- **Modularity**: Breaking a large task into smaller cooperating processes enhances modularity and simplifies debugging and maintenance.
- **Convenience**: In some cases, it is easier to express complex problems as cooperating processes.

## Producer-Consumer Problem (A Classic Example of Process Cooperation)

This problem illustrates the typical challenge faced by cooperating processes. The **producer** process generates data (e.g., files, images) that is consumed by the **consumer** process.

- **Unbounded Buffer**: The buffer size is theoretically unlimited, and the producer can continue to produce data without waiting for the consumer.
- **Bounded Buffer**: The buffer has a fixed size, and the producer must stop producing once the buffer is full until the consumer has consumed some data. Synchronization between producer and consumer is key to avoid **race conditions**.

## Analogies and Learning Tools

- **Zombie Processes**: Think of a zombie process as a document that has been written and closed, but the system is still holding it open in case the creator (parent process) wants to read the document's status (exit code).
  
- **Orphan Processes**: Imagine a teacher (parent process) assigning homework (task) to a student (child process). If the teacher leaves the school without checking the homework, the homework becomes an "orphan," but another teacher (the init process) might take over to check it.

- **Shared Memory**: It's like multiple chefs working in the same kitchen. They all have access to the same set of ingredients (shared memory), but they need to coordinate (synchronization) to avoid messing up a dish.

- **Message Passing**: Picture two people sending letters (messages) to each other. One person writes a letter and waits for a response (synchronous), or they send letters and don’t wait for a reply before sending the next (asynchronous).

**Further Learning Tools**:
- Explore these concepts practically using operating system simulators like **Nachos** or **Xv6** to see how processes are created, synchronized, and terminated.
- Use tools like **GDB** (GNU Debugger) to track process states and behaviors during execution in real-time.

This structured approach should provide a clearer understanding of process termination and interprocess communication (IPC) models.