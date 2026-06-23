# FPGA_FABRIC_DESIGN_And_ARCITECTURE_WORKSHOP-byVSD
---

Its a Documentation of 10-Day's FPGA Fabric Design & Architecture Online Workshop conduted by VSD.This is the documentation of complete contents that i learned in this workshop like: implementation flow, analysis reports, simulations and FPGA architecture exploration performed during the FPGA Fabric Design and Architecture Workshop.
This documented work focuses on understanding the complete FPGA implementation flow starting from RTL design to FPGA architecture generation,placement, routing, timing analysis, utilization analysis, power
analysis, and post-implementation verification using open-source FPGA CAD tools.

The hands on flow of this document is:
-VTR (Verilog-to-Routing)
-VPR (Versatile Place and Route)
-OpenFPGA
-Vivado
-SOFA FPGA Fabric
-RISC-V Processor Core (RVMYTH)

---

## Workshop Overview

This workshop provided a comprehensive understanding of the complete FPGA design flow on demonstrating how a digital RTL design is transformed into a hardware implementation on an FPGA using open-source FPGA CAD tools.
This documententation is my learning and hands-on work completed during the **10-Day FPGA Fabric Design & Architecture Workshop**.Throughout this workshop, I explored the complete FPGA implementation process,That includes:

* RTL Design
* Functional Simulation
* FPGA Synthesis
* Technology Mapping
* Placement
* Routing
* Static Timing Analysis
* FPGA Resource Utilization Analysis
* Power Analysis
* FPGA Fabric Generation
* Post-Implementation Simulation

In addition to the implementation flow, this workshop also introduced the design and customization of FPGA architectures using the **SOFA FPGA** framework and the **OpenFPGA** ecosystem, providing practical exposure to modern open-source FPGA design methodologies.

---

## Tools/Software  Used

| Tool/Software Name | Purpose |
|------|---------|
| Vivado | RTL Simulation, Synthesis, Implementation, and FPGA Analysis |
| Verilog HDL | RTL Design and Hardware Description |
| VTR | FPGA CAD Flow |
| VPR | Placement and Routing |
| OpenFPGA | FPGA Fabric Generation and Architecture Exploration |
| SDC | Timing Constraints |

---

## Workshop Schedule

| Day | Description |
|:---:|-------------|
| [Day 1](Day1/README.md) | Introduction to FPGA Flow, Vivado Simulation and Basic Counter Design |
| [Day 2](Day2/README.md) | VTR Flow, FPGA Architecture, Timing Constraints, Routing and FPGA Reports |
| [Day 3](Day3/README.md) | Mythcore Processor Implementation, ILA Debugging and FPGA Analysis |
| [Day 4](Day4/README.md) | Post Synthesis Simulation, Primitive Models and Timing Verification |
| [Day 5](Day5/README.md) | RISC-V Core on Custom SOFA FPGA Fabric using OpenFPGA |

---

# Topics that covered in workshop
---
## FPGA Basics

- FPGA Architecture
- LUTs and Flip-Flops
- Routing Resources
- FPGA Logic Blocks
- Clock Networks

---

## RTL Design and Simulation

-Verilog HDL
-Testbench Creation
-RTL Functional Simulation
-Waveform Analysis

---

##FPGA Architecture Exploration

-EArch FPGA Architecture
-SOFA FPGA Fabric
-FPGA Resource Utilization
-Routing Congestion Analysis

---

## FPGA Architecture

- Configurable Logic Blocks (CLBs)
- Switch Boxes (SBs)
- Connection Boxes (CBs)
- Input/Output Blocks (IOBs)
- Routing Channels

---

##FPGA CAD Flow

-RTL Synthesis
-Technology Mapping
-Placement
-Routing
-Bitstream Generation

---

## Open-Source FPGA Flow

-FPGA Fabric Generation
-FPGA Architecture Mapping
-Netlist Generation
-Post Implementation Simulation

---

## Verification

- Functional Simulation
- Post-Synthesis Simulation
- Timing Verification
- Waveform Analysis

---

## Reports and Analysis

- Resource Utilization
- Timing Reports
- Power Analysis
- Routing Reports

---

##Timing Analysis

-Setup Timing
-Hold Timing
-Critical Path Analysis
-Timing Constraints using SDC

---
## Key Concepts 

Throughout this workshop, the following core FPGA design and implementation concepts were explored:

- **FPGA Fabric Architecture** :-In Understanding the internal organization of configurable logic blocks (CLBs), routing resources, and programmable interconnects.
- **Routing Architecture**0 :– Exploring switch boxes, connection boxes, and routing channels used to establish signal paths.
- **Placement and Routing (P&R)** :– Mapping synthesized logic onto FPGA resources and optimizing routing for performance.
- **Timing Closure** :– Analyzing setup and hold timing requirements to ensure reliable circuit operation.
- **FPGA Resource Utilization** :– Evaluating the usage of LUTs, Flip-Flops, BRAMs, DSP blocks, I/O pins, and other FPGA resources.
- **Power Estimation and Analysis** :– Studying static and dynamic power consumption and techniques for power optimization.
- **Processor Implementation on FPGA** :– Implementing and validating a RISC-V based processor on FPGA hardware.
- **FPGA Netlist Generation** :– Converting synthesized RTL designs into technology-specific FPGA netlists.
- **Open-Source FPGA Toolchain** :– Gaining hands-on experience with open-source FPGA CAD tools, including Yosys, VTR, VPR, OpenFPGA, and SOFA FPGA for synthesis, placement, routing, and FPGA fabric generation.

---

# FPGA Arhitectures

---

 ## EArch FPGA Architecture

The **EArch FPGA architecture** was used to explore the FPGA implementation flow, including routing resources, placement, timing analysis, and FPGA netlist generation.

---

## SOFA FPGA Implementation

The **SOFA FPGA** fabric was used with the **OpenFPGA** framework to implement the **RVMYTH RISC-V processor**. The implementation covered FPGA fabric generation, placement and routing, timing analysis, resource utilization, and post-implementation simulation.

## Outputs Generated

- FPGA Netlist
- Timing Report
- Simulation Results
- RTL Simulation Waveforms
- Hold Reports
- Setup Reports
- FPGA Utilization Reports
- Placement Reports
- Routing Reports
- Post Implementation Simulation
- FPGA Architecture Reports
- Power Analysis Reports

---

## Learning Outcomes

By the end of this workshop, I gained practical knowledge of:

- FPGA design and implementation flow
- FPGA architecture and fabric exploration
- RTL to FPGA implementation process
- Open-source FPGA CAD tools and frameworks
- Placement and routing techniques
- Timing analysis and timing closure
- FPGA resource utilization analysis
- RISC-V processor implementation on FPGA

---




