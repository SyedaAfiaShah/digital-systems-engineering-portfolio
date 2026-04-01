# Electronic Combination Lock — Sequential FSM Design

**Category:** Advanced FPGA | **Board:** Spartan-6 LX9  
**Tools:** Xilinx ISE 14.7 | **Language:** Verilog (behavioral)

---

## Objective

Design and deploy an FSM-based electronic combination lock with two input buttons (0 and 1), a reset button, and an LED unlock output. The lock opens only on the correct 5-bit input sequence. Two sequences were implemented: `11011` and `01011`.

---

## Approach

The design uses a full hardware signal conditioning pipeline before the FSM: a **synchronizer** (two-stage flip-flop chain) eliminates metastability from asynchronous button presses, and a **level-to-pulse converter** ensures each button press generates exactly one clock-edge-aligned pulse to the FSM — preventing multi-state transitions from a single press. This is production-quality button handling for FPGA inputs.

---

## Design / Implementation

### Signal Conditioning Pipeline

```
Button (async) → Synchronizer (2×D-FF) → Level-to-Pulse → FSM input
```

**Synchronizer:** Two cascaded D flip-flops re-clock the asynchronous button signal to eliminate metastability violations.

**Level-to-Pulse converter:** Detects the rising edge by AND-ing the current synchronised input with the complement of the previous cycle's value (`pulse = ~Q & synch_input`). Produces one clean pulse per button press regardless of hold duration.

### FSM — Sequence `11011` (6 states: S0–S5)

| State | Received so far | Unlock |
|-------|----------------|--------|
| S0 | — | No |
| S1 | 1 | No |
| S2 | 11 | No |
| S3 | 110 | No |
| S4 | 1101 | No |
| S5 | 11011 | **YES** — LED ON |

State transitions advance on correct input or return to the appropriate prefix state on incorrect input (Mealy-style error recovery).

The seven-segment display shows the current state number (0–5) for real-time debug visibility.

### Sequence `01011` — Modified FSM

A second implementation unlocks on `01011`. This required a different 6-state transition diagram — the FSM was redesigned from scratch rather than modified, demonstrating that the sequence determines the entire state graph structure.

---

## Why This Matters

This is a practical application of FSM theory. The same architecture — synchronizer + edge detector + state machine — appears in UART receivers, SPI decoders, keyboard scan controllers, and any digital system that must reliably respond to asynchronous physical inputs. The button debounce pipeline is something most textbook FSM labs omit; implementing it correctly on real hardware demonstrates understanding beyond simulation.

---

## Key Learnings

- Metastability is a real hardware failure mode — two-stage synchronization is the minimum required for any asynchronous input to a clocked design
- Level-to-pulse conversion is essential when button hold time exceeds one clock cycle — without it, one press causes multiple FSM transitions
- The FSM transition graph is entirely determined by the unlock sequence — changing the sequence requires redesigning the state graph, not just changing a parameter
- The seven-segment state display transforms an abstract FSM into a visible, debuggable system — essential during hardware bring-up

---

## Hardware Evidence

Board photographs showing LED illumination on correct sequence entry, and seven-segment state display for both `11011` and `01011` implementations, are in `assets/`.

---

## Reference

📎 Full Lab Report: `lab_report.pdf`
