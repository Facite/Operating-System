# Chapter 1: Introduction

## Index

###### 1.1 [What do operating systems do?](#11-what-operating-systems-do)
+ 1.1.1 [User View of Operating Systems](#111-user-view-of-operating-systems)
+ 1.1.2 [System View of Operating Systems](#112-system-view-of-operating-systems)
+ 1.1.3 [Defining Operating Systems](#113-defining-operating-systems)

###### 1.2 [Computer-Systems Organization](#12-computer-system-organization)
+ 1.2.1 [Interrupts](#121-interrupts)
	+ 1.2.1.1 [Overview of Interrupts](#1211-overview-of-interrupts)
	+ 1.2.1.2 [Implementation of Interrupts](#1212-implementation-of-interrupts)
+ 1.2.2 [Storage Structure](#122-storage-structure)
+ 1.2.3 [I/O Sructure](#123-io-structure)

###### 1.3 [Computer System Architectures](#13-computer-system-architecture)
+ 1.3.1 [Single-Processor Systems](#131-single-processor-systems) 
+ 1.3.2 [Multiprocessor Systems](#132-multiprocessor-systems)
+ 1.3.3 [Clustered Systems](#133-clustered-systems)

###### 1.4 [Operating System Operation](#14-operating-system-operations)
___

### 1.1 What operating systems do?

##### Computer Systems
+ A computer system can be divided roughly into four components: 
	+ **Hardware**
		+ Computing resources for the system.
		+ Eg: CPU, Memory, I/O devices, Storage, etc. 
	+ **Application programs**
		+ Programs that define the ways in which resources can be used to solve the users' computing problems.
		+ Eg: Word processors, Compilers, Web Browsers, Spreadsheets, Medai Players
	+ **Operating system** 
		+ Software that manages the computer's hardware.
		+ Provides a building ground for application programs.
		+ Controls the hardware and coordinates its use (allocates it) among the various application programs for various users.
		+ Works as the intermediary between the computer user and the computer hardware.
	+ **User**
		+ The end-user who uses the computer system.
+ An operating system is similar to a government. Like a government, it performs no useful function by itself. It simply provides an environment within which other programs can do useful work.

#     
<!--Empty Heading--> 

#### 1.1.1 User View of Operating Systems
+ User's view of the OS depends on the interface being used.
+ For a laptop or PC, it is designed:
	+ For one user to monopolize its resources.
	+ Mostly for ease-of-use, with some attention to performance and security.
	+ Resource utilization is not given that much attention in this scenario.
+ For embedded computers in home devices/automobiles:
	+ They have little/no user view.
	+ OS designed primarily to run without user intervention.

#     
<!--Empty Heading--> 

#### 1.1.2 System View of Operating Systems
+ From the computer's point of view, the OS us the program closest to the hardware. 
+ OS is the **resource allocator**. It faces numerous requests for resources, and decides how to allocate them to specific programs and users.
+ A slightly varied view of the OS emphasizes on the need to control the various I/O devices and user programs. 
+ In this scenario, the OS is seen as a **control program** - a program that manages the execution of user programs to prevent errors and improper use of the computer. 

#     
<!--Empty Heading--> 

#### 1.1.3 Defining Operating Systems
+ Operating systems exist because they offer a reasonable way to solve the problem of creating a usable computing system.
+ There are broadly the following types of programs:
	+ **Kernel** - Core of the OS. It is always running and it is responsible for managing and allocating resources. 
	+ **Middleware frameworks** - A set of software frameworks that provide additional services (that is, provide services over what the kernel provides) to application developers.
	+ **System Programs** - They are associated with the operating system but are not necessarily part of the kernel. 
	+ **Application programs** - They include all programs not associated with the operation of the system.
+ For our purposes, the operating system includes the always-running kernel, middleware frameworks that ease application development
and provide features, and system programs that aid in managing the system while it is running. 

___

### 1.2 Computer-System Organization
+ A modern computer usually consist of one or more CPUs and a number of **device controllers** connected through **a common bus** that provides access between components and shared memory.
+ A device controller:
	+ Is in charge of a specific type of device (Eg: disk drive, audio device, graphics display).
	+ Maintains local buffer storage for the devices that it is connected to.
	+ Moves the data between the peripheral devices that it controls and its local buffer storage. 
+ Typically, an OS has a **device driver** for each device controller.
	+ The device driver understands the device controller, and provides the rest of the OS with a uniform interface to the device. 
+ The CPU and the device controllers can execute in parallel, competing for memory cycles. 
	+ A memory controller synchronizes access to the memory to ensure orderly access to the shared memory.

##### Diagramatic representation of flow of control
+ &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; || 
+ OS ----> Device Driver ----> Device Controller -----> I/O Device 
+ &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; || &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; |---------> Local Buffer Storage

#     
<!--Empty Heading--> 

#### 1.2.1 Interrupts
+ Consider a typical I/O operation (Eg: to read input from keyboard):
	+ The device driver loads the appropriate registers in the device controller.
	+ The device controller examines the contents of these registers to determine what action to take (read input from keyboard).
	+ The device controller starts the transfer of data from the device (keyboard) to its local buffer.
	+ The device controller informs the device driver when it has finished the operation.
	+ The device driver then gives control to other parts of the OS (Returns the data read from keyboard).
+ The device controller informs the device driver that it has finished its operation with the help of interrupts. 

#     
<!--Empty Heading--> 

##### 1.2.1.1 Overview of Interrupts
+ Interrupts must be handled quickly as they occur very frequently.
+ The interrupt architecture must also save the state information of whatever was interrupted, so that it can be restored after the interrupt is serviced.
+ Interrupts are a key part of how the OS and the hardware interact.
	+ Hardware may trigger an interrupt at any time by sending a signal to the CPU (usually through the system bus).
	+ When the CPU is interrupted, it stops what it is doing and immediately transfers execution to the starting address of the service routine for the interrupt.
	+ A simple way to do that is to invoke a generic routine that examines the interrupt information, based on which the generic routine calls the interrupt-specific routine.
	+ A faster way to do the same is to use **interrupt vectors**. 
		+ They are a table of pointers to the various interrupt service routines.
		+ The specific interrupt routine needed to be exectued for a given interrupt is called indirectly through the table, without any other intermediate routine being used.
		+ This table is generally stored in low memory (the first hundred or so locations).
		+ Windows and UNIX use this mechanism to dispatch interrupts.  
	+ After the interrupt service routine is completed, the CPU resumes the interrupted computation.

#     
<!--Empty Heading--> 

##### 1.2.1.2 Implementation of Interrupts

##### Basic interrupt mechanism
+ CPU hardware has a wire called the **interrupt-request line**, which is senses after executing every instruction to check if a device controller has **raised an interrupt** on it.
+ When the CPU **catches an interrupt**, it uses the interrupt number as an index into the **interrupt-vector**. 
+ Using the vector table, it calls the required **interrupt-handler routine** and **dispathes the interrupt** to that handler.
+ The interrupt-handler routine **clears the interrupt** by:
	+ Saving the states that it will be changing during its operation.
	+ Determining the cause of the interrupt. 
	+ Performing the necessary processing and servicing the device that raised the interrpt. 
	+ Performing a state restore and returning the CPU to the execution state it was in prior to the interrupt.

##### Problems with the basic interrupt mechanism
+ In modern computers, we need more sophesticated features like:
	+ Ability to defer interrupt handling during crital processing.
	+ A more efficient way to dispatch the proper interrupt handler for a device. 
	+ Different levels of interrupts, so that the OS can respond to them with the appropriate degree of urgency.

##### Implementing the features
+ Using **two interrupt lines**:
	+ The non-maskable interrupt line which is reserved for interrupts that the system cannot ignore.
	+ The maskable interrupt line which can be turned off the CPU while in critical processing.
+ Using **interrupt chaining** as a hybrid approach to dipatch the proper interrupt handler: 
	+ Usually computers have more interrupt-handlers than they have address elements in the vector-table.
	+ So each element in the interrupt vector table points to the head of a list of interrupt handlers (instead of one).
	+ When an interrupt is raised, all the handlers on the corresponding list are called one by one, until one is found that can service the request.  
	+ This is a compromise solution between the overhead of a huge interrupt table and the inefficiency of using a generic interrupt routine.
+ Using a system of **interrupt priority levels**:
	+ The levels enable the CPU to defer the handling of low-priority interrupts without masking all interrupts.
	+ It also makes it possible for a high-priority interrupt to preempt the execution of a low-priority interrupt.

#     
<!--Empty Heading--> 

#### 1.2.2 Storage Structure
+ The access time, distance from CPU, and the storage size usually increase as we go from the top to the bottom of the table.
+ Apart from the storage medias listed, computers also use ROM. It is used to run the power-on bootstrap program, which loads the OS from the secondary memory to the primary memory. 

| Name                   | Volitality   | Storage level  | Storage type |
|------------------------|--------------|----------------|--------------|
| Registers              | Volatile     | Primary        | Electronic   |           
| Cache                  | Volatile     | Primary        | Electronic   |
| Main Memory (RAM)      | Volatile     | Primary        | Electronic   |
| SSD/Flash memory       | Non-volatile | Secondary      | Electronic   |
| Hard Disk (HDD)        | Non-volatile | Secondary      | Mechanical   |
| Optical Disk (CD/DVD)  | Non-volatile | Tertiary       | Mechanical   |
| Magnetic Tapes         | Non-volatile | Tertiary       | Mechanical   |

##### Volitality
+ Volatile storage loses its contents when the computer is powered off. 
+ Non-volatile storage retains data even when the system is powerd off.

##### Storage levels
+ Primary memory is volatile in nature and acts as the working space for the processor, which accesses it directly. 
+ Secondary memory is used to hold data on a more permanent basis. When the processor needs data that is present in the secondary memory, it is first loaded in the primary memory. 
+ Tertiary memory are storage medias that are so large and slow that they are mostly used for special purposes, like storing backup copies.  

##### Storage types
+ Mechanical storage is generally larger and less-expensive-per-byte than electrical storage. 
+ Electrical storage is typically faster than mechanical storage.


#     
<!--Empty Heading--> 

#### 1.2.3 I/O Structure
+ A large portion of the OS is dedicated to managing the I/O.
+ The interrupt-driven I/O described in [Section 1.2.1](#121-interrupts) is fine for moving small amounts of data, but can produce high overload when large amounts of data is involved.
+ **Direct Memory Access (DMA)** is used to solve this problem.
	+ The device controller itself transfers an entire block of data directly in between its I/O device and main memory, with no intervention by the CPU. 
	+ Only one interrupt is generated per block, to tell the device driver that the operation has completed (instead of generating one interrupt per byte).
	+ While the device controller is performing these operations, the CPU is available for other work.

___


### 1.3 Computer-System Architecture

#### 1.3.1 Single-Processor Systems 
+ A core is the processing-unit component of the CPU. It executes instructions and registers for storing data locally. 
+ Years ago, most of the computers had a simple CPU and a single core. 
+ This core was capable of executing a general-purpose instruction set, including instructions from processes.

##### Special-purpose processors
+ Those single-core processor CPUs had device-specific special-purpose processors as well (for disk, keyboard, graphics, etc).
+ These special-purpose processors can only run a limited instruction set and can't run other processes. For example:
	+ A disk-controller microprocessor would recieve a sequence of requests from the main CPU core. 
	+ Then it implements its own disk queue and scheduling algorithm. 
	+ This way, the main CPU is relieved of the overhead of disk scheduling.
+ Sometimes, the special-purpose processors are low-level components built into the hardware. The OS can't communicate with them directly and they do their jobs autonomously. For example: 
	+ PCs contain a microprocessor in the keyboard to convert the keystrokes into codes to be sent to the CPU.
+ The use of special-purpose microprocessors is common, and does not turn a single-general-purpose-CPU-with-a-single-processing-core system into a multiprocessor system.

#     
<!--Empty Heading--> 

#### 1.3.2 Multiprocessor Systems
+ Multiprocessor systems dominate the computing landscape - from mobile deives to servers. 
+ Their main advantage is the increased throughput. 
+ They can broadly be categorised in two types:
	+ Systems with multiple single-core processors.
	+ Systems with multiple cores on a single processor. 

##### Systems with Multiple Single-Core Processors
+ The processors share resources like the computer bus, the clock, the memory, etc.
+ The speed-up ratio with N processors is not N, because 
	+ Certain overhead is incurred in communication between the processors, and
	+ Contention for shared resources adds to the lowering of expected gain.

##### Symmetric Multiprocessing structure
+ Multiprocessor systems most commonly use symmetric multiprocessing (SMP), where
	+ Each processor performs all tasks, including operating-system functions and user processes. 
	+ Each processor has its own set of registers, as well as a private (or local) cache. 
	+ All processors share the physical memory over the system bus.
	+ As the processors are separate, one may be sitting idle while another is overloaded, resulting in inefficiencies.
	+ To lower the workload variance between processors, dynamic sharing of processes and resources among the various processors can be implemented.

##### Systems with a Single Multi-Core Processor
+ Multicore systems can be more efficient than systems with multiple single-core processors, as on-chip communication is faster than between-chip communication.
+ Additionally, one-chip-with-multiple-cores uses significantly less power than multiple-single-core-chips (an important issue for mobiles and laptops).
+ Most multi-core systems have the design where:
	+ Each core has its own registers and a local cache (often known the level 1 or L1 cache).
	+ The processor has level 2 (or L2 cache) that is shared by all the cores on the processor.
	+ The local lower-level caches are generally smaller and faster than the higher-level shared caches.
+ All major modern OS (including Windows, macOS, Linux, Android, and iOS) support multicore SMP systems.
+ As per [wiki](https://en.wikipedia.org/wiki/Symmetric_multiprocessing), "In the case of multi-core processors, the SMP architecture applies to the cores, treating them as separate processors."

##### Non-Uniform Memory Access structure
+ As mentioned earlier, adding additional cores/processors doesn't scale well after a point because contention for the system bus becomes the bottleneck.
+ A solution to that is to use the NUMA structure, where: 
	+ Each CPU (or group of CPU) is provided with their own memory, that they can access via a smaller and faster local bus. 
	+ The CPUs are connected together by a **shared system interconnect** (so all CPUs share one physical address space). 
+ As a CPU can access its memory fast and without any contention on the system interconnect, NUMA systems can scale more effectively. 
+ A drawback with a NUMA system is the increased latency when a CPU needs to access memory across the system interconnect. The OS tries to minimize this latency through careful CPU scheduling and memory management.
+ As NUMA systems can scale to accommodate a large number of processors, they are becoming increasingly popular on servers and high-performance computing systems. 

##### Blade Servers structure
+ These are systems in which multiple processor boards, I/O boards, and networking boards are placed in the same chassis.
+ Each blade processor board boots independently and runs its own operating system. 
+ Some blade-server boards are multiprocessor as well, which blurs the lines between types of computers, as these servers consist of multiple independent multiprocessor systems.

#     
<!--Empty Heading--> 

#### 1.3.3 Clustered Systems 
+ Clustured systems can be thought of as another type of multiprocessor system.
+ They consist of two or more individual systems (or nodes) joined together. 
	+ Each node is typically a multicore system in itself.
	+ The nodes are said to be **loosely coupled**.
+ The generally accepted definition is that clustered computers share storage and are closely linked (using LAN or a faster interconnect).

##### Clustering for providing high availability services
+ High avalibility services are services which continue even after one or more systems in the cluster fail. 
+ This is done by adding a level of redundancy in the system.
	+ A "cluster" software runs on each node which monitors one or more of the other nodes.
	+ If/when a monitored node fails, the monitoring node takes ownership of its storage and restarts the applications that were running on the failed machine.
	+ The users of the service only expeirence a brief interruption of service instead of facing an outage.
	+ This way, high avalibality provides increased reliability.
+ The ability to continue functioning proportional to the surviving hardware is called **graceful degredation**.
	+ **Fault tolerant** systems can suffer failure of any single component and still continue operation. 
	+ They employ a mechanism that allows the system to detect, diagnose, and possibly correct the failure.
+ Clustering can be structured asymmetrically or symmetrically.
	+ **Asymmetric Clustering**:
		+ One node runs the application while the other remains in a **hot stand-by mode**. 
		+ The hot stand-by node just monitors the active server.
		+ When the active server fails, the stand-by node takes over and becomes the active server.
	+ **Symmetric Clustering**:
		+ Two are more hosts, all of them running applications. 
		+ They monitor each other and when one node goes down, the monitoring node takes over.
		+ It is more efficient as it utilizes the avaliable hardware better.

##### Clustering for provinding high performace computing
+ As clusters are several computer systems connected via a high-speed network, they can be used to provide high-performance computing environments.
+ Clusters can provide a significantly higher amount of computational power as compared to single-processor or SMP multiprocessor systems, because they can run different parts of an application concurrently on their systems. 
+ However, the application must specifically be written in a way to take advantage of the cluster.
	+ This involves a technique called **parallelization**.
	+ With the help of parellalization, the original problem is divided into separate components.
	+ Each of the components are then run parallely on individual cores of a computer or on individual computers of a cluster.
	+ The results from all the nodes are later combined to form the final answer.

##### Clustering for simultaneous data access
+ Another form of clustering is **parallel clustering** - which allows multiple hosts to access the same data on a shared storage.
+ Most OS lack support for simultaneous data access by multiple hosts, hence parallel clusters usually require special versions of softwares.
+ For example, "Oracle Real Application Cluster" is a version of Oracle’s database that has been designed to run on a parallel cluster.
	+ Each machine runs Oracle and has full access to the data on the shared database. 
	+ The software tracks access to the shared disk (access control).
	+ It also provides a locking mechanism to ensure that no conflicting operations occur (distributed lock manager).
+ Cluster technology is changing rapidly, and with fast WANs, some cluster products support nodes that are separated by miles. 
+ The concepts of **Storage Area Networks (SAN)** is helpful in the advancements. 
	+ In SAN, many systems attach to the same pool of storage. 
	+ If the applications and their data is in the SAN, then the cluster software can assign any host connected to the SAN to run an application.
	+ In a database cluster, dozens of hosts sharing the same database greatly increases performance and reliability.

___


### 1.4 Operating System Operations 
