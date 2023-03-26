# IO_Serdes Implementation Specification
The purpose of this module is to virtually increase the number of IO pins by ratioing the core clock and io clock. In the following diagram, there are m* core signals to IO, and there are n * io pins. To match its throughput, it needs to meet the equation, **m x core_clk = n x io_clk**. For example, if there are only 16 IO pins available for interconnetion between Caravel and FPGA, and the clock ratio m/n = 10, i.e. IO runs at 50Mhz, and core runs at 5Mhz. We virtual have 160 IO pins available. 

![](https://i.imgur.com/BhmuNDY.png)


In designing IO_Serdes, one design issue is that both Caravel and FPGA chip needs to agree on a common phase The IO_Serdes is implemented by shifters and muxes. Both transmission and receiving sides need to agree on a common phase states, i.e. counter value for the mux select. The initialization is done right after Caravel chip comes out reset state by sending a intialization patterns. After the initialization phase, both side runs synchronously afterward, until Caravel chip reset again. 
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


