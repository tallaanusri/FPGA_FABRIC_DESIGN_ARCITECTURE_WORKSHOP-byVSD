# Mythcore Processor Implementation and FPGA Analysis

## Objective

The objective of Day 3 was to implement and analyze a **Mythcore RISC-V processor** on an FPGA using the Xilinx Vivado design flow. Unlike the simple counter design from previous experiments, this session focused on a more complex processor architecture to understand FPGA implementation, debugging, and performance analysis.

The experiment included:

* RTL Simulation
* Synthesis
* RTL Schematic Generation
* Package and Pin Mapping
* Integrated Logic Analyzer (ILA)
* Timing Analysis
* Resource Utilization Analysis
* Power Analysis
* Hardware Verification

---

# Introduction

A **Mythcore RISC-V processor** design was implemented to study how a processor-based RTL design is synthesized, placed, routed, and analyzed on an FPGA.

Compared to a simple sequential circuit, processor implementations require significantly more FPGA resources and involve complex routing, timing paths, and debugging techniques.

---

# Design Files

The following source files were used during implementation:

* `mythcore_test.v`
* `mythcore_test_gn.v`

### Reference Repository

> https://github.com/shivanishah269/risc-v-core/tree/master

---

# FPGA Design Flow

The complete FPGA implementation flow followed during this experiment is illustrated below:

```text
RTL Design
     │
     ▼
Functional Simulation
     │
     ▼
Synthesis
     │
     ▼
RTL Schematic
     │
     ▼
Implementation
 ├── Placement
 ├── Routing
 └── Timing Analysis
     │
     ▼
Bitstream Generation
     │
     ▼
Hardware Debugging (ILA)
```

---

# RTL Simulation

Before synthesis, the Mythcore processor was simulated to verify its functional behavior.

### Observations

* Clock signal toggled correctly.
* Reset initialized the processor successfully.
* Processor outputs responded according to execution.
* Functional behavior matched the expected design.

## RTL Simulation Output

<img width="940" src="https://github.com/user-attachments/assets/c733cc34-ec14-4639-bb95-c7be878720ff"/>

*Figure: RTL simulation waveform of the Mythcore processor.*

---

# Schematic Generation

After synthesis, Vivado generated the RTL schematic representing the processor architecture.

The schematic contains:

* Lookup Tables (LUTs)
* Flip-Flops (FFs)
* Processor datapath
* Internal routing
* Logic interconnections

Since Mythcore is significantly larger than the counter design, the generated schematic is much more complex.

## RTL Schematic

<img width="940" src="https://github.com/user-attachments/assets/5e46f9d8-6b2e-40ba-8a09-0940976834af"/>

*Figure: RTL schematic generated after synthesis.*

---

#  FPGA Package View

The Package View provides a physical representation of FPGA resources and I/O assignments.

It helps visualize:

* FPGA package layout
* I/O pin mapping
* Resource allocation
* Routing regions

## Package View

<img width="940" src="https://github.com/user-attachments/assets/1fb84060-2fb8-4aba-8707-3b6410bceb27"/>

*Figure: FPGA package and pin mapping.*

---

# Integrated Logic Analyzer (ILA)

## Objective

The **Integrated Logic Analyzer (ILA)** was used to monitor internal FPGA signals in real time.

Unlike simulation, ILA captures live hardware signals after programming the FPGA, making it an effective tool for debugging processor designs.

---

## ILA Instantiation

```verilog
ila_0 your_instance_name (
    .clk(clk),

    .probe0(reset),
    .probe1(out)
);
```

---

## Connected Signals

| Signal  | Description  |
| ------- | ------------ |
| `clk`   | System clock |
| `reset` | Reset signal |
| `out`   | Output probe |

---

# XDC Constraint File

The XDC constraint file defines the physical implementation details for the FPGA.

It includes:

* Clock pin assignment
* Reset pin assignment
* I/O standards
* Clock frequency
* Debug Hub configuration

## Constraints

```tcl
set_property PACKAGE_PIN V6 [get_ports clk]
set_property IOSTANDARD LVCMOS33 [get_ports clk]

set_property PACKAGE_PIN P2 [get_ports reset]
set_property IOSTANDARD LVCMOS33 [get_ports reset]

create_clock -period 10.000 -name clk -waveform {0.000 5.000} [get_ports clk]

set_property C_CLK_INPUT_FREQ_HZ 300000000 [get_debug_cores dbg_hub]
set_property C_ENABLE_CLK_DIVIDER false [get_debug_cores dbg_hub]
set_property C_USER_SCAN_CHAIN 1 [get_debug_cores dbg_hub]

connect_debug_port dbg_hub/clk [get_nets clk_IBUF_BUFG]
```

---

# Resource Utilization Analysis

The utilization report summarizes FPGA resource consumption after synthesis and implementation.

### Resources Used

* Lookup Tables (LUTs)
* LUTRAM
* Flip-Flops
* Block RAM (BRAM)
* I/O Blocks

Compared to the previous counter implementation, the Mythcore processor required substantially more FPGA resources.

## Utilization Report

<img width="778" src="https://github.com/user-attachments/assets/9a21a045-3139-4f65-9d5a-92367041cb9c"/>

*Figure: FPGA utilization report.*

---

# Timing Analysis

Timing analysis was performed after placement and routing to verify that all timing constraints were satisfied.

### Parameters Verified

* Setup Timing
* Hold Timing
* Clock Synchronization
* Timing Slack

<img width="940" src="https://github.com/user-attachments/assets/f54df0f0-8c2e-4235-8dba-cf1e4f0b3ef1"/>

*Figure: Timing analysis summary.*

---

# Power Analysis

Power estimation was performed after implementation.

The report included:

* Dynamic Power
* Static Power
* Clock Power
* Logic Power
* Signal Power

### Observations

* Clock networks consumed a significant portion of dynamic power.
* Logic switching activity increased total power consumption.
* Static FPGA power remained nearly constant.

## Power Report

<img width="776" src="https://github.com/user-attachments/assets/dcc1ed07-e60d-4c0d-a01b-da582730bece"/>

*Figure: Power analysis report.*

---
# Design Analysis

## Increased Design Complexity

Compared with the simple counter implementation:

* Routing complexity increased significantly.
* Larger FPGA resource utilization was observed.
* More critical timing paths were generated.
* Implementation time increased.

---

## FPGA Resource Usage

The Mythcore processor utilized:

* A large number of LUTs
* Multiple Flip-Flops
* Additional routing resources
* More clock distribution resources

---

## Importance of Timing Constraints

Proper timing constraints are essential for successful FPGA implementation.

Without timing constraints:

* Timing violations may occur.
* Placement quality decreases.
* Routing delays increase.
* Timing closure becomes difficult.

Well-defined constraints enable FPGA tools to optimize placement and routing effectively.

---

# Day 3 Learning Outcomes

 The Day 3 focused on implementing a **Mythcore RISC-V processor** on an FPGA using the Vivado design flow.The implementation included RTL simulation, synthesis, schematic generation, package mapping, ILA-based debugging, timing analysis, utilization analysis, and power estimation.

It provided valuable hands-on experience in implementing and analyzing a processor-based FPGA design while strengthening the understanding of FPGA architecture, implementation flow, hardware debugging, and performance optimization.

 The following things are gained by me in Day3 of workshop:- 

* Implementing a processor-based RTL design on FPGA
* Functional verification using RTL simulation
* Understanding synthesized RTL schematics
* FPGA package and pin mapping
* Real-time debugging using Integrated Logic Analyzer (ILA)
* FPGA resource utilization analysis
* Timing analysis and timing closure
* Power estimation and analysis
* Working with large-scale FPGA designs

---

