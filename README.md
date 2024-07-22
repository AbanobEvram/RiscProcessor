# Introduction:
In this research we concerned to implement a 16-bit length MIPS single cycle processor which contains seven 16-bit general purpose registers (R1-R7). Also, R0 is hardwired to zero and cannot be written. There are only three instruction format which are R-type, I-type, and J-type. Each instruction is only 16 bits. 
A single cycle processor is a processor that carries one instruction in a single cycle clock. An instruction is fetched from the memory, then decoded, and executed to store the result in a single clock cycle. This is a simple model of MIPS processor in the terms of the hardware requirements. It has a poor data throughput (limited number of instructions).

# Register file:
Mainly, a register file is a set of 16-bit processor registers (8 registers) in the central processing unit (CPU). It contains seven 16-bit registers (R1-R7) with two read and one write port. In addition to the R0 register which is hardwired to zero. These registers are made of D flip-flops. 
Our implementation relies on one decoder to choose in which register we prefer to write. Also, we use 7 AND gates for input enabled for all registers. Moreover, two multiplexors of 3 selectors to decide which register its data will be exhibited on the BUS A. Since BUS A represents the data in the register RS and BUS B represents the data in RT.
