# 📘 Day 4 – Gate Level Simulation (GLS) & Blocking Assignment Caveats

<p align="center">
  <img src="https://img.shields.io/badge/Workshop-SKY130%20RTL%20Design-blue?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Tool-Yosys-green?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Simulation-GTKWave-orange?style=for-the-badge"/>
</p>

---

# 📌 Overview

Day 4 focused on understanding:

* Gate-Level Simulation (GLS)
* Synthesis vs Simulation mismatch
* Blocking assignment caveats
* RTL debugging using GTKWave and Yosys

The experiments demonstrated how improper RTL coding styles can produce mismatches between RTL simulation and synthesized hardware behavior.

---

# 📚 Topics Covered

* Gate-Level Simulation (GLS)
* Synthesis-Simulation Mismatch
* Blocking Assignments
* RTL Coding Issues
* GTKWave Waveform Analysis
* Yosys Synthesis

---

# 🔹 What is Gate-Level Simulation (GLS)?

Gate-Level Simulation is performed after synthesis to verify whether the synthesized hardware behaves correctly.

Unlike RTL simulation, GLS represents the actual gate-level netlist generated after synthesis using SKY130 standard cells.

---

# 🎯 Importance of GLS

* Verifies synthesized hardware functionality
* Detects RTL coding mistakes
* Identifies synthesis-simulation mismatch
* Confirms actual hardware behavior
* Helps debug RTL issues

---

# 🔹 Synthesis vs Simulation Mismatch

A mismatch occurs when:

* RTL simulation behaves correctly
  BUT
* Synthesized hardware behaves differently

This usually happens because of incorrect RTL coding styles.

---

# 📌 Common Reasons for Mismatch

* Incorrect sensitivity list
* Improper blocking assignments
* Incomplete combinational logic
* Order-dependent statements
* Unsupported RTL coding styles

---

# 🧪 Lab 1 – GLS of Bad MUX

# 📖 Logic Explanation

The bad MUX intentionally contains RTL coding mistakes.

Example:

```verilog
always @(sel)
```

This sensitivity list is incomplete because the output also depends on:

* `i0`
* `i1`

But the block reacts only when `sel` changes.

---

# ❌ Why Mismatch Happens?

Suppose:

| sel | i0 | i1 | Expected y |
| --- | -- | -- | ---------- |
| 0   | 0  | 1  | 0          |

Now only `i0` changes:

| sel | i0 | i1 | Expected y |
| --- | -- | -- | ---------- |
| 0   | 1  | 1  | 1          |

Since `sel` did not change:

* `always @(sel)` does not execute again
* output `y` keeps old value

Therefore RTL simulation becomes incorrect.

However, synthesized hardware continuously responds to all input changes.

This creates a mismatch between:

* RTL Simulation
* Gate-Level Simulation

---

# ✅ Correct RTL Coding Style

```verilog
always @(*)
```

OR

```verilog
always @(i0 or i1 or sel)
```

Now the output updates whenever any input changes.

---

# 🧪 RTL Simulation

## Step 1 – Compile

```bash
iverilog bad_mux.v tb_bad_mux.v
```

---

## Step 2 – Run Simulation

```bash
./a.out
```

---

## Step 3 – Open GTKWave

```bash
gtkwave tb_bad_mux.vcd
```

---

# 📸 GTKWave Output – mismatch waveform

<img width="1280" height="400" alt="WhatsApp Image 2026-05-22 at 11 21 46 PM" src="https://github.com/user-attachments/assets/3d47ffc9-979e-48af-8dc1-156faf46c51e" />

---

# 📸 GTKWave Output – correct waveform

<img width="1280" height="400" alt="WhatsApp Image 2026-05-22 at 11 22 49 PM" src="https://github.com/user-attachments/assets/3de55f13-98bc-404d-b1bd-92a4964d2493" />

---

# 📖 Observation

* RTL simulation produced inconsistent outputs.
* GLS exposed the actual synthesized hardware behavior.
* Incorrect sensitivity list caused mismatch conditions.

---

# 🧪 Lab 2 – Blocking Assignment Caveat

# 📖 Logic Explanation

This experiment demonstrates how blocking assignments can create incorrect combinational behavior when assignment ordering is improper.

Example:

```verilog
always @(*) begin
   d = x & c;
   x = a | b;
end
```

Here:

1. `d` is evaluated first
2. `x` still contains OLD value
3. Only after that, `x` gets updated

As a result:

* `d` uses stale data
* Incorrect output is generated
* RTL simulation may not represent intended hardware behavior

---

# ❌ Why Incorrect Output Happens?

Suppose:

| a | b | c |
| - | - | - |
| 1 | 0 | 1 |

Expected calculation:

```text
x = 1 | 0 = 1
d = 1 & 1 = 1
```

But because of incorrect ordering:

```verilog
d = x & c;
x = a | b;
```

`d` gets calculated BEFORE updating `x`.

Therefore:

* `d` uses previous value of `x`
* incorrect output is generated
* mismatch behavior is observed

---

# ✅ Correct Coding Style

```verilog
always @(*) begin
   x = a | b;
   d = x & c;
end
```

Now:

1. `x` updates first
2. `d` uses updated value
3. Correct combinational behavior is obtained

---

# 🧪 RTL Simulation

## Step 1 – Compile

```bash
iverilog blocking_caveat.v tb_blocking_caveat.v
```

---

## Step 2 – Run Simulation

```bash
./a.out
```

---

## Step 3 – Open GTKWave

```bash
gtkwave tb_blocking_caveat.vcd
```

---

# 📸 GTKWave Output – Correct Behavior

<img width="1600" height="874" alt="WhatsApp Image 2026-05-22 at 11 41 35 PM" src="https://github.com/user-attachments/assets/f8080243-5ce1-4397-b737-e207a39599d8" />

---

# 📸 GTKWave Output – Mismatch Behavior

<img width="1600" height="874" alt="WhatsApp Image 2026-05-22 at 11 41 09 PM" src="https://github.com/user-attachments/assets/63710b0c-beed-49ba-a1df-31a938732ed4" />

---

# 📖 Observation

* Assignment ordering directly affected output behavior.
* Blocking assignments execute sequentially from top to bottom.
* Incorrect ordering caused mismatch conditions.
* Proper coding style is important for combinational logic design.

---

# 🧪 Yosys Synthesis – Blocking Caveat

## Step 1 – Launch Yosys

```bash
yosys
```

---

## Step 2 – Read Liberty File

```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

---

## Step 3 – Read Verilog File

```bash
read_verilog blocking_caveat.v
```

---

## Step 4 – Synthesize

```bash
synth -top blocking_caveat
```

---

## Step 5 – Technology Mapping

```bash
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

---

## Step 6 – Generate Schematic

```bash
show
```

---

# 📸 Yosys Synthesized Output

<img width="1600" height="874" alt="WhatsApp Image 2026-05-22 at 11 41 35 PM" src="https://github.com/user-attachments/assets/63f4b493-1bb1-4446-9369-804c0c570012" />

---

# ⚙️ Tools Used

| Tool           | Purpose                      |
| -------------- | ---------------------------- |
| Icarus Verilog | RTL Compilation & Simulation |
| GTKWave        | Waveform Analysis            |
| Yosys          | RTL Synthesis                |
| SKY130 Library | Standard Cell Mapping        |

---

# 🎯 Learning Outcome

By the end of Day 4, I understood:

* Importance of Gate-Level Simulation
* Causes of synthesis-simulation mismatch
* Correct usage of blocking assignments
* Importance of proper RTL coding style
* Debugging RTL behavior using GTKWave and Yosys
* Difference between RTL simulation and synthesized hardware behavior

---

# ✅ Conclusion

Day 4 provided practical understanding of how RTL coding style directly affects synthesized hardware behavior.

Through GLS experiments and blocking assignment analysis, I learned the importance of writing clean, synthesizable, and hardware-accurate Verilog RTL code.

---

