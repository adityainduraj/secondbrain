---
time created: 2024-11-12 21:31
tags:
  - academics
  - os
  - endterm
references: 
follows:
---
The filesystem is the most visible aspect of a general purpose operating system. It provides access to both data and programs on the operating system and all the users of that system. 

The filesystem consists of 2 distinct parts:
1. A collection of files, each storing related data. 
2. A directory structure, which organizes and provides information about all the files in the system. 

---
## File Concept
Computers can store information on various devices, NVM, HDDs etc. To make the system more convenient to use, the OS provides a **uniform, logical view** of the stored information. 
The OS abstracts from the physical properties of its storage devices to define a logical storage unit, **the file**. 

> Files are mapped by the operating system onto physical storage devices. 


> [!info] The Definition of a File
> A file is a named collection of related infromation that is recorded on a secondary storage. 
> From the user's perspective, a file is the smallest allotment of logical secondary storage, i.e. **data can't be written to secondary storage unless they are within a file**. 
> In general, a file is a sequence of bits, bytes, lines or records. 

### Different Types of Files
Given below are a few examples of different types of files. 
1. **Text File**: A sequence of characters organized into lines, or maybe pages. 
2. **Source File**: A sequence of functions, each of which is further organized as declarations followed by executable functions. 
3. **Executable File**: A series of code sections that the loader can bring into memory and execute. 

---
## File Attributes

>A file is named for the convenience of its user, and is usually a string of characters. 

When a file is named, it becomes independent of the process, user or system that created it. A different user can use it, edit it, write it to a USB, send it over e-mail etc. 

The attributes of a file may vary from one OS to another, but typically consist of these: 
1. **Name**: The symbolic file name is the only information kept in human-readable form. 
2. **Identifier**: This unique tag (usually a number), identifies the file within the file system. It's the non-human-readable name for the file. 
3. **Type**
4. **Location**: This is a pointer to a device and to the location of the file on that device. 
5. **Size**: The current size of the file in bytes, words or blocks.  
6. **Protection**: Access-control information -> who can read, write, execute and so on. 
7. **Timestamps and User Identification**: Timestamps for creation, last modification, last use. 

> Some newer file systems support **extended file attributes** including character encoding, file checksum. 

The information about all files is kept in the directory structure, which resides on the same device as the files themselves. Typically, a directory entry consists of the file name and its unique identifier. That identifier in turn locates the other file attributes

It takes a certain amount of space to store file attributes for the entire filesystem. Since this storage scales fast, it's stored locally and brought into memory as required. 

---
## File Operations
**A file is an abstract data type**. To define it properly, we need to consider the operations that can be performed on it. Let's look at the **7 Basic File Operations** which will make it easy for us to see how other similar operations like renaming a file can be implemented. 

1. **Creating a File**: There are 2 necessary steps to create a file. First, we must find space in the filesystem for the file. Second, an entry for the new file must be made in the directory. 
2. **Opening a File**: Instead of having all file operations specify the file name each time, all operations other than create and delete require a file to be `open()` first. If successful, the open call returns a **file handle** that is used as an argument in other calls. 
3. **Writing a File**: To write a file, we make a system call specifying **the open file handle** and **the information to be written into the file**. The system must keep a **write pointer** to the location in the file where the next write is to take place if it is sequential. This **write pointer** must be updated on every write. 
4. **Reading a File**: To read, we use a system call that specifies the file handle and where the next block of the file should be put. The system, needs to keep a **read pointer** (same reason as above). Because a process is either reading from or writing to a file, the current operation location can be kept as a per-process **current-file-position pointer** for streamlining and using one pointer for both purposes. 
5. **Repositioning within a File**: The **current-file-position pointer** of the open file is repositioned to the new, given value. This is also known as a **file seek**. 
6. **Deleting a File**: To delete a file, we search the directory for the named file. Then we release all file space. Some systems allow **hard links** ([[NixOS]]) -> multiple directory entries for the same file. In this case, the actual file content is not deleted until the last link is deleted. 
7. **Truncating a File**: Th euser may want to erase the contents of the file but keep its attributes. For this, instead of deleting the file and creating a new one, the length is reset to zero and its file space is released, keeping the attributes as they are. 

We mentioned the concept of `open()` before as well. Since every operation seems to involve searching the directory again and again, we use `open()` to make a table of open files that can then be searched easily, cutting down on searching costs. This table is called the **open file table**. 

> Most systems require a file to be opened using `open()` before it can be used in any way. Once done, they are closed with `close()`. 

In the case where the file can be open in multiple places, there are 2 levels of internal tables:
- **Per-Process Table**: Tracks all files that a process has open. In this table, information regarding the process's use of the file is stored. Each entry in this table points to a system-wide open file table. 
- **System-Wide Open File Table**: Contains process independent information, such as the location of the file on disk, access dates, file size etc. When another process executes an `open()` on a file already open in another process, a new entry is simply added to the process's table that points to the appropriate system-wide table. 

Typically, the open-file table also has an **open count** associated with each file to indicate how many processes have the file open. Each `close()` decreases it and when it reaches 0, (the file is closed) the entry is removed from the table. 

> [!info]- Stuff Associated With An Open File
> 1. **File Pointer**: System must track last read-write location s a current-file-position pointer. 
> 2. **File-Open Count**: Mentioned above. 
> 3. **File Location**
> 4. **Access Rights**

Some operating systems allow for **file locks**. 
**Shared Locks** are like reader locks, several processes can acquire the lock concurrently. 
**Exclusive Lock** is like a writer lock, only one at a time. 

> Some operating systems provide both types, some provide just exclusive locks. 

> [!tip] Mandatory vs Advisory Locks
> A **mandatory** lock is when the operating system ensures locking integrity (Windows). On the other hand, in **advisory** locks, it is left up to the software developers to ensure that locks are appropriately acquired and released by their software/apps (UNIX). 

---
## File Types
A common technique for implementing file types is to include the type as part of the file name. The name is split into 2 parts, the name and its extension, usually separated by a period. 

The system uses the extension to indicate the type of the file and the type of operations that can be done on that file. 

In MacOS, each file has a type, stored as an extension, (.app for applications). Each file also has a creator attribute that contains the name of the program that created it. For example, if the word processor creates a file, then that word processor's name is stored as its creator. When th euser opens it, that word processor is invoked automatically. 

The UNIX system uses a **magic number** stored at the beginning of some binary files to indicate the type of data in the file. For example, it uses a text magic number at the start of text files. Not all files have magic numbers, so system features cannot solely be based on this information. 

---
