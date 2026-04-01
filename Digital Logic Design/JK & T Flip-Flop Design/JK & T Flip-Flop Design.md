# JK & T Flip-Flop Design and Analysis

**Category:** Advanced | **Hardware:** 74xx TTL ICs on breadboard  
**ICs Used:** 7402 (NOR), 74011 (3-input AND)

---

## Objective

Implement and verify the JK flip-flop and T (Toggle) flip-flop on hardware, confirm that the JK design resolves the indeterminate state of the RS flip-flop, observe toggle behaviour under clock control, and analyse the master-slave configuration.

---

## Approach

The JK flip-flop was constructed using NOR and 3-input AND gates. The T flip-flop was derived by tying J and K together. State tables were verified experimentally by cycling through all input combinations with a manual clock pulse. The master-slave architecture was analysed to understand how race conditions in basic flip-flops are eliminated.

---

## Design / Implementation

### JK Flip-Flop

The JK flip-flop is a refinement of the RS flip-flop. The previously indeterminate J=K=1 state is now defined as **toggle** — Q switches to its complement on the clock edge.

| CP | J | K | Q(t+1) |
|----|---|---|--------|
|  0 | x | x | Qt (no change) |
|  1 | 0 | 0 | Qt (no change) |
|  1 | 0 | 1 | 0 (Reset) |
|  1 | 1 | 0 | 1 (Set) |
|  1 | 1 | 1 | Qt' (Toggle) ✓ |

This makes the JK flip-flop the most versatile of the standard flip-flop types — it can hold, set, reset, or toggle on every clock cycle.

**Implementation:** 3-input AND gates (74011) used to create clock-gated input terms. NOR gate cross-coupled pair forms the storage element. J and K inputs are combined with Q' and Q feedback respectively to implement the toggle condition.

### T Flip-Flop (Toggle)

Derived from JK by permanently tying J = K = T. The result is a single-input flip-flop that toggles output on every clock edge when T=1, and holds when T=0.

| CP | T | Q(t+1) |
|----|---|--------|
|  0 | x | Qt (no change) |
|  1 | 0 | Qt (no change) |
|  1 | 1 | Qt' (Toggle) |

T flip-flops are the primary building block of binary counters — each stage divides the clock frequency by 2.

### Master-Slave Configuration

The master-slave JK flip-flop uses two cascaded flip-flops driven by complementary clock signals:
- **Master** is enabled when CP=1; it samples inputs
- **Slave** is enabled when CP=0 (inverted clock); it propagates the master's state to output

This eliminates the race condition where output feedback changes input during the same clock cycle, ensuring single, stable output transitions per clock edge.

---

## Why This Matters

JK flip-flops and T flip-flops are the direct foundation of binary counters, frequency dividers, and shift registers. Every synchronous counter in digital design is implemented using T or JK flip-flops. The master-slave configuration introduced here is the architectural basis for modern edge-triggered D flip-flops used in all synchronous digital designs — and directly explains why setup and hold time constraints exist in timing analysis.

---

## Key Learnings

- The toggle state (J=K=1) of the JK flip-flop is what makes it uniquely useful for counter design — one flip-flop per bit, each toggling at half the frequency of the previous
- T flip-flop is not a separate hardware design — it is always derived from JK by wiring J and K together
- The master-slave architecture resolves the 1s-catching problem where output feedback causes multiple transitions per clock pulse
- A chain of T flip-flops produces a ripple counter — each stage's Q output clocks the next, dividing frequency by 2 per stage
- Understanding setup/hold time violations starts with understanding why the master-slave constraint exists at the physical level

---

## Hardware Evidence

Breadboard implementation using IC 7402 and IC 74011 is documented in `assets/`. State tables for JK and T flip-flop verified experimentally. Master-slave logic diagram included in reference report.

---

## Reference

📎 Full Lab Report: `lab_report.pdf`
