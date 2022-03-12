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
![](pll_workshop/FD_cir.png)

```
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
```
![](pll_workshop/FD_waveform.png)
## Part10 PLL components circuit simulations
![](pll_workshop/CP_cir.png)
```
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
```
![](pll_workshop/CP_waveform.png)
![](pll_workshop/CP_waveform2.png)
```
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
```
![](pll_workshop/VCO_cir.png)
![](pll_workshop/VCO_waveform.png)
```
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
```
![](pll_workshop/PD_cir.png)
![](pll_workshop/PD_waveform.png)
## Part11 Steps to combine PLL subcircuits and PLL full design simulation
![](pll_workshop/pll_cir.png)
```
.include sky130nm.lib

xx1 Clk_Ref Clk_Out_by_8 up down pd
xx2 up down VCtrl cp

*Loop Filter
r1 VCtrl rc1 490
c1 rc1 0 355f
r2 rc1 rc2 490
c2 VCtrl 0 350f
r3 rc2 rc3 490
c3 rc3 0 345f
 
xx3 rc3 Clk_Out vco

xx4 Clk_Out Clk_Out_by_2 fd
xx5 Clk_Out_by_2 Clk_Out_by_4 fd
xx6 Clk_Out_by_4 Clk_Out_by_8 fd

v1 Clk_Ref 0 PULSE 0 1.8 0 6ps 6ps 40ns 80ns

.ic v(VCtrl) = 0
.ic v(Clk_Out_by_2) = 0
.ic v(Clk_Out_by_4) = 1.8
.ic v(Clk_Out_by_8) = 0
.control
tran 0.1ns 180us
plot v(Clk_Ref) v(Clk_Out_by_8) v(Clk_Out_by_4)+2 v(Clk_Out_by_2)+4 v(Clk_Out)+6 v(up)+8 v(down)+10 v(VCtrl)+12
.endc

*PFD
.subckt pd Clk1 Clk2 up down 
xm1 1 clk1 3 1 sky130_fd_pr__pfet_01v8 l=150n w=640n 
xm2 3 clk1 4 0 sky130_fd_pr__nfet_01v8 l=150n w=1800n
xm3 4 clk2 0 0 sky130_fd_pr__nfet_01v8 l=150n w=360n

xm4 1 clk2 6 1 sky130_fd_pr__pfet_01v8 l=150n w=640n 
xm5 6 clk2 7 0 sky130_fd_pr__nfet_01v8 l=150n w=1800n
xm6 7 clk1 0 0 sky130_fd_pr__nfet_01v8 l=150n w=360n

xm7 8 clk1 3 0 sky130_fd_pr__nfet_01v8 l=150n w=2400n 
xm8 clk1 clk1 8 1 sky130_fd_pr__pfet_01v8 l=150n w=640n

xm11 upb 8 1 1 sky130_fd_pr__pfet_01v8 l=150n w=720n
xm12 upb 8 0 0 sky130_fd_pr__nfet_01v8 l=150n w=360n

xm15 up upb 1 1 sky130_fd_pr__pfet_01v8 l=150n w=720n
xm16 up upb 0 0 sky130_fd_pr__nfet_01v8 l=150n w=360n
  
xm9 9 clk2 6 0 sky130_fd_pr__nfet_01v8 l=150n w=2400n
xm10 clk2 clk2 9 1 sky130_fd_pr__pfet_01v8 l=150n w=640n

xm13 downb 9 1 1 sky130_fd_pr__pfet_01v8 l=150n w=720n
xm14 downb 9 0 0 sky130_fd_pr__nfet_01v8 l=150n w=360n

xm17 down downb 1 1 sky130_fd_pr__pfet_01v8 l=150n w=720n
xm18 down downb 0 0 sky130_fd_pr__nfet_01v8 l=150n w=360n


*output cap
*c1 up 0 6f
*c2 down 0 6f

v1 1 0 1.8
.ends pd


*CP
.subckt cp up down out
xm43 3 2 1 1 sky130_fd_pr__pfet_01v8 l=150n w=18u 
xm44 out downb 3 1 sky130_fd_pr__pfet_01v8 l=150n w=420n 
xm31 out up 7 0 sky130_fd_pr__nfet_01v8 l=150n w=420n
xm32 7 8 0 0 sky130_fd_pr__nfet_01v8 l=150n w=4.8u

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

*r1 out rc 200
*c1 rc 0 8f

v1 1 0 1.8
.ends cp


*VCO
.subckt vco in 17
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

xm19 1 16 11 1 sky130_fd_pr__pfet_01v8 l=150n w=720n
xm20 11 16 0 0 sky130_fd_pr__nfet_01v8 l=150n w=360n

xm21 1 11 17 1 sky130_fd_pr__pfet_01v8 l=150n w=720n
xm22 17 11 0 0 sky130_fd_pr__nfet_01v8 l=150n w=360n

*c1 11 0 24f
v1 1 0 1.8
.ends vco


*FD
.subckt fd Clk 10

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

xm15 7 6 1 1 sky130_fd_pr__pfet_01v8 l=150n w=720n 
xm16 7 6 0 0 sky130_fd_pr__nfet_01v8 l=150n w=360n

xm19 1 7 10 1 sky130_fd_pr__pfet_01v8 l=150n w=720n
xm20 10 7 0 0 sky130_fd_pr__nfet_01v8 l=150n w=360n 


*c1 7 0 18f
v1 1 0 1.8
.ends fd
.end
```

![](pll_workshop/pll_waveform.png)
![](pll_workshop/pll_waveform2.png)
## Part12 Troubleshooting steps
Observe the kinds of issue faced.
Debug individual components fully before trying to debug the combined simulation.
Check if any signals are coming flat or if the simulation is crashing. If this is the case then check if all the connections are done properly. Also check if there are any issues like wrong naming, capitalization issues or parameter value issues.
If the signals are right but the mimicking is not happening then verify the following:
For what range of frequencies is the VCO working properly: Is our required output frequency range lying in the VCO's working range.
Is the Phase Frequency Detector able to detect the differences: If the phase difference is very small, the PFD might not be able to detect it.
Is the rate of Charge Pump output charging and discharging fast: Is it too fast or too slow? Is there too much fluctuations in charging or discharging? This means that the transistor sizing is the thing to pay attention to. Check the response of the CP when 0V is given as the input. If it is still charging then the charge leakage is the issue.
Whether the loop filter values are working out: This can be found out using the thumb rules.
## Part13 Layout design
The way to draw here in magic is first to make a box and then fill it with a material.

By left clicking, we fix the left bottom corner of the box.

By right clicking, we fix the right top corner of the box.

If we hover upon a layer (layers panel is present on the right hand side of the window), we can see the name of the layer on the top right corner of the screen.

By middle clicking on a layer, the box gets filled by the selected layer.

Materials needed for the transistors:

P-diffusion for the PMOS and N-diffusion for the NMOS.
For the gate, polysilicon layer is needed.
DRC error means that there is a size issue or a closeness issue. To know what exactly the issue is, select some part of the error region and press the "?" button on the keyboard and then the error will show up on the "magic command window".

Before creating a PMOS, we must create an N-Well region and then place the PMOS over it.

To copy a transistor, make a box around the transistor and press "A" button on the keyboard. Now, place the cursor where we wish to copy it and press "C" button. If we press the "M" button then it becomes the move operation.

For placing Vdd and ground, place the metal1 layer.

To connect two transistors, use the loacal interconnect layer (locali) like a wire.

To connect two layers, look for contact layers in the tray. They are represented with a cross symbol.

As long as we do not connect a contact, two different layers will not touch even if they overlap.

If we get a DRC error at this point it means that either the size of the contact is too small or the region of metal1 and local interconnect around the contact is not enough.

To create a label, draw a line around the edge of the layer that we want to label. A line is drawn by making a box of zero thickness. Type the command label name in the magic command window. Name can be any name to be used as label.

To make a port, draw a box around the label and type port make in the magic command window.

We need to make ports because when we extract the parasitics from our design, the input and output ports will automatically have these names as we have labelled them as ports.
## Part14 Layout Walkthrough
![](pll_workshop/FD_layout.png)
![](pll_workshop/CP_layout.png)
![](pll_workshop/VCO_layout.png)
![](pll_workshop/PFD_layout.png)
## Part15 Parasitic Extraction
![](pll_workshop/PFD_layoutspice.png)
## Part16 Post Layout simulations
![](pll_workshop/PFD_postwaveform.png)
![](pll_workshop/PFD_postwaveform2.png)
## Part17 Steps to combine layouts
![](pll_workshop/PLL_layout.png)
## Part18 Tapeout theory
![](pll_workshop/pll_workshops.png)
## Part19 Tapeout labs
![](pll_workshop/pll_workshops.png)
Download the layout file for the analog user project area.

Once downloaded, extract it to the directory of your choice.

Now, open it with magic and the technology file sky130A.tech

Place the design in the user project area and make the connections with the pins.

It is important to not that one must not move any of the pins that they have given from their location. We only use the pins that we need and connect our design to them.

At the bottom of the user project area are the wishful ports.

Along the left of the user project area are the input-output ports and some power ports like Vdd and Vss.

Along the right of the user project area are a few more input-output ports and power ports like Vccd, Vssd, Vcca and Vssa.

At the top of the user project area are the analog input-output pins.

In our design, we have five pins:

Enable pins for CP and VCO.
Reference clock input pin.
Output clock pin.
VCO direct input pin.
The enable pins are connected to the digital input-output pins.

Reference clock input pin and output clock pin are also digital and so we connect them to the digital input-output pins.

VCO direct input pin is control voltage pin of analog nature. So, we need to connect it to an analog input-output pin.

Considering this scenarion, we place our design at the top-right corner and make the connections to the pins using wire tool and contact layers.
## Acknowledgement
1. Kunal Ghosh, Co-founder, VSD Corp. Pvt. Ltd. - kunalpghosh@gmail.com
2. Chinmay panda, IIT Hyderabad
3. Sameer Durgoji, NIT Karnataka
4. [Synopsys Team/Company](https://www.synopsys.com/)
5. https://www.iith.ac.in/events/2022/02/15/Cloud-Based-Analog-IC-Design-Hackathon/
## Conclusion
I. S. Dhanjal. 4 bit carry look ahead adder transistor level
implementation using static cmos logic.
https://youtu.be/WItAXzrfPrE

M. Hasan. High-performance design of a 4-bit carry look-ahead adder
in static cmos logic.
http://section.iaesonline.com/index.php/IJEEI/article/view/2582
  

