<img width="1920" height="1080" alt="Lab-6 GIF" src="https://github.com/LogEman/VHDL-Lab6/blob/main/images/lab6-gif.gif?raw=true"/>

**7-Segment Truth Table:**
| Output | sw0 | sw1 | sw2 | sw3 | g | f | e | d | c | b | a | Match |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| **0** | 0 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | X |
| **1** | 0 | 0 | 0 | 1 | 1 | 1 | 1 | 1 | 0 | 0 | 1 | X |
| **2** | 0 | 0 | 1 | 0 | 0 | 1 | 0 | 0 | 1 | 0 | 0 | X |
| **3** | 0 | 0 | 1 | 1 | 0 | 1 | 1 | 0 | 0 | 0 | 0 | X |
| **4** | 0 | 1 | 0 | 0 | 0 | 0 | 1 | 1 | 0 | 0 | 1 | X |
| **5** | 0 | 1 | 0 | 1 | 0 | 0 | 1 | 0 | 0 | 1 | 0 | X |
| **6** | 0 | 1 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 1 | 0 | X |
| **7** | 0 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 0 | 0 | 0 | X |
| **8** | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | X |
| **9** | 1 | 0 | 0 | 1 | 0 | 0 | 1 | 1 | 0 | 0 | 0 | X |
| **A** | 1 | 0 | 1 | 0 | 0 | 0 | 0 | 1 | 0 | 0 | 0 | X |
| **B** | 1 | 0 | 1 | 1 | 0 | 0 | 0 | 0 | 0 | 1 | 1 | X |
| **C** | 1 | 1 | 0 | 0 | 1 | 0 | 0 | 0 | 1 | 1 | 0 | X |
| **D** | 1 | 1 | 0 | 1 | 0 | 1 | 0 | 0 | 0 | 0 | 1 | X |
| **E** | 1 | 1 | 1 | 0 | 0 | 0 | 0 | 0 | 1 | 1 | 0 | X |
| **F** | 1 | 1 | 1 | 1 | 0 | 0 | 0 | 1 | 1 | 1 | 0 | X |

Inputs(sw0 - sw3)[via sw bus] correspond to outputs(a-g)[via seg bus]. No logical equation used, hardcoded manually for every value (0-F)[based on attached specification]. an(0-3) receives a hardcoded "1110"(which selects the furthest right display).

**Seven Segment Display Specification:**<img width="708" height="587" alt="Screenshot 2026-03-05 112655" src="https://github.com/user-attachments/assets/9ad35584-ac38-4050-8a2f-cdfb226033fa" />

**Inputs**: sw[0-3, via bus]  
**Output(s)**: seg[0-7, via bus], an[0-3, via bus]   
**Match:** Tested on hardware and matches table

Running **Should** be as simple as downloading folder(as zip **using the giant green code button**), extracting, and running "lab6.xpr" (Vivado Project File). Code can be seen directly in .srcs

## Design Sources Code
**Path**: lab6.srcs/sources_1/new/lab6.vhd

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
entity lab6 is
    Port ( sw : in STD_LOGIC_VECTOR (3 downto 0);
           seg : out STD_LOGIC_VECTOR (6 downto 0);
           an : out STD_LOGIC_VECTOR (3 downto 0));
end lab6;
architecture Behavioral of lab6 is
begin
an <= "1110"; -- Selects the furthest right display on the board
with sw select seg <=
    "1000000" when "0000", --0
    "1111001" when "0001", --1
    "0100100" when "0010", --2
    "0110000" when "0011", --3
    "0011001" when "0100", --4
    "0010010" when "0101", --5
    "0000010" when "0110", --6
    "1111000" when "0111", --7
    "0000000" when "1000", --8
    "0011000" when "1001", --9
    "0001000" when "1010", --A
    "0000011" when "1011", --b
    "1000110" when "1100", --C
    "0100001" when "1101", --d
    "0000110" when "1110", --E
    "0001110" when "1111", --F
    "0111111" when others; -- "-": used if an invalid input is (somehow) received, this (shouldnt?) be possible but was added just in case.
end Behavioral;
```

## Constraints Code
**Path**: lab6.srcs/constrs_1/new/lab6.xdc

```tcl
## Switches
set_property PACKAGE_PIN V17 [get_ports {sw[0]}]					
	set_property IOSTANDARD LVCMOS33 [get_ports {sw[0]}]
set_property PACKAGE_PIN V16 [get_ports {sw[1]}]					
	set_property IOSTANDARD LVCMOS33 [get_ports {sw[1]}]
set_property PACKAGE_PIN W16 [get_ports {sw[2]}]					
	set_property IOSTANDARD LVCMOS33 [get_ports {sw[2]}]
set_property PACKAGE_PIN W17 [get_ports {sw[3]}]					
	set_property IOSTANDARD LVCMOS33 [get_ports {sw[3]}]

##7 segment display
set_property PACKAGE_PIN W7 [get_ports {seg[0]}]					
	set_property IOSTANDARD LVCMOS33 [get_ports {seg[0]}]
set_property PACKAGE_PIN W6 [get_ports {seg[1]}]					
	set_property IOSTANDARD LVCMOS33 [get_ports {seg[1]}]
set_property PACKAGE_PIN U8 [get_ports {seg[2]}]					
	set_property IOSTANDARD LVCMOS33 [get_ports {seg[2]}]
set_property PACKAGE_PIN V8 [get_ports {seg[3]}]					
	set_property IOSTANDARD LVCMOS33 [get_ports {seg[3]}]
set_property PACKAGE_PIN U5 [get_ports {seg[4]}]					
	set_property IOSTANDARD LVCMOS33 [get_ports {seg[4]}]
set_property PACKAGE_PIN V5 [get_ports {seg[5]}]					
	set_property IOSTANDARD LVCMOS33 [get_ports {seg[5]}]
set_property PACKAGE_PIN U7 [get_ports {seg[6]}]					
	set_property IOSTANDARD LVCMOS33 [get_ports {seg[6]}]

set_property PACKAGE_PIN U2 [get_ports {an[0]}]					
	set_property IOSTANDARD LVCMOS33 [get_ports {an[0]}]
set_property PACKAGE_PIN U4 [get_ports {an[1]}]					
	set_property IOSTANDARD LVCMOS33 [get_ports {an[1]}]
set_property PACKAGE_PIN V4 [get_ports {an[2]}]					
	set_property IOSTANDARD LVCMOS33 [get_ports {an[2]}]
set_property PACKAGE_PIN W4 [get_ports {an[3]}]					
	set_property IOSTANDARD LVCMOS33 [get_ports {an[3]}]
```
from https://wiki.ittc.ku.edu/ittc_wiki/images/6/6f/Basys3_Master.txt
