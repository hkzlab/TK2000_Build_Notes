# TK2000 Build Notes

## Introduction
This document collects my notes on the building of a TK2000 clone from [here](https://github.com/clemarfolly/Microdigital-TK2000). Note that I am building a Rev. 1 board.

The BOM provided with the project is incomplete, sometimes not matching original boards and the clone also needed a few mods to work properly.
Plus, I made some changes of my own to better match the Apple II+ schematics.

I also wired the board to use 4164, 5V-only powered DRAMs.

## BOM Changes

### Missing diodes in series with R69/R71

The original schematic has the emitter of Q5 and Q6 connected to GND, while original boards (and the clone) have the base connected to -12V.
Original boards also introduce a protection diode in series with R69 and R71, which is not indicated in the clone's BOM.

The direction indicated in the clone's schematic is also wrong, and the diode should be facing the other direction as the one presented.

The diodes I have used here are 1N4148, for a lack of better information on what is used on original boards.

![R69 and R71 with diode in series](pics/fixes/resistor_diodes.jpg)

### -5V regulation circuit

The TK2000 regulates the -5V using a zener diode. Both diode type and series resistor are left unspecified in the schematic. 

Given that -5V is used only for the negative bias of the LM741 op-amp and on the external bus (where I expect the board making use of it will also have similarly low load), I decided to fit the following:

- D12 - TZX5V1A
- R72 - 750ohm

I expect to handle loads of slightly less of 10mA this way, and it has worked for now.

### Other changes

- L1 - 27uH inductor (clone's schematic has 47uH, Apple II and original boards show 27uH inductors)
- R17 - 1Kohm (the schematic has 2K, photos of original boards show 1K resistors)
- J2 must be closed to provide 5V to DRAMs
- J13 should be closed by default on original boards
- J12 should be closed by default on original boards
- J4 for 4164, this must be closed to bring the muxed A7 line to pin 9 of the DRAMs. Also, remember to **REMOVE or NOT TO INSTALL C13 and C17** if you close this!


## Fixes

### Cassette input


## Mods

### Apple II video amplifying circuit

I replaced part of the video circuit in the schematic (which doesn't have most of the values for components indicated) with the video circuit from the Apple II.

- Q3 - 2N3904 (remember to rotate the transistor to account for different pinout). This change is probably not needed.
- R23 - 10 ohm
- R46 - 20 ohm
- R47 - 180 ohm

Then I cut the trace going from the junction of R46 and R47 to the center of the coax connector, and replaced it with a 27 ohm resistor in series with a 2.7 uH inductor.

![Underside of the monitor connection with resistor + inductor mod](pics/mods/video_resistor_inductor.jpg)

### Clock generation circuit

I did not have BF494B transistors on hand, and the schematic also did not indicate values for the capacitors in the clock circuit, so I peformed the following replacements

- Q2 - 2N2222A (probably not necessary if you have the proper transistor), remember to account for the different pinout and twist the legs of the replacement properly
- C46 - 10pF, note that depending on the crystal You're using, you might have to tweak the value of this
- C49 - 100pF, same as above
- C50 - 10pF

I used [these](https://www.mouser.it/ProductDetail/815-ABL-14.31818B2) crystals for my NTSC modded board.

![Closeup photo of the transistor at Q2](pics/mods/clock_transistor.jpg)

