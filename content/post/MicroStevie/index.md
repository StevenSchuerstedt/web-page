---
title: MicroStevie
author: admin
type: page
date: 2021-08-18T06:56:38+00:00
featured_image: "cpu.jpg"

---


Designing and implementing a CPU using the Basys-3 FPGA. 

OISC (= One instruction set computer)
There is only one instruction needed for universal computation (=> Turing complete)

A von Neumann execution Model (data and instructions are in memory, sequential program execution)

For Example: SUBLEQ => subtract and branch if equal or less

SUBLEQ(A, B, C)

==> lets introduce some terminology

![](https://github.com/StevenSchuerstedt/MicroStevie/blob/main/images/architecture_topLevel.png)

### Programming FPGA using Verilog
MicroStevie CPU is a module in Verilog
What are inputs/outputs of a CPU?
=> Based on 8-Bit Computer and the 6502 Processor. 
There are data lines and address lines and a clock line. 

### Architecture
- The MicroStevie CPU
- some RAM
- a clock

## RAM 
standard RAM with write enable and output enable control signals and some address and data lines. Implemented in a Verilog Module. 
=> Write and test all modules seperatly

=> fixed stupid ram bug, reading values from ram and initalizing ram works 
initialize ram with software
byte addresable etc?
address space: total number of addressable entrys, e.g. 2^address size
addressability: how many bits in each location

## ALU
word size? what is the size of one operand the alu can operate on? 
For our OISC we only need the alu to do subtraction. thats pretty easy. Also there needs to be a way to indicate if the value is <= 0, so we need a flags mechanism for that.
## Program Counter
Simple counter

## Control Logic
every module has control- and data signals. The control logic is the heart of the cpu. It controls the flow of the modules, so they all can work together. 
control signals are stored in memory, microprogrammed
Fetch cycle, execution cycle

## Registers
Two general purpose registers, tied directly to the alu A and B. A memory address register and a instruction register. 

### MicroStevie Architecture
- ProgrammCounter, with jump cabability
- ALU, capable of Subtraction
- some branching mechanism, maybe flags?
- some registers

### Input/Output
only output for the beginning. Input is implicit by programming ram directly when initializing the fpga. 
Output would be possible with specific command, or by using a specific address in memory. 
Memory mapped Output. Have a specific address in ram always be displayed at the 7-segment. This way the subleq instruction alone is sufficient for output.