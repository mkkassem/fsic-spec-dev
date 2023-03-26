# Axis_Switch Implementation Specification
Perform the following functions:
1. Axilite transaction - Axi_switch decodes a downstream axis transaction with User_id = axilite, it passes the cycle to Axis_Axilite module. Note: a fsic_axis specifitiion to be developed
2. Downstream axis  - it is a broadcast axis bus to each user project. Note - we may use a shifter to pipeline the data to each user projects to minimize routing area, and improve timing. 
3. Upstream axis - axis bus from multiple user projects, attached user_id, arbitration, and send the axis data upstream.

Upstream data is from 
1. User project - there is only one active user project. The data from user projects are chained, thus, there is only one data source. 
2. LogicAnalyzer
3. Tester
4. MailboxBrief Explanation about the module function

Note: **axis downstream** is defined the data flows from FPGA to Caravel.
Note: **axis upstream** is defined as the data flows from Caravel to FPGA.

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


