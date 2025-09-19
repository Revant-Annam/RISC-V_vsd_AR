# RISC-V VSD AR - TASK 1

## Summary of the First Video  

The video explains the flow of designing a **System-on-Chip (SoC)**, from a high-level model to a fabricated chip.  
Throughout the flow, there are **four output stages** produced by the design stages and the C testbench.  
These four outputs must **match** to ensure that the chip is fully functional.  

---

## Output Stages  

### 🟢 Chip Modelling (O0 and O1)  
- The required specifications are written in a **C model**.  
- The **C testbench** is run through the specs using **GCC (C compiler)**.  
- Purpose: Verify whether the specifications are **feasible**.  

---

### 🟡 RTL Architecture (O2)  
- Specifications are converted into **RTL (Verilog)** → the soft copy of the hardware.  
- The **C testbench** is run on the RTL.  
- Result: **O2**.  

---

### 🔵 SoC Integration (O3)  
- RTL is divided into two major parts:  
  1. **Processor** – Must be synthesizable RTL (convertible into gate-level netlist).  
  2. **Peripherals** – Include:  
     - **Macros**: RTL codes instantiated multiple times, must be synthesizable.  
     - **Analog IPs**: (e.g., ADCs, PLLs) modeled at the transistor level → must be functional RTL.  
- SoC integration is performed using **GPIOs**, through which the **C testbench** is applied.  
- Result: **O3**.  

---

### 🔴 RTL2GDS and Fabrication (O4)  
- From the synthesized netlist, the **RTL-to-GDS flow** involves physical design steps:  
  - Floorplanning  
  - Placement  
  - Routing  
  - CTS (Clock Tree Synthesis)  
- Final result: **GDSII file**.  
- Before fabrication:  
  - Run **DRC (Design Rule Check)**  
  - Run **LVS (Layout vs. Schematic)**  
- The **GDSII** is sent for **tapeout**.  
- Fabricated chips (tape-in) are tested by running the **C testbench** via peripherals.  
- Result: **O4**.  

---

## Key Insight  

This SoC design flow can be **reused** for different products (e.g., iWatch, Arduino boards, TV panels, AC applications), depending on the **target operating frequency**.  
