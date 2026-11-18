# Design-of-a-4-to-1-Multiplexer-Based-Bus-System-for-Transferring-4-Bit-Data-Between-Registers

#1. Aim
To design a 4-to-1 multiplexer-based bus system for transferring 4-bit data between registers using selection lines, and to verify data transfer by loading and observing register contents.
#2. Components Required
• Four 4-bit source registers: R0, R1, R2 and R3.
• Four 4-to-1 multiplexers (one for each data bit).
• Two selection lines: S1 and S0.
• One 4-bit destination register RD, a clock pulse and a load-enable signal.
• Digital logic simulator such as Logisim Evolution, or equivalent digital logic components.
#3. Theory
A multiplexer (MUX) is a combinational circuit that selects one of several input signals and forwards the selected input to asingle output. Four 4-bit source registers share a common 4-bit bus. The two selection lines select one source register at a time.
Four 4-to-1 multiplexers operate in parallel—one per bit position. The four outputs form the common 4-bit bus. The destinationregister loads the bus value when load-enable is active and the required clock edge occurs; otherwise, it retains its previousvalue.
#4. Block Diagram
The block diagram shows the full data path from the source registers through the multiplexer bank and common bus to the destination register.

