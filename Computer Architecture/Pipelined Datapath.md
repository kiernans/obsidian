[Pipelined Datapath Example](https://www.cs.fsu.edu/~zwang/files/cda3101/Fall2017/Lecture8_cda3101.pdf)

[Example of Datapaths for different instructions](https://www.cim.mcgill.ca/~langer/273/13-notes.pdf)

[Organization of Computer Systems: Processor & Datapath](https://www.cise.ufl.edu/~mssz/CompOrg/CDA-proc.html)

[Very good walkthrough for different instructions](https://www.cs.fsu.edu/~zwang/files/cda3101/Fall2017/Lecture5_cda3101.pdf)


**Above link shows instruction flow through datapath**

![[Pasted image 20241110122323.png]]

![[Pasted image 20241110150843.png]]

## Notes for the above diagram
Instruction\[20-16\] and Instruction\[15-11\] seem to be the RT and RD registers respectively.  They are taken so that the Write Register step is done during the WriteBack stage, telling what register to write the data to. 

In other words, when the arrow at the end goes back to the Write Data section, we also need to know what register to write the data to, which the above two arrows will provide that. The register chosen is controlled by the RegDst flag.

![[Pasted image 20241110124106.png]]

[[Memory Addressing|Branching]] happens when the Controller sets the Branch flag and the ALU sets the Zero flag. The Zero flag represents when the values of RS and RT are the same and is found by subtracting them and checking if the result is zero. 

![[Pasted image 20241110160049.png | Table for Controller logic]]


## Issues found...

Fails at NOR for some reason...??? The NOR of 0000000a and 00000006 results in fffffff1, which seems to be correct for 32 bits.

![[Pasted image 20241110170347.png]]