# PLL_IC_design_on_open_source_google_skywater_using_130nm_Workshop
Phase-Locked Loop(PLL) IC design on Open-Source Google-Skywater 130nm Workshop

 - [Day 1: PLL Theory and Lab setup](#Day1:-PLL-Theory-and-Lab-setup)
  * [Part 1: Introduction to PLL](#reference-circuit-details)
  * [Part 2: Introduction to Phase Frequency Detector](#reference-circuit-diagram)
  * [Part 3: Introduction to Charge Pump](#reference-circuit-waveform)
  * [Part 4: Introduction to Voltage Controlled Oscillator and Frequency Divider](#desirable-truth-table)
  * [Part 5: Tool Setup and Design Flow](#tools-used)
  * [Part 6: Introduction to PDK, specifications and pre-layout circuits](#tools-used)
  * [Part 7: Circuit design simulation tool - Ngspice Setup](#tools-used)
  * [Part 8: Layout design tool - Magic Setup](#tools-used)
- [Day 2: PLL Labs and post-layout simulations](#simulation-in-synopsys)
  * [Part 9: PLL components circuit design](#xor_block)
  * [Part 10: PLL components circuit simulations](#carry_lookahead_bit1_block)
  * [Part 11: Steps to combine PLL sub-circuits and PLL full design simulation](#carry_lookahead_bit2_block)
](#carry_lookahead_bit3_block)
  * [Part 12: Troubleshooting steps](#carry_lookahead_bit4_block)
  * [Part 13: Layout design](#carry_lookahead_adder_4bit)
  * [Part 14: Layout Walkthrough](#output-waveform)
  * [Part 15: Parasitic Extraction](#netlist)
  * [Part 16: Post Layout simulations](#conclusion)
  * [Part 17: Steps to combine layouts](#author)
  * [Part 18: Tapeout theory](#acknowlegement)
  * [Part 19: Tapeout labs](#references)


## Day 1: PLL Theory and Lab setup
Carry Lookahead Adder (CLA) or parallel adder is faster than the normal Full Adder. Unlike normal Full Adder the CLA does not have to wait for the previous carry bit to be calculated. In Carry Lookahead Adder all carry bits are calculated in parallel before calculating the sum. It takes advantage of computational parallelization at the cost of power consumption and circuit complexity. Transistor size must be carefully selected to reach the needed performance.

## Reference Circuit Details

I will be designing a 4-bit Carry Lookahead Adder by using conventional static CMOS. The proposed circuit will be implemented in Synopsys EDA tool and will be done using 28nm technology. Basic element of the circuit is implemented using NMOS and PMOS. Looking at the diagram in Figure 1, the general formula for carry-out bits can be written as: 𝐶𝑖+1 = 𝐶𝑖 (𝐴𝑖 + 𝐵𝑖 ) + 𝐴𝑖𝐵𝑖. Once the carry-out bits have been calculated, the sums are found using the simple XOR operation. Although the equation is similar to equations of 4-bit Ripple Carry Adder (RCA), the transistor level design methodology presented in this section will transform the RCA equations into CLA process.

## Reference Circuit Diagram
![](analog/reference_circuit.png)
##### CLA bit 1-4
![](analog/CLA_reference_circuit.png)
##### XOR
![](analog/xor.png)

## Reference Circuit Waveform
![](analog/CLA_reference_waveform.png)

## Desirable Truth Table
![](truth_table.png)


## Tools Used:
• Synopsys Custom Compiler:
 The Synopsys Custom Compiler™ design environment is a modern solution for full-custom analog, custom digital, and mixed-signal IC design. As the heart of the Synopsys Custom Design Platform, Custom Compiler provides design entry, simulation management and analysis, and custom layout editing features. This tool was used to design the circuit on a transistor level.
 
 ![custom_compiler](https://user-images.githubusercontent.com/59500283/155473715-c6a1fd5b-71c7-4655-936a-5fe3befabfd8.png)


• Synopsys Primewave:
 PrimeWave™ Design Environment is a comprehensive and flexible environment for simulation setup and analysis of analog, RF, mixed-signal design, custom-digital and memory designs within the Synopsys Custom Design Platform. This tool helped in various types of simulations of the above designed circuit.

• Synopsys 28nm PDK:
 The Synopsys 28nm Process Design Kit(PDK) was used in creation and simulation of the above designed circuit.

# Simulation in Synopsys
## XOR_Block
![](analog/xor_schematic.png)
![](analog/xo_tb.png)
![](analog/xor_waveform.png)

## Carry_Lookahead_bit1_Block
![](analog/CLA_1_schematic.png)
![](analog/CLA_1_tb.png)
![](analog/CLA_1_waveform.png)

## Carry_Lookahead_bit2_Block
![](analog/CLA_2_schematic.png)
![](analog/CLA_2_tb.png)
![](analog/CLA_2_waveform.png)

## Carry_Lookahead_bit3_Block
![](analog/CLA_3_schematic.png)
![](analog/CLA_3_tb.png)
![](analog/CLA_3_waveform.png)

## Carry_Lookahead_bit4_Block
![](analog/CLA_4_schematic.png)
![](analog/CLA_4_tb.png)
![](analog/CLA_4_waveform.png)
![](analog/CLA_4_waveform1.png)
![](analog/CLA_4_waveform2.png)
![](analog/CLA_4_waveform3.png)

## Carry_Lookahead_Adder_4bit
![](analog/CLA_final_schematic.png)
![](analog/CLA_final_tb.png)
![](analog/CLA_final_waveform.png)

## Output Waveform
![](analog/CLA_final_waveform1.png)
![](analog/CLA_final_waveform2.png)
![](analog/CLA_final_waveform3.png)
![](analog/CLA_final_waveform4.png)


## Netlist
```

*  Generated for: PrimeSim
*  Design library name: glenn_DAC_new
*  Design cell name: glenn_CLAfinal_tb
*  Design view name: schematic
.lib 'saed32nm.lib' TT

*Custom Compiler Version S-2021.09
*Sat Feb 26 09:55:29 2022

.global gnd!
********************************************************************************
* Library          : glenn_DAC_new
* Cell             : glenn_xor_new
* View             : schematic
* View Search List : hspice hspiceD schematic spice veriloga
* View Stop List   : hspice hspiceD
********************************************************************************
.subckt glenn_xor_new a b sum vdda vssa
xm5 net53 a vdda vdda p105 w=0.1u l=0.03u nf=1 m=1
xm4 net52 b vdda vdda p105 w=0.1u l=0.03u nf=1 m=1
xm3 sum net52 net13 vdda p105 w=0.1u l=0.03u nf=1 m=1
xm2 net13 a vdda vdda p105 w=0.1u l=0.03u nf=1 m=1
xm1 sum net53 net5 vdda p105 w=0.1u l=0.03u nf=1 m=1
xm0 net5 b vdda vdda p105 w=0.1u l=0.03u nf=1 m=1
xm11 net53 a vssa vssa n105 w=0.1u l=0.03u nf=1 m=1
xm10 net52 b vssa vssa n105 w=0.1u l=0.03u nf=1 m=1
xm9 net37 a vssa vssa n105 w=0.1u l=0.03u nf=1 m=1
xm8 sum b net37 vssa n105 w=0.1u l=0.03u nf=1 m=1
xm7 net29 net53 vssa vssa n105 w=0.1u l=0.03u nf=1 m=1
xm6 sum net52 net29 vssa n105 w=0.1u l=0.03u nf=1 m=1
.ends glenn_xor_new

********************************************************************************
* Library          : glenn_DAC_new
* Cell             : glenn_CLA1
* View             : schematic
* View Search List : hspice hspiceD schematic spice veriloga
* View Stop List   : hspice hspiceD
********************************************************************************
.subckt glenn_cla1 a b c c1 vdda vssa
xm5 c1 net54 vdda vdda p105 w=0.1u l=0.03u nf=1 m=1
xm4 net54 b net17 vdda p105 w=0.1u l=0.03u nf=1 m=1
xm3 net17 a vdda vdda p105 w=0.1u l=0.03u nf=1 m=1
xm2 net54 a net9 vdda p105 w=0.1u l=0.03u nf=1 m=1
xm1 net54 b net9 vdda p105 w=0.1u l=0.03u nf=1 m=1
xm0 net9 c vdda vdda p105 w=0.1u l=0.03u nf=1 m=1
xm11 net45 a vssa vssa n105 w=0.1u l=0.03u nf=1 m=1
xm10 net54 b net45 vssa n105 w=0.1u l=0.03u nf=1 m=1
xm9 net54 b vssa vssa n105 w=0.1u l=0.03u nf=1 m=1
xm8 net33 c vssa vssa n105 w=0.1u l=0.03u nf=1 m=1
xm7 net54 a net33 vssa n105 w=0.1u l=0.03u nf=1 m=1
xm6 c1 net54 vssa vssa n105 w=0.1u l=0.03u nf=1 m=1
.ends glenn_cla1

********************************************************************************
* Library          : glenn_DAC_new
* Cell             : glenn_CLA2
* View             : schematic
* View Search List : hspice hspiceD schematic spice veriloga
* View Stop List   : hspice hspiceD
********************************************************************************
.subckt glenn_cla2 a0 a1 b0 b1 c0 c2 vdda vssa
xm9 c2 net80 vdda vdda p105 w=0.1u l=0.03u nf=1 m=1
xm8 net80 a1 net33 vdda p105 w=0.1u l=0.03u nf=1 m=1
xm7 net33 a0 net27 vdda p105 w=0.1u l=0.03u nf=1 m=1
xm6 net27 c0 vdda vdda p105 w=0.1u l=0.03u nf=1 m=1
xm5 net33 b0 net27 vdda p105 w=0.1u l=0.03u nf=1 m=1
xm4 net80 b1 net33 vdda p105 w=0.1u l=0.03u nf=1 m=1
xm3 net33 b0 net13 vdda p105 w=0.1u l=0.03u nf=1 m=1
xm2 net13 a0 vdda vdda p105 w=0.1u l=0.03u nf=1 m=1
xm1 net80 b1 net5 vdda p105 w=0.1u l=0.03u nf=1 m=1
xm0 net5 a1 vdda vdda p105 w=0.1u l=0.03u nf=1 m=1
xm19 c2 net80 vssa vssa n105 w=0.1u l=0.03u nf=1 m=1
xm18 net73 c0 vssa vssa n105 w=0.1u l=0.03u nf=1 m=1
xm17 net69 a0 net73 vssa n105 w=0.1u l=0.03u nf=1 m=1
xm16 net80 a1 net69 vssa n105 w=0.1u l=0.03u nf=1 m=1
xm15 net69 b0 net73 vssa n105 w=0.1u l=0.03u nf=1 m=1
xm14 net57 a0 vssa vssa n105 w=0.1u l=0.03u nf=1 m=1
xm13 net69 b0 net57 vssa n105 w=0.1u l=0.03u nf=1 m=1
xm12 net80 b1 net69 vssa n105 w=0.1u l=0.03u nf=1 m=1
xm11 net45 a1 vssa vssa n105 w=0.1u l=0.03u nf=1 m=1
xm10 net80 b1 net45 vssa n105 w=0.1u l=0.03u nf=1 m=1
.ends glenn_cla2

********************************************************************************
* Library          : glenn_DAC_new
* Cell             : glenn_CLA3
* View             : schematic
* View Search List : hspice hspiceD schematic spice veriloga
* View Stop List   : hspice hspiceD
********************************************************************************
.subckt glenn_cla3 a0 a1 a2 b0 b1 b2 c0 c3 vdda vssa
xm13 net77 b2 net53 vdda p105 w=0.1u l=0.03u nf=1 m=1
xm12 net53 a2 vdda vdda p105 w=0.1u l=0.03u nf=1 m=1
xm11 net77 b2 net45 vdda p105 w=0.1u l=0.03u nf=1 m=1
xm10 net45 b1 net41 vdda p105 w=0.1u l=0.03u nf=1 m=1
xm9 net41 a1 vdda vdda p105 w=0.1u l=0.03u nf=1 m=1
xm8 net45 b1 net33 vdda p105 w=0.1u l=0.03u nf=1 m=1
xm7 net33 b0 net115 vdda p105 w=0.1u l=0.03u nf=1 m=1
xm6 net115 a0 vdda vdda p105 w=0.1u l=0.03u nf=1 m=1
xm5 net33 b0 net29 vdda p105 w=0.1u l=0.03u nf=1 m=1
xm4 c3 net77 vdda vdda p105 w=0.1u l=0.03u nf=1 m=1
xm3 net77 a2 net45 vdda p105 w=0.1u l=0.03u nf=1 m=1
xm2 net45 a1 net33 vdda p105 w=0.1u l=0.03u nf=1 m=1
xm1 net33 a0 net29 vdda p105 w=0.1u l=0.03u nf=1 m=1
xm0 net29 c0 vdda vdda p105 w=0.1u l=0.03u nf=1 m=1
xm27 net109 a2 vssa vssa n105 w=0.1u l=0.03u nf=1 m=1
xm26 net105 a1 vssa vssa n105 w=0.1u l=0.03u nf=1 m=1
xm25 net101 b1 net105 vssa n105 w=0.1u l=0.03u nf=1 m=1
xm24 net97 a0 vssa vssa n105 w=0.1u l=0.03u nf=1 m=1
xm23 net93 b0 net97 vssa n105 w=0.1u l=0.03u nf=1 m=1
xm22 net89 c0 vssa vssa n105 w=0.1u l=0.03u nf=1 m=1
xm21 net93 a0 net89 vssa n105 w=0.1u l=0.03u nf=1 m=1
xm20 net101 a1 net93 vssa n105 w=0.1u l=0.03u nf=1 m=1
xm19 net77 b2 net109 vssa n105 w=0.1u l=0.03u nf=1 m=1
xm18 net77 b2 net101 vssa n105 w=0.1u l=0.03u nf=1 m=1
xm17 net101 b1 net93 vssa n105 w=0.1u l=0.03u nf=1 m=1
xm16 net93 b0 net89 vssa n105 w=0.1u l=0.03u nf=1 m=1
xm15 c3 net77 vssa vssa n105 w=0.1u l=0.03u nf=1 m=1
xm14 net77 a2 net101 vssa n105 w=0.1u l=0.03u nf=1 m=1
.ends glenn_cla3

********************************************************************************
* Library          : glenn_DAC_new
* Cell             : glenn_CLA4
* View             : schematic
* View Search List : hspice hspiceD schematic spice veriloga
* View Stop List   : hspice hspiceD
********************************************************************************
.subckt glenn_cla4 a0 a1 a2 a3 b0 b1 b2 b3 c0 vdda vssa vout
xm17 net109 b3 net69 vdda p105 w=0.1u l=0.03u nf=1 m=1
xm16 net69 a3 vdda vdda p105 w=0.1u l=0.03u nf=1 m=1
xm15 net109 b3 net61 vdda p105 w=0.1u l=0.03u nf=1 m=1
xm14 net61 b2 net57 vdda p105 w=0.1u l=0.03u nf=1 m=1
xm13 net57 a2 vdda vdda p105 w=0.1u l=0.03u nf=1 m=1
xm12 net51 a1 vdda vdda p105 w=0.1u l=0.03u nf=1 m=1
xm11 net47 b1 net51 vdda p105 w=0.1u l=0.03u nf=1 m=1
xm10 net61 b2 net47 vdda p105 w=0.1u l=0.03u nf=1 m=1
xm9 net47 b1 net37 vdda p105 w=0.1u l=0.03u nf=1 m=1
xm8 net37 b0 net33 vdda p105 w=0.1u l=0.03u nf=1 m=1
xm7 net33 a0 vdda vdda p105 w=0.1u l=0.03u nf=1 m=1
xm6 net37 b0 net5 vdda p105 w=0.1u l=0.03u nf=1 m=1
xm5 vout net109 vdda vdda p105 w=0.1u l=0.03u nf=1 m=1
xm4 net109 a3 net61 vdda p105 w=0.1u l=0.03u nf=1 m=1
xm3 net61 a2 net47 vdda p105 w=0.1u l=0.03u nf=1 m=1
xm2 net47 a1 net37 vdda p105 w=0.1u l=0.03u nf=1 m=1
xm1 net37 a0 net5 vdda p105 w=0.1u l=0.03u nf=1 m=1
xm0 net5 c0 vdda vdda p105 w=0.1u l=0.03u nf=1 m=1
xm35 net133 b0 net89 vssa n105 w=0.1u l=0.03u nf=1 m=1
xm34 net137 a0 vssa vssa n105 w=0.1u l=0.03u nf=1 m=1
xm33 net133 b0 net137 vssa n105 w=0.1u l=0.03u nf=1 m=1
xm32 net113 b1 net133 vssa n105 w=0.1u l=0.03u nf=1 m=1
xm31 net125 a1 vssa vssa n105 w=0.1u l=0.03u nf=1 m=1
xm30 net121 a3 vssa vssa n105 w=0.1u l=0.03u nf=1 m=1
xm29 net117 a2 vssa vssa n105 w=0.1u l=0.03u nf=1 m=1
xm28 net113 b1 net125 vssa n105 w=0.1u l=0.03u nf=1 m=1
xm27 net109 b3 net121 vssa n105 w=0.1u l=0.03u nf=1 m=1
xm26 net105 b2 net117 vssa n105 w=0.1u l=0.03u nf=1 m=1
xm25 net109 b3 net105 vssa n105 w=0.1u l=0.03u nf=1 m=1
xm24 net105 b2 net113 vssa n105 w=0.1u l=0.03u nf=1 m=1
xm23 vout net109 vssa vssa n105 w=0.1u l=0.03u nf=1 m=1
xm22 net89 c0 vssa vssa n105 w=0.1u l=0.03u nf=1 m=1
xm21 net133 a0 net89 vssa n105 w=0.1u l=0.03u nf=1 m=1
xm20 net113 a1 net133 vssa n105 w=0.1u l=0.03u nf=1 m=1
xm19 net105 a2 net113 vssa n105 w=0.1u l=0.03u nf=1 m=1
xm18 net109 a3 net105 vssa n105 w=0.1u l=0.03u nf=1 m=1
.ends glenn_cla4

********************************************************************************
* Library          : glenn_DAC_new
* Cell             : glenn_CLAfinal
* View             : schematic
* View Search List : hspice hspiceD schematic spice veriloga
* View Stop List   : hspice hspiceD
********************************************************************************
.subckt glenn_clafinal a0 a1 a2 a3 b0 b1 b2 b3 c0 c4 s0 s1 s2 s3 vdda vssa
xi3 a3 b3 net135 vdda vssa glenn_xor_new
xi0 a2 b2 net136 vdda vssa glenn_xor_new
xi4 net135 net141 s3 vdda vssa glenn_xor_new
xi28 a0 b0 net149 vdda vssa glenn_xor_new
xi1 a1 b1 net137 vdda vssa glenn_xor_new
xi29 c0 net149 s0 vdda vssa glenn_xor_new
xi6 net163 net137 s1 vdda vssa glenn_xor_new
xi5 net139 net136 s2 vdda vssa glenn_xor_new
xi8 a0 b0 c0 net163 vdda vssa glenn_cla1
xi9 a0 a1 b0 b1 c0 net139 vdda vssa glenn_cla2
xi10 a0 a1 a2 b0 b1 b2 c0 net141 vdda vssa glenn_cla3
xi11 a0 a1 a2 a3 b0 b1 b2 b3 c0 vdda vssa c4 glenn_cla4
.ends glenn_clafinal

********************************************************************************
* Library          : glenn_DAC_new
* Cell             : glenn_CLAfinal_tb
* View             : schematic
* View Search List : hspice hspiceD schematic spice veriloga
* View Stop List   : hspice hspiceD
********************************************************************************
xi0 a0 a1 a2 a3 b0 b1 b2 b3 c0 c4 s0 s1 s2 s3 net35 gnd! glenn_clafinal
v25 c0 gnd! dc=0 pulse ( 0 1 0 .1u .1u 320u 640u )
v24 b3 gnd! dc=0 pulse ( 0 1 0 .1u .1u 160u 320u )
v23 b2 gnd! dc=0 pulse ( 0 1 0 .1u .1u 80u 160u )
v22 b1 gnd! dc=0 pulse ( 0 1 0 .1u .1u 40u 80u )
v21 b0 gnd! dc=0 pulse ( 0 1 0 .1u .1u 20u 40u )
v20 a3 gnd! dc=0 pulse ( 0 1 0 .1u .1u 10u 20u )
v19 a2 gnd! dc=0 pulse ( 0 1 0 .1u .1u 5u 10u )
v18 a1 gnd! dc=0 pulse ( 0 1 0 .1u .1u 2.5u 5u )
v17 a0 gnd! dc=0 pulse ( 0 1 0 .1u .1u 1.25u 2.5u )
v26 net35 gnd! dc=1
c15 c4 gnd! c=1p
c14 s0 gnd! c=1p
c13 s1 gnd! c=1p
c12 s2 gnd! c=1p
c11 s3 gnd! c=1p








.tran '1u' '640u' name=tran

.option primesim_remove_probe_prefix = 0
.probe v(*) i(*) level=1
.probe tran v(a0) v(a1) v(a2) v(a3) v(b0) v(b1) v(b2) v(b3) v(c0) v(c4) v(s0)
+ v(s1) v(s2) v(s3)

.temp 25



.option primesim_output=wdf


.option parhier = LOCAL






.end


```


## Author
Glenn Frey Olamit , self.
## Acknowledgement
1. Kunal Ghosh, Co-founder, VSD Corp. Pvt. Ltd. - kunalpghosh@gmail.com
2. Chinmay panda, IIT Hyderabad
3. Sameer Durgoji, NIT Karnataka
4. [Synopsys Team/Company](https://www.synopsys.com/)
5. https://www.iith.ac.in/events/2022/02/15/Cloud-Based-Analog-IC-Design-Hackathon/
## References
I. S. Dhanjal. 4 bit carry look ahead adder transistor level
implementation using static cmos logic.
https://youtu.be/WItAXzrfPrE

M. Hasan. High-performance design of a 4-bit carry look-ahead adder
in static cmos logic.
http://section.iaesonline.com/index.php/IJEEI/article/view/2582
  

