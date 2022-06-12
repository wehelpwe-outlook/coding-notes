## OS main 2 functions:
- Abstraction
- Arbitration 仲裁者

## Layers of the system
- Application
- Libraries
- Operating System (Kernel)
- Hardware

## Multics
- Time sharing start in 1964 - failure

## Unix
- Originally developed at Bell Lab in 1969 is commercial distributions.
- Name is a play on Multics
- Free version : BSD and Linux

##  BSD
- Berkeley System Distribution
- Base on AT&T's commercial distribution
- UC Berkeley sued AT&T, eventual settlement allowed Berkeley to distribute BSD freely in both source and binary form
- Today's descendant include:
   - FreeBSD
   - OpenBSD
   - Mac OS X

## Linux
 - Operating system kernel project started by Linus Torvalds as an undergraduate at University of Helsinki
- First release in source code form in 1991
- Often combined with a set of libraries and utilities created by the GNU project.

- Commercially successful for many device classes: 
   - web server, 
   - network and embedded devices and 
   - mobile phone (Android)
- Some us as a desktop platform: Ubuntu and Fedora(Redhat), CentOS(Redhat Enterprise Release)
- Kernel is scalable from embedded devices to supercomputers.

## Linux Kernel
- Monolithic kernel: Device driver built in, but with module support
- Latest version      5.18 on 2022-5-31

## Linux Distributions
- [Distribution https://distrowatch.com/](https://distrowatch.com/) 2022-6-1 TOP3
   - [1 MX Linux - base on debian](https://mxlinux.org/)	
   - [2 EndeavourOS - base on Arch]()
   - [3 Mint - base on Ubuntu](https://linuxmint.com/)

- can be classified by package manager
   - RPM (Redhat Package Manager): RHEL, Fedora, Mandriva, OpenSUSE
   - APT (Advanced Package Tool): Debian, Ubuntu
   - Other binary formats: pacman(Arch Linux)
   - Source formats: Gentoo(ebuids), Linux From Scratch, etc

## Debian GNU/Linux
- which was established by **Ian Murdock** on August 16, 1993. The first version of Debian (0.01) was released on September 15, 1993,[7] and its first stable version (1.1) was released on June 17, 1996.
- Full open source distribution (avoid proprietary software ) 
- Emphasis on security and stability
   - Achieves stability by using well-tested packages
   - Package version tend to be dated
- An "unstable" version is available with more bleeding-edge package.
- APT package format

## Arch Linux
- Minimalist framework for creating a custom system , different philosophy.
- Not based on any prior distribution (writing form scratch)
- Rolling distribution
   - No discrete versions
   - Updating the system updates all packages to the latest versions, with all the latest bugs.
- Pacman package manager

## Journaling File System
- Many filesystem operations requires several steps to complete.
- If the computer crash or loses power in between the steps, the file system may enter an inconsistent state 
   - repair tools usually exist but are slow to execute
- Journaling relieves this problem by recording all the steps that are to be taken, prior to taking the steps
- In the event of a crash, the file system replays the journal to restore itself to a consistent state.

## Filesystem Layout
- FAT File Allocation Table, DOS Win31,95,98
   - Single table on each partition stores metadata and address of data for each file
- inodes
   - Data structures on Unix system that contain file metadata, including pointers to tha actual data , DOES NOT include the filename.\
   - Directories are data structures that map filenames to inode numbers
   - Maximum number of files that a single filesystem can hold is limited by the number of inodes created when the filesystem is formatted

## Extents 
- Support larger maximum file sizes by allow ing files to be composed of several non-contiguous blocks of space
- Extent information is stored with the metadata in the file inode (or similar structure) for filesystems that support extents

## Filesystem Summary
- Filesystems organize data, store metadata, provide an abstraction of the underlying store medium, and arbitrate access to the storage space
- Creating a filesystem is a process known as **formatting**
- Filesystems can be journaled to reduce the risk of data loss
- Internally filesystem use allocation tables, inodes, extends and other data structures to organize information
- Filesystem are made available in a running system by **mounting** them.

## CPU Overview
- Multiprogramming and Hardware Requirements
   - Interrupt mechanism
   - Clock
   - CPU protection levels: protected and privileged
- CPU Privilege Modes
   - Supervisor Mode (Kernel Mode, can do anything)
   - User Mode (Process in sandbox)
- x86 Protection Rings
   - 4 rings, numbered 0 to 3
   - ring 0 = supervisor mode 
   - ring 3 = user mode
   - In practice, most OS only use rings 0 and 3
- Hypervisor Mode
   - Newer Intel CPUs with VT-x extensions and AMD CPU with AMD-V extensions
   - Adds an extra privilege level below ring 0 (-1)
   - Enable instructions that allow multiple operating systems to share the same processor.
- Mode Switches
- Interrupt
   - Involuntary (external to the process)
      - I/O interrupts
      - Clock interrupts
      - Page faults (virtual memory)
   - Voluntary ( caused by the process)
      - System calls
      - Exceptions (Segfault, divided by zero, etc)
   - CPU provide hardware mechanism for detecting interrupt requests and handle interrupts

## Linux Kernel
- Functions of the kernel
   - Abstraction: to hardware, scheduling, inter-process communication(IPC)
   - Arbitration: sandbox for process, access privileges, minimize the risk one application crash the whole system

- Mechanism vs Policy
   - Mechanism: sw method tell system how to run. eg:NIC driver
   - Policy: sw method allow who can run
   - Good design: mechanism separated with policy. 
  
- Seminal Early Kernels
   - eg: 1969 RC4000, has a center component call "monitor", performance was awful, but the system was stable and reliable
   - vs UNIX kernel, emphasized performance, driver , IPC, scheduling, memory management are all put into the kernel and run in kernel space 
 - Monolithic Kernels vs Microkernels
   - **Monolithic**
   - Entire operating system is placed in the kernel space
   - All OS code runs in privileged mode (ring 0 on x86)
   - Different functions of the OS are divided into subsystems.
   - Higher performance at the expense of poorer modularity and separation of components
   - **Microkernel**
   - Bare minimum of code runs in the kernel. only do: addressing, IPC, and scheduling; under 10000 line of code as a rule of thumb
   - Rest of the OS is  divided into a collection of servers running in userspace with low privilege
   - Excellent modularity, but performance suffers
   - **Hybrid Kernels**
   - Almost all the kernels in widespread used today  have properties of both monolithic kernels and microkernels
      - A non-modular monolithic kernel would be unmaintainable
      - A true microkernel would have unacceptable performance

## Kernel Examples
Monolithic | Microkernel
-|-
Linux|RC 4000 Monitor
BSD | Mach
System V Unix | L4
MS-DOS | MIT Exokernel
Win9x |
Win NT/XP/Vista/7 for those who reject the "hybird" terminology | Win NT kernel is derived from a microkernel design but is not a true microkernel
Mac OS X kernel for those who reject "hybird" | Mac OS X kernel is based on Mach, but OS X kernel is not a true microkernel

## Linux Command line
- ps ax // to see all the processes
- ps aux // to see all the user owns processes
- ldd /user/bin/evince // To see what libs are used by a binary program

## Interrupts and I/O
- Handling Device Events
   - Hardware devices product events at times and in patterns not know in advance. eg: Keyboard press, Incoming network packages, Mouse movement
- Two Input Options
   - Polling, OS periodically queries each device to see if new Information is available
   - Polling, Simple design, but high latency, inefficiency
   - Interrupts, Device sends a signal to the OS tha request attention, OS preempts running process to handle device request
   - Interrupts, More complex implementation (requires hardware support), OS preempts any running process(switches context away from the running process) to handle the event

## Interrupt Controllers
- It is a interface for hardware to signal the CPU whenever a device needs attention
   - Programmable Interrupt Controller (PIC)
   - Advanced Programmable Interrupt Controller (APIC)
- Old mechanism for interrupt signals required dedicated lines to be added to the motherboard (ISA 1987 and old PIC buses)
- New mechanism uses a shared bus and memory registers on the APIC (PCI Express and newer PCI devices)

## Interrupt Request Lines (old PC)
- 2 Programmable Interrupt Controller chip
- 1 Master ,1 Slave, each chip have 8 line, can handle 8 Interrupt Requests
- IRQ 0 - Timer
- IRQ 1 - Keyboard
- IRQ 8 - RTC
- IRQ 12 - PS/2 Mouse
- IRQ 13 - Math Coprocessor
- IRQ 14 - Primary IDE Controller
- IRQ 15 - Secondary IDE Controller
- **ISA and PCI devices had to share the IRQs**
   - Led to hardware conflicts that could lock system
   - Performance issue since OS had to poll each device sharing an IRQ to determin who raised interrupt.
## Modern System (Message Signaled Interrupt MSI or MSI-X)
- Timer, RTC, USB Host Controller, SATA Controller shared bus and send a message to the APIC memory register
- Only interrupt mechanism available on PCI Express
- Legacy PCI/ISA support required APIC to support physical interrupt lines as well as MSC registers
- MSI supports up to 32 IRQs per device, MSI-X up to 2048

## CPU Fetch-Execute Cycle
- Start : Fetch Instruction -> Increment Program Counter -> Execute Instruction -> Interrupt Pending ? 
   - Yes, Switch to kernel mode -> Push PC onto stack -> Load Program Counter(new) from Fixed Memory Location (Interrupt Vector Table)
   - No, back to start, Fetch Instruction again

## Interrupt Vector Table
- stores mapping of interrupt type to address of the associated handler.

## Interrupt Descriptor Table
- Intel and AMD use for faster interrupt handling and protection ring management
- IDT is a reserved block of RAM used by CPU to branch quickly to a specific interrupt handler
- Mapped to kernel space (original to address 0)
- First 32 entries are reserved for mapping handlers for CPU faults 

## 2 types of Interrupt handler
- Fast, handle immediately, atomic, while processing CPU can not handle other fast interrupt
- Slow, interrupt be putted in a task queue, to be execute after No more fast interrupt

## Memory
- Random Access Memory (RAM)
  - Dedicated hardware memory
  - Volatile (contents lost when power interrupted)
  - Separated from long-term persistent storage (disk)
## OS View
- see memory as one large block
- Byte level addressing
- 32bit OS only can access 4G Byte memory
## Process View
- Stack
   - Automatic variables
- Heap
   - Allocated memory
- Globals
- Text
   - Read-Only
   - Program code

## Process Memory Use
- Automatic variables in stack
   - Placed on stack automatically by the compiler
   - Local variable in functions
- Heap allocated variables, structures, or objects
   - Explicit allocation/deallocation in many languages
      - malloc and free (C)
      - new and delete (C++)
   - Explicit allocation with garbage collection
      - new (Java)
   - Implicit allocation with garbage collection
      - Python objects, lists, etc
- Typical heap memory allocations: Small(hundreds of bytes), Frequent
- No way for OS to predict when a process will allocated for free memory
- Multiprogramming systems need to share memory among several processes at the same time  

## Kernel Memory Use
- Kernel also use memory !
   - Kernel code, data structure, etc.
   - eg: PCBs(Process? Control Backlist), ready list, scheduling queues, etc
- Explicit allocation / deallocation (Linux examples)
   - kmalloc
   - kzalloc
   - kvalloc
   - kfree
   - vfree

## Sharing Memory
Memory|
:-:|
Stack
Free Space
Heap
Global variables
Text
**Kernel** 

## Partitioning memory
Memory|
:-:|
Stack
Free Space
Heap
Global variables
**Text**
Stack
Free Space
Heap
Global variables
**Text**
**Kernel** 

- Number of concurrent processes limited to number of partitions
- Maximum memory available to any one process limited
- Waste memory for small processes

## Dynamic Memory Allocation - 4 types
- Best Fit: find the smallest chunk, search entire free list, **external fragmentation**
- Worst Fit: find the largest chunk, search entire free list, still **fragmentation**
- First Fit: find the first chunk
   - fragments accumulated near the start of list, reducing performance over time
   - Using in Linux for embedded devices: SLOB allocator (Simple List of Blocks)
- Next Fit: find the first chunk **starting from the previous allocation**
   - Avoid accumulated fragments

## Kernel Memory Allocation - The Power of Two Methods
- Divide memory into large blocks (superblocks), then divide a **supreblock** to smaller-sized blocks(sub-blocks), **sub-block** can also be divided into smaller-smaller blocks
- Faster allocation / free operations
   - Scalable to multiple parallel CPU cores
- Reduces external fragmentation, still have some internal fragmentation
- Using a **binary tree** to replace list to track allocated and free blocks

## Buddy Memory Allocation
- Power of two method
- Set superblock size to an upper limit U (power of two)
- Set minimum block size to a lower limit L (power of two)
   - Smaller L requires larger data structure to track blocks
   - Larger L produce more internal fragmentation
- Allocate using regular power of two technique, dividing larger block into smaller ones as needed
- Coalescence - 聚结，就近合并，2的整数倍
   - Whenever freeing a block in buddy memory system, check to see if neighboring blocks are free. 512->1024->2048->4096

## Slab Allocation - for kernel data structures
- File descriptor, semaphores, PCB structures, etc
- Arrange kernel memeory into fixed-size slabs
- Each slab is divided into regions sized for specific types of kernel objects
   - Initial division performed at complie-time
   - Additional slabs can be provisioned at run-time
- Several slabs are pre-allocated into caches, which provide fast allocation on demand
- Linux SLAB allocator
   - default before 2.6.23
   - Good allocation performance on shared memory system with few CPU cores
   - Internal data structures wasted considerable space on extremely large, shared memory system, such as SGI rendering farms
- Linux SLUB allocator
   - default since 2.6.23
   - Designed by Christoph Lameter at SGI
   - Improves scalability by reducing the size of the data structures needed to track allocated and free objects
   - Original implementation had a performance bug
      - caused by adding partially used slabs beginning of a linked list instead of end of the list
      - Fix was one line code

## Paging
- Mechanism that facilitates sharing memory among multiple userspace processes
- Process sees logical memory addresses broken into pages
- Corresponding frame in physical memory need not to be contiguous
- CPU Memory Management(MMU) is to translation from logical address (pages) to physical address (frame)
- Page size, 512B to 8KB, modern CPU up to 1GB
- Page Table store in physical memory (in kernel space)
- Problem: each page translation requires table lookup, double memory accesses
   - 每次数据获取都需要2次内存访问，1次查表，1次取数
   - 每次指令执行都需要2次内存访问，1次查表，1次取指令
- Solution: table cache in CPU called Translation Lookaside Buffer (TLB)
   - CPU component 
   - A type of memory
   - Constant-time lookups (parallel search)
   - Fast
   - Expensive
   - TLB size 8 - 4096 entries depends on money

## Page Tables - 2 types
- Hardware-managed TLBs (x86, x86-64, SPARC, ARM)
   - CPU traverse(遍历) page table automatically whenever a TLB misss occurs
   - Increase in memory access time, but no CPU fault(interrupt), so no context switch
- Software-management TLBs (MIPS)
   - OS must traverse page table on TLB miss
   - TLB miss results in fault (interrupt)
   - Context switch to page table search routing

## Hierachical Page Tables
- Divide page tables into pages
- Logical address

t | p |d
:-:|:-:|:-:
outer page table | Pageable offset | Frame offset

- 2 level for simplicity

## Hashed Page Table
- Address spaces on 64-bit CPU are large
   - 34-48 bit true hardware address size
   - Lookup time long
- Hash table to improve search speed
- Page table stores **linked list of page-to-frame** mappings.
   - Chaining solution to hash collisions

## Extend Page Tables (EPT) for Virtualization
- Intel call : EPT
- AMD call: RVI (Rapid Virtualization Indexing)
- Two level of mapping
   - 1st pages -> guest frame numbers
   - 2nd (EPT table) guest frame numbers -> physical frame numbers
- Enable each VM to manage its own memory efficiently, without having to invoke the hypervisor to perform page mapping

## Memory Protection
- Segmentation (old)
   - Bit used to set permissions on a per-segment basis: read/write/execute valid/invalid
- Valid / Invalid Pages 
   - Adding permission on the page table
   - Memory access to an invalid page results in a segmentation fault or bus error
      - Trap causes context switch to kernel
      - Kernel send SIGSEGV or SIGBUS to process to let that process exit immediately

## Shared Pages - read only code
- eg: Program code for a web browser needs to be loaded into memory only once. Multiple tabs share same Chromium core code

## NX bit for prevent execution
- Intel: XD bit
- AMD: Enhanced Virus Protection (1st use)

## Virtual Memory - Part I
- Used to solve program need more and more RAM
- By swapping pages of memory out to a backing store(disk)
- Page Swapping 
   - Swapping : historically means moving entire process into RAM from disk
   - Paging: aka Page swap, refers to moving a single page from disk to memory
   - Dedicated disk space used for swapping pages, eg: linux **swap partition**
   - Modern Linux kernels using a **swap file**
- Paging Fault
   - A logical memory page is not in the physical memory
   - MMU causes a trap (interrupt) to CPU, resulting in a context switch to the OS paging mechanism
   - OS loads the missing page from disk, possibly moving another page from memory to disk

- Pag Table, Revised
   - Add a bit to the page table: present in physical memory or not

- Backing stores
   - Disk vs SSD (limited erased/written times)
   - Compcache: Linux Compressed Caching (zram disk)

## Virtual Memory - Part II
- Paging Performance: Slow
- If the fraction of memory accesses resulting in page faults become too high, system performance will degrade significantly, called "thrashing" or "swapping", often caused by memory over-subscribed by a program bug
- Demand Paging - Virtual memory fetch policy
   - Premise: only whe required
   - per-fetching  
- Pure Demand Paging - No prefetching
   - Page fault occurs at the start of a new program , loading the first section of the code into memory
   - Used in limited physical memory, monitor system.
- Copy on write (COW)
   - Unix process are created by cloning another process using the fork system call
   - Both original process and cloned process initially share memory pages
      - Mark read-only so MMU fault on write
      - OS handles fault by copying the page and turning of the read-only bit
- Memory mapped files
- **Shared Libraries**
   - Shared pages facilitate sharing of certain routines common to many different programs
   - Routines from shared libraries must be added to program
      - Dynamic Linking: loader adds at run time
      - Static Linking: linker adds at compile time
   -Compile libraries stored on disk in binary form
      - Shared Objects(.so file) on Unix
      - Dynamic Link Libraries (.dll files) on Windows

shared libraries|
:-:|
Stack
*free memory*
**Shared Objects**
*free memory*
Heap
Global Variables
Text:Program code converted to machine instructions

## Page Replacement
- Which page, when, why be chosen to swap to disk
- Global vs Local replacement
   - Processes can steal frame from each other
   - Any frame in memory can be replaced
   - vs:
   - Limited to number of frames
   - Only frame belong to this process can be replaced
- Local replacement policy need to know how many frames allocated to the process
- More frames not necessarily better
- Too few frames causes thrashing
- 2 more bit added to Page Table
   - Referenced bit: set when page is accessed by process
   - Dirty bit: set when page is modified by process

Page Number|Frame Number|Present|Valid|Read-only|NX|Referenced|Dirty
-|-|-|-|-|-|-|-
0x04a1|0x0100|1|1|0|1|1|1|1
0x04a2|0x0100|0|1|0|1|1|0|0
0x04a3|0x0100|0|1|0|1|1|0|0
0x04a4|0x0100|1|1|1|0|1|1|0
0x04a5|0x0100|0|1|0|1|0|1|0

- Page replacement algorithms
   - Random (Ineffective)
   - Oldest Page (Ineffective)
   - Least Frequently Used - LFU (Ineffective)
   - Most Frequently Used - MFU (Stupid)
   - Least Recently Used - LRU (Theoretical effective, can not track last access time)
   - Optimal Algorithm - OPT (Provably optimal but Impractical due to need to future memory acesses)
   - Not Used Recently - NUR (Reasonable performance using reference bit, dirty bit, and /or an age counter)

## Process
- **Definition**
   - An instance of a computer program in execution
   - Sometimes also called a job or task
   - Processed on modern systems can have several threads of execution, or several different pieces of the program running in parallel
   - Each process has its own private resources, data, and associated statistics
- Information associated with a process
   - Program Counter
   - Data stored in memory by the program
   - And, opened files
   - Networking connections
   - eg:
   - Process ID
   - Owner
   - CPU time used
   - RAM used
- Process States
   - New: created by the OS
   - Ready: waiting to be assigned to a CPU core
   - Running: executing on a CPU core
   - Waiting: waiting for some event
   - Terminated: finished execution
- Process Creation
   - All process are created from a parent process, except for **init** on linux (**launchd on OS X)
   - **init** (**launchd**)
      - Process created by the OS kernel
      - Must remain alive for the entire time the system is up and running, otherwise kernel panic occurs
      - PID = 1
      - All other processes on the system are descendants
    - forking to create a new process
      - Parent process makes a copy of itself
      - Child process memory space initially duplicates the parent
   - Shared resources
      - Parent process determine what to share: Open file descriptors, open network connections, others
      - Parent can share nothing make child completely independent
   - Parent process and Child process can run parallel
   - Parent may wait for child to terminate or can terminate child
   - Kill parent may or may not kill child process, unix support both
   - Child may load a different program into memory, replacing the copy of the parent("exec-ing") 
- 2 process type
   - Independent process: working alone
   - Cooperating process: working with other process

## fork
- Processed are created on Unix via the fork() system call
- in C lib unistd.h
- in python using os module

## fork() in C

```
#include <stdio.h>
#include <unistd.h>

int main (int argc, char ** argv)
{
  int pid;

  printf("Before forking in the parent\n");

  pid = fork();

  if (pid == 0 )
  {     
    printf("I am the child.\n");
    printf("I am the child process, my pid is:%d\n",getpid());
    printf("I am the child process, my parent pid is:%d\n",getppid());
  }     
  else  
  {     
    printf("I am the parent, my child pid is:%d\n",pid);
    printf("I am the parent process, my pid is:%d\n",getpid());
    printf("I am the parent process, my parent pid is:%d\n",getppid());
  } 
  return 0;
}    
```

## fork() in PYTHON 

```
#!/usr/bin/env python2
import os

print '----Before forking in the parent----'

pid = os.fork()
if pid == 0:
    print 'I am the child process.'
    print 'I am the child process, my pid is:', os.getpid()
    print 'I am the child process, my parent pid is:', os.getppid()
else:
    print 'I am the parent process, my parent pid is:', os.getppid()
    print 'I am the parent process, my pid is:', os.getpid()
    print 'I am the parent process, my child pid is:', pid
```

## Fork概念
一个进程定义，包括代码、数据和分配给进程的资源。fork（）函数通过系统调用创建一个与原来进程几乎完全相同的进程，也就是两个进程可以做完全相同的事，但如果初始参数或者传入的变量不同，两个进程也可以做不同的事。

一个进程调用fork（）函数后，系统先给新的进程分配资源，例如存储数据和代码的空间。然后把原来的进程的所有值都复制到新的新进程中，只有少数值与原来的进程的值不同。相当于克隆了一个自己。

由fork函数创建的新进程被称为子进程。fork函数被调用一次，但是返回两次。父进程返回的值是新进程的进程ID，而子进程返回的值是0。

## fork函数返回值的三种情况
- 返回子进程Id给父进程

   - 因为一个进程的子进程可能有多个，并且没有一个函数可以获得一个进程的所有子进程ID。
- 返回给子进程值为0
   - 一个进程只会有一个父进程，所以子进程总是可以调用getpid以获得当前进程Id以及调用getppid获得父进程Id.
- 出现错误，返回负值
   - 当前进程数已经达到系统规定的上限，这时errno的值被设置为EAGAIN
   - 系统内存不足，这时errno的值被设置为ENOMEM

>创建新进程成功后，系统中出现两个基本完全相同的进程，这两个进程执行没有固定的先后顺序，哪个进程先执行要看系统的进程调度策略

- 子进程执行代码开始位置
   - fork确实创建可一个子进程并完全复制父进程，但是子进程是从fork后面到那个指令开始执行。如果子进程也从main开头到尾执行所有指令，那么它执行到fork指令时也必定会创建一个个子子进程，子子孙孙无穷尽。

- 常见的两种应用场景
   - 一个父进程希望复制自己，使父、子进程同时执行不同的代码段。这在网络服务中是常见的。
   - 父进程等待客户端的服务请求，当这种请求到达时，父进程调用fork，使子进程处理此请求。父进程则继续等待下一个服务请求的到达

   -  一个进程要执行一个不同的程序。这是shell中常见的情况，子进程从fork返回后立即调用exec


## exec() Loading Another Program
- Fork a process(the parent process)
- In the child, load the new program into memory, replacing the copy of the parent process
   - exec system call
   - os.exec in Python
- Note that the process code that calls exec is **replaced** by the new program cde. Thus, any instructions after the exec call will NEVER run!

## exec arguments variations
- Fixed number of arguments vs an array of arguments
   - 1st arguments : is the name of the program being loaded
- Searching system path to find new program vs the full path
- Modifying environment variables seen by the new program 更改新程序中可见到的环境变量

## C exec() Functions
- int execl(const char *path, const char *arg,...)
- int execlp(const char *file, const char *arg,...)
- int execle(const char *path, const char *arg, char * const envp[]...)

- int execv(const char *path, char  *const argv[],...)
- int execvp(const char *file, char  *const argv[],...)
- int execvpe(const char *file, char  *const argv[], char * const envp[]...)

- "l": fixed number of arguments
- "v": fixed number of arguments

- "p": search the path to find the program
- "e": allow environment variables to be modified upon loading the new program

## Python exec Functions

```
import os
os.execl(path, arg0, arg1, ...)
os.execle(path, arg0, arg1, ..., env)
os.execlp(file, arg0, arg1, ...)
os.execlpe(file, arg0, arg1, ..., env)

os.execv(path, args)
os.execve(path, args, env)
os.execvp(file, args)
os.execvpe(file, args, env)
```
- in C:
   - execl("/bin/ls", "ls", "-l", NULL);
   ---
   - char *argv[] = {"ls", "-l", NULL};
   - execv("/bin/ls", argv);
   ---
   - execv()
```
#include <unistd.h>
 
int main(void) {
  char *binaryPath = "/bin/ls";
  char *args[] = {binaryPath, "-lh", "/home", NULL};
 
  execv(binaryPath, args);
 
  return 0;
}
   ```
   - execvp
```
#include <unistd.h>
 
int main(void) {
  char *programName = "ls";
  char *args[] = {programName, "-lh", "/home", NULL};
 
  execvp(programName, args);
 
  return 0;
}
```
   - execle()
```
#include <unistd.h>
 
int main(void) {
  char *binaryPath = "/bin/bash";
  char *arg1 = "-c";
  char *arg2 = "echo "Visit $HOSTNAME:$PORT from your browser."";
  char *const env[] = {"HOSTNAME=www.linuxhint.com", "PORT=8080", NULL};
 
  execle(binaryPath, binaryPath,arg1, arg2, NULL, env);
 
  return 0;
}

```

- Waiting on a child process
   - Parent process can wait for a child process to terminate
   - If parent process terminates before child, child becomes an orphan process
   - Child process that are designed to run in the background after the parent terminates are called **daemons or services**

## Process Management
- Process Context
   - Minimal set of state information that must be stored to allow a process to be stopped and re-started later
   - Information stored in the CPU
      - Contents of registers
      - Program Counter(PC)(aka Instrucion Pointer)指令运行到那一条了。
   - Information stored in RAM
- Context and Process Switches
   - Terms often used interchangeably, but there is difference
   - Context Switch: a process to kernel code, expensive operation
   - Process Switch: a process to another process, two context switches required
- Staring a Process
   - Process is create via fork()
   - Process is scheduled by CPU schedular to decide when and where to run
   - Process is started by Dispatcher
- Blocking operations
   - Definition: instructions that wait for something to be completed before allowing the program to progress. eg: reading data from file; database query
   - when a process request I/O, the process is removed from execution while the I/O device completed its operation. **Yield CPU core(s)** for use by another process
- Preemption
   - CPU scheduler must share the processor core(s) by remove(preemption) one process so that other can run
   - HW as keyboard, also produce events that OS must handle(interrupt)
- Process Control Block - PCB
   - OS data structure used to represent a process internally
   - Used for process switch
   - Blocks are stored in linked lists within the OS
- Process Scheduling Queues
   - Job Queue
   - Ready List (Read Queue)
   - Device Queue
## Process Schedulers - 3 types
- Job Schedular (Long-Term Schedular)
   - Long intervals (seconds to minites)
   - Selects processes to bring into ready list
- Mid-Term Schedular
   - Swap inactive process out to disk
   - Restores swapped process from disk on demand
- CPU Schedular (Short-Term Schedular)
   - Short intervals (milliseconds)
   - Selects processes from ready list to run on the CPU cores
- Dispatcher
   - Receives a PCB from the short-term schedular
   - Restores the context of the process (PC and register)
   - Switches back to user privilege level
   - Gives control of the CPU core to the process, jump to the location specified by the restored PC