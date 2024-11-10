We have two ways to go to a memory address: Branch or jump.

Three types of formatting: I format, J Format and R Format.

[MIPS Instruction Formats](https://www.cs.kzoo.edu/cs315/Resources/MIPS/MachineXL/InstructionFormats.html)
### Branch Addressing
Takes two registers and a target address (**I Format**) as shown:

![[Pasted image 20241110124106.png]]

I format supports 16 bits, addresses are 32 bits. Generally doesn't matter because next address is usually nearby. Instruction addresses are multiples of 4 in 32 bit words.

![[Pasted image 20241110124526.png]]


Because of above, **PC-Relative Addressing** is used:
	**Target Address = (PC + 4) + offset * 4**
		Relates to the Datapath bc constant is pulled from the instruction and fed to the Shift Left 2 (same as offset * 4), and result is set to the Adder that takes the PC + 4 as input as well. Result from Adder is sent back to PC and used if PCSrc flag is true.

[Branch and Jump examples](https://courses.cs.washington.edu/courses/cse378/02au/Lectures/07controlI.pdf)
[MIPS Reference Data Sheet from UofA](https://uweb.engr.arizona.edu/~ece369/Resources/spim/MIPSReference.pdf)
### Jump Addressing

Uses J-format. Because only 26 bits available for address and addresses are 32 bits, need to use **Pseudo-Direct Jump Addressing**...
	Target address = (PC most sig bits) : (target address * 4)
		Get the above equation from the following assumptions:
		- Unlikely to jump so far that the PC top 4 bits change, so keep those. (32 - 4 so far). 
		- Lower two bits are always 0 in 32 bit words (32 - 4 - 2 = 26 needed for target address)