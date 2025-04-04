### Windows Kernel and HAL
#### Kernel
>  The kernel is the core of the operating system and has control over the entire computer. It handles all of the input and output requests, memory, and all of the peripherals connected to the computer.
>  - Kernel is not completely dependent on HAL to communicate with hardware.

#### HAL
> A hardware abstraction layer (HAL) is software that handles all of the communication between the hardware and the kernel.

- User mode: installed application code
- Kernel mode: operating system code


> [!NOTE] Difference in user and kernel mode
> Kernel mode code and operating system code work on the same space which means if there is an application that crashes in kernel level, it might affect the whole system to crash.
> But in user mode, the kernel allocates separate space for the applications which will ensure that even if an application crashes the operating system still keeps running.


![[Pasted image 20250404212438.png]]


### File system types
NOTE
- exFAT
	- supported by many operating systems
	- not used commonly as a storage since it has restrictions to amount of partitions stored in the single disk
	- FAT32 is more common than FAT16 since it has fewer restrictions
- Hierarchial File system plus (HFS+):
	- commonly used by MacOS since it supports longer:
		- filenames
		- file sizes
		- partition sizes
	- not supported for windows but windows can read data from it
- Extended File System (EXT):
	- it used for linux
	- windows not supported but can view data using **special software**
- New Technologies File System (NTFS):
	- most common file system in windows)
	- it supports all versions of windows and linux
	- macOS can only read from it but also can write using by installing drivers
	- has recovery & security features
	- tracks permissions and timestamps
	- supports FS encryption
	- it tracks timestamps for MACE (Modify, Access, Create and Entry Modified) which makes it better for investigation and debugging
	- **When formatted for NTFS, it creates below structures**
		- **Partition Boot Sector** - This is the first 16 sectors of the drive. It contains the location of the Master File Table (MFT). The last 16 sectors contain a copy of the boot sector.
		- **Master File Table (MFT)** - This table contains the locations of all the files and directories on the partition, including file attributes such as security information and timestamps.
		- **System Files** - These are hidden files that store information about other volumes and file attributes.
		- **File Area** - The main area of the partition where files and directories are stored.


> [!NOTE] Cleaning up a drive
> When formatting a drive, data might not be cleaned completely, that is why performing a **secure wipe** is important when reusing a drive.

### Windows Boot Process

- Firmware: BIOS or UEFI
	- BIOS: Basic Input Output System - Found in 1980 - doesn't support new features
	- UEFI: Unified Extenfible Firmware Interface - created to replace BIOS

1. POST (power on self test)
	1. make sure every device is communicating
	2. ends when the system disk is discovered
	3. lastly, it looks for Master Boot Record or **MBR**
2. MBR
	1. consists of small program to locate and load the OS
	2. BIOS executes the program to load the OS
3. Bootmgr.exe
	1. after windows installation is run the bootmgr.exe is run right away
	2. it switches the system from real mode to protected mode to use all of the system memory
	3. it reads the BCD or Boot Configuration Database
4. BCD
	1. it runs any additional code needed to start the computer
	2. indication ? hibernation
		1. it continues with **Winresume.exe** which allows to read Hyberfil.sys
		2. Hyberfil.sys contains the state of the computer when it was put into hibernation
	3. indication ? cold start
		1. **Winload.exe** is loaded
		2. Winload.exe creates hardware configuration in the registry.
			1. registry has records of all settings, configurations, options and others
		3. Winload.exe uses Kernel mode code signing KMCS to make sure kernels are digitally signed
		4. Winload.exe runs Ntoskrnl.exe which starts windows kernel and sets up HAL
5. Session Manager Subsystem (SMSS):
	1. Session Manager Subsystem (SMSS) reads registry
	2. sets up user environments
	3. sets up **Winlogon** service

![[Pasted image 20250404215443.png]]


