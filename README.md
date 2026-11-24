# Design-of-a-4-to-1-Multiplexer-Based-Bus-System-for-Transferring-4-Bit-Data-Between-Registers

# Abstract 
In modern digital computer architecture, efficient data transfer between internal processor registers is critical to overall system performance. Connecting every register directly to every other register using dedicated point-to-point wires creates severe wiring congestion, increases hardware cost, and introduces signal propagation delays. The objective of this mini project is to design and verify a collision-free common bus system capable of transferring 4-bit parallel binary data among four source registers (R0, R1, R2, and R3) to a destination register (RD) using 4-to-1 multiplexers.  
The methodology involves organizing four 4-to-1 multiplexers (MUX0 to MUX3) in parallel, where each multiplexer handles one specific bit position across all four source registers. Two shared selection lines (S1 and S0) serve as control inputs to select which source register’s 4-bit word is placed onto the common 4-bit bus (B3 B2 B1 B0). When the load-enable signal is active, the destination register RD latches the bus data on the active clock edge. The circuit was modeled and tested using digital logic simulation tools (Logisim Evolution) and standard TTL IC specifications (IC 74153 and IC 74173). The results confirm deterministic, contention-free data routing across all selection states. 
# Table of Contents
| Nos | Title | 
| :--- | :--- |
| *1.* | Introduction (Aim - Theory)
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
# 1. Aim
To design a 4-to-1 multiplexer-based bus system for transferring 4-bit data between registers using selection lines, and to verify data transfer by loading and observing register contents.
# 2. Components Required 
- Four 4-bit source registers: R0, R1, R2 and R3.
- Four 4-to-1 multiplexers (one for each data bit).
- Two selection lines: S1 and S0.
- One 4-bit destination register RD, a clock pulse and a load-enable signal.
- Digital logic simulator such as Logisim Evolution, or equivalent digital logic components.
# 3. Theory
A multiplexer (MUX) is a combinational circuit that selects one of several input signals and forwards the selected input to a
single output. Four 4-bit source registers share a common 4-bit bus. The two selection lines select one source register at a time.
Four 4-to-1 multiplexers operate in parallel—one per bit position. The four outputs form the common 4-bit bus. The destination
register loads the bus value when load-enable is active and the required clock edge occurs; otherwise, it retains its previous
value.
# 4. Block Diagram
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
# 5. Gate-Level Logical Circuit Diagram
The following circuit shows the standard AND–OR realization of one bit of a 4-to-1 multiplexer. Each AND gate forms
one product term, and the OR gate combines the terms. S1' and S0' are generated using NOT gates. The one-bit circuit is repeated for all four bit positions.
GATE-LEVEL LOGIC CIRCUIT FOR ONE BIT OF A 4-to-1 MUX
<img width="3060" height="4080" alt="1000188354" src="https://github.com/user-attachments/assets/23b1046a-d1f4-4449-87a0-e867fc05edd2" />
<img width="867" height="582" alt="1000188357" src="https://github.com/user-attachments/assets/82e40c4a-ad3a-4d45-8c65-7fdfff485cf6" />
The OR gate combines all four product terms to produce the selected output bit B[i].
Repeat this one-bit circuit for i = 3, 2, 1 and 0 to form the 4-bit bus.
# 6.Selection-Line Truth Table
| S1 | S0 | Selected register | Bus output |
| :--- | :--- | :--- | :--- |
| 0 | 0 | R0 | B = R0 |
| 0 | 1 | R1 | B = R1 |
| 1 | 0 | R2 | B = R2 |
| 1 | 1 | R3 | B = R3 |
# 7. Logic Equation
For each bit position i = 0, 1, 2, 3:
B[i] = (S1' S0' R0[i]) + (S1' S0 R1[i])+ (S1 S0' R2[i]) + (S1 S0 R3[i])

Here, ' means NOT, adjacency means AND, and + means OR. The destination register loads RD ← B when load-enable is active on the active clock edge.
# 8. Example Input Values
| Source register | 4-bit binary data |
| :--- | :--- |
| R0 | 1010 |
| R1 | 1100 |
| R2 | 0111 |
| R3 | 1001 |
# 9. Observation Table
| S1S0 | Selected register | Bus data | RD after clock |
| :--- | :--- | :--- | :--- |
| 00 | R0 | 1010 | 1010 |
| 01 | R1 | 1100 | 1100 |
| 10 | R2 | 0111 | 0111 |
| 11 | R3 | 1001 | 1001 |

Note: These are sample test values. Record the actual values observed during simulation or hardware testing.
# 10. Procedure
1. Create four 4-bit source registers and one 4-bit destination register.
2. Connect the corresponding bits of the four source registers to four 4-to-1 multiplexers.
3. Connect S1 and S0 to the selection inputs of all four multiplexers.
4. Connect the four multiplexer outputs to the common 4-bit bus.
5. Connect the bus to the destination register input.
6. Connect the clock and load-enable signal to the destination register.
7. Load different binary values into the source registers.
8. Change the selection lines, observe the bus, enable loading and apply a clock edge.
9. Verify that the destination register contains the selected source register's data.
# 11. Result
A 4-to-1 multiplexer-based bus system was designed. The selection lines select one of four source registers, and its 4-bit data is transferred through the common bus to the destination register when loading is enabled.
# 12. Conclusion
The 4-to-1 multiplexer-based bus system for transferring 4-bit data between registers was successfully designed. The four source registers share a common 4-bit bus, and the selection lines S1 and S0 determine which register’s data is routed through the multiplexer bank. The selected data is transferred to the destination register RD when load-enable is active and the clock edge is applied. The block diagram, gate-level logic diagram, selection truth table, and
observation table demonstrate the working of the system. Thus, the design verifies that a multiplexer-based bus provides an organized and efficient way to select and transfer data between registers
# 13. Project Demonstration Video
1. **Block Diagram Explanation:**(https://drive.google.com/file/d/1yLS4STdzwSplDBKHqje8NWgC7C8UEHXV/view?usp=drivesdk)
2. **Circuit Diagram & Simulation:**(https://drive.google.com/file/d/1WDicCBzCVZr8cwTdA47BpuD3D2_ILlfV/view?usp=drivesdk)
# Simulation & Output Screenshots

# 1. Block Diagram Simulation
(<img width="2169" height="2679" alt="1000198553" src="https://github.com/user-attachments/assets/3c279eb6-78e3-4565-821c-3302277268b7" />
)

# 2. Circuit Diagram & Output Simulation
(<img width="2160" height="2535" alt="1000198530" src="https://github.com/user-attachments/assets/ad31918f-9b50-4c9b-874f-d8c77c751aaf" />
)
