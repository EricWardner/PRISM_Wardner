PRISM_Wardner
=============
###ALU
Given was a shell for the implementation of an ALU. The shell did not have the logic nessicary for a proper implentation. Looking at the PRISM manual each operation was matched with its function and opcode as follows. Without the following logic or with incorrect logic, the operations might not match their opcodes or their functions resulting in a failed ALU.
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
Initialy I attempted to implement the logic using ```if``` statements. After that wouldn't work I used ```CASE is when``` in a similar way as [Lab 3](https://github.com/EricWardner/ECE281_Lab3)to succesfully implement the logic.

###Datapath
#####Simulation
![alt tag](https://raw.githubusercontent.com/EricWardner/PRISM_Wardner/master/DataPath_Simulation.PNG)
