
# SOFA FPGA Fabric Implementation using OpenFPGA

## Objective

The objective of Day 4 was to explore the **SOFA (Skywater Open-source FPGA) Fabric** using the **OpenFPGA Framework** and understand the complete FPGA implementation flow on a custom FPGA architecture.

A **4-bit synchronous up-counter** was implemented to study FPGA fabric generation, placement and routing, timing analysis, post-synthesis simulation, and power estimation.

The Day 4 coveres:

* OpenFPGA flow execution
* FPGA fabric generation
* VTR implementation flow
* Placement and routing
* Timing analysis
* Power analysis
* Post-synthesis simulation
* Analysis of generated implementation reports

---

# Introduction to SOFA FPGA Fabric

**SOFA (Skywater Open-source FPGA)** is an open-source FPGA architecture built using the **SkyWater 130nm PDK** and the **OpenFPGA Framework**. It enables FPGA architecture exploration, custom FPGA fabric generation, and implementation using the VTR CAD flow.

### FPGA Architecture Used

* **Architecture:** `FPGA1212_QLSOFA_HD_PNR`
* **Maximum Operating Frequency:** 50 MHz
* **Logic Resources:**

  * 1152 Lookup Tables (LUTs)
  * 2304 Flip-Flops (FFs)
  * 1152 Soft Adders

---

#  OpenFPGA Design Flow

The complete implementation flow followed during this experiment is illustrated below.

```text
Verilog RTL
      │
      ▼
OpenFPGA Flow
      │
      ▼
FPGA Fabric Generation
      │
      ▼
VTR Flow
 ├── Packing
 ├── Placement
 ├── Routing
 └── Timing Analysis
      │
      ▼
Post-Synthesis Simulation
      │
      ▼
Power Analysis
```

---

# Running the OpenFPGA Flow

The OpenFPGA framework was executed to generate the FPGA fabric and implement the counter design on the SOFA FPGA architecture.

## Command

```bash
make runOpenFPGA
```

---

## OpenFPGA Execution Log

<img width="940" src="https://github.com/user-attachments/assets/a9ca4995-b440-4e4d-9604-0a28fac81af8"/>

*Figure: OpenFPGA execution log showing successful task completion.*

---

# Generated Output Files

The OpenFPGA flow generated several implementation reports and design files used for further analysis.

<img width="940" src="https://github.com/user-attachments/assets/c306773f-9854-4752-89b1-73eb56daf93a"/>

*Figure: Generated implementation files after OpenFPGA execution.*

---

# Design Under Test

A **4-bit synchronous up-counter** was used as the reference design throughout the experiment.

## Verilog Code

```verilog
/* Important:
 * The simulation runs continuously because of the always block.
 * Press Ctrl + Z to stop the execution; otherwise,
 * the generated VCD file will continue to grow indefinitely.
 */

module up_counter(
    out,
    enable,
    clk,
    reset
);

output [3:0] out;
input enable, clk, reset;

reg [3:0] out;

always @(posedge clk)
begin
    if (reset)
        out = 4'b0000;
    else if (enable)
        out = out + 1;
end

endmodule
```

---

# Area Utilization Analysis

After implementation, the generated reports were analyzed to understand FPGA resource utilization.

### Resources Analyzed

* Lookup Tables (LUTs)
* Flip-Flops (FFs)
* Logic Elements
* FPGA Fabric Mapping
* Resource Occupancy

## Area Report

<img width="940" src="https://github.com/user-attachments/assets/a4becbf9-7d1b-4111-8009-304db251c916"/>

*Figure: Area utilization report of the counter mapped onto the SOFA FPGA fabric.*

---

## Logical Elements

<img width="940" src="https://github.com/user-attachments/assets/8feb8ee3-00b8-4891-83ec-8d4fef4bda1d"/>

*Figure: Logical elements extracted from `vpr_stdout.log`.*

---

# Timing Constraints

Timing constraints were specified using an **SDC (Synopsys Design Constraints)** file to guide the implementation tools during timing optimization.

## SDC Constraint File

```tcl
create_clock -period 10 up_counter_clk

set_input_delay  -clock up_counter_clk -max 0 [get_ports {*}]

set_output_delay -clock up_counter_clk -max 0 [get_ports {*}]
```

---

# Running VPR with Timing Constraints

The VPR implementation flow was modified to include the SDC constraint file during placement and routing.

<img width="940" src="https://github.com/user-attachments/assets/adb3cec8-f6c1-4f47-b4cf-3038d7a83b61"/>

*Figure: Updated VPR command including timing constraints.*

---

# Post-Synthesis Simulation

Post-synthesis simulation was performed using the generated netlist and timing information to verify gate-level functionality.

### Generated Files

* `.blif`
* `.net`
* `.place`
* `.route`
* `.sdf`
* Post-synthesis Verilog Netlist

---

## Simulation Command

<img width="940" src="https://github.com/user-attachments/assets/6ae14644-2110-4c63-840d-cd28cca7c964"/>

*Figure: Command used for post-synthesis simulation.*

---

## Simulation Waveform

<img width="999" src="https://github.com/user-attachments/assets/59ae682a-9728-4364-b829-757837d50eb7"/>

*Figure: Post-synthesis simulation waveform of the 4-bit up-counter.*

---

# Timing Analysis

Initially, setup timing violations were observed during implementation.

After applying proper timing constraints:

* Setup timing requirements were satisfied.
* Hold timing requirements were met.
* Positive timing slack was achieved.

---

## Setup Timing Report

<img width="940" src="https://github.com/user-attachments/assets/8f23f3b6-bf62-41a6-941d-8afac4deb0f4"/>

*Figure: Setup timing analysis report.*

---

## Hold Timing Report

<img width="940" src="https://github.com/user-attachments/assets/9c518ab4-4c20-4f50-bf7e-cfd6e3e9aa3d"/>

*Figure: Hold timing analysis report.*

---

# Power Analysis

Power estimation was performed using the implementation reports generated by the VTR flow.

### Parameters Evaluated

* Dynamic Power
* Static Power
* Clock Power
* Logic Power
* Signal Power

## Power Report

<img width="940" src="https://github.com/user-attachments/assets/b43ec986-e60f-4008-8a64-1725df5af1ea"/>

*Figure: Power analysis report for the counter implemented on the SOFA FPGA fabric.*

---

# Generated Implementation Files

| File                       | Description                  |
| -------------------------- | ---------------------------- |
| `counter.blif`             | Synthesized BLIF netlist     |
| `counter.net`              | Logical netlist connectivity |
| `counter.place`            | Placement information        |
| `counter.route`            | Routing information          |
| `counter.sdf`              | Timing delay annotation file |
| `counter_post_synthesis.v` | Gate-level Verilog netlist   |
| `report_timing.setup.rpt`  | Setup timing analysis report |
| `report_timing.hold.rpt`   | Hold timing analysis report  |
| `vpr_stdout.log`           | VPR execution log            |
| `power.rpt`                | Power estimation report      |

---

# Key Observations in Day 4

* Successfully explored the SOFA FPGA architecture.
* Generated a custom FPGA fabric using OpenFPGA.
* Performed placement and routing using the VTR flow.
* Analyzed FPGA resource utilization and logical mapping.
* Applied SDC timing constraints to resolve timing violations.
* Verified the design through post-synthesis simulation.
* Evaluated setup and hold timing reports.
* Estimated power consumption of the implemented design.
* Explored the implementation reports generated throughout the OpenFPGA flow.

---

# Learning Outcomes

Day 4 focused on implementing a **4-bit synchronous up-counter** on the **SOFA FPGA Fabric** using the **OpenFPGA Framework**.

The experiment demonstrated the complete open-source FPGA implementation flow, including FPGA fabric generation, placement and routing, timing verification, post-synthesis simulation, and power analysis.

This hands-on exercise strengthened the understanding of custom FPGA architectures, OpenFPGA workflows, timing optimization using SDC constraints, and implementation analysis through the VTR CAD flow.

By the end of Day 4, I gained practical experience in:

* Understanding the SOFA FPGA architecture
* Working with the OpenFPGA framework
* FPGA fabric generation
* Running the VTR implementation flow
* FPGA placement and routing
* Timing analysis and timing closure
* Applying SDC timing constraints
* Performing gate-level post-synthesis simulation
* FPGA power estimation
* Interpreting implementation reports generated by OpenFPGA and VTR

---



