# Design-of-a-4-to-1-Multiplexer-Based-Bus-System-for-Transferring-4-Bit-Data-Between-Registers

# Abstract 
In modern digital computer architecture, efficient data transfer between internal processor registers is critical to overall system performance. Connecting every register directly to every other register using dedicated point-to-point wires creates severe wiring congestion, increases hardware cost, and introduces signal propagation delays. The objective of this mini project is to design and verify a collision-free common bus system capable of transferring 4-bit parallel binary data among four source registers (R0, R1, R2, and R3) to a destination register (RD) using 4-to-1 multiplexers.  
The methodology involves organizing four 4-to-1 multiplexers (MUX0 to MUX3) in parallel, where each multiplexer handles one specific bit position across all four source registers. Two shared selection lines (S1 and S0) serve as control inputs to select which source register’s 4-bit word is placed onto the common 4-bit bus (B3 B2 B1 B0). When the load-enable signal is active, the destination register RD latches the bus data on the active clock edge. The circuit was modeled and tested using digital logic simulation tools (Logisim Evolution) and standard TTL IC specifications (IC 74153 and IC 74173). The results confirm deterministic, contention-free data routing across all selection states. 
# Table of Contents
| Nos | Title | 
| :--- | :--- |
| *1.* | Introduction
| *2.* | Problem Statement
| *3.* | Objectives
| *4.* | Literature Survey / Existing System
| *5.* | System Requirements
| *6.* | System Design (Block Diagram)
| *7.* | Implementation (Gate-Level Logical Circuit Diagram, Logic Equation, Procedure)
| *8.* | Results & Discussion (Truth Table, Observation Table, Result)
| *9.* | Conclusion
| *10.* | References
| *11.* | Appendix
# 1. Introduction
**1.1 Aim**
To design a 4-to-1 multiplexer-based bus system for transferring 4-bit data between registers using selection lines, and to verify data transfer by loading and observing register contents.
**1.2 Components Required** 
- Four 4-bit source registers: R0, R1, R2 and R3.
- Four 4-to-1 multiplexers (one for each data bit).
- Two selection lines: S1 and S0.
- One 4-bit destination register RD, a clock pulse and a load-enable signal.
- Digital logic simulator such as Logisim Evolution, or equivalent digital logic components.
**1.3 Theory**
A multiplexer (MUX) is a combinational circuit that selects one of several input signals and forwards the selected input to a
single output. Four 4-bit source registers share a common 4-bit bus. The two selection lines select one source register at a time.
Four 4-to-1 multiplexers operate in parallel—one per bit position. The four outputs form the common 4-bit bus. The destination
register loads the bus value when load-enable is active and the required clock edge occurs; otherwise, it retains its previous
value.
# 2. Problem Statement 
In a digital system comprising n registers, establishing direct point-to-point transfer paths between every register pair requires n(n − 1) separate multi-bit connections. As the register count and data word size grow, direct interconnection introduces critical hardware drawbacks:  
- **Excessive Wiring Congestion:** High number of physical tracks increases circuit board area and routing complexity.  
- **Increased Hardware Cost & Power Consumption:** Additional logic gates and buffers draw higher static and dynamic power.  
- **Bus Contention Risk:** Without strict selection control, multiple registers attempting to drive shared lines simultaneously can cause signal corruption or hardware damage.
   
Therefore, there is a need to design a simplified, deterministic, and collision-free 4-bit common bus system that uses 4-to-1 multiplexers and minimal selection lines (S1 and S0) to cleanly route data from one of four source registers (R0–R3) to a destination register (RD).  
# 3. Objectives  
The primary objectives of this Computer Organisation mini project are:  
- To design a 4-to-1 multiplexer-based common bus system capable of transferring 4-bit parallel data between registers using two shared selection lines (S1 and S0).  
- To understand and apply Register Transfer Language (RTL) micro-operations and parallel multiplexer organization in CPU datapath design.  
- To construct and analyze the gate-level AND–OR logic realization of a single-bit 4-to-1 multiplexer slice.  
- To verify accurate data transfer from source registers (R0, R1, R2, R3) to the destination register (RD) under active load-enable and clock-edge conditions using truth tables and logic simulation. 
# 4. Literature Survey / Existing System 
Several bus and interconnection topologies have been studied in computer architecture to handle inter-register data transfers:   
- **Point-to-Point Dedicated Wiring:** Early digital circuits connected registers directly using dedicated gate networks. Although latency is low, the quadratic growth in wiring makes it unfeasible for multi-register processors.  
- **Three-State (Tri-State) Buffer Bus:** Widely used in external system buses, tri-state buffers output logic 0, logic 1, or a high-impedance (Z) state. However, slight timing skews in enable signals can lead to bus contention, where two buffers drive the bus at the same time. 
- **Multiplexer-Based Common Bus (Proposed System):** As presented in standard computer architecture texts (e.g., M. Morris Mano), a MUX-based bus eliminates bus contention entirely. Because each multiplexer inherently routes only one selected input line based on S1 and S0, it guarantees safe and predictable internal register transfers. 
# 5. System Requirements 
**Hardware Requirements** 
- **Source Registers:** Four 4-bit source registers (R0, R1, R2, and R3) — e.g., 4 × IC 74173 (4-bit D-type registers). 
- **Multiplexers:** Four 4-to-1 multiplexers (MUX0, MUX1, MUX2, MUX3 — one for each data bit) — e.g., 2 × IC 74153 (Dual 4-to-1 Line Multiplexers). 
- **Destination Register:** One 4-bit destination register (RD) with clock pulse and load-enable control signal (1 × IC 74173). 
- **Control & Display Components:** Two selection switches (S1 and S0), DIP switches for binary input, 4 LEDs with 220Ω resistors, +5V DC power supply, and breadboard.

**Software Requirements** 
- **Digital Logic Simulator:** Logisim Evolution (or equivalent digital circuit simulator such as Proteus / Multisim). 
- **HDL Verification Tool (Optional):** Icarus Verilog / Xilinx Vivado for behavioral simulation. 
- **Operating System:** Windows 10/11 or Linux (Minimum 4 GB RAM, Dual-Core Processor). 

# 6. System Design
**6.1 Block Diagram**
The block diagram shows the full data path from the source registers through the multiplexer bank and common bus to the destination register.

```text
+------------------+       +--------------------+       +--------------+       +----------------------+

| SOURCE REGISTERS |       |    4 x 4-to-1 MUX  |       |  COMMON BUS  |       | DESTINATION REGISTER |
|     R0 (4 bits)  |       | one multiplexer    |       |  B3 B2 B1 B0 |       |      RD (4 bits)     |
|     R1 (4 bits)  | ----> |     per bit        | ----> |              | ----> |  load enable + clock |
|     R2 (4 bits)  |       | shared S1, S0 lines|       |              |       |                      |
|     R3 (4 bits)  |       +--------------------+       +--------------+       +----------------------+
+------------------+      s1----selection
                          s0----control        
```
BLOCK DIAGRAM: 4-BIT MULTIPLEXER BUS SYSTEM
<img width="2894" height="3030" alt="1000188337" src="https://github.com/user-attachments/assets/15bc165f-a516-4ad2-9903-337fdc550740" />
Here A=R0, B=R1, C=R2, D=R3
<img width="993" height="654" alt="1000188336" src="https://github.com/user-attachments/assets/c1038e2d-4564-46c0-883b-aceab277519e" />
# 7. Implementation
**7.1 Gate-Level Logical Circuit Diagram**
The following circuit shows the standard AND–OR realization of one bit of a 4-to-1 multiplexer. Each AND gate forms
one product term, and the OR gate combines the terms. S1' and S0' are generated using NOT gates. The one-bit circuit is repeated for all four bit positions.
GATE-LEVEL LOGIC CIRCUIT FOR ONE BIT OF A 4-to-1 MUX
<img width="3060" height="4080" alt="1000188354" src="https://github.com/user-attachments/assets/23b1046a-d1f4-4449-87a0-e867fc05edd2" />
<img width="867" height="582" alt="1000188357" src="https://github.com/user-attachments/assets/82e40c4a-ad3a-4d45-8c65-7fdfff485cf6" />

The OR gate combines all four product terms to produce the selected output bit B[i].
Repeat this one-bit circuit for i = 3, 2, 1 and 0 to form the 4-bit bus.
**7.2 Logic Equation**
For each bit position i = 0, 1, 2, 3:
B[i] = (S1' S0' R0[i]) + (S1' S0 R1[i])+ (S1 S0' R2[i]) + (S1 S0 R3[i])
Here, ' means NOT, adjacency means AND, and + means OR. The destination register loads RD ← B when load-enable is active on the active clock edge.
**7.3 Procedure**
1. Create four 4-bit source registers and one 4-bit destination register.
2. Connect the corresponding bits of the four source registers to four 4-to-1 multiplexers.
3. Connect S1 and S0 to the selection inputs of all four multiplexers.
4. Connect the four multiplexer outputs to the common 4-bit bus.
5. Connect the bus to the destination register input.
6. Connect the clock and load-enable signal to the destination register.
7. Load different binary values into the source registers.
8. Change the selection lines, observe the bus, enable loading and apply a clock edge.
9. Verify that the destination register contains the selected source register's data.
# 8. Result & Discussion    
**8.1 Selection-Line Truth Table**
| S1 | S0 | Selected register | Bus output |
| :--- | :--- | :--- | :--- |
| 0 | 0 | R0 | B = R0 |
| 0 | 1 | R1 | B = R1 |
| 1 | 0 | R2 | B = R2 |
| 1 | 1 | R3 | B = R3 |

**8.2 Example Input Values**
| Source register | 4-bit binary data |
| :--- | :--- |
| R0 | 1010 |
| R1 | 1100 |
| R2 | 0111 |
| R3 | 1001 |

**8.3 Observation Table**
| S1S0 | Selected register | Bus data | RD after clock |
| :--- | :--- | :--- | :--- |
| 00 | R0 | 1010 | 1010 |
| 01 | R1 | 1100 | 1100 |
| 10 | R2 | 0111 | 0111 |
| 11 | R3 | 1001 | 1001 |

Note: These are sample test values. Record the actual values observed during simulation or hardware testing.
**8.4 Result**
A 4-to-1 multiplexer-based bus system was designed. The selection lines select one of four source registers, and its 4-bit data is transferred through the common bus to the destination register when loading is enabled.
# 9. Conclusion
**9.1 Summary of Achievements** 
In this project, a 4-to-1 multiplexer-based common bus system for transferring 4-bit data between registers was designed, modeled at both block and gate levels, and verified against theoretical truth tables. The circuit reliably routes 4-bit binary words from four source registers (R0–R3) to a destination register (RD) using only two selection control lines (S1 and S0).  
**9.2 Limitations** 
While ideal for small register sets, pure multiplexer-based bus architectures require larger multiplexers (e.g., 16-to-1 or 32-to-1) as the number of registers increases. This increases gate fanin and propagation delay compared to tri-state buffer buses in large-scale microprocessors.  
**9.3 Future Enhancements** 
- Scaling the bus width from 4-bit to 8-bit, 16-bit, or 32-bit parallel data paths. 
- Integrating an Arithmetic Logic Unit (ALU) and control unit to build a complete basic CPU datapath. 
**9.4 GitHub Link-**   https://github.com/rayaparupa432-source/Design-of-a-4-to-1-Multiplexer-Based-Bus-System-for-Transferring-4-Bit-Data-Between-Registers-
# 10. References 
1. M. M. Mano, Computer System Architecture, 3rd ed. Upper Saddle River, NJ, USA: Pearson Education, 1993, pp. 93–99. 
2. V. C. Hamacher, Z. G. Vranesic, and S. G. Zaky, Computer Organization, 5th ed. New York, NY, USA: McGraw-Hill, 2002. 
3. W. Stallings, Computer Organization and Architecture: Designing for Performance, 10th ed. Hoboken, NJ, USA: Pearson, 2016. 
4. J. P. Hayes, Computer Architecture and Organization, 3rd ed. New York, NY, USA: McGrawHill, 1998. 
5. Texas Instruments, “SN74153 Dual 4-Line to 1-Line Data Selectors/Multiplexers Datasheet,” 
# 11. Appendix
**11.1 Simulation & Output Screenshots**
**11.1.a Block Diagram Simulation**
<img width="2169" height="2679" alt="1000198553" src="https://github.com/user-attachments/assets/3c279eb6-78e3-4565-821c-3302277268b7" />

**11.1.b Circuit Diagram & Output Simulation**
<img width="2160" height="2535" alt="1000198530" src="https://github.com/user-attachments/assets/ad31918f-9b50-4c9b-874f-d8c77c751aaf" />

**11.2 Project Demonstration Video**

**11.2.a Block Diagram Explanation:**(https://drive.google.com/file/d/1yLS4STdzwSplDBKHqje8NWgC7C8UEHXV/view?usp=drivesdk)

**11.2.b Circuit Diagram & Simulation:**(https://drive.google.com/file/d/1WDicCBzCVZr8cwTdA47BpuD3D2_ILlfV/view?usp=drivesdk)



