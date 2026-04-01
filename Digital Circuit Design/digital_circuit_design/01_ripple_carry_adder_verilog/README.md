# 4-bit Ripple Carry Adder — Gate-Level & Dataflow Modeling

**Category:** HDL Simulation | **Tool:** ModelSim  
**Modeling Levels:** Gate-level (structural) and Dataflow  
**Language:** Verilog

---

## Objective

Implement a 4-bit Ripple Carry Adder in Verilog using two distinct abstraction levels — gate-level structural modeling and dataflow modeling — then verify both implementations through testbench simulation in ModelSim.

---

## Approach

The design follows a bottom-up methodology: primitive gate modules (XOR for sum, AND for carry) are assembled into a half adder, two half adders form a full adder, and four full adder instances are chained to produce the 4-bit RCA. The same circuit was then re-implemented using continuous assignment (`assign`) statements to compare modeling styles.

---

## Design / Implementation

### Task 1a — Gate-Level Structural Modeling

Hierarchy: `Sum` (XOR) → `Carry` (AND) → `HA` (Half Adder) → `FA` (Full Adder) → `RCA`

```verilog
module FA(S, C, A, B, Cin);
  wire s1, c1, c2;
  HA h1(s1, c1, A, B);
  HA h2(S, c2, Cin, s1);
  or o(C, c1, c2);
endmodule

module RCA(S, Cout, A, B, Cin);
  input [3:0] A, B; input Cin;
  output [3:0] S; output Cout;
  wire [3:0] c;
  FA f1(S[0], c[0], A[0], B[0], Cin);
  FA f2(S[1], c[1], A[1], B[1], c[0]);
  FA f3(S[2], c[2], A[2], B[2], c[1]);
  FA f4(S[3], Cout, A[3], B[3], c[2]);
endmodule
```

### Task 1b — Dataflow Modeling

The same full adder is expressed using `assign` statements rather than gate instantiation:

```verilog
module FA(S, C, A, B, Cin);
  wire s1, c1, c2;
  assign s1 = A ^ B;
  assign c1 = A & B;
  assign S  = s1 ^ Cin;
  assign c2 = s1 & Cin;
  assign C  = c1 | c2;
endmodule
```

### Verified Simulation Results

| A    | B    | Cin | Sum  | Cout |
|------|------|-----|------|------|
| 0010 | 0010 |  0  | 0100 |  0   |
| 0010 | 1100 |  0  | 1110 |  0   |

Both implementations produced identical waveform outputs in ModelSim, confirming functional equivalence across abstraction levels.

---

## Why This Matters

This lab demonstrates the two fundamental ways to describe hardware in HDL. Gate-level modeling maps directly to physical gates — useful for manual analysis and post-synthesis netlist understanding. Dataflow modeling is closer to how synthesis tools receive RTL descriptions and translate them into gate networks. Understanding both levels is essential for reading synthesis reports and debugging netlists in tools like Xilinx ISE or Vivado.

---

## Key Learnings

- Gate-level and dataflow models of the same circuit are functionally identical but differ in readability and synthesis control
- Structural instantiation mirrors the physical hierarchy of a real chip — each `FA` instance becomes a separate placed cell after synthesis
- The `$monitor` system task in the testbench enables real-time output tracking without needing to manually read waveforms
- Carry propagation delay is observable in simulation — each FA stage adds one gate delay before its carry output stabilises

---

## Simulation Evidence

ModelSim waveform screenshots showing correct Sum and Cout outputs for both test vectors are in `assets/`.

---

## Reference

📎 Full Lab Report: `lab_report.pdf`
