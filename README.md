# OpenLANE-Sky130-Physical-Design
Documentation for the 5 day workshop: Advanced Physical Design using OpenLane/Sky130.Aim is to cover the complete RTL2GDS flow using the open-source flow OpenLane with SKY130nm PDK.
DAY 1: Inception of Open-source EDA, OpenLane and Sky130 PDK
The core of the chip will contain two types of blocks:

Foundry IP Blocks (e.g. ADC, DAC, PLL, and SRAM) = blocks which requires some amount of intelligent techniques to build which can only be designed by foundries.
Macro blocks (e.g. RISC-V SOC and SPI) = pure digital logic blocks compared to IP's which might require some analog parts.
![Chip](https://github.com/PriyankaShinde2/OpenLANE-Sky130-Physical-Design/assets/135041446/1f761ee6-b500-4549-8bda-025f02e90220)

Open Source Digital ASIC Design requires three open-source components:

    RTL Designs = github.com, librecores.org, opencores.org
    EDA Tools = OpenROAD, OpenLANE,QFlow
    PDK = Google + Skywater 130nm Production PDK
PDK (Process Design Kit) = A set of data files and documents which serves as the interface between the designer and the fab. This includes cell libraries, IO libraries, design rules (DRC, LVS, etc.)

OpenLane = An open-source ASIC development flow reference. It consists of multiple open-source tools needed for the whole RTL to GDSII flow. This is tuned epecially for Sky130 PDK. It also works for OSU 130nm. It is recommended to read the OpenLANE documentation before moving forward
![DesignFlow](https://github.com/PriyankaShinde2/OpenLANE-Sky130-Physical-Design/assets/135041446/5598aa22-01ec-45de-a987-df1a6edec648)

Lab [Day 1] - Determine Flip-flop Ratio:
The task is to find the flip-flop ratio for the design picorv32a. This is the ratio of the number of flip flops to the total number of cells.

Steps to be followed after installing OpenLANE :
1. Run OpenLANE:

$ make mount = Open the docker platform inside the openlane/
% flow.tcl -interactive = Used to run the RTL to GDSII flow in interactive mode
% package require openlane 0.9 == retrives all dependencies for running v0.9 of OpenLANE

![openlane](https://github.com/PriyankaShinde2/OpenLANE-Sky130-Physical-Design/assets/135041446/7f5f616d-50c0-4a2b-9917-377844288046)

2. Design Setup Stage:
![design Prep](https://github.com/PriyankaShinde2/OpenLANE-Sky130-Physical-Design/assets/135041446/f8064ace-077d-43fb-9ca0-e4903e43d6d8)

% prep -design picorv32a = Setup the filesystem where the OpenLANE tools can dump the outputs. This also creates a run/ folder inside the specific design directory which contains the command log files, results, and the reports dump by each tools. These folders will be empty for now except for lef files generated by this design setup stage. This merged the cell LEF files .lef and technology LEF files .tlef generating merged.nom.lef inside run/tmp/



3. Run synthesis:

% run_synthesis = Run yosys RTL synthesis, ABC scripts (for technology mapping), and OpenSTA.
![FFRatio](https://github.com/PriyankaShinde2/OpenLANE-Sky130-Physical-Design/assets/135041446/202aafd9-6d31-4990-8926-3592335ceaba)

The flipflop ratio is (number of flip flops)/(total number of cells) is 1613/18036 = 0.0894. Or 8.94%

After running synthesis, inside the runs/[date]/results/synthesis is picorv32a_synthesis.v which is the mapping of the netlist to standard cell library using ABC. The runs/[date]/reports/synthesis will contain synthesis statistic reports and static timing analysis reports. The runs/[date]/synthesis/logs contains log files for the terminal output dumps for running yosys and OpenSTA.
