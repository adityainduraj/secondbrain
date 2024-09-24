---
time created: 2024-09-21 17:14
tags:
  - academics
  - os
references:
  - "[[Exam Schedules and Syllabus]]"
follows: "[[Chapter 3 - Processes]]"
---
---
# Objectives of Multithreading
Multithreading is used to enhance the efficiency and performance of applications by allowing multiple threads to execute concurrently. The main objectives include: 
- **Responsiveness**: If a thread performing a lengthy task blocks, other threads can continue to execute, maintaining the application's responsiveness. 
- **Resource Sharing**: Threads of the same process share memory, files and other resources, allowing for efficient data exchange without the need for inter-process communication. 
- **Economy**: Thread creation requires fewer resources compared to process creation. Context switching between threads is less expensive because threads share the same memory and resource space. 
- **Utilization of Multiprocessor Architectures**: Multithreading allows better use of multi-core processors, enabling threads to run in parallel on multiple cores. This improves performance and throughput. 

---
# Process Creation vs Thread Creation

## Process Creation
- A process is an independent execution unit with its own memory space. 
- Process creation involves duplicating the memory space, which is computationally expensive. 
- Example -> `fork()` system call in UNIX systems. This is slower due to the overhead of memory allocation. 
## Thread Creation
- A thread is a lightweight process that shares the process's memory space and resources. 
- Creating a new thread is faster than creating a new process because threads share memory and resources, reducing overhead. 
- Example -> Thread creation in POSIX systems uses `pthread_create()`. 

> **Analogy**: Imagine processes as separate houses (each with its own kitchen, bathroom, etc.) while threads are like roommates living in the same house and sharing facilities. 

---
# Kernels and Multithreading
The **kernel** is responsible for managing system resources, including [[Chapter 5 - CPU Scheduling]], memory and I/O. In multithreading:
- **Thread Management**: The kernel can manage threads by allocating CPU time, handling synchronization and managing communication between threads. 
- **Kernel Threads**: These are threads managed directly by the kernel. The kernel has full control over the thread's scheduling and execution. Switching between kernel threads involves a context switch, but it's more efficient than switching between processes. 
- **User Threads**: These are threads managed at the user level (example -> by a user-level thread library). The kernel is unaware of user threads, so scheduling is handled in user space. They have lower overhead but may not leverage multiple CPU cores efficiently without kernel intervention. 
---
# Multithreaded Server Architecture
In a **multithreaded server**, multiple threads are used to handle requests simultaneously. There are two common approaches:
1. **Thread-per-request**: The server spawns a new thread for every incoming client request. Each thread handles a client connection independently. This allows simultaneous handling of many clients without the overhead of creating a new process for each. 
2. **Thread Pool**: The server maintains a pool of worker threads that can handle incoming requests. When a new request arrives, an available thread from the pool is assigned to handle the request. This reduces the overhead of frequent thread creation. 

> **Analogy**: Imagine a restaurant with waiters. Threads-per-request is like hiring a new waiter every time a customer walks in. The customer gets an assured waiter with personal attention, but it takes time to hire one. Thread Pool is like having a fixed number of waiters, with the customer being assigned a waiter from the pool. It cuts down on the hiring time but if all waiters are busy, some customers might encounter a waiting period. 

---
# User Threads vs Kernel Threads

| USER THREADS                                                                                    | KERNEL THREADS                                                                          |
| ----------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| Managed by a thread library in user space, without kernel awareness.                            | Managed at the operating system kernel.                                                 |
| Lightweight and fast, but cannot leverage multi-core processors unless supported by the kernel. | Can leverage multiple cores, and kernel schedules each thread independently.            |
| If one user thread blocks a system call, all threads in that process may be blocked.            | Heavier than user threads in terms of overhead because kernel involvement is required.  |
> **Analogy**: *User Threads* are like workers in a small office who manage their own tasks without outside help. *Kernel Threads* are like workers in a factory here the manager (the OS) oversees everything and helps coordinate tasks. 

In short, user threads are managed by the program, while kernel threads are managed by the operating system. 

---
# Multithreading Models
## 1. Many-to-One Model
- Multiple user threads are mapped to a single kernel thread.
- The OS can only manage one thread at a time per process, leading to inefficiencies on multi-core systems. 
- Example -> Some early versions of JVM used this model. 

## 2. One-to-One Model
- Each user thread maps to a unique kernel thread.
- Allows true parallelism on multiprocessor systems. 
- Overhead is higher due to the number of kernel threads. 
- Example -> Windows and Linux support this model. 

## 3. Many to Many Model:
- Multiple user threads map to a smaller or equal number of kernel threads. 
- Combines the benefits of the above 2 models. The OS schedules kernel threads to run on multiple cores while user-level libraries manage user threads. 
- Example -> Solaris and modern Linux distros implement this model. 


> [!NOTE] Two-Level Model
> This is a hybrid of the [[#3. Many to Many Model]] where a user-thread can be bound to a dedicated kernel thread for direct execution for special cases, offering a flexible combination of parallelism and threading efficiency. It allows both the efficient management of user threads and the performance benefits of kernel threads. 

---
# Thread Libraries
Thread libraries provide the API for creating and managing threads. Popular libraries include:
1. **Pthreads (POSIX Threads)**: Standardized thread library for UNIX-like systems. It provides functions for thread creation, synchronization (using mutexes) and communication. 
2. **Win32 Threads**: Kernel-level library for Windows systems. 
3. **Java Threads**: Part of the Java runtime, allowing thread creation and management in a platform-independent way. 
---

# Pthreads (and an example)
**Pthreads (POSIX Threads)** is a widely used API for creating and managing threads in UNIX-like operating systems. It provides a variety of thread maangement functions like creating threads, joining threads and synchronizing them. API specifies behavior of the thread library, implementation is up to the development of the library. 

### Example of `pthread` code:
```c
# check if code is in syllabus and insert here
```
