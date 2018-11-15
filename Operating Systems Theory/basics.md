# Operating Systems Basics
## Path
An operating system works on a base, from the start:
- Hardware
- Bootloader
- Kernel
- Syscall
- Userland

### Hardware
The computer's components. To use the hardware, the programs need to use the kernel's syscall.  
For instance, if Discord wants to access the webcam, it need to perform a syscall.

---

## Types of OS
### Mono-process vs multi-process
Named monolithic, a mono-processus operating system means that it can only run one process at a time. A multi-process OS, on the other hand, can run multiple process at the same time.

Today, monolithic OS doesn't exists anymore. Windows, MacOS, Linux, all of them are multi-process operating systems.

- Mono-users vs multi-users
- Families

---

## "Machine" language
Named `assembler`, it's used to make simple instructions to the processor like mathematical expressions on integers (ADD/SUB/DIV).

---

## Known hardware breaches
### Meltdown
Meltdown is a hardware vulnerability affecting Intel x86 microprocessors, IBM POWER processors, and some ARM-based microprocessors. It allows a rogue process to read all memory, even when it is not authorized to do so. 

Source: [Meltdown (security vulnerability) on Wikipedia](https://en.wikipedia.org/wiki/Meltdown_(security_vulnerability))

### Spectre
Spectre is a vulnerability that affects modern microprocessors that perform branch prediction. On most processors, the speculative execution resulting from a branch misprediction may leave observable side effects that may reveal private data to attackers. For example, if the pattern of memory accesses performed by such speculative execution depends on private data, the resulting state of the data cache constitutes a side channel through which an attacker may be able to extract information about the private data using a timing attack.

Source: [Spectre (security vulnerability) on Wikipedia](https://en.wikipedia.org/wiki/Spectre_(security_vulnerability))

### Out-of-Order execution
Process were able to access memory before even using it, meaning it was faster but accessed potential useless memory (Hyperthreading). This means some exploit could access the memory.

---

## Interruption
An interruption means a program make a syscall "Please Kernel gimme memory". The Kernel will generate an interruption, and send a request to a data bus. The processor will stop (realize an interruption). He then realize the action appropriated, and restart after sending answer to the Kernel.  

There is emergency level when there is several interruption. Keyboard is less important than Network hardwares.
How does the processor know how to react to a keyboard pressing ? On the boot, Kernel will load a table where there is all address and number of interruption, and it will then know how to react accordingly.

---

## Kernel Mode vs User Mode
What would disable me to fetch any memory ?  
Any processor has 2 mode : Kernel or User. 

In Kernel mode, processor can do whatever he wants.   

User mode : Hardware interruption disabled, no interruptino at all. You can't see all of the memory in user mode.  

When computer start-up, the bootloader starts a kernel mode. When Kernel load a program, he changes his mode accordingly to his rights.  

When you're in User mode you can't get back to Kernel mode. But you can go back to Kernel as the processor know he musts go back to Kernel mode.

As hardware interruption is not possible, he(the program who needs to write memory, for example) will do a software interruption that will request an hardware interruption. This is a two level interruption to reach Hardwares interruption in User mode

### Virtualisation
In virtualisation, there is no hardware interruption as there is no hardware. Those will be simulated by Hypervisor.  
The hypervisor of the host machine will fetch the request and stock request in a file. It will then deliver answers to the virtual machine.

---

## Boot
```
   Hardware 

      |
      v
+-------------+
|  Processor  |  <-- ASM  <--  BIOS EPROM (BIOS job is to read MBR and load it in memory)
+-------------+
      |
      v

RAM         Stock
            MBR Bootstrap
            Bootcode
            Kernel
            Init
```

### UEFI
MBR limited to 2.2 To.
Complete GPT supported (128 partitions of 9.4 Zo)
UEFI possess a Shell with which you can interract.
UEFI allows native Disk clones (Stormtroopers)
UEFI can make dedicated Partitions.(NTFS FAT, size > 40 Mo)
Drivers (material, NTFS) + Bootloader

## API syscall
A Syscalls allows an abstract access to an hardware. We do need not need to know how does work an hardware to access and use it.


## Monolithical UNIX
A Unix Kernel has 2 parts.
The high parts is all declenched by the userland, and by syscalls (API). 
The low parts purpose is bounded to Exceptions and interruptions (and its reactions appropriated) 





## Facts 
- A processor is way faster than any other hardware components, even flash memory.  
- The network harware generate a lot of Interruption (Make interruption at any request on a network).  
- A network flood generating a lot of interruption may be used as an attack.  
- If you try to make a division by 0 : It will generate an interruption and the OS will be requested to interract with this monstruosity. 
- There is two type of interruption, software interruptions and hardware interruptions.  

## Glossaire 
### MBR
Master Boot Record -> 512 first bytes of our Memory. A part of the MBR, the Bootstrap 416 first bytes whose purpose is to be loaded by the Bios to be executed by the process on Boot. 
me


### EPROM
Erasable Programmable Read Only Memory:


### BIOS 

### ASM
Machine language used to make processor's call. (ACRONIM MEANING ??????)
 
### RAM

### FAT

### UEFI

Unified Extensible Firmware Interface is the successor of BIOS. It's a micro-software used in most part of modern computers.

The UEFI is used to make an interface between the firmware and the OS.


### GPT

### NTFS
Disk encoding format.

### FAT
Another disk encoding format.