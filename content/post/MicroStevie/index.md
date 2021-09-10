---
title: MicroStevie
author: admin
type: page
date: 2021-08-01T14:52:38+00:00
featured_image: cpu.jpg
published: true
---

MicroStevie is the implementation of a OISC (= One instruction set computer) CPU using the Basys-3 FPGA and verilog. 

How many instructions does a computer need to be turing complete? CISC Architectures like Intel x86 have thousand of different instructions, but what is the bare minimun needed for computation?
As is turns out there is only one instruction needed for universal computation. This kind of architectures are called OISC and there are alot of different ways to design this one instruction. In this example I choosed the SUBLEQ instruction.

SUBLEQ => subtract and branch if equal or less
SUBLEQ(A, B, C)

RAM[B] = RAM[B] - RAM[A]
if(RAM[B] <= 0)
 goto C


To be fair this instruction is actually combining three instructions, subtract, store in memory and conditional jump. All other OISC instruction are built in a similar way.

With a OISC we save a lot of hardware, since we dont need any decode mechanism, there is only one instruction anyways, so the processor always knows what to execute. The only thing that changes are the parameters. 

The OISC architecture is a good (or maybe quite extreme) example of the relationship and interchangeability between hardware and software. Every algorithm can be implemented in hardware directly, or in software using simpler hardware mechanisms (equality of hardware and software). 
With the OISC architecture we choose to minimize and simplify the hardware to the extreme. There is nothing we can remove from the hardware without making it loosing core functionality (turing completness). But this simplicity of the hardware comes with a price, now the sofware is much more complicated. Writing a simple "add" or "multiply" programm requires a lot of thought from the programmer, and many lines of code using just the SUBLEQ instruction. So what we actually did is just to shift the complexity from hardware to software. The art in computer architecture (or in life) is to find a good balance between these two (harware/software, body/mind?).

A von Neumann execution Model (data and instructions are in memory, sequential program execution)


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
