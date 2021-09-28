---
title: MicroStevie
author: admin
type: page
date: {}
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
With the OISC architecture we choose to minimize and simplify the hardware to the extreme. There is nothing we can remove from the hardware without making it loose core functionality (turing completness). But this simplicity of the hardware comes with a price, now the sofware is much more complicated. Writing a simple "add" or "multiply" programm requires a lot of thought from the programmer, and many lines of code using just the SUBLEQ instruction. So what we actually did is just to shift the complexity from hardware to software. The art in computer architecture (or in life) is to find a good balance between these two (harware/software, body/mind?).

code: https://github.com/StevenSchuerstedt/MicroStevie


### lets introduce some terminology

address space (total number of addressable entrys of RAM): 2^8, 256

addressability (how many bits in each location): 24

word size (size of one operand the alu can operate on): 24 

instructions: 1


![](https://github.com/StevenSchuerstedt/MicroStevie/blob/main/images/architecture_topLevel.png)

### Programming FPGA using Verilog
The actual implementation of the CPU uses Verilog and a FPGA. Verilog is a hardware description language (HDL). Unlike programming languages, there is no sequential execution of the code (in a way there is no execution at all..) but only a description what the hardware should look like. 


### Architecture
The basic architecture follows the von Neumann execution Model, this means data and instructions are in memory and the program is executed sequentially instruction by instruction. (SISD, single instruction / single data, in contrast to SIMD for gpus, parallel processor)
The basic components are:
- The MicroStevie CPU, with a address line to controll the ram, and a bus line to output data
- some RAM, with the program and the corresponding data 
- a clock, to synchronize everything

The basic computing procedure is very simple, the MicroStevie CPU fetches (loads) the correct instruction from RAM (according to program counter), this instruction is placed in the insruction register and then gets executed. The results are written back into the RAM and the next instruction is fetched. 

## Control Logic
The control logic is the heart of the cpu. It controls the flow of the modules, so they all can work together. 

Fetch Cycle - fetch the next operands from the RAM into the IR

Execution Cycle - execute the SUBLEQ instruction

## Registers
Two general purpose registers A and B 24 bits wide, tied directly into the ALU. 

A memory address register for the 8 bit address of the RAM.

A instruction register to hold the 24 bit "instruction" (because OISC, only operands), aka three 8 bit operands.

A flags register for the less or equal than 0 flag.


### Input/Output
MicroStevie can only output the result of the computation. Input is implicit by programming the ram directly when initializing the fpga. 
For output I decided for memory-mapped i/o, so only the SUBLEQ instruction is sufficienct for output. 
I decided memory address 100 is always written to the seven segment display of the fpga.

### some programs



### Speedup
Pipelining, Multithreading, Interrupts, vga output, programm the system on itself

This project took roughly ~3 months
