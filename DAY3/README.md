# 📘 Day 3 – Combinational & Sequential Optimization

<p align="center">
  <img src="https://img.shields.io/badge/Workshop-SKY130%20RTL%20Design-blue?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Tool-Yosys-green?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Simulation-GTKWave-orange?style=for-the-badge"/>
</p>

---

# 📌 Overview

Day 3 focused on optimization techniques used in digital VLSI design.
The session explored how synthesis tools simplify logic, remove redundant hardware, and optimize sequential as well as combinational circuits.

The optimization flow was analyzed using:

* Verilog RTL coding
* GTKWave simulation
* Yosys synthesis
* SKY130 standard cell mapping

---

# 📚 Topics Covered

* Constant Propagation
* State Optimization
* Cloning
* Retiming
* Sequential Optimization
* Unused Output Optimization
* Optimization of Flip-Flop Based Circuits
* Yosys Optimization Flow

---

# 🔹 Constant Propagation

Constant propagation is an optimization technique where fixed logic values (`0` or `1`) are directly propagated through the circuit during synthesis.

### Example

```verilog id="3k7v2r"
assign y = a ? 1'b0 : b;
```

### Explanation

* If `a = 1`, output `y` always becomes `0`
* If `a = 0`, output `y` becomes `b`

During synthesis, Yosys identifies constant values and removes unnecessary logic gates automatically.

---

# 🎯 Benefits

* Reduced hardware complexity
* Lower area usage
* Reduced power consumption
* Simplified gate-level implementation

---

# 🔹 State Optimization

State optimization reduces redundant states in sequential circuits and FSMs.

### Example

If two states perform identical operations, synthesis tools merge them into a single state.

### Result

* Fewer flip-flops required
* Reduced combinational logic
* Improved timing performance

---

# 🔹 Cloning

Cloning duplicates logic cells to improve timing and reduce signal loading.

### Example

Instead of:

```text id="3kcl8z"
One Gate → 20 Outputs
```

The synthesis tool may create:

```text id="h2p7wv"
Gate 1 → 10 Outputs
Gate 2 → 10 Outputs
```

### Result

* Reduced fanout delay
* Improved timing
* Better signal distribution

---

# 🔹 Retiming

Retiming repositions flip-flops across combinational logic while preserving circuit functionality.

### Example

Before Retiming:

```text id="qg9i0o"
FF → Large Combinational Logic → FF
```

After Retiming:

```text id="x4j13m"
FF → Small Logic → FF → Small Logic
```

### Result

* Reduced critical path delay
* Improved clock frequency
* Better timing optimization

---

# 🧪 Optimization Labs

The optimization behavior was verified using:

* RTL Simulation
* GTKWave Waveform Analysis
* Yosys RTL Synthesis
* Gate-Level Schematic Visualization

The main focus was on:

* `dff_const4`
* `dff_const5`
* `counter_opt`

---

# 🔧 Lab 1 – dff_const4

# 📖 Logic Explanation

This design contains D Flip-Flops with asynchronous reset.

The registers are continuously assigned constant logic values. During synthesis, Yosys identifies constant behavior and removes redundant logic automatically.

This demonstrates sequential optimization using constant propagation.

---

# 🧪 RTL Simulation Flow

## Step 1 – Compile the RTL Design

```bash id="y5i0j8"
iverilog dff_const4.v tb_dff_const4.v
```

---

## Step 2 – Run the Simulation

```bash id="qtjlwm"
./a.out
```

---

## Step 3 – Open GTKWave

```bash id="bjp8jp"
gtkwave tb_dff_const4.vcd
```

---

# 📸 RTL Code

<img width="1600" height="360" alt="WhatsApp Image 2026-05-22 at 10 53 48 PM" src="https://github.com/user-attachments/assets/ee57932f-3d0d-4ac1-8a51-480570981f76" />

---

# 📸 GTKWave Output

<img width="1600" height="874" alt="WhatsApp Image 2026-05-22 at 10 54 02 PM" src="https://github.com/user-attachments/assets/a27eb32b-88fd-47ec-9679-eaa6796c7301" />

---

# 🔧 RTL Synthesis using Yosys

## Step 1 – Launch Yosys

```bash id="5g35lk"
yosys
```

---

## Step 2 – Read Liberty File

```bash id="b2kwmv"
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

---

## Step 3 – Read Verilog Design

```bash id="r85a6y"
read_verilog dff_const4.v
```

---

## Step 4 – Synthesize the Design

```bash id="e2l8rx"
synth -top dff_const4
```

---

## Step 5 – Technology Mapping

```bash id="lg0mq7"
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

---

## Step 6 – Generate Gate-Level Schematic

```bash id="jlwmk3"
show
```

---

# 📸 Yosys Synthesized Output

<img width="1600" height="874" alt="WhatsApp Image 2026-05-22 at 10 54 09 PM" src="https://github.com/user-attachments/assets/e5971028-a85f-487d-a3d7-5b089c5ca4df" />

---

# 📖 Observation

* Constant propagation optimization was observed.
* Redundant sequential elements were minimized.
* The synthesized circuit became simpler and more efficient.

---

# 🔧 Lab 2 – dff_const5

# 📖 Logic Explanation

The design continuously drives the output with constant logic `1`.

Since the outputs do not change dynamically, synthesis optimization heavily simplifies the circuit.

Unnecessary flip-flops and combinational paths are removed automatically during synthesis.

---

# 🧪 RTL Simulation Flow

## Step 1 – Compile the RTL Design

```bash id="bwd6of"
iverilog dff_const5.v tb_dff_const5.v
```

---

## Step 2 – Run Simulation

```bash id="ehsf1l"
./a.out
```

---

## Step 3 – Open GTKWave

```bash id="im9sl4"
gtkwave tb_dff_const5.vcd
```

---

# 📸 RTL Code

<img width="1600" height="529" alt="WhatsApp Image 2026-05-22 at 10 53 48 PM" src="https://github.com/user-attachments/assets/ce49453b-ddc1-4a73-a38c-cc2ad971e502" />


---

# 📸 GTKWave Output

<img width="1600" height="874" alt="WhatsApp Image 2026-05-22 at 11 07 16 PM" src="https://github.com/user-attachments/assets/684b8e16-d380-46bb-b516-fa7bb7204a2b" />

---

# 🔧 RTL Synthesis using Yosys

```bash id="r9bxz2"
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const5.v
synth -top dff_const5
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

---

# 📸 Yosys Synthesized Output

<img width="1600" height="874" alt="WhatsApp Image 2026-05-22 at 11 07 25 PM" src="https://github.com/user-attachments/assets/3e7fec5c-87fd-486c-bee8-eb847a6b2d18" />

---

# 📖 Observation

* Yosys optimized constant assignments automatically.
* Redundant flip-flops were removed.
* Sequential optimization reduced hardware complexity significantly.

---

# 🧪 Lab 3 – Unused Output Optimization (`counter_opt`)

# 📖 Logic Explanation

This experiment demonstrates how synthesis tools optimize and remove unused outputs or unnecessary logic from a design.

In many RTL designs:

* some outputs may never be used
* certain internal signals may not affect final functionality

During synthesis, Yosys identifies such redundant logic and removes it automatically to optimize:

* Area
* Power
* Hardware complexity

---

# 🔹 What is Unused Output Optimization?

Unused output optimization is a synthesis technique where:

* logic driving unused outputs
* redundant internal signals
* unnecessary combinational paths

are removed during optimization.

---

# 📌 Example Scenario

Suppose a counter module generates:

* `count[3:0]`
* `temp_signal`

But only `count[1:0]` is actually used.

Then:

* unused logic gets optimized away
* unnecessary hardware is removed automatically

---

# 🧪 RTL Synthesis using Yosys

## Step 1 – Launch Yosys

```bash id="7pdw1z"
yosys
```

---

## Step 2 – Read SKY130 Liberty File

```bash id="i7n2jz"
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

---

## Step 3 – Read Verilog Design

```bash id="w0x71m"
read_verilog counter_opt.v
```

---

## Step 4 – Synthesize the Design

```bash id="gh3ghm"
synth -top counter_opt
```

---

## Step 5 – Technology Mapping

```bash id="fc7qxt"
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

---

## Step 6 – Generate Schematic

```bash id="6ymhh9"
show
```

---

# 📸 Yosys Synthesized Output

<img width="1600" height="874" alt="WhatsApp Image 2026-05-23 at 9 53 20 AM" src="https://github.com/user-attachments/assets/0a4eae41-a374-4bb9-85d5-480976d4f0bf" />

---

# 📖 Observation

* Unused logic was removed automatically during synthesis.
* Redundant outputs and unnecessary hardware were optimized away.
* The synthesized netlist became smaller and more efficient.

---

# ⚙️ Tools Used

| Tool           | Purpose                      |
| -------------- | ---------------------------- |
| Icarus Verilog | RTL Compilation & Simulation |
| GTKWave        | Waveform Analysis            |
| Yosys          | RTL Synthesis & Optimization |
| SKY130 Library | Standard Cell Mapping        |

---

# 🎯 Learning Outcome

By the end of Day 3, I learned:

* Combinational optimization techniques
* Sequential optimization concepts
* Constant propagation in RTL synthesis
* Retiming and cloning basics
* Unused output optimization
* Gate-level optimization using Yosys
* Visualization of optimized hardware using synthesized schematics

---

# ✅ Conclusion

Day 3 provided practical understanding of optimization techniques used in RTL synthesis and digital VLSI design.

The lab sessions demonstrated how synthesis tools optimize RTL circuits by removing redundant logic and simplifying gate-level hardware using SKY130 standard cell libraries.

---
