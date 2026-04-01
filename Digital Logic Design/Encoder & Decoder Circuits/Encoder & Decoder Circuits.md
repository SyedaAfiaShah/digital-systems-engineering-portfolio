# Encoder & Decoder Circuit Implementation

**Category:** Intermediate | **Hardware:** 74xx TTL ICs on breadboard  
**ICs Used:** 7400 (NAND — decoder), 7432 (OR — encoder), 7404 (NOT)

---

## Objective

Implement a 2-to-4 line decoder and an 8-to-3 line encoder on breadboard, verify their truth tables experimentally, and explore the relationship between decoders and combinational circuit synthesis.

---

## Approach

Both circuits were constructed from discrete gate ICs, not dedicated decoder/encoder chips, to demonstrate the underlying logic. The decoder was designed using NAND gates (active-low output convention), and the encoder was constructed using OR gates following the minterm-sum structure of the output expressions.

---

## Design / Implementation

### 2-to-4 Line Decoder

A decoder with 2 select inputs (A, B) and an active-low enable (E) produces 4 mutually exclusive outputs. With E=0 (enabled), exactly one output is asserted LOW for each input combination.

| E | A | B | D0 | D1 | D2 | D3 |
|---|---|---|----|----|----|----|
| 1 | x | x |  0 |  0 |  0 |  0 |
| 0 | 0 | 0 |  0 |  1 |  1 |  1 |
| 0 | 0 | 1 |  1 |  0 |  1 |  1 |
| 0 | 1 | 0 |  1 |  1 |  0 |  1 |
| 0 | 1 | 1 |  1 |  1 |  1 |  0 |

Implemented using NAND gates → active-low outputs. Three LEDs ON, one LED OFF indicates the selected output (NAND convention). Replacing NAND with AND would invert this — one LED ON, three OFF.

The enable input E allows decoder cascading: multiple decoders can be chained by routing address bits to their enable pins to build larger address spaces.

### 8-to-3 Line Priority Encoder

An encoder performs the inverse operation — maps 8 one-hot inputs to a 3-bit binary code. Output expressions are OR sums of the input lines that share each bit position:

```
A = Y4 + Y5 + Y6 + Y7
B = Y2 + Y3 + Y6 + Y7
C = Y1 + Y3 + Y5 + Y7
```

Each output bit is wired by connecting the appropriate input lines through cascaded IC 7432 (OR gate) stages. The IC 7432 chip was used in multi-stage OR configurations to handle the 4-input OR required per output.

---

## Why This Matters

Decoders are fundamental to memory systems — every RAM chip uses an internal address decoder to select one row out of thousands. The same structure underlies instruction decoding in processor control units. Encoders appear in interrupt controllers (priority encoders) and any system where a one-hot signal must be converted to a compact binary code for further processing.

---

## Key Learnings

- A decoder with an active enable input is structurally identical to a DEMUX — the same hardware serves both functions depending on how inputs are used
- NAND-based decoders produce active-low outputs; this is a common source of confusion when reading IC datasheets and must be accounted for in circuit design
- The encoder output equations are derived directly from which minterms contain each bit — no K-map needed, just inspecting the truth table columns
- A 3×8 decoder can perform binary-to-octal conversion and can implement any 3-variable Boolean function by OR-ing its selected outputs

---

## Hardware Evidence

Breadboard circuit photos for both the decoder (with enable control) and the encoder (8-input) are in `assets/`. Experimental results match the theoretical truth tables.

---

## Reference

📎 Full Lab Report: `lab_report.pdf`
