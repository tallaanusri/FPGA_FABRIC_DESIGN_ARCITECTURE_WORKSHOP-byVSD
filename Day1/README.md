# Introduction to FPGA Architecture and Vivado Design Flow
---

## Objective

The primary objective of Day 1 was to gain a basic fundamentals of FPGA architecture and the complete FPGA design flow using **Xilinx Vivado Design Suite**. 

A simple 4-bit counter was designed, simulated, synthesized, implemented, and programmed onto the **Basys 3 FPGA board** to understand each stage of the development process.

The topics covered during the Day1 sessions:

* Introduction to Field Programmable Gate Arrays (FPGAs)
* Comparison between FPGA and ASIC technologies
* Internal FPGA architecture
* Configurable Logic Blocks (CLBs), Look-Up Tables (LUTs), Flip-Flops, and routing resources
* Vivado design flow from RTL to Bitstream generation
* Behavioral simulation
* RTL elaboration
* Synthesis and implementation
* Timing analysis
* Device utilization analysis
* Power estimation
* Constraint file creation and pin mapping
* Virtual Input/Output (VIO)

---

# Introduction to FPGA

A **Field Programmable Gate Array (FPGA)** is a programmable integrated circuit whose hardware functionality can be configured by the user even after manufacturing. Unlike Application-Specific Integrated Circuits (ASICs), FPGAs provide flexibility by allowing the hardware configuration to be modified multiple times, making them highly suitable for rapid prototyping and hardware development.

FPGAs are used in applications such as:

* Digital Signal Processing (DSP)
* Embedded Systems
* Hardware Acceleration
* Artificial Intelligence and Machine Learning
* Aerospace and Defense Systems
* High-Performance Computing (HPC)
* Communication Systems

---

# FPGA vs ASIC

Both FPGA and ASIC technologies are widely used in digital hardware design, but they differ significantly in terms of flexibility, cost, and performance.

| FPGA                                | ASIC                                                   |
| ----------------------------------- | ------------------------------------------------------ |
| Reprogrammable after manufacturing  | Permanently fixed after fabrication                    |
| Suitable for rapid prototyping      | Long design and fabrication cycle                      |
| Lower initial development cost      | High non-recurring engineering (NRE) cost              |
| Flexible and reusable               | Optimized for maximum performance and power efficiency |
| Generates Bitstream for programming | Produces physical silicon layout for fabrication       |

---

# FPGA Architecture

An FPGA is composed of several programmable hardware resources interconnected through configurable routing networks. The primary components include:

* Configurable Logic Blocks (CLBs)
* Look-Up Tables (LUTs)
* Flip-Flops (FFs)
* Programmable Interconnects
* Input/Output (I/O) Blocks
* Block RAM (BRAM)
* DSP Slices

These resources collectively enable the implementation of both combinational and sequential digital circuits.

---

## Configurable Logic Blocks (CLBs)

The **Configurable Logic Block (CLB)** is the fundamental logic element inside an FPGA. It performs most of the user-defined logic operations.

Each CLB generally contains:

* Look-Up Tables (LUTs)
* Flip-Flops
* Carry Chains for arithmetic operations
* Multiplexers
* Local routing resources

CLBs can implement both combinational logic and sequential circuits efficiently.

---

## Look-Up Tables (LUTs)

A **Look-Up Table (LUT)** is the basic combinational logic element inside an FPGA. Instead of using discrete logic gates, a LUT stores the truth table of a logic function in memory.

- For an **N-input LUT**, the number of memory locations required is: [2^N]
- Each memory location corresponds to one possible input combination.

For example, a **3-input LUT** contains **8 memory entries**, allowing it to implement any Boolean function involving three variables.

---

# FPGA Design Flow

The FPGA implementation process using Vivado follows a sequence of design stages:

1. RTL Design using Verilog HDL
2. Testbench Development
3. Behavioral Simulation
4. RTL Elaboration
5. Synthesis
6. Constraint File (XDC) Creation
7. Placement and Routing (Implementation)
8. Timing Verification
9. Bitstream Generation
10. FPGA Programming

Each stage verifies the design before proceeding to the next, ensuring correct functionality and optimized hardware implementation.

---

# Basys 3 FPGA Development Board

The experiments were performed using the **Basys 3 FPGA Development Board**, which is based on the **Xilinx Artix-7 FPGA**.

### Major Hardware Features

| Component             | Function                       |
| --------------------- | ------------------------------ |
| Push Buttons          | User input signals             |
| Slide Switches        | Manual input selection         |
| LEDs                  | Display output states          |
| Seven-Segment Display | Numeric output display         |
| VGA Interface         | Video output                   |
| USB-JTAG Interface    | FPGA programming and debugging |
| 100 MHz Oscillator    | System clock source            |

The Basys 3 board provides an ideal platform for learning FPGA design and digital hardware implementation.

---

# Example – 4-Bit Up Counter Design

To understand the FPGA design flow, a **4-bit synchronous up counter** was implemented in Verilog HDL.

The design performs the following operations:

* Counts on every positive edge of the divided clock
* Resets to zero whenever the reset signal becomes active
* Displays the counter value using on-board LEDs
* Uses an internal clock divider to slow down counting for visible LED transitions

---

# Verilog Implementation

```verilog
module counter_clk_div(
    input clk,
    input rst,
    output reg [3:0] counter_out
);

reg div_clk;
reg [25:0] delay_count;

always @(posedge clk) begin
    if(rst) begin
        delay_count <= 26'd0;
        div_clk <= 1'b0;
    end
    else begin
        if(delay_count == 26'd212) begin
            delay_count <= 26'd0;
            div_clk <= ~div_clk;
        end
        else begin
            delay_count <= delay_count + 1;
        end
    end
end
always @(posedge div_clk) begin
    if(rst)
        counter_out <= 4'b0000;
    else
        counter_out <= counter_out + 1;
end

endmodule
```

---

# Behavioral Simulation

Behavioral simulation was performed before synthesis to verify the logical functionality of the design.

The simulation confirmed the following:

* Correct clock generation
* Proper counter increment operation
* Successful reset functionality
* Expected output waveform

## Simulation Output

<img width="940" height="486" alt="image" src="https://github.com/user-attachments/assets/456706e8-76f9-4326-ace5-82c53904f2f8" />

*Behavioral simulation waveform of 4-bit up counter in Vivado.*

---

# RTL Elaboration

RTL Elaboration converts the Verilog HDL description into a Register Transfer Level (RTL) schematic.

This stage helps verify:

* Module hierarchy
* Signal connectivity
* Register implementation
* Data flow
* Logical correctness before synthesis

### RTL Schematic

<img width="940" height="509" alt="image" src="https://github.com/user-attachments/assets/f7dc36a1-9bd2-4ff7-a69d-f282429c0aaa" />


*RTL elaborated schematic of the counter design.*

---

# Constraints and Pin Mapping

The FPGA pins were assigned using an **Xilinx Design Constraints (.xdc)** file.

The constraint file specifies:

* Clock pin assignment
* Reset pin assignment
* LED output pins
* I/O standards

Correct pin mapping ensures that the FPGA design interfaces properly with the external hardware available on the Basys 3 board.

## Constraint File

<img width="940" height="534" alt="image" src="https://github.com/user-attachments/assets/49e5f102-7d0c-445a-bc7d-977c337e64a7" />


### FPGA Package View

<img width="940" height="508" alt="image" src="https://github.com/user-attachments/assets/6d116368-aa34-4a35-a568-6161a491ee69" />


*FPGA package view showing physical pin assignments.*

---

# Synthesis

During synthesis, the HDL design is translated into FPGA-specific hardware primitives.

The synthesis process performs:

* Logic optimization
* Resource allocation
* Register inference
* LUT mapping
* Initial timing estimation

The synthesized netlist forms the basis for implementation.

---

# Timing Analysis

Timing analysis verifies whether all signal paths satisfy the required clock timing constraints.

The two primary timing checks are:

### Setup Time Analysis

Setup timing ensures that data reaches the destination register before the active clock edge.

The setup timing condition is expressed as:

[T_{CQ}+T_{Logic}<T_{Clock}-T_{Setup}]

---

### Hold Time Analysis

Hold timing ensures that data remains stable for the required duration after the clock edge, preventing incorrect data capture.

---

### Slack

Slack is defined as:

[Slack = Required\ Time - Arrival\ Time]

* **Positive Slack** → Timing requirements satisfied
* **Negative Slack** → Timing violation exists

### Timing Summary

<img width="940" height="178" alt="image" src="https://github.com/user-attachments/assets/ec86bbd1-37fb-46ef-8bce-af7fc6712ddf" />


*Timing analysis summary.*

---

# Device Utilization

After implementation, Vivado generates a resource utilization report indicating the FPGA resources consumed by the design.

The report includes:

* Look-Up Tables (LUTs)
* Flip-Flops (Registers)
* Input/Output Buffers
* Block RAM
* DSP Resources
* Clock Buffers

This information helps evaluate design efficiency and available hardware resources.

### Utilization Report

<img width="800" height="318" alt="image" src="https://github.com/user-attachments/assets/cb63ddd1-a63c-4668-802f-bbf1ef172858" />


*FPGA resource utilization report after implementation.*

---

# Power Analysis

Vivado estimates the power consumption of the implemented design by considering switching activity and hardware resource usage.

The report provides:

* Static Power
* Dynamic Power
* Clock Power
* Signal Power
* I/O Power

Power analysis is essential for optimizing energy-efficient FPGA designs.

### Power Report

<img width="778" height="384" alt="image" src="https://github.com/user-attachments/assets/606e0804-d336-4ec0-b000-e4b85e7c67bb" />


*Power analysis report generated after FPGA implementation.*

---

# Virtual Input/Output (VIO)

Virtual Input/Output (VIO) is an integrated debugging feature provided by Vivado Hardware Manager.

It allows designers to monitor and control internal FPGA signals in real time without requiring additional external hardware connections.

Common applications of VIO include:

* Internal signal observation
* Real-time debugging
* Functional verification
* Runtime testing of digital designs

---

# Learning Outcomes

By the end of Day 1, the following concepts were understood and implemented:

* Fundamentals of FPGA architecture
* Difference between FPGA and ASIC design methodologies
* Structure and operation of CLBs, LUTs, and Flip-Flops
* Complete FPGA implementation flow using Vivado
* Behavioral simulation and RTL verification
* Synthesis and implementation process
* Timing analysis and slack interpretation
* Device utilization and power estimation
* FPGA pin assignment using XDC constraints
* Practical implementation of a Verilog-based counter on the Basys 3 FPGA board
