# Binary Adder/Subtractor Circuit — Hardware Implementation

**Category:** Intermediate | **Hardware:** 74xx TTL ICs on breadboard  
**ICs Used:** 7483 (4-bit full adder), 7486 (XOR), 7404 (NOT), DIP switches, LEDs

---

## Objective

Design and implement a complete binary arithmetic unit capable of performing both addition and subtraction on multi-bit operands, progressing from a half adder to a full adder to a cascaded ripple-carry adder/subtractor.

---

## Approach

Arithmetic circuits were derived from first principles using K-map minimisation, then physically implemented on breadboard. Subtraction was performed via 2's complement addition controlled by a single mode-select switch, demonstrating how real CPUs unify ADD and SUB into a single hardware path.

---

## Design / Implementation

### Half Adder
Derives Sum and Carry from two 1-bit inputs.

| Function | Expression |
|----------|-----------|
| Sum | `S = A ⊕ B` |
| Carry | `C = A · B` |

Implemented using IC 7486 (XOR) and IC 7408 (AND).

### Full Adder
Extends the half adder with a carry-in input `Cin`. Two full adders can be cascaded.

| Function | Expression |
|----------|-----------|
| Sum | `S = A ⊕ B ⊕ Cin` |
| Carry Out | `Cout = Cin(A⊕B) + AB` |

### Ripple Carry Adder
Three full adders cascaded to add two 3-bit numbers. Carry-out of each stage feeds into the carry-in of the next — the propagation pattern that gives the design its name.

### 4-bit Adder/Subtractor (IC 7483)
The IC 7483 contains four cascaded full adders. Combined with IC 7486 (XOR gates), subtraction is implemented using 2's complement:

- **Mode switch = 0 (Addition):** XOR gates pass B bits unchanged; 7483 computes A + B
- **Mode switch = 1 (Subtraction):** XOR gates invert all B bits; mode switch feeds carry-in = 1; 7483 computes A + B' + 1 = A − B

An 8-bit version was constructed by cascading two 7483 ICs, with carry-out of the first feeding carry-in of the second.

---

## Verified Results (4-bit operation)

**Addition:** `0001 + 0011 = 0100` — S2 LED lit, carry-out = 0 ✓  
**Subtraction:** Verified via 2's complement with mode switch toggled to HIGH

---

## Why This Matters

This is the exact architecture inside every ALU. Modern processors implement the same concept — XOR-controlled inversion for 2's complement subtraction — at transistor scale. The ripple carry adder is the conceptual precursor to carry-lookahead and parallel prefix adders used in high-speed hardware. Building it on breadboard makes the delay chain physically observable.

---

## Key Learnings

- Half and full adder logic is directly derivable from K-map minimisation of truth tables
- Ripple carry architecture introduces carry propagation delay — the fundamental latency problem that carry-lookahead adders solve
- A single XOR gate per bit, controlled by one mode line, is sufficient to switch between ADD and SUB — this is how real ALUs operate
- IC 7483 cascading extends the adder to arbitrary bit widths; two 7483s produce an 8-bit unit

---

## Hardware Evidence

Breadboard photos showing half adder, full adder, and 4-bit adder/subtractor configurations are in `assets/`. Experimental truth tables from all three circuits match theoretical values.

---

## Reference

📎 Full Lab Report: `lab_report.pdf`
