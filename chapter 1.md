# Chapter 1: Introduction

## Index

###### 1.1 [What do operating systems do?](#11-what-operating-systems-do)
+ 1.1.1 [User View of Operating Systems](#111-user-view-of-operating-systems)
+ 1.1.2 [System View of Operating Systems](#112-system-view-of-operating-systems)
+ 1.1.3 [Defining Operating Systems](#113-defining-operating-systems)

###### 1.2 [Computer-Systems Organization](#12-computer-system-organization)
+ 1.2.1 [Interrupts](#121-interrupts)


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


#### 1.2.1 Interrupts
+ Consider a typical I/O operation (Eg: to read input from keyboard):
	+ The device driver loads the appropriate registers in the device controller.
	+ The device controller examines the contents of these registers to determine what action to take (read input from keyboard).
	+ The device controller starts the transfer of data from the device (keyboard) to its local buffer.
	+ The device controller informs the device driver when it has finished the operation.
	+ The device driver then gives control to other parts of the OS (Returns the data read from keyboard).
+ The device controller informs the device driver that it has finished its operation with the help of interrupts. 
