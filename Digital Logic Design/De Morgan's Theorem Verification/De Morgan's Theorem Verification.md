# De Morgan's Theorem — Hardware Verification

**Category:** Foundation | **Hardware:** 74xx TTL ICs on breadboard  
**ICs Used:** 7400 (NAND), 7402 (NOR), 7404 (NOT), 7408 (AND), 7432 (OR)

---

## Objective

Verify both forms of De Morgan's theorem through live breadboard circuits, confirming that logically equivalent expressions produce identical outputs for all input combinations.

---

## Approach

Two independent circuits were built for each theorem — one implementing the left-hand side expression, one implementing the right-hand side — and their outputs were compared experimentally across all input states using LEDs as output indicators.

**Theorem 1:** `(A + B)' = A' · B'`  → NOR gate ≡ Bubbled AND  
**Theorem 2:** `(A · B)' = A' + B'`  → NAND gate ≡ Bubbled OR

---

## Design / Implementation

**Theorem 1 — LHS:** Input A and B fed into a NOR gate (IC 7402). Output passed to LED.  
**Theorem 1 — RHS:** A and B individually inverted via IC 7404, then passed to an AND gate (IC 7408). Output passed to second LED.

**Theorem 2 — LHS:** A and B fed into a NAND gate (IC 7400). Output to LED.  
**Theorem 2 — RHS:** A and B inverted via 7404, then OR'd via IC 7432. Output to LED.

In all four cases, LHS and RHS LED outputs matched for every input combination, confirming equivalence experimentally.

**Extended task:** Simplified `F = ((A·B)' + A)'` using De Morgan's theorem to `F = (A·B)·A'`, then verified on hardware. Also confirmed circuit equivalence between `(A + B')·C'` and `((A'·B) + C)'` algebraically and experimentally.

---

## Verified Truth Tables

**Theorem 1:**

| A | B | (A+B)' | A'·B' |
|---|---|--------|-------|
| 0 | 0 |   1    |   1   |
| 0 | 1 |   0    |   0   |
| 1 | 0 |   0    |   0   |
| 1 | 1 |   0    |   0   |

**Theorem 2:**

| A | B | (A·B)' | A'+B' |
|---|---|--------|-------|
| 0 | 0 |   1    |   1   |
| 0 | 1 |   1    |   1   |
| 1 | 0 |   1    |   1   |
| 1 | 1 |   0    |   0   |

---

## Why This Matters

De Morgan's theorem is the formal basis for gate substitution in real IC design. It is why NAND gates are the dominant primitive in digital ASICs and FPGAs — any Boolean expression can be implemented with NAND-only logic. Understanding this at the hardware level is essential before working with synthesis tools that apply these transformations automatically.

---

## Key Learnings

- Physically demonstrated that a NOR gate and a bubbled-AND gate are electrically interchangeable
- Confirmed that logical equivalence means identical hardware behaviour, not just identical truth tables on paper
- Applied De Morgan's theorem to simplify a multi-gate expression and reduced it to a fewer-gate equivalent circuit
- Understood why NAND gates are favoured commercially — faster switching, fewer transistors, CMOS-compatible

---

## Hardware Evidence

Breadboard circuit photos (Figs 1–5) documenting each circuit configuration are available in `assets/`. Experimental truth tables match theoretical values for all input combinations.

---

## Reference

📎 Full Lab Report: `lab_report.pdf`
