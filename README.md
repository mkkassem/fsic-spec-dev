# Implementation Specification for FSIC Modules
Total of 17 modules except fsic architecture specification.

## FSIC Architecture Specification
Full-Stack IC (FSIC) is an IC validation system that trains a full-stack IC designer. A Full-stack IC designer can complete an IC product development with the skills of  IC design, FPGA design, and embedded programming. 
This document defines the architecture for the IC validation platform, namely FSIC Test Architecture. In addition, it provides the following specifications
- System Function Specification
- Architecture Specification
- Implementation Specification


### IC Validation System Block Diagram
The following is the proposed IC validation system. 

![FSIC System Block Diagram](https://i.imgur.com/BA8rvdv.png)

The system consists of three components, the Caravel chip, FPGA and remote Jupyter Notebook.  The Caravel chip hosts the user projects. There could be multiple user projects in the user area. The Caravel chip contains a prebuilt SOC design released from eFabless. For details, please refer to [Caravel Harness](https://caravel-harness.readthedocs.io/en/latest). 
The FPGA is an FPGA chip with SOC. Currently, we support the PYNQ-Z2 board. The following is the block diagram. It consists of PS (SOC) and PL (FPGA). A Jupyter notebook server runs on the PS. The designer can access the system remotely through a Jupyter Notebook web service. The PL part consists of part of the user project or testing functions. 
The designer uses Jupyter Notebook web browser to access and control the remote validation system. It performs various tasks, including
1. Develop Caravel RISCV test program and download into Caravel chip
2. Develop FPGA test logics and download it into FPGA chip.
3. Communicate with Caravel RISCV to conduct various testing functions.

![PYNQ-Z2 FPGA Block Diagram](https://i.imgur.com/eIJLILc.jpg)



## FSIC IC Validation System Function 


1. The standard IO frame and packaging are used for hosting the chip. This way, we can use a standard circuit board with the socket to host different user project
2. Multiple user projects can be embedded in one chip ( DUT )
3. A RISCV SOC performs a management function embedded in the chip. The designer uses the embedded processor for chip initialization and testing. 
4. In addition to the RISC-V SOC, an external FPGA is used to perform the following functions :
    1. Extend the design function in FPGA - each design can use a limited chip area; only the most core functions can be put in, and the rest of the functions are placed in FPGA, and the memory of FPGA can also be used as temporary memory to extend. It enables large systems to be designed with a limited area.
    2. Realize the real-time interactive test function (Tester) on FPGA. Generally, during simulation, the designer will develop a test function (Testbench). This interactive test function can be implemented on the hardware FPGA.
    3. Realize the functions of Logic Analyzer and Tester on FPGA. As a result, it can save expensive instrument construction and be controlled and observed remotely.
    4. The server running Jupyter notebook on the SOC in the FPGA is used to interact with the remote tester.
5. Remote test software development – Designers can use Python to develop test software at the remote end, including the test software for the embedded microprocessor in the DUT. Various functions in the FPGA can be remotely controlled and observed.
6. Remote control, scalability – The verification platform in the remote laboratory has an automatic management function, and users can rent a test platform for verification anytime, anywhere, as long as they have access to the Internet. Multiple designers share one verification machine. Such establishments can be extended to all parts of the world.

### A Proposed Architecture for Accelerator Design
The following Refer to the course “Application Acceleration with High-Level Synthesis” proposes an architecture for accelerator design. 
1.	This architecture separates the memory IO subsystem from the kernel processing system. 
2.	The Memory IO subsystem performs memory access extraction/transformation/optimization (Polyhedral Analysis). 
3.	The kernel processing system uses dataflow architecture. 
4.	The memory IO subsystem and kernel processing interface is the stream （axis）interface. 


![](https://i.imgur.com/GRy3oal.png)

### Embed Accelerator Architecture into FSIC Architecture
The two subsystems (IOP and PE)  of the accelerator architecture are partitioned in FPGA and Caravel chips. The Memory IO subsystem is implemented in the FPGA. And thekernel processing system is implemented in the Caravel SOC chip. 

#### FPGA
1. Memory, IO Access function, includes memoryaccess extraction/transformation/optimization (Polyhedral Analysis). Note: itcan be an evolutionary optimization process. FPGA provides the benefit ofreprogramming when a new idea comes up.
2. It provides necessary memory buffers.
3. Part of kernel processing can be moved to theFPGA side o reduce the area of the Caravel chip.
4. Provide data to the kernel processing system through the stream interface (axis)
5. Kernel-specific testbench code implemented inFPGA hardware (through direct wires). It provides a hardware level of cycle-by-cycle activation-and-response interaction. 

#### Caravel Chip
1. Kernel processing logic is in the user project area. It interfaces with FPGAthrough stream (axis) interfaces or direct wires （GPIOpins). 
2. The Kernel processing logic can also use standard Caravel-UserProject Interface schemes, i.e. Wishbone and/or LogicAnalyzer. 

![](https://i.imgur.com/H4lqF1O.png)

### System Operation Paths
The system supports three operation paths in the below figure. 
1. splash download from the remote user before Caravel RISCV boots
2. RISCV communicates with the remote user through FPGA SOC
3. Remote user program user projects in Caravel chip just like Caravel RISCV communicate with users project through Wishbone

![](https://i.imgur.com/9rwMLGh.png)

#### Remote user downloads RISCV test code to spiflash 
Remote user need to download RISC-V test code to spiflash on Caravel board before RISCV boots. Later the control is transferred to RISCV. Thus, the spiflash control is mixed between Caravel and the remote. In the eFabless Caravel board, the spiflash is mixed between the Caravel chip and USB. Refer to [Caravel_schematic](https://github.com/efabless/caravel_board/blob/main/docs/Caravel_schematic.pdf) We will use FPGA to replace USB. By properly controlling the IO enables of the Caravel chip and FPGA, we can have direct wire FPGA and Caravel flash control signals to spiflash.

#### RISCV communicates with remote user
In current practice, RISCV communicates with the test bench through mprj_io (GPIO). The role of FPGA is like the test bench in the simulation system. We will use a mailbox for communication between Caravel RISCV and FPGA. Define a mailbox address rang in user address space in 'h3000_0000 - 'h3fff_ffff. Caravel/RISCV and FPGA/ARM exchange messages through the mailbox.

#### Remote User Programs User Projects
In the case of a test chip without SOC built-in, the FPGA needs a way to act the role of RISC-V in the Caravel chip.  FPGA uses the same memory-mapped IO addresses as defined in Caravel to access user project areas. 

## **User Project Area**
User project area is defined in the blue box in the following picture. It occupies about 10 mm^2^. 
![](https://i.imgur.com/xP0W9CM.png)


### User Project Wrapper (module: user_project_wrapper)

The user project area is wrapped with **User Project Wrapper** which includes
1. One or more user projects. There is only one active user project in a running system.
2. Aggregator/Dis-aggregator logic for
    - Wishbone interface
    - Logic Analyzer
    - GPIO
    - IRQ
3. Instrumentation tools, including
    - Logic Analyzer
    - Tester
4. Protocol Converter
    - Wishbone - Axilite
    - Axis-Wishbone
    - Axis-Axilite
5. IO serialization logic
6. Communication bwtween Caravel and FPGA 
    - Mailbox

The interface signals of **User Project Wrapper** in the following figure include
- user_clock2
- Wishbone interface (wb_xxx) 
- mprj_io (38 GPIO pins, io_in, io_out, io_oeb )
- logic analyzer data (la_data_in, la_data_out, la_oenb)
- user_irq
Note: the user_project_wrapper should be compatible with current Caravel SOC definition. 

![](https://i.imgur.com/ZYPF1Md.png)


### User Projects (module:user_project)
We use a new user project interface which is different from Caravel chip. It provides the following interface
- core_clk, io_clk - user clock, and IO clock.
    - the io clock is the original clock user project clock (user_clock2)
    - the user_clock is divided from io_clock by N where N is the same ratio of io_serdes
- axilite - it is used for configuration. 
- axis - AXI stream interface for data transfer.
- la - Logic Analyzer interface. 
- irq - Interrupt
#### user project IO serialization
We apply the same IO serialization technique in **io_serdes** to reduce the interconnetion from user project to wrapper. Further the signals from multiple user projects are searilly transmitted. The user IO section is either a mux plus an optional flip-flop depending on the need of retiming.The mux select the signal either from the previous user_project or current project depends on if the current project is active project.

![](https://i.imgur.com/ogsQNfB.png)


There will be multiple user projects wrapped in a user_projext warpper. 
The new project interface will fit into the existing user project wrapper illustrated below.

![](https://i.imgur.com/mxRZsiP.png)

## Block Diagram of User Project Wrapper

The following is the block diagram of User Project Wrapper and validation FPGA. There will be a spearate documentation to define its implemementation specification.

![](https://i.imgur.com/av79pyc.png)


Inside **User Project Wrapper**
- **WB_Axilite** (Wishbone to Axilite Converter)
- **Axilite_WB** (Axilite to Wishbone Converter) ? - may not needed
- **LogicAnalyzer**
- **Tester**
- **Mailbox**
- **Axis_Axilite** (Axis to Axilite Converter)
- **Axis_Switch** (Axis Switch)
- **Aggregator** (Mux irq from user projects, it is cascade among multiple user projects, only seleted module select its output)
- **Disaggregator** (Demux to user projects, )
- **IO_Serdes**


Inside **FPGA** 
- Axis_Axim (Axis to Axim DMA - upstream)
- Axim_Axis (Axim to Axis DMA - downstream)
- Axilite_Axis
- User Project - this is the extension of user project which is not in the scope of the architecture.
![](https://i.imgur.com/hxfJMso.png)


Other **system specifications**
- FSIC-AXIS interface specification. This is an extensition of Axis specification to include FSIC architecture specific function, e.g. axilite configuration transaction embedded in Axis. 
- Memory_Map (Memory map table) - Include new IP in user project wrapper
- System Clock - clocking scheme for IO serialization (IO_serdes) and user project interface searlization. 
- Power-on and reset - Power-on and reset sequence between Caravel chip and FPGA
- Spliflash download - remote user downloads RISC-V code.
- Caravel (RISC-V) software programming guide
- Validation FPGA software programming guide



### WB_Axilite - Wishbone to Axilite Converter
Caravel SOC/RISC-V is the Wishbone bus interface and the user project uses the Axilite interface. thus, it needs a protocol converter. It is a 32-bit data bus and a 10-bit address, and an axilite_en signal. The 10-bit address is an offset indexed to its configuration space. It is assumed the configuration space size is limited to 1KB, if more configuration space is needed, we can expand number of address bits. 
In the module, an address decoder is to decode the User Project address space from 'h3000_0000' to ‘h3fff_ffff’, which includes
- user_project
- other IP in the user wrapper, includes axis_switch, Logic_Analyzier, Tester, IO_serdes ...

### Axilite_WB - Axilite to WB converter - may not needed
The module is used for FPGA to access Caravel chip WB devices, e.g. Housekeeping, Spiflash ... to replace the role of RISCV. We may not need this function.

### Axis_Switch
Perform the following functions:
1. Axilite transaction - Axi_switch decodes a downstream axis transaction with User_id = axilite, it passes the cycle to Axis_Axilite module. Note: a fsic_axis specifitiion to be developed
2. Downstream axis  - it is a broadcast axis bus to each user project. Note - we may use a shifter to pipeline the data to each user projects to minimize routing area, and improve timing. 
3. Upstream axis - axis bus from multiple user projects, attached user_id, arbitration, and send the axis data upstream.

Upstream data is from 
1. User project - there is only one active user project. The data from user projects are chained, thus, there is only one data source. 
2. LogicAnalyzer
3. Tester
4. Mailbox

Note: **axis downstream** is defined the data flows from FPGA to Caravel.
Note: **axis upstream** is defined as the data flows from Caravel to FPGA.


### Axis_Axilite  (host read write Caravel module)
It has a downstream and upstream axis interface. The downstream axis cycle includes
1. Axilite writes - It first looks up the mmio table, and generatates individual **axilite_en_id** to each user project.Note: We currently don't support non-posted writing. Axilite write will be posted, There is no write acknowledge cycle. It may potentially create problems for multiple concurrent thread operations.
2. Axilite read - It consists of two phases, address phase, and data phase. The address phase generates axilite transaction. The data phase will convert to axis upstream transaction with user_id attached.
A FSIC-Axis protocol specification further defines the packet definition. 


### LogicAnalyzer
LogicAnalyzer performs the following functions
1. Monitor signals provided by user project. Support up to  TBD (64?) monitoring signals
2. Singal conditioning to trigger signal logging
3. Compress the logged signals and sent them to remote users using the Axis port- Waveform display in remote jupyter notebook.
 
**Note:** The compression algorithm should use as minimum area as possible.
**Note:** The LogicAnalyzer function is different from the Caravel LogicAnalyzer function (signals: la_xx ). In Caravel LogicAnzlyzer signals are used for general-purpose gpio signals controlled by RISC-V.

### Tester - will be implement in a later stage ??
The test program development flow is
1. Define a test instruction set
2. Test program s generated by monitoring interface signals
3. Translate the cycle-by-cycle vectors into test program
4. The output is compared with expected value (in the test program)

### Mailbox - Exchange message between Caravel/RISC-V and FPGA
The mailbox is provided a communication channel between Caravel/RISC-V and FPGA/ARM. 
One ready usage is provide a test-bench enviornment in existing Caravel verification environment. In current practice, RISCV communicates with the test bench through mprj_io (GPIO). The role of FPGA is like the test bench in the simulation system. We will use a mailbox for communication between Caravel RISCV and FPGA. Define a mailbox address rang in user address space in 'h3000_0000 - 'h3fff_ffff. Caravel/RISCV and FPGA/ARM exchange messages through the mailbox.
The mailbox provides Wishbone (RISC-V) and Axilite (FPGA) interface, and a set of registers (size of mailbox).


### Aggregator/Disaggregator
Aggregator/Disaggregator should be designed in a parametrized way, depending on the number of user projects. Considering using HLS C++ template coding style for easy extension. 

#### Disaggregator ####
- Axis to multiple Axilite interface (for each user project) decode with axilite_en
- Wishbone to multiple user projects
- Axis downstream data to each user project. It is a common bus data for each user project. Only the active user project will receive the data. 


#### Aggregator
- Axis to multiple Axilite read data (from user projects) 
- Wishbone read to multiple user projects
- Multiple user projects la_data_out to  LogicAnalyzer 
- Upstream axis data from user projects to Axis-switch
- user_irq

### IO_Serdes
The purpose of this module is to virtually increase the number of IO pins by ratioing the core clock and io clock. In the following diagram, there are m* core signals to IO, and there are n * io pins. To match its throughput, it needs to meet the equation, **m x core_clk = n x io_clk**. For example, if there are only 16 IO pins available for interconnetion between Caravel and FPGA, and the clock ratio m/n = 10, i.e. IO runs at 50Mhz, and core runs at 5Mhz. We virtual have 160 IO pins available. 

![](https://i.imgur.com/BhmuNDY.png)


In designing IO_Serdes, one design issue is that both Caravel and FPGA chip needs to agree on a common phase The IO_Serdes is implemented by shifters and muxes. Both transmission and receiving sides need to agree on a common phase states, i.e. counter value for the mux select. The initialization is done right after Caravel chip comes out reset state by sending a intialization patterns. After the initialization phase, both side runs synchronously afterward, until Caravel chip reset again. 

### Axis_Axim (Axis to Axim DMA - upstream) - FPGA
This provides upstream access from Caravel chip **write** to system memory. 
A single descriptor, like Xilinx XDMA (may extend to chained descriptor, like QDMA) in the future. The descriptor includes
- Transfer length
- Memory buffer address

### Axim_Axis (Axim to Axis DMA - downstream) - FPGA
This provide downstream access from Caravel chip **read** from system memory. 
A single descriptor, like Xilinx XDMA (may extend to chained descriptor, like QDMA) in the future. The descriptor includes
- Transfer length
- Memory buffer address

### Axilite_Axis (Axilite to Axis Conversion) - FPGA
This provide a channel for FPGA/ARM to access modules in Carvel chip. FPGA/ARM uses memory mapped IO (same memory map as Carvel chip). The Axilite transaction is translated to Axis protocol (refer to fsic-axis specification), and send to axis interface. For Axilite read transaction it is splited to address phase which is 1 cycle of address phase downstream, and one cycle of data phase in later time. When the later axis data phase completes, it sends axilite data and complete the axilite read transaction. 
For axilite write, we use a simple 2 cycle of axis transaction, one cycle for address, one cycle for data. **Please note, the axilite write completion is returned without waiting the downstream axilie is actually completed to simplified the design** The chance of running into race condition is very low. 

Note: If the Caravel mamory-map io conflicts with FPGA/ARM memory-map io, and re-mapped scheme is needed. For example if user-project mmio 'h3000-0000'-'3fff_ffff' is used by FPGA, then we can remap to 'hf000-0000' - 'hffff-ffff' if it is available. 

## System Specifications
### FSIC-AXIS interface specification
This is an extensition of Axis specification to include FSIC architecture specific function, e.g. axilite configuration transaction embedded in Axis. 

### FSIC Memory_Map (Memory map table)
This defines memory map for FSIC IP implemented in user project wrapper and validation FPGA. 

### System Clocking Scheme
Clocking scheme for IO serialization (IO_serdes) and user project interface searlization.


### Power-on and reset 
Power-on and reset sequence and its control mechanism in the validation system of Caravel-FPGA

### Spliflash download 
Define the sequence and its control mechanism for remote user to download RISC-V executable to Caravel spiflash

### Caravel (RISC-V) software programming guide
Programming guide and examples for Caravel RISC-V program.

### Validation FPGA software programming guide
Programming guide and examples for validation FPGA ARM program

## Implementation Note
To facilitate integration and future enhancement. We will recommend the following coding style

#### Automate the user-project wrapper integration process. 
- Use standard naming for signals. There two type of naming: port name, and signal name. If the signal is user project specific, the interconnect signal is named as **portname_userid**. User id is predefined before design integration.
- Develop a script to assembly and integrate user projects for wrapper module integration.
- Aggregator, Disaggregate, Axi-Switch should adopt parameterized coding style. If use HLS, use C++ template coding. 


## To Be Completed

## Clocking - TBD


## Power-on Reset - TBD


## System Initialization Sequence
1.	Reset Exit Sequence : FPGA -> Caravel
    - FPGA GPIO output (default: assert to reset)  to control Caravel reset, and power circuit
    - Caravel power-on After FPGA bit-stream download
    - Caravel reset released after Caravel firmware download
2.	FPGA download Caravel flash content from host
3.	FPGA release Caravel reset-pin, and wait for Caravel chip initialization done
4.	Caravel chip initialization


