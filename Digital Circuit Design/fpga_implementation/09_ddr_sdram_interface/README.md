# DDR SDRAM Interface on Spartan-6

**Category:** Advanced System Integration | **Board:** Mimas V2 (Spartan-6 LX9)  
**Tools:** Xilinx ISE 14.7, Xilinx Memory Interface Generator (MIG)  
**Memory:** Micron MT46H32M16 512Mbit LPDDR @ 100 MHz

---

## Objective

Interface and access DDR SDRAM (512Mbit LPDDR) on the Mimas V2 FPGA board using the Xilinx Memory Interface Generator (MIG) to create a Spartan-6 memory controller, configure UCF pin constraints for the target board, build the full design flow, and verify memory operation via onboard LEDs.

---

## Approach

The Spartan-6 LX9 contains two hardened DDR memory controllers. One is connected to the onboard LPDDR device on Mimas V2 (Bank 3). MIG generates a complete memory controller IP core and example design. This lab involved adapting the auto-generated design to the Mimas V2 hardware by modifying I/O standards and pin assignments in the UCF constraints file, then executing the full ISE synthesis-to-bitstream flow.

---

## Design / Implementation

### System Architecture

```
User Logic ─→ Wrapper Interface ─→ MIG Memory Controller ─→ LPDDR SDRAM
                                         (Spartan-6 hard macro)
```

### MIG Configuration
- Device: XC6SLX9, CSG324 package
- Memory: Micron MT46H32M16 LPDDR
- Bank: Bank 3 (connected to onboard LPDDR on Mimas V2)
- DDR Clock: 100 MHz (matching onboard clock — avoids PLL reconfiguration)
- Port: Single port, Port 0

### UCF Constraint Modifications (Mimas V2 adaptation)

The auto-generated UCF targets Xilinx's reference board. Key changes required for Mimas V2:

| Parameter | Original | Mimas V2 |
|-----------|----------|----------|
| VCCAUX | 2.5V | **3.3V** |
| `calib_done` IOSTANDARD | LVCMOS18 | **LVCMOS33** |
| `error` IOSTANDARD | LVCMOS18 | **LVCMOS33** |
| `calib_done` LOC | B2 | **T18 (LED1)** |
| `error` LOC | A2 | **T17 (LED2)** |
| Clock IOSTANDARD | LVCMOS25 | **LVCMOS33** |
| `c3_sys_clk` LOC | — | **V10** |
| `c3_sys_rst_n` LOC | — | **M16 (SW3)** |

Additionally: `NET "c3_sys_rst_n" PULLDOWN` added to keep the memory controller out of reset without requiring an explicit external signal.

### Build & Verification

Build executed via `ise_flow.bat`, generating `example_top.bin` for flash programming. **LED1 (calib_done) lights up** after power-on to confirm DDR calibration completed successfully. **LED2 (error)** remains OFF to indicate no memory test failures.

---

## Why This Matters

DDR SDRAM interfacing is a prerequisite skill for any FPGA application requiring significant data storage — image processing pipelines, video frame buffers, data loggers, and high-speed signal capture all require external memory beyond the limited Block RAM available in FPGA fabric. The Spartan-6's hardened memory controller and MIG tooling represent the standard industry approach to memory IP integration. Adapting auto-generated UCF files to a specific board is a practical skill that distinguishes engineers who can deploy designs from those who can only simulate them.

---

## Key Learnings

- DDR memory controllers have strict I/O standard requirements — mixing LVCMOS18 and LVCMOS33 I/O on the same bank causes DRC errors during implementation
- The `calib_done` signal is the critical indicator of successful DDR initialisation — the controller runs an automatic calibration sequence before becoming accessible to user logic
- MIG generates a complete example design including a self-test pattern — this allows hardware verification without writing any user logic
- The active-high/active-low convention for `c3_sys_rst_n` (despite the name) is a known MIG configuration quirk — `PULLDOWN` is the correct workaround for Mimas V2

---

## Hardware Evidence

LED1 (calib_done) ON and LED2 (error) OFF after programming confirms successful DDR calibration on Mimas V2 hardware.

---

## Reference

📎 Full Lab Report: `lab_report.pdf`
