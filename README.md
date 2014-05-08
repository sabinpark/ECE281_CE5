ECE281_CE5
==========

Sabin's CE 5

## Task #1: *Simple MIPS Assembly Program*

*Goal*:  Write a MIPS assembly program that will initialize registers $S0 and $S1 with the decimal values 44 and -37 respectively.  Use a register instruction to add these values together and store the result in $S2.  Finally, write an instruction that will store $S2 to the hexadecimal memory address x54.  Accomplish this in 4 lines or less.  Reference Code Example 6.10.

#### Code
```
addi $s0, $0, 44  # stores 44 into $s0
addi $s1, $0, -37  # stores -37 into $s1
add $s2, $s0, $s1  # stores the addition of 44 and -37 into $s2
sw $s2, 0x54($0)  # the 54 hex could also be converted to decimal (84)
```

## Task #2: *Machine Code*

| Assembly Code | Machine Code | Machine Code (Hex) |
|---------------|--------------|--------------------|
| `addi $s0, $0, 44` | 001000 00000 10000 0000000000101100 | 0x2010002C |
| `addi $s1, $0, -37` | 001000 00000 10001 1111111111011011 | 0x2011FFDB |
| `add $s2, $s0, $s1` | 000000 10000 10001 10010 00000 100000 | 0x02119020 |
| `sw $s2, 0x54($0)` | 101011 00000 10010 0000000000110110 | 0xAC120054 |

### Task #2 Simulation
![alt test](https://raw.githubusercontent.com/sabinpark/ECE281_CE5/master/task2_simulation.PNG "Task 2 Simulation")

The simulation shown above demonstrates that the program works because the program stores 44 and -37 into registers 16 and 17.  The two values are added and put into register 18.  The output is then set to register 0x54.

### Difficulties
I had trouble showing the simulation with the waveform file.  I changed the datapath of the wavefile appropriately (used labs 4 and 5 as references).  However, when I opened the updated wavefile from the simulation, I only had a blank simulation.  No amount of trouble shotting seemed to help.  In the end, I had to manually add in the desired signals. 

## Task #3: *Adding the ORI Instruction*
For Task 3, I was required to add in a new instruction:  ```ori $S3, $S2, x8000```

First, I had to decide which method I would use to implement the ori instruction.  I found it helpful to first go through the main and ALU decoder tables.  Noticing that the last row for each table was left blank and unused, I modified the tables to include the ori function as shown below.

### Main Decoder Table Modification
![alt test](https://raw.githubusercontent.com/sabinpark/ECE281_CE5/master/main_decoder_modification.PNG "Main Decoder Table")

### ALU Decoder Table Modification
![alt test](https://raw.githubusercontent.com/sabinpark/ECE281_CE5/master/ALU_decoder_modification.PNG "ALU Decoder Table")

With the tables finished, I proceeded to start implementing ideas for the schematic modification.  In the first attempt, I tried to expand the mux2 into a 3-input mux.  After making the changes in the vhdl code, I realized that there are a lot of nitpicky things that had to be modified as well.  I altered my methodology to instead include another mux.  This new mux that I created had a 6 bit source input.  After attempting a simulation, I found that there was an out-of-sync problem with the clock cycles.  Ultimately, I had to use another 2-input mux instead of the new mux I made.

I made a zero extend that branched off of the signal, *Instr*.  The output for the new zero extend would be an input for the second mux that I created.  The other input for the new mux was the *ImmExt* that came right out of the sign extend.  Depending on the selector for this mux, the output would be zero or sign extended.  The selector is connected to the ALUsrc, which was updated to include an additional bit.  The modified schematic is shown below:

![alt test](https://raw.githubusercontent.com/sabinpark/ECE281_CE5/master/schematic_modification.jpg "Modified Schematic")

### Task #2 Simulation
After checking the syntax, I tested my simulation by adding my 5th instruction:
```vhdl
		instr <= X"36538000";
```
As expected, the value in $S2 (0x00007) was OR'ed with the immediate value of 0x8000 to yield 0x8007.

The simulation is shown below:
![alt test](https://raw.githubusercontent.com/sabinpark/ECE281_CE5/master/task3_simulation.PNG "Task 3 Simulation")

## Documentation
*Task #1*: JP Terragnoli and I double-checked each other's code.  Our codes were the same.  C3C Sean Bapty told me that it would work to convert the x54 hexadecimal address into decimal.

*Task #2*: JP Terragnoli and I double-checked each other's code.  Our codes were the same.  Jarrod Wooden showed me how to manually add in the desired signals in the testbench simulation.

*Task #3*:  JP Terragnoli and I discussed the possible options for implementing the ori instruction.  We talked about the possibility of modifying the provided mux or simply adding another mux.  We double-checked each other's instruction code and the syntax for our vhdl modifications.  Austin Bolinger helped by mentioning that I needed to update all instances of ALUsrc, and that I could reuse the mux2 instead of creating another mux component.
