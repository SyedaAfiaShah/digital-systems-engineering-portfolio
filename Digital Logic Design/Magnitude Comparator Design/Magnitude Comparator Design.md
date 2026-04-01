# Magnitude Comparator Design — 1-bit to 8-bit

**Category:** Intermediate | **Hardware:** 74xx TTL ICs on breadboard  
**ICs Used:** 7404 (NOT), 7408 (AND), 7432 (OR), 7486 (XOR), 7485 (4-bit comparator IC)  
**Other:** DIP switches, 1kΩ resistors, 3 LEDs (A>B, A=B, A<B)

---

## Objective

Design and implement magnitude comparators from the ground up — starting with a 1-bit comparator derived from Boolean expressions, scaling to 2-bit using K-map simplification, and finally constructing an 8-bit comparator by cascading two IC 7485 chips.

---

## Approach

Three output functions were defined for any comparison: `A>B`, `A=B`, and `A<B`. Boolean expressions were derived for each, simplified using K-maps, and then wired on breadboard. The 8-bit design uses the cascade input pins of IC 7485 to chain comparators, mirroring how real hardware scales comparison width.

---

## Design / Implementation

### 1-bit Comparator

| Function | Expression |
|----------|-----------|
| A > B | `F1 = A · B'` |
| A < B | `F2 = A' · B` |
| A = B | `F3 = (A ⊕ B)' = A⊙B` |

Wired using IC 7408 (AND), IC 7404 (NOT), IC 7486 (XOR). Three LEDs indicate the result.

### 2-bit Comparator

For 2-bit numbers A1A0 and B1B0:

| Function | Expression |
|----------|-----------|
| A > B | `F1 = A1B1' + (A1⊙B1)A0B0'` |
| A = B | `F3 = (A1⊙B1)(A0⊙B0)` |
| A < B | `F2 = F1'·F3'` (derived from K-map) |

K-map simplification reduced F2 to `F2' = (F1 + F3)'`, directly wired using NOR-equivalent NAND logic.

### 8-bit Comparator (IC 7485 Cascade)

IC 7485 is a 4-bit comparator with three cascade input pins (1A<B, 1A=B, 1A>B). Two ICs are chained:
- IC 7485 (1) compares lower 4 bits (A3A2A1A0 vs B3B2B1B0)
- Its three outputs feed the cascade inputs of IC 7485 (2), which compares upper 4 bits
- Final outputs (A>B, A=B, A<B) from IC 7485 (2) drive three LEDs

This produces a complete 16-input, 3-output 8-bit comparator with verified hardware results.

---

## Verified Results (8-bit)

| A | B | A>B | A=B | A<B |
|---|---|-----|-----|-----|
| 0000 0000 | 0000 0000 | 0 | 1 | 0 |
| 0001 0001 | 0000 0000 | 1 | 0 | 0 |
| 0000 0000 | 0001 0001 | 0 | 0 | 1 |

---

## Why This Matters

Magnitude comparators are a core component in processor datapaths — they appear in branch condition evaluation (`if A > B`), sorting networks, and priority arbiters. The cascade architecture of IC 7485 directly reflects how modern hardware scales comparison width using carry/borrow chains, analogous to how adders use carry propagation.

---

## Key Learnings

- Three distinct Boolean functions are required for a full magnitude comparison — greater, equal, and less than cannot be derived from each other without additional logic
- K-map minimisation of the A<B function revealed it is simply the complement of the OR of the other two outputs — a non-obvious result that reduces gate count
- IC 7485's cascade pins allow width extension without redesigning the circuit — understanding pinout and cascade connections is essential for multi-chip designs
- Breadboard photo from the 8-bit implementation shows 16 input lines, two cascaded ICs, and three output LEDs simultaneously active

---

## Hardware Evidence

Breadboard photo of the 8-bit comparator with two cascaded IC 7485 chips is in `assets/`. Verified truth table confirms correct comparison output for equal, greater-than, and less-than cases.

---

## Reference

📎 Full Lab Report: `lab_report.pdf`
