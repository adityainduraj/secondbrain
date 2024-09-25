---
time created: 2024-09-20 15:24
tags:
  - academics
  - os
references:
  - "[[Exam Schedules and Syllabus]]"
follows: "[[OS - Chapter 5 - CPU Scheduling]]"
---
> While revising, it's recommended to skip right to [[#Differences]] for a tabular overview. 
---
# Background
In concurrent systems, multiple processes or threads might access shared resources simultaneously. Process synchronization is needed to manage this access and avoid issue such as **race conditions** where multiple processes attempt to modify shared resources concurrently, leading to unpredictable outcomes. 

## Producer-Consumer Problem
This is a classic synchronization problem where one process (the producer) generates data and another process (the consumer) uses that data. 
They share a common buffer, and their operations need to be synchronized to prevent data corruption. 
Given below is the code for each. 
#### Producer Code
```c
while (true) {
	/* produce an item in next produced */
	while (counter == BUFFER_SIZE) : // wait if the buffer is full
		/* do nothing */
	buffer[in] = next_produced; // shared circular buffer used for storage
	in = (in + 1) % BUFFER_SIZE  // updates 'in' to next available slot
	
	/* the modulo operation ensures that the index 'in' wraps around
	 when it reaches the end of the buffer (circular buffer behavior) */
	
	counter++; // counter keeps track of the number of items in the buffer
}
```
#### Consumer Code
```c
while (true) {
	while (counter == 0) : // wait if the buffer is empty
		/* do nothing */
	next_consumed = buffer[out];
	out = (out + 1) % BUFFER_SIZE // move the 'out' index forward
	counter--;
}
```

> The 'waiting till the buffer is full or empty' implementation above is not the most optimal approach, since the program is looping infinitely without doing any useful work. 
> In real implementations, a more efficient wait mechanism would be used (example -> blocking or using semaphores). 
#### Explanation
- **Producer**: Continuously produces items and places them in the buffer. It waits if the buffer is full (`counter == BUFFER_SIZE`), then inserts the item and updates the buffer position (`in`). 
- **Consumer**: Continuously consumes items from the buffer. It waits until the buffer is not empty (`counter == 0`), retrieves the item and updates the buffer position (`out`). 
- **Race Condition**: Without proper synchronization, both producer and consumer might simultaenously access shared variables like `counter` leading to inconsistent results. 

> [!NOTE] Race Condition
> A **race condition** occurs when multiple processes or threads attempt to modify shared data concurrently without proper synchronization. The outcome depends on the timing of their execution, often leading to erroneous or unpredictable results. 

---
## Critical Section Problem (and Solution)
A **critical section** is a part of the program where shared resources (like variables or data structures) are accessed. To avoid problems like race conditions, we must ensure that only one process enters the critical section at a time. 

### Solution using Locks
Locks are synchronization mechanisms that prevent multiple processes from accessing the critical section simultaneously. 

**Lock-based Solution**:
```c
acquire(lock);// before entering the critical section, acquire the lock

// critical section code here

release(lock); // after leaving the critical section, release the lock. 
```
Locks can lead to potential issues like [[OS - Chapter 7 - Deadlocks]] (if multiple processes are waiting for each other's locks) or **priority inversion** (if a lower-priority process holds the lock while a higher-priority process is waiting). 

---
# Peterson's Solution
Peterson's Solution is a software based method for ensuring mutual exclusion in critical sections, specifically for two processes. 

### Algorithm
```c
flag[i] = true; // indicate that process 'i' wants to enter critical sec.
turn = j; // give another process 'j' a chance to enter critical sec. 

while (flag[j] && turn == j);
// wait until j is done or has given up the turn

// critical section

flag[i] == false; // Indicate that process 'i' has left the critical section.
```

### Explanation
- Each process sets its flag to indicate its intention to enter the critical section. 
- The `turn` variable ensures that both processes don't enter the critical section simultaneously. A process waits if the other process also wants to enter and it's the other process's turn. 
- This solution works well for two processes, guaranteeing mtuual exclusion, progress and bounded waiting. 

--- 

# Synchronization Hardware
Modern processors don't depend on software solutions like [[#Peterson's Solution]] for process synchronization. They provide hardare support for process synchronization to ensure mutual exclusion with minimal overhead. 

Some common hardware mechanisms include:
### Test-and-Set:
```c
bool TestAndSet(bool *target) {
	bool rv = *target;
	*target = true;
	return rv;
}
```
**Explanation:** This atomic operation checks if a variable is true or false and sets it to true. It prevents other processes from entering the critical section while a process holds the ock. 

### Compare-and-Swap
```c
bool CompareAndSwap(int *value, int expected, int new_value) {
	int temp = *value;
	if (temp == expected)
		*value = new_value;
	return temp;
}
```
**Explanation**: This operation compares the contents of a variable with an expected value. If they match, it swaps the contents with a new value, ensuring mutual exclusion. 

These hardware solutions are efficient and avoid busy-waiting. 

---
# Mutex Locks
A **mutex** lock is a binary lock used to protect critical sections. 
## Basic Operations
1. `lock()` : Acquires the lock. preventing other processes from entering the critical section. 
2. `unlock()`: Releases the lock, allowing other processes to enter. 
### Example
```c
pthread_mutex_t lock;
pthread_mutex_lock(&lock); // acquire the lock
// critical section code here
pthread_mutex_unlock(&lock); // release the lock
```
## Usage
Mutex locks ensure that only one thread or process accesses shared resources at a time. They are widely used in multi-threaded programs for thread synchronization. 

---
# Semaphores

> [!NOTE] What is a Semaphore?
> A **semaphore** is a more advanced synchronization tool taht mutex locks. It can be used to control access to a resource by multiple processes. 
## Types of Semaphores
1. **Binary Semaphores**: Acts like a mutex lock with values 0 or 1. 
2. **Counting Semaphore**: Manages access to a resource that has multiple instances (example -> multiple buffer slots). 

## Basic Operations
- **`wait(semaphore)` (also known as `P` operation)**: Decreases semaphore value and waits if the value is 0. 
- **`signal(semaphore)` (also known as `V` operation)**:  Increases semaphore value, signalling that the resource is available. 
## Example
```c
sem_t semaphore;
sem_wait(&semaphore): // decrease semaphore, thus entering critical section
// critical section code here
sem_post(&semaphore);
```
## Usage
Semaphores are useful for managing limited resources (like a fixed-size buffer) and for solving synchronization problems like producer-consumer or reader-writer problems. 

---
# Differences
A comparison between [[#Mutex Locks]], [[#Semaphores]], and [[#Synchronization Hardware]] for process synchronization, structured in a table:

| Feature                       | **Mutex Locks**                                                                                          | **Semaphores**                                                                                   | **Hardware Solutions**                                                                             |
| ----------------------------- | -------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------- |
| **Basic Concept**             | A lock that allows only one process or thread to access a resource at a time.                            | A signaling mechanism that uses counters to manage access to shared resources.                   | Synchronization implemented through hardware instructions (e.g., test-and-set, atomic operations). |
| **Type**                      | Binary (0 or 1) - Only two states (locked/unlocked).                                                     | Counting or binary.                                                                              | Atomic hardware instructions, such as Test-and-Set or Compare-and-Swap.                            |
| **Primary Use**               | Mutual exclusion for critical sections.                                                                  | Controlling access to resources, especially when multiple instances of a resource are available. | Prevent race conditions or ensure atomicity in resource access.                                    |
| **Lock/Unlock**               | A process locks the mutex before entering a critical section and unlocks it upon exit.                   | The semaphore value is decremented on resource request and incremented on resource release.      | Hardware automatically ensures that critical sections are protected via atomic operations.         |
| **Blocking**                  | Yes, if a process tries to acquire a locked mutex, it is blocked.                                        | Can be blocking or non-blocking depending on the implementation (wait/signal).                   | No blocking (busy waiting occurs in some hardware solutions like test-and-set).                    |
| **Busy-Waiting**              | Not typical (if implemented correctly with a blocking mechanism).                                        | Can be avoided if implemented with proper wait/signal mechanisms.                                | Often uses busy-waiting (e.g., spinlocks in test-and-set).                                         |
| **Efficiency**                | High efficiency for single-threaded access but not suitable for multiple resource management.            | Suitable for managing multiple resources and more efficient when using counting semaphores.      | Can be efficient, but busy-waiting can lead to inefficiencies in multi-process environments.       |
| **Race Condition Handling**   | Protects against race conditions in critical sections.                                                   | Helps avoid race conditions when managing multiple resources.                                    | Directly prevents race conditions using atomic hardware operations.                                |
| **Deadlock Potential**        | Possible, if not used carefully. For example, if two processes wait indefinitely for each other's mutex. | Possible, especially with multiple semaphores (can lead to priority inversion).                  | Less prone to deadlock since the operations are atomic and usually very low-level.                 |
| **Implementation Complexity** | Simple to implement and use, especially for small-scale mutual exclusion.                                | More complex compared to mutexes, especially with counting semaphores.                           | Implementation can be complex, depending on the hardware instruction set used.                     |
| **Shared Resources Control**  | Controls access to only one shared resource (binary).                                                    | Can control access to multiple instances of a shared resource (counting).                        | Controls access to shared memory or resources through hardware-level atomicity.                    |
| **Atomicity**                 | Mutex itself does not guarantee atomicity unless used with proper OS primitives.                         | Semaphore operations (wait, signal) need to be atomic to work correctly.                         | Operations are inherently atomic due to hardware support.                                          |
| **Typical Use Cases**         | Protecting a single shared resource or critical section in multi-threaded programs.                      | Managing multiple units of resources (e.g., thread pool, buffer space).                          | Low-level critical section protection in real-time systems or operating systems.                   |
| **Programming Level**         | Software-level synchronization (OS support needed).                                                      | Software-level synchronization (OS or library support needed).                                   | Low-level, hardware-driven synchronization.                                                        |
| **Examples**                  | Used in thread libraries (e.g., `pthread_mutex_lock()` in POSIX).                                        | Producer-Consumer problem, Readers-Writers problem.                                              | `Test-and-Set`, `Swap` instructions in hardware, atomic increment/decrement.                       |

## Key Takeaways:

### 1. Mutex Locks:
- Primarily used for **mutual exclusion** where only one process can enter a critical section at a time.
- Simple but effective when protecting a single shared resource.
- Can cause **deadlocks** if not managed carefully.

### 2. Semaphores
- More versatile than mutex locks as they can manage access to multiple resources (using counting semaphores).
- Can be **blocking** or **non-blocking**, depending on the implementation.
- Commonly used in synchronization problems such as the **Producer-Consumer Problem**.

### 3. Synchronization Hardware
- Implemented at the hardware level for atomic operations, preventing **race conditions**.
- Includes operations like **Test-and-Set**, **Compare-and-Swap**, and **Load-Link/Store-Conditional**.
- Can involve **busy-waiting** (inefficient for some use cases) but is reliable for ensuring **atomicity**.
