# Day 2 – Timing Libraries, Synthesis Approaches & Efficient Flip-Flop Coding
<p align="center">
  <img src="https://img.shields.io/badge/Workshop-SKY130%20RTL%20Design-blue?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Tool-Yosys-green?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Simulation-GTKWave-orange?style=for-the-badge"/>
</p>

---

# 📘 Overview

Day 2 of the RTL Design and Synthesis Workshop focused on understanding timing libraries, synthesis methodologies, and efficient sequential circuit coding styles using the SKY130 standard cell library.

The session mainly covered:

* SKY130 Timing Libraries
* Liberty (`.lib`) Files
* Hierarchical vs Flattened Synthesis
* Flip-Flop Coding Styles
* RTL Simulation using Icarus Verilog
* Waveform Analysis using GTKWave
* Synthesis using Yosys
* Technology Mapping using SKY130 Standard Cells

---

# 🛠️ Tools Used

| Tool       | Purpose               |
| ---------- | --------------------- |
| iverilog   | RTL Simulation        |
| GTKWave    | Waveform Viewer       |
| Yosys      | RTL Synthesis         |
| SKY130 PDK | Standard Cell Library |

---

# ⏱️ Timing Libraries

## SKY130 PDK Overview

The SKY130 PDK is an open-source Process Design Kit based on SkyWater’s 130nm CMOS technology.

It contains:

* Standard cell libraries
* Timing information
* Power models
* Sequential and combinational logic cells
* Process variation data

These libraries are essential for synthesis and technology mapping.

---

# 📚 Understanding the Liberty File

The standard library file used during synthesis:

```bash
sky130_fd_sc_hd__tt_025C_1v80.lib
```

## File Name Meaning

| Term | Meaning                |
| ---- | ---------------------- |
| tt   | Typical Process Corner |
| 025C | Temperature = 25°C     |
| 1v80 | Supply Voltage = 1.8V  |

This naming convention defines the operating conditions used during synthesis and timing analysis.

---

# 🔍 Opening the Liberty File

## Install gedit

```bash
sudo apt install gedit
```

## Open the `.lib` File

```bash
gedit sky130_fd_sc_hd__tt_025C_1v80.lib
```

The liberty file contains:

* Timing Information
* Power Data
* Cell Delays
* Pin Information
* Setup/Hold Timing Constraints

---

# 🏗️ Hierarchical vs Flattened Synthesis

## Hierarchical Synthesis

Hierarchical synthesis preserves the original RTL module structure during synthesis.

### Advantages

* Easier debugging
* Better modularity
* Faster synthesis for large designs

### Disadvantages

* Limited cross-module optimization

---

## Flattened Synthesis

Flattened synthesis removes hierarchy and combines all modules into a single netlist.

### Advantages

* Better optimization
* Improved timing performance

### Disadvantages

* Harder debugging
* Increased netlist complexity

---

# 🔄 Flip-Flop Coding Styles

Sequential circuits use flip-flops to store binary data.

Different reset/set styles can be implemented based on design requirements.

---

# ⚡ Asynchronous Reset D Flip-Flop

## Verilog Code

```verilog
module dff_asyncres(input clk,input async_reset,input d,output reg q);

always @(posedge clk,posedge async_reset)
begin
    if(async_reset)
        q <= 1'b0;
    else
        q <= d;
end

endmodule
```

---

# 📖 Working Principle

* When `async_reset = 1`, output `q` becomes `0` immediately.
* Reset operation does not wait for the clock edge.
* When reset is inactive, the flip-flop captures input `d` on every positive clock edge.

---

# 🧪 RTL Simulation using Icarus Verilog

## Step 1 – Compile the Design

```bash
iverilog dff_asyncres.v tb_dff_asyncres.v
```

---

## Step 2 – Run the Simulation

```bash
./a.out
```

---

## Step 3 – Open GTKWave

```bash
gtkwave tb_dff_asyncres.vcd
```

---

# 📸 GTKWave Output

<img width="1600" height="874" alt="WhatsApp Image 2026-05-22 at 10 36 20 PM" src="https://github.com/user-attachments/assets/e736895f-fe96-4e7e-b9f7-214d3106babb" />


---

# 🔧 RTL Synthesis using Yosys

## Step 1 – Launch Yosys

```bash
yosys
```

---

## Step 2 – Read SKY130 Liberty File

```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

---

## Step 3 – Read Verilog Design

```bash
read_verilog dff_asyncres.v
```

---

## Step 4 – Synthesize the Design

```bash
synth -top dff_asyncres
```

---

## Step 5 – Map Flip-Flops

```bash
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

This command maps RTL flip-flops to actual SKY130 standard cells.

---

## Step 6 – Technology Mapping

```bash
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

Technology mapping converts generic RTL logic into SKY130 gate-level cells.

---

## Step 7 – Generate Gate-Level Schematic

```bash
show
```

---

# 📸 Yosys Synthesized Output

> Add your Yosys schematic screenshot below.

<img width="1600" height="874" alt="WhatsApp Image 2026-05-22 at 9 34 17 PM" src="https://github.com/user-attachments/assets/d2ab692f-2434-4e94-ab18-d178c4f60cbf" />

# 📖 What I Learned

Through the Day 2 lab sessions, I learned:

* Importance of timing libraries in synthesis
* Understanding SKY130 liberty files
* Difference between hierarchical and flattened synthesis
* Sequential circuit coding styles
* Working of asynchronous reset D flip-flops
* RTL simulation using Icarus Verilog
* Waveform visualization using GTKWave
* Technology mapping using Yosys and SKY130 standard cells

---

# ✅ Conclusion

Day 2 provided a strong understanding of timing libraries, sequential circuit design, and synthesis methodologies.

The hands-on labs helped in understanding how RTL code is simulated, synthesized, and mapped into actual SKY130 standard cells using open-source EDA tools.

---
