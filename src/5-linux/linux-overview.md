# Linux Overview 
Linux is commonly used to refer to the entire UNIX-like operating system of which the Linux kernel forms a part. 

## Linux System Architecture

*Add picture*

The *physical machine*, the bottom or base of the system, made up of memory (RAM) and the processor or central processing unit (CPU), as well as input/output (I/O) devices such as storage, networking, and graphics. The CPU performs computations and reads from, and writes to, memory.

## Linux Kernel
The **Linux Kernel** is a free and open-source, monolithic, modular, multitasking UNIX-like operating system kernel. It is the core interface between a computer’s hardware and its process. It communicates between the two, managing resources as efficiently as possible.

The Linux kernel executable typically resides at the pathname */boot/vmlinuz*. 

The kernel performs the following tasks:
* **Process management:** determine which processes can use the central processing unit (CPU), when and for how long.
* **Memory management:** keep track of how much memory is used to store what and where.
* **Provision of a file system:** allow files to be created, retrieved, updated, deleted and so on.
* **Creation and termination of processes:** permit load a new program into memory, providing it with the resources that it needs to run. When a process ha completed the execution, the kernel ensures that the resources it uses are freed for subsequent reuse by later programs.
* **Device drivers:** acts as intermediary between the hardware and processes.
* **Networking:** transmits and receives network messages on behalf of user processes. 
* **Provision of a system call application programming interface (API):** process can request the kernel to perform various tasks using kernel entry points known as system calls. 

The kernel resolves potential conflicts in accessing hardware resources, so users and processes are generally unaware of the conflicts.

### Kernel mode and user mode
The processor architectures typically allow the CPU to operate in at least two different modes:
* **User Mode:** the CPU can access only memory that is marked as being in user space; attempts to access memory in kernel space results in a hardware exception.
* **Kernel Mode:** the CPU can access both user and kernel memory space.

## Shell 
A **shell**, known as a *command interpreter*, is special-purpose program designed to read commands typed by a user and execute appropriate programs in response to those commands. 
The shell is a user process. The shell are designed not merely for interactive use, but also for the interpretation of shell scripts, which are text files with shell commands.
Types of shells:
* Bourne shell (sh)
* C shell (csh)
* Korn shell (ksh)
* Bourne again shell (bash)

## Users and Groups
Each user on the system is uniquely identified, and users may belong to groups. 

### Users
Each user has a unique username and user ID (UID). The password file,  */etc/passwd*, includes the following information for each user:
1. Username
2. Password: It is set as *x* in this file and stored in the /etc/shadow file.
3. User ID
4. Group ID
5. Full name of the user
6. Home Directory: initial directory into which the user is placed after logging in.
7. Login shell: name of the program to be executed to interpret user commands.

* **Add image**

### Groups
The users are organized into **groups**, for controlling access to files and other system resources.
The group file, */etc/group*, includes the following information for each group:
1. Group name
2. Group ID (GID)
3. User list

* **Add image**

### Superuser
The **superuser** has special privileges within the system. Its user ID is 0 and has the login name *root*. It bypasses all permission checks in the system.
The system administrator uses the superuser account to perform various administrative tasks on the system.

### Directory Hierarchy, Directories, Links and Files.
The kernel maintains a single hierarchical directory structure to organize all files in the system. At the base of this hierarchy is the root directory (/). All files and directories are children or further removed descendants of the root directory.

### File Types
Each file is marked with a type, indicating what kind of file it is. One file could be regular or plain, devices, pipes, sockets, directories, and symbolic links.

### Directory and links
A **directory** is a file whose contents take the form of a table of filenames coupled with references to the corresponding files. This filename-plus-reference association is called a **link**. 
A file may have multiple links, and thus multiple names, in the same or different directories.
Directories may contain links both to files and to other directories. 

### Symbolic link
A **symbolic link** provides an alternative name for a file. It has a filename-plus-pointer entry in a directory, and the file referred to by the pointer contains a string that names another file. 

### File ownership and permissions
The ownership of a file is used to determine the access rights available to users of the file. 
The system divides users into three categories:  the owner of the file, users who are members of the group ID, and others. Three permission bits may be set for each of these categories: read, write and execute.

* **Add image**

### File I/O Model
The same system calls are used to perform I/O on all types of files, including devices. The kernel translates the application’s I/O requests into appropriate filesystem or device-driver operations that preform I/O on the target file or device.

### Programs
The **programs** normally exist in two forms: **source code** and **binary**. The two forms are considered synonymous since the step of compiling and linking converts source code into semantically equivalent binary machine code.

### Process
A **process** is an instance of an executing program. When a program is executed, the kernel load the code of the program into virtual memory, allocates space for program variables, and sets up kernel bookkeeping data structures to record various information about the proves. 

A process is logically divided into the following parts, known as segments:
* **Text:** the instructions of the program.
* **Data:** the static variables used by the program.
* **Heap:** an area from which programs can dynamically allocate extra memory.
* **Stack:** a piece of memory that grows and shrinks as functions are called and return and that is used to allocate storage for local variables and function call linkage information.

Each process has a unique integer **process identifies** (**PID**). Each process also has a **parent process identifies** (**PPID**), which identifies the process that requested the kernel to create this process.
A process can terminate in one of two ways: by requesting its own termination or by being killed by the delivery of a signal. 

#### Bootloader
The **bootloader** is responsible for loading the kernel and initial ramdisk before initiating the boot process. It is a piece of software started by the firmware (BIOS or UEFI). It is responsible for loading the kernel with the wanted kernel parameters and any external initramfs images. 	

#### Init System
**Init** is the first process started during booting of the operating system and runs until the system shuts down. It is the process ID 1.
Its role is to create process from script stored in the /etc/inittad which is a configuration file which is to be used by initialization system. It is the latest step of the kernel in the boot sequence. 

#### Daemons
A **daemon** is a process with the following characteristics:
* It is long-lived. It is common that the daemon is created at system startup and runs until the system is shut down.
* It runs in the background and has not controlling terminal. It ensures that the kernel never automatically generates any job-control or terminal-related signals, i.e. SIGINIT, SIGTSTP, SIGUP for a daemon.

**Daemons** are written to carry out specific tasks, for example:
* *cron:* exectues commands at a schedule time.
* *sshd:* secure shell daemon, to login from remote hosts using a secure communications protocol.
* *httpd:* HTTP server daemon, which server web pages.
* *inetd:* Internet superserver daemon, which listens for incoming network connectionsn on specified TCP/IP ports and launches appropriate server programs to handle these connections.