Lab3
====

ECE 281 Lab 3.
#Lab Requirements:
	* Critique FSM Shell code given.
	* All vhdl files available (with comments and updated headers)	


#Critique [Bad Code]
=======
-This line will set up a process that is sensitive to the clock

floor_state_machine: process(clk)

begin

	--clk'event and clk='1' is VHDL-speak for a rising edge
	
	if clk'event and clk='1' then
	
		--reset is active high and will return the elevator to floor1
		
		--Question: is reset synchronous or asynchronous?
		
		if reset='1' then
		
			floor_state <= floor1;
			
		--now we will code our next-state logic
		
		else
		
			case floor_state is
			
				--when our current state is floor1
				
				when floor1 =>
				
					--if up_down is set to "go up" and stop is set to 
					
					--"don't stop" which floor do we want to go to?
					
					if (up_down/="000" and stop='0') then 
					
						--floor2 right?? This makes sense!
						
						floor_state <= floor2;
						
						
					else
					
						floor_state <= floor1;
						
					end if;
					
				--when our current state is floor2
				
				when floor2 => 
				
				
######Reason: 

This code combines the flip flop and the process which stores the "next stage case." This is not in accordance to what we learned in class. We learned that  the code should be broken into three parts, the flip/flop, process next stage, and the output logic. By combining these steps, it makes it much harder to analyze and also understand.


#Critique [GOOD CODE]
====
-- Here you define your output logic. Finish the statements below

floor <= "0001" when (floor_state = floor1      ) else

			"0010" when (  floor_state = floor2  ) else
			
			"0011" when (   floor_state = floor3) else
			
			"0100" when (     floor_state = floor4  ) else
			
			"0101" when (     floor_state = floor5  ) else
			
			"0110" when (     floor_state = floor6  ) else
			
			"0111" when (     floor_state = floor7  ) else
			
			"0001";

######Reason: 

The reason this is a good code is because it is a seperate component as learned in class. As mentioned earlier the first two components, the flip flop and the process next stage is combined, but the output logic remains independent, thus is according to the teaching in class
=============

#Functionality:

Required Functionality:
*	Demonstrated Basic Elevator Controller *[COMPLETE]*
*	Demonstrate Mealy Elevator Controller *[COMPLETE]*

B Functionality:
*	Demonstrated More Floors *[COMPLETE]*
*	Demonstrated Change Inputs *[COMPLETE]*

A Functionality:
*	Demonstrated Moving Lights *[COMPLETE]*
*	Demonstrated Multiple Elevators *[INCOMPLETE]*



schematic:

![alt text](https://github.com/vipersfly23/Lab3/blob/master/Schematic.GIF?raw=true "Schematic")

Required VHDL Files: 

[Clock Divider](https://github.com/vipersfly23/Lab3/blob/master/Clock_Divider.vhd)

[Nexys2 Top Shell](https://github.com/vipersfly23/Lab3/blob/master/Nexys2_top_shell.vhd)

[Nexys2 SSEG](https://github.com/vipersfly23/Lab3/blob/master/nexys2_sseg.vhd)

[Nibble to SSEG](https://github.com/vipersfly23/Lab3/blob/master/nibble_to_sseg.vhd)

[PINOUT.ucf](https://github.com/vipersfly23/Lab3/blob/master/pinout.ucf)

## Documentation:

No help received.
