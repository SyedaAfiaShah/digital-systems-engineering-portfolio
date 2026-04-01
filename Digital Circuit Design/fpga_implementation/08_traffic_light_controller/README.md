# Traffic Light Controller — Mealy Machine FSM

**Category:** Advanced FPGA | **Board:** Spartan-6 LX9  
**Tools:** Xilinx ISE 14.7 | **Language:** Verilog (behavioral)

---

## Objective

Design and implement a traffic light controller as a Mealy finite state machine in Verilog, integrating a hardware timer module for timed state transitions, and deploy on the Spartan-6 with LED outputs representing highway and farm road signals.

---

## Approach

The controller manages two intersecting roads: a highway (normally GREEN) and a farm road (normally RED). The farm road gets a GREEN phase only when a vehicle is detected. A separate hardware timer module counts real seconds, with its `second_count` output feeding the FSM transition conditions directly.

---

## Design / Implementation

### State Machine — 4 States

| State | Highway | Farm | Transition Condition |
|-------|---------|------|----------------------|
| `HG_FR` | 🟢 Green | 🔴 Red | Vehicle detected → `HY_FR` |
| `HY_FR` | 🟡 Yellow | 🔴 Red | timer ≥ 3s → `HR_FG` |
| `HR_FG` | 🔴 Red | 🟢 Green | timer ≥ 10s → `HR_FY` |
| `HR_FY` | 🔴 Red | 🟡 Yellow | timer ≥ 3s → `HG_FR` |

Output encoding: `3'b001` = Green, `3'b010` = Yellow, `3'b100` = Red (one-hot RGB-style)

```verilog
HG_FR: begin
  highwaySignal = 3'b001; farmSignal = 3'b100;
  if (vehicle) next = HY_FR;
  else         next = HG_FR;
end
```

### Timer Module

```verilog
always @(posedge clk) begin
  if (enable) begin
    if (count >= 49_999_999) begin
      count <= 0;
      second_count <= second_count + 1;
    end else count <= count + 1;
  end else begin
    count <= 0; second_count <= 0;  // Reset when disabled
  end
end
```

The timer is enabled whenever the FSM is not in `HG_FR` state, and resets when returning to the idle (highway green) state.

### Top Module Wiring
```verilog
assign enable_timer = (fsm.state != 2'b00);
```

Timer enable is derived directly from FSM state, creating a feedback dependency between controller and timer — the FSM reads the timer, the timer enable comes from the FSM.

---

## Why This Matters

Traffic light controllers are the canonical real-world FSM example because they combine event-driven transitions (vehicle sensor) with time-driven transitions (phase timers) — the two fundamental trigger types in reactive systems. The same architecture governs elevator controllers, protocol handshake sequencers, and industrial automation state machines. The Mealy machine structure (outputs depend on both state and inputs) is more efficient here than Moore — it allows the vehicle sensor to immediately influence the transition without waiting for an extra clock cycle.

---

## Key Learnings

- A Mealy machine produces outputs based on current state AND inputs — this makes the `HG_FR` state immediately responsive to the vehicle sensor without an extra clock latency
- The timer must reset on every state transition back to the idle state — forgetting this causes incorrect timing on the second cycle through
- Deriving `enable_timer` from FSM state rather than a separate control signal keeps the timer and FSM tightly coupled and eliminates the possibility of the timer running when it shouldn't
- The one-hot signal encoding for traffic light colours (`001/010/100`) maps cleanly to three individual LED outputs — no decoder needed

---

## Hardware Evidence

Board LED photographs showing highway and farm signal states through a complete light cycle are in `assets/`.

---

## Reference

📎 Full Lab Report: `lab_report.pdf`
