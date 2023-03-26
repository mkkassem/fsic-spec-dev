# Axilite_Axis (FPGA Axilite to Axis Conversion) Implementation Specification
This provide a channel for FPGA/ARM to access modules in Carvel chip. FPGA/ARM uses memory mapped IO (same memory map as Carvel chip). The Axilite transaction is translated to Axis protocol (refer to fsic-axis specification), and send to axis interface. For Axilite read transaction it is splited to address phase which is 1 cycle of address phase downstream, and one cycle of data phase in later time. When the later axis data phase completes, it sends axilite data and complete the axilite read transaction. 
For axilite write, we use a simple 2 cycle of axis transaction, one cycle for address, one cycle for data. **Please note, the axilite write completion is returned without waiting the downstream axilie is actually completed to simplified the design** The chance of running into race condition is very low. 

Note: If the Caravel mamory-map io conflicts with FPGA/ARM memory-map io, and re-mapped scheme is needed. For example if user-project mmio 'h3000-0000'-'3fff_ffff' is used by FPGA, then we can remap to 'hf000-0000' - 'hffff-ffff' if it is available. 


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


