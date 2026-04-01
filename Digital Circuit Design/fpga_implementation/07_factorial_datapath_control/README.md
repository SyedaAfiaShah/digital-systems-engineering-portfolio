# Factorial Computation Circuit — Datapath & Control Design

**Category:** Advanced FPGA | **Board:** Spartan-6 LX9  
**Tools:** Xilinx ISE 14.7 | **Language:** Verilog (behavioral)

---

## Objective

Design a hardware circuit that computes the factorial of a 4-bit input number by separating the design into an explicit **datapath** (registers, arithmetic) and a **controller FSM** (sequencing, control signals). Extend the architecture to implement multiplication by repeated addition using the same datapath/control pattern.

---

## Approach

The datapath/control separation is the fundamental architectural pattern in processor design. The controller FSM generates enable and select signals at the correct clock cycles; the datapath executes arithmetic operations on registers based on those signals. Neither module is meaningful without the other — they communicate through a `done` handshake signal.

---

## Design / Implementation

### System Architecture

```
start ──→ FactorialController (FSM) ──→ load_A, start_calc ──→ FactorialDataPath
                    ↑                                                  │
                    └──────────────── datapath_done ←─────────────────┘
                                           │
                                       result ──→ output
```

### FSM Controller — 4 States

| State | Action | Outputs |
|-------|--------|---------|
| IDLE | Wait for start | — |
| LOAD | Load input N into A_reg and B_reg; set factorial=1 | `load_A=1` |
| CALC | Multiply: `factorial×B_reg`, decrement `B_reg` each cycle | `start_calc=1` |
| DONE | Assert done, hold result | `done=1` |

```verilog
CALC: begin
  start_calc = 1;
  if (datapath_done) next = DONE;
  else               next = CALC;
end
```

### Datapath — Registered Computation

```verilog
if (start_calc && B_reg > 1) begin
  factorial <= factorial * B_reg;  // Iterative multiply
  B_reg     <= B_reg - 1;
end else if (start_calc && B_reg <= 1 && done == 0) begin
  result_out <= factorial;
  done       <= 1;
end
```

The datapath iterates B_reg from N down to 2, accumulating the product. The `done` flag is a single-cycle handshake that tells the FSM the computation is complete.

### Task 2 — Multiplier by Repeated Addition

The same FSM template (IDLE → LOAD → CALC → DONE) was reused for a multiplier: instead of `factorial × B_reg`, the datapath accumulates `sum + A_reg` while decrementing `B_reg` from B down to 0. This demonstrates the pattern's reusability across different arithmetic algorithms.

---

## Why This Matters

This is the architecture of every ALU and arithmetic accelerator: a datapath containing registers and combinational arithmetic, controlled by an FSM that sequences operations. Modern DSP blocks, matrix multiply units, and cryptographic accelerators all use this separation. It is also the conceptual foundation of microarchitecture — the fetch/decode/execute cycle of a processor is the same FSM-controlled datapath pattern at larger scale.

---

## Key Learnings

- Separating datapath and control makes each component independently verifiable — the FSM can be tested with a simple datapath stub, and the datapath with a simple counter-like control signal
- The `done` handshake between datapath and controller is the critical interface — a race condition here (done asserted too early) produces wrong results that are difficult to diagnose without waveforms
- The CALC state loops for N-1 cycles for factorial(N) — latency is data-dependent, unlike pipelined designs where throughput is fixed
- The same FSM template (IDLE/LOAD/CALC/DONE) is directly reusable for any iterative computation, making it a high-value design pattern to internalize

---

## Reference

📎 Full Lab Report: `lab_report.pdf`
