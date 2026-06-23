# RVMYTH RISC-V Core Implementation on SOFA FPGA Fabric

## Objective

The objective of Day 5 was to implement the **RVMYTH RISC-V processor** on a **custom SOFA FPGA Fabric** using the **OpenFPGA** and **VTR** toolchain.

Unlike previous experiments that used a simple counter design, this session focused on implementing a complete processor core to analyze FPGA resource utilization, timing performance, routing, and post-implementation verification.

The Day5 of workshop included:

* RVMYTH processor implementation
* OpenFPGA and VTR execution
* FPGA resource utilization analysis
* Timing constraint implementation
* Setup and hold timing verification
* Post-implementation simulation
* FPGA mapping analysis

---

# Introduction

The **RVMYTH** processor is a lightweight **RISC-V CPU core** implemented on a custom **SOFA FPGA architecture** using the OpenFPGA framework.

The complete implementation flow converts the RTL design into an FPGA implementation by performing synthesis, placement, routing, timing analysis, and post-implementation verification.

Compared to earlier counter-based examples, the processor requires significantly more FPGA resources and generates more complex timing and routing reports.

---

# Implementation Flow

The complete FPGA implementation flow followed during this experiment is shown below.

```text
RVMYTH RTL Design
        │
        ▼
OpenFPGA Flow
        │
        ▼
VTR Implementation
 ├── Synthesis
 ├── Packing
 ├── Placement
 ├── Routing
 └── Timing Analysis
        │
        ▼
Post-Implementation Simulation
        │
        ▼
Report Generation
```

---

# FPGA Resource Utilization

After synthesis and mapping, the utilization report provides detailed information about the FPGA resources consumed by the RVMYTH processor.

### Circuit Statistics

| Parameter    | Value |
| ------------ | ----: |
| Total Blocks |  5526 |
| Inputs       |     2 |
| Outputs      |     8 |
| Latches      |  1807 |
| 0-LUTs       |     4 |
| 4-LUTs       |  3705 |

### Net Statistics

| Parameter      | Value |
| -------------- | ----: |
| Total Nets     |  5518 |
| Average Fanout |   3.1 |
| Maximum Fanout |  1807 |
| Minimum Fanout |   1.0 |

### Timing Graph

| Parameter          |  Value |
| ------------------ | -----: |
| Timing Graph Nodes | 22,705 |
| Netlist Clocks     |      1 |

<img width="737" src="https://github.com/user-attachments/assets/17be3dde-1973-4f0d-b3ef-595d39615bb9"/>

*Figure: FPGA utilization report showing primitive block usage and logic resource statistics.*

---

# Timing Constraints

An **SDC (Synopsys Design Constraints)** file was created to define the timing requirements for the processor implementation.

The constraints specify:

* Clock period
* Input delay
* Output delay

## SDC Constraint File

```tcl
create_clock -period 200 clk

set_input_delay  -clock clk -max 0 [get_ports {*}]

set_output_delay -clock clk -max 0 [get_ports {*}]
```

<img width="940" src="https://github.com/user-attachments/assets/189d0a21-364a-44cf-8984-dbb105813b27"/>

*Figure: SDC timing constraint file used during FPGA implementation.*

---

# Setup Timing Analysis

Timing analysis was performed after placement and routing to verify that the processor satisfies all setup timing requirements.

### Observations

* Positive setup slack achieved
* Data arrival time within required limits
* Successful setup timing closure

<img width="658" src="https://github.com/user-attachments/assets/322adca8-4b7e-4e0d-9ce2-d97e25f2f13f"/>

*Figure: Setup timing report showing positive timing slack.*

---

# Hold Timing Analysis

Hold timing analysis verifies that the data remains stable for the required duration after the active clock edge.

### Observations

* Positive hold slack
* No hold timing violations
* Stable timing behavior after routing

<img width="703" src="https://github.com/user-attachments/assets/56ddd848-a357-4390-9668-ffed0fdb4a0e"/>

*Figure: Hold timing report confirming successful timing closure.*

---

# Post-Implementation Simulation

After synthesis, placement, and routing, a post-implementation simulation was performed to verify the behavior of the generated FPGA netlist.

The simulation validates:

* Correct processor execution
* Functional behavior after implementation
* Stable output transitions
* Proper clock and reset operation

## Simulation Waveform

<img width="973" src="https://github.com/user-attachments/assets/0faf8ae4-f2db-4f7f-886a-d55028ed1691"/>

*Figure: Post-implementation simulation waveform of the RVMYTH processor.*

---

# Design Analysis

## FPGA Resource Utilization

Compared to previous counter-based implementations, the RVMYTH processor required significantly more FPGA resources due to its increased architectural complexity.

The implementation utilized:

* Large number of LUTs
* Multiple latches
* Extensive routing resources
* Complex logic interconnections

---

## Timing Performance

After applying timing constraints:

* Setup timing requirements were satisfied.
* Hold timing requirements were satisfied.
* Positive timing slack was achieved.
* Successful timing closure was obtained.

---

## Implementation Verification

The OpenFPGA and VTR flow successfully generated:

* Placement reports
* Routing reports
* Timing reports
* Utilization reports
* Post-implementation simulation outputs

The generated reports confirmed that the processor was correctly mapped onto the custom SOFA FPGA architecture.

---

# Generated Reports

The implementation produced the following important reports and output files:

| File                      | Description                         |
| ------------------------- | ----------------------------------- |
| `utilization.rpt`         | FPGA resource utilization report    |
| `report_timing.setup.rpt` | Setup timing analysis               |
| `report_timing.hold.rpt`  | Hold timing analysis                |
| `vpr_stdout.log`          | VTR execution log                   |
| `post_implementation.v`   | Post-implementation Verilog netlist |
| `post_implementation.sdf` | Timing delay annotation file        |
| Simulation Waveform       | Gate-level verification results     |

---

#  Learning Outcomes of Day5:-

Day 5 focused on implementing the **RVMYTH RISC-V processor** on a **custom SOFA FPGA Fabric** using the **OpenFPGA** and **VTR** framework.

The implementation successfully completed synthesis, placement, routing, timing analysis, and post-implementation verification. Resource utilization, timing reports, and simulation waveforms confirmed the correct operation of the processor while satisfying all timing constraints.

This experiment provided valuable hands-on experience with implementing a processor-based design on a custom FPGA architecture and strengthened the understanding of open-source FPGA design flows, timing optimization, and hardware verification.

By the end of Day 5, I gained practical experience in:

* Implementing a RISC-V processor on a custom FPGA fabric
* Executing the OpenFPGA and VTR implementation flow
* FPGA resource utilization analysis
* Applying SDC timing constraints
* Setup and hold timing verification
* FPGA routing and mapping analysis
* Post-implementation simulation
* Understanding large-scale FPGA processor implementations
* Interpreting implementation reports generated by OpenFPGA and VTR

---
