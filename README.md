PRISM_Wardner
=============
###ALU
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
#####Simulation
![alt tag](https://raw.githubusercontent.com/EricWardner/PRISM_Wardner/master/ALU_Simulation.PNG)

###Datapath
#####Simulation
![alt tag](https://raw.githubusercontent.com/EricWardner/PRISM_Wardner/master/DataPath_Simulation.PNG)
