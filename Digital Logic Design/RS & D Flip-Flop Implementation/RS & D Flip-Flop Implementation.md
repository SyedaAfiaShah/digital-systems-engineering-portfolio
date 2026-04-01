# RS & D Flip-Flop Implementation — Bistable Storage Elements

**Category:** Advanced | **Hardware:** 74xx TTL ICs on breadboard  
**ICs Used:** 7410 (3-input NAND)

---

## Objective

Implement and verify the RS flip-flop, SR latch, and D flip-flop on hardware, observe the distinction between level-triggered latches and edge-triggered flip-flops, and confirm state tables experimentally across all input conditions.

---

## Approach

All circuits were constructed from NAND gates using IC 7410 (3-input NAND). The SR latch used a static DIP switch as the enable signal. The clocked RS and D flip-flops required a proper clock pulse input to observe edge-triggered behaviour. Truth tables were recorded by manually cycling through all input combinations.

---

## Design / Implementation

### RS Flip-Flop (Clocked)

A clocked RS flip-flop has Set (S), Reset (R), and Clock (CP) inputs. It is the fundamental bistable memory element.

| CP | S | R | Q(t+1) |
|----|---|---|--------|
|  0 | x | x | Qt (no change) |
|  1 | 0 | 0 | Qt (no change) |
|  1 | 0 | 1 | 0 (Reset) |
|  1 | 1 | 0 | 1 (Set) |
|  1 | 1 | 1 | Indeterminate ⚠️ |

The forbidden state (S=R=1) produces Q=Q'=0, violating the complementary output requirement. This is the fundamental limitation that the JK flip-flop resolves.

**Logic:** Built using two cross-coupled NAND gates with clock-controlled input gates (3-input NAND for gated operation).

### SR Latch

Structurally identical to the RS flip-flop but triggered by the input level of the enable signal rather than clock edges. Enable implemented using a DIP switch.

| S | R | Q   | Q'  |
|---|---|-----|-----|
| 0 | 0 | latch | latch |
| 0 | 1 | 0   | 1   |
| 1 | 0 | 1   | 0   |
| 1 | 1 | 0   | 0 ⚠️ |

### D Flip-Flop

Derived from the RS flip-flop by connecting D directly to S and D' (via inverter) to R. This eliminates the indeterminate state — only a single data input exists.

| CP | D | Q(t+1) |
|----|---|--------|
|  0 | x | Qt (no change) |
|  1 | 0 | 0 |
|  1 | 1 | 1 |

The D flip-flop samples and holds the input value at each clock edge — the fundamental operation of all register storage.

---

## Why This Matters

The RS flip-flop is the lowest-level storage primitive in digital hardware. Every register in a CPU is built from D flip-flops. RAM cells use the latch structure. The distinction between latches (level-sensitive) and flip-flops (edge-triggered) is critical in digital design — mixing them incorrectly causes timing violations that are among the most difficult hardware bugs to diagnose. This lab builds physical intuition for that distinction.

---

## Key Learnings

- The indeterminate state of the SR flip-flop (S=R=1) is not just a theoretical concern — it produces physically unpredictable output that can latch into either state after the inputs change
- A D latch can be enabled by any logic level (DIP switch); a D flip-flop must be clocked — this is the practical difference between asynchronous and synchronous storage
- The D flip-flop is derived from RS by ensuring S and R are always complements — a single input drives both through an inverter
- Flip-flops are the building blocks of registers, counters, and FIFOs; latches are used in address decoding and bus interfaces where level-sensitive control is appropriate

---

## Hardware Evidence

Breadboard photos and experimental truth tables for the RS latch, clocked RS flip-flop, and D flip-flop are in `assets/`. All results match expected state tables.

---

## Reference

📎 Full Lab Report: `lab_report.pdf`
