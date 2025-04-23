# NASSCOM-VSD-SoC-Design-Program  
Exploring the ASIC design flow using OpenLane and the SkyWater 130nm PDK, a collaboration between Google and SkyWater. This program, led by industry expert Kunal Ghosh, provides hands-on experience in standard cell design, physical design, and GDSII generation.  

## 📌 About the Repository  
This repository documents my learnings from a 5-day intensive workshop on VLSI SoC design.  

### 🔹 **Note:**  
*All block diagrams and flowcharts in this repository are sourced from the VLSI System Design (VSD) SoC Design course.*  

[Visit VLSI System Design (VSD)](https://www.vlsisystemdesign.com/)  
## 📖 Table of Contents  
### 🟢 **Day 1: Introduction & Synthesis**
- Familarization with Openlane/sky130.  
- Physical Design (PnR) stages. 
- Design Preparationstage.
- Execution & analysis of Synthesis.
  
### 🟢 **Day 2: Floorplanning & Placement**  
- Introduction to floorplan phase.
- LEF vs DEF 
- Special cells.
- Execution & analysis of Placement.
  
  ### 🟢 **Day 3: Standard Cell Design & Characterization**  
- Std.cell design using sky130 PDK.
- SPICE simulation in ngspice.
- Sid.cell Characterization.

  ### 🟢 **Day 4: Timing Analysis & Clock Tree Synthesis (CTS)**  
- PnR with custom cell.
- Clock Tree Synthesis.
- Static Timing Analysis.
- Timing ECOs.

  ### 🟢 **Day 5: RTL-to-GDS Flow Completion**  
- Routing.
- Algorithm behind TritonRoute.
- SPEF analysis and extraction.

  ## 🏆 Acknowledgment  

---

## DAY-1 : 

### INTRODUCTION

DAY 1 Lecture Summary

  - Learned how VLSI technology integrates millions of tiny transistors into chips, enabling faster, smaller, and more affordable electronics.

  - Explored the Arduino board to see why its central microcontroller unit (MCU) is crucial.

  - The MCU handles everything—from reading sensors to controlling lights—thanks to VLSI advancements.

  - The Arduino board includes key components like the microprocessor (the brain of the system), voltage regulator, I/O pins, and communication interfaces, all working together to perform various tasks.

<li> Chip Packaging and Components </li>
<br>
  - Facilities that manufacture chips, called 'FOUNDRIES', are crucial because they determine the chip’s efficiency and performance.

<br>
  - Inside these chips are pre-designed digital blocks called 'MACROS' that help improve the chip’s functionality and speed.

<br>

Chips are generally made up of three major physical parts:

1) <b>Core</b>: The central zone where the full logic circuit is laid out.

2) <b>Pads</b>: Act as gateways for electrical signals to move in and out of the chip.

3) <b>Die<b>: Represents the total silicon area of the chip that contains both logic and I/O components.

![Screenshot 2025-04-10 175151](https://github.com/user-attachments/assets/71656b65-7f85-402f-adfe-eaecc95f9be4)
![Screenshot 2025-04-10 175839](https://github.com/user-attachments/assets/13c62d15-5733-4815-a589-29e8a047c770)

**<li>Components of open-source digital asic design </li>**
<br>
![Screenshot 2025-04-10 185239](https://github.com/user-attachments/assets/5178f569-824a-4bef-ba24-3564f0c72965)
<br>

Process Design Kit is PDK data which contains information about the manufacturing process. In 2020, Google collaborated with **SkyWater Technology** to release an **open-source FOSS 130nm** Production PDK.

**<li>RTL2GDS Flow</li>**

![Screenshot 2025-04-10 230449](https://github.com/user-attachments/assets/e0c449b7-3c0e-4c17-81ac-3372ecd65dad)

<br>

Above shows simplified RTL to GDSII flow.Steps invloved are explained as follows:

**Step 1 – Synthesis:**
This step converts the RTL design into a gate-level netlist using pre-designed blocks from the Standard Cell Library (SCL). These standard cells come in regular shapes but different sizes to perform basic logic functions.

**Step 2 – Floorplanning & Power Planning:**
Here, we decide the layout of major blocks on the chip—where inputs, outputs, and various components should go to save space.
Power planning involves setting up the chip's power supply network (VDD and GND) using dedicated routing structures.

**Step 3 – Placement:**
Standard cells are placed in their planned locations. This happens in two stages:

 Global Placement – Rough positioning of cells.

 Detailed Placement – Fine-tuning of exact cell positions.

**Step 4 – Clock Tree Synthesis (CTS):**
The clock signal is distributed throughout the chip using special tree-like structures to ensure it reaches all parts at the same time, preventing timing delays.

**Step 5 – Routing:**
Once placement and clock planning are done, we connect all components using metal layers:

 Global Routing – Planning the paths.

 Detailed Routing – Drawing the exact wire routes.

**Step 6 – Sign-off Checks:**
In the final step, the design is checked for correctness:

 DRC (Design Rule Check) – Ensures layout follows manufacturing rules.

 LVS (Layout vs. Schematic) – Confirms that layout matches the circuit design.

 STA (Static Timing Analysis) – Verifies the design meets timing constraints.

 
## OPENLANE AND STRIVE CHIPSETS

OpenLane began as an open-source project aiming to facilitate a genuine open-source tape-out experiment. It originated from e-fabless and serves as a platform that incorporates various tools like **Yosys, OpenRoad, Magic, KLlayout**, and other open-source tools. OpenLane streamlines the different stages of silicon implementation, making them easier to understand and work with. At e-fabless, they have a series of SOC (System on Chip) designs called Strive. Strive SOCs are entirely open, featuring open PDK (Process Design Kit), open RTL (Register Transfer Level), and open EDA (Electronic Design Automation) tools.

The Main aim of Openlane is to **produce clean GDSII without human intervention.**

<br>

**<li>OpenLANE ASIC design flow</li>**

<br>

![Screenshot 2025-04-10 233610](https://github.com/user-attachments/assets/e404c718-71c2-4b4b-ac74-d65719094bb7)

</ul>
<br>

### COMMANDS USED IN OpenLANE FLOW:

```
1. prep -design <design> -tag <tag> -config <config> -init_design_config -overwrite
2. run_synthesis
3. run_floorplan
4. run_placement
5. run_cts
6. run_routing
7. run_magic
8. run_magic
9. run_magic_spice_export
10. run_magic_drc
11. run_netgen
12. run_magic_antenna_check

for fully automated run we can use command : ./flow.tcl -deisgn picorv32a
```


### TOOL INVOCATION & OPERATION:

- We're using the Sky130_fd_sc_hd PDK variant.
- "Sky130" refers to the process or node name.
- "fd" indicates the foundry name, which is SkyWater foundry.
- "sc" denotes standard cell library files.
- "hd" represents high density, a specific variant.
- The Sky130_fd_sc_hd variant includes various technology files such as Verilog, Spice, Techlef, Meglef, Mag, GDS, CDL, LIB, LEF, etc.
- The techlef file provides layer information essential for the design process.

  <br>

<ul


**<li>Directory order to invoke the tool OPENLANE </li>**
```
Desktop/work/tools/openlane_working_dir/openlane
```

In order to enter into BASH in terminal ,we must use a command 
```
docker
```

Now enter the follwing commands to invoke the openlane in terminal i.e using bash programming:

```
-bash-4.2$ pwd
/OpenLANE_flow 
-ls -ltr ( it includes several files like flow.tcl,scripts,conf.py files,README files  nearly 136 files etc) as shown in below image

```
![Screenshot from 2025-04-11 15-13-23](https://github.com/user-attachments/assets/f9c3a482-0bbe-409f-a6e2-5c316071f21a)
Design preperation completed
![Screenshot from 2025-04-11 15-21-53](https://github.com/user-attachments/assets/679f4c75-336a-447a-bf24-b4a85f068f2d)

### GETTING STARTED - SYNTHESIZING THE DESIGN :

Next step is we need to perform the Synthesis process on the design. command used is

```
run_synthesis
```
It'll take a while (1-2 min) to perform synthesis but once it's done,we will see a message saying **'Synthesis was successful'**.
![Screenshot from 2025-04-11 15-37-43](https://github.com/user-attachments/assets/3eab5523-48e8-4060-af43-b9cc6c39c077)
![Screenshot from 2025-04-11 15-38-47](https://github.com/user-attachments/assets/2aacf3e9-a24f-434e-b0dc-c138b3e36445)
Total number of counter D flip-flops is **1613** and the total number of cells is **18036**.
The flipflop percentage is obtained by formula i.e **Flop Ratio = ((no of D_flipflops) / (Total no of cells))100**
so we get Flop ratio =(1613/18036)*100 = **8.94 %.**


## DAY-2

Configuration variables and their default values for *floorplanning* , *placement* , *CTS* , *Routing* , *magic* and *LVS*.
![Screenshot from 2025-04-11 23-28-41](https://github.com/user-attachments/assets/59584575-da7f-4b1b-94d4-4a30f50900f8)
![Screenshot from 2025-04-11 23-28-55](https://github.com/user-attachments/assets/9a122c4e-be18-4ee0-8b4d-02dd0f3142d6)
![Screenshot from 2025-04-11 23-29-08](https://github.com/user-attachments/assets/c683808c-02c3-4b22-b0d3-473b73367f68)
![Screenshot from 2025-04-11 23-29-19](https://github.com/user-attachments/assets/165a3739-6db4-4abd-b042-07012f377b5c)

### CHIP FLOOR PLANNING CONSIDERATION 

<ul>
    
**<li> UTILIZATION FACTOR AND ASPECT RATIO </li>**

In order to calculate the Utilization Factor and Aspect Ratio, we must know the height and width of core and die areas.
formula is given by:

**Utilization Factor = area occupied by the netlist / Total area occupied by the core**

--> If the utilization factor is 1 then it denotes 100 % utilization.
--> We never go for 100 % utilization, we go for 50 % or 60 % factor.

**Aspect Ratio = height of the core / width of the core**

--> When aspect ratio is 1 , then its a square chip.
![Screenshot 2025-04-11 174127](https://github.com/user-attachments/assets/4c9a36b3-5bed-4bc1-ba48-df3f7f0d5887)

### STEPS TO RUN FLOORPLAN & REVIEW FLOORPLAN FILES 

Once synthesis is sucessful,next step is floorplan. we use command 
```
run_floorplan
```
This will create a folder inside **runs** folder of **picorv32a** directory.
it will take a while to execute.once done we will get **PDN GENERATION IS SUCESSFUL** as shown in below image.
![Screenshot from 2025-04-11 23-26-36](https://github.com/user-attachments/assets/5e39b09f-abfe-4440-9b33-4a15f6915618)
![Screenshot from 2025-04-11 23-35-43](https://github.com/user-attachments/assets/d5da6b11-42b1-4402-9aa3-b29d0e852110)
Units is measured in microns,
**DIEAREA** is given in the below image:
![Screenshot from 2025-04-11 23-45-25](https://github.com/user-attachments/assets/78ad6dc4-ca2b-4af4-8b39-11f9bacd2714)

To see the actual layout after the floorplan ,go the folders as shown below:
```
openlane/designs/picorv32a/runs/08-02_06-43/results/floorplan
```

now we need to open the **magic** file by the  following command
<br>

```
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```
![Screenshot from 2025-04-11 23-50-19](https://github.com/user-attachments/assets/25460a84-cbe8-4c98-b0f6-bddbb91db711)
<br>
To zoom in press left button of mouse then right button and press z 
<br>
![Screenshot from 2025-04-11 23-51-06](https://github.com/user-attachments/assets/ba73e104-3a00-4a1b-9a7e-39749c219d25)
<br>

In order to know the details of any cell in the layout , just move the cursor to that cell and press **S** to select the cell and then in the window of **tkcon** enter the command **"what"**. then it will displey the details of the selected cell.
lets see the detail of horizontal and vertical pins , in tkcon window it shows that the pin is in the metal 3 for horizontal pins ,similarly for the vertical pins, we see that thepin is at metal 2. image is shown below.
<br>
![Screenshot from 2025-04-11 23-57-49](https://github.com/user-attachments/assets/18687fb1-f773-4cd2-a638-4405aec81a72)
![Screenshot from 2025-04-12 00-00-19](https://github.com/user-attachments/assets/1f5e1688-8e95-4f6e-b0b5-5d492adbb008)

<br>

![Screenshot from 2025-04-12 00-02-45](https://github.com/user-attachments/assets/7ba4b55a-b72b-4ee2-8347-865003d7e151)
![Screenshot from 2025-04-12 00-02-14](https://github.com/user-attachments/assets/8dd29a53-6499-45eb-b009-73433be85541)

<br>

### Library Binding and Placement

Netlist binding and initial place design.
![Screenshot 2025-04-12 124252](https://github.com/user-attachments/assets/4fb55a31-c928-4b6e-8da0-a4794a056e8d)
![Screenshot 2025-04-12 122723](https://github.com/user-attachments/assets/87f9f41f-d378-40ef-883c-f053affdca1e)

Optimize placement using estimated wire-length and capacitance.

When sender and receiver are far diastance use buffer and repeaters.Hence there will no single loss.
![Screenshot 2025-04-12 130144](https://github.com/user-attachments/assets/0fe18d7b-b2b1-40f4-b3dd-7148d81390f7)

### STEPS TO RUN PLACEMENT

Once the Floorplanning is done sucessfully , the next step in the process is Placement. 
To run the placement use the command
```
run_placement
```
![Screenshot from 2025-04-12 13-25-36](https://github.com/user-attachments/assets/85305ce2-b750-457f-ae41-3fa95d6e6634)

Once Placement process is done , next step is to check whether the cells are placed correctly or not.by using **magic** tool.

use the below command to view the standerd cells placement. image is shown below
```
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &

```

![Screenshot from 2025-04-12 13-30-06](https://github.com/user-attachments/assets/7423f9a5-3895-4ed6-892e-b1f51abfdfd3)
![Screenshot from 2025-04-12 13-31-46](https://github.com/user-attachments/assets/9df78398-bd70-4ac2-8774-508f49e750f2)

### Cell Design Flow

![Screenshot 2025-04-12 144428](https://github.com/user-attachments/assets/54e33ac6-6361-4127-a468-f14d0dfa8401)


## DAY-3

### **SPICE deck creation for CMOS inverter**

- Component connectivity
- Component values
- Identify 'nodes'
- Name 'nodes'
![Screenshot 2025-04-12 183145](https://github.com/user-attachments/assets/20b81dd1-121f-4b45-b21d-ee6e07b422af)
![Screenshot 2025-04-12 184428](https://github.com/user-attachments/assets/874ef770-d5b4-4bec-aafe-d85263c5bbc4)

Switching Threshold Vm
![Screenshot 2025-04-12 185024](https://github.com/user-attachments/assets/976bd445-a1df-4641-ba45-c2704ee598b5)


  1.Git clone vsdstdcelldesign    
  Commands to open inverter layout in magic  
  
```bash
  #Change directory to openlane
  cd Desktop/work/tools/openlane_working_dir/openlane

  #Clone the repository with custom inverter design
  git clone https://github.com/nickson-jose/vsdstdcelldesign

  #Change into repository directory
  cd vsdstdcelldesign

  #Copy magic tech file to the repo directory for easy access
  cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech .

  #Check contents whether everything is present
  ls

  #Command to open custom inverter layout in magic
  magic -T sky130A.tech sky130_inv.mag &
```
![Screenshot from 2025-04-12 21-34-00](https://github.com/user-attachments/assets/939b694e-3721-413f-b80b-d6bd81d69169)
![Screenshot from 2025-04-12 21-33-15](https://github.com/user-attachments/assets/86e86bbc-3ce4-4229-a185-7a0a153f97ee)
CMOS inverter layout
![Screenshot from 2025-04-12 21-33-00](https://github.com/user-attachments/assets/cc20cd98-bbe3-409b-8dad-18860d920e96)
NMOS
![Screenshot from 2025-04-12 21-59-06](https://github.com/user-attachments/assets/7c96f708-a5cf-4d48-bfbf-15adac647d22)
PMOS
![Screenshot from 2025-04-12 21-55-43](https://github.com/user-attachments/assets/1fc9036f-ed5f-4bc1-946b-6eedb6998af4)
Y(output) connecting the drain of nmos and pmos
![Screenshot from 2025-04-12 21-58-31](https://github.com/user-attachments/assets/90b76091-a32a-4c7e-8416-bc88c930f89b)
Poly
![Screenshot from 2025-04-12 22-00-11](https://github.com/user-attachments/assets/9f081bb3-d615-423c-98d7-777701c13166)
CMOS fabrication 
![Screenshot 2025-04-12 215022](https://github.com/user-attachments/assets/97f40cec-99c8-4350-9730-536ba7936453)
Spice file
![Screenshot from 2025-04-13 09-33-00](https://github.com/user-attachments/assets/137b8f19-c308-46fe-8fc6-ac2b6dca77ec)

2.Editing the spice model file for analysis through simulation. Edited spice file
![Screenshot from 2025-04-13 10-01-05](https://github.com/user-attachments/assets/f8381a63-2e85-4c50-b4f2-c355558bceba)

 3.Spice extraction of inverter in magic.  

```bash
  #Check current directory
  pwd

  #Extraction command to extract to .ext format
  extract all

  #Before converting ext to spice this command enable the parasitic extraction also
  ext2spice cthresh 0 rthresh 0

  #Converting to ext to spice
  ext2spice
```

![Screenshot from 2025-04-13 09-25-35](https://github.com/user-attachments/assets/be1ac524-24b7-411f-bdcb-3a28293f5197)
 4.Ngspice simulation
  Commands for ngspice simulation
  
```bash
  #Command to directly load spice file for simulation to ngspice
  ngspice sky130_inv.spice

  #Now that we have entered ngspice with the simulation spice file loaded we just have to  load the plot
  plot y vs time a
```
![Screenshot from 2025-04-13 09-49-20](https://github.com/user-attachments/assets/c828dc22-155b-48da-ad4d-3bfa1d0ad0b1)
Transient Analysis of the CMOS inverter is given below:
When C=0.07fF
 ![Screenshot from 2025-04-13 09-50-04](https://github.com/user-attachments/assets/dd45778d-f6a3-4075-a919-7574844b248d)
when C=2fF
![Screenshot from 2025-04-13 09-54-03](https://github.com/user-attachments/assets/7dbc06a7-cb9a-4987-867a-d5c5ee531871)


next step is to calculate the rise time , fall time , cell rise delay and cell fall delay:
click on the waveforms to get values i.e 

--> 20 % of VDD (3.3 V) is 0.66V
--> 80 % of VDD (3.3 V) is 2.64V
--> 50 % of VDD (3.3 V) is 1.65V
<br>
1) **Rise time**- Time taken to the output waveform to 20% value to 80% value.

<br>

![Screenshot 2025-04-13 102528](https://github.com/user-attachments/assets/d18974c0-8425-48c4-ada0-6ed073bdc82f)

<br>
therfore rise time= (2.24 - 2.18)e-09 = 0.64e-09s
  
2) **Fall time**- Time taken to the output waveform to 80% value to 20% value.
<br>


<br>
therfore fall time = (2.17999 - 2.12)e-09 = 0.5999e-09s


5) **cell rise delay**-Time difference between the 50% value of input and 50% value of the output.

<br>

![Screenshot 2025-04-13 102543](https://github.com/user-attachments/assets/92003410-05bc-498f-acac-1c1a9f6f7bf9)


<br>
therfore cell rise delay or propogation delay = (2.2107 - 2.1501)e-09 = 0.061e-09s

7) **cell fall delay**-Time difference between output falling to 50% and input is rising to 50% value.

<br>

![Screenshot 2025-04-13 102603](https://github.com/user-attachments/assets/9463fbc8-2717-458e-b148-915cbe34b375)


<br>
thefore cell fall delay = (4.077 -4.05)e-09 = 0.027e-09 s

</ul>

### INTRODUCTION TO MAGIC TOOL AND STEPS TO LOAD SKY130 TECH - RULES 
<ul>
First step is to download the lab files required for DRC error fixing by below coammand
    
```
   wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
```
to open magic use 
```
  magic -d XR

```
![Screenshot from 2025-04-13 11-56-05](https://github.com/user-attachments/assets/983b78a2-2145-428d-8136-26796249ee16)
![Screenshot from 2025-04-13 11-58-28](https://github.com/user-attachments/assets/d1c14eb3-5d20-4f67-80a9-84ae89536ddb)
.magicrc file
![Screenshot from 2025-04-13 11-57-40](https://github.com/user-attachments/assets/431cfce0-aab9-4ba7-a54b-c6d41ad22a50)

Open met met3.mag file from file -> Open in magic window

![Screenshot from 2025-04-13 12-01-01](https://github.com/user-attachments/assets/defa4041-eb6d-479b-a962-a0a6ce6ae758)
![Screenshot from 2025-04-13 12-01-38](https://github.com/user-attachments/assets/a67f7acc-67b3-4d60-b4ef-da7fcfdfe794)
<br>

 ![Screenshot from 2025-04-13 12-05-25](https://github.com/user-attachments/assets/e5dfe07f-d453-485c-83d3-d467a2cffd1c)
 
![Screenshot from 2025-04-13 12-05-32](https://github.com/user-attachments/assets/d8ecf475-fe49-453c-94b7-df979bdc3605)
![Screenshot from 2025-04-13 12-06-54](https://github.com/user-attachments/assets/89af72ad-e06b-4d97-a2a4-df663910a0d4)

<br>
There are some changes to be made in **sky130A.tech file**. Search for the **poly.9** in the **sky130A.tech** file. It appears in both the **POLY** and **uhrpoly** sections,changes given below:

open sky130A.tech file as shown below
<br>

![Screenshot 2025-04-13 125739](https://github.com/user-attachments/assets/0ede15cf-3fdb-416b-a3c4-e58eb1087de6)

<br>

<br>

![Screenshot from 2025-04-13 12-13-11](https://github.com/user-attachments/assets/4e6c523b-63ad-4f2c-91da-3fbf428cd330)
<br>
<br>

![Screenshot from 2025-04-13 12-16-32](https://github.com/user-attachments/assets/e3a93a31-4dfd-4e06-8825-e0c5e820fcef)

<br>
next step is to load the sky130A.tech file in tckon window as as shown below:
<br>

![Screenshot from 2025-04-13 12-18-41](https://github.com/user-attachments/assets/4ec15f54-f9f3-460b-8338-b710a6a24256)
<br>

DRC is checked as shown in below image:
<br>
![Screenshot from 2025-04-13 12-20-40](https://github.com/user-attachments/assets/932bc1af-1481-443e-a39c-529ce2975284)
<br>
</ul>

## DAY-4


### PRELAYOUT TIMING ANALYSIS AND CLOCK TREE SYNTHESIS <ul>
The next step is to get the **.lef** file from the inverter design.

Open the tracks.info file 
directory is : 
```
openlane_working_dir/pdk/sky130/libs.tech /openlane/sky130_fd_sc_hd/track.info 
```
![Screenshot from 2025-04-13 14-27-10](https://github.com/user-attachments/assets/283d203f-0fd1-4e26-a65b-ad6dd3d3a6aa)

Change grid into tracks in console window as shown in below image:
![Screenshot from 2025-04-13 14-31-00](https://github.com/user-attachments/assets/71b10579-3438-44dc-9d97-a7ebe1f35668)
![Screenshot from 2025-04-13 14-32-46](https://github.com/user-attachments/assets/4d99b07e-3dae-4485-98ff-6a5d5a688960)
![Screenshot from 2025-04-13 14-33-04](https://github.com/user-attachments/assets/67cd8257-45f3-48cf-85d3-0620e8ea9624)

Intersection of grids at the input(A) and output(Y) of the inverter.
![Screenshot from 2025-04-13 14-34-21](https://github.com/user-attachments/assets/abf9353d-e49a-4333-920f-f35bfe3c32da)
![Screenshot from 2025-04-13 14-40-16](https://github.com/user-attachments/assets/05d5b600-764d-4064-ae4d-f35242163d7b)

*<li>Steps to convert magic layout to std cell LEF</li>*

for the extraction of lef file ,enter the given below command in the tckon window.

```
lef write
```
![Screenshot from 2025-04-13 19-07-06](https://github.com/user-attachments/assets/92e2ab6b-adc2-4aa7-90e7-bab456d575c5)
![Screenshot from 2025-04-13 19-07-37](https://github.com/user-attachments/assets/1b9833af-0334-457d-9388-afdadcc6a76e)

now the *sky130_inv.lef* file is created in *vsdstdcelldesign* folder in *openlane* directory.

![Screenshot from 2025-04-13 19-09-13](https://github.com/user-attachments/assets/1db2bd47-5eca-4ce7-9b48-cf171e1891ee)

The next step is to copy the newly created *.lef* file to src folder of *picorv32a* directory along with some important libraries.

libraries are present in following directory
![Screenshot 2025-04-14 174756](https://github.com/user-attachments/assets/ec8c7289-cf8d-4d85-a5fd-f60f31133c54)
![Screenshot from 2025-04-13 21-14-50](https://github.com/user-attachments/assets/34b41a86-599c-41dd-8ba5-2e229a1b5394)

Now the next step is to edit the config.tcl of picorv32a design.

![Screenshot from 2025-04-14 11-14-12](https://github.com/user-attachments/assets/aeb8eace-0225-4239-b1f6-e2e3aa0bad4a)

config.tcl file is present in picorv32a folder

NOW go to the openlane directory run **docker** command to go to bash terminal

next commands are given below:

```
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]      
add_lefs -src $lefs
run_synthesis
```
![Screenshot from 2025-04-14 11-05-49](https://github.com/user-attachments/assets/f3879604-b67d-490d-be8f-9c50d98f054a)
![Screenshot from 2025-04-14 11-13-27](https://github.com/user-attachments/assets/6bebb053-6f1e-472a-9ced-08c138d4f2bb)

**<li> Steps to configure synthesis settings to fix slack </li>**

Once the synthesis is sucessfull, check the slack

![Screenshot from 2025-04-14 12-31-42](https://github.com/user-attachments/assets/214f0302-49d0-4091-acf3-32732ec44a85)
![Screenshot from 2025-04-14 12-33-14](https://github.com/user-attachments/assets/9f0a6c71-9dbc-4bcd-b3df-bc50a2102387)

we get wns= -23.89  and tns== -711.5.

next command is to run floorplan

```
run_floorplan

```
if error occurs in floorplan stage use below commands one by one :
```
init_floorplan

place_io

tap_decap_or

```
![Screenshot from 2025-04-14 12-39-24](https://github.com/user-attachments/assets/cde42ef5-27c2-4a8f-bc1c-0594fcfdc563)
![Screenshot from 2025-04-14 12-40-23](https://github.com/user-attachments/assets/df87b5a4-ad57-4b39-8132-19daaf5973d8)
![Screenshot from 2025-04-14 17-32-46](https://github.com/user-attachments/assets/1048e039-32d2-4049-a5ee-432fbec861d8)
![Screenshot from 2025-04-14 17-33-30](https://github.com/user-attachments/assets/6c965f4a-24dc-4402-bdfb-6e6e797fcadf)

next command is to run placement
```
run_placement

```
![Screenshot from 2025-04-14 17-38-02](https://github.com/user-attachments/assets/3981dd89-861d-4b25-81d2-b595050f0b28)
The standard cell inverter can be seen in the below image using magic tool
![Screenshot from 2025-04-14 18-30-33](https://github.com/user-attachments/assets/b9226a86-d938-4acc-9f17-978bbd15523b)
use the below command in tckon window to check whether inverter placed properly or not
```
expand
```
![Screenshot from 2025-04-14 18-39-43](https://github.com/user-attachments/assets/f3d8ae3e-f87e-4755-bb43-e51e838826af)
![Screenshot from 2025-04-14 18-54-22](https://github.com/user-attachments/assets/f4de0937-8816-448b-96f0-94a891f443d9)
![Screenshot from 2025-04-14 19-00-50](https://github.com/user-attachments/assets/7786d11a-78bc-4cdc-9acb-f0873087472a)
![Screenshot from 2025-04-14 18-54-49](https://github.com/user-attachments/assets/6cb3e238-0f53-453c-8ba5-50bb5166d207)

next create a my_base.sdc file in openlane/designs/picorv32a/src directory. write the code as given in below image in my_base.sdc file.

![Screenshot from 2025-04-15 19-37-05](https://github.com/user-attachments/assets/da3c6b1c-899b-4a74-b8e4-d3ef5e8e0d28)

![Screenshot from 2025-04-15 19-34-27](https://github.com/user-attachments/assets/152cef12-eb8c-4e29-90f7-f0aa4f66805d)

![Screenshot from 2025-04-15 19-35-30](https://github.com/user-attachments/assets/c55ee800-c98d-4d21-9c0c-4eef9cc855cc)

![Screenshot from 2025-04-15 23-06-45](https://github.com/user-attachments/assets/bea8dfd6-d21b-4038-897b-812d0d1a0224)

![Screenshot from 2025-04-15 23-33-34](https://github.com/user-attachments/assets/bce96766-cf44-4a3d-81ac-5e89a1f0dbad)

![Screenshot from 2025-04-15 23-56-55](https://github.com/user-attachments/assets/2c8b7a24-7639-47c8-9106-584f29397351)

To Run clocktree synthesis use below command :

```
run_cts
```

![Screenshot from 2025-04-16 00-00-15](https://github.com/user-attachments/assets/b6ceb29c-7de6-44c1-b467-c2d695638c05)


next commands :

```
set ::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST) 0 0]
```


```
set ::env(CURRENT_DEF) /openLANE_flow/designs/picorv32a/runs/16-02_07-04/results/placement/picorv32a.placement.def
run_cts

```


<li> Enter the openROAD flow and check timing by using the following commands </li>

```
openroad
read_lef /openLANE_flow/designs/picorv32a/runs/16-02_07-04/tmp/merged.lef
read_def /openLANE_flow/designs/picorv32a/runs/16-02_07-04/results/cts/picorv32a.cts.def
write_db pico_cts1.db
read_db pico_cts1.db
read_verilog /openLANE_flow/designs/picorv32a/runs/16-02_07-043/results/synthesis/picorv32a.synthesis_cts.v
read_liberty $::env(LIB_SYNTH_COMPLETE)
link_design picorv32a
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc
set_propagated_clock [all_clocks]
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

exit

echo $::env(CTS_CLK_BUFFER_LIST)
set ::env(CTS_CLK_BUFFER_LIST) [linsert $::env(CTS_CLK_BUFFER_LIST) 0 sky130_fd_sc_hd__clkbuf_1]
echo $::env(CTS_CLK_BUFFER_LIST)


```

![Screenshot from 2025-04-16 00-18-08](https://github.com/user-attachments/assets/dced4030-6ce8-45c7-ba92-f494fc38735f)


## DAY-5 


###  Final step for RTL2GDS using tritinRoute and openSTA

first step is to build  the power distribution network by follwing command :
```
gen_pdn

```
![Screenshot from 2025-04-16 23-58-36](https://github.com/user-attachments/assets/a40f60e6-1d2a-4d13-bc41-ba2ee3c14bc8)
![Screenshot from 2025-04-16 23-58-53](https://github.com/user-attachments/assets/296647d3-3872-481b-bb83-aa1207032e30)

To check PDN, open Magic tool go to /tmp/floorplan/ indside the run folder in openlane directory by using below commands :

```

magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read 14-pdn.def &
```  Commands to load PDN def in magic in another terminal

```bash
  #Change directory to path containing generated PDN def
  cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/16-02_07-04/tmp/floorplan/

  #Command to load the PDN def in magic tool
  magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read 14-pdn.def &

```
![Screenshot 2025-04-17 195641](https://github.com/user-attachments/assets/80d209d8-a0bd-4fef-b68e-bacbff428c31)
![Screenshot 2025-04-17 195656](https://github.com/user-attachments/assets/250fddd1-b2c3-470d-98fe-8db9ada085b1)
 Final step is routing to run routing use below command :

```
run_routing
```
![Screenshot from 2025-04-17 00-24-15](https://github.com/user-attachments/assets/262f671c-7c77-46ca-bac4-f4788793ae2d)
![Screenshot from 2025-04-17 00-35-33](https://github.com/user-attachments/assets/41bf7f9e-69da-4dcc-8899-a3d6af45cc20)

To view the final design  with routing in Magic tool , go to the results/routing/ in  the runs folder of openlane directory by using below command :

```
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.def &

```
![Screenshot from 2025-04-17 19-07-42](https://github.com/user-attachments/assets/cd89a5b8-8cdd-433f-947b-c09e60b752e5)
![Screenshot from 2025-04-17 19-08-33](https://github.com/user-attachments/assets/dd1bf345-9ddd-4bec-8b51-690b079a0b77)
![Screenshot from 2025-04-17 19-08-23](https://github.com/user-attachments/assets/61d4785b-4387-4467-b4ee-7680effa9656)


## Acknowledgement


I want to give a big thank you to Mr. Kunal Ghosh, one of the founders of VLSI System Design (VSD) Corp. Pvt. Ltd., and Mr. Nickson Jose for their amazing help during the DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING workshop. They really know their stuff when it comes to chip design using OpenLANE software and other cool techniques. Their guidance has been super valuable in teaching me all about it.








































