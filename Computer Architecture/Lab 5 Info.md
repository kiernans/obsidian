

![[Pasted image 20241110150843.png]]
## Notes for the above diagram
Instruction\[20-16\] and Instruction\[15-11\] seem to be the RT and RD registers respectively.  They are taken so that the Write Register step is done during the WriteBack stage, telling what register to write the data to. 

In other words, when the arrow at the end goes back to the Write Data section, we also need to know what register to write the data to, which the above two arrows will provide that. The register chosen is controlled by the RegDst flag.


![[Pasted image 20241110124106.png]]

[[Memory Addressing|Branching]] happens when the Controller sets the Branch flag and the ALU sets the Zero flag. The Zero flag represents when the values of RS and RT are the same and is found by subtracting them and checking if the result is zero. 

![[Pasted image 20241110160049.png | Table for Controller logic]]

## Lab Notes

Controller Shift Check seems to be the offset when branch instructions are fed as input.

## Working so far...
- Tested branching instructions to see if they jump to their given labels and they seem to work
- J, Jal and Jr also seem to work


## Issues found...

Fails at NOR for some reason...??? The NOR of 0000000a and 00000006 results in fffffff1, which seems to be correct for 32 bits.

![[Pasted image 20241110170347.png]]



NOT ENOUGH MEMORY WAS ALLOCATED IN INSTRUCTION MEMORY CAUSING ONLY UP TO NOR INSTRUCTION TO BE LOADED INTO MEMORY AHHHHHHHHHHHHHHHHHHH (should be 0:294 (number of lines in instruction.mem file))😭😭😭

There should be no output when doing branches, jumps, sll, etc...

Jr instruction jumps to the j b6 instruction, which jumps to the b6 label....which branches to exit if BLTZ. Gets to the BLTZ section in ALU control, gets to the SLL module to shift offset, seems to get to the Branch ADDER with the shifted offset, gets to the ZeroANDBranch, but MEM_Zero is false. 

Issue seems to be that in ALUOp == 14 (BLTZ case), A contains a very big number, which returns 1 in this case. Then jumps below to see if ALUResult  == 0 and it's not, but we want ALUResult == 0. Looks like A is from $t5, which gets added by -1 at dummy_func. Maybe this add is going wrong.

EX_SEOutput is giving 0001ffff, when it should be 0000ffff for -1. Something must be wrong with the SignExtend. SignExtend was not handling Signed Extensions properly... needed to be 'hFFFF instead of 'b1. The latter doesn't add enough 1's to the upper 16 bits, resulting in a really big non-negative number. Now decrements properly to allow for branching to EXIT.


## Changes

- Commented out SRL and _ShiftMux
- Changed ALU to take ReadData1
- Commented out BLTZ case in Controller.v bc same as BGEZ
- Increased memory size in InstructionMemory.v
- Fixed SignExtension.v by changing from 16'b1 to 16'hFFFF