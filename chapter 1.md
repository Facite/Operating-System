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
+ OS ----> Device Driver ----> Device Controller -----> Device 
+ &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; |---------------> Local Buffer Storage

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






