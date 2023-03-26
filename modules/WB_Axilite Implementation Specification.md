# WB_Axilite Implementation Specification
Caravel SOC/RISC-V is the Wishbone bus interface and the user project uses the Axilite interface. thus, it needs a protocol converter. It is a 32-bit data bus and a 10-bit address, and an axilite_en signal. The 10-bit address is an offset indexed to its configuration space. It is assumed the configuration space size is limited to 1KB, if more configuration space is needed, we can expand number of address bits. 
In the module, an address decoder is to decode the User Project address space from 'h3000_0000' to ‘h3fff_ffff’, which includes
- user_project
- other IP in the user wrapper, includes axis_switch, Logic_Analyzier, Tester, IO_serdes ...
Brief Explanation about the module function

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


