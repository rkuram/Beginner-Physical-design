# Beginner physical design using open-source EDA tools

This repository contains all the information included in the beginner physical design using open- source EDA tools organized by VLSI System Design Corporation. This workshop helped me gain hands-on experience of tools that are used in physical design flow using Qflow tool chain with complete RTL to GDSII flow on PicoRV32.

## Contents:

- [Introduction to Qflow](https://github.com/rkuram/Beginner-Physical-design/blob/main/README.md#1-introduction-to-qflow)

- [Day 1: Study and review of various components of RISC-V based picoSoC](https://github.com/rkuram/Beginner-Physical-design/blob/main/README.md#day2-chip-planning-strategies-and-introduction-to-foundry-library-cells)

- [Day 2: Chip planning strategies and introduction to foundry library cells](https://github.com/rkuram/Beginner-Physical-design/blob/main/README.md#day2-chip-planning-strategies-and-introduction-to-foundry-library-cells)

- [Day 3: Design characterize one library cell using MAGIC Layout tool and ngSPICE](https://github.com/rkuram/Beginner-Physical-design#day3-design-characterize-one-library-cell-using-magic-layout-tool-and-ngspice)

- [Day4: Pre-layout timing analysis and importance of good clock tree](https://github.com/rkuram/Beginner-Physical-design#day4-pre-layout-timing-analysis-and-importance-of-good-clock-tree)

- [Day5: Final Steps for RTL2GDS](https://github.com/rkuram/Beginner-Physical-design#day5-final-steps-for-rtl2gds)

- [Acknowledgements](https://github.com/rkuram/Beginner-Physical-design/blob/main/README.md#acknowledgements)

## Introduction to Qflow:

Qflow is a complete tool chain for synthesizing digital circuits starting from Verilog source and ending in physical layout for a specific target fabrication process which is developed by Open Circuit Design. The main goal of this workshop is to use the tool chain which contains open source tools like Yosys, graywolf, MAGIC, Netgen etc., to get an overview of how the physical design flow works on an SoC.

More details on Qflow can be obtained [here](http://opencircuitdesign.com/qflow/#:~:text=Qflowisacompletetool,aspecifictargetfabricationprocess)

## Day1:  Study and review of various components of RISC-V based PicoSoC

Day 1 starts with an introduction on IC design component technologies where we discuss the on how this workshop is base on the chip design, which is not related to embedded devices. The package, that is on the chip, discussed in this workshop is Quad Flat No-leads (QFN-48) which is of size 7mm x 7mm. The chip is connected to the package by wire bounds where it contains the pads, core and die.  The consists of logic blocks which are classified into Macros (like SPI and SoC) and Foundry Ip’s (like SRAM, ADC, etc.,).

We then discuss about RISV-V Instruction Set Architecture (ISA) flow and how it helps the applications on the computers communicate with the hardware. In ISA flow, the app code (high level language) is converted to instructions based .exe file using a compiler which in turn is converted to binary using an assembler. The compiled instructions are based on the architecture type the hardware is based on and it is an implementation of RTL using hardware Descriptive Language (HDL). For graphical representation, please refer to the below picture.

![PIC1](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day1_1.PNG)

The core that we use in this workshop is PicoRV32, which is a RISC-V RV32IMC core. The key features of the PicoRV32 core, PicoSoc, Raven SoC, and Raven chip along with the differences between the two Soc’s are explained. Please refer the below figure for PicoSoC.

![PIC2](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day1_2.PNG)

A brief introduction to IC design components along with their respective open-source tools are mentioned as below:

- Logic Synthesis – **Yosys open synthesis suite**
- FloorPlaanning, Placement, CTS – **Graywolf**
- Routing – **Qrouter**
- Static Timing Analysis (STA) – **Opentimer**
-	Layout – **MAGIC layout viewer**
-	Pre/Post-layout spice simulations – **ngSPICE**
-	Schematic editor – **esim**

Q flow is the tool chain that incorporates all these open-source tools for a complete RTL2GSS flow.

### Lab:

- **To clone VSDflow from github**

![PIC3](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day1_Lab_1.PNG)

- **Files in outdir_spi_slave** 

![PIC4](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day1_Lab_2.PNG)

![PIC5](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day1_Lab_4.PNG)

- **Qflow display**

![PIC6](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day1_Lab_3.PNG)

-	**Magic layout**

![PIC7](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day1_Lab_5.PNG)

-	**Synthesis log**

![PIC8](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day1_Lab_6.PNG)


## Day2: Chip planning strategies and introduction to foundry library cells

On Day 2, definition of width and heigh of the core were explained. Later, concepts like Utilization factor and Aspect ratio were explained in detail and their importance in designing the floorplan was also mentioned. Ideally, utilization factor is around 0.5 (50%) or 0.6 (60%).

- **Utilization factor** = (Area occupied by netlist)/ (Total are of the core)
- **Aspect ratio** = (Height of the die)/ (Width of the die)

Steps involved to define pre-placed cells and it’s advantages of enhancing reusability, and also how de-coupling capacitors are placed around the pre-placed cells which help during switching avoiding IR drop and the signal to be within noise margin was explained. As de-coupling capacitors ensures proper local communication by pre-charging, power planning the core ensures global communication by providing multiple Vdd and ground tap points to avoid voltage droop and ground bounce in the wires and other logic cells. The initial placement is optimized during the PnR tool flow by estimating wirelength and capacitance and adding buffer. Steps of pin placement and logical cell placement blockage are explained so that PnR tool does not place anything between core and die. 

![PIC9](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day2_1.PNG)

**Cell design Flow:**

One thing that is common in all the physical design flow is the library cell. Cell design flow contains 3 steps to characterize the cells.

- **Inputs**: Inputs for cell design flow are provided by foundry from PDKs (Process design kits), which includes DRC and LVS rules, SPICE models, Library, and user defined specs
- **Design steps**: Design steps of cell design involve circuit design (implementing the function and model it to meet the library requirements), layout design (implementing the values from circuit design into the layout using the art of layout) and characterization. The software GUNA is used for characterization and can be classifies as timing, power, and noise characterization.
- **Outputs**: Outputs of the design are CDL (circuit description language), GDSII, LEF, extracted spice netlist (.cir), timing, noise, power libs and function.

**Typical characterization flow**:

- Reading the model file of CMOS containing basic property definitions.
- Reading the extracted Spice Netlist.
- Recognize / Define the behavior of the buffer cell.
- Read the subcircuits -- of the inverter.
- Attach the necessary power sources.
- Apply an input or the stimulus.
- Provide the necessary load capacitance.
- Provide the necessary simulation commands.

**Important parameters of Timing Characterization**:

- **Rise Delay**: Time taken for waveform to rise from 20% to 80% of VDD.
- **Fall Delay**: Time taken for waveform to fall from 80% to 20% of VDD.
- **Propagation Delay**: Measured between 50% of Input transition to 50% of Output transition.

### Lab:

- **Pin Placement by auto arranging** 

![PIC10](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day2_Lab_1.PNG)

- **Running placement with setting aspect ratio and utilization factor**

![PIC11](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day2_Lab_2.PNG)

- **Placement log file**

![PIC12](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day2_Lab_3.PNG)

- **Layout area after placement**

![PIC13](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day2_Lab_4.PNG)


## Day3: Design characterize one library cell using MAGIC Layout tool and ngSPICE

On Day3, SPICE deck creation for an CMOS inverter along with the art of layout including DRC layout rules was explained. SPICE deck formation contains information like connectivity of the netlist, values, and information about nodes. The W/L ratio of CMOS impacts the co Conductivity and the width of pmos is generally maintained around 2 to 2.5 times of width of nmos. CMOS robustness is defined byt eh switching threshold (vm), which is a parameter defining the conditions 

- Vin = Vout 
- Vgs = vds 
- Idsp = -Idsn.

![PIC14](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day3_1.PNG)

**Netlist description of CMOS**:

- M1 <drain> <gate> <source> <substrate> pmos W=<#>u L= <#>u
- M2 <drain> <gate> <source> <substrate> nmos W= <#>u L=<#>u

**Command for DC Analysis**:

.dc Vin <low level> <high level> <step increase>

**Command for transient analysis**:

.tran <Tstep> <Tstop>

**Art of layout and DRC rules**:

The art of layout uses Euler’s path and stick diagram to design the layout according to the DRC rules provided by the foundry.

![PIC15](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day3_2.PNG)

**16-Mask CMOS Process steps**:

- Substrate Selection: Selection of base layer on which other regions will be formed.
- Create active region for transistors: SiO2 and si3N2 deposited. Pockets created using photoresist and lithography.
- Nwell & Pwell formation: Pwell uses boron and nwell uses phosphorous. Drive in diffusion by placing in high temp furnace.
- Creating Gate terminal: For desired threshold value NA (doping Concentration) and Cox to be set.
- Lightly Doped Drain (LDD) formation: LDD done to avoid hot electron effect and short channel effect.
- Source and Drain formation: Forming the source and drain.
- Contacts & local interconnect Creation: SiO2 removed using HF etch. Titanium deposited using sputtering.
- Higher Level metal layer formation: Upper layers of metals deposited.

![PIC16](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day3_3.PNG)

### Lab:

- **Inverter spice file and it change in waveform with respective to change in Wp from 0.5 to 0.75**

![PIC17](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day3_Lab_1.PNG)

![PIC18](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day3_Lab_2.PNG)

![PIC19](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day3_Lab_3.PNG)

- **Inverter transient analysis at Wp = 0.75**

![PIC20](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day3_Lab_4.PNG)

- **Pre-layout spice file and simulation**

![PIC21](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day3_Lab_5.PNG)

![PIC22](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day3_Lab_6.PNG) 

- **Post-layout magic and area**

![PIC23](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day3_Lab_7.PNG)

- **Post layout extracted spice file modification and simulation**

![PIC24](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day3_Lab_8.PNG)

![PIC25](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day3_Lab_9.PNG)


## Day4: Pre-layout timing analysis and importance of good clock tree

On Day4, concepts of time modelling using delay tables, power aware clock tree synthesis, signal integrity and timing analysis with both ideal and real clock were explained. I learned that delay and output transition in a cell is based on input transition and load capacitance. Power aware CTS refers to clock gating which helps in reducing the power consumption by not enabling the clock to the cell that are not in use aa a particular period.

Clock Tree Synthesis (CTS) is used for providing clock to all the logic cells with the goal of maintaining zero skew. The H-tree algorithm along with clock buffering were explained based on the delay tables and how there are certain conditions to be satisfied for the clock buffers (equal rise and fall time) for an efficient CTS. The concepts of clock-net shielding are explained to reduce crosstalk issues like glitch and delta delay which may affect the clock skew.

![PIC26](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day4_1.PNG)

![PIC27](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day4_2.PNG)

**Setup and Hold Slack Analysis**:

Setup and hold time define a window of time in which our data should remain unchanged for desired data transfer to take place. Factors like uncertainty (due to jitter) and skew plays an important role in this. Slack is defined as difference between actual time and required time and a design with positive or zero slack is desired which indicates that there is no violation of timing. Below are some figures showing the equations for setup and hold time analysis.

![PIC28](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day4_3.PNG)

![PIC29](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day4_4.PNG)


### Lab:

- **Inverter transient spice file and analysis with 20fF load**

![PIC30](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day4_Lab_1.PNG)

![PIC31](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day4_Lab_2.PNG)

- **Osu018 standard cell library**

![PIC32](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day4_Lab_3.PNG)

- **Files to perform timing analysis**

![PIC33](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day4_Lab_4.PNG)

![PIC34](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day4_Lab_5.PNG)

- **Timing analysis with ideal and real clocks**

![PIC35](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day4_Lab_6.PNG)

![PIC36](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day4_Lab_7.PNG)


## Day5: Final Steps for RTL2GDS

Routing method finds out best possible pattern for connection between two end points, of which one point is target node while the other is source node. Maze Routing-Lee's Algorithm was introduced. In this technique firstly routing grids are created, and source & target nodes are identified. Then the blocks adjacent to one under consideration are assigned same numbers and this process is repeated till we reach the target node. Once this done the pattern with minimum number of turns preferably a L spaced pattern is finalized for route.

**Typical DRC rules for pair of wires**:

- Wire width
- Wire pitch
- Wire Spacing

These DRC rules exist because of limitations of the Lithography technique. Deviation takes place in lithography leading to changes which might lead to an unintended open circuit or short circuit and to avoid this the DRC rules are fixed. Another DRC Violation type is Signal short which can be dealt by changing layers through Vias.

**Typical DRC rules for Vias**:
- Via width
- Via spacing

Concepts of parasitic extraction has been discussed where we use IEEE 1481 – 1999 SPEF (Standard Parasitic Exchange Format) as a representation format. We have discussed on a single wire connecting the input port to a buffer.

**The components mentioned in the SPEF file are**:

- I – Input port
- *L – Load (capacitance)
- *C – Co-ordinate (location)
- *D - Driver
- *S – Waveform shape (rise & fall slew)
- *NAME_MAP – to give it a unique name
- *PORTS – list of ports are under here
- *D_NET – wire/net (distributed nets)
- *R_NET – wire/net (reduced nets)
- *P – if driver or receiver is a port
- *D – Driver type
- *I – if driver or receiver is an internal pin
- *RES – Distributed Resistance
- *CAP – Distributed Capacitance (when added, it gives you the lumped capacitance)
- *END – end of the component

We also need a header file to include the SPEF, design name and the default units that are assigned to the SPEF file. The below picture shows you the representation of a wire as a SPEF file.

![PIC37](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day5_1.PNG)


### Lab:

-	**Routing in progress using qrouter**

![PIC38](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day5_Lab_1.PNG)

- **Layout showing metal connections after routing**

![PIC38](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day5_Lab_2.PNG)

- **Pre and post routing timing analysis**

![PIC39](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day5_Lab_3.PNG)

![PIC40](https://github.com/rkuram/Beginner-Physical-design/blob/main/Images/Day5_Lab_4.PNG)

## Acknowledgements:

1.	Kunal Ghosh, Co-founder (VSD Corp. Pvt. Ltd)
2.	Nickson P Jose, Teaching Assistant (VSD Corp. Pvt. Ltd)





