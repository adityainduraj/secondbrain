### Operating System Services
There are 9 OS services:
1. **User Interface** - Varies between CLI and GUI
2. **Program Execution**
	System must be able to load a program into memory and run that program and end execution.
3. **I/O Operations**
4. **File System Manipulation**
	Programs need to read and write files or directories, create and delete them, search them, list file information, permission management
5. **Comms** 
	Exchange information either on the same network or on different networks
6. **Error Detection**
	OS needs to be constantly aware of possible errors. Find out what and where it has happened, and use debugging.
7. **Resource Allocation**
	Proper resource allocation for when multiple users or jobs are running concurrently. 
8. **Accounting**
	Keep track of which users use how much and what kinds of computer resources
9. **Protection and Security**
	Ensuring all access to system resources is controlled (consider [[Privileges]]) Implementing user authentication to defend external I/O devices from invalid access attempts. 

### System Calls
Programming interface to the services provided by the OS. They are typically written in a high-level language such as C or C++. 

Mostly accessed by programs via a high-level API rather than direct system call use. 

Three most common APIs are:
1. Win32 for Windows
2. POSIX API for POSIX-based systems (all versions of UNIX)
3. Java API for JVM

#### System Call Implementation
Typically, a number is associated with each system call. 
System-Call Interface maintains a table indexed according to those numbers. The system-call interface then invokes the intended system call in OS kernel and returns status of system call and any return values. 

The caller doesn't need to know anything about how the system call is implemented. They just need to obey the API and understand what the OS will do as a result of their system call. 

#### System Call Parameter Passing
There are three general methods to pass parameters to the OS:
1. **Simplest** : Pass the parameters in registers. 
2. **Block** : Pass the parameters in a  block or table, in memory, and address of the block is passed as a parameter in a register. This approach is taken by Linux and Solaris. 
3. **Stack** : Parameters are placed or pushed onto the stack by the program and popped off the stack by the OS. 

#### Types of System Calls
System Calls can be grouped roughly into six major categories: 
1. Process Control
2. File Manipulation
3. Device Manipulation
4. Information Maintenance
5. Communications
6. Protection

### MS-DOS vs FreeBSD : A Comparison

| MS-DOS                                              | FreeBSD                                                                                                                                                                       |
| --------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Single Tasking                                      | Multitasking                                                                                                                                                                  |
| Shell invoked when system boots                     | Post-login, invoke user's choice of shell                                                                                                                                     |
| Simple method to run program, no process is created | Shell executes fork() system call to create process. Executes exec() to load program into process. Shell then waits for process to terminate or continues with user commands. |
| Program Exit -> Shell Reloaded                      | Process exits with either `Code = 0` which means no error, or `code > 0` where 0 is replaced by the error code                                                                |
### System Programs
They provide a convenient environment for program development and execution. Some of them are simply user interfaces to system calls, others are considerably more complex. They can be divided into the following categories:
1. File Manipulation
2. Status Information sometimes stored in a File Modification
3. Programming Language Support (LSPs)
4. Program Loading and Execution
5. Communications
6. Background Services
7. Application Programs

### Operating System Structure
#### MS-DOS
Its written to provide the most functionality in the least space. It is NOT divided into modules. 
Although MS-DOS has some structure, its interfaecs and levels of functionality are not well separated. 

#### Traditional UNIX System Structure
Beyond simple, but its not fully layered. 
![[UNIX OS Structure.png]]

#### Layered Approach
The OS is divided into a number of layers, each built on top of the lower layer. The bottom layer is the hardware, and the highest layer (N) is the user interface. 

With such modularity, layers are selected such that each uses functions and services of only lower-level layers. 

### Microkernel System Structure
Moves as much from the kernel into user space. 
Mach, the MacOS X kernel is an example of a microkernel. 
#### Benefits of Microkernels: 
1. Easier to extend.
2. Easier to port.
3. More reliable.
4. More Secure.

#### Detriments of Microkernels:
1. Performance overhead of user space to kernel space communication.

### Monolithic vs Microkernel : A Comparison

| Monolithic                                                     | Microkernel                                                                                                                        |
| -------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| A type of kernel where the entire OS works in the kernel space | A kernel type that provides low-level address space management, thread management and interprocess communiction to implement an OS |
| Kernel contains the OS services                                | OS services and kernel are separated                                                                                               |
| Fast                                                           | Slow                                                                                                                               |
| Failure in one component will affect the entire system         | Failure in one component will not affect other components                                                                          |
| Difficult to add new functionalities                           | Easier to add new functionalities                                                                                                  |
| Larger in size                                                 | Smaller in size                                                                                                                    |
### System Boot
When power is initialized on system, the execution starts at a fixed memory location. 
Firmware ROM used to hold initial boot code. 

Operating system must be made available to hardware so hardware can start it. 
- Some small piece of code - bootstrap loader, stored in ROM or EEPROM locates the kernel, loads it into memory and starts it. 
- Sometimes, there is a 2-step process where the **boot block** at fixed location is loaded by ROM code, which loads the bootstrap loader from disk.

Common bootstrap loader, **GRUB** allows selection of the kernel from multiple disks, versions and kernel options. 