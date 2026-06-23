# Exploring OpenFPGA, VPR & VTR Flow

## Objective

The objective of Day 2 was to understand the complete **open-source FPGA CAD flow**, starting from Verilog RTL and progressing through synthesis, placement, routing, timing analysis, post-synthesis simulation, and power estimation using the following tools:

- OpenFPGA
- VPR (Versatile Place and Route)
- VTR (Verilog-to-Routing)

---

# Introduction

Unlike vendor-specific FPGA tools such as Vivado or Quartus, the **OpenFPGA ecosystem** provides an open-source framework for FPGA architecture exploration and CAD flow research.

The workflow covered during this session includes:

```
Verilog RTL
      │
      ▼
ODIN II (Synthesis)
      │
      ▼
ABC (Logic Optimization)
      │
      ▼
VPR
 ├── Packing
 ├── Placement
 ├── Routing
 └── Timing Analysis
      │
      ▼
Reports & FPGA Visualization
```

---

# OpenFPGA

OpenFPGA is an open-source framework designed for FPGA architecture research and development.

## OpenFPGA Features

- FPGA architecture exploration
- FPGA fabric generation
- Bitstream generation
- CAD automation
- Design verification
- Verilog-to-Bitstream flow

---

## VPR (Versatile Place and Route)

VPR is responsible for implementing the synthesized design onto an FPGA architecture.

## Major Stages

1. Packing
2. Placement
3. Routing
4. Timing Analysis

---

# Running VPR

```bash
$VTR_ROOT/vpr/vpr \
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
<design>.blif \
--route_chan_width 100 \
--disp on
```

---

#  VPR Design Flow

 1. Packing :- Groups logic primitives into configurable logic blocks (CLBs).

 2. Placement :- Assigns logic blocks to physical locations inside the FPGA grid.

 3. Routing :- Creates routing paths between placed logic blocks.

 4. Timing Analysis :- Calculates setup time, hold time, critical paths, and maximum operating frequency.

---

# VPR GUI Visualization

The VPR graphical interface allows visualization of:

- FPGA Grid
- Logic Blocks
- Routing Resources
- Nets
- Critical Paths

## Output

<img width="940" height="771" alt="image" src="https://github.com/user-attachments/assets/af78fc1d-c4be-40b9-9910-12a2a8a96c13"/>

*Figure: VPR placement and routing visualization.*

---

# EArch FPGA Architecture Analysis

The FPGA architecture described in **EArch.xml** was analyzed using VPR.

The generated architecture consists of:

- Logic Blocks
- Routing Channels
- Switch Blocks
- Interconnect Resources
- FPGA Grid

---

#  Nets Analysis

The routing report provides information about:

- Source nodes
- Destination nodes
- Routing paths
- Resource utilization

<img width="940" height="720" src="https://github.com/user-attachments/assets/709a35a3-7938-491d-8c06-9a9130a1cfbf"/>

*Figure: Routed net connections.*

---

# Logical Connections

Logical connections illustrate how signals travel between FPGA logic blocks.

<img width="940" height="709" src="https://github.com/user-attachments/assets/cdf823c2-2ea1-4c48-85c4-6ad9fe6846de"/>

*Figure: Logical routing connections.*

---

# Critical Path Analysis

The critical path determines the slowest signal propagation path inside the FPGA.

It is used to calculate:

- Worst-case delay
- Maximum operating frequency
- Timing bottlenecks

<img width="940" height="711" alt="image" src="https://github.com/user-attachments/assets/44e6e49c-3f2b-412e-81ba-1414fa151c5b" />

*Figure: Critical path report.*

---

# Routing Utilization

Routing utilization provides information regarding:

- Channel occupancy
- Congestion
- Routing efficiency
- FPGA resource usage

<img width="940" height="721" src="https://github.com/user-attachments/assets/42dc365d-d40b-463f-8e63-024111186a4c"/>

*Figure: Routing utilization.*

---

# Timing Constraints

Timing constraints were specified using an **SDC (Synopsys Design Constraints)** file.

```tcl
create_clock -period 10 -name pclk
set_input_delay -clock pclk -max 0 [get_ports {*}]
set_output_delay -clock pclk -max 0 [get_ports {*}]
```

Run VPR with constraints:

```bash
$VTR_ROOT/vpr/vpr \
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
tseng.blif \
--route_chan_width 100 \
--sdc_file tseng.sdc \
--disp on
```

---

# Setup Timing

Setup timing ensures data reaches the destination register before the active clock edge.

After applying constraints:

- Improved setup slack
- Reduced timing violations
- Successful timing closure

<img width="940" height="486" src="https://github.com/user-attachments/assets/d1ab7bd8-25ce-4826-8ce1-c80a23df133c"/>

---

# Hold Timing

Hold timing verifies that data remains stable after the clock edge.

<img width="940" height="194" src="https://github.com/user-attachments/assets/eb897b40-8f6c-4ddf-98ee-b4b0dee00bfa"/>

---

# VTR (Verilog-to-Routing)

VTR is a complete open source FPGA CAD flow 

The VTR flow :
- RTL Elaboration
- Synthesis
- Technology Mapping
- Packing
- Placement
- Routing
- Timing Analysis

  Tools Involved:-
  

| Tool        | Purpose                                          |
| ----------- | ------------------------------------------------ |
| **ODIN II** | Verilog elaboration and synthesis                |
| **ABC**     | Logic optimization and technology mapping        |
| **VPR**     | Packing, Placement, Routing, and Timing Analysis |

Overall design flow:

```
RTL
 │
 ▼
ODIN II
 │
 ▼
ABC
 │
 ▼
VPR
 │
 ▼
Reports
```

---

# Running VTR

```bash
$VTR_ROOT/vtr_flow/scripts/run_vtr_flow.py \
counter.v \
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
-temp_dir . \
-route_chan_width 100
```

---

# Counter Design

A 4-bit synchronous up-counter was used to demonstrate the VTR implementation flow.

## Counter Verilog Code

```verilog

module up_counter (
out      ,
enable   ,
clk      ,
reset
);

output [3:0] out;

input enable, clk, reset;

reg [3:0] out;

always @(posedge clk)

if (reset) begin
    out = 4'b0 ;
end

else if (enable) begin
    out = out + 1;
end

endmodule

/*Note: Once you run ./a.out, it will keep running infinitely, because it is in an always block. You need to hit Ctrl + Z to stop it, else, the vcd will become a large file and will never end.
*/

```
# Generated Output Files

After successful implementation, VTR generates several useful output files.

| File              | Description             |
| ----------------- | ----------------------- |
| `.blif`           | Synthesized netlist     |
| `.net`            | Logical netlist         |
| `.place`          | Placement information   |
| `.route`          | Routing information     |
| Timing Reports    | Timing analysis results |
| Routing Reports   | Routing utilization     |
| Placement Reports | Placement statistics    |

---

# Post-Synthesis Simulation

Post-synthesis simulation verifies the synthesized netlist together with routing delays.

Generated files:

- `up_counter_post_synthesis.v`
- `up_counter_post_synthesis.sdf`

Generate the post-synthesis netlist using:

```bash
$VTR_ROOT/vpr/vpr \
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
counter.pre-vpr.blif \
--gen_post_synthesis_netlist on
```

### Simulation Waveform

<img width="871" height="278" src="https://github.com/user-attachments/assets/b656b2b0-47f4-4870-b87b-b8f8eaeb0307"/>

---

# Nets & Logical Connections

The generated reports provide detailed information about:

* Net connectivity
* Routing utilization
* Critical paths
* Logical interconnections

## Nets Report

<img width="940" src="https://github.com/user-attachments/assets/fcb0ac03-e295-4f8c-a568-46af29977e44"/>

*Figure: Generated net connections.*

---

## Logical Connections

<img width="940" src="https://github.com/user-attachments/assets/f27bb6e5-088c-49f3-8603-83bcca565393"/>

*Figure: Logical routing connections generated by VPR.*

---

#  Critical Path Analysis

Critical path analysis identifies the **longest delay path** in the design, which determines the maximum operating frequency.

### Parameters Analyzed

* Worst-case delay
* Maximum operating frequency
* Critical timing path

## Critical Path Report

<img width="940" src="https://github.com/user-attachments/assets/0cb2e8be-232b-4ce8-9d5f-af98a2b580e7"/>

*Figure: Critical timing path after routing.*

---

# Timing Constraints

Timing constraints were specified using an **SDC (Synopsys Design Constraints)** file.

The constraints define:

* Clock period
* Input delay
* Output delay

## SDC Constraint File

```tcl
create_clock -period 10 up_counter_clk
set_input_delay  -clock up_counter_clk -max 0 [get_ports {*}]
set_output_delay -clock up_counter_clk -max 0 [get_ports {*}]
```

---

# Running VPR with Constraints

```bash
$VTR_ROOT/vpr/vpr \
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
counter.pre-vpr.blif \
--route_chan_width 100 \
--sdc_file counter.sdc
```

---

# Setup Timing Analysis

After applying the timing constraints:

* Improved setup slack
* Reduced timing violations
* Successful timing closure

<img width="940" src="https://github.com/user-attachments/assets/71a1a2fa-7149-4880-9904-13b02eb13a0b"/>

*Figure: Setup timing report after applying timing constraints.*

---

# Hold Timing Analysis

Hold timing verifies that data remains stable after the active clock edge.

<img width="940" src="https://github.com/user-attachments/assets/702ba1e6-d8ab-4793-a580-681d6d97afb0"/>

*Figure: Hold timing report.*

---

# Post-Synthesis Simulation

Post-synthesis simulation validates the synthesized design using the generated netlist and timing information.

### Generated Files

* `up_counter_post_synthesis.v`
* `up_counter_post_synthesis.sdf`

---

#  Generate Post-Synthesis Netlist

```bash
$VTR_ROOT/vpr/vpr \
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
counter.pre-vpr.blif \
--gen_post_synthesis_netlist on
```

---

#  Testbench

The following testbench was used for gate-level simulation.

```verilog
`timescale 1ns/1ps

module upcounter_testbench();

reg clk, reset, enable;
wire [3:0] out;

up_counter dut(
    .\up_counter^enable(enable),
    .\up_counter^clk(clk),
    .\up_counter^reset(reset),
    .\up_counter^out~0(out[0]),
    .\up_counter^out~1(out[1]),
    .\up_counter^out~2(out[2]),
    .\up_counter^out~3(out[3])
);

initial
    $sdf_annotate("up_counter_post_synthesis.sdf", dut);

initial begin
    clk = 0;
    enable = 0;
    reset = 1;

    #20;

    reset = 0;
    enable = 1;
end

always #5 clk = ~clk;

endmodule
```

---

# Post-Synthesis Waveform

The waveform verifies:

* Gate-level functionality
* Routing delays
* Timing correctness

<img width="871" src="https://github.com/user-attachments/assets/b656b2b0-47f4-4870-b87b-b8f8eaeb0307"/>

*Figure: Post-synthesis simulation waveform.*

---

#  Power Analysis

Power estimation was performed using the VTR power analysis flow.

### Estimated Parameters

* Dynamic Power
* Routing Power
* Clock Power
* Leakage Power

## Command

```bash
$VTR_ROOT/vtr_flow/scripts/run_vtr_flow.py \
counter.v \
$VTR_ROOT/vtr_flow/arch/timing/k6_frac_N10_mem32K_40nm.xml \
-power \
-temp_dir . \
-route_chan_width 100
```

---

## stdout.log

<img width="940" src="https://github.com/user-attachments/assets/15846909-5252-489f-a236-85ffe7b730ce"/>

*Figure: Power analysis execution log.*

---

## Power Report

<img width="940" src="https://github.com/user-attachments/assets/a3728e51-12e1-46be-b4a8-7c16d556aec0"/>

*Figure: Estimated power consumption report.*

---

#  Generated Reports

The VTR flow generates the following reports:

* Timing Reports
* Placement Reports
* Routing Reports
* Power Reports
* Post-Synthesis Netlists
* `.net` Files
* `.place` Files
* `.route` Files
* `.blif` Files

---

#  Frequently Used Commands

## Run VTR Flow

```bash
$VTR_ROOT/vtr_flow/scripts/run_vtr_flow.py \
counter.v \
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
-temp_dir . \
-route_chan_width 100
```

## Launch the VPR GUI

```bash
$VTR_ROOT/vpr/vpr \
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
counter.pre-vpr.blif \
--route_chan_width 100 \
--disp on
```

## Run VPR with Timing Constraints

```bash
$VTR_ROOT/vpr/vpr \
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
counter.pre-vpr.blif \
--route_chan_width 100 \
--sdc_file counter.sdc
```

## Generate Post-Synthesis Netlist

```bash
$VTR_ROOT/vpr/vpr \
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
counter.pre-vpr.blif \
--gen_post_synthesis_netlist on
```

---

# Learning Outcomes

By the end of Day 2, I gained hands-on experience with:

- Understanding the OpenFPGA ecosystem
- FPGA implementation using VPR
- FPGA architecture exploration
- Packing, Placement, and Routing
- Timing Analysis (Setup & Hold)
- Critical Path Analysis
- SDC Timing Constraints
- VTR implementation flow
- Post-Synthesis Simulation
- FPGA Power Analysis
- Interpretation of generated reports and routing data

---
````
