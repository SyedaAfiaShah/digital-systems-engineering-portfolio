# Universal Gate Implementation using NAND and NOR

**Category:** Foundation | **Hardware:** 74xx TTL ICs on breadboard  
**ICs Used:** 7400 (NAND), 7402 (NOR), 7404 (NOT), 7408 (AND), 7432 (OR), 7486 (XOR)

---

## Objective

Demonstrate the functional completeness of NAND and NOR gates by realising every basic logic gate — NOT, AND, OR, XOR, XNOR — using only NAND (IC 7400) and separately only NOR (IC 7402). Then synthesise a custom 3-variable Boolean function using each universal gate type independently.

---

## Approach

A given Boolean function was derived from a truth table using K-map minimisation, then implemented twice on hardware — once using only NAND gates, once using only NOR gates. Output was verified against the expected truth table using LED indicators connected to DIP switch inputs.

**Derived function from truth table:**

Using minterms: `F = m1 + m3 + m6 + m7`  
Simplified: **`F = A'C + AB`**

---

## Design / Implementation

**Basic gate synthesis using NAND (IC 7400):**
- NOT: Tie both inputs of a NAND gate together → output = A'
- AND: NAND followed by an inverter (two NAND gates)
- OR: Invert both inputs, then NAND
- XOR and XNOR: Constructed using 4-NAND gate configurations

**Function synthesis — NAND implementation:**
- Gate 1: Invert A → A'
- Gate 2: NAND A' with C → `(A'·C)'`
- Gate 3: NAND A with B → `(A·B)'`
- Gate 4: NAND outputs of Gate 2 and Gate 3 → `(A'C + AB)` via double negation

**Function synthesis — NOR implementation:**
- Seven NOR gates used across two IC 7402 chips
- C inverted, then NOR'd with A to produce `(A+C')'`
- A and B each inverted, then combined to produce `(A'+B')'` = AB
- Final NOR gate combines both terms and inverts to give F

Both implementations produced identical experimental truth tables.

---

## Verified Truth Table

| A | B | C | F |
|---|---|---|---|
| 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 1 |
| 0 | 1 | 0 | 0 |
| 0 | 1 | 1 | 1 |
| 1 | 0 | 0 | 0 |
| 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 1 |
| 1 | 1 | 1 | 1 |

---

## Why This Matters

The concept of gate universality underpins how physical chips are manufactured. Standard cell libraries in ASIC design are NAND-based. Modern FPGAs implement all logic using configurable LUTs — which are fundamentally NAND-equivalent structures. Knowing how to synthesise any function from a single gate type is the foundation of practical digital design and logic synthesis.

---

## Key Learnings

- Any Boolean function can be physically realised using only NAND or only NOR gates — no other gate types are required
- NAND gates are preferred commercially because they are faster and require fewer transistors than AND-OR implementations
- K-map minimisation directly translates to fewer gates needed on hardware, reducing IC count and propagation delay
- The same function `F = A'C + AB` required 4 NAND gates vs 7 NOR gates — NAND synthesis was more efficient for this expression

---

## Hardware Evidence

Breadboard photos showing NOT, AND, XOR, XNOR, and NOR gate implementations are in `assets/`. Experimental results match expected truth tables for all 8 input combinations.

---

## Reference

📎 Full Lab Report: `lab_report.pdf`
