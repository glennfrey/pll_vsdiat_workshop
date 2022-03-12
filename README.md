# PLL_IC_design_on_open_source_google_skywater_using_130nm_Workshop
Phase-Locked Loop(PLL) IC design on Open-Source Google-Skywater 130nm Workshop
![](pll_workshops.png)
 - [Day 1: PLL Theory and Lab setup](#Day1)
  * [Part 1: Introduction to PLL](#Part1-Introduction-to-PLL)
  * [Part 2: Introduction to Phase Frequency Detector](#Part2-Introduction-to-Phase-Frequency-Detector)
  * [Part 3: Introduction to Charge Pump](#Part3-Introduction-to-Charge-Pump)
  * [Part 4: Introduction to Voltage Controlled Oscillator and Frequency Divider](#Part4-Introduction-to-Voltage-Controlled-Oscillator-and-Frequency-Divider)
  * [Part 5: Tool Setup and Design Flow](#Part5-Tool-Setup-and-Design-Flow)
  * [Part 6: Introduction to PDK, specifications and pre-layout circuits](#Part6-Introduction-to-PDK-specifications-and-pre-layout-circuits)
  * [Part 7: Circuit design simulation tool Ngspice Setup](#Part7-Circuit-design-simulation-tool-Ngspice-Setup)
  * [Part 8: Layout design tool Magic Setup](#Part8-Layout-design-tool-Magic-Setup)
- [Day 2: PLL Labs and post-layout simulations](#Day2)
  * [Part 9: PLL components circuit design](#Part9-PLL-components-circuit-design)
  * [Part 10: PLL components circuit simulations](#Part10-PLL-components-circuit-simulations)
  * [Part 11: Steps to combine PLL sub-circuits and PLL full design simulation](#Part11-Steps-to-combine-PLL-subcircuits-and-PLL-full-design-simulation)
  * [Part 12: Troubleshooting steps](#Part12-Troubleshooting-steps)
  * [Part 13: Layout design](#Part13-Layout-design)
  * [Part 14: Layout Walkthrough](#Part14-Layout-Walkthrough)
  * [Part 15: Parasitic Extraction](#Part15-Parasitic-Extraction)
  * [Part 16: Post Layout simulations](#Part16-Post-Layout-simulations)
  * [Part 17: Steps to combine layouts](#Part17-Steps-to-combine-layouts)
  * [Part 18: Tapeout theory](#Part18-Tapeout-theory)
  * [Part 19: Tapeout labs](#Part19-Tapeout-labs)


## Day1
Carry Lookahead Adder (CLA) or parallel adder is faster than the normal Full Adder. Unlike normal Full Adder the CLA does not have to wait for the previous carry bit to be calculated. In Carry Lookahead Adder all carry bits are calculated in parallel before calculating the sum. It takes advantage of computational parallelization at the cost of power consumption and circuit complexity. Transistor size must be carefully selected to reach the needed performance.
![]theory/1.1PLL_internal_components.png)
## Part1 Introduction to PLL

I will be designing a 4-bit Carry Lookahead Adder by using conventional static CMOS. The proposed circuit will be implemented in Synopsys EDA tool and will be done using 28nm technology. Basic element of the circuit is implemented using NMOS and PMOS. Looking at the diagram in Figure 1, the general formula for carry-out bits can be written as: 𝐶𝑖+1 = 𝐶𝑖 (𝐴𝑖 + 𝐵𝑖 ) + 𝐴𝑖𝐵𝑖. Once the carry-out bits have been calculated, the sums are found using the simple XOR operation. Although the equation is similar to equations of 4-bit Ripple Carry Adder (RCA), the transistor level design methodology presented in this section will transform the RCA equations into CLA process.
![](pll_workshop/1.1PLL.png)
## Part2 Introduction to Phase Frequency Detector
![](pll_workshop/PFD_schematic.png)
![](theory/1.2PFD_diff_freq.png)
![](theory/1.2PFD_lagging_and_leading.png)


## Part3 Introduction to Charge Pump
![](pll_workshop/CP_schematic.png)

## Part4 Introduction to Voltage Controlled Oscillator and Frequency Divider
![](pll_workshop/VCO_schematic.png)
![](pll_workshop/FD_schematic.png)
![](theory/1.4Frequency_divider.png)


## Part5 Tool Setup and Design Flow
![](theory/1.5Tools_Setup.png)
![](theory/1.5Tools_Command.png)
![](theory/1.5Development_Flow.png)



## Part6 Introduction to PDK, specifications and pre-layout circuits
![](theory/1.6PLL_Specification.png)
![](skywater_pdk.png)

## Part7 Circuit design simulation tool Ngspice Setup
![](pll_workshop/PFD_files.png)

## Part8 Layout design tool Magic Setup
![](pll_workshop/pll_workshops.png)

## Day2
![](pll_workshop/pll_workshops.png)

## Part9 PLL components circuit design
![](pll_workshop/pll_workshops.png)
.include sky130nm.lib

xm1 1 2 3 1 sky130_fd_pr__pfet_01v8 l=150n w=720n 
xm2 0 2 3 0 sky130_fd_pr__nfet_01v8 l=150n w=360n 

xm3 3 Clkb 4 1 sky130_fd_pr__pfet_01v8 l=150n w=420n 
xm4 3 Clk 4 0 sky130_fd_pr__nfet_01v8 l=150n w=840n 

xm7 1 4 5 1 sky130_fd_pr__pfet_01v8 l=150n w=720n 
xm8 0 4 5 0 sky130_fd_pr__nfet_01v8 l=150n w=360n 

xm9 5 Clk 6 1 sky130_fd_pr__pfet_01v8 l=150n w=420n 
xm10 5 Clkb 6 0 sky130_fd_pr__nfet_01v8 l=150n w=640n 

xm11 1 6 2 1 sky130_fd_pr__pfet_01v8 l=150n w=720n 
xm12 0 6 2 0 sky130_fd_pr__nfet_01v8 l=150n w=360n 

xm13 1 Clk Clkb 1 sky130_fd_pr__pfet_01v8 l=150n w=720n 
xm14 0 Clk Clkb 0 sky130_fd_pr__nfet_01v8 l=150n w=360n 

v1 1 0 1.8
v2 Clk 0 PULSE 0 1.8 1n 6p 6p 5ns 10ns

c1 6 0 10f
.control
tran 0.1ns 0.2us
plot v(6) v(Clk)+2
.endc

.end

## Part10 PLL components circuit simulations
![](pll_workshop/CP_cir.png)
.include sky130nm.lib

xm43 3 2 1 1 sky130_fd_pr__pfet_01v8 l=150n w=5.4u 
xm44 out downb 3 1 sky130_fd_pr__pfet_01v8 l=150n w=420n 
xm31 out up 7 0 sky130_fd_pr__nfet_01v8 l=150n w=420n
xm32 7 8 0 0 sky130_fd_pr__nfet_01v8 l=150n w=5.4u

xm33 2 2 1 1 sky130_fd_pr__pfet_01v8 l=150n w=420n 
xm34 8 8 0 0 sky130_fd_pr__nfet_01v8 l=150n w=420n

xm35 9 down 3 1 sky130_fd_pr__pfet_01v8 l=150n w=5400n 
xm36 9 9 0 0 sky130_fd_pr__nfet_01v8 l=150n w=420n

xm37 10 10 1 1 sky130_fd_pr__pfet_01v8 l=150n w=420n 
xm38 10 upb 7 0 sky130_fd_pr__nfet_01v8 l=150n w=5400n

xm39 1 down downb 1 sky130_fd_pr__pfet_01v8 l=150n w=720n 
xm40 0 down downb 0 sky130_fd_pr__nfet_01v8 l=150n w=360n 

xm41 1 up upb 1 sky130_fd_pr__pfet_01v8 l=150n w=720n 
xm42 0 up upb 0 sky130_fd_pr__nfet_01v8 l=150n w=360n 
v1 1 0 1.8	
v2 up 0 0 
*PULSE 0 1.8 1n 6p 6p 100ns 200ns
v3 down 0 0

r1 out rc 200
c1 rc 0 64f
c2 out 0 10f

.ic v(out) = 0
.ic c(out) = 0
.control
tran 1ns 20us
plot v(out) C(out)
*plot v(6) V(Clk)+2
* v(D) v(Clk) v(6)
.endc

.end
![](pll_workshop/CP_waveform.png)
![](pll_workshop/CP_waveform2.png)
.include sky130nm.lib

xm1 10 16 3 10 sky130_fd_pr__pfet_01v8 l=150n w=420n
xm2 3 16 9 9  sky130_fd_pr__nfet_01v8 l=150n w=420n

xm3 10 3 4 10 sky130_fd_pr__pfet_01v8 l=150n w=420n
xm4 4 3 9 9 sky130_fd_pr__nfet_01v8 l=150n w=420n

xm5 10 4 12 10 sky130_fd_pr__pfet_01v8 l=150n w=420n
xm6 12 4 9 9 sky130_fd_pr__nfet_01v8 l=150n w=420n

xm11 10 12 13 10 sky130_fd_pr__pfet_01v8 l=150n w=420n
xm12 13 12 9 9 sky130_fd_pr__nfet_01v8 l=150n w=420n

xm13 10 13 14 10 sky130_fd_pr__pfet_01v8 l=150n w=420n
xm14 14 13 9 9 sky130_fd_pr__nfet_01v8 l=150n w=420n

xm15 10 14 15 10 sky130_fd_pr__pfet_01v8 l=150n w=420n
xm16 15 14 9 9 sky130_fd_pr__nfet_01v8 l=150n w=420n

xm17 10 15 16 10 sky130_fd_pr__pfet_01v8 l=150n w=2400n
xm18 16 15 9 9 sky130_fd_pr__nfet_01v8 l=150n w=1200n

xm7 10 5 1 1 sky130_fd_pr__pfet_01v8 l=150n w=1080n
xm8 5 5 1 1 sky130_fd_pr__pfet_01v8 l=150n w=840n
xm9 5 in 0 0 sky130_fd_pr__nfet_01v8 l=150n w=840n
xm10 9 in 0 0 sky130_fd_pr__nfet_01v8 l=150n w=1080n

xm19 1 16 11 1 sky130_fd_pr__pfet_01v8 l=150n w=1080n
xm20 11 16 0 0 sky130_fd_pr__nfet_01v8 l=150n w=540n

*c1 11 0 10f
v1 1 0 1.8
v2 in 0 0.6


.control
tran 0.1ns 0.5us
plot v(in) v(11)
*setplot tran1
*linearize v(14)
*set specwindow=blackman
*fft v(14)
*dc v2 0 1.2 0.01
*plot mag(v(14))
.endc
.end

![](pll_workshop/VCO_cir.png)
![](pll_workshop/VCO_waveform.png)
.include sky130nm.lib

xm1 1 2 3 1 sky130_fd_pr__pfet_01v8 l=150n w=720n 
xm2 0 2 3 0 sky130_fd_pr__nfet_01v8 l=150n w=360n 

xm3 3 Clkb 4 1 sky130_fd_pr__pfet_01v8 l=150n w=420n 
xm4 3 Clk 4 0 sky130_fd_pr__nfet_01v8 l=150n w=840n 

xm7 1 4 5 1 sky130_fd_pr__pfet_01v8 l=150n w=720n 
xm8 0 4 5 0 sky130_fd_pr__nfet_01v8 l=150n w=360n 

xm9 5 Clk 6 1 sky130_fd_pr__pfet_01v8 l=150n w=420n 
xm10 5 Clkb 6 0 sky130_fd_pr__nfet_01v8 l=150n w=640n 

xm11 1 6 2 1 sky130_fd_pr__pfet_01v8 l=150n w=720n 
xm12 0 6 2 0 sky130_fd_pr__nfet_01v8 l=150n w=360n 

xm13 1 Clk Clkb 1 sky130_fd_pr__pfet_01v8 l=150n w=720n 
xm14 0 Clk Clkb 0 sky130_fd_pr__nfet_01v8 l=150n w=360n 

v1 1 0 1.8
v2 Clk 0 PULSE 0 1.8 1n 6p 6p 5ns 10ns

c1 6 0 10f
.control
tran 0.1ns 0.2us
plot v(6) v(Clk)+2
.endc

.end

![](pll_workshop/PD_cir.png)
![](pll_workshop/PD_waveform.png)
## Part11 Steps to combine PLL subcircuits and PLL full design simulation
![](pll_workshop/pll_workshops.png)
## Part12 Troubleshooting steps
![](pll_workshop/pll_workshops.png)
## Part13 Layout design
![](pll_workshop/pll_workshops.png)
## Part14 Layout Walkthrough
![](pll_workshop/pll_workshops.png)
## Part15 Parasitic Extraction
![](pll_workshop/pll_workshops.png)
## Part16 Post Layout simulations
![](pll_workshop/pll_workshops.png)
## Part17 Steps to combine layouts
![](pll_workshop/pll_workshops.png)
## Part18 Tapeout theory
![](pll_workshop/pll_workshops.png)
## Part19 Tapeout labs
![](pll_workshop/pll_workshops.png)
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
  

