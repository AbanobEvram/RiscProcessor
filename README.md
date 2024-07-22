# Introduction:
In this research we concerned to implement a 16-bit length MIPS single cycle processor which contains seven 16-bit general purpose registers (R1-R7). Also, R0 is hardwired to zero and cannot be written. There are only three instruction format which are R-type, I-type, and J-type. Each instruction is only 16 bits. 
A single cycle processor is a processor that carries one instruction in a single cycle clock. An instruction is fetched from the memory, then decoded, and executed to store the result in a single clock cycle. This is a simple model of MIPS processor in the terms of the hardware requirements. It has a poor data throughput (limited number of instructions).

# Register file:
Mainly, a register file is a set of 16-bit processor registers (8 registers) in the central processing unit (CPU). It contains seven 16-bit registers (R1-R7) with two read and one write port. In addition to the R0 register which is hardwired to zero. These registers are made of D flip-flops. 
Our implementation relies on one decoder to choose in which register we prefer to write. Also, we use 7 AND gates for input enabled for all registers. Moreover, two multiplexors of 3 selectors to decide which register its data will be exhibited on the BUS A. Since BUS A represents the data in the register RS and BUS B represents the data in RT.

# ALU:
It is a 16-bit arithmetic and logical unit used to perform all the required operations mentioned. We have about 16 different operations which can be chosen through the ALU MUX (ALU signal). In our ALU, there are two inputs (A), (B), and a MUX with 4 bits selector (ALU signal) in order to choose which operation is required to be performed. 
We used a comparator to check the zero flag. According to the shifting functions, input B is used as the shift amount so, we used a splitter to control the number of bits which will be considered as the shift amount. We also checked the over flow in addition and subtraction operations by Xoring the most significant bit of the output with the carry out. If the most significant bit of the output and the carry out are the same so there is no overflow, if not, so the overflow signal is activated. 
Furthermore, we used two other comparators; the first one is an unsigned comparator to check the set less than unsigned (SLTU). The second one is to check SLT and all branches. The result of the branches is one bit so we used an extended to extend its value. 
## The operations are: 

# ALU Control:
The ALU Control unit receives two input signals which are the ALU-OP from the main control unit and 2-bit function (9&10) from the output of the instruction memory. These two input signals identify which operation will be performed and determine whether the instruction is R-type or I-type. Since the OP code of some R-type instruction is the same so the 2 bit function is necessary to clarify which function is desired. While in I-type, all the operations have different OP code. Thus; we used two MUXs to know which operation will be activated and both MUXs have an enable signal to activate one and deactivate the other. 
The role of the Main MUX in the figure is to differentiate which MUX will be used. If the enable 1 signal is 0 and enable 2 signal is 1 then the Main Mux selects the I-type multiplexer. Finally the result is represented in 4 bits on the ALU signal which will be used in the ALU to identify specifically which operation will be performed.
## The following shows the truth table of the ALU signal:


# Control Unit:
The control unit is meant to be the brain of the whole processor as it controls every single sub-device in the processor. Each device in the processor has signals these signals are defined in the control unit. This control unit contains only one input which is the OP-Code. Actually, we used a decoder with the OP-code signal (5 bits) which identifies each operation according to its op-code. The first to outputs are ored together and passed to the R-type because the R-format is identified by OP Code 0 or 1. Op-code 2 is R-format also but a special case since it is a jump instruction.

## Control unit signal equations:
### 1-MEMORY (RAM) SIGNALS:
Memory write = SW 
Memory read = LW 
Memory Enable = SW + LW
### 2-REGDST:
Signal which is used as a selector to select which Register will be accessed in the register file (destination register). 
RegDst (00) = ANDI + ORI + XORI + ADDI + SLL + SRL + SRA + ROR + LW ? RT 
RegDst (01) = R- TYPE ?RD 
RegDst (10) = LUI ? R1 
RegDst (11) = JAL ? R7
### 3-REGWRITE:
A signal enables writing in the register file if it is 1. 
RegWrite = ANDI + ORI + XORI + ADDI + SLL + SRL + SRA + ROR + LW + JAL + LUI + R_TYPE.
### 4-ALUSRC:
A signal enables us to choose to between BUSB, signed Imm5, and unsigned Imm5. 
ALUSrc (00) = R-type + BEQ + BNE + BLT + BGE 
ALUSrc (01) = ADDI + LW + SW 
ALUSrc (10) = ANDI + ORI + XORI + SLL + SRL + SRA + ROR
### 5-MEMORY TO REGISTER:
A signal used to select what will return to BUSW (ALU operation or value from memory or JAL or LUI) 
MemtoReg (00) = ANDI + ORI + XORI + ADDI + SLL + SRL + SRA+ ROR + R_TYE 
MemtoReg (01) = LW 
MemtoReg (10) = JAL 
MemtoReg (11) = LUI
### 6-BRANCH SELECTOR:
This signal is to branch to a certain label with a specific address. 
Branch selc = (BEQ * ZERO FLAG) + (BNE * ZERO FLAG) + (BLT * ALU RESULT) + (BGE* ALUR ESULT).
### 
7-JUMP SELECTOR:
A signal used to select whether you branch or jump. 
Jump selc (00) = ANDI + ORI + XORI + ADDI + SLL + SRL + SRA+ ROR + R_TYE + BEQ + BNE + BLT + BGE + LUI + LW + SW 
Jump selc (01) = J + JAL 
Jump selc (10) = JR * FUNC

  Each design in the in the processor is illustrated using these signals in the control unit. So, we used (AND) and (OR) gates to implement these signals. We also tend to use some encoders to get the signals needed to be passed to other devices to resume its processing. All of these signals own a truth table which simply shows how and when it works. 
