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

## Documentation
*Task #1*: JP Terragnoli and I double-checked our code.  Our codes were the same.  C3C Sean Bapty told me that it would work to convert the x54 hexadecimal address into decimal.
