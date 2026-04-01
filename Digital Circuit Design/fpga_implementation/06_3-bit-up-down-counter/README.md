# FPGA-Based Sequential Counter Systems

## Objective

Design and implement multiple sequential counter architectures on an FPGA using a hardware clock source, with real-time output displayed on a seven-segment interface.

## Approach

A unified hardware system was developed using a modular design strategy:
- A clock divider converts the FPGA’s 100 MHz clock into a 1 Hz signal
- Different counter designs were implemented and tested independently
- Outputs were visualized using a seven-segment display

This approach ensures consistency across all designs while enabling direct comparison of counter behaviors.

## Design / Implementation

### 1. Clock Management

- A clock divider module reduces the 100 MHz onboard clock to 1 Hz
- Enables human-observable transitions on hardware
- Ensures stable synchronous operation across all modules

---

### 2. Counter Implementations

#### a) 3-bit Up Counter
- Counts in ascending order (0 → 7)
- Resets to zero on reset signal
- Demonstrates basic synchronous counting behavior

#### b) 3-bit Down Counter
- Counts in descending order (7 → 0)
- Initializes from maximum state
- Highlights reverse sequencing logic

#### c) 3-bit Up/Down Counter
- Direction controlled via input signal:
  - High → Count Up
  - Low → Count Down
- Demonstrates dynamic control of sequential logic

#### d) Ring Counter (8-bit)
- Circular shift register implementation
- Single active bit rotates through all positions
- Output mapped to numerical position for display

---

### 3. Display Interface

- Counter outputs are encoded using a seven-segment decoder
- Binary values are mapped to segment patterns
- Real-time visualization of system state on FPGA board

---

## Why This Matters

These designs represent fundamental building blocks in digital systems:

- Counters → timing systems, control units, memory addressing  
- Ring counters → state machines, sequencing circuits  
- Clock division → essential for hardware timing control  

Together, they form the basis of:
- Embedded systems
- Processor design
- Digital control logic

## Key Learnings

- Designing synchronous sequential systems in Verilog  
- Managing clock signals and timing in FPGA environments  
- Implementing multiple counter architectures efficiently  
- Interfacing logic with hardware output devices  
- Understanding practical hardware behavior beyond simulation  

## Hardware / Implementation Evidence

- Implemented on FPGA (Spartan-6)
- Verified using onboard seven-segment display
- Stable operation observed with clock-driven transitions

(Images / hardware validation to be added)

## Reference

📎 Full Lab Report: lab_report.pdf