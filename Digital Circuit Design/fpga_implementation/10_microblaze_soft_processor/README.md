# MicroBlaze Soft Processor — 32-bit RISC Core on FPGA

**Category:** Advanced System Integration | **Board:** Spartan-6 LX9  
**Tools:** Xilinx ISE 14.7, Xilinx Coregen, Xilinx SDK (EDK)  
**Processor:** Xilinx MicroBlaze MCS (32-bit RISC soft core)

---

## Objective

Integrate and configure the Xilinx MicroBlaze 32-bit RISC soft processor core on the Spartan-6 FPGA, complete the full SoC design flow from HDL instantiation through SDK-based software development, and deploy a running application on hardware.

---

## Approach

Unlike all previous labs that implement pure RTL logic, this lab introduces the concept of a **soft processor** — a CPU implemented entirely in FPGA fabric (LUTs and flip-flops) rather than as a dedicated silicon block. The design flow spans both hardware (HDL + synthesis) and software (C/assembly + SDK), requiring coordination between Xilinx ISE and Xilinx SDK.

---

## Design / Implementation

### Full SoC Design Flow

```
1. ISE: Import MicroBlaze MCS IP core via Coregen
2. Verilog: Instantiate MicroBlaze in top-level module
3. ISE: Synthesise hardware design → generate BMM file
4. SDK: Create new application project → write/compile C code → generate .elf
5. ISE: Implement design → generate initial bitstream
6. data2mem: Merge .elf into bitstream
   microblaze_mcs_data2mem HelloWorld.elf
7. promgen: Generate binary configuration file
   promgen -w -p bin -u 0x0 helloworld.bit -spi -o helloworld
8. Program Spartan-6 flash → run application
```

### MicroBlaze MCS Configuration
- 32-bit RISC architecture, Harvard memory architecture
- Configurable peripherals: GPIO, UART, timers
- Program stored in BRAM (Block RAM) inside FPGA fabric
- `.elf` (Executable and Linkable Format) merged into bitstream via `data2mem`

### Key Tool Commands

```bash
# Export BMM file after synthesis
source ipcore_dir/microblaze_mcs_setup.tcl

# Merge compiled application into bitstream
microblaze_mcs_data2mem sdk/HelloWorld/Debug/HelloWorld.elf

# Generate binary for flash programming
promgen -w -p bin -u 0x0 helloworld.bit -spi -o helloworld
```

---

## Why This Matters

MicroBlaze demonstrates the fundamental advantage of FPGAs over fixed-architecture processors: the processor itself is a design choice. When pure RTL logic is insufficient — complex communication protocols, operating system requirements, dynamic algorithms — a soft processor can be instantiated and customised in minutes. This is how modern FPGA SoCs work: a hard ARM core (Zynq) or soft MicroBlaze core manages system control while custom RTL accelerators handle data-intensive tasks in parallel. Understanding the full hardware-software co-design flow is the entry point to embedded systems architecture.

---

## Key Learnings

- A soft processor consumes FPGA fabric (LUTs, BRAMs) that would otherwise be available for logic — the area/performance tradeoff must be evaluated for each application
- The `data2mem` step is non-obvious: without merging the `.elf` into the bitstream, the processor boots with no program in memory
- The `promgen` binary generation step separates the configuration bitstream from SPI flash programming — understanding the output formats matters for production deployment
- MicroBlaze's peripheral set (GPIO, UART, timers) is software-configurable after synthesis — changing peripheral behaviour requires recompiling the `.elf`, not re-synthesising the hardware

---

## Reference

📎 Full Lab Report: `lab_report.pdf`
