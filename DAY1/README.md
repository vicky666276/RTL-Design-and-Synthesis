RTL Design and Synthesis using SKY130

Overview

This repository contains my complete documentation and lab work from the RTL Design and Synthesis Workshop using SKY130.
The workshop focused on Verilog RTL design, simulation, synthesis, optimization techniques, and gate-level analysis using open-source EDA tools.

Throughout the workshop, I worked with tools such as:

* iverilog – Verilog Simulation
* GTKWave – Waveform Visualization
* Yosys – RTL Synthesis
* SKY130 Standard Cell Library – Technology Mapping

The lab sessions covered the complete RTL-to-Gate-Level flow including:

* Writing Verilog RTL code
* Creating testbenches
* Running simulations
* Generating waveform outputs
* Synthesizing RTL designs
* Technology mapping using SKY130 libraries
* Viewing gate-level schematics

⸻

Tools Used

Tool	Purpose
iverilog	Verilog Compilation & Simulation
GTKWave	Waveform Viewer
Yosys	RTL Synthesis
SKY130 Library	Standard Cell Technology Mapping

⸻

Day 1 – Introduction to Verilog RTL Design & Synthesis

Topics Covered

* Introduction to Verilog HDL
* Design and Testbench
* Simulation Flow
* Introduction to iverilog
* GTKWave Waveform Analysis
* Introduction to Yosys
* Standard Cell Libraries
* Technology Mapping
* Gate-Level Schematic Generation

⸻

Simulation Flow

Design + Testbench
        ↓
     iverilog
        ↓
      a.out
        ↓
     GTKWave

⸻

Lab – 2:1 Multiplexer Simulation

Clone the Repository

git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop
cd sky130RTLDesignAndSynthesisWorkshop

⸻

Move to Verilog Files

cd verilog_files

⸻

Compile the Design

iverilog good_mux.v tb_good_mux.v

⸻

Run the Simulation

./a.out

⸻

Open GTKWave

gtkwave tb_good_mux.vcd

⸻

GTKWave Output

Add your waveform screenshot here

<img width="1600" height="874" alt="WhatsApp Image 2026-05-22 at 9 36 29 PM" src="https://github.com/user-attachments/assets/aed77aaf-49fc-48c9-af9d-b2ff1791165e" />


⸻

RTL Code – 2:1 Multiplexer

module good_mux (input i0,input i1,input sel,output reg y);
always @ (*)
begin
    if(sel)
        y <= i1;
    else
        y <= i0;
end
endmodule

⸻

Working Principle

sel	Output
0	i0
1	i1

The multiplexer selects one input based on the select signal.

⸻

Introduction to Yosys

Yosys is an open-source RTL synthesis tool used to convert Verilog RTL designs into gate-level netlists.

Functions performed by Yosys:

* RTL Parsing
* Logic Optimization
* Technology Mapping
* Netlist Generation

⸻

SKY130 Standard Cell Library

The SKY130 library contains different standard logic cells such as:

* NAND Gates
* NOR Gates
* Buffers
* Inverters
* Flip-Flops

Different variants of gates exist for:

* Speed Optimization
* Area Reduction
* Power Optimization
* Drive Strength

⸻

Synthesis Flow using Yosys

Start Yosys

yosys

⸻

Read SKY130 Liberty File

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

⸻

Read Verilog Design

read_verilog good_mux.v

⸻

Synthesize the Design

synth -top good_mux

⸻

Technology Mapping

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

⸻

Generate Gate-Level Schematic

show

⸻

Gate-Level Schematic

Add your synthesized schematic screenshot here

<img width="928" height="291" alt="WhatsApp Image 2026-05-22 at 9 35 59 PM" src="https://github.com/user-attachments/assets/9a362513-3588-4021-bce7-f6960b8bbd5c" />


⸻

Learning Outcomes

By the end of Day 1, I learned:

* Basics of Verilog HDL
* Difference between design and testbench
* RTL simulation using iverilog
* Waveform analysis using GTKWave
* RTL synthesis using Yosys
* Technology mapping using SKY130 standard cells
* Gate-level schematic visualization

⸻

Conclusion

This workshop provided practical exposure to RTL design and synthesis using open-source EDA tools.
The hands-on labs helped in understanding the complete flow from Verilog RTL coding to gate-level implementation using SKY130 standard cell libraries.
