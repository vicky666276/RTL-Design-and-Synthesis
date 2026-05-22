<div align="center">

# Day 1 – Introduction to Verilog RTL Design & Synthesis

### RTL Simulation, Waveform Analysis and Synthesis using Open-Source EDA Tools

<br>

![Verilog](https://img.shields.io/badge/Verilog-HDL-orange?style=flat-square)
![iverilog](https://img.shields.io/badge/iverilog-Simulator-blue?style=flat-square)
![GTKWave](https://img.shields.io/badge/GTKWave-Waveform_Viewer-yellow?style=flat-square)
![Yosys](https://img.shields.io/badge/Yosys-Synthesis-green?style=flat-square)
![SKY130](https://img.shields.io/badge/SKY130-Standard_Cell_Library-red?style=flat-square)

</div>

---

# 📌 Overview

Day 1 focused on understanding the fundamentals of Verilog HDL, RTL simulation flow, waveform analysis, and RTL synthesis using open-source EDA tools.

The lab sessions introduced the complete RTL-to-Gate-Level design flow beginning from Verilog RTL coding to waveform generation and synthesis using the SKY130 standard cell library.

---

# 🎯 Objectives

* Understand the basics of Verilog HDL
* Learn the difference between Design and Testbench
* Perform RTL simulation using iverilog
* Analyze waveforms using GTKWave
* Understand RTL synthesis flow using Yosys
* Learn technology mapping using SKY130 standard cells

---

# 🛠️ Tools Used

| Tool                         | Purpose                          |
| ---------------------------- | -------------------------------- |
| iverilog                     | Verilog Compilation & Simulation |
| GTKWave                      | Waveform Visualization           |
| Yosys                        | RTL Synthesis                    |
| SKY130 Standard Cell Library | Technology Mapping               |

---

# 📘 Topics Covered

* Introduction to Verilog HDL
* Design and Testbench
* RTL Simulation Flow
* Introduction to iverilog
* GTKWave Waveform Analysis
* Introduction to Yosys
* SKY130 Standard Cell Libraries
* Technology Mapping
* Gate-Level Schematic Generation

---

# 🔄 RTL Simulation Flow

Design + Testbench
        ↓
     iverilog
        ↓
      a.out
        ↓
     .vcd File
        ↓
     GTKWave


---

# 🧪 Lab Experiment – 2:1 Multiplexer Simulation

A simple 2-to-1 multiplexer was simulated using Verilog HDL to understand the RTL simulation and verification flow.

---

## Step 1 – Clone the Workshop Repository

```bash
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop
cd sky130RTLDesignAndSynthesisWorkshop
```

---

## Step 2 – Navigate to the Verilog Source Directory

```bash
cd verilog_files
```

This directory contains:

* RTL design files
* Testbench files
* Simulation examples

---

## Step 3 – Compile the RTL Design and Testbench

```bash
iverilog good_mux.v tb_good_mux.v
```

This command compiles:

* RTL Design (`good_mux.v`)
* Testbench (`tb_good_mux.v`)

and generates the executable simulation file `a.out`.

---

## Step 4 – Execute the Simulation

```bash
./a.out
```

Running the executable generates the waveform dump (`.vcd`) file required for waveform analysis.

---

## Step 5 – Open GTKWave

```bash
gtkwave tb_good_mux.vcd
```

GTKWave displays the waveform generated during simulation and helps verify the functional behavior of the RTL design.

---

# 📸 GTKWave Simulation Output

> Insert the waveform screenshot captured during simulation.

<img width="1600" height="874" alt="WhatsApp Image 2026-05-22 at 9 36 29 PM" src="https://github.com/user-attachments/assets/24b8381b-3d36-4152-ae43-8105cae3548e" />

---

# 💻 RTL Code – 2:1 Multiplexer

```verilog
module good_mux (input i0,input i1,input sel,output reg y);

always @ (*)
begin
    if(sel)
        y <= i1;
    else
        y <= i0;
end

endmodule
```

---

# ⚙️ Working Principle

| sel | Output |
| --- | ------ |
| 0   | i0     |
| 1   | i1     |

The multiplexer selects one of the two inputs depending on the value of the select signal.

---

# 🧠 Introduction to Yosys

Yosys is an open-source RTL synthesis tool used to convert Verilog RTL designs into gate-level netlists.

Major operations performed by Yosys include:

* RTL Parsing
* Logic Optimization
* Technology Mapping
* Gate-Level Netlist Generation

---

# 📚 SKY130 Standard Cell Library

The SKY130 library contains different standard logic cells used during synthesis.

Examples include:

* NAND Gates
* NOR Gates
* Buffers
* Inverters
* Flip-Flops

Different gate variants exist for:

* Speed Optimization
* Area Reduction
* Power Optimization
* Drive Strength Requirements

---

# 🔄 RTL Synthesis Flow using Yosys

## Launch Yosys

```bash
yosys
```

---

## Read SKY130 Liberty File

```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

This command loads the SKY130 standard cell timing library.

---

## Read Verilog RTL Design

```bash
read_verilog good_mux.v
```

---

## Synthesize the Design

```bash
synth -top good_mux
```

This step converts the RTL code into generic logic representation.

---

## Technology Mapping

```bash
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

Technology mapping replaces generic logic cells with actual SKY130 standard cells.

Examples:

* Generic MUX → SKY130 MUX Cell
* Generic NAND → SKY130 NAND Gate

---

## Generate Gate-Level Schematic

```bash
show
```

This command displays the synthesized gate-level schematic of the design.

---

# 📸 Gate-Level Schematic

<img width="928" height="291" alt="WhatsApp Image 2026-05-22 at 9 35 59 PM" src="https://github.com/user-attachments/assets/221cf132-00f0-4f40-adf4-8fe204441070" />

---

# 🎯 Learning Outcomes

By the end of Day 1, I gained practical understanding of:

* Verilog RTL Coding
* Design and Testbench Concepts
* RTL Simulation using iverilog
* Waveform Analysis using GTKWave
* RTL Synthesis using Yosys
* Technology Mapping using SKY130 Libraries
* Gate-Level Schematic Visualization

---

# 🚀 Conclusion

Day 1 provided a strong foundation in RTL Design and Synthesis using open-source EDA tools.
The hands-on lab sessions helped in understanding how Verilog RTL code is simulated, verified, synthesized, and converted into gate-level hardware implementation using SKY130 standard cell libraries.

---

Conclusion

This workshop provided practical exposure to RTL design and synthesis using open-source EDA tools.
The hands-on labs helped in understanding the complete flow from Verilog RTL coding to gate-level implementation using SKY130 standard cell libraries.
