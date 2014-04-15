PRISM_Wardner
=============
###ALU
In basic terms, the ALU is the computer part that completes arithmetic operations. In the case of PRISM, those operations are

|  Mnemonic|Encoding|
|:--:|:--: |
| AND |  000|  
| NEG  |  001|  
| NOT  |  010| 
| ROR  |  011|
| OR  |  100| 
| IN  |  101|
| ADD  |  110|
| LD  |  111|

Given was a shell for the implementation of an ALU. The shell did not have the logic nessicary for a proper implentation. Looking at the PRISM manual each operation was matched with its function and opcode as follows. Without the following logic or with incorrect logic, the operations might not match their opcodes or their functions may have resulted in a failed ALU.
#####Code
```VHDL
CASE OpSel is
				when "000" =>
					Result <= Accumulator and Data;
				when "001" =>
					Result <= not(Accumulator) + "0001";
				when "010" => 
					Result <= not Accumulator;
				when "011" => 
					Result <= To_StdLogicVector(To_BitVector(Accumulator) ror 1);
				when "100" =>
					Result <= Data or Accumulator;
				when "101" =>
					Result <= Data;
				when "110" =>
					Result <= Accumulator + Data;
				when others =>
					Result <= Data;
			end case;
```
As a test of the code, the following waveform was simulated.
#####Simulation
![alt tag](https://raw.githubusercontent.com/EricWardner/PRISM_Wardner/master/ALU_Simulation.PNG)
The accuracy of the ALU was checked using the PRISM maunual. For example looking at the manual the opcode "110" preforms the operation "ADDI" which "Adds the value of the operand to the number in the accumulator." To check the simulation first the OpSel value "110' was found then looking at the waveform and knowing the operation, data (1001, 9) should be added with the accumulator (0010, 2) to equal 1. So the result should be 1011, which it is according to the waveform. This same process was used for ach change in "OpSel" to check the validity of the waveform.

######Debugging
Initialy I attempted to implement the logic using ``` if ``` statements. After that wouldn't work I used ``` CASE is when ``` in a similar way as [Lab 3](https://github.com/EricWardner/ECE281_Lab3) to succesfully implement the logic.

###Datapath
It is easiest to think of the datapath as the overall collection of funtions (including the ALU) that are used together to preform operations, this includes registors, selectors, and logic units. In PRISM these include 

**Program Counter**  - keeps track of position of the program in memory by generating the memory adderess of instructions and operands.

**Memory Adress Register** - Stores the addresses created by the operands of operations such as STA that store data in memory.

**Instruction Register** - Stores the current instructions opcode through the entire instruction cycle

**Address Selector** - Selects between the PC and MAR as the address source. 

**Jump Selector** - Loads value of MAR to PC in order to execute a jump to a different place in the memory (instructions).

Again, a shell for the Datapath was given. The ALU had not been implemented in the Datapath code yet, it was added as a component with a component and signal declaration. The only completed logic for the shell was the program counter, the rest of the above registers and selectors had to be implemented. Using the same format as the given Program Counter logic, the rest was implemented successfully. 

The given testbench was meant to simulate a program, there were some problems with the given code. It seemed that while commenting out code, the author forgot to change the arrangement of the semi-colons. Once complete the code worked perfectly. 

######Debugging
I first did not understand the "RESET_L" signal. I did not know that the "_L" indicated it was responsive when a '0'. At first all of my code looked like 
```VHDL
if(Reset_L = '1') then
```
it was changed correctly when the distinction was understood to
```VHDL
if(Reset_L = '0') then
```
#####Simulation
Using the datapath code a simulation was run with a given internal signals representing an Assembly program's execution through the datapath. The simulation of the program from 0ns to 50ns is shown below and matches Captain Silva's given simulation.

![alt tag](https://raw.githubusercontent.com/EricWardner/PRISM_Wardner/master/DataPath_Simulation.PNG)

###Reverse Engineering
#####0ns - 50ns Analysis
The simulation was then viewed from 0-50 ns and an attempt was made to reverse engineer the section of the program

![alt tag](https://raw.githubusercontent.com/EricWardner/PRISM_Wardner/master/50-100%20analysis.PNG)

The ROR is the only FULL operation that occurs in that time frame.
#####Jump analysis at 225ns

![alt tag](https://raw.githubusercontent.com/EricWardner/PRISM_Wardner/master/JumpSel_waveform.PNG)

The rest of the waveform was reverse engineered in a similar manner to get the following assembly code.
![alt tag](https://raw.githubusercontent.com/EricWardner/PRISM_Wardner/master/ReversedProgram.PNG)

