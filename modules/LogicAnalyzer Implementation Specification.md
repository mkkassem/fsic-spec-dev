# LogicAnalyzer Implementation Specification
LogicAnalyzer performs the following functions
1. Monitor signals provided by user project. Support up to  TBD (64?) monitoring signals
2. Singal conditioning to trigger signal logging
3. Compress the logged signals and sent them to remote users using the Axis port- Waveform display in remote jupyter notebook.
 
**Note:** The compression algorithm should use as minimum area as possible.
**Note:** The LogicAnalyzer function is different from the Caravel LogicAnalyzer function (signals: la_xx ). In Caravel LogicAnzlyzer signals are used for general-purpose gpio signals controlled by RISC-V.

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


