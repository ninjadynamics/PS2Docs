

# SPU2 Overview


Copyright © 2002 Sony Computer Entertainment Inc.
All Rights Reserved.
SCE Confidential




SCE CONFIDENTIAL SPU2 Overview Version 6.0


© 2002 Sony Computer Entertainment Inc.


Publication date: April 2002


Sony Computer Entertainment Inc.


1-1, Akasaka 7-chome, Minato-ku


Tokyo 107-0052 Japan


Sony Computer Entertainment America
919 East Hillsdale Blvd.
Foster City, CA 94404, U.S.A.


Sony Computer Entertainment Europe
30 Golden Square
London W1F 9LD, U.K.


The SPU2 Overview is supplied pursuant to and subject to the terms of the Sony Computer Entertainment
PlayStation® license agreements.


The SPU2 Overview is intended for distribution to and use by only Sony Computer Entertainment licensed
Developers and Publishers in accordance with the PlayStation® license agreements.


Unauthorized reproduction, distribution, lending, rental or disclosure to any third party, in whole or in part, of
this book is expressly prohibited by law and by the terms of the Sony Computer Entertainment PlayStation®
license agreements.


Ownership of the physical property of the book is retained by and reserved by Sony Computer Entertainment.
Alteration to or deletion, in whole or in part, of the book, its presentation, or its contents is prohibited.


The information in the SPU2 Overview is subject to change without notice. The content of this book is
Confidential Information of Sony Computer Entertainment.


® and PlayStation® are registered trademarks, and GRAPHICS SYNTHESIZERTM and


© SCEI


- 2-




SCE CONFIDENTIAL SPU2 Overview Version 6.0


## About This Manual


The "SPU2 Overview" describes the functions, configuration, and sound generation mechanisms of the SPU2,
the sound processor for the PlayStation 2.


- Chapter 1 "Overview of SPU2" describes the configuration and main functions of the SPU2 and the format of waveform data used as a sound source.

- Chapter 2 "Sound Generation" describes voice generation using waveform data as a sound source, sound data stream input/output, mixing, and effects. - Chapter 3 "Register List" describes the registers that control the SPU2. - Chapter 4 "Appendix" shows the volume variation rates for values of the envelope rate parameters.


## Changes Since Release of 5th Edition


Since release of the 5th Edition of the SPU2 Overview Manual, the following changes have been made.
Note that each of these changes is indicated by a revision bar in the margin of the affected page.


## Ch. 2: Sound Generation


- Section 2.2.6. Monaural Output, has been added on page 29.

- A correction has been made to the description following Figure 2-12 Sound Data Output on page 31.


## Ch. 4: Appendix


- A correction has been made to the "Time" heading in the Exponential Decrement Mode table on page 76.

- A correction has been made to the "0.1110" row in the Linear Decrement Mode table on page 78.

- 3-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


(This page is left blank intentionally)


- © SCEI

- 4-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


## Glossary


<table>
 <tr>
  <th>Term</th>
  <th>Definition</th>
 </tr>
 <tr>
  <td>EE</td>
  <td>Emotion CPU of the 2.</td>
 </tr>
 <tr>
  <td>EE Core</td>
  <td>Generalized and control unit of EE. Core of the CPU.</td>
 </tr>
 <tr>
  <td>COP0</td>
  <td>EE Core control</td>
 </tr>
 <tr>
  <td>COP1</td>
  <td>EE Core Also referred to as FPU.</td>
 </tr>
 <tr>
  <td>COP2</td>
  <td>Vector unit as a of EE Core. VPU0.</td>
 </tr>
 <tr>
  <td>GS</td>
  <td>Graphics Synthesizer.<br/>Graphics processor connected to EE.</td>
 </tr>
 <tr>
  <td>GIF</td>
  <td>EE Interface unit to GS.</td>
 </tr>
 <tr>
  <td>IOP</td>
  <td>Processor connected to EE for controlling input/output devices.</td>
 </tr>
 <tr>
  <td>SBUS</td>
  <td>Bus connecting EE to IOP.</td>
 </tr>
 <tr>
  <td>VPU (VPU0/VPU1)</td>
  <td>Vector operation unit.<br/>EE contains 2 VPUs: VPU0 and VPU1.</td>
 </tr>
 <tr>
  <td>VU (VU0/VU1)</td>
  <td>VPU core unit.</td>
 </tr>
 <tr>
  <td>VIF (VIF0/VIF1)</td>
  <td>VPU data unit.</td>
 </tr>
 <tr>
  <td>VIFcode</td>
  <td>Instruction code for VIF.</td>
 </tr>
 <tr>
  <td>SPR</td>
  <td>Quick-access data memory built into EE Core memory).</td>
 </tr>
 <tr>
  <td>IPU</td>
  <td>EE unit.</td>
 </tr>
 <tr>
  <td>word</td>
  <td>Unit of data 32 bits</td>
 </tr>
 <tr>
  <td></td>
  <td>Unit of data 128 bits</td>
 </tr>
 <tr>
  <td>Slice</td>
  <td>Physical unit of DMA transfer: 8 or less</td>
 </tr>
 <tr>
  <td>Packet</td>
  <td>Data to be handled as a unit for transfer</td>
 </tr>
 <tr>
  <td>Transfer list</td>
  <td>A of transferred in serial DMA transfer</td>
 </tr>
 <tr>
  <td></td>
  <td>Additional data data size and other attributes of</td>
 </tr>
 <tr>
  <td>DMAtag</td>
  <td>Tag positioned first in DMA packet to indicate address/size of data and address<br/>of the</td>
 </tr>
 <tr>
  <td>GS</td>
  <td>Data to indicate elements such as and</td>
 </tr>
 <tr>
  <td>Context</td>
  <td>A set of drawing information (e.g. texture, distant fog color, and dither matrix)<br/>applied to two or more primitives uniformly. Also referred to as the drawing<br/>environment.</td>
 </tr>
 <tr>
  <td>GIFtag</td>
  <td>Additional data to indicate attributes of GS primitives.</td>
 </tr>
 <tr>
  <td>Display list</td>
  <td>A group of GS primitives to indicate batches of images.</td>
 </tr>
</table>


- 5-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


(This page is left blank intentionally)


- © SCEI

- 6-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


## Contents


1. Overview of SPU2...................................................................................................................................................................9 1.1. Features of SPU2...........................................................................................................................................................10 1.1.1. Core..........................................................................................................................................................................10 1.1.2. Sound Data Input Function..................................................................................................................................10 1.1.3. Voice Processing Function ...................................................................................................................................10 1.1.4. Sound Data Output Function...............................................................................................................................11 1.1.5. Digital Effect Processing.......................................................................................................................................11 1.1.6. Local Memory.........................................................................................................................................................11 1.1.7. External Output......................................................................................................................................................12 1.2. Local Memory ................................................................................................................................................................13 1.2.1. Data Allocated in Local Memory.........................................................................................................................13 1.2.2. Addressing in Local Memory................................................................................................................................14 1.2.3. Interrupt by Access................................................................................................................................................14 1.3. Waveform Data Format................................................................................................................................................15 1.3.1. Waveform Data Block...........................................................................................................................................15 1.3.2. Endpoint..................................................................................................................................................................15 1.3.3. Loop Processing.....................................................................................................................................................16 1.4. Reset ................................................................................................................................................................................17 2. Sound Generation .................................................................................................................................................................19 2.1. Voice Processing............................................................................................................................................................20 2.1.1. Sound Sources.........................................................................................................................................................20 2.1.2. Pitch Transformation.............................................................................................................................................22 2.1.3. Pitch Modulation....................................................................................................................................................23 2.1.4. Envelope..................................................................................................................................................................24 2.1.5. Volume ....................................................................................................................................................................25 2.1.6. Key-On/Key-Off...................................................................................................................................................26 2.1.7. Mixing Switch .........................................................................................................................................................27 2.2. Sound Data Input Processing.......................................................................................................................................28 2.2.1. Sound Data Input Area .........................................................................................................................................28 2.2.2. Volume Processing.................................................................................................................................................28 2.2.3. Mixing with Voice Output ....................................................................................................................................29 2.2.4. Bypass Processing (CORE0) ................................................................................................................................29 2.2.5. 32-bit Sound Data Input (CORE1) .....................................................................................................................29 2.2.6. Monaural Output....................................................................................................................................................29 2.3. Sound Data Output Processing ...................................................................................................................................30 2.4. Mixing..............................................................................................................................................................................32 2.5. Digital Effect Processing ..............................................................................................................................................33 2.5.1. Signal Flow for Digital Effect Processing...........................................................................................................33 2.5.2. Work Area for Digital Effect Processing............................................................................................................33 2.5.3. Effect Volume ........................................................................................................................................................34

- © SCEI

- 7-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


1. Master Volume ...............................................................................................................................................................35 2.7. Interrupt Processing.......................................................................................................................................................36

2. Register List ............................................................................................................................................................................39 3.1. Classification of Registers..............................................................................................................................................40 3.2. Registers in Pairs.............................................................................................................................................................41 VOLL / VOLR : Voice volume......................................................................................................................................42 PITCH : Pitch when sound is generated........................................................................................................................44 ADSR1 / ADSR2 : Envelope..........................................................................................................................................45 ENVX : Current value of envelope................................................................................................................................46 VOLXL / VOLXR : Current value of volume.............................................................................................................47 PMON0 / PMON1 : Pitch modulation specification..................................................................................................48 NON0 / NON1 : Voice allocation to noise generator................................................................................................49 VMIX* : Mixing specification of voice output .............................................................................................................50 MMIX : Output specification after voice mixing..........................................................................................................51 IRQAH / IRQAL : Interrupt address specification.....................................................................................................52 KON0 / KON1 : Key-on specification.........................................................................................................................53 KOF0 / KOF1 : Key-off specification..........................................................................................................................54 TSAH / TSAL : Transfer start address..........................................................................................................................55 SSAH / SSAL : Starting address of waveform data .....................................................................................................56 LSAXH / LSAXL : Address of loop point ...................................................................................................................57 NAXH / NAXL : Address of waveform data to be read next...................................................................................58 ESAH / ESAL : Starting address in the work area for effect processing .................................................................59 EEAH : End address in the work area for effect processing......................................................................................60 ENDX0 / ENDX1 : Endpoint passing flag .................................................................................................................61 MVOLL / MVOLR : Master volume ............................................................................................................................62 EVOLL / EVOLR : Return volume of effect..............................................................................................................64 AVOLL / AVOLR : Volume for external input ..........................................................................................................65 BVOLL / BVOLR : Volume for sound data input......................................................................................................66 MVOLXL / MVOLXR : Current value of master volume.........................................................................................67 4. Appendix.................................................................................................................................................................................69 4.1. Rate Parameter Table.....................................................................................................................................................70 4.1.1. +Lin Mode...............................................................................................................................................................71 4.1.2. -Lin Mode ...............................................................................................................................................................72 4.1.3. +Exp Mode (Normal phase).................................................................................................................................73 4.1.4. +Exp Mode (Reverse phase).................................................................................................................................74 4.1.5. -Exp Mode..............................................................................................................................................................75 4.1.6. Decay Rate (DR).....................................................................................................................................................76 4.1.7. Sustain Level (SL) ...................................................................................................................................................77 4.1.8. -Lin Mode for Release Rate (RR).........................................................................................................................78 4.1.9. -Exp Mode for Release Rate (RR) .......................................................................................................................79

- © SCEI

- 8-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


This chapter provides an overview of the SPU2.


## 1. Overview of SPU2


- 9-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


## 1.1. Features of SPU2


The SPU2 is a sound synthesis processor, which is composed of two cores. The SPU2 also contains local
memory and external I/O.
The two cores have the ability to:


- Reproduce sound data input successively from the host.

- Process voices.

- Output voice-processed sound data to the host successively.

- Perform digital effect processing.


The following sections describe the SPU2 components and core functions.


## 1.1.1. Core


The cores (CORE0 and CORE1) are the basic components of the SPU2, each having a sound generation
function with 24 voices. They operate at a frequency of 36.864 MHz, and have a sound generation resolution of
48 kHz. The unit of processing, 1/48000 second, is represented as 1Ts.
When setting the same register successively (e.g. when varying the pitch of a sound consecutively to realize a
portamento), write operations to the register must be at least 1Ts apart. (Write operations to some registers
must be at least 2 Ts apart. For details, refer to the Description for each register in "3. Register List".) If the
register is written in less than the specified time interval (less than 1 Ts when not specified), the SPU2 operations
become indeterminate, and expected results cannot be obtained. This produces serious effects, particularly on
registers working as a switch, such as key-on or key-off.
CORE0 and CORE1 are functionally equal and operate independently. They are connected in such a way that
the output from CORE0 is input to CORE1 and the final mixed sound is output from CORE1.


## 1.1.2. Sound Data Input Function


The sound data input function processes 16-bit or 32-bit data strings transferred successively from the host to
the SPU2 as sound data, and outputs them by mixing with the voice-processed output. CORE0 and CORE1
each have one stereo input channel.
The input buffer is a reserved area in the local memory. To transfer data smoothly, it uses a double buffer
function, which requests a data transfer when half of the area is processed.
For details, refer to "2.2. Sound Data Input Processing".


## 1.1.3. Voice Processing Function


Sound in the SPU2 is generated in units of voices. Each core has 24 voices, so the whole SPU2 can generate 48
voices.
Each voice has waveform data compressed by ADPCM as a sound source. After pitch transformation, pitch
modulation and envelope processing, the outputs of each voice are mixed and become the final sound output.


- © SCEI

- 10-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


## Sound Source


The sound source is waveform data that has been compressed by ADPCM, and is decoded (decompressed)
by hardware at a sampling rate of 48 kHz. The noise generator of each core can be used as a sound source as
well.


## Pitch Transformation


Sound can be generated by varying the pitch of the sound source within the range of -12 octaves to +2
octaves.


## Pitch Modulation


The pitch of the sound source can be modulated by using the crest value of another voice.


## Envelope Processing


The envelope, which controls volume variation from key-on to key-off, is specified with five parameters:
Attack Rate, Decay Rate, Sustain Rate, Sustain Level and Release Rate. For Attack Rate, Sustain Rate and
Release Rate, non-linear variation can be specified.


## Voice Volume


Volume can be set for the L channel and R channel of each voice. Constant, linear and exponential variation
curves can be selected.


## Mixing/Switching


24 voices/stereo output (48 channels in total) are synthesized into 2 stereo units in each core. Each voice
can be added to the output or not.


Refer to "2.1. Voice Processing" for details of voice processing.


## 1.1.4. Sound Data Output Function


Mixed sounds (2 stereo units per core) and a specified two-channel voice can be output to the host successively.
This allows the sound data generated by the SPU2 to be processed by the host processor.
An output buffer is reserved in the local memory. In order to transfer data smoothly, it uses a double buffer
function, which requests a data transfer when half of the area is processed.
Refer to "2.3. Sound Data Output Processing for details.


## 1.1.5. Digital Effect Processing


Digital effects such as reverb, echo and delay can be applied to the mixed sounds.
Although digital effects can be processed independently in each core, the effects in CORE1 can be reapplied to
the final output from CORE0.
The work area for digital effect processing is in the local memory.
Refer to "2.5. Digital Effect Processing" for details.


## 1.1.6. Local Memory


The SPU2 has 2 MBytes (16 Mbits) of local memory for its exclusive use. This memory is used as a buffer for
sound data I/O, a waveform data area for voice processing, and a work area for digital effect processing. The
remaining memory can be used freely.


- 11-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


It is possible to access the local memory from the host via DMA transfer or single transfer. Sound generation
never stops when transferring data, but it might not be correctly executed when overwriting waveform data
being used by the SPU2. To avoid this, an interrupt can be generated for the host when each core accesses a
specific address in the local memory.
Refer to "1.2. Local Memory" for details.


## 1.1.7. External Output


The SPU2 adopts the following functions as methods of outputting final sounds.


- Digital output through S/PDIF (Sony/Philips Digital Interface)

- Analog output through D/A converter

- © SCEI

- 12-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


## 1.2. Local Memory


The local memory is memory for the SPU2's exclusive use; it is used as a data I/O buffer with the host and as a
work area of the SPU2.


## 1.2.1. Data Allocated in Local Memory


The local memory is divided into the following four areas.


Host Sound Data
Host Memory Output Area


Sound Data
Input Area


Local
SPU2 Memory


Address
(in short-word units)
00 0000


00 1FFF
00 2000


00 27FF
00 2800


Waveform
Data Area


0X XXXX


0? FFFF


Work Area for
Digital Effect
Delay
Processing


0y yyyy
0F FFFF


(2 MB)


Figure 1-1 Memory Allocation and Addressing in Local Memory


## Sound Data Input Area


This is the sound data input buffer from the host; its address is fixed.
Sound data is successively written from the host, and the written data is sequentially read via hardware and
processed as sound data by the SPU2.


## Sound Data Output Area


This is the sound data output buffer from the SPU2 to the host; its address is fixed.
Sound data generated in the SPU2 is written successively, and is readable from the host sequentially.


- 13-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


processing
locations.
start
restrictions


allocated
(short
lower
22
show
which
addressing
a


Address L
Lower Address Register


15


## YYYY


XX 0000


128 KB


## XX YYYY


XX FFFF


2 MB


0


0F FFFF


Figure 1-2 Addressing


## 1.2.3. Interrupt by Access


When one of the cores accesses a specific address in the local memory, an interrupt can be generated for the
host. The address for generating an interrupt can be set, one per core, with the IRQAH and IRQAL registers.
Interrupts generated by both cores are detected at the same time on the host. A function provided by the sound
library represents which core has generated the interrupt.


CORE0
[IRQAH/L]


Local Memory


Arbitrary
Processing


Figure 1-3 Interrupt by Process Access


(Example: Specification in CORE0)


For details, refer to "2.7. Interrupt Processing".


Interrupt to Host


- © SCEI

- 14-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


## 1.3. Waveform Data Format


The waveform data that becomes the sound source of each voice is in a format unique to the SPU2, by adopting
ADPCM as a compression method.


## 1.3.1. Waveform Data Block


Waveform data is configured in units of 16-byte blocks, each of which includes a 16-bit header and 28 4-bit
samples. The following attributes are included in the header.


<table>
 <tr>
  <th>Field</th>
  <th></th>
  <th>Bit Position</th>
  <th>Contents</th>
  <th></th>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td>10</td>
  <td>Loop point information</td>
  <td></td>
 </tr>
 <tr>
  <td>LOOP</td>
  <td></td>
  <td>9</td>
  <td>Loop existence/non-existence</td>
  <td></td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td>8</td>
  <td>Endpoint information</td>
  <td></td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td>7:0</td>
  <td>Parameter for decoding</td>
  <td></td>
 </tr>
 <tr>
  <td></td>
  <td>15</td>
  <td>11</td>
  <td>7 3</td>
  <td>0</td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
  <td>Decode Parameter</td>
  <td></td>
 </tr>
 <tr>
  <td></td>
  <td>SD3</td>
  <td>SD2</td>
  <td>SD1 SD0</td>
  <td></td>
 </tr>
 <tr>
  <td></td>
  <td>SD7</td>
  <td>SD6</td>
  <td>SD5 SD4</td>
  <td></td>
 </tr>
 <tr>
  <td></td>
  <td>SD11</td>
  <td>SD10</td>
  <td>SD9 SD8</td>
  <td></td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td>SD14</td>
  <td>SD13 SD12</td>
  <td></td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td>SD18</td>
  <td>SD17 SD16</td>
  <td></td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td>SD22</td>
  <td>SD21 SD20</td>
  <td></td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td>SD26</td>
  <td>SD25 SD24</td>
  <td></td>
 </tr>
</table>


Sound
Sample
Data


Loop Flag


15


(reserved)


11 10
L
O
O
P
/
S
T
A
R
T


9 8
L L
O O
O O
P P
/
E
N
D


Figure 1-4 1 Block of Waveform Data


## 1.3.2. Endpoint


The number of blocks of waveform data is arbitrary. By setting the LOOP/END bit of the header to 1, the
endpoint is specified, showing the position where the waveform data ends.
When sound generation reaches the block specified as the endpoint, the last sample data of the block is
processed, and then sound generation moves to the block which has the loop point specification immediately
before (i.e. the block shown by the LSAXH/L register). When no loop is specified, the last sample data of the
block is processed, and then muting is applied to the voice in process by hardware. As a result, sound
generation of the voice stops.


- 15-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


## 1.3.3. Loop Processing


By setting the LOOP/START bit of the header to 1, the loop point is specified. The header address is
maintained in the LSAXH/L register when sound generation moves to this block, and sound generation moves
to the first sample data of this block after processing the endpoint block.
Only 1 loop point specification can be in effect in a set of waveform data. If there are two or more loop point
blocks, the block closest to the endpoint becomes the loop point when sound is actually generated.
If an address is set in the LSAXH/L register after sound generation has started, the loop point specification in
the waveform data is disregarded until the next time the voice is keyed on, and the set address becomes the loop
point.
For waveform data with a loop point set, the LOOP bit of the header must be set to 1 in all blocks.


[SSAH / L]


Start Block


LOOP=1


<table>
 <tr>
  <th>Loop Point Block</th>
  <td></td>
 </tr>
 <tr>
  <th>Endpoint Block</th>
  <td>LOOP=1, LOOP/END=1</td>
 </tr>
</table>


LOOP=1, LOOP/START=1


[LSAXH / L]


Figure 1-5 Loop Processing


- © SCEI

- 16-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


## 1.4. Reset


The sound library provides a resetting feature. In resetting, neither register values nor data in the local memory
is guaranteed.


- 17-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


(This page is left blank intentionally)


- © SCEI

- 18-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


This chapter describes sound generation in each core.


## 2. Sound Generation


- 19-

- © SCEI




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


## 2.1. Voice Processing


Voice processing generates sound primarily by decoding waveform data and varying the decoded sound data by
time.
The generated sound is transferable to the host as data, and can also be processed on the host.
The entire flow of voice processing is shown as follows:


OUTX(i-1)


Local Memory


Sound
Data
Output


[VOLL]
[VOLXL]
VOL(L)
Control


[PMON]


[PITCH]


Pitch [ADSR1]
[ADSR2] [VMIXL]
Modulation
Envelope
Control


ƒ°


Dry L


Local Memory


ƒ°


Waveform ADPCM Pitch
data Decode Control


Wet L


[VMIXEL]


ƒ°


Dry R


[NON] [VMIXR]


Noise
Generator


ƒ°


Wet R


[SSAH/L]
[LSAXH/L]


[VMIXER] [MMIX]
VOL(R)
Control


[VOLR]
[VOLXR]


OUTX(i)


Figure 2-1 Voice Processing


## 2.1.1. Sound Sources


Waveform data stored in the local memory or the noise generator (each core has 1 unit) can be used as a sound
source for each voice. This selection is specified by the NON0/1 register.


<table>
 <tr>
  <th rowspan="2">Waveform Data</th>
  <td colspan="2">[PITCH]</td>
 </tr>
 <tr>
  <td></td>
  <td>ADPCM Pitch<br/>Decoding Processing</td>
 </tr>
 <tr>
  <th colspan="2">Local Memory</th>
  <td>Noise Generator</td>
 </tr>
</table>


[SSAH / L]


[PMON0/1]


[NON0/1]


Figure 2-2 Switching between Waveform Data and Noise Generator


- © SCEI

- 20-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


When waveform data is the sound source, the address in the local memory where the waveform data is located is
specified by the SSAH/L register of the voice attribute. Specify the location of the header in the waveform data
block to the SSAH/L register.
Since each voice generates a single tone, it is necessary to vary the pitch of the same sound in two or more
voices to generate a chord. In this case, however, the same waveform data address is specified to the SSAH/L
register in each voice.


The waveform data is decoded by hardware. The user can know the decoding progress from each of the
following registers:


- Address of the waveform data to be read next (NAXH/L register)

- Header address in the loop point block (LSAXH/L register: after passing the loop point) • Endpoint block passing flag (ENDX0/1 register)


The NAXH/L register is incremented as decoding advances and shows up to which sample data the sound
generation has been completed. That is, sound processing has been completed up to the address immediately
preceding the one indicated by the NAXH/L register.
Since the waveform data includes the header, however, the above does not mean all the addresses before the one
indicated by the NAXH/L register have been processed. When replacing the sound-generated waveform data,
replacements can be made in block units up to the block immediately before the one having the address
specified by the NAXH/L register. (However, if a replacement is made at a place between the loop point and
endpoint in waveform data including loop processing, decoding becomes discontinuous and might cause a
noise.)
When decoding advances to the loop point block, the header address in the block is written to the LSAXH/L
register. The loop point can be changed by rewriting the value of this register while sound is being generated 4
Ts after the key-on. (Rewriting within 4 Ts is disregarded.) When rewriting, specify the location of the header in
the waveform data block in the LSAXH/L register. After the change, the address in this LSAXH/L register is
used as a loop point, and the loop point information in the header of the waveform data is disregarded until the
next time the voice is keyed on.


When decoding of the endpoint block is finished, regardless of the presence of the loop, the bit corresponding
to the voice is set to 1 in the ENDX register and kept until the next key-on.


- 21-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


SSAH/L


LOOP/START=1


NAXH/L


ENDX0/1


1 LOOP/END=1


Figure 2-3 Sound Generation Status


## 2.1.2. Pitch Transformation


Sound can be generated from waveform data by varying the pitch within the range of -12 to +2 octaves. The
pitch transformation is specified by the PITCH register of voice attribute.
Assuming the original pitch of the sound source is f0, the value of the PITCH register is [PITCH], and the
finally generated sound is f, the following expression is met:


f = [


f0 ]


PITCH
212


That is, sound is generated to the pitch of the original sound by specifying 0x1000(=212) in the PITCH register.


The above relationship is met only when the waveform data is sampled at 48 kHz. The sound from waveform
data sampled at a rate other than 48 kHz (24 kHz, for example) is generated one octave higher than the original
sound when specifying 0x1000 in the PITCH register to generate sound.
Assuming the sampling rate to be s kHz, the above-mentioned expression is expanded as follows.


48 PITCH
f = [
s 212


f0 ]


If an appropriate value is specified to the PITCH register according to this expression, sound can be generated
to the pitch of the original sound even from the waveform data whose sampling rate is not 48 kHz. However,
the acoustic characteristic varies along with pitch transformation processing.


- © SCEI

- 22-




<table>
 <tr>
  <th>SCE CONFIDENTIAL</th>
  <td></td>
  <td></td>
  <td></td>
  <td>SPU2 Overview Version 6.0</td>
 </tr>
 <tr>
  <th></th>
  <td></td>
  <td>Pitch of</td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th>-2 octaves</th>
  <td>-1 octave</td>
  <td>Original Sound</td>
  <td>+1 octave</td>
  <td>+2 octaves</td>
 </tr>
 <tr>
  <th>0x400</th>
  <td>0x800</td>
  <td>0x1000</td>
  <td>0x2000</td>
  <td>0x3fff [PITCH]</td>
 </tr>
</table>


Figure 2-4 Pitch Transformation of Waveform Data Sampled at 48 kHz
Pitch of


-1 octave Original Sound +1 octave
0x400 0x800 0x1000 [PITCH]


Figure 2-5 Pitch Transformation of Waveform Data Sampled at 24 kHz


When the sound source is the noise generator, the acoustic pitch can be specified in each core by using the
sound library. When there are two or more voices allocated to the noise generator in each core, their sound will
be all generated at the same acoustic pitch.


The speed of sound generation advance changes with the specification of the pitch. The lower the pitch is set,
the slower the advance in the transition of the waveform data address shown by the NAXH/L register becomes.


## 2.1.3. Pitch Modulation


In two voices with consecutive voice numbers, voice n can be modulated by using the output value from voice


n-1.


The value used for modulation is the product of the crest value immediately after decoding and the envelope
value in the waveform data for voice n-1, which is 1Ts before in terms of time. This is called the OUTX of
voice n-1. OUTX is not reflected in the register.


<table>
 <tr>
  <th rowspan="2">Waveform Data<br/>Decoding</th>
  <td></td>
  <td rowspan="2">Pitch<br/>Transformation</td>
  <td></td>
  <td rowspan="2">Envelope<br/>Processing</td>
  <td></td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th colspan="6" rowspan="2">before (OUTX)<br/>[PITCH] [PMON0/1.Vn]</th>
 </tr>
 <tr>
 </tr>
 <tr>
  <th rowspan="2">Waveform Data<br/>Decoding</th>
  <td></td>
  <td rowspan="2">Pitch<br/>Transformation</td>
  <td></td>
  <td rowspan="2">Envelope<br/>Processing</td>
  <td></td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
 </tr>
</table>


## Voice n-1


Voice n


[PITCH]


Processing
1Ts


Figure 2-6 Pitch Modulation


- 23-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


Assuming the crest value of voice n-1 used for modulation of voice n to be OUTX and the pitch of voice n to
be P, then P', the value to be used for pitch transformation of voice n, is decided by the following expression.


P' = P (1 + OUTX)


Whether pitch modulation is performed or not can be specified with the PMON0/1 register. When not
performed, the result becomes the same as the case for OUTX=0.


## 2.1.4. Envelope


The envelope specifies the volume variation by time from key-on to key-off according to the following five
parameters.


<table>
 <tr>
  <th>Parameter</th>
  <td>Code</td>
  <td></td>
 </tr>
 <tr>
  <th>Attack Rate</th>
  <td>AR</td>
  <td>after</td>
 </tr>
 <tr>
  <th>Rate</th>
  <td>DR</td>
  <td>Attenuation from the maximum value</td>
 </tr>
 <tr>
  <th>Sustain Level</th>
  <td>SL</td>
  <td>Transition from to Sustain</td>
 </tr>
 <tr>
  <th>Sustain Rate</th>
  <td>SR</td>
  <td>Attenuation (or increment) from Sustain Level<br/>to</td>
 </tr>
 <tr>
  <th>Release Rate</th>
  <td>RR</td>
  <td>Attenuation after key-off</td>
 </tr>
</table>


For Attack Rate, Sustain Rate and Release Rate, curves of variation by time can be selected. Attack Rate and
Release Rate can use two kinds and Sustain Rate can use four kinds of curves as shown in the table below.
Decay Rate is fixed to an exponential decrement curve.


<table>
 <tr>
  <th>Parameter</th>
  <th>Selection</th>
  <th>Set Value</th>
 </tr>
 <tr>
  <td rowspan="2">Attack Rate</td>
  <td>Linear increment</td>
  <td>ADSR1.X=0</td>
 </tr>
 <tr>
  <td>Pseudo increment</td>
  <td>ADSR1.X=1</td>
 </tr>
 <tr>
  <td rowspan="4">Sustain Rate</td>
  <td>Linear increment</td>
  <td>ADSR2.Y=000</td>
 </tr>
 <tr>
  <td>Linear</td>
  <td>ADSR2.Y=010</td>
 </tr>
 <tr>
  <td>Pseudo increment</td>
  <td>ADSR2.Y=100</td>
 </tr>
 <tr>
  <td>decrement</td>
  <td>ADSR2.Y=110</td>
 </tr>
 <tr>
  <td rowspan="2">Release Rate</td>
  <td>Linear decrement</td>
  <td>ADSR2.Z=0</td>
 </tr>
 <tr>
  <td>decrement</td>
  <td>ADSR2.Z=1</td>
 </tr>
</table>


The "pseudo exponential increment" is a variation in line, in which linear volume increment is lowered in
increment rate when 75% of the maximum value (0x6000) is exceeded.


- © SCEI

- 24-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


<table>
 <tr>
  <th rowspan="3">0.75<br/>SL</th>
  <th>SR(+exp)<br/>DR(-exp)</th>
 </tr>
 <tr>
  <td>SR(+lin) RR(-lin)</td>
 </tr>
 <tr>
  <td>AR(+exp) SR(-lin)<br/>AR(+lin) RR(-exp)<br/>SR(-exp)<br/>RR(-lin)<br/>RR(-exp)</td>
 </tr>
</table>


key-on


key-off


Figure 2-7 Parameters for Envelope and Their Curves


t


The value of the envelope varies successively, but the value is reflected in the ENVX register and can be referred
to. When sound is generated from loop-less waveform data, ENVX is set to 0 regardless of the envelope status,
at the moment the ENDX register bit corresponding to the voice is set to 1.


## 2.1.5. Volume


In each voice, the volume for the L channel and R channel can be set independently. By setting different values,
the panpot can be configured.
The volume settings are made in the VOLL and VOLR registers.


[VOLL]


Volume(L)
Control


Envelope
Control


Volume(R)
Control


[VOLR]


Figure 2-8 Volume Processing


- 25-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


The mode specification, by which the volume varies over time, can be set for the L channel and R channel
independently, as in the case of the Sustain Rate of the envelope.


<table>
 <tr>
  <th>Mode</th>
  <th>VOLL/R</th>
  <th>Volume Variation</th>
 </tr>
 <tr>
  <td>Direct</td>
  <td>Bit 15=0</td>
  <td>Constant</td>
 </tr>
 <tr>
  <td>+lin</td>
  <td>Bit 15:13=100</td>
  <td>Linear increment</td>
 </tr>
 <tr>
  <td>-lin</td>
  <td>Bit 15:13=101</td>
  <td>Linear decrement</td>
 </tr>
 <tr>
  <td></td>
  <td>Bit 15:13=110</td>
  <td>Pseudo increment</td>
 </tr>
 <tr>
  <td></td>
  <td>Bit 15:13=111</td>
  <td>decrement</td>
 </tr>
</table>


First specify the standard volume in direct mode and start sound generation, even when specifying a mode other
than Direct mode. If a mode other than Direct mode is specified again later with a variation rate, variation of
the volume starts instantly.
The phase can be reversed by setting a negative value in Direct mode.


1


0.75


+exp


+lin


Direct


- exp

- lin


0


Key on


VOLL/R
Register Setting


Figure 2-9 Volume Variation by Time


t


The volume varies successively in modes other than Direct mode, but the value is reflected in the VOLX register
and can be referred to.


## 2.1.6. Key-On/Key-Off


Key-on (starting sound generation) and key-off (stopping sound generation) can be controlled by the KON0/1
and KOF0/1 registers, respectively, for each voice.


At key-on, sound generation of the waveform data indicated by the SSAH/L register is started according to the
parameters of pitch, envelope, volume, etc.
At key-off, the envelope enters the Release phase and sound generation stops according to the Release Rate.


If there is no loop specification in the waveform data, sound generation of the voice ends when reaching the
endpoint block of waveform data even before key-off, and muting is applied by hardware.


- © SCEI

- 26-




<table>
 <tr>
  <th>SCE CONFIDENTIAL</th>
  <td></td>
  <td>SPU2 Overview Version 6.0</td>
 </tr>
 <tr>
  <th>2.1.7. Mixing Switch</th>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th>The output from each voice is mixed into<br/>through the control of the mixing switch.<br/>For the mixing switch, on/off of each voice's</th>
  <td>channels of Dry L,<br/>output to the</td>
  <td>L, Dry R and Wet R in each core<br/>channels is specified in the</td>
 </tr>
 <tr>
  <th>VMIXL0/1, VMIXR0/1, VMIXEL0/1 and<br/>off, it is equivalent to a voice to which muting<br/>The mixing results are successively stored in<br/>output from the core through switching by<br/>For the final output, on/off can be specified<br/>MMIX switches off is equivalent to applying</th>
  <td>registers.<br/>is applied.<br/>local memory<br/>MMIX register at<br/>Dry L, Wet L, Dry<br/>to all the voice</td>
  <td>When a voice's output to all the channels is<br/>data output area, and become the final<br/>same time.<br/>R and Wet R independently. Turning all the<br/>outputs.</td>
 </tr>
 <tr>
  <th>Channel</th>
  <td>Switch</td>
  <td>Switch</td>
 </tr>
 <tr>
  <th>L direct</th>
  <td>VMIXL0/1</td>
  <td>MMIX.MSNDL</td>
 </tr>
 <tr>
  <th>R direct</th>
  <td>VMIXR0/1</td>
  <td>MMIX.MSNDR</td>
 </tr>
 <tr>
  <th>Wet L effect</th>
  <td>VMIXEL0/1</td>
  <td>MMIX.MSNDEL</td>
 </tr>
 <tr>
  <th>Wet R effect</th>
  <td>VMIXER0/1</td>
  <td>MMIX.MSNDER</td>
 </tr>
</table>


Volume(L)
Control


<table>
 <tr>
  <th></th>
  <th></th>
  <th></th>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
 </tr>
</table>


Volume(R)
Control Mixing
Switches for
Each voice


[VMIXL]


[VMIXEL]
[VMIXR]
[VMIXER]


Mixing of
All Voices
to 4 Channels


To Sound Data
Output Area
in Local Memory


Figure 2-10 Voice Mixing


Dry L


Wet L


Dry R


Wet R


Switches for
Mixing Results


1. [MMIX]

- 27-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


## 2.2. Sound Data Input Processing


Successive 16-bit data (little endian) transfer to the local memory sound data input area by the host enables the
SPU2 to apply volume processing to the data as sound data, mix it with the output from voice processing, and
then apply digital effects to it. It is also possible to output the sound data to the output block directly by
bypassing the internal processing in the SPU2.


## 2.2.1. Sound Data Input Area


Each core is provided with one stereo unit as the sound data input. The addresses in the input area are as
follows. These areas are reserved areas, and other data cannot be placed there.


<table>
 <tr>
  <th>Address</th>
  <th>Area</th>
  <th></th>
 </tr>
 <tr>
  <td>2000-21FF</td>
  <td>CORE0 MEMIN(L)</td>
  <td>CORE0 L channel sound data input area</td>
 </tr>
 <tr>
  <td>2200-23FF</td>
  <td>CORE0 MEMIN(R)</td>
  <td>CORE0 R channel sound data input area</td>
 </tr>
 <tr>
  <td>2400-25FF</td>
  <td>CORE1 MEMIN(L)</td>
  <td>CORE1 L channel sound data input area</td>
 </tr>
 <tr>
  <td>2600-27FF</td>
  <td>CORE1 MEMIN(R)</td>
  <td>CORE1 R channel sound data input area</td>
 </tr>
</table>


The sound data input area is 512 short words (1024 bytes) in size and is composed of double buffers of 256
short words.
Data transfer to the sound data input area can be realized easily by using the Auto DMA write transfer. For
details, refer to the appropriate sound library document.


Host


Local Memory


<table>
 <tr>
  <th></th>
 </tr>
 <tr>
  <th>[MINL/R]<br/>[MINEL/ER]</th>
 </tr>
</table>


Volume
[BVOL]


Voice
Processing


Figure 2-11 Sound Data Input


## 2.2.2. Volume Processing


The volume can be set to the sound obtained from the sound data input with the BVOLL and BVOLR registers.
However, the mode of variation by time cannot be specified and the constant mode is always set.


- © SCEI

- 28-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


## 2.2.3. Mixing with Voice Output


The sound obtained from the sound data input can be mixed with the Dry L/Wet L/Dry R/Wet R channel of
the voice output. This is controlled by the flags of the MMIX register.


<table>
 <tr>
  <th>Sound Data</th>
  <th>Voice</th>
  <th>Switch</th>
 </tr>
 <tr>
  <td></td>
  <td>L</td>
  <td>MMIX.MINL</td>
 </tr>
 <tr>
  <td></td>
  <td>R</td>
  <td>MMIX.MINR</td>
 </tr>
 <tr>
  <td></td>
  <td>Wet L</td>
  <td>MMIX.MINEL</td>
 </tr>
 <tr>
  <td></td>
  <td>Wet R</td>
  <td>MMIX.MINER</td>
 </tr>
</table>


## 2.2.4. Bypass Processing (CORE0)


It is possible to specify a mode that connects directly to the S/PDIF digital output of the output block by
bypassing internal processing, for sound data input to CORE0. Since volume processing is not performed, the
encoded data, which is not ordinary 16-bit digital data, can be directly output from the host.
In this mode, however, CORE1 output, the final output from the core, is not connected to the S/PDIF digital
output. Moreover, this switching is performed only for the S/PDIF digital output and sound data input to
CORE0 cannot be directly connected to the D/A converter output. For details, refer to the appropriate sound
library document.


## 2.2.5. 32-bit Sound Data Input (CORE1)


It is possible to specify a mode that connects directly to the output block by bypassing the internal processing,
for sound data input to CORE1. In this mode, the sound data input is processed in 32-bit units (24 bits enabled
and lower 8 bits disabled). It can mix the 32-bit data (little endian) transferred from the host as higher quality
digital data with the D/A converter output or the S/PDIF digital output.


The sound data input area is processed in 32-bit units in this mode. Since the unit of data volume doubles, the
speed of data reading also doubles.
For details, refer to the appropriate sound library document.


generate


- 29-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


## 2.3. Sound Data Output Processing


The SPU2 can write the generated 16-bit sound data to the local memory sound data output area at any time.
Various processes can be applied to the sound data being generated by the SPU2 by reading the data successively
on the host.


The contents of the sound data to be output and the output area are fixed as shown in the table. This area is a
reserved area, and cannot be used for other purposes.


<table>
 <tr>
  <th>Core</th>
  <th>Channel</th>
  <th>Contents</th>
  <th>Area</th>
 </tr>
 <tr>
  <td rowspan="6">CORE0</td>
  <td>Voice1</td>
  <td>Crest value after multiplication of<br/>envelope in voice1</td>
  <td>000400 - 0005FF</td>
 </tr>
 <tr>
  <td>Voice3</td>
  <td>Crest value after multiplication of<br/>in voice3</td>
  <td>000600 - 0007FF</td>
 </tr>
 <tr>
  <td></td>
  <td>L after 24 voices</td>
  <td>001000 - 0011FF</td>
 </tr>
 <tr>
  <td></td>
  <td>R after 24 voices</td>
  <td>001200 - 0013FF</td>
 </tr>
 <tr>
  <td></td>
  <td>Wet L after 24 voices</td>
  <td>001400 - 0015FF</td>
 </tr>
 <tr>
  <td></td>
  <td>Wet R after 24 voices</td>
  <td>001600 - 0017FF</td>
 </tr>
 <tr>
  <td rowspan="8">CORE1</td>
  <td></td>
  <td>CORE0 L</td>
  <td>000800 - 0009FF</td>
 </tr>
 <tr>
  <td></td>
  <td>CORE0 R</td>
  <td>000A00 - 000BFF</td>
 </tr>
 <tr>
  <td>Voice1</td>
  <td>Crest value after multiplication of<br/>in voice1</td>
  <td>000C00 - 000DFF</td>
 </tr>
 <tr>
  <td>Voice3</td>
  <td>Crest value after multiplication of<br/>envelope in voice3</td>
  <td>000E00 - 000FFF</td>
 </tr>
 <tr>
  <td>MEMOUT(L)</td>
  <td>Dry L after mixing 24 voices</td>
  <td>001800 - 0019FF</td>
 </tr>
 <tr>
  <td>MEMOUT(R)</td>
  <td>Dry R after mixing 24 voices</td>
  <td>001A00 - 001BFF</td>
 </tr>
 <tr>
  <td>MEMOUT(EL)</td>
  <td>Wet L after mixing 24 voices</td>
  <td>001C00 - 001DFF</td>
 </tr>
 <tr>
  <td>MEMOUT(ER)</td>
  <td>Wet R after mixing 24 voices</td>
  <td>001E00 - 001FFF</td>
 </tr>
</table>


- © SCEI

- 30-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


Local Memory


Host 00 0000
00 0400


(Reserved)


CORE0 Voice1
CORE0 Voice3
CORE1 SIN(L)
CORE1 SIN(R)
CORE1 Voice1
CORE1 Voice3
CORE0 MEMOUT(L)
CORE0 MEMOUT(R)
CORE0 MEMOUT(EL)
CORE0 MEMOUT(ER)
CORE1 MEMOUT(L)
CORE1 MEMOUT(R)
CORE1 MEMOUT(EL)


Voice
Processing


Mixing


Voice
Processing


Output Block


Figure 2-12 Sound Data Output


The output area is 512 short words (1024 bytes) in size for each channel, and is composed of double buffers of
256 short words each.
Data can be read easily from the sound data output area by using the Auto DMA read transfer. For details, refer
to the appropriate sound library document.


- 31-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


## 2.4. Mixing


Mixing performs volume processing and switching to voice output, sound data input and external input, and
distributes the data to direct output and digital effects.
Each core has the following four sources:


<table>
 <tr>
  <th>Source</th>
  <th>Volume</th>
  <th>Switch</th>
  <th>Destination</th>
 </tr>
 <tr>
  <td>Voice Direct Output<br/>(Dry L/R)</td>
  <td>-</td>
  <td>MMIX.MSNDL/R</td>
  <td>Direct Output</td>
 </tr>
 <tr>
  <td>Voice Effect Output</td>
  <td>-</td>
  <td>MMIX.MSNDEL/ER</td>
  <td>Digital Effect</td>
 </tr>
 <tr>
  <td rowspan="2">Sound Data Input</td>
  <td rowspan="2">BVOLL/R</td>
  <td>MMIX.MINL/R</td>
  <td>Direct</td>
 </tr>
 <tr>
  <td>MMIX.MINEL/ER</td>
  <td>Effect</td>
 </tr>
 <tr>
  <td rowspan="2">External Input<br/>(Final Output from CORE0)</td>
  <td rowspan="2">AVOLL/R</td>
  <td>MMIX.SINL/R</td>
  <td>Direct</td>
 </tr>
 <tr>
  <td>MMIX.SINEL/ER</td>
  <td>Effect</td>
 </tr>
</table>


* External input is performed only to


CORE1.


[MSNDL/R]


<table>
 <tr>
  <th>Voice<br/>Processing</th>
  <th rowspan="3">Dry<br/>Wet L/R<br/>Volume</th>
 </tr>
 <tr>
  <td>Sound Data<br/>Input</td>
 </tr>
 <tr>
  <td></td>
 </tr>
</table>


[MSNDEL/ER]


To Direct
Output


External
Input
Volume
[AVOL]


Figure 2-13 Mixing


To Digital
Effect


For sound data input and external input, only the constant mode is set for volume variation with time.


- © SCEI

- 32-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


## 2.5. Digital Effect Processing


Various sound effects such as reverb, echo and delay can be added by performing digital effect processing on the
sounds generated by each core and input from the sound data input.
For other digital effect procedures, refer to the sound library document.


## 2.5.1. Signal Flow for Digital Effect Processing


The input to digital effect processing is the mixture of the following sound sources as described in "2.4. Mixing".


- Voice Effect Output (Wet L/R)

- Sound Data Input

- External Input *CORE1 only


However, various connection forms can be taken by passing through the host with switch on/off setting and
sound data output function.


CORE0


Voice
Processing


Sound
Data Input


Mixing


CORE1


<table>
 <tr>
  <th colspan="4">Voice</th>
 </tr>
 <tr>
  <th colspan="4">Processing</th>
 </tr>
 <tr>
  <th>Sound</th>
  <td>Mixing</td>
  <td></td>
  <td>Effect</td>
 </tr>
 <tr>
  <th>Data Input</th>
  <td></td>
  <td></td>
  <td>Volume</td>
 </tr>
 <tr>
  <th></th>
  <td></td>
  <td></td>
  <td>[EVOL]</td>
 </tr>
 <tr>
  <th>External</th>
  <td></td>
  <td>Digital</td>
  <td></td>
 </tr>
 <tr>
  <th>Input</th>
  <td></td>
  <td>Effect</td>
  <td></td>
 </tr>
</table>


Effect
Volume


[EVOL]


Digital
Effect


To
Output
Block


Local Memory


Figure 2-14 Signal Flow of Digital Effects


## 2.5.2. Work Area for Digital Effect Processing


When performing digital effect processing, it is necessary to reserve a work area in the local memory.
The start address of the work area is specified with the ESAH/L register and the end address is specified with
the EEAH register.
Decide the start address by totaling the size required by each delay block for digital effects.
The end address is specified in the upper 6 bits only, and it is assumed that the lower 16 bits are 0xFFFF. That
is, the end address in the work area is always on a 64K short-word (128 KB) boundary in the local memory.


- 33-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


CORE0
[ESAH / L]


CORE0
[EEAH]


Digital Effect
Work Area
for CORE0


0? FFFF


CORE1
[EEAH]


Digital Effect
Work Area
for CORE1


0? FFFF


CORE1
[ESAH / L]


Figure 2-15 Work Area for Digital Effects


## 2.5.3. Effect Volume


When the output from a digital effect is mixed with the direct output, volume control can be performed to the
output from the digital effect.
The effect volume is set with the EVOLL/R register. The panpot of the effect can be decided by setting the L
channel and R channel independently. Only the constant mode is set for the variation with time.


The volume set by the EVOLL/R register corresponds to the return volume of the effect. The following
volume settings of each source correspond to the send volume respectively.


<table>
 <tr>
  <th>Source</th>
  <th>Send Volume</th>
  <th>Remarks</th>
 </tr>
 <tr>
  <td>Voice</td>
  <td>VOLL/R</td>
  <td></td>
 </tr>
 <tr>
  <td>Sound Data</td>
  <td>BVOLL/R</td>
  <td></td>
 </tr>
 <tr>
  <td>External</td>
  <td>AVOLL/R</td>
  <td>CORE1</td>
 </tr>
</table>


- © SCEI

- 34-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


## 2.6. Master Volume


The final volume is determined by adding the master volume to the mixing result of the output from the digital
effect and direct output (Dry L/R). The master volume of the L channel and R channel can be set
independently by the MVOLL and MVOLR register. By setting the values, the whole panpot is decided.


Mixing


Effect
Volume


[EVOLL/R]


Master
Volume


Digital [MVOLL/R]
Effect


Figure 2-16 Master Volume Processing


To
Output


The mode of varying the master volume with time can be specified as follows, as well as the voice volume:


<table>
 <tr>
  <th>Mode</th>
  <th>MVOLL/R Register Setting</th>
  <th>Volume Variation</th>
 </tr>
 <tr>
  <td>Direct</td>
  <td>Bit 15=0</td>
  <td>Constant (no variation with time)</td>
 </tr>
 <tr>
  <td>+lin</td>
  <td>Bit 15:13=100</td>
  <td>Linear increment</td>
 </tr>
 <tr>
  <td>-lin</td>
  <td>Bit 15:13=101</td>
  <td>Linear decrement</td>
 </tr>
 <tr>
  <td>+exp</td>
  <td>Bit 15:13=110</td>
  <td>Pseudo exponential increment</td>
 </tr>
 <tr>
  <td>-exp</td>
  <td>Bit 15:13=111</td>
  <td>Exponential decrement</td>
 </tr>
</table>


The L channel and R channel can be specified independently for the volume mode as well.
When specifying a mode other than "Direct", first specify the standard volume value in Direct mode. Then, the
volume variation starts when other volume mode variation rates are re-specified. Moreover, the phase can be
reversed by specifying a negative value as a variation rate.
The current value of the master volume can be read from the MVOLXL/MVOLXR register.


- 35-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


## 2.7. Interrupt Processing


In most SPU2 processing, when each core accesses a specific address in the local memory, an interrupt can be
generated to the host. The address where an interrupt is generated is set by the IRQAH and IRQAL registers.
Local Memory
CORE 0 Interrupt to Host
[IRQAH / L]
Arbitrary
Processing


CORE 1
[IRQAH / L]


Figure 2-17 Interrupt Processing


No matter which core accesses this address, an interrupt is generated. Interrupts generated by both cores are
detected at the same time on the host. An interface provided by the sound library represents which core has
generated the interrupt.
In the following situations, there are limitations and warnings applied to the relationship between each core's
access and interrupts.


Initial state and waveform data without a loop


Voices without key-on after resetting the SPU2, and voices generated after decoding the LOOP/END block
of loop-less waveform data, access the local memory unnecessarily (free-run the entire local memory area) as
an internal operation of the SPU2. This may cause an unexpected interrupt.
Therefore, when applying an interrupt to loop-less waveform data, suppress unnecessary accesses from a
voice not in process, by adding a soundless block whose LOOP/START, LOOP, and LOOP/END bits are
set to 1 to the end of the data. (For waveform data format, refer to "1.3. Waveform Data Format".) Actions
for the voices to which key-on has not been applied after resetting are handled via the sound library.


## Position of waveform data which sets interrupt generation address


If an address between the starting and end addresses in waveform data is specified to the SSAH/L or
LSAXH/L register by mistake and the IRQA/H register is specified to the same address, an interrupt does
not occur.


## Local memory data transfer


When local memory data transfer is performed, the internal pointer stays at the following addresses at the
end of the transfer:
Transfer from host to local memory: TSAH/L + Data Size + 1
Transfer from local memory to host: TSAH/L + Data Size + 0 x 20
(Both are in short-word units)


- © SCEI

- 36-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


Therefore, if the IRQAH/L register has been set at these positions, an interrupt occurs after the transfer has
been ended.


## Digital effect processing


When digital effect processing is disabled, interrupts by digital effect processing are not generated even if the
interrupt generation address is specified to the digital effect work area.


- 37-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


(This page is left blank intentionally)


- © SCEI

- 38-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


## 3. Register List


- 39-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


## 3.1. Classification of Registers


The registers of the SPU2 are classified as follows:


## Voice basic parameter registers


These registers show the basic parameters of each voice. Each voice in each core has a set of registers.


Voice control parameter registers
These registers control on/off of each function in voice processing. Each core has a set of registers.


## Addressing registers


These registers perform address specification in the local memory. The registers, which show the upper 6
bits and the lower 16 bits in the address, are paired. Each core has a set of registers.


Digital effect addressing registers
These registers perform addressing related to digital effects. The registers, which show the upper 6 bits and
the lower 16 bits in the address, are paired. Each core has a set of registers.


## Volume registers


These registers specify the mixing volume of each voice. Each core has a set of registers.


- © SCEI

- 40-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


## 3.2. Registers in Pairs


The SPU2 registers include registers to be used in pairs.


Addresses in the local memory are specified by a pair of registers showing the upper 6 bits and the lower 16 bits
of one address. For example, the starting address of the waveform data for each voice is maintained in the
SSAH register (the upper 6 bits) and the SSAL register (the lower 16 bits). Therefore, SSAH and SSAL are
treated as a pair and written as the SSAH/L register.
The EEAH register is an exception to the addressing registers, and does not include a register which shows the
lower 16 bits.


Registers that specify the switching for each voice are composed of a pair of registers corresponding to Voice 0-
15 and Voice 16-23. As for the specification of the voice output to the Dry L channel, for example, Voice0-15
and Voice16-23 are specified by the VMIXL0 register and VMIXL1 register respectively. These two registers
are treated as a pair and written as the VMIXL0/1 register.


- 41-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


## VOLL / VOLR : Voice volume


Constant specification mode (direct mode)
15 12 8


<table>
 <tr>
  <th></th>
  <th colspan="4">0 VALUE</th>
 </tr>
 <tr>
  <td></td>
  <td>Name<br/>VALUE</td>
  <td>Pos.<br/>14:0</td>
  <td>Format<br/>int 1:0:14</td>
  <td>Contents<br/>Constant volume value<br/>The reverses for a value.</td>
 </tr>
 <tr>
  <td colspan="5">Linear increment mode (+lin mode)<br/>15 12 8 4 0</td>
 </tr>
 <tr>
  <td colspan="5">1 0 0 X (reserved) VALUE</td>
 </tr>
 <tr>
  <td></td>
  <td>Name</td>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <td rowspan="2"></td>
  <td>VALUE</td>
  <td>6:0</td>
  <td>int 0:7:0</td>
  <td>Addition constant Ts</td>
 </tr>
 <tr>
  <td>X</td>
  <td>12</td>
  <td>int 0:1:0</td>
  <td>Polarity specification<br/>0 Normal phase (specifiable when the current<br/>value is positive.)<br/>Linear increment to +1.0<br/>1 Reverse phase (specifiable when the current<br/>value is negative.)<br/>Linear decrement to -1.0</td>
 </tr>
 <tr>
  <td colspan="5">Linear decrement mode (-lin mode)<br/>15 12 8 4 0</td>
 </tr>
 <tr>
  <td></td>
  <td colspan="2">1 0 1 X</td>
  <td colspan="2">(reserved) 0 VALUE</td>
 </tr>
 <tr>
  <td></td>
  <td>Name</td>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <td rowspan="2"></td>
  <td>VALUE</td>
  <td>6:0</td>
  <td>int 0:7:0</td>
  <td>Subtraction constant Ts</td>
 </tr>
 <tr>
  <td>X</td>
  <td>12</td>
  <td>int 0:1:0</td>
  <td>Polarity specification<br/>0 Normal phase (specifiable when the current<br/>value is positive.)<br/>Linear decrement to 0<br/>1 Reverse phase (specifiable when the current<br/>value is negative.)<br/>Linear increment to 0</td>
 </tr>
</table>


4


0


- © SCEI

- 42-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


## Pseudo inverse-exponential increment mode (+exp mode)


15 12 8 4 0


<table>
 <tr>
  <th colspan="2">1 1 0 X</th>
  <td colspan="2">(reserved) 0 VALUE</td>
 </tr>
 <tr>
  <th>Name</th>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <th>VALUE</th>
  <td>6:0</td>
  <td>int 0:7:0</td>
  <td>Addition constant per Ts</td>
 </tr>
 <tr>
  <th>X</th>
  <td>12</td>
  <td>int 0:1:0</td>
  <td>Polarity specification<br/>0 Normal phase (specifiable when the current<br/>value is positive.)<br/>Increment to +1.0 in a line<br/>1 Reverse phase (specifiable when the current<br/>value is negative.)<br/>Decrement to -1.0 in a line</td>
 </tr>
</table>


## Exponential decrement mode (-exp mode)


15 12 8 4


(reserved)


VALUE


## Name Pos. Format Contents


VALUE 6:0
0:7:0 Multiplication constant per Ts


0


## Description


These registers specify the volume for each voice.
The values of the upper three bits specify the pattern of the volume variation with time. When specifying a
mode other than constant mode, the volume value varies with time, because the value corresponding to the
value of the VALUE field is added to, subtracted from or multiplied by the volume value per Ts.
For the relationship between the VALUE field value and actual volume duration, refer to "4.1. Rate
Parameter Table".


- 43-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


## PITCH : Pitch when sound is generated


15 12


<table>
 <tr>
  <th>0 0</th>
  <td></td>
  <td>VALUE</td>
 </tr>
 <tr>
  <th>Name Pos.</th>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <th>VALUE 13:0</th>
  <td>int 0:14:0</td>
  <td>Pitch value</td>
 </tr>
</table>


8


4


0


Description


This register specifies the pitch (degree of highness or lowness of sound) of each voice.
Assuming the pitch of the original sound (waveform data) to be f0, the relationship between the
the pitch specification, and the pitch f, from which sound is generated, is as follows:


VALUE, or


VALUE
f = f0


4096
When the sound source is a noise generator, there is no acoustic variation even though the pitch
specification is changed. The pitch of the noise can be specified for each core by using the sound library.


## Notes


Pitch specification affects the progressing speed of sound generation.
slower the sound generation proceeds.


The lower the pitch is specified, the


- © SCEI

- 44-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


## ADSR1 / ADSR2 : Envelope


## ADSR1


15 12


<table>
 <tr>
  <th></th>
  <th>AR DR SL Name X</th>
  <th>14:8 7:4 3:0 Pos.</th>
  <th>int 0:7:0 int 0:4:0 int 0:4:0 Format AR</th>
  <th>Attack Rate Decay Rate Sustain Level Contents DR SL</th>
 </tr>
 <tr>
  <td></td>
  <td>X</td>
  <td>15</td>
  <td>int 0:1:0</td>
  <td>Mode specification for Attack Rate<br/>0 Linear increment mode (+lin mode)<br/>1 Pseudo exponential increment mode<br/>(+exp mode)</td>
 </tr>
 <tr>
  <td rowspan="4"></td>
  <td>15</td>
  <td>12</td>
  <td></td>
  <td>8 4 0</td>
 </tr>
 <tr>
  <td>Y</td>
  <td></td>
  <td>SR</td>
  <td>Z RR</td>
 </tr>
 <tr>
  <td>Name</td>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <td>RR</td>
  <td>4:0</td>
  <td>int 0:5:0</td>
  <td>Release Rate</td>
 </tr>
 <tr>
  <td></td>
  <td>Z</td>
  <td>5</td>
  <td>int 0:1:0</td>
  <td>Mode specification for Release Rate<br/>0 Linear decrement mode (-lin mode)<br/>1 decrement mode</td>
 </tr>
 <tr>
  <td rowspan="2"></td>
  <td>SR</td>
  <td>12:6</td>
  <td>int 0:7:0</td>
  <td>Sustain Rate</td>
 </tr>
 <tr>
  <td>Y</td>
  <td>15:13</td>
  <td>int 0:3:0</td>
  <td>Mode specification for Sustain Rate<br/>000 Linear increment mode (+lin mode)<br/>010 Linear decrement mode (-lin mode)<br/>100 Pseudo exponential increment mode<br/>(+exp mode)<br/>110 Exponential decrement mode</td>
 </tr>
</table>


8


4


0


## Description


These registers specify each parameter for the envelope.
For the relationship between the Rate/Level field values and actual envelope
Parameter Table".


duration, refer to "4.1. Rate


- 45-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


## ENVX : Current value of envelope


15 12


<table>
 <tr>
  <th></th>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th></th>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th></th>
  <td></td>
  <td>VALUE</td>
 </tr>
 <tr>
  <th>Name Pos.</th>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <th>VALUE 15:0</th>
  <td>int 1:0:15</td>
  <td>Current value of</td>
 </tr>
</table>


8


4


0


Description


This register indicates the current value of the envelope.
When SR and RR for the envelope specify linear decrement, a negative value is set only for 1 Ts.
When sound is generated from loop-less waveform data, ENVX is set to 0 regardless of the envelope status,
at the moment the ENDX register bit corresponding to the voice is set to 1.


- © SCEI

- 46-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


## VOLXL / VOLXR : Current value of volume


15 12


<table>
 <tr>
  <th></th>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th></th>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th></th>
  <td></td>
  <td>VALUE</td>
 </tr>
 <tr>
  <th>Name Pos.</th>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <th>VALUE 15:0</th>
  <td>int 1:0:15</td>
  <td>Current value of voice volume</td>
 </tr>
</table>


8


4


0


Description


These registers indicate the current volume of each voice.
When VOL is in a mode other than constant specification mode, the value varies per Ts according to the
volume variation.


- 47-

- © SCEI




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


## PMON0 / PMON1 : Pitch modulation specification


## PMON0


15 12


<table>
 <tr>
  <th>V<br/>15</th>
  <th>V<br/>14</th>
  <th>V<br/>13</th>
  <th>V<br/>12</th>
  <th>V<br/>11</th>
  <th>V<br/>10</th>
  <th>V<br/>9</th>
  <th>V<br/>8</th>
  <th>V<br/>7</th>
  <th>V<br/>6</th>
  <th>V<br/>5</th>
  <th>V<br/>4</th>
  <th>V<br/>3</th>
  <th>V<br/>2</th>
  <th>V<br/>1</th>
  <th>-</th>
 </tr>
 <tr>
  <td colspan="2">Name</td>
  <td colspan="14">Pos. Format Contents</td>
 </tr>
 <tr>
  <td colspan="2">V1</td>
  <td colspan="14">1 int 0:1:0 Pitch modulation specification for Voice1<br/>0 Pitch modulation off<br/>1 Pitch modulation Voice0</td>
 </tr>
 <tr>
  <td colspan="16"></td>
 </tr>
 <tr>
  <td colspan="2">V15</td>
  <td colspan="14">15 int 0:1:0 Pitch modulation specification for Voice15<br/>0 Pitch modulation off<br/>1 Pitch modulation by Voice14</td>
 </tr>
 <tr>
  <td>15</td>
  <td></td>
  <td></td>
  <td>12</td>
  <td></td>
  <td></td>
  <td></td>
  <td>8</td>
  <td></td>
  <td></td>
  <td></td>
  <td>4</td>
  <td></td>
  <td></td>
  <td></td>
  <td>0</td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
  <td>(</td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td>V<br/>23</td>
  <td>V<br/>22</td>
  <td>V<br/>21</td>
  <td>V<br/>20</td>
  <td>V<br/>19</td>
  <td>V<br/>18</td>
  <td>V<br/>17</td>
  <td>V<br/>16</td>
 </tr>
 <tr>
  <td colspan="2">Name</td>
  <td colspan="4">Pos. Format</td>
  <td colspan="10">Contents</td>
 </tr>
 <tr>
  <td colspan="2">V16</td>
  <td>0</td>
  <td>int</td>
  <td>0:1:0</td>
  <td></td>
  <td colspan="10">Pitch modulation specification for Voice16<br/>0 Pitch modulation off<br/>1 Pitch modulation Voice15</td>
 </tr>
 <tr>
  <td colspan="16"></td>
 </tr>
 <tr>
  <td colspan="2">V23</td>
  <td>7</td>
  <td>int</td>
  <td>0:1:0</td>
  <td></td>
  <td colspan="10">Pitch modulation specification for Voice23<br/>0 Pitch modulation off<br/>1 Pitch modulation by Voice22</td>
 </tr>
</table>


8 4 0


## PMON1


## Description


These registers specify whether to apply the pitch modulation to each voice by using the crest value of the
voice of a number lower.
Voice0 is disabled.


- © SCEI

- 48-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


## NON0 / NON1 : Voice allocation to noise generator


## NON0


15 12 8 4 0
V
9


<table>
 <tr>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
 </tr>
 <tr>
  <td>15</td>
  <td>14</td>
  <td>13</td>
  <td>12</td>
  <td>11</td>
  <td>10</td>
  <td>8</td>
  <td>7</td>
  <td>6</td>
  <td>5</td>
  <td>4</td>
  <td>3</td>
  <td>2</td>
  <td>1</td>
  <td>0</td>
 </tr>
 <tr>
  <td colspan="2">Name</td>
  <td colspan="2">Pos.</td>
  <td colspan="2">Format</td>
  <td colspan="9">Contents</td>
 </tr>
 <tr>
  <td colspan="2">V0</td>
  <td colspan="2">0</td>
  <td colspan="2">int 0:1:0</td>
  <td colspan="9">Sound source specification for Voice0<br/>0 Waveform data<br/>1 Noise</td>
 </tr>
 <tr>
  <td colspan="15"></td>
 </tr>
 <tr>
  <td colspan="2">V15</td>
  <td colspan="2">15</td>
  <td colspan="2">int 0:1:0</td>
  <td colspan="9">Sound source specification for Voice15<br/>0 Waveform data<br/>1 Noise</td>
 </tr>
 <tr>
  <td>15</td>
  <td></td>
  <td></td>
  <td>12</td>
  <td></td>
  <td></td>
  <td>8</td>
  <td></td>
  <td></td>
  <td></td>
  <td>4</td>
  <td></td>
  <td></td>
  <td></td>
  <td>0</td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
  <td>(</td>
  <td></td>
  <td></td>
  <td></td>
  <td>V<br/>23</td>
  <td>V<br/>22</td>
  <td>V<br/>21</td>
  <td>V<br/>20</td>
  <td>V<br/>19</td>
  <td>V<br/>18</td>
  <td>V<br/>17</td>
  <td>V<br/>16</td>
 </tr>
 <tr>
  <td colspan="2">Name</td>
  <td colspan="2">Pos.</td>
  <td colspan="2">Format</td>
  <td colspan="9">Contents</td>
 </tr>
 <tr>
  <td colspan="2">V16</td>
  <td colspan="2">0</td>
  <td colspan="2">int 0:1:0</td>
  <td colspan="9">Sound source specification for Voice16<br/>0 Waveform data<br/>1 Noise</td>
 </tr>
 <tr>
  <td colspan="15"></td>
 </tr>
 <tr>
  <td colspan="2">V23</td>
  <td colspan="2">7</td>
  <td colspan="2">int 0:1:0</td>
  <td colspan="9">Sound source specification for Voice23<br/>0 Waveform data<br/>1 Noise</td>
 </tr>
</table>


## NON1


## Description


These registers specify whether to allocate each voice to the noise generator as a sound source.


- 49-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


## VMIX* : Mixing specification of voice output


## VMIXL0, VMIXEL0, VMIXR0, VMIXER0


15 12
V


<table>
 <tr>
  <th>V V</th>
  <th>V V</th>
  <th>V V</th>
  <th>V V V V V V V V V</th>
 </tr>
 <tr>
  <td>15 14</td>
  <td>13 12</td>
  <td>11 10 9</td>
  <td>8 7 6 5 4 3 2 1 0</td>
 </tr>
 <tr>
  <td>Name</td>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <td>V0</td>
  <td>0</td>
  <td>int 0:1:0</td>
  <td>Output switch for Voice0<br/>0 No output to applicable channel.<br/>1 to channel.</td>
 </tr>
 <tr>
  <td colspan="4"></td>
 </tr>
 <tr>
  <td>V15</td>
  <td>15</td>
  <td>int 0:1:0</td>
  <td>Output switch for Voice15<br/>0 No output to applicable channel.<br/>1 to channel.</td>
 </tr>
</table>


8 4 0


<table>
 <tr>
  <th></th>
  <td colspan="3">( reserved) 23 V 22 V 21 V 20 V V 19 18 V 17 V V 16</td>
 </tr>
 <tr>
  <th>Name</th>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <th>V16</th>
  <td>0</td>
  <td>int 0:1:0</td>
  <td>Output switch for Voice16<br/>0 No output to applicable channel.<br/>1 to channel.</td>
 </tr>
 <tr>
  <th colspan="4"></th>
 </tr>
 <tr>
  <th>V23</th>
  <td>7</td>
  <td>int 0:1:0</td>
  <td>Output switch for Voice23<br/>0 No output to applicable channel.<br/>1 to channel.</td>
 </tr>
</table>


## VMIXL1, VMIXEL1, VMIXR1, VMIXER1


15 12


8 4 0


## Description


These registers specify whether to output the output from each voice to each channel of Dry L/Wet L/Dry
R/Wet R. Each register corresponds to each channel as shown below.


<table>
 <tr>
  <th>Register</th>
  <th>Channel</th>
 </tr>
 <tr>
  <td>VMIXL0</td>
  <td>Dry L (Direct output (L))</td>
 </tr>
 <tr>
  <td>VMIXL1</td>
  <td>Dry L (Direct output (L))</td>
 </tr>
 <tr>
  <td>VMIXEL0</td>
  <td>Wet L (Effect output (L))</td>
 </tr>
 <tr>
  <td>VMIXEL1</td>
  <td>Wet L (Effect output (L))</td>
 </tr>
 <tr>
  <td>VMIXR0</td>
  <td>Dry R (Direct output (R))</td>
 </tr>
 <tr>
  <td>VMIXR1</td>
  <td>Dry R (Direct output (R))</td>
 </tr>
 <tr>
  <td>VMIXER0</td>
  <td>Wet R (Effect output (R))</td>
 </tr>
 <tr>
  <td>VMIXER1</td>
  <td>Wet R (Effect output (R))</td>
 </tr>
</table>


- © SCEI

- 50-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


## MMIX : Output specification after voice mixing


15 12


<table>
 <tr>
  <th colspan="4">M M M M M M M M S S S S<br/>S S S S I I I I I I I I<br/>N N N N N N N N N N N N<br/>(reserved) D D D D L R E E L R E E<br/>L R E E L R L R<br/>L R</th>
 </tr>
 <tr>
  <th>Name</th>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <th>SINER</th>
  <td>0</td>
  <td>int 0:1:0</td>
  <td>External ! Effect</td>
 </tr>
 <tr>
  <th>SINEL</th>
  <td>1</td>
  <td>int 0:1:0</td>
  <td>External ! Effect</td>
 </tr>
 <tr>
  <th>SINR</th>
  <td>2</td>
  <td>int 0:1:0</td>
  <td>External input (R) ! Direct output</td>
 </tr>
 <tr>
  <th>SINL</th>
  <td>3</td>
  <td>int 0:1:0</td>
  <td>External ! Direct</td>
 </tr>
 <tr>
  <th>MINER</th>
  <td>4</td>
  <td>int 0:1:0</td>
  <td>Sound data input (R) ! Effect output</td>
 </tr>
 <tr>
  <th>MINEL</th>
  <td>5</td>
  <td>int 0:1:0</td>
  <td>Sound data input (L) ! Effect output</td>
 </tr>
 <tr>
  <th>MINR</th>
  <td>6</td>
  <td>int 0:1:0</td>
  <td>Sound data ! Direct</td>
 </tr>
 <tr>
  <th>MINL</th>
  <td>7</td>
  <td>int 0:1:0</td>
  <td>Sound data input (L) ! Direct output</td>
 </tr>
 <tr>
  <th>MSNDER</th>
  <td>8</td>
  <td>int 0:1:0</td>
  <td>Voice Wet R ! Effect</td>
 </tr>
 <tr>
  <th>MSNDEL</th>
  <td>9</td>
  <td>int 0:1:0</td>
  <td>Voice Wet L ! Effect</td>
 </tr>
 <tr>
  <th>MSNDR</th>
  <td>10</td>
  <td>int 0:1:0</td>
  <td>Voice output Dry R ! Direct output</td>
 </tr>
 <tr>
  <th>MSNDL</th>
  <td>11</td>
  <td>int 0:1:0</td>
  <td>Voice L ! Direct</td>
 </tr>
</table>


8 4 0
M
M
S
N
N
D
D
L
E
L


## Description


This is a switching specification register, which divides the voice, sound data input and external input into
direct output and digital effect. When each bit is 0, the output is off. When 1, the output is on.
For SINL/R and SINEL/ER, specify 0 in CORE0 at all times.


- 51-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


## IRQAH / IRQAL : Interrupt address specification


## IRQAH


15 12


(reserved)


Name
Format


ADDRH
0:6:0


## IRQAL


8


4


ADDRH


0


Contents


Local memory address where an interrupt is
generated (upper
bits)


ADDRL


Name
Format


ADDRL
0:16:0


Contents


Local memory address where an interrupt is
generated (lower
bits)


Description


When each core accesses a specific address in the local memory, an interrupt can be generated for the host.
The above registers specify the address.
For details, refer to "2.7. Interrupt Processing".


- © SCEI

- 52-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


## KON0 / KON1 : Key-on specification


## KON0


15 12 8 4 0
V


<table>
 <tr>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
 </tr>
 <tr>
  <td>15</td>
  <td>14</td>
  <td>13</td>
  <td>12</td>
  <td>11</td>
  <td>10 9</td>
  <td>8</td>
  <td>7</td>
  <td>6</td>
  <td>5</td>
  <td>4</td>
  <td>3</td>
  <td>2</td>
  <td>1</td>
  <td>0</td>
 </tr>
 <tr>
  <td colspan="2">Name</td>
  <td colspan="2">Pos.</td>
  <td colspan="2">Format</td>
  <td colspan="9">Contents</td>
 </tr>
 <tr>
  <td colspan="2">V0</td>
  <td colspan="2">0</td>
  <td colspan="2">int 0:1:0</td>
  <td colspan="9">Key-on switch of Voice0</td>
 </tr>
 <tr>
  <td colspan="15">(Omitted)</td>
 </tr>
 <tr>
  <td colspan="2">V15</td>
  <td colspan="2">15</td>
  <td colspan="2">int 0:1:0</td>
  <td colspan="9">Key-on switch of Voice15</td>
 </tr>
 <tr>
  <td>15</td>
  <td></td>
  <td></td>
  <td>12</td>
  <td></td>
  <td></td>
  <td>8</td>
  <td></td>
  <td></td>
  <td></td>
  <td>4</td>
  <td></td>
  <td></td>
  <td></td>
  <td>0</td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td>V<br/>23</td>
  <td>V<br/>22</td>
  <td>V<br/>21</td>
  <td>V<br/>20</td>
  <td>V<br/>19</td>
  <td>V<br/>18</td>
  <td>V<br/>17</td>
  <td>V<br/>16</td>
 </tr>
 <tr>
  <td colspan="2">Name</td>
  <td colspan="2">Pos.</td>
  <td colspan="2">Format</td>
  <td colspan="9">Contents</td>
 </tr>
 <tr>
  <td colspan="2">V16</td>
  <td colspan="2">0</td>
  <td colspan="2">int 0:1:0</td>
  <td colspan="9">Key-on switch of Voice16</td>
 </tr>
 <tr>
  <td colspan="15">(Omitted)</td>
 </tr>
 <tr>
  <td colspan="2">V23</td>
  <td colspan="2">7</td>
  <td colspan="2">int 0:1:0</td>
  <td colspan="9">Key-on switch of Voice23</td>
 </tr>
</table>


## KON1


## Description


These registers specify key-on (start of sound generation) of each voice. When writing these registers, the
sound generation of the voice, which corresponds to the bit set to 1 among the written values, is started.


## Notes


The value read from this register does not reflect the voice that has actually been generated.
Do not write to any bits of the same register (KON0 or KON1) twice within 2 Ts. The period of time
between commands may not be sufficient for the commands to execute properly.
Do not specify key-on and key-off of the same voice within 2 Ts. If specified, the voice which actually starts
/ ends sound generation is indeterminate.
Key-on can be specified for the voice in the process of sound generation, by writing 1 to the bit again
without specifying key-off.


- 53-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


## KOF0 / KOF1 : Key-off specification


## KOF0


15 12 8 4 0
V


<table>
 <tr>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
  <th>V</th>
 </tr>
 <tr>
  <td>15</td>
  <td>14</td>
  <td>13</td>
  <td>12</td>
  <td>11</td>
  <td>10 9</td>
  <td>8</td>
  <td>7</td>
  <td>6</td>
  <td>5</td>
  <td>4</td>
  <td>3</td>
  <td>2</td>
  <td>1</td>
  <td>0</td>
 </tr>
 <tr>
  <td colspan="2">Name</td>
  <td colspan="2">Pos.</td>
  <td colspan="2">Format</td>
  <td colspan="9">Contents</td>
 </tr>
 <tr>
  <td colspan="2">V0</td>
  <td colspan="2">0</td>
  <td colspan="2">int 0:1:0</td>
  <td colspan="9">Key-off switch of Voice0</td>
 </tr>
 <tr>
  <td colspan="15">(Omitted)</td>
 </tr>
 <tr>
  <td colspan="2">V15</td>
  <td colspan="2">15</td>
  <td colspan="2">int 0:1:0</td>
  <td colspan="9">Key-off switch of Voice15</td>
 </tr>
 <tr>
  <td>15</td>
  <td></td>
  <td></td>
  <td>12</td>
  <td></td>
  <td></td>
  <td>8</td>
  <td></td>
  <td></td>
  <td></td>
  <td>4</td>
  <td></td>
  <td></td>
  <td></td>
  <td>0</td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td>V<br/>23</td>
  <td>V<br/>22</td>
  <td>V<br/>21</td>
  <td>V<br/>20</td>
  <td>V<br/>19</td>
  <td>V<br/>18</td>
  <td>V<br/>17</td>
  <td>V<br/>16</td>
 </tr>
 <tr>
  <td colspan="2">Name</td>
  <td colspan="2">Pos.</td>
  <td colspan="2">Format</td>
  <td colspan="9">Contents</td>
 </tr>
 <tr>
  <td colspan="2">V16</td>
  <td colspan="2">0</td>
  <td colspan="2">int 0:1:0</td>
  <td colspan="9">Key-off switch of Voice16</td>
 </tr>
 <tr>
  <td colspan="15">(Omitted)</td>
 </tr>
 <tr>
  <td colspan="2">V23</td>
  <td colspan="2">7</td>
  <td colspan="2">int 0:1:0</td>
  <td colspan="9">Key-off switch of Voice23</td>
 </tr>
</table>


## KOF1


## Description


These registers specify key-off (end of sound generation) of each voice. When writing these registers, the
envelope of the voice, which corresponds to the bit set to 1 among the written values, goes to the release
phase.


## Notes


Do not write to any bits of the same register (KOF0 or KOF1) twice within 2 Ts. The period of time
between commands may not be sufficient for the commands to execute properly.
Do not specify key-on and key-off of the same voice within 2 Ts. If specified, the voice which actually starts
/ ends sound generation is indeterminate.


- © SCEI

- 54-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


## TSAH / TSAL : Transfer start address


## TSAH


15 12


(reserved)


8


4


ADDRH


0


## TSAL


Name
Format


ADDRH
0:6:0


Contents


starting address
the transfer area
local memory (upper
bits)


ADDRL


Name
Format


ADDRL
0:16:0


Contents


starting address
the transfer area
local memory (lower
bits)


Description


These registers specify the starting address in the local memory area, which becomes the source/destination
in DMA read transfer, DMA write transfer, Auto DMA read transfer and single write transfer.
In the SSAL, which specifies the starting address of the waveform data (lower 16 bits) to the voice, the lower
3 bits must be specified to 0 when transferring the waveform data to the local memory. Therefore, in the
case of the TSAL, it is also necessary to transfer data to the address whose lower 3 bits are set to 0.


## Notes


The value is invariant regardless of the transfer execution status.
If the value is changed during data transfer, operation and transferred data become indeterminate.


- 55-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


## SSAH / SSAL : Starting address of waveform data


## SSAH


15 12 8 4
(reserved) ADDRH


0


## SSAL


Name
Format


ADDRH
0:6:0


Contents


Starting address
waveform data (upper 6
bits)


ADDRL


Name
Format


ADDRL
0:16:0


Contents


Starting address
waveform data (lower 16
bits;
specified
lower
bits)


Description


These registers specify the starting address of the waveform data, which becomes the sound source of each
voice.


- © SCEI

- 56-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


## LSAXH / LSAXL : Address of loop point


15 12


## LSAXH


(reserved)


8


4


ADDRH


0


Name
Format


ADDRH
0:6:0


Contents


Loop point address (upper
bits)


## LSAXL


ADDRL


## Name Format


ADDRL
0:16:0


## Contents


Loop point address (lower
bits; when
writing,
specified
lower 3


## Description


These registers show the starting address of the block specified as a loop point (the block whose
LOOP/START bit of the header is set to 1) in the waveform data. These are set when the loop point block
is passed through with the advance of sound generation.
The loop point can be set or changed by writing the address of an appropriate block header to this register
during sound generation (4 Ts after the key-on). (Rewriting within 4 Ts is disregarded.) In this case, the
loop point specification of the waveform data is disregarded temporarily until the next time the voice is
keyed on.


- 57-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


## NAXH / NAXL : Address of waveform data to be read next


## NAXH


## NAXL


15 12 8 4
ADDRH
reserved)


0


Name
Format


ADDRH
0:6:0


Contents


Address of
waveform
to
read next
(upper
bits)


ADDRL


Name
Format


ADDRL
0:16:0


Contents


Address of
waveform
to
read next
(lower


Description


These registers show the address of the waveform data to be read next. They are updated automatically with
the advance of sound generation.


- © SCEI

- 58-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


## ESAH / ESAL : Starting address in the work area for effect processing


## ESAH


15 12


(reserved)


8


4


ADDRH


0


## ESAL


Name
Format
ADDRH
0:6:0


Contents


Starting address
the work area
effect
(upper
bits)


ADDRL


Name
Format


ADDRL
0:16:0


Contents


Starting address
the work area
effect
(lower


Description


These registers specify the starting address in the work area to be used for digital effect processing.


- 59-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


## EEAH : End address in the work area for effect processing


## EEAH


15 12


<table>
 <tr>
  <th></th>
  <th>(reserved)</th>
  <th>ADDRH</th>
 </tr>
 <tr>
  <td>Name Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <td>ADDRH 5:0</td>
  <td>int 0:6:0</td>
  <td>End address in the work area for effect<br/>6</td>
 </tr>
</table>


8


4


0


## Description


This register specifies the end address in the work area to be used for digital effect processing. Unlike other
addressing registers, this register, which specifies the upper 6 bits only, is not paired with a register, which
specifies the lower 16 bits. Because of this, the end address in the work area can only be specified on the
64K short-word (128 KB) boundary.


- © SCEI

- 60-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


## ENDX0 / ENDX1 : Endpoint passing flag


15 12 8 4 0
V


<table>
 <tr>
  <th rowspan="5"></th>
  <th>V<br/>15</th>
  <th>V<br/>14</th>
  <th>V<br/>13</th>
  <th>V<br/>12</th>
  <th>V<br/>11</th>
  <th>V<br/>10 9</th>
  <th>V<br/>8</th>
  <th>V<br/>7</th>
  <th>V<br/>6</th>
  <th>V<br/>5</th>
  <th>V<br/>4</th>
  <th>V<br/>3</th>
  <th>V<br/>2</th>
  <th>V<br/>1</th>
  <th>V<br/>0</th>
 </tr>
 <tr>
  <td colspan="2">Name</td>
  <td colspan="2">Pos.</td>
  <td colspan="2">Format</td>
  <td colspan="9">Contents</td>
 </tr>
 <tr>
  <td colspan="2">V0</td>
  <td colspan="2">0</td>
  <td colspan="2">int 0:1:0</td>
  <td colspan="9">Endpoint passing flag for Voice0<br/>0 Has not been passed.<br/>1 Has been</td>
 </tr>
 <tr>
  <td colspan="15"></td>
 </tr>
 <tr>
  <td colspan="2">V15</td>
  <td colspan="2">15</td>
  <td colspan="2">int 0:1:0</td>
  <td colspan="9">Endpoint passing flag for Voice15<br/>0 Has not been passed.<br/>1 Has been</td>
 </tr>
 <tr>
  <td>ENDX1</td>
  <td>15</td>
  <td></td>
  <td></td>
  <td>12</td>
  <td></td>
  <td></td>
  <td>8</td>
  <td></td>
  <td></td>
  <td></td>
  <td>4</td>
  <td></td>
  <td></td>
  <td></td>
  <td>0</td>
 </tr>
 <tr>
  <td rowspan="5"></td>
  <td></td>
  <td></td>
  <td></td>
  <td>(</td>
  <td></td>
  <td></td>
  <td></td>
  <td>V<br/>23</td>
  <td>V<br/>22</td>
  <td>V<br/>21</td>
  <td>V<br/>20</td>
  <td>V<br/>19</td>
  <td>V<br/>18</td>
  <td>V<br/>17</td>
  <td>V<br/>16</td>
 </tr>
 <tr>
  <td colspan="2">Name</td>
  <td colspan="2">Pos.</td>
  <td colspan="2">Format</td>
  <td colspan="9">Contents</td>
 </tr>
 <tr>
  <td colspan="2">V16</td>
  <td colspan="2">0</td>
  <td colspan="2">int 0:1:0</td>
  <td colspan="9">Endpoint passing flag for Voice16<br/>0 Has not been passed.<br/>1 Has been</td>
 </tr>
 <tr>
  <td colspan="15"></td>
 </tr>
 <tr>
  <td colspan="2">V23</td>
  <td colspan="2">7</td>
  <td colspan="2">int 0:1:0</td>
  <td colspan="9">Endpoint passing flag for Voice23<br/>0 Has not been passed.<br/>1 Has been</td>
 </tr>
</table>


## ENDX0


## Description


These registers show whether or not the endpoint block has been reached with the advance of sound
generation of each voice.
The bit corresponding to the voice is set to 0 by specifying key-on.
All the bits are cleared to 0 by writing an arbitrary value (including a value other than 0) to these registers.


- 61-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


## MVOLL / MVOLR : Master volume


Constant specification mode (direct mode)
15 12 8 4 0


<table>
 <tr>
  <th></th>
  <th colspan="4">0 VALUE</th>
 </tr>
 <tr>
  <td></td>
  <td>Name<br/>VALUE</td>
  <td>Pos.<br/>14:0</td>
  <td>Format<br/>int 1:0:14</td>
  <td>Contents<br/>Volume value<br/>The reverses for a value.</td>
 </tr>
 <tr>
  <td colspan="5">Linear increment mode (+lin mode)<br/>15 12 8 4 0</td>
 </tr>
 <tr>
  <td></td>
  <td>1 0</td>
  <td>0 X</td>
  <td>(reserved)</td>
  <td>0 VALUE</td>
 </tr>
 <tr>
  <td></td>
  <td>Name</td>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <td rowspan="2"></td>
  <td>VALUE</td>
  <td>6:0</td>
  <td>int 0:7:0</td>
  <td>Addition constant Ts</td>
 </tr>
 <tr>
  <td>X</td>
  <td>12</td>
  <td>int 0:1:0</td>
  <td>Polarity specification<br/>0 Normal phase (specifiable when the<br/>current value is positive.)<br/>Linear increment to +1.0<br/>1 Reverse phase (specifiable when the<br/>current value is negative.)<br/>Linear decrement to -1.0</td>
 </tr>
 <tr>
  <td colspan="5">Linear decrement mode (-lin mode)<br/>15 12 8 4 0</td>
 </tr>
 <tr>
  <td></td>
  <td>1 0</td>
  <td>1 X</td>
  <td>(reserved)</td>
  <td>0 VALUE</td>
 </tr>
 <tr>
  <td></td>
  <td>Name</td>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <td rowspan="2"></td>
  <td>VALUE</td>
  <td>6:0</td>
  <td>int 0:7:0</td>
  <td>Subtraction constant Ts</td>
 </tr>
 <tr>
  <td>X</td>
  <td>12</td>
  <td>int 0:1:0</td>
  <td>Polarity specification<br/>0 Normal phase (specifiable when the<br/>current value is positive.)<br/>Linear decrement to 0<br/>1 Reverse phase (specifiable when the<br/>current value is negative.)<br/>Linear increment to 0</td>
 </tr>
</table>


- © SCEI

- 62-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


## Pseudo inverse-exponential increment mode (+exp mode)


15 12 8 4


<table>
 <tr>
  <th>1 1</th>
  <th>0 X</th>
  <th>(reserved)</th>
  <th>0 VALUE</th>
 </tr>
 <tr>
  <td>Name</td>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <td>VALUE</td>
  <td>6:0</td>
  <td>int 0:7:0</td>
  <td>Addition constant per Ts</td>
 </tr>
 <tr>
  <td>X</td>
  <td>12</td>
  <td>int 0:1:0</td>
  <td>Polarity specification<br/>0 Normal phase (specifiable when the<br/>current value is positive.)<br/>Increment to +1.0 in a line.<br/>1 Reverse phase (specifiable when the<br/>current value is negative.)<br/>Decrement to -1.0 in a line.</td>
 </tr>
</table>


0


## Exponential decrement mode (-exp mode)


15 12 8 4


(reserved)


VALUE


## Name Pos. Format Contents


VALUE 6:0
Multiplication constant


0


## Description


These registers specify the master volume for each core.
The values of the upper three bits specify the pattern of the volume variation with time. When specifying a
mode excluding the constant mode, the master volume value varies with time, because the value
corresponding to the value of the VALUE field is added to, subtracted from or multiplied by the master
volume value per Ts.
For the relationship between the VALUE field value and actual volume duration, refer to "4.1. Rate
Parameter Table".


- 63-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


## EVOLL / EVOLR : Return volume of effect


15 12


<table>
 <tr>
  <th></th>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th></th>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th></th>
  <td></td>
  <td>VALUE</td>
 </tr>
 <tr>
  <th>Name Pos.</th>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <th>VALUE 15:0</th>
  <td>int 1:0:15</td>
  <td>Return volume of effect<br/>The reverses for a value.</td>
 </tr>
</table>


8


4


0


Description


These registers specify the volume when mixing the output from digital effect processing with the Dry
L/Dry R channel.


- © SCEI

- 64-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


## AVOLL / AVOLR : Volume for external input


15 12


<table>
 <tr>
  <th></th>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th></th>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th></th>
  <td></td>
  <td>VALUE</td>
 </tr>
 <tr>
  <th>Name Pos.</th>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <th>VALUE 15:0</th>
  <td>int 1:0:15</td>
  <td>Volume for external input<br/>The reverses for a value.</td>
 </tr>
</table>


8


4


Description


These registers specify the volume for the external input.
They are disabled in CORE0.


0


- 65-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


## BVOLL / BVOLR : Volume for sound data input


15 12


<table>
 <tr>
  <th></th>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th></th>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th></th>
  <td></td>
  <td>VALUE</td>
 </tr>
 <tr>
  <th>Name Pos.</th>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <th>VALUE 15:0</th>
  <td>int 1:0:15</td>
  <td>Volume for sound data input<br/>The reverses for a value.</td>
 </tr>
</table>


8


4


Description


These registers specify the volume for sound data input.


0


- © SCEI

- 66-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


## MVOLXL / MVOLXR : Current value of master volume


15 12 8


<table>
 <tr>
  <th></th>
  <td></td>
  <td>VALUE</td>
 </tr>
 <tr>
  <th>Name Pos.</th>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <th>VALUE 15:0</th>
  <td>int 1:0:15</td>
  <td>Current value of master volume</td>
 </tr>
</table>


4


0


Description


These registers indicate the current value of the master volume.
When MVOL is in a mode other than constant specification mode, the value varies per Ts according to the
volume variation.


- 67-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


(This page is left blank intentionally)


- © SCEI

- 68-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


## 4. Appendix


- 69-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


## 4.1. Rate Parameter Table


The following tables show the volume variation rate for values of the envelope rate/level parameters (AR, DR,
SL, SR and RR) and the parameters to specify volume variation with time. The variation rate is shown with the
time required for the volume to vary from 0 to 1 (1 to 0, or 1 to 0.1). For SL, the variation rate is shown with
level.


- © SCEI

- 70-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


## 4.1.1. +Lin Mode


The variation is shown in linear increment for normal phase and in linear decrement for reverse phase.


<table>
 <tr>
  <th>Set Value</th>
  <th>0 -&gt; 1 Time</th>
 </tr>
 <tr>
  <td>010 0111</td>
  <td>46</td>
 </tr>
 <tr>
  <td>010 1000</td>
  <td>53</td>
 </tr>
 <tr>
  <td>010 1001</td>
  <td>62</td>
 </tr>
 <tr>
  <td>010 1010</td>
  <td>74</td>
 </tr>
 <tr>
  <td>010 1011</td>
  <td>93</td>
 </tr>
 <tr>
  <td>010 1100</td>
  <td>0.11</td>
 </tr>
 <tr>
  <td>010 1101</td>
  <td>0.12</td>
 </tr>
 <tr>
  <td>010 1110</td>
  <td>0.15</td>
 </tr>
 <tr>
  <td>010 1111</td>
  <td>0.19</td>
 </tr>
 <tr>
  <td>011 0000</td>
  <td>0.21</td>
 </tr>
 <tr>
  <td>011 0001</td>
  <td>0.25</td>
 </tr>
 <tr>
  <td>011 0010</td>
  <td>0.30</td>
 </tr>
 <tr>
  <td>011 0011</td>
  <td>0.37</td>
 </tr>
 <tr>
  <td>011 0100</td>
  <td>0.42</td>
 </tr>
 <tr>
  <td>011 0101</td>
  <td>0.50</td>
 </tr>
 <tr>
  <td>011 0110</td>
  <td>0.59</td>
 </tr>
 <tr>
  <td>011 0111</td>
  <td>0.74</td>
 </tr>
 <tr>
  <td>011 1000</td>
  <td>0.85</td>
 </tr>
 <tr>
  <td>011 1001</td>
  <td>0.99</td>
 </tr>
 <tr>
  <td>011 1010</td>
  <td>1.2</td>
 </tr>
 <tr>
  <td>011 1011</td>
  <td>1.5</td>
 </tr>
 <tr>
  <td>011 1100</td>
  <td>1.7</td>
 </tr>
 <tr>
  <td>011 1101</td>
  <td>2.0</td>
 </tr>
 <tr>
  <td>011 1110</td>
  <td>2.4</td>
 </tr>
 <tr>
  <td>011 1111</td>
  <td>3.0</td>
 </tr>
 <tr>
  <td>100 0000</td>
  <td>3.4</td>
 </tr>
 <tr>
  <td>100 0001</td>
  <td>4.0</td>
 </tr>
 <tr>
  <td>100 0010</td>
  <td>4.8</td>
 </tr>
 <tr>
  <td>100 0011</td>
  <td>5.9</td>
 </tr>
 <tr>
  <td>100 0100</td>
  <td>6.8</td>
 </tr>
 <tr>
  <td>100 0101</td>
  <td>7.9</td>
 </tr>
 <tr>
  <td>100 0110</td>
  <td>9.5</td>
 </tr>
 <tr>
  <td>100 0111</td>
  <td>12</td>
 </tr>
 <tr>
  <td>100 1000</td>
  <td>14</td>
 </tr>
 <tr>
  <td>100 1001</td>
  <td>16</td>
 </tr>
 <tr>
  <td>100 1010</td>
  <td>19</td>
 </tr>
 <tr>
  <td>100 1011</td>
  <td>24</td>
 </tr>
 <tr>
  <td>100 1100</td>
  <td>27</td>
 </tr>
 <tr>
  <td>100 1101</td>
  <td>32</td>
 </tr>
</table>


<table>
 <tr>
  <th>Set Value</th>
  <th>0 -&gt; 1 Time</th>
 </tr>
 <tr>
  <td>100 1110</td>
  <td>38</td>
 </tr>
 <tr>
  <td>100 1111</td>
  <td>48</td>
 </tr>
 <tr>
  <td>101 0000</td>
  <td>54</td>
 </tr>
 <tr>
  <td>101 0001</td>
  <td>63</td>
 </tr>
 <tr>
  <td>101 0010</td>
  <td>76</td>
 </tr>
 <tr>
  <td>101 0011</td>
  <td>95</td>
 </tr>
 <tr>
  <td>101 0100</td>
  <td>109</td>
 </tr>
 <tr>
  <td>101 0101</td>
  <td>127</td>
 </tr>
 <tr>
  <td>101 0110</td>
  <td>152</td>
 </tr>
 <tr>
  <td>101 0111</td>
  <td>190</td>
 </tr>
 <tr>
  <td>101 1000</td>
  <td>218</td>
 </tr>
 <tr>
  <td>101 1001</td>
  <td>254</td>
 </tr>
 <tr>
  <td>101 1010</td>
  <td>304</td>
 </tr>
 <tr>
  <td>101 1011</td>
  <td>380</td>
 </tr>
 <tr>
  <td>101 1100</td>
  <td>436</td>
 </tr>
 <tr>
  <td>101 1101</td>
  <td>508</td>
 </tr>
 <tr>
  <td>101 1110</td>
  <td>608</td>
 </tr>
 <tr>
  <td>101 1111</td>
  <td>760</td>
 </tr>
 <tr>
  <td>110 0000</td>
  <td>872</td>
 </tr>
 <tr>
  <td>110 0001</td>
  <td>1016</td>
 </tr>
 <tr>
  <td>110 0010</td>
  <td>1216</td>
 </tr>
 <tr>
  <td>110 0011</td>
  <td>1520</td>
 </tr>
 <tr>
  <td>110 0100</td>
  <td>1744</td>
 </tr>
 <tr>
  <td>110 0101</td>
  <td>2032</td>
 </tr>
 <tr>
  <td>110 0110</td>
  <td>2432</td>
 </tr>
 <tr>
  <td>110 0111</td>
  <td>3040</td>
 </tr>
 <tr>
  <td>110 1000</td>
  <td>3488</td>
 </tr>
 <tr>
  <td>110 1001</td>
  <td>4064</td>
 </tr>
 <tr>
  <td>110 1010</td>
  <td>4864</td>
 </tr>
 <tr>
  <td>110 1011</td>
  <td>6080</td>
 </tr>
 <tr>
  <td>110 1100<br/>:<br/>111 1110</td>
  <td>(reserved)</td>
 </tr>
 <tr>
  <td>111 1111</td>
  <td>infinity</td>
 </tr>
</table>


<table>
 <tr>
  <th>Set Value</th>
  <th>0 -&gt; 1 Time</th>
 </tr>
 <tr>
  <td>000 0000</td>
  <td>0.05</td>
 </tr>
 <tr>
  <td>000 0001</td>
  <td>0.06</td>
 </tr>
 <tr>
  <td>000 0010</td>
  <td>0.07</td>
 </tr>
 <tr>
  <td>000 0011</td>
  <td>0.09</td>
 </tr>
 <tr>
  <td>000 0100</td>
  <td>0.10</td>
 </tr>
 <tr>
  <td>000 0101</td>
  <td>0.12</td>
 </tr>
 <tr>
  <td>000 0110</td>
  <td>0.15</td>
 </tr>
 <tr>
  <td>000 0111</td>
  <td>0.18</td>
 </tr>
 <tr>
  <td>000 1000</td>
  <td>0.21</td>
 </tr>
 <tr>
  <td>000 1001</td>
  <td>0.24</td>
 </tr>
 <tr>
  <td>000 1010</td>
  <td>0.29</td>
 </tr>
 <tr>
  <td>000 1011</td>
  <td>0.36</td>
 </tr>
 <tr>
  <td>000 1100</td>
  <td>0.41</td>
 </tr>
 <tr>
  <td>000 1101</td>
  <td>0.48</td>
 </tr>
 <tr>
  <td>000 1110</td>
  <td>0.58</td>
 </tr>
 <tr>
  <td>000 1111</td>
  <td>0.73</td>
 </tr>
 <tr>
  <td>001 0000</td>
  <td>0.83</td>
 </tr>
 <tr>
  <td>001 0001</td>
  <td>0.97</td>
 </tr>
 <tr>
  <td>001 0010</td>
  <td>1.2</td>
 </tr>
 <tr>
  <td>001 0011</td>
  <td>1.5</td>
 </tr>
 <tr>
  <td>001 0100</td>
  <td>1.7</td>
 </tr>
 <tr>
  <td>001 0101</td>
  <td>1.9</td>
 </tr>
 <tr>
  <td>001 0110</td>
  <td>2.3</td>
 </tr>
 <tr>
  <td>001 0111</td>
  <td>2.9</td>
 </tr>
 <tr>
  <td>001 1000</td>
  <td>3.3</td>
 </tr>
 <tr>
  <td>001 1001</td>
  <td>3.9</td>
 </tr>
 <tr>
  <td>001 1010</td>
  <td>4.6</td>
 </tr>
 <tr>
  <td>001 1011</td>
  <td>5.8</td>
 </tr>
 <tr>
  <td>001 1100</td>
  <td>6.6</td>
 </tr>
 <tr>
  <td>001 1101</td>
  <td>7.7</td>
 </tr>
 <tr>
  <td>001 1110</td>
  <td>9.3</td>
 </tr>
 <tr>
  <td>001 1111</td>
  <td>12</td>
 </tr>
 <tr>
  <td>010 0000</td>
  <td>13</td>
 </tr>
 <tr>
  <td>010 0001</td>
  <td>15</td>
 </tr>
 <tr>
  <td>010 0010</td>
  <td>19</td>
 </tr>
 <tr>
  <td>010 0011</td>
  <td>23</td>
 </tr>
 <tr>
  <td>010 0100</td>
  <td>27</td>
 </tr>
 <tr>
  <td>010 0101</td>
  <td>31</td>
 </tr>
 <tr>
  <td>010 0110</td>
  <td>37</td>
 </tr>
</table>


- 71-

- © SCEI




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


## 4.1.2. -Lin Mode


The variation is shown in linear decrement for normal phase and in linear increment for reverse phase.


<table>
 <tr>
  <th>-Lin Value</th>
  <th>1 -&gt; 0 Time</th>
 </tr>
 <tr>
  <td>100 1110</td>
  <td>32</td>
 </tr>
 <tr>
  <td>100 1111</td>
  <td>38</td>
 </tr>
 <tr>
  <td>101 0000</td>
  <td>48</td>
 </tr>
 <tr>
  <td>101 0001</td>
  <td>54</td>
 </tr>
 <tr>
  <td>101 0010</td>
  <td>63</td>
 </tr>
 <tr>
  <td>101 0011</td>
  <td>76</td>
 </tr>
 <tr>
  <td>101 0100</td>
  <td>95</td>
 </tr>
 <tr>
  <td>101 0101</td>
  <td>109</td>
 </tr>
 <tr>
  <td>101 0110</td>
  <td>127</td>
 </tr>
 <tr>
  <td>101 0111</td>
  <td>152</td>
 </tr>
 <tr>
  <td>101 1000</td>
  <td>190</td>
 </tr>
 <tr>
  <td>101 1001</td>
  <td>218</td>
 </tr>
 <tr>
  <td>101 1010</td>
  <td>254</td>
 </tr>
 <tr>
  <td>101 1011</td>
  <td>304</td>
 </tr>
 <tr>
  <td>101 1100</td>
  <td>380</td>
 </tr>
 <tr>
  <td>101 1101</td>
  <td>436</td>
 </tr>
 <tr>
  <td>101 1110</td>
  <td>508</td>
 </tr>
 <tr>
  <td>101 1111</td>
  <td>608</td>
 </tr>
 <tr>
  <td>110 0000</td>
  <td>760</td>
 </tr>
 <tr>
  <td>110 0001</td>
  <td>872</td>
 </tr>
 <tr>
  <td>110 0010</td>
  <td>1016</td>
 </tr>
 <tr>
  <td>110 0011</td>
  <td>1216</td>
 </tr>
 <tr>
  <td>110 0100</td>
  <td>1520</td>
 </tr>
 <tr>
  <td>110 0101</td>
  <td>1744</td>
 </tr>
 <tr>
  <td>110 0110</td>
  <td>2032</td>
 </tr>
 <tr>
  <td>110 0111</td>
  <td>2432</td>
 </tr>
 <tr>
  <td>110 1000</td>
  <td>3040</td>
 </tr>
 <tr>
  <td>110 1001</td>
  <td>3488</td>
 </tr>
 <tr>
  <td>110 1010</td>
  <td>4064</td>
 </tr>
 <tr>
  <td>110 1011</td>
  <td>4864</td>
 </tr>
 <tr>
  <td>110 1100<br/>:<br/>111 1110</td>
  <td>(reserved)</td>
 </tr>
 <tr>
  <td>111 1111</td>
  <td>infinity</td>
 </tr>
</table>


<table>
 <tr>
  <th>-Lin Value</th>
  <th>1 -&gt; 0 Time</th>
 </tr>
 <tr>
  <td>010 0111</td>
  <td>37</td>
 </tr>
 <tr>
  <td>010 1000</td>
  <td>46</td>
 </tr>
 <tr>
  <td>010 1001</td>
  <td>53</td>
 </tr>
 <tr>
  <td>010 1010</td>
  <td>62</td>
 </tr>
 <tr>
  <td>010 1011</td>
  <td>74</td>
 </tr>
 <tr>
  <td>010 1100</td>
  <td>93</td>
 </tr>
 <tr>
  <td>010 1101</td>
  <td>0.11</td>
 </tr>
 <tr>
  <td>010 1110</td>
  <td>0.12</td>
 </tr>
 <tr>
  <td>010 1111</td>
  <td>0.15</td>
 </tr>
 <tr>
  <td>011 0000</td>
  <td>0.19</td>
 </tr>
 <tr>
  <td>011 0001</td>
  <td>0.21</td>
 </tr>
 <tr>
  <td>011 0010</td>
  <td>0.25</td>
 </tr>
 <tr>
  <td>011 0011</td>
  <td>0.30</td>
 </tr>
 <tr>
  <td>011 0100</td>
  <td>0.37</td>
 </tr>
 <tr>
  <td>011 0101</td>
  <td>0.42</td>
 </tr>
 <tr>
  <td>011 0110</td>
  <td>0.50</td>
 </tr>
 <tr>
  <td>011 0111</td>
  <td>0.59</td>
 </tr>
 <tr>
  <td>011 1000</td>
  <td>0.74</td>
 </tr>
 <tr>
  <td>011 1001</td>
  <td>0.85</td>
 </tr>
 <tr>
  <td>011 1010</td>
  <td>0.99</td>
 </tr>
 <tr>
  <td>011 1011</td>
  <td>1.2</td>
 </tr>
 <tr>
  <td>011 1100</td>
  <td>1.5</td>
 </tr>
 <tr>
  <td>011 1101</td>
  <td>1.7</td>
 </tr>
 <tr>
  <td>011 1110</td>
  <td>2.0</td>
 </tr>
 <tr>
  <td>011 1111</td>
  <td>2.4</td>
 </tr>
 <tr>
  <td>100 0000</td>
  <td>3.0</td>
 </tr>
 <tr>
  <td>100 0001</td>
  <td>3.4</td>
 </tr>
 <tr>
  <td>100 0010</td>
  <td>4.0</td>
 </tr>
 <tr>
  <td>100 0011</td>
  <td>4.8</td>
 </tr>
 <tr>
  <td>100 0100</td>
  <td>5.9</td>
 </tr>
 <tr>
  <td>100 0101</td>
  <td>6.8</td>
 </tr>
 <tr>
  <td>100 0110</td>
  <td>7.9</td>
 </tr>
 <tr>
  <td>100 0111</td>
  <td>9.5</td>
 </tr>
 <tr>
  <td>100 1000</td>
  <td>12</td>
 </tr>
 <tr>
  <td>100 1001</td>
  <td>14</td>
 </tr>
 <tr>
  <td>100 1010</td>
  <td>16</td>
 </tr>
 <tr>
  <td>100 1011</td>
  <td>19</td>
 </tr>
 <tr>
  <td>100 1100</td>
  <td>24</td>
 </tr>
 <tr>
  <td>100 1101</td>
  <td>27</td>
 </tr>
</table>


<table>
 <tr>
  <th>-Lin Value</th>
  <th>1 -&gt; 0 Time</th>
 </tr>
 <tr>
  <td>000 0000</td>
  <td>0.04</td>
 </tr>
 <tr>
  <td>000 0001</td>
  <td>0.05</td>
 </tr>
 <tr>
  <td>000 0010</td>
  <td>0.06</td>
 </tr>
 <tr>
  <td>000 0011</td>
  <td>0.07</td>
 </tr>
 <tr>
  <td>000 0100</td>
  <td>0.09</td>
 </tr>
 <tr>
  <td>000 0101</td>
  <td>0.10</td>
 </tr>
 <tr>
  <td>000 0110</td>
  <td>0.12</td>
 </tr>
 <tr>
  <td>000 0111</td>
  <td>0.15</td>
 </tr>
 <tr>
  <td>000 1000</td>
  <td>0.18</td>
 </tr>
 <tr>
  <td>000 1001</td>
  <td>0.21</td>
 </tr>
 <tr>
  <td>000 1010</td>
  <td>0.24</td>
 </tr>
 <tr>
  <td>000 1011</td>
  <td>0.29</td>
 </tr>
 <tr>
  <td>000 1100</td>
  <td>0.36</td>
 </tr>
 <tr>
  <td>000 1101</td>
  <td>0.41</td>
 </tr>
 <tr>
  <td>000 1110</td>
  <td>0.48</td>
 </tr>
 <tr>
  <td>000 1111</td>
  <td>0.58</td>
 </tr>
 <tr>
  <td>001 0000</td>
  <td>0.73</td>
 </tr>
 <tr>
  <td>001 0001</td>
  <td>0.83</td>
 </tr>
 <tr>
  <td>001 0010</td>
  <td>0.97</td>
 </tr>
 <tr>
  <td>001 0011</td>
  <td>1.2</td>
 </tr>
 <tr>
  <td>001 0100</td>
  <td>1.5</td>
 </tr>
 <tr>
  <td>001 0101</td>
  <td>1.7</td>
 </tr>
 <tr>
  <td>001 0110</td>
  <td>1.9</td>
 </tr>
 <tr>
  <td>001 0111</td>
  <td>2.3</td>
 </tr>
 <tr>
  <td>001 1000</td>
  <td>2.9</td>
 </tr>
 <tr>
  <td>001 1001</td>
  <td>3.3</td>
 </tr>
 <tr>
  <td>001 1010</td>
  <td>3.9</td>
 </tr>
 <tr>
  <td>001 1011</td>
  <td>4.6</td>
 </tr>
 <tr>
  <td>001 1100</td>
  <td>5.8</td>
 </tr>
 <tr>
  <td>001 1101</td>
  <td>6.6</td>
 </tr>
 <tr>
  <td>001 1110</td>
  <td>7.7</td>
 </tr>
 <tr>
  <td>001 1111</td>
  <td>9.3</td>
 </tr>
 <tr>
  <td>010 0000</td>
  <td>12</td>
 </tr>
 <tr>
  <td>010 0001</td>
  <td>13</td>
 </tr>
 <tr>
  <td>010 0010</td>
  <td>15</td>
 </tr>
 <tr>
  <td>010 0011</td>
  <td>19</td>
 </tr>
 <tr>
  <td>010 0100</td>
  <td>23</td>
 </tr>
 <tr>
  <td>010 0101</td>
  <td>27</td>
 </tr>
 <tr>
  <td>010 0110</td>
  <td>31</td>
 </tr>
</table>


- © SCEI

- 72-




SCE CONFIDENTIAL


## 4.1.3. +Exp Mode (Normal phase)


Pseudo Inverse-Exponential Increment Mode


<table>
 <tr>
  <th>Set</th>
  <th>1 -&gt; 0 Time</th>
 </tr>
 <tr>
  <td>Value</td>
  <td></td>
 </tr>
 <tr>
  <td>010 0100</td>
  <td>46 (msec)</td>
 </tr>
 <tr>
  <td>010 0101</td>
  <td>54</td>
 </tr>
 <tr>
  <td>010 0110</td>
  <td>65</td>
 </tr>
 <tr>
  <td>010 0111</td>
  <td>81</td>
 </tr>
 <tr>
  <td>010 1000</td>
  <td>93</td>
 </tr>
 <tr>
  <td>010 1001</td>
  <td>0.11 (sec)</td>
 </tr>
 <tr>
  <td>010 1010</td>
  <td>0.13</td>
 </tr>
 <tr>
  <td>010 1011</td>
  <td>0.16</td>
 </tr>
 <tr>
  <td>010 1100</td>
  <td>0.19</td>
 </tr>
 <tr>
  <td>010 1101</td>
  <td>0.22</td>
 </tr>
 <tr>
  <td>010 1110</td>
  <td>0.26</td>
 </tr>
 <tr>
  <td>010 1111</td>
  <td>0.33</td>
 </tr>
 <tr>
  <td>011 0000</td>
  <td>0.37</td>
 </tr>
 <tr>
  <td>011 0001</td>
  <td>0.43</td>
 </tr>
 <tr>
  <td>011 0010</td>
  <td>0.52</td>
 </tr>
 <tr>
  <td>011 0011</td>
  <td>0.65</td>
 </tr>
 <tr>
  <td>011 0100</td>
  <td>0.74</td>
 </tr>
 <tr>
  <td>011 0101</td>
  <td>0.87</td>
 </tr>
 <tr>
  <td>011 0110</td>
  <td>1.0</td>
 </tr>
 <tr>
  <td>011 0111</td>
  <td>1.3</td>
 </tr>
 <tr>
  <td>011 1000</td>
  <td>1.5</td>
 </tr>
 <tr>
  <td>011 1001</td>
  <td>1.7</td>
 </tr>
 <tr>
  <td>011 1010</td>
  <td>2.1</td>
 </tr>
 <tr>
  <td>011 1011</td>
  <td>2.6</td>
 </tr>
 <tr>
  <td>011 1100</td>
  <td>3.0</td>
 </tr>
 <tr>
  <td>011 1101</td>
  <td>3.5</td>
 </tr>
 <tr>
  <td>011 1110</td>
  <td>4.2</td>
 </tr>
 <tr>
  <td>011 1111</td>
  <td>5.2</td>
 </tr>
 <tr>
  <td>100 0000</td>
  <td>5.9</td>
 </tr>
 <tr>
  <td>100 0001</td>
  <td>6.9</td>
 </tr>
 <tr>
  <td>100 0010</td>
  <td>8.3</td>
 </tr>
 <tr>
  <td>100 0011</td>
  <td>10</td>
 </tr>
 <tr>
  <td>100 0100</td>
  <td>12</td>
 </tr>
 <tr>
  <td>100 0101</td>
  <td>14</td>
 </tr>
 <tr>
  <td>100 0110</td>
  <td>17</td>
 </tr>
 <tr>
  <td>100 0111</td>
  <td>21</td>
 </tr>
</table>


<table>
 <tr>
  <th>Set</th>
  <th>1 -&gt; 0 Time</th>
 </tr>
 <tr>
  <td>Value</td>
  <td></td>
 </tr>
 <tr>
  <td>000 0000</td>
  <td>0.09 (msec)</td>
 </tr>
 <tr>
  <td>000 0001</td>
  <td>0.11</td>
 </tr>
 <tr>
  <td>000 0010</td>
  <td>0.13</td>
 </tr>
 <tr>
  <td>000 0011</td>
  <td>0.16</td>
 </tr>
 <tr>
  <td>000 0100</td>
  <td>0.18</td>
 </tr>
 <tr>
  <td>000 0101</td>
  <td>0.21</td>
 </tr>
 <tr>
  <td>000 0110</td>
  <td>0.25</td>
 </tr>
 <tr>
  <td>000 0111</td>
  <td>0.32</td>
 </tr>
 <tr>
  <td>000 1000</td>
  <td>0.36</td>
 </tr>
 <tr>
  <td>000 1001</td>
  <td>0.42</td>
 </tr>
 <tr>
  <td>000 1010</td>
  <td>0.51</td>
 </tr>
 <tr>
  <td>000 1011</td>
  <td>0.64</td>
 </tr>
 <tr>
  <td>000 1100</td>
  <td>0.73</td>
 </tr>
 <tr>
  <td>000 1101</td>
  <td>0.85</td>
 </tr>
 <tr>
  <td>000 1110</td>
  <td>1.0</td>
 </tr>
 <tr>
  <td>000 1111</td>
  <td>1.3</td>
 </tr>
 <tr>
  <td>001 0000</td>
  <td>1.5</td>
 </tr>
 <tr>
  <td>001 0001</td>
  <td>1.7</td>
 </tr>
 <tr>
  <td>001 0010</td>
  <td>2.0</td>
 </tr>
 <tr>
  <td>001 0011</td>
  <td>2.5</td>
 </tr>
 <tr>
  <td>001 0100</td>
  <td>2.9</td>
 </tr>
 <tr>
  <td>001 0101</td>
  <td>3.4</td>
 </tr>
 <tr>
  <td>001 0110</td>
  <td>4.1</td>
 </tr>
 <tr>
  <td>001 0111</td>
  <td>5.1</td>
 </tr>
 <tr>
  <td>001 1000</td>
  <td>5.8</td>
 </tr>
 <tr>
  <td>001 1001</td>
  <td>6.8</td>
 </tr>
 <tr>
  <td>001 1010</td>
  <td>8.1</td>
 </tr>
 <tr>
  <td>001 1011</td>
  <td>10</td>
 </tr>
 <tr>
  <td>001 1100</td>
  <td>12</td>
 </tr>
 <tr>
  <td>001 1101</td>
  <td>14</td>
 </tr>
 <tr>
  <td>001 1110</td>
  <td>16</td>
 </tr>
 <tr>
  <td>001 1111</td>
  <td>20</td>
 </tr>
 <tr>
  <td>010 0000</td>
  <td>23</td>
 </tr>
 <tr>
  <td>010 0001</td>
  <td>27</td>
 </tr>
 <tr>
  <td>010 0010</td>
  <td>33</td>
 </tr>
 <tr>
  <td>010 0011</td>
  <td>41</td>
 </tr>
</table>


SPU2 Overview Version 6.0


<table>
 <tr>
  <th>Set</th>
  <th>1 -&gt; 0 Time</th>
 </tr>
 <tr>
  <td>Value</td>
  <td></td>
 </tr>
 <tr>
  <td>100 1000</td>
  <td>24 (sec)</td>
 </tr>
 <tr>
  <td>100 1001</td>
  <td>28</td>
 </tr>
 <tr>
  <td>100 1010</td>
  <td>33</td>
 </tr>
 <tr>
  <td>100 1011</td>
  <td>42</td>
 </tr>
 <tr>
  <td>100 1100</td>
  <td>48</td>
 </tr>
 <tr>
  <td>100 1101</td>
  <td>55</td>
 </tr>
 <tr>
  <td>100 1110</td>
  <td>67</td>
 </tr>
 <tr>
  <td>100 1111</td>
  <td>83</td>
 </tr>
 <tr>
  <td>101 0000</td>
  <td>95</td>
 </tr>
 <tr>
  <td>101 0001</td>
  <td>111</td>
 </tr>
 <tr>
  <td>101 0010</td>
  <td>133</td>
 </tr>
 <tr>
  <td>101 0011</td>
  <td>166</td>
 </tr>
 <tr>
  <td>101 0100</td>
  <td>190</td>
 </tr>
 <tr>
  <td>101 0101</td>
  <td>222</td>
 </tr>
 <tr>
  <td>101 0110</td>
  <td>266</td>
 </tr>
 <tr>
  <td>101 0111</td>
  <td>333</td>
 </tr>
 <tr>
  <td>101 1000</td>
  <td>380</td>
 </tr>
 <tr>
  <td>101 1001</td>
  <td>444</td>
 </tr>
 <tr>
  <td>101 1010</td>
  <td>532</td>
 </tr>
 <tr>
  <td>101 1011</td>
  <td>666</td>
 </tr>
 <tr>
  <td>101 1100</td>
  <td>760</td>
 </tr>
 <tr>
  <td>101 1101</td>
  <td>888</td>
 </tr>
 <tr>
  <td>101 1110</td>
  <td>1064</td>
 </tr>
 <tr>
  <td>101 1111</td>
  <td>1332</td>
 </tr>
 <tr>
  <td>110 0000</td>
  <td>1520</td>
 </tr>
 <tr>
  <td>110 0001</td>
  <td>1776</td>
 </tr>
 <tr>
  <td>110 0010</td>
  <td>2128</td>
 </tr>
 <tr>
  <td>110 0011</td>
  <td>2664</td>
 </tr>
 <tr>
  <td>110 1100<br/>:<br/>111 1110</td>
  <td>(reserved)</td>
 </tr>
 <tr>
  <td>111 1111</td>
  <td></td>
 </tr>
</table>


- 73-

- © SCEI




SCE CONFIDENTIAL


## 4.1.4. +Exp Mode (Reverse phase)


Pseudo Inverse-Exponential Mode with Negative Volume Value


<table>
 <tr>
  <th>Set</th>
  <th>1 -&gt; 0 Time</th>
 </tr>
 <tr>
  <td>Value</td>
  <td></td>
 </tr>
 <tr>
  <td>010 0100</td>
  <td>41 (msec)</td>
 </tr>
 <tr>
  <td>010 0101</td>
  <td>46</td>
 </tr>
 <tr>
  <td>010 0110</td>
  <td>54</td>
 </tr>
 <tr>
  <td>010 0111</td>
  <td>64</td>
 </tr>
 <tr>
  <td>010 1000</td>
  <td>81</td>
 </tr>
 <tr>
  <td>010 1001</td>
  <td>93</td>
 </tr>
 <tr>
  <td>010 1010</td>
  <td>0.11 (sec)</td>
 </tr>
 <tr>
  <td>010 1011</td>
  <td>0.13</td>
 </tr>
 <tr>
  <td>010 1100</td>
  <td>0.16</td>
 </tr>
 <tr>
  <td>010 1101</td>
  <td>0.19</td>
 </tr>
 <tr>
  <td>010 1110</td>
  <td>0.22</td>
 </tr>
 <tr>
  <td>010 1111</td>
  <td>0.26</td>
 </tr>
 <tr>
  <td>011 0000</td>
  <td>0.33</td>
 </tr>
 <tr>
  <td>011 0001</td>
  <td>0.37</td>
 </tr>
 <tr>
  <td>011 0010</td>
  <td>0.43</td>
 </tr>
 <tr>
  <td>011 0011</td>
  <td>0.52</td>
 </tr>
 <tr>
  <td>011 0100</td>
  <td>0.65</td>
 </tr>
 <tr>
  <td>011 0101</td>
  <td>0.74</td>
 </tr>
 <tr>
  <td>011 0110</td>
  <td>0.87</td>
 </tr>
 <tr>
  <td>011 0111</td>
  <td>1.0</td>
 </tr>
 <tr>
  <td>011 1000</td>
  <td>1.3</td>
 </tr>
 <tr>
  <td>011 1001</td>
  <td>1.5</td>
 </tr>
 <tr>
  <td>011 1010</td>
  <td>1.7</td>
 </tr>
 <tr>
  <td>011 1011</td>
  <td>2.1</td>
 </tr>
 <tr>
  <td>011 1100</td>
  <td>2.6</td>
 </tr>
 <tr>
  <td>011 1101</td>
  <td>3.0</td>
 </tr>
 <tr>
  <td>011 1110</td>
  <td>3.5</td>
 </tr>
 <tr>
  <td>011 1111</td>
  <td>4.2</td>
 </tr>
 <tr>
  <td>100 0000</td>
  <td>5.2</td>
 </tr>
 <tr>
  <td>100 0001</td>
  <td>5.9</td>
 </tr>
 <tr>
  <td>100 0010</td>
  <td>6.9</td>
 </tr>
 <tr>
  <td>100 0011</td>
  <td>8.3</td>
 </tr>
 <tr>
  <td>100 0100</td>
  <td>10</td>
 </tr>
 <tr>
  <td>100 0101</td>
  <td>12</td>
 </tr>
 <tr>
  <td>100 0110</td>
  <td>14</td>
 </tr>
 <tr>
  <td>100 0111</td>
  <td>17</td>
 </tr>
</table>


<table>
 <tr>
  <th>Set</th>
  <th>1 -&gt; 0 Time</th>
 </tr>
 <tr>
  <td>Value</td>
  <td></td>
 </tr>
 <tr>
  <td>000 0000</td>
  <td>0.08 (msec)</td>
 </tr>
 <tr>
  <td>000 0001</td>
  <td>0.09</td>
 </tr>
 <tr>
  <td>000 0010</td>
  <td>0.11</td>
 </tr>
 <tr>
  <td>000 0011</td>
  <td>0.13</td>
 </tr>
 <tr>
  <td>000 0100</td>
  <td>0.16</td>
 </tr>
 <tr>
  <td>000 0101</td>
  <td>0.18</td>
 </tr>
 <tr>
  <td>000 0110</td>
  <td>0.21</td>
 </tr>
 <tr>
  <td>000 0111</td>
  <td>0.25</td>
 </tr>
 <tr>
  <td>000 1000</td>
  <td>0.32</td>
 </tr>
 <tr>
  <td>000 1001</td>
  <td>0.36</td>
 </tr>
 <tr>
  <td>000 1010</td>
  <td>0.42</td>
 </tr>
 <tr>
  <td>000 1011</td>
  <td>0.51</td>
 </tr>
 <tr>
  <td>000 1100</td>
  <td>0.64</td>
 </tr>
 <tr>
  <td>000 1101</td>
  <td>0.73</td>
 </tr>
 <tr>
  <td>000 1110</td>
  <td>0.85</td>
 </tr>
 <tr>
  <td>000 1111</td>
  <td>1.0</td>
 </tr>
 <tr>
  <td>001 0000</td>
  <td>1.3</td>
 </tr>
 <tr>
  <td>001 0001</td>
  <td>1.5</td>
 </tr>
 <tr>
  <td>001 0010</td>
  <td>1.7</td>
 </tr>
 <tr>
  <td>001 0011</td>
  <td>2.0</td>
 </tr>
 <tr>
  <td>001 0100</td>
  <td>2.5</td>
 </tr>
 <tr>
  <td>001 0101</td>
  <td>2.9</td>
 </tr>
 <tr>
  <td>001 0110</td>
  <td>3.4</td>
 </tr>
 <tr>
  <td>001 0111</td>
  <td>4.1</td>
 </tr>
 <tr>
  <td>001 1000</td>
  <td>5.1</td>
 </tr>
 <tr>
  <td>001 1001</td>
  <td>5.8</td>
 </tr>
 <tr>
  <td>001 1010</td>
  <td>6.8</td>
 </tr>
 <tr>
  <td>001 1011</td>
  <td>8.1</td>
 </tr>
 <tr>
  <td>001 1100</td>
  <td>10</td>
 </tr>
 <tr>
  <td>001 1101</td>
  <td>12</td>
 </tr>
 <tr>
  <td>001 1110</td>
  <td>14</td>
 </tr>
 <tr>
  <td>001 1111</td>
  <td>16</td>
 </tr>
 <tr>
  <td>010 0000</td>
  <td>20</td>
 </tr>
 <tr>
  <td>010 0001</td>
  <td>23</td>
 </tr>
 <tr>
  <td>010 0010</td>
  <td>27</td>
 </tr>
 <tr>
  <td>010 0011</td>
  <td>33</td>
 </tr>
</table>


SPU2 Overview Version 6.0


<table>
 <tr>
  <th>Set</th>
  <th>1 -&gt; 0 Time</th>
 </tr>
 <tr>
  <td>Value</td>
  <td></td>
 </tr>
 <tr>
  <td>100 1000</td>
  <td>21 (sec)</td>
 </tr>
 <tr>
  <td>100 1001</td>
  <td>24</td>
 </tr>
 <tr>
  <td>100 1010</td>
  <td>28</td>
 </tr>
 <tr>
  <td>100 1011</td>
  <td>33</td>
 </tr>
 <tr>
  <td>100 1100</td>
  <td>42</td>
 </tr>
 <tr>
  <td>100 1101</td>
  <td>48</td>
 </tr>
 <tr>
  <td>100 1110</td>
  <td>55</td>
 </tr>
 <tr>
  <td>100 1111</td>
  <td>67</td>
 </tr>
 <tr>
  <td>101 0000</td>
  <td>83</td>
 </tr>
 <tr>
  <td>101 0001</td>
  <td>95</td>
 </tr>
 <tr>
  <td>101 0010</td>
  <td>111</td>
 </tr>
 <tr>
  <td>101 0011</td>
  <td>133</td>
 </tr>
 <tr>
  <td>101 0100</td>
  <td>166</td>
 </tr>
 <tr>
  <td>101 0101</td>
  <td>190</td>
 </tr>
 <tr>
  <td>101 0110</td>
  <td>222</td>
 </tr>
 <tr>
  <td>101 0111</td>
  <td>266</td>
 </tr>
 <tr>
  <td>101 1000</td>
  <td>333</td>
 </tr>
 <tr>
  <td>101 1001</td>
  <td>380</td>
 </tr>
 <tr>
  <td>101 1010</td>
  <td>444</td>
 </tr>
 <tr>
  <td>101 1011</td>
  <td>532</td>
 </tr>
 <tr>
  <td>101 1100</td>
  <td>666</td>
 </tr>
 <tr>
  <td>101 1101</td>
  <td>760</td>
 </tr>
 <tr>
  <td>101 1110</td>
  <td>888</td>
 </tr>
 <tr>
  <td>101 1111</td>
  <td>1064</td>
 </tr>
 <tr>
  <td>110 0000</td>
  <td>1332</td>
 </tr>
 <tr>
  <td>110 0001</td>
  <td>1520</td>
 </tr>
 <tr>
  <td>110 0010</td>
  <td>1776</td>
 </tr>
 <tr>
  <td>110 0011</td>
  <td>2128</td>
 </tr>
 <tr>
  <td>110 1100<br/>:<br/>111 1110</td>
  <td>(reserved)</td>
 </tr>
 <tr>
  <td>111 1111</td>
  <td></td>
 </tr>
</table>


- © SCEI

- 74-




SCE CONFIDENTIAL


## 4.1.5. -Exp Mode


Exponential Decrement Mode


<table>
 <tr>
  <th>Set</th>
  <th>1 -&gt; 0.1</th>
 </tr>
 <tr>
  <td>Value</td>
  <td>Time</td>
 </tr>
 <tr>
  <td>000 0000</td>
  <td>0.07 (msec)</td>
 </tr>
 <tr>
  <td>000 0001</td>
  <td>0.09</td>
 </tr>
 <tr>
  <td>000 0010</td>
  <td>0.11</td>
 </tr>
 <tr>
  <td>000 0011</td>
  <td>0.14</td>
 </tr>
 <tr>
  <td>000 0100</td>
  <td>0.18</td>
 </tr>
 <tr>
  <td>000 0101</td>
  <td>0.21</td>
 </tr>
 <tr>
  <td>000 0110</td>
  <td>0.25</td>
 </tr>
 <tr>
  <td>000 0111</td>
  <td>0.31</td>
 </tr>
 <tr>
  <td>000 1000</td>
  <td>0.39</td>
 </tr>
 <tr>
  <td>000 1001</td>
  <td>0.45</td>
 </tr>
 <tr>
  <td>000 1010</td>
  <td>0.53</td>
 </tr>
 <tr>
  <td>000 1011</td>
  <td>0.64</td>
 </tr>
 <tr>
  <td>000 1100</td>
  <td>0.81</td>
 </tr>
 <tr>
  <td>000 1101</td>
  <td>0.93</td>
 </tr>
 <tr>
  <td>000 1110</td>
  <td>1.1</td>
 </tr>
 <tr>
  <td>000 1111</td>
  <td>1.3</td>
 </tr>
 <tr>
  <td>001 0000</td>
  <td>1.6</td>
 </tr>
 <tr>
  <td>001 0001</td>
  <td>1.9</td>
 </tr>
 <tr>
  <td>001 0010</td>
  <td>2.2</td>
 </tr>
 <tr>
  <td>001 0011</td>
  <td>2.6</td>
 </tr>
 <tr>
  <td>001 0100</td>
  <td>3.3</td>
 </tr>
 <tr>
  <td>001 0101</td>
  <td>3.8</td>
 </tr>
 <tr>
  <td>001 0110</td>
  <td>4.4</td>
 </tr>
 <tr>
  <td>001 0111</td>
  <td>5.3</td>
 </tr>
 <tr>
  <td>001 1000</td>
  <td>6.7</td>
 </tr>
 <tr>
  <td>001 1001</td>
  <td>7.6</td>
 </tr>
 <tr>
  <td>001 1010</td>
  <td>8.9</td>
 </tr>
 <tr>
  <td>001 1011</td>
  <td>11</td>
 </tr>
 <tr>
  <td>001 1100</td>
  <td>13</td>
 </tr>
 <tr>
  <td>001 1101</td>
  <td>15</td>
 </tr>
 <tr>
  <td>001 1110</td>
  <td>18</td>
 </tr>
 <tr>
  <td>001 1111</td>
  <td>21</td>
 </tr>
 <tr>
  <td>010 0000</td>
  <td>27</td>
 </tr>
 <tr>
  <td>010 0001</td>
  <td>31</td>
 </tr>
 <tr>
  <td>010 0010</td>
  <td>36</td>
 </tr>
 <tr>
  <td>010 0011</td>
  <td>43</td>
 </tr>
 <tr>
  <td>010 0100</td>
  <td>53</td>
 </tr>
 <tr>
  <td>010 0101</td>
  <td>61</td>
 </tr>
 <tr>
  <td>010 0110</td>
  <td>71</td>
 </tr>
</table>


<table>
 <tr>
  <th>Set</th>
  <th>1 -&gt; 0.1</th>
 </tr>
 <tr>
  <td>Value</td>
  <td>Time</td>
 </tr>
 <tr>
  <td>010 0111</td>
  <td>86 (msec)</td>
 </tr>
 <tr>
  <td>010 1000</td>
  <td>0.11 (sec)</td>
 </tr>
 <tr>
  <td>010 1001</td>
  <td>0.12</td>
 </tr>
 <tr>
  <td>010 1010</td>
  <td>0.14</td>
 </tr>
 <tr>
  <td>010 1011</td>
  <td>0.17</td>
 </tr>
 <tr>
  <td>010 1100</td>
  <td>0.21</td>
 </tr>
 <tr>
  <td>010 1101</td>
  <td>0.24</td>
 </tr>
 <tr>
  <td>010 1110</td>
  <td>0.29</td>
 </tr>
 <tr>
  <td>010 1111</td>
  <td>0.34</td>
 </tr>
 <tr>
  <td>011 0000</td>
  <td>0.43</td>
 </tr>
 <tr>
  <td>011 0001</td>
  <td>0.49</td>
 </tr>
 <tr>
  <td>011 0010</td>
  <td>0.57</td>
 </tr>
 <tr>
  <td>011 0011</td>
  <td>0.68</td>
 </tr>
 <tr>
  <td>011 0100</td>
  <td>0.86</td>
 </tr>
 <tr>
  <td>011 0101</td>
  <td>0.98</td>
 </tr>
 <tr>
  <td>011 0110</td>
  <td>1.1</td>
 </tr>
 <tr>
  <td>011 0111</td>
  <td>1.4</td>
 </tr>
 <tr>
  <td>011 1000</td>
  <td>1.7</td>
 </tr>
 <tr>
  <td>011 1001</td>
  <td>2.0</td>
 </tr>
 <tr>
  <td>011 1010</td>
  <td>2.3</td>
 </tr>
 <tr>
  <td>011 1011</td>
  <td>2.7</td>
 </tr>
 <tr>
  <td>011 1100</td>
  <td>3.4</td>
 </tr>
 <tr>
  <td>011 1101</td>
  <td>3.9</td>
 </tr>
 <tr>
  <td>011 1110</td>
  <td>4.6</td>
 </tr>
 <tr>
  <td>011 1111</td>
  <td>5.5</td>
 </tr>
 <tr>
  <td>100 0000</td>
  <td>6.8</td>
 </tr>
 <tr>
  <td>100 0001</td>
  <td>7.8</td>
 </tr>
 <tr>
  <td>100 0010</td>
  <td>9.1</td>
 </tr>
 <tr>
  <td>100 0011</td>
  <td>11</td>
 </tr>
 <tr>
  <td>100 0100</td>
  <td>14</td>
 </tr>
 <tr>
  <td>100 0101</td>
  <td>16</td>
 </tr>
 <tr>
  <td>100 0110</td>
  <td>18</td>
 </tr>
 <tr>
  <td>100 0111</td>
  <td>22</td>
 </tr>
 <tr>
  <td>100 1000</td>
  <td>27</td>
 </tr>
 <tr>
  <td>100 1001</td>
  <td>31</td>
 </tr>
 <tr>
  <td>100 1010</td>
  <td>36</td>
 </tr>
 <tr>
  <td>100 1011</td>
  <td>44</td>
 </tr>
 <tr>
  <td>100 1100</td>
  <td>55</td>
 </tr>
 <tr>
  <td>100 1101</td>
  <td>63</td>
 </tr>
</table>


SPU2 Overview Version 6.0


<table>
 <tr>
  <th>Set</th>
  <th>1 -&gt; 0.1</th>
 </tr>
 <tr>
  <td>Value</td>
  <td>Time</td>
 </tr>
 <tr>
  <td>100 1110</td>
  <td>73 (sec)</td>
 </tr>
 <tr>
  <td>100 1111</td>
  <td>88</td>
 </tr>
 <tr>
  <td>101 0000</td>
  <td>109</td>
 </tr>
 <tr>
  <td>101 0001</td>
  <td>125</td>
 </tr>
 <tr>
  <td>101 0010</td>
  <td>146</td>
 </tr>
 <tr>
  <td>101 0011</td>
  <td>175</td>
 </tr>
 <tr>
  <td>101 0100</td>
  <td>219</td>
 </tr>
 <tr>
  <td>101 0101</td>
  <td>250</td>
 </tr>
 <tr>
  <td>101 0110</td>
  <td>292</td>
 </tr>
 <tr>
  <td>101 0111</td>
  <td>350</td>
 </tr>
 <tr>
  <td>101 1000</td>
  <td>438</td>
 </tr>
 <tr>
  <td>101 1001</td>
  <td>500</td>
 </tr>
 <tr>
  <td>101 1010</td>
  <td>584</td>
 </tr>
 <tr>
  <td>101 1011</td>
  <td>700</td>
 </tr>
 <tr>
  <td>101 1100</td>
  <td>876</td>
 </tr>
 <tr>
  <td>101 1101</td>
  <td>1000</td>
 </tr>
 <tr>
  <td>101 1110</td>
  <td>1168</td>
 </tr>
 <tr>
  <td>101 1111</td>
  <td>1400</td>
 </tr>
 <tr>
  <td>110 0000</td>
  <td>1752</td>
 </tr>
 <tr>
  <td>110 0001</td>
  <td>2000</td>
 </tr>
 <tr>
  <td>110 0010</td>
  <td>2336</td>
 </tr>
 <tr>
  <td>110 0011</td>
  <td>2800</td>
 </tr>
 <tr>
  <td>110 0100</td>
  <td>3504</td>
 </tr>
 <tr>
  <td>110 0101</td>
  <td>4000</td>
 </tr>
 <tr>
  <td>110 0110</td>
  <td>4672</td>
 </tr>
 <tr>
  <td>110 0111</td>
  <td>5600</td>
 </tr>
 <tr>
  <td>110 1000</td>
  <td>7008</td>
 </tr>
 <tr>
  <td>110 1001</td>
  <td>8000</td>
 </tr>
 <tr>
  <td>110 1010</td>
  <td>9344</td>
 </tr>
 <tr>
  <td>110 1011</td>
  <td>11200</td>
 </tr>
 <tr>
  <td>110 1100<br/>:<br/>111 1110</td>
  <td>(reserved)</td>
 </tr>
 <tr>
  <td>111 1111</td>
  <td></td>
 </tr>
</table>


- 75-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


## 4.1.6. Decay Rate (DR)


Exponential Decrement Mode


<table>
 <tr>
  <th>Set</th>
  <th>1 -&gt; 0.10</th>
 </tr>
 <tr>
  <td>Value</td>
  <td>Time</td>
 </tr>
 <tr>
  <td>0000</td>
  <td>0.07</td>
 </tr>
 <tr>
  <td>0001</td>
  <td>0.18</td>
 </tr>
 <tr>
  <td>0010</td>
  <td>0.39</td>
 </tr>
 <tr>
  <td>0011</td>
  <td>0.81</td>
 </tr>
 <tr>
  <td>0100</td>
  <td>1.6</td>
 </tr>
 <tr>
  <td>0101</td>
  <td>3.3</td>
 </tr>
 <tr>
  <td>0110</td>
  <td>6.7</td>
 </tr>
 <tr>
  <td>0111</td>
  <td>13</td>
 </tr>
 <tr>
  <td>1000</td>
  <td>27</td>
 </tr>
 <tr>
  <td>1001</td>
  <td>53</td>
 </tr>
 <tr>
  <td>1010</td>
  <td>0.11</td>
 </tr>
 <tr>
  <td>1011</td>
  <td>0.21</td>
 </tr>
 <tr>
  <td>1100</td>
  <td>0.43</td>
 </tr>
 <tr>
  <td>1101</td>
  <td>0.86</td>
 </tr>
 <tr>
  <td>1110</td>
  <td>1.7</td>
 </tr>
 <tr>
  <td>1111</td>
  <td>3.4</td>
 </tr>
</table>


- © SCEI

- 76-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


## 4.1.7. Sustain Level (SL)


<table>
 <tr>
  <th>Set Value</th>
  <th></th>
 </tr>
 <tr>
  <td></td>
  <td>Level</td>
 </tr>
 <tr>
  <td>0000</td>
  <td>1/16</td>
 </tr>
 <tr>
  <td>0001</td>
  <td>2/16</td>
 </tr>
 <tr>
  <td>0010</td>
  <td>3/16</td>
 </tr>
 <tr>
  <td>0011</td>
  <td>4/16</td>
 </tr>
 <tr>
  <td>0100</td>
  <td>5/16</td>
 </tr>
 <tr>
  <td>0101</td>
  <td>6/16</td>
 </tr>
 <tr>
  <td>0110</td>
  <td>7/16</td>
 </tr>
 <tr>
  <td>0111</td>
  <td>8/16</td>
 </tr>
 <tr>
  <td>1000</td>
  <td>9/16</td>
 </tr>
 <tr>
  <td>1001</td>
  <td>10/16</td>
 </tr>
 <tr>
  <td>1010</td>
  <td>11/16</td>
 </tr>
 <tr>
  <td>1011</td>
  <td>12/16</td>
 </tr>
 <tr>
  <td>1100</td>
  <td>13/16</td>
 </tr>
 <tr>
  <td>1101</td>
  <td>14/16</td>
 </tr>
 <tr>
  <td>1110</td>
  <td>15/16</td>
 </tr>
 <tr>
  <td>1111</td>
  <td>1</td>
 </tr>
</table>


Envelope
the set value is 1111, the Decay section


Note: when


immediately before is processed only for 1 Ts.


- 77-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


## 4.1.8. -Lin Mode for Release Rate (RR)


Linear Decrement Mode


<table>
 <tr>
  <th>-Lin Set</th>
  <th>1 -&gt; 0 Time</th>
 </tr>
 <tr>
  <td>Value</td>
  <td></td>
 </tr>
 <tr>
  <td>0 0000</td>
  <td>0.04</td>
 </tr>
 <tr>
  <td>0 0001</td>
  <td>0.09</td>
 </tr>
 <tr>
  <td>0 0010</td>
  <td>0.18</td>
 </tr>
 <tr>
  <td>0 0011</td>
  <td>0.36</td>
 </tr>
 <tr>
  <td>0 0100</td>
  <td>0.73</td>
 </tr>
 <tr>
  <td>0 0101</td>
  <td>1.5</td>
 </tr>
 <tr>
  <td>0 0110</td>
  <td>2.9</td>
 </tr>
 <tr>
  <td>0 0111</td>
  <td>5.8</td>
 </tr>
 <tr>
  <td>0 1000</td>
  <td>12</td>
 </tr>
 <tr>
  <td>0 1001</td>
  <td>23</td>
 </tr>
 <tr>
  <td>0 1010</td>
  <td>46</td>
 </tr>
 <tr>
  <td>0 1011</td>
  <td>93</td>
 </tr>
 <tr>
  <td>0 1100</td>
  <td>0.19</td>
 </tr>
 <tr>
  <td>0 1101</td>
  <td>0.37</td>
 </tr>
 <tr>
  <td>0 1110</td>
  <td>0.74</td>
 </tr>
 <tr>
  <td>0 1111</td>
  <td>1.5</td>
 </tr>
 <tr>
  <td>1 0000</td>
  <td>3.0</td>
 </tr>
 <tr>
  <td>1 0001</td>
  <td>5.9</td>
 </tr>
 <tr>
  <td>1 0010</td>
  <td>12</td>
 </tr>
 <tr>
  <td>1 0011</td>
  <td>24</td>
 </tr>
 <tr>
  <td>1 0100</td>
  <td>48</td>
 </tr>
 <tr>
  <td>1 0101</td>
  <td>95</td>
 </tr>
 <tr>
  <td>1 0110</td>
  <td>190</td>
 </tr>
 <tr>
  <td>1 0111</td>
  <td>380</td>
 </tr>
 <tr>
  <td>1 1000</td>
  <td>760</td>
 </tr>
 <tr>
  <td>1 1001</td>
  <td>1520</td>
 </tr>
 <tr>
  <td>1 1010</td>
  <td>3040</td>
 </tr>
 <tr>
  <td>1 1011<br/>:<br/>1 1110</td>
  <td>(reserved)</td>
 </tr>
 <tr>
  <td>1 1111</td>
  <td>infinity</td>
 </tr>
</table>


- © SCEI

- 78-




SCE CONFIDENTIAL


SPU2 Overview Version 6.0


## 4.1.9. -Exp Mode for Release Rate (RR)


Exponential Decrement Mode


<table>
 <tr>
  <th>Set</th>
  <th>1 -&gt; 0.1</th>
 </tr>
 <tr>
  <td>Value</td>
  <td>Time</td>
 </tr>
 <tr>
  <td>0 0000</td>
  <td>0.07</td>
 </tr>
 <tr>
  <td>0 0001</td>
  <td>0.18</td>
 </tr>
 <tr>
  <td>0 0010</td>
  <td>0.39</td>
 </tr>
 <tr>
  <td>0 0011</td>
  <td>0.81</td>
 </tr>
 <tr>
  <td>0 0100</td>
  <td>1.6</td>
 </tr>
 <tr>
  <td>0 0101</td>
  <td>3.3</td>
 </tr>
 <tr>
  <td>0 0110</td>
  <td>6.7</td>
 </tr>
 <tr>
  <td>0 0111</td>
  <td>13</td>
 </tr>
 <tr>
  <td>0 1000</td>
  <td>27</td>
 </tr>
 <tr>
  <td>0 1001</td>
  <td>53</td>
 </tr>
 <tr>
  <td>0 1010</td>
  <td>0.11</td>
 </tr>
 <tr>
  <td>0 1011</td>
  <td>0.21</td>
 </tr>
 <tr>
  <td>0 1100</td>
  <td>0.43</td>
 </tr>
 <tr>
  <td>0 1101</td>
  <td>0.86</td>
 </tr>
 <tr>
  <td>0 1110</td>
  <td>1.7</td>
 </tr>
 <tr>
  <td>0 1111</td>
  <td>3.4</td>
 </tr>
 <tr>
  <td>1 0000</td>
  <td>6.8</td>
 </tr>
 <tr>
  <td>1 0001</td>
  <td>14</td>
 </tr>
 <tr>
  <td>1 0010</td>
  <td>27</td>
 </tr>
 <tr>
  <td>1 0011</td>
  <td>55</td>
 </tr>
 <tr>
  <td>1 0100</td>
  <td>109</td>
 </tr>
 <tr>
  <td>1 0101</td>
  <td>219</td>
 </tr>
 <tr>
  <td>1 0110</td>
  <td>438</td>
 </tr>
 <tr>
  <td>1 0111</td>
  <td>876</td>
 </tr>
 <tr>
  <td>1 1000</td>
  <td>1752</td>
 </tr>
 <tr>
  <td>1 1001</td>
  <td>3504</td>
 </tr>
 <tr>
  <td>1 1010</td>
  <td>7008</td>
 </tr>
 <tr>
  <td>1 1011<br/>:<br/>1 1110</td>
  <td>(reserved)</td>
 </tr>
 <tr>
  <td>1 1111</td>
  <td>infinity</td>
 </tr>
</table>


- 79-

- © SCEI




SCE CONFIDENTIAL SPU2 Overview Version 6.0


(This page is left blank intentionally)


- © SCEI

- 80-


