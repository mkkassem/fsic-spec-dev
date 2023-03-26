#  Mailbox Implementation Specification - Exchange message between Caravel/RISC-V and FPGA
The mailbox is provided a communication channel between Caravel/RISC-V and FPGA/ARM. 
One ready usage is provide a test-bench enviornment in existing Caravel verification environment. In current practice, RISCV communicates with the test bench through mprj_io (GPIO). The role of FPGA is like the test bench in the simulation system. We will use a mailbox for communication between Caravel RISCV and FPGA. Define a mailbox address rang in user address space in 'h3000_0000 - 'h3fff_ffff. Caravel/RISCV and FPGA/ARM exchange messages through the mailbox.
The mailbox provides Wishbone (RISC-V) and Axilite (FPGA) interface, and a set of registers (size of mailbox).


## Interface Blocks
Block diagram shows its interconnected module

## Feature Lists
List of functions and features
1. Feature#1
2. Feature#2

## Interface Signals


| Port | in/out | Descriptiion |
|:------:|:------:|:------------ |
|  clk   |   in   | clock        |
| rst_n |   in   | reset is active low        |
|sig1_i | in | signal #1 indicates data is valid |
|sig2_o | out | sig_o is an output signal |
|sig3_i_n| in | sign3_ni is active low input |

## Register Description
A table shows register definitions
### Register Group Name : Based Address

|RegisterName|Offset Address| Description |
|:----------:|:------------:| :-----------|
|Control     |'h0             | Control Register block Definition<br>bit 0: bit 0 function<br>bit 1: bit 1 function<br>bit 2: bit 2 function is<br>bit 3: bit 3 function |
|Status      | 'h4          | Status register block definition<br>bit 0: bit 0 status is<br>bit 1: bit 1 interrupt status|

## Function Description

### Function 1:
Description of the function 1, including 
- block diagram
- Datapath flow
- Control flow
- Logic structure
- Structure component used, e.g. RAM, Shifter, State machine 

## Programming Guide
- Code illustration to control the function

## Future Work


