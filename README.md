Lab3
====

ECE 281 Lab 3.
#Lab Requirements:
	* Critique FSM Shell code given.
	* All vhdl files available (with comments and updated headers)	


#Critique [Bad Code]
=======
 	--This line will set up a process that is sensitive to the clock

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

'''This code combines the flip flop and the process which stores the "next stage case." This is not in accordance to what we learned in class. We learned that  the code should be broken into three parts, the flip/flop, process next stage, and the output logic. By combining these steps, it makes it much harder to analyze and also understand. 


#Critique [BAD CODE]
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

######Fix: 

	-- Here you define your output logic. Finish the statements below

			floor <= "0001" when (floor_state = floor1      ) else

			"0010" when (  floor_state = floor2  ) else
			
			"0011" when (   floor_state = floor3) else
			
			"0100" when (     floor_state = floor4  ) else
			
			"0101" when (     floor_state = floor5  ) else
			
			"0110" when (     floor_state = floor6  ) else
			
			"0111" when (     floor_state = floor7  ) else
			
			"0001" when others;

######Reason: 
	
	This is a fix because it now covers all the cases. Before, the code didn't cover all "other cases"
	
#Critique [BAD CODE]
====
	
	process (state_reg, count_reg) is
	begin
		state_next <= state_reg;
		
		if count_reg = TICKS_IN_MS then
			case (state_reg) is
				when s0 =>
					state_next <= s1;
				when s1 =>
					state_next <= s2;
				when s2 =>
					state_next <= s3;
				when s3 =>
					state_next <= s0;
			end case;
		end if;
	end process;
	
######Reason:

	The reason this is bad is because if the state is anything other then specified this code would crash. There is nothing specified for "other"



######Fix: 

	
	process (state_reg, count_reg) is
	begin
		state_next <= state_reg;
		
		if count_reg = TICKS_IN_MS then
			case (state_reg) is
				when s0 =>
					state_next <= s1;
				when s1 =>
					state_next <= s2;
				when s2 =>
					state_next <= s3;
				when s3 =>
					state_next <= s0;
				when others => state_next<=s1;
			end case;
		end if;
	end process;

######Reason: 
	
	This is a fix because it now covers all the states. Before, the code didn't cover all "other states"

#Critique [BAD CODE]
====
	
		process (state_next, sseg_reg, sel_reg, sseg0, sseg1, sseg2, sseg3) is
	begin
		sseg_next <= sseg_reg;
		sel_next <= sel_reg;
		
		case (state_next) is
				when s0 =>
					sseg_next <= sseg0;
					sel_next <= "1110";
				when s1 =>
					sseg_next <= sseg1;
					sel_next <= "1101";
				when s2 =>
					sseg_next <= sseg2;
					sel_next <= "1011";
				when s3 =>
					sseg_next <= sseg3;
					sel_next <= "0111";
			end case;
	end process;
	
######Reason:

	The reason this is bad is because if the state is anything other then specified this code would crash. There is nothing specified for "other"



######Fix: 

		process (state_next, sseg_reg, sel_reg, sseg0, sseg1, sseg2, sseg3) is
	begin
		sseg_next <= sseg_reg;
		sel_next <= sel_reg;
		
		case (state_next) is
				when s0 =>
					sseg_next <= sseg0;
					sel_next <= "1110";
				when s1 =>
					sseg_next <= sseg1;
					sel_next <= "1101";
				when s2 =>
					sseg_next <= sseg2;
					sel_next <= "1011";
				when s3 =>
					sseg_next <= sseg3;
					sel_next <= "0111";
				when others =>
				sseg_next <= sseg0;
					sel_next <= "1110";
				
			end case;
	end process;

######Reason: 
	
	This is a fix because it now covers all the states. Before, the code didn't cover all "other states"
	
	

=============

#Functionality:

Required Functionality:
*	Demonstrated Basic Elevator Controller *[COMPLETE]*
*	Demonstrate Mealy Elevator Controller *[COMPLETE]*

B Functionality:
*	Demonstrated More Floors *[COMPLETEd INCORRRECTLY]*
	*	The moore elevator logic was incorrectly implemented thus, the floor doesn't increment correctly.
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

i spoke with C3C Terragnoli about how to implement the changing light. Captain silva also guided me with the changing lights.
