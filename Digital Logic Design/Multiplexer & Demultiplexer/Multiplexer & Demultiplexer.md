# Multiplexer & Demultiplexer — Data Routing Circuits

**Category:** Advanced | **Hardware:** 74xx TTL ICs on breadboard  
**ICs Used:** 7411 (3-input AND), 7432 (OR), 7404 (NOT — inverter)

---

## Objective

Design and implement a 4×1 multiplexer and a 1×4 demultiplexer from discrete gate ICs, verify their function tables on hardware, and extend the design to an 8×1 MUX using two 74153 ICs and a 74157 selector.

---

## Approach

Both circuits were constructed from first principles using AND, OR, and NOT gates without using dedicated MUX/DEMUX ICs. This required deriving the sum-of-products expression for each output and mapping it directly to gate interconnections on breadboard. A second design task extended the 4×1 MUX to 8×1 using a hierarchical architecture.

---

## Design / Implementation

### 4×1 Multiplexer

A MUX routes one of 2ⁿ inputs to a single output based on n select lines.

**Output expression:**  
`Y = S1'S0'·I0 + S1'S0·I1 + S1S0'·I2 + S1S0·I3`

Each product term is implemented with a 3-input AND gate (IC 7411). The four AND outputs are OR'd together via IC 7432. Select lines S1, S0 are inverted using IC 7404 to generate complemented terms.

| S1 | S0 | Output Y |
|----|----|---------|
|  0 |  0 |   I0    |
|  0 |  1 |   I1    |
|  1 |  0 |   I2    |
|  1 |  1 |   I3    |

### 1×4 Demultiplexer

A DEMUX routes a single input to one of 2ⁿ outputs based on select lines — structurally equivalent to a decoder with the data line as enable.

| S0 | S1 | I | Y0 | Y1 | Y2 | Y3 |
|----|----|---|----|----|----|----|
|  0 |  0 | 1 |  1 |  0 |  0 |  0 |
|  0 |  1 | 1 |  0 |  1 |  0 |  0 |
|  1 |  0 | 1 |  0 |  0 |  1 |  0 |
|  1 |  1 | 1 |  0 |  0 |  0 |  1 |

### 8×1 MUX — Hierarchical Design (74153 + 74157)

To extend to 8 inputs without redesigning from scratch:
- Two 74153 (dual 4×1 MUX) ICs handle the lower and upper 4 inputs respectively using select lines S0, S1
- Their outputs feed a 74157 (dual 2×1 MUX), which uses select line S2 to choose between the two groups
- This tree structure scales to any input width by adding levels

---

## Why This Matters

Multiplexers are foundational to digital system interconnects. Inside an FPGA, every logic cell output passes through a MUX tree to reach routing fabric — FPGA routing is essentially a massive configurable MUX network. In processor datapaths, MUXes select between ALU operand sources (register, immediate, memory). The hierarchical 8×1 MUX design demonstrates the scaling principle used in all practical digital systems.

---

## Key Learnings

- A MUX implements any Boolean function — by setting data inputs to 0 or 1, any truth table can be realised with one MUX, making them functionally complete
- A DEMUX is structurally identical to a decoder; the only conceptual difference is whether the enable pin carries a data signal or simply enables/disables the circuit
- Cascading two 4×1 MUXes with a 2×1 output selector is more gate-efficient than constructing an 8×1 from scratch using discrete AND-OR logic
- Interrupt controllers in CPUs use priority MUX logic to select the highest-priority interrupt request from multiple simultaneous sources

---

## Hardware Evidence

Breadboard circuit photos for both the 4×1 MUX and the 1×4 DEMUX are in `assets/`. Experimental function table results match expected behaviour for all select line combinations.

---

## Reference

📎 Full Lab Report: `lab_report.pdf`
