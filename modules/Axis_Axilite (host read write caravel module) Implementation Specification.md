# Axis_Axilite (host read write caravel module) Implementation Specification
It has a downstream and upstream axis interface. The downstream axis cycle includes
1. Axilite writes - It first looks up the mmio table, and generatates individual **axilite_en_id** to each user project.Note: We currently don't support non-posted writing. Axilite write will be posted, There is no write acknowledge cycle. It may potentially create problems for multiple concurrent thread operations.
2. Axilite read - It consists of two phases, address phase, and data phase. The address phase generates axilite transaction. The data phase will convert to axis upstream transaction with user_id attached.
A FSIC-Axis protocol specification further defines the packet definition. 
 



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


