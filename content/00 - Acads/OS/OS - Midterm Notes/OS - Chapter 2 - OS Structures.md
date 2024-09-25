---
time created: 2024-09-25 19:30
tags:
  - academics
  - os
references:
  - "[[Exam Schedules and Syllabus]]"
follows: "[[OS - Chapter 1 - Introduction]]"
---
---
# OS Services
There are 9 operating system services. 
1. **User Interface**: Almost all operating systems have a UI. Varies between CLI, GUI. 
2. **Program Execution**: The system must be able to load a program onto memory and run that program, and end its execution either normally or abnormally. 
3. **I/O Operations**: A running program might require I/O which may involve a file or an I/O device.
4. **File System Manipulation**: Programs need to read and write files and directories, create and delete them, search them, list file information and manage their permissions. 
5. **Communications**: Processes may exchange information on the same computer or with others over a network. These communications may be via **shared memory** or through **message passing** (packets moved by the OS). 
6. **Error Detection**: OS needs to be constantly aware of possible errors. These may occur in the CPU and memory hardware, in I/O devices or in user programs. OS should take appropriate action for each type of error and should have good debugging facilities. 
7. **Resource Allocation**: When multiple users or multiple jobs running concurrently, resources must be allocated to them. 
8. **Accounting**: To keep track of which users use how much and what kind of resources.
9. **Protection & Security**: Protection involves ensuring that all access to system resources is controlled. Security from outsiders requires user authentication and extends to defending external I/O devices from invalid access attempts.

![[a view of os services.png]]

---
# User-OS Interface
## CLI or Command Interpreter
1. Sometimes implemented in the kernel, sometimes by system programs. 
2. Sometimes multiple flavors are implemented called **shells**. 
3. Primarily fetches a command from a user and executes it. 
4. Sometimes the commands are built-in, sometimes they are just names of programs. If its the latter, then adding new features doesn't require shell modification. 

## GUI
- User-friendly desktop interface. 
- Icons represent files, programs, actions etc.
---
# System Calls
**System Calls** are programming interfaces to the [[#OS Services]]. They are typically written in a high-level language like C or C++. 

System calls are mostly accessed by programs via a high-level API rather than direct system call use. Examples -> Win32, POSIX and Java API. 

## System Call Implementation
Typically, a number is associated with each system call. The system-call interfaec maintains an indexed table according to these numbers. 
The system call interface invokes the intended system call in OS kernel and returns that status of the system call and return values (if any). 

The caller doesn't need to know anything about how the system call is implemented. They just need to obey the API and understand what will the OS do as a result of that system call. 

## Passing System Call Parameters
Often, more information is required than just the name of the system call. There are three general methods to pass this information to the OS. 
1. **Simplest**: We pass the parameters to the registers. The issue with this is that in some cases, there may be more parameters than registers. 
2. **Table**: We store the parameters in a table which is stored in memory and the address of the memory block is passed as a parameter in a register. This is the approach taken by Linux. 
3. **Stack**: The parameters are pushed onto the stack by the program and popped off the stack by the OS. This also fixes the length issue with the simplest method. 
---
# Types of System Calls
System calls can be roughly grouped into six major categories. 
1. **Process Control**: Creating, terminating, ending, aborting, loading, executing, waiting, allocating and freeing memory etc.
2. **File Manipulation**: Creating, deleting, opening, closing, reading, writing, repositioning, getting and setting file attributes.
3. **Device Manipulation**: Requesting and releasing devices, getting and setting device attributes, logically attach and detach devices.
4. **Information Maintenance**: Getting the time or date, getting system data, getting and setting file or device attributes.
5. **Communications**: Creating and deleting communication connections, transfer status info, sending and receiving messages from client to the server.
6. **Protection**: Control access to resources ,get and set permissions, allow and deny user access.
![[os-system-calls-example.png]]





---
# System Programs

> General stuff, mostly covered above. Read the ppt once during revision.



---
# OS Structure
There are various ways to structure an OS. 
1. Simple -> MS-DOS
2. More Complex -> UNIX
3. Layered -> An abstraction
4. Microkernel -> Mach

> MS-DOS was written to provide the most functionality in the least space. It's interfaces and functionality levels are not well separated.

> Traditional UNIX was limited by hardware functionality. It was beyond simple but not fully layered. UNIX consisted of two separable parts -> System Programs and The Kernel. The kernel provides everything below the system-call interface and above the physical hardware.

> In the layered approach, the OS is divided into a number of layers, each built on top of the other. The bottom layer is the hardware, the top is the UI. 
> With modularity, layers are selectign such that each uses operations and services of only lower-level layers.

> Microkernels move as much of the kernel as they can into the user space. **Mach** is an example of the kernel. 
> The benefits are that its easily extensible, easier to port to OS to new architectures, more reliable and secure.
> The detriments are the performance overhead of the user space to kernel space communication.


| MICROKERNEL                                                                                                                                    | MONOLITHIC                                                      |
| ---------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------- |
| A kernel type that provides mechanisms such as low-level address space management, thread management and interprocess communication to the OS. | A type of kernel where the entire OS works in the kernel space. |
| OS services and the kernel are separated.                                                                                                      | The kernel contains the OS services.                            |
| Slow.                                                                                                                                          | Fast.                                                           |
| Failure in one component will not affect other components.                                                                                     | Failure in one component will affect the entire system.         |
| Easier to add new functionalities.                                                                                                             | Difficult to add new functionalities.                           |
| Smaller in size.                                                                                                                               | Larger in size.                                                 |

---
# System Boot
When the power is initialized on the system, the execution starts at a **fixed memory location**. The firmware ROM is used to hold the initial boot code. 

The operating system must be made available to the hardware so that the hardware can start it. 
A small piece of code called the **bootstrap loader** is stored in **ROM** or **EEPROM** locates the kernel, loads it into memory and starts it. 
Sometimes, there is a two-step process where a **boot block** is placed at a fixed location by the ROM code, which loads the **bootstrap loader** from disk. 

> Boostrap Loader == Bootloader

Common bootstrap loader, **GRUB** allows the selection of kernel from multiple disks, versions, and kernel options. [[Linux Helpbox]]. 

Kernel is loaded and the system starts running. 

---
