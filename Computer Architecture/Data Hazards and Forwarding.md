
### TLDR
#### - Create Forwarding Unit
#### - Hazard Detection Unit
#### - Move branching logic to ID stage
#### - Stall using the Mux after Control that Zeroes all of the Control flags

[Forwarding video](https://youtu.be/2kbi-UhTHks?si=Q91KXkRqKodCpJxU)
[Stalling and Control Hazards video](https://youtu.be/CKeXzX5zv30?si=HZQ_NAannQx4FiAb)

**Data Hazards** occur when you need to read from a register that is depending on a write to the same register in nearby instructions.

Need to support the following:
- Forwarding from beginning of MEM to beginning of EX
- Forwarding from beginning of WB to beginning of EX

This means we need to allow for the ALU to take input for both inputs from either the Register File (ReadData1 or ReadData2 like before), from the MEM stage, or from the WB stage (arrow leading away from the far right Mux)

## Creating Forwarding Unit

![[Pasted image 20241111134347.png]]

Above shows getting EX/MEM ALU Result and bringing it back to the two new Multiplexors needed for ALU inputs
![[Pasted image 20241111134545.png]]

Fowarding from WB stage to the new Muxes

![[Pasted image 20241111134812.png]]

Forwarding unit will provide Control signals to the new Muxes. Needs to know source and destination registers to see of forwarding is necessary; Above shows making that connection.

![[Pasted image 20241111135025.png]]

Also need Destination register and RegWrite from MEM. If not writing back to a register, then no need to forward (such as for branch or store word instructions. **Also this might be a hint for what you want to output in the simulation?**).

![[Pasted image 20241111135628.png]]

And Destination register and RegWrite from WB I guess... To know if register is actually being written back to Register file

6 Inputs for Forwarding Unit:
- EX_Instruction26b\[20:16\] (Rt)
- EX_SEOutput (Rs)
- WB_RegWriteAddress (WB Destination register)
- WB_RegWrite
- MEM_RegWrite
- MEM_RegWriteAddress (MEM Destination register)


![[Pasted image 20241111140615.png]]

Above shows condition for forwarding from MEM:
- Is the source register that's needed in EX the Destination register in MEM?
- Is the Destination register the 0th register? (Can't write to 0th register)
- Are we writing the MEM register back to the Reg File?

![[Pasted image 20241111140945.png]]

Similar to the one above, but checks if MEM Dest Register is the same as the EX 2nd Source Register (so ReadData2 instead of ReadData1)

![[Pasted image 20241111141313.png]]

The most recent being the **sub $1 $4 $5** instruction.

![[Pasted image 20241111141504.png]]

Above shows forwarding from the WB stage. Due to Double Data Hazard, we only want to take from WB stage if there is not a more recent instruction; that's why the **and not** section is necessary.

![[Pasted image 20241111141713.png]]

Same as above, but checking against Rt instead of Rs.

## Stalling

### Load Use Hazards

![[Pasted image 20241111141922.png]]

Because LW gives usable value at the WB stage, but the AND needs the contents 1 stage before, cannot forward, must stall. (Cannot provide value back in time)

![[Pasted image 20241111142300.png]]
After stalling using nop instruction, the WB stage of lw is aligned with the AND EX stage, allowing for forwarding.

## Creating Hazard Detection Unit

![[Pasted image 20241111142634.png]]
Insert nop into EX stage with the Mux above, cancelling out all of the Control signals for the EX stage

![[Pasted image 20241111142845.png]]
Stall the previous two instructions using above Control signals. **Don't allow PC to fetch next instruction and don't allow IF/ID to pass along already fetched instruction**

![[Pasted image 20241111142953.png]]

### To Identify Load Use Hazard, need to know...
- ##### If preceding instruction requires load instruction's register

![[Pasted image 20241111143109.png]]

![[Pasted image 20241111143624.png]]

- #### If load instruction is being used
![[Pasted image 20241111143446.png]]


![[Pasted image 20241111143750.png]]

**Stall if Load instruction and source register from load is needed by preceding instruction...**

## Branch Hazards

![[Pasted image 20241111144011.png]]

## Moving Branch handling to ID

![[Pasted image 20241111144358.png]]
Compare registers used to happen at the ALU with the Zero flag, now moved to end of ID stage. Branch Adder also moved into ID stage.  The AND instruction gets thrown out with IF Flush flag


![[Pasted image 20241111144424.png]]
Stall added in ID stage, former instruction was flushed and replaced with the instruction that the branch sent you to (**lw $4...**)


![[Pasted image 20241111164835.png]]

![[Pasted image 20241111165037.png]]

![[Pasted image 20241111165203.png]]

Since Branching logic has moved to the ID stage, a Data Hazard is caused if branch is preceded immediately by an ALU instruction (add, sub, mul, etc) or a load instruction was issued 2 instructions before. 

![[Pasted image 20241111170102.png]]

BEQ stalled and the ID stage is repeated to align with preceding instructions

![[Pasted image 20241111170303.png]]
Load stage is immediately before a branch instruction

![[Pasted image 20241111170335.png]]
So stall 2 cycles...



