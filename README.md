# Digital Systems Engineering Portfolio

A hardware and HDL engineering portfolio spanning the full digital design stack — from Boolean logic on breadboard through FPGA-deployed FSMs, datapath/control architecture, DDR memory interfacing, and soft processor integration.

---

## Repository Structure

```
digital-systems-engineering-portfolio/
├── digital_logic_design/          ← 74xx TTL ICs on breadboard (Fall 2023)
│   ├── 01_demorgan_theorem_verification/
│   ├── 02_universal_gate_implementation/
│   ├── 03_binary_adder_subtractor/
│   ├── 04_magnitude_comparator/
│   ├── 05_encoder_decoder_circuits/
│   ├── 06_multiplexer_demultiplexer/
│   ├── 07_rs_d_flipflop/
│   └── 08_jk_t_flipflop/
│
├── digital_circuit_design/        ← Verilog simulation (Spring 2025)
│   └── 01_ripple_carry_adder_verilog/
│
└── fpga_implementation/           ← Spartan-6 FPGA deployment (Spring 2025)
    ├── 01_fpga_bringup_arithmetic/
    ├── 02_mux_decoder_fpga/
    ├── 03_bcd_seven_segment/
    ├── 04_ring_counter/
    ├── 05_synchronous_updown_counter/
    ├── 06_combination_lock_fsm/
    ├── 07_factorial_datapath_control/
    ├── 08_traffic_light_controller/
    ├── 09_ddr_sdram_interface/
    └── 10_microblaze_soft_processor/
```

---

## Skills Demonstrated

| Domain | Skills |
|--------|--------|
| **Digital Logic Design** | Boolean algebra, K-map minimisation, truth table verification |
| **Combinational Circuits** | Adders, subtractors, comparators, encoders/decoders, MUX/DEMUX |
| **Sequential Logic** | RS, D, JK, T flip-flops; ring counters; synchronous up/down counters |
| **Verilog HDL** | Gate-level, dataflow, and behavioral modeling; testbench writing |
| **FPGA Development** | Synthesis, pin constraint (UCF), bitstream generation, deployment |
| **FSM Design** | Mealy and Moore machines; state encoding; button debounce pipeline |
| **Datapath & Control** | Separated datapath/FSM controller architecture |
| **Display Interfacing** | BCD decoder, seven-segment (single & multiplexed), clock division |
| **Memory Interfacing** | DDR SDRAM via Xilinx MIG, UCF adaptation, calibration verification |
| **Soft Processor** | MicroBlaze MCS instantiation, SDK application flow, data2mem |

---

## Tools & Hardware

| Category | Details |
|----------|---------|
| **FPGA** | Xilinx Spartan-6 LX9 (CSG324), Mimas V2 development board |
| **EDA Tools** | Xilinx ISE 14.7, Xilinx Coregen, Xilinx SDK |
| **Simulation** | ModelSim |
| **HDL** | Verilog (all abstraction levels) |
| **Hardware ICs** | 74xx TTL series (7400, 7402, 7404, 7408, 7410, 7411, 7432, 7483, 7485, 7486) |
| **Prototyping** | Breadboard, DIP switches, jumper wires, LEDs |

---

## Lab Progression

### Digital Logic Design — Hardware (Fall 2023)

| # | Lab | Concept |
|---|-----|---------|
| 01 | De Morgan's Theorem Verification | Algebraic equivalence on breadboard |
| 02 | Universal Gate Implementation | NAND/NOR completeness, circuit synthesis |
| 03 | Binary Adder/Subtractor Circuit | Ripple carry, 2's complement via 7483 |
| 04 | Magnitude Comparator Design | 1-bit to 8-bit, cascaded IC 7485 |
| 05 | Encoder & Decoder Circuits | Binary encoding, address decoding |
| 06 | Multiplexer & Demultiplexer | Data routing, 8x1 hierarchical MUX |
| 07 | RS & D Flip-Flop Implementation | Bistable storage, latch vs. flip-flop |
| 08 | JK & T Flip-Flop Design | Toggle logic, master-slave configuration |

### Digital Circuit Design — HDL Simulation (Spring 2025)

| # | Lab | Concept |
|---|-----|---------|
| 01 | 4-bit RCA — Verilog Modeling | Gate-level vs. dataflow, ModelSim testbench |

### FPGA Implementation — Spartan-6 (Spring 2025)

| # | Lab | Concept |
|---|-----|---------|
| 01 | FPGA Bring-Up: Arithmetic Circuits | First deployment: gates + full subtractor |
| 02 | MUX & Decoder on FPGA | Combinational HDL to hardware |
| 03 | BCD to Seven-Segment Decoder | Display interfacing, pin mapping |
| 04 | Ring Counter — Shift Register | Clock division, behavioral sequential logic |
| 05 | Synchronous Up/Down Counter | Clocked counter, direction control |
| 06 | Electronic Combination Lock | FSM + button synchronizer + edge detector |
| 07 | Factorial Circuit — Datapath + Control | Separated datapath/FSM architecture |
| 08 | Traffic Light Controller | Mealy FSM + hardware timer module |
| 09 | DDR SDRAM Interface | MIG IP core, UCF adaptation, memory calibration |
| 10 | MicroBlaze Soft Processor | 32-bit RISC core, hardware-software co-design |

---

## About

**Institution:** University of Engineering and Technology, Peshawar
**Department:** Computer Systems Engineering
**Courses:** CSE-202L Digital Logic Design (Fall 2023) · Digital System Design (Spring 2025)

> All DLD circuits were physically implemented with 74xx TTL ICs on breadboard.
> All FPGA designs were synthesised and verified on the Spartan-6 board.
