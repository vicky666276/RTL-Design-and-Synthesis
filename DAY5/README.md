# 📘 Day 5 – If/Case Hazards, For Loops and Generate Blocks

<p align="center">
  <img src="https://img.shields.io/badge/Workshop-SKY130%20RTL%20Design-blue?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Topic-RTL%20Coding-orange?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Tool-Yosys-green?style=for-the-badge"/>
  <img src="https://img.shields.io/badge/Simulation-GTKWave-red?style=for-the-badge"/>
</p>

---

# 📌 Overview

Day 5 focused on RTL coding styles and how improper Verilog coding can create hardware issues such as:

* Inferred Latches
* Synthesis-Simulation Mismatch
* Ambiguous Case Statements
* Latch-like Behaviour

The session also explored:

* For loops
* Generate blocks
* Ripple Carry Adders
* Scalable RTL design techniques

The experiments were verified using:

* Icarus Verilog
* GTKWave
* Yosys synthesis
* SKY130 standard cells

---

# 📚 Theory Topics Covered

* Inferred Latches
* Incomplete If Statements
* Incomplete Case Statements
* Bad Case Statements
* Synthesis-Simulation Mismatch
* For Loops in RTL
* Generate Blocks
* Ripple Carry Adders

---

# 🔹 Inferred Latches

A latch is inferred when all output conditions are not defined inside a combinational `always` block.

When the synthesis tool cannot determine what the output should be for certain input conditions, it stores the previous value.

This creates latch-like behaviour unintentionally.

---

# ⚠️ Problems Caused by Inferred Latches

* Timing issues
* Unpredictable behaviour
* Synthesis mismatch
* Difficult timing closure
* Unwanted memory elements

---

# 🔹 Example – Incomplete If Statement

## 📖 Logic Explanation

If an `if` statement does not contain an `else` condition, then for some input conditions the output value becomes undefined.

To preserve the previous output value, the synthesis tool inserts a latch automatically.

---

## 🧠 Behaviour

Example concept:

```verilog
if(i0)
   y = i1;
```

### What happens?

| Condition | Output             |
| --------- | ------------------ |
| `i0 = 1`  | `y = i1`           |
| `i0 = 0`  | Output not defined |

Since output is undefined when `i0 = 0`, the tool stores the previous value of `y`.

Thus:

* `y` behaves like memory
* latch gets inferred

---

# 📸 RTL Simulation Waveform

<img width="1600" height="874" alt="WhatsApp Image 2026-05-23 at 10 06 13 AM" src="https://github.com/user-attachments/assets/3ec44a5b-1c88-4ee3-a425-3d523320a2eb" />

---

# 📸 Yosys Synthesized Output

<img width="957" height="708" alt="WhatsApp Image 2026-05-23 at 10 06 45 AM" src="https://github.com/user-attachments/assets/9a79b45e-1ed3-40c5-8475-9b6d9089f401" />

---

# 📖 Observation

From the waveform:

* whenever `i0` becomes LOW,
* output `y` holds its previous value.

This confirms latch behaviour.

The synthesized schematic also confirms that Yosys inferred a D-Latch.

---

# 🔹 Complete Case Statement

## 📖 Logic Explanation

A complete case statement defines output behaviour for all possible conditions.

Example concept:

```verilog
case(sel)
  2'b00 : y = i0;
  2'b01 : y = i1;
  default : y = i2;
endcase
```

Since every condition is covered:

* no undefined outputs exist
* no latch is inferred

---

# 📸 GTKWave Output

<img width="1275" height="365" alt="WhatsApp Image 2026-05-23 at 10 16 07 AM" src="https://github.com/user-attachments/assets/faa2be26-b33d-4c27-ac09-77e138fe1ee6" />

---

# 📸 Yosys Synthesized Output


<img width="1264" height="519" alt="WhatsApp Image 2026-05-23 at 10 16 11 AM" src="https://github.com/user-attachments/assets/c19542a1-b710-40fe-9208-efb1be22f173" />

---

# 📖 Observation

The waveform behaves exactly like a multiplexer:

* output changes immediately based on `sel`
* no output holding occurs
* no latch inference is present

The synthesized schematic also confirms pure combinational MUX behaviour.

---

# 🔹 Incomplete Case Statement

## 📖 Logic Explanation

If all possible values of `sel` are not covered, then output becomes undefined for the missing cases.

Example:

```verilog
case(sel)
  2'b00 : y = i0;
  2'b01 : y = i1;
endcase
```

Missing conditions:

* `10`
* `11`

For these values:

* output retains previous value
* latch gets inferred

---

# 📸 GTKWave Output

<img width="1280" height="375" alt="WhatsApp Image 2026-05-23 at 11 24 40 AM" src="https://github.com/user-attachments/assets/5cf475ff-6884-4530-87fc-b91ec24de250" />

---

# 📸 Yosys Synthesized Output

<img width="1269" height="226" alt="WhatsApp Image 2026-05-23 at 10 23 11 AM" src="https://github.com/user-attachments/assets/2c7ad02d-1dfb-4946-a490-c4595fc2bcd3" />

---

# 📖 Observation

The waveform shows:

* output freezes for uncovered case conditions
* previous output value is retained

This confirms latch inference.

The synthesized hardware also contains latch structures.

---

# 🧠 Golden Rule in Combinational Logic

In combinational `always` blocks:

✅ Always define outputs for ALL possible conditions.

This can be done using:

* final `else`
* `default` in case statements

Otherwise:

* latches may be inferred unintentionally.

---

# 🔹 For Loops in RTL

For loops help reduce repetitive RTL code and improve scalability.

They are commonly used in:

* Multiplexers
* Demultiplexers
* Bitwise operations
* Large bus structures

---

# 🔹 MUX Using For Loop

## 📖 Logic Explanation

A `for` loop can repeatedly check multiple input conditions and generate compact RTL code.

Instead of writing:

```verilog
if(sel==0)
if(sel==1)
if(sel==2)
```

the loop automates the process.

This makes RTL:

* cleaner
* scalable
* easier to maintain

---

# 📸 GTKWave Output

<img width="1270" height="387" alt="WhatsApp Image 2026-05-23 at 11 29 04 AM" src="https://github.com/user-attachments/assets/c6e706cf-f218-4d5d-920c-4a28df6eec74" />

---

# 📖 Observation

The waveform shows:

* output changes according to select signal
* proper MUX functionality is observed

---

# 🔹 Demultiplexer Using Case Statement

## 📖 Logic Explanation

A demultiplexer routes one input signal to multiple outputs based on the select line.

Example:

* input `i`
* outputs `o0` to `o7`

Depending on `sel`, only one output becomes active.

---

# 📸 GTKWave Output

<img width="1277" height="467" alt="WhatsApp Image 2026-05-23 at 11 30 39 AM" src="https://github.com/user-attachments/assets/345e008b-24f7-467e-9f31-2b22cd613c2b" />

---

# 📖 Observation

As `sel` changes:

* output shifts from one line to another
* demultiplexer operation is verified

---

# 🔹 Ripple Carry Adder Using Generate Block

## 📖 Logic Explanation

A Ripple Carry Adder connects multiple full adders in series.

Each stage:

* adds two bits
* propagates carry to next stage

Generate blocks instantiate multiple full adders automatically.

---

# 🧠 Key Difference – Generate vs For Loop

| Generate Block                 | For Loop                         |
| ------------------------------ | -------------------------------- |
| Creates hardware copies        | Executes during simulation       |
| Used during elaboration        | Used inside procedural blocks    |
| Structural hardware generation | Behavioural logic simplification |

---

# 🧪 Simulation Flow

## Step 1 – Compile RTL

```bash
iverilog design.v tb.v
```

---

## Step 2 – Run Simulation

```bash
./a.out
```

---

## Step 3 – Open GTKWave

```bash
gtkwave tb.vcd
```
# 📸 GTKWave Output

<img width="1280" height="465" alt="WhatsApp Image 2026-05-23 at 11 35 04 AM" src="https://github.com/user-attachments/assets/d4836ebd-b208-4ba7-915e-955a6f6245b5" />

---

# 🔧 Yosys Synthesis Flow

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog design.v
synth -top <top_module>
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

---

# ⚙️ Tools Used

| Tool           | Purpose               |
| -------------- | --------------------- |
| Icarus Verilog | RTL Compilation       |
| GTKWave        | Waveform Analysis     |
| Yosys          | RTL Synthesis         |
| SKY130 Library | Standard Cell Mapping |

---

# 🎯 Learning Outcome

By the end of Day 5, I learned:

* How inferred latches occur
* Importance of complete if/case statements
* Causes of synthesis-simulation mismatch
* Proper combinational RTL coding practices
* Usage of for loops in scalable RTL
* Structural design using generate blocks
* Ripple Carry Adder implementation

---

# ✅ Conclusion

Day 5 provided practical understanding of RTL coding hazards and scalable hardware design techniques.

The experiments demonstrated:

* how incomplete coding styles infer latches,
* how mismatches occur,
* and how proper RTL practices help create reliable synthesizable hardware.

The session also introduced efficient RTL design using:

* for loops
* generate blocks
* scalable arithmetic structures.

---

