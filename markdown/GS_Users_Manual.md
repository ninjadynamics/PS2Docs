

# GS User's Manual


Copyright © 2002 Sony Computer Entertainment Inc.
All Rights Reserved.
SCE Confidential




SCE CONFIDENTIAL GS User's Manual Version 6.0


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


The GS User's Manual is supplied pursuant to and subject to the terms of the Sony Computer Entertainment
PlayStation® license agreements.


The GS User's Manual is intended for distribution to and use by only Sony Computer Entertainment licensed
Developers and Publishers in accordance with the PlayStation® license agreements.


Unauthorized reproduction, distribution, lending, rental or disclosure to any third party, in whole or in part, of
this book is expressly prohibited by law and by the terms of the Sony Computer Entertainment PlayStation®
license agreements.


Ownership of the physical property of the book is retained by and reserved by Sony Computer Entertainment.
Alteration to or deletion, in whole or in part, of the book, its presentation, or its contents is prohibited.


The information in the GS User's Manual is subject to change without notice. The content of this book is
Confidential Information of Sony Computer Entertainment.


® and PlayStation® are registered trademarks, and GRAPHICS SYNTHESIZERTM and


© SCEI


- 2-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## About This Manual


The "GS User's Manual" describes the functional specifications and command set of the Graphics Synthesizer.
For information on how to access the Graphics Synthesizer from the Emotion Engine, refer to the "EE User's
Manual".


- Chapter 1 "Overview" describes the features, block configuration, and performance characteristics of the
Graphics Synthesizer.
- Chapter 2 "Local Memory" describes the Graphics Synthesizer's local memory and the formats for data
stored in memory.
- Chapter 3 "Drawing Function" describes the drawing primitives, called GS primitives, and the drawing
environment. It also describes the stages of pixel generation based on the GS primitives and drawing
environment, and explains texture mapping, fogging, antialiasing, pixel test, alpha-blending, and dithering.
- Chapter 4 "Image Data Transmission" describes the functions of data transmission between the Graphics
Synthesizer and the Emotion Engine, and within local memory.
- Chapter 5 "CRTC" describes the video signal output functions and feedback input functions.
- Chapter 6 "Signal" describes the signal generation function.
- Chapter 7 "Registers" describes the registers accessible from the Emotion Engine.
- Chapter 8 "Details of GS Local Memory" provides detailed descriptions of local memory addressing.


## Changes Since Release of 5th Edition


Since release of the 5th Edition of the GS User's Manual, the following changes have been made.
Note that each of these changes is indicated by a revision bar in the margin of the affected page.


## Ch. 1: Overview


- A correction has been made to "Local Memory" in the GS functional block description on page 17.

- A correction has been made to the "Shading" column of the Processing Performance in Sprite table on page 18.

- A correction has been made to the "Shading" column of the Processing Performance in Point table on page 18.


## Ch. 3: Drawing Function


- Figure 3-1 Block Diagram for Drawing Processing, on page 34, has been revised.

- A correction has been made to "Pixel Test" in the process descriptions on page 35.

- Corrections have been made to section 3.4.1. Outline of Texture Mapping Process, on page 50. The texture page buffer was also added to Figure 3-9 Texture Mapping Process Flow. • A new section, "Texture Page Buffer", has been added to section 3.4.2. Texture Mapping Process Flow, on page 51. This section provides details about the texture page buffer. • Information about the texture coordinate range was added to section 3.4.4. Specification of Texture Coordinate values, on page 52. • A correction has been made to section 3.7. Pixel Test, on page 66. Further clarification about the position of scissoring in the pixel pipeline was also added. • A correction has been made to section 3.7.2. Alpha Test, on page 67. • A correction has been made to section 3.7.3. Destination Alpha Test, on page 68.

- A correction has been made to section 3.7.4. Depth Test, on page 68. A note on hardware bug relating to Z test modes was also added.

- 3-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## Ch. 4: Image Data Transmission


• Corrections have been made to the introduction for section 4. Image Date Transmission, on page 73.
• The procedure in section 4.2.2. Transmission from Local Buffer to Host, on page 77, has been revised.
• Corrections have been made to section 4.2.3. Transmission from Local Buffer to Local Buffer, on page
77.
• Section 4.2.4. Restrictions on Drawing after Transmission, has been added to page 78. This section
contains information about a hardware bug related to texture transmission and drawing on the same
page.


## Ch. 5: CRTC


• Corrections have been made to the "NTSC" and "PAL" rows in the Corresponding Video Signal table
on page 84.


## Ch. 6: Signal


-  page 95.

-  95.


Corrections have been made and information has been added to section 6.2.1. SIGNAL Register, on


Corrections have been made and information has been added to section 6.2.2. FINSH Register, on page


## Ch. 7: Registers


- A reference note (*3) has been added to the BITBLTBUF register FIELD table on page 101.

- A correction has been made to the "Contents" column in the FBA_1/FBA_2 register FIELD table on page 106. • Information has been added to the notes for the RGBAQ register FIELD table on page 119.

- Information has been added to the "S" and "T" rows in the ST register FIELD table on page 123.

- A correction has been made to the "ZTE" row in the TEST_1/TEST_2 register FIELD table on page 124.

- Information has been added to the description of the BUSDIR (w) register on page 144.

- A correction has been made to the note for the CSR (r/w) register FIELD table on page 146.


## Ch. 8: Details of GS Local Memory


- Corrections have been made and information added to "Column" in section 8.1. Memory Access Units, on page 162.

- Information has been added to section 8.3.1. PSMCT32/PSMCT24/PSMZ32/PSMZ24, on page 164.

- A correction has been made to section 8.6. Pixel Storage Format Conversion, on page 172.

- A correction has been made to section 8.6.3. PSMT16, on page 175.

- © SCEI

- 4-




SCE CONFIDENTIAL GS User's Manual Version 6.0


Glossary


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
  <td>Graphics Synthesizer.<br/>connected to EE.</td>
 </tr>
 <tr>
  <td>GIF</td>
  <td>EE Interface unit to GS.</td>
 </tr>
 <tr>
  <td>IOP</td>
  <td>Processor connected to EE for devices.</td>
 </tr>
 <tr>
  <td>SBUS</td>
  <td>Bus EE to IOP.</td>
 </tr>
 <tr>
  <td>VPU (VPU0/VPU1)</td>
  <td>Vector operation unit.<br/>EE contains 2 VPUs: VPU0 and VPU1.</td>
 </tr>
 <tr>
  <td>VU (VU0/VU1)</td>
  <td>VPU core operation unit.</td>
 </tr>
 <tr>
  <td>VIF (VIF0/VIF1)</td>
  <td>VPU data decompression unit.</td>
 </tr>
 <tr>
  <td>VIFcode</td>
  <td>Instruction code for VIF.</td>
 </tr>
 <tr>
  <td>SPR</td>
  <td>Quick-access data memory built into EE Core (Scratchpad memory).</td>
 </tr>
 <tr>
  <td>IPU</td>
  <td>EE Image processor unit.</td>
 </tr>
 <tr>
  <td>word</td>
  <td>Unit of data length: 32 bits</td>
 </tr>
 <tr>
  <td>qword</td>
  <td>Unit of data length: 128 bits</td>
 </tr>
 <tr>
  <td>Slice</td>
  <td>Physical unit of DMA transfer: 8 qwords or less</td>
 </tr>
 <tr>
  <td>Packet</td>
  <td>Data to be handled as a logical unit for transfer processing.</td>
 </tr>
 <tr>
  <td>Transfer list</td>
  <td>A group of packets transferred in serial DMA transfer processing.</td>
 </tr>
 <tr>
  <td>Tag</td>
  <td>Additional data indicating data size and other attributes of packets.</td>
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
  <td></td>
  <td>Additional data to indicate attributes of GS</td>
 </tr>
 <tr>
  <td>list</td>
  <td>A of GS to indicate batches of</td>
 </tr>
</table>


- 5-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


(This page is left blank intentionally)


- © SCEI

- 6-




SCE CONFIDENTIAL


GS User's Manual Version 6.0


## Contents


1. Overview.................................................................................................................................................................................13 1.1. GS Features ....................................................................................................................................................................14 1.1.1. Drawing Process.....................................................................................................................................................14 1.1.2. Local Memory.........................................................................................................................................................15 1.1.3. Video I/O ...............................................................................................................................................................15 1.2. Block Configuration ......................................................................................................................................................16 1.2.1. Whole Block Diagram ...........................................................................................................................................16 1.3. Maximum Performance.................................................................................................................................................18 2. Local Memory........................................................................................................................................................................19 2.1. Data Stored in Local Memory......................................................................................................................................20 2.2. Addressing ......................................................................................................................................................................21 2.2.1. Address Value.........................................................................................................................................................21 2.2.2. Starting Address of Buffer ....................................................................................................................................21 2.2.3. Address in Buffer ...................................................................................................................................................21 2.3. Data Formats..................................................................................................................................................................22 2.3.1. Color Data Format (Pixel/Texel Format)...........................................................................................................22 2.3.2. Z Value Format ......................................................................................................................................................22 2.4. Frame Buffer ..................................................................................................................................................................23 2.4.1. Size of Frame Buffer..............................................................................................................................................23 2.4.2. Starting Address of Frame Buffer........................................................................................................................23 2.4.3. Coordinate Systems................................................................................................................................................23 2.4.4. Pixel Correspondence............................................................................................................................................24 2.4.5. Pixel Storage Format..............................................................................................................................................25 2.5. Z Buffer...........................................................................................................................................................................26 2.5.1. Starting Address of Z Buffer ................................................................................................................................26 2.5.2. Coordinate System of Z Buffer............................................................................................................................26 2.5.3. Z Value Storage Format........................................................................................................................................26 2.5.4. Combinations of Frame Buffer and Z Buffer Formats ....................................................................................26 2.6. Texture Buffer................................................................................................................................................................27 2.6.1. Starting Address of Texture Buffer......................................................................................................................27 2.6.2. Coordinate System of Texture Buffer .................................................................................................................27 2.6.3. Texel Storage Format.............................................................................................................................................29 2.7. CLUT Buffer ..................................................................................................................................................................30 2.7.1. CLUT Configuration .............................................................................................................................................30 2.7.2. Starting Address......................................................................................................................................................30 2.7.3. CLUT Storage Mode..............................................................................................................................................30 2.7.4. CLUT Storage Format...........................................................................................................................................31 3. Drawing Function .................................................................................................................................................................33 3.1. Outline of Drawing Function.......................................................................................................................................34 3.2. GS Primitive ...................................................................................................................................................................36

- © SCEI

- 7-




SCE CONFIDENTIAL


GS User's Manual Version 6.0


1. Types of GS Primitives ..........................................................................................................................................36


3.2.2. Drawing Attributes.................................................................................................................................................37
3.2.3. Outline of Drawing Procedure .............................................................................................................................38
3.2.4. Example of the Drawing Procedure.....................................................................................................................39
3.2.5. Drawing Control.....................................................................................................................................................42
3.2.6. Change of Drawing Attributes..............................................................................................................................43
3.2.7. Register Setting Order and Drawing Order ........................................................................................................43
3.2.8. Enabled Vertex Information.................................................................................................................................43
3.2.9. Primitive Drawing Rule..........................................................................................................................................44


1. Drawing Environment...................................................................................................................................................47 3.3.1. Drawing Environment Setting Registers .............................................................................................................47 3.3.2. Two Context Drawing Environment...................................................................................................................47


3.4. Texture Mapping ............................................................................................................................................................50
3.4.1. Outline of Texture Mapping Process...................................................................................................................50
3.4.2. Texture Mapping Process Flow............................................................................................................................50
3.4.3. Texture Information Setting..................................................................................................................................51
3.4.4. Specification of Texture Coordinate Values .......................................................................................................52
3.4.5. Texture Wrap Modes..............................................................................................................................................52
3.4.6. Format Conversion.................................................................................................................................................54
3.4.7. CLUT Buffer Control ............................................................................................................................................55
3.4.8. Texel Sampling........................................................................................................................................................58
3.4.9. Texture Function ....................................................................................................................................................59
3.4.10. Perspective Correction.........................................................................................................................................60
3.4.11. MIPMAP................................................................................................................................................................61
3.4.12. LOD Setting ..........................................................................................................................................................62
3.4.13. Texture Mapping Procedure ...............................................................................................................................63
3.5. Fog Effect........................................................................................................................................................................64
3.6. Antialiasing ......................................................................................................................................................................65
3.6.1. Principle of Antialiasing.........................................................................................................................................65
3.6.2. Antialiasing Control................................................................................................................................................65
3.7. Pixel Test .........................................................................................................................................................................66
3.7.1. Scissoring Test.........................................................................................................................................................66
3.7.2. Alpha Test................................................................................................................................................................67
3.7.3. Destination Alpha Test..........................................................................................................................................68
3.7.4. Depth Test...............................................................................................................................................................68
3.8. Alpha Blending ...............................................................................................................................................................69
3.8.1. Blending Setting ......................................................................................................................................................69
3.8.2. PABE Flag ...............................................................................................................................................................69
3.9. Writing to the Frame Buffer .........................................................................................................................................70
3.9.1. Dithering..................................................................................................................................................................70
3.9.2. Color Clamp ............................................................................................................................................................71
3.9.3. Alpha Value Correction .........................................................................................................................................71
3.9.4. Format Conversion.................................................................................................................................................71
3.9.5. Masking ....................................................................................................................................................................72


1. Image Data Transmission.....................................................................................................................................................73 4.1. Transmission Parameters...............................................................................................................................................74

- © SCEI

- 8-




SCE CONFIDENTIAL


GS User's Manual Version 6.0


1. Destination Buffer / Source Buffer (BITBLTBUF Register) ..........................................................................74 4.1.2. Offset in Buffer (TRXPOS Register) ..................................................................................................................74 4.1.3. Width and Height of Transmission Area (TRXREG Register) .......................................................................75 4.1.4. Transmission Direction and Transmission Start (TRXDIR Register) ............................................................75 4.1.5. Limitations on Transmission Start Coordinates and Width .............................................................................76


4.2. Transmission Process....................................................................................................................................................77
4.2.1. Transmission from Host to Local Buffer ...........................................................................................................77
4.2.2. Transmission from Local Buffer to Host ...........................................................................................................77
4.2.3. Transmission from Local Buffer to Local Buffer ..............................................................................................77
4.2.4. Restrictions on Drawing after Transmission......................................................................................................78


1. Transmission Data Format...........................................................................................................................................79

2. CRTC......................................................................................................................................................................................81 5.1. CRTC Function..............................................................................................................................................................82 5.1.1. PCRTC Features.....................................................................................................................................................82 5.1.2. Corresponding Video Signal .................................................................................................................................84 5.2. Video Output..................................................................................................................................................................85 5.2.1. Video Clock.............................................................................................................................................................85 5.2.2. Rectangular Area Read Output Circuit................................................................................................................85 5.2.3. Merge Circuit ..........................................................................................................................................................87 5.3. Feedback Write...............................................................................................................................................................90 5.3.1. Rectangular Area Write Input Circuit..................................................................................................................90

3. Signal .......................................................................................................................................................................................93 6.1. Outline of Signal Processing.........................................................................................................................................94 6.2. Signal Register ................................................................................................................................................................95 6.2.1. SIGNAL Register...................................................................................................................................................95 6.2.2. FINISH Register ....................................................................................................................................................95 6.2.3. LABEL Register .....................................................................................................................................................95

4. Registers..................................................................................................................................................................................97 7.1. General Purpose Registers............................................................................................................................................98 ALPHA_1 / ALPHA_2 : Alpha Blending Setting.....................................................................................................100 BITBLTBUF : Setting for Transmission between Buffers .......................................................................................101 CLAMP_1 / CLAMP_2 : Texture Wrap Mode.........................................................................................................102 COLCLAMP : Color Clamp Control...........................................................................................................................103 DIMX : Dither Matrix Setting ......................................................................................................................................104 DTHE : Dither Control.................................................................................................................................................105 FBA_1 / FBA_2 : Alpha Correction Value................................................................................................................106 FINISH : FINISH Event Occurrence Request..........................................................................................................107 FOG : Vertex Fog Value Setting ..................................................................................................................................108 FOGCOL : Distant Fog Color Setting........................................................................................................................109 FRAME_1 / FRAME_2 : Frame Buffer Setting .......................................................................................................110 HWREG : Data Port for Transmission between Buffers.........................................................................................111 LABEL : LABEL Event Occurrence Request ...........................................................................................................112 MIPTBP1_1 / MIPTBP1_2 : MIPMAP Information Setting (Level 1 to 3).........................................................113 MIPTBP2_1 / MIPTBP2_2 : MIPMAP Information Setting (Level 4 to 6).........................................................114 PABE : Alpha Blending Control in Units of Pixels ...................................................................................................115 PRIM : Drawing Primitive Setting................................................................................................................................116

- © SCEI

- 9-




SCE CONFIDENTIAL


GS User's Manual Version 6.0


PRMODE : Setting for Attributes of Drawing Primitives ....................................................................................... 117
PRMODECONT : Specification of Primitive Attribute Setting Method.............................................................. 118
RGBAQ : Vertex Color Setting.................................................................................................................................... 119
SCANMSK : Raster Address Mask Setting ................................................................................................................ 120
SCISSOR_1 / SCISSOR_2 : Setting for Scissoring Area......................................................................................... 121
SIGNAL : SIGNAL Event Occurrence Request ...................................................................................................... 122
ST : Specification of Vertex Texture Coordinates..................................................................................................... 123
TEST_1 / TEST_2 : Pixel Test Control..................................................................................................................... 124
TEX0_1 / TEX0_2 : Texture Information Setting................................................................................................... 125
TEX1_1 / TEX1_2 : Texture Information Setting................................................................................................... 127
TEX2_1 / TEX2_2 : Texture Information Setting................................................................................................... 128
TEXA : Texture Alpha Value Setting.......................................................................................................................... 129
TEXCLUT : CLUT Position Specification ................................................................................................................ 130
TEXFLUSH : Texture Page Buffer Disabling ........................................................................................................... 131
TRXDIR : Activation of Transmission between Buffers ......................................................................................... 132
TRXPOS : Specification of Transmission Areas in Buffers..................................................................................... 133
TRXREG : Specification of Transmission Areas in Buffers.................................................................................... 134
UV : Specification of Vertex Texture Coordinates.................................................................................................... 135
XYOFFSET_1 / XYOFFSET_2 : Offset Value Setting ......................................................................................... 136
XYZ2 : Setting for Vertex Coordinate Values ........................................................................................................... 137
XYZ3 : Setting for Vertex Coordinate Values (without Drawing Kick) ................................................................ 138
XYZF2 : Setting for Vertex Coordinate Values......................................................................................................... 139
XYZF3 : Setting for Vertex Coordinate Values (without Drawing Kick).............................................................. 140
ZBUF_1 / ZBUF_2 : Z Buffer Setting ...................................................................................................................... 141


1. Privileged Registers ..................................................................................................................................................... 142 BGCOLOR (w) : Background Color Setting ............................................................................................................. 143 BUSDIR (w) : Host Interface Bus Switching............................................................................................................. 144 CSR (r/w) : System Status............................................................................................................................................. 145 DISPFB1 (w) : Setting for Rectangular Area Read Output Circuit 1...................................................................... 147 DISPFB2 (w) : Setting for Rectangular Area Read Output Circuit 2...................................................................... 148 DISPLAY1 (w) : Setting for Rectangular Area Read Output Circuit 1 .................................................................. 149 DISPLAY2 (w) : Setting for Rectangular Area Read Output Circuit 2 .................................................................. 150 EXTBUF (w) : Setting for Feedback Write Buffer.................................................................................................... 151 EXTDATA (w) : Feedback Write Setting .................................................................................................................. 152 EXTWRITE (w) : Feedback Write Function Control.............................................................................................. 153 IMR (w) : Interrupt Mask Control............................................................................................................................... 154 PMODE (w) : PCRTC Mode Setting.......................................................................................................................... 155 SIGLBLID (r/w) : Signal ID Value Read................................................................................................................... 156 SMODE2 (w) : Setting for Modes Related to Video Synchronization................................................................... 157


7.3. Register List in Address Order.................................................................................................................................. 158


1. Details of GS Local Memory............................................................................................................................................. 161 8.1. Memory Access Units................................................................................................................................................. 162 8.2. Page Arrangement in Buffer ...................................................................................................................................... 163 8.3. Data Structure in a Page ............................................................................................................................................. 164 8.3.1. PSMCT32/PSMCT24/PSMZ32/PSMZ24..................................................................................................... 164 8.3.2. PSMCT16/PSMCT16S....................................................................................................................................... 165

- © SCEI

- 10-




SCE CONFIDENTIAL GS User's Manual Version 6.0


1. PSMT8...................................................................................................................................................................166


8.3.4. PSMT4...................................................................................................................................................................168


1. Occupied Memory Area..............................................................................................................................................170 8.5. Pointing within a Page.................................................................................................................................................171 8.6. Pixel Storage Format Conversion..............................................................................................................................172 8.6.1. PSMT4...................................................................................................................................................................172 8.6.2. PSMT8...................................................................................................................................................................174 8.6.3. PSMT16.................................................................................................................................................................175

- 11-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


(This page is left blank intentionally)


- © SCEI

- 12-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 1. Overview


- 13-

- © SCEI




SCE CONFIDENTIAL


GS User's Manual Version 6.0


## 1.1. GS Features


The GS is a high-performance graphics processor. The following sections summarize the features of the
drawing process, local memory and video input/output.


## 1.1.1. Drawing Process


The GS draws primitives, such as Polygon or Line, to the frame buffer in local memory, based on drawing
requests from the host processor. The main features are:


Full color internal operation


32-bit calculation precision (8 bits each for RGB + 8 bits for Alpha value)


A wide variety of drawing primitives
Point, Line, LineStrip, Triangle, TriangleStrip, TriangleFan, and Sprite


Shading
Flat shading and Gouraud shading


Texture mapping
Perspective correction
Bilinear and trilinear texture mapping
MIPMAP
Tiled textures specified by wrap mode


Texture mode
16/24/32 bits (without CLUT), 4/8 bits (with CLUT)


Z buffer
High-speed Z buffer processing without speed decrease
Z value format of 3 types: 16 bits, 24 bits and 32 bits


Alpha blending
High-speed blending without speed decrease


Edge antialiasing
Removing the edge aliasing of Lines and Triangles


Fogging
Fog effects in pixel units enabled for all primitives


High-efficiency scissoring
High-efficiency scissoring to negate the need for scissoring in geometry calculation


Registers with 2 contexts
Makes saving and restoring the drawing environment by a context switch unnecessary.


- © SCEI

- 14-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 1.1.2. Local Memory


The chip contains 4 MB of DRAM as local memory to save pixel information (frame buffer and Z buffer)
generated by the drawing function and texture information.


## Unified configuration


Contains the frame buffer, Z buffer, textures and CLUTs in the same memory space.


## 2 port configuration


In this configuration, the frame + Z buffer port is independent of the texture port.
buffer of 8KB, and are concurrently accessible.


They each have a page


## Wide bandwidth


Bandwidth of frame buffer: 38.4 GBytes/sec (1024 bits x 150 MHz x 2)
Texture bandwidth: 9.6 GBytes/sec (512 bits x 150 MHz)
Transmission from DRAM to page buffer: 8192 bits @ 1cycle at maximum


High-speed transmission between buffers
Host -> Local, Local -> Local and Local -> Host transmissions
Maximum transmission speed: 1.2 GB/sec


## 1.1.3. Video I/O


This function reads image information from memory in the scanning order of the television signal, performs
D/A conversion and outputs the video signal.


## Various output formats


NTSC/PAL/VESA
Interlace/non-interlace can be switched.


- 15-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## Block Configuration


## Whole Block Diagram


GS configuration


Drawing Function
Block


Local/Host


Host/Local


Host Interface


Vertex Info/Drawing Environment


Setup/Rasterizing


XYZ/RGBA/STQ etc.


Pixel Pipeline x 16 (Max)


#0 #1 #2 #3 #4


#14 #15


Texel x 16


RGBA/Z


Memory Interface


RGBA + Z
(1024 bits x 2)


Texture


Display Function
Block


Memory Block


(512 bits)


PCRTC


Video Out


Frame Page Buffer


Texture Page Buffer


Local Memory


(4 MB)


Figure 1-1 GS Whole Block Diagram


The functional blocks in the figure perform as follows:


- © SCEI

- 16-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## Host Interface


This interface transfers data with the host (CPU). Drawing data and buffer transfer data from the host pass
through this interface.


## Setup/Rasterizing (preprocessing)


This block develops the graphics to draw to the pixels based on vertex information received from the host,
and calculates information such as RGBA value, Z value, texture value, and fog value for each pixel.


## Pixel Pipeline


This block performs processes such as texture mapping, Fogging, and Alpha-blending, and determines the
final drawing color based on pixel information calculated in the Rasterizing block. It can process a
maximum of 16 pixels concurrently.


## Memory Interface


This block reads data from and writes data to the GS local memory. It writes to memory the drawing pixel
values (RGBA, Z) at the end of a pixel operation, reads from memory the pixel values of the frame buffer
(used for the pixel test or Alpha-blending), and reads from memory the RGBA values for the display image.


## Local Memory


The GS has a 32-Mbit built-in local memory, containing the frame buffer, Z buffer, texture and CLUT. This
memory has a 1024-bit read port and a 1024-bit write port for drawing and accessing the frame buffer and Z
buffer and a 512-bit port for texture reading.


## PCRTC


PCRTC displays the contents of the frame memory in the specified output format.


- 17-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 1.3. Maximum Performance


## Maximum pixel writing rate


The GS processes 16 pixels per cycle when not performing texture mapping and 8 pixels per cycle when
performing texture mapping. The maximum achievable pixel writing rates are:


<table>
 <tr>
  <th>Texture</th>
  <td>Max Pixel Rate</td>
 </tr>
 <tr>
  <th>Without Texture Mapping</th>
  <td>2.4 Gpixel/sec</td>
 </tr>
 <tr>
  <th>With Texture Mapping</th>
  <td>1.2 Gpixel/sec</td>
 </tr>
</table>


Mapping
(Each case has 32-bit pixels, Z-buffering


ON and Alpha-Blending ON.)


Maximum setup performance
Setup performance means how many polygons the preprocessor can process per second; the upper limit
occurs when pixel writing does not create a processing bottleneck. It changes depending on the kind of
primitives to be drawn and the drawing conditions, as shown below.


Processing Performance in Triangle Drawing
cycle
(triangle/sec)


<table>
 <tr>
  <th>Texture</th>
  <th>Shading</th>
  <th>Fogging</th>
  <th>Anti-aliasing</th>
  <th colspan="2">Necessary Performance<br/>count</th>
 </tr>
 <tr>
  <td>OFF</td>
  <td>Flat</td>
  <td>ON/OFF</td>
  <td>OFF</td>
  <td>2</td>
  <td>75M</td>
 </tr>
 <tr>
  <td>ON</td>
  <td>Flat/Gouraud</td>
  <td>OFF</td>
  <td>OFF</td>
  <td>4</td>
  <td>37.5M</td>
 </tr>
 <tr>
  <td>ON</td>
  <td>Flat/Gouraud</td>
  <td>ON</td>
  <td>OFF</td>
  <td>5</td>
  <td>30M</td>
 </tr>
 <tr>
  <td>ON/OFF</td>
  <td>Gouraud</td>
  <td>OFF</td>
  <td>OFF</td>
  <td>4</td>
  <td>37.5M</td>
 </tr>
 <tr>
  <td>ON/OFF</td>
  <td>Gouraud</td>
  <td>ON</td>
  <td>OFF</td>
  <td>5</td>
  <td>30M</td>
 </tr>
 <tr>
  <td>OFF</td>
  <td>Flat</td>
  <td>ON/OFF</td>
  <td>ON</td>
  <td>6</td>
  <td>25M</td>
 </tr>
 <tr>
  <td>ON</td>
  <td>Flat/Gouraud</td>
  <td>OFF</td>
  <td>ON</td>
  <td>8</td>
  <td>18M</td>
 </tr>
 <tr>
  <td>ON</td>
  <td>Flat/Gouraud</td>
  <td>ON</td>
  <td>ON</td>
  <td>9</td>
  <td>16M</td>
 </tr>
 <tr>
  <td>ON/OFF</td>
  <td>Gouraud</td>
  <td>OFF</td>
  <td>ON</td>
  <td>8</td>
  <td>18M</td>
 </tr>
 <tr>
  <td>ON/OFF</td>
  <td>Gouraud</td>
  <td>ON</td>
  <td>ON</td>
  <td>9</td>
  <td>16M</td>
 </tr>
</table>


Processing


<table>
 <tr>
  <th>Texture Shading Fogging Anti-aliasing Necessary Performance<br/>count</th>
 </tr>
 <tr>
  <td>ON Flat/Gouraud OFF OFF 4 37.5M</td>
 </tr>
 <tr>
  <td>ON Flat/Gouraud ON OFF 5 30M</td>
 </tr>
 <tr>
  <td>OFF Flat ON/OFF ON 6 25M</td>
 </tr>
 <tr>
  <td>ON Flat/Gouraud OFF ON 7 21M</td>
 </tr>
 <tr>
  <td rowspan="2">Processing Performance in Sprite<br/>Texture Shading Fogging Anti-aliasing Necessary (sprite/sec) Performance<br/>cycle count<br/>ON/OFF Flat/Gouraud ON/OFF OFF 3 50M</td>
 </tr>
 <tr>
 </tr>
</table>


in


Drawing


Processing


in


Texture


Necessary


Performance


cycle count


(point/sec)


ON/OFF


Flat/Gouraud


ON/OFF


OFF


1


150M


Performance
Line
cycle
(line/sec)


Performance
Point
Shading Fogging Anti-aliasing


When a difference in Z coordinates between the vertices composing one drawing primitive exceeds 216, a penalty
occurs and the cycle count required increases.


- © SCEI

- 18-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 2. Local Memory


- 19-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 2.1. Data Stored in Local Memory


The GS uses four kinds of data in local memory for drawing. The user must secure data areas (buffers) in local
memory before starting drawing.


## Frame buffer


Area for drawing. Stores pixels (RGBA) of the drawing result.


## Z buffer


Area for drawing. Stores Z value of the drawing result.


## Texture buffer


Stores texture image data.


## CLUT buffer


Stores the Color Look up Table (CLUT) used when the texture is an index color.


These buffers can be freely arranged in local memory.


- © SCEI

- 20-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 2.2. Addressing


## 2.2.1. Address Value


Local memory addressing uses linear addresses in 32-bit word units.
32 bits


(0x00000)


Frame


Z


Texture


Texture


CLUT


FBP: Frame Buffer Starting Address


Address


ZBP: Z Buffer Starting Address


(0xfffff)


TBP: Texture Buffer Starting Address


CBP: CLUT Buffer Starting Address


Figure 2-1 Local Memory of GS


## 2.2.2. Starting Address of Buffer


The different buffer types require different starting address alignments, as follows:


<table>
 <tr>
  <th>Buffer Type</th>
  <td>Alignment Size</td>
 </tr>
 <tr>
  <th>Frame buffer</th>
  <td>2K words</td>
 </tr>
 <tr>
  <th>Z buffer</th>
  <td>2K words</td>
 </tr>
 <tr>
  <th>Texture buffer</th>
  <td>64 words</td>
 </tr>
 <tr>
  <th>CLUT buffer</th>
  <td>64 words</td>
 </tr>
</table>


32-bit address, but in units of the address divided by the


buffer is specified,


The starting address of a
above alignment size.


not as a


## 2.2.3. Address in Buffer


Two-dimensional coordinates are used to specify the data position in each buffer. Given the starting address
and the buffer width (maximum X), the XY coordinate values in the buffer are converted to one-dimensional
real memory addresses. When converting from two-dimensional coordinates to a memory address, the GS
performs special processing to improve drawing efficiency. (For details of the conversion, see "8. Details of GS
Local Memory".)


- 21-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 2.3. Data Formats


The formats for data stored in the buffer (pixels, Z values, etc.) are shown below.


## 2.3.1. Color Data Format (Pixel/Texel Format)


The GS supports the following five color data formats.


## RGBA32


<table>
 <tr>
  <th>31</th>
  <td>24</td>
  <td>23</td>
  <td>16</td>
  <td>15</td>
  <td>8 7 0</td>
 </tr>
 <tr>
  <th>A (8</th>
  <td>bits)</td>
  <td>B (8</td>
  <td></td>
  <td>G (8</td>
  <td>R (8 bits)</td>
 </tr>
 <tr>
  <th>23</th>
  <td>16</td>
  <td>15</td>
  <td>8</td>
  <td>7</td>
  <td rowspan="3">0</td>
 </tr>
 <tr>
  <th>B (8</th>
  <td>bits)</td>
  <td>G (8</td>
  <td></td>
  <td>R (8</td>
 </tr>
 <tr>
  <th></th>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th>15 14<br/>A B (5</th>
  <td>10 9<br/>bits) G</td>
  <td>5 4<br/>(5 bits)</td>
  <td>0<br/>(5 bits)</td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th>7<br/>index</th>
  <td>0<br/>bits)</td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
</table>


## RGB24


## RGBA16


## IDTEX8


## IDTEX4


3 0
index


(4 bits)


Color formats IDTEX8 and IDTEX4 can be used only in the texture buffer. They become index values for
the Color Look up Table (CLUT). For details of the interpretation of the index value, see "2.7. CLUT
Buffer".


## 2.3.2. Z Value Format


Data in the following three formats can be stored in the Z buffer.


## Z32


31


Z (32 bits)


0


Z24


23
Z (24 bits)


0


Z16


15 0
Z (16 bits)


- © SCEI

- 22-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 2.4. Frame Buffer


The frame buffer is the area where image data of drawing results are stored. Settings related to the frame buffer
are made in the FRAME register (FRAME_1 or FRAME_2).


## 2.4.1. Size of Frame Buffer


The frame buffer width must be a multiple of 64 pixels. This value is specified in the FBW field of the FRAME
register.
The frame buffer height is arbitrary, and not specially set by the register. The height can be limited by limiting
the rectangular area where drawing is performed with the Scissoring function (described later in this document).


## 2.4.2. Starting Address of Frame Buffer


The starting address of the frame buffer in local memory is specified in the FBP field of the FRAME register.
Since the alignment of the frame buffer is 2K words, the FBP field is set as the address divided by 2048.


## 2.4.3. Coordinate Systems


There are two kinds of coordinate systems that point to the pixels in the frame buffer.


## Primitive Coordinate System


This is the coordinate system of the drawing space. It is used to set vertex coordinate values during the
drawing process. Both X and Y coordinates are fixed 16-bit decimal values (12-bit integer and 4-bit decimal)
in the range of 0 to 4095.9375. The rectangular area in the frame buffer where drawing is actually performed
is defined in this space.


## Window Coordinate System


This coordinate system uses the upper left point of the rectangular area of the frame buffer as the origin.
The calculation of memory addresses is based on the coordinate values.
Mapping to the Primitive coordinate system is done by using the offset value. The offset is stored in the
XYOFFSET register.
Assuming that the Primitive coordinate values are (Px,Py) and the Offset values are (Offx,Offy), the Window
coordinate values (Wx,Wy) are obtained from the following formulas:
Wx = Px - Offx
Wy = Py - Offy
The following figure shows the relation between the Primitive Coordinate System and the Window
Coordinate System:


- 23-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


(0,0)


Origin Point
in Primitive
Coordinate System


Origin Point
in Window
Coordinate System


Px,Py


Offset


(0,0)


Wx,Wy


4095.9375


x


x


4095.9375
y


y


Figure 2-2 Coordinate Systems in Frame Buffer


## 2.4.4. Pixel Correspondence


The pixels in the frame buffer are centered on positions where the fractional parts of the Window coordinate
values are 0 (the intersection of the grid in the figure).


Wx 1.0 2.0 3.0


Wy Frame
Buffer


1. 


Center of Pixel


1. 

2. 


Figure 2-3 Frame Buffer Coordinates and Pixel Position


- © SCEI

- 24-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 2.4.5. Pixel Storage Format


The pixel storage format defines how the pixels are arranged in each 32-bit word of local memory. The eight
formats are shown below:


## PSMCT32


31


<table>
 <tr>
  <th></th>
  <td></td>
 </tr>
 <tr>
  <th>31 24 23</th>
  <td>0</td>
 </tr>
 <tr>
  <th>Not used</th>
  <td>RGB24</td>
 </tr>
 <tr>
  <th>PSMCT16S</th>
  <td></td>
 </tr>
 <tr>
  <th>31 16</th>
  <td>15 0</td>
 </tr>
 <tr>
  <th>RGBA16 (Upper)</th>
  <td>RGBA16 (Lower)</td>
 </tr>
 <tr>
  <th>31</th>
  <td>0</td>
 </tr>
 <tr>
  <th></th>
  <td></td>
 </tr>
 <tr>
  <th>31 24 23</th>
  <td>0</td>
 </tr>
 <tr>
  <th>Not used</th>
  <td>Z24</td>
 </tr>
 <tr>
  <th>PSMZ16S</th>
  <td></td>
 </tr>
 <tr>
  <th>31 16</th>
  <td>15 0</td>
 </tr>
 <tr>
  <th>Z16</th>
  <td>Z16</td>
 </tr>
</table>


0


PSMCT24


PSMCT16,


## PSMZ32


## PSMZ24


## PSMZ16,


The pixel storage format used is specified in the PSM field (Pixel Storage Mode) of the FRAME register.


- 25-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 2.5. Z Buffer


The Z buffer is an area where the Z values of the pixels are stored as a result of drawing. The size (width and
height) of the Z buffer is the same as that of the frame buffer.


## 2.5.1. Starting Address of Z Buffer


The starting address of the Z buffer in local memory is specified in the ZBP field of the ZBUF register. The
alignment of the Z buffer is 2048 words, so the ZBP field is set to the address divided by 2048.


## 2.5.2. Coordinate System of Z Buffer


The coordinate system of the Z buffer is the same as that of the frame buffer.


## 2.5.3. Z Value Storage Format


The data storage format of the Z buffer is as follows:


## PSMZ32


31
Z32


<table>
 <tr>
  <th></th>
  <th></th>
 </tr>
 <tr>
  <td>31 24 23</td>
  <td>0</td>
 </tr>
 <tr>
  <td>Not used</td>
  <td>Z24</td>
 </tr>
 <tr>
  <td>PSMZ16S</td>
  <td></td>
 </tr>
 <tr>
  <td>31 16</td>
  <td>15 0</td>
 </tr>
 <tr>
  <td>Z16</td>
  <td>Z16</td>
 </tr>
</table>


0


## PSMZ24


## PSMZ16,


## 2.5.4. Combinations of Frame Buffer and Z Buffer Formats


To use the pixel storage format for the frame buffer and the Z value storage format for the Z buffer in
combination, both the formats must belong to the same group (Group1 or 2 below).


<table>
 <tr>
  <th>1</th>
  <td></td>
  <td>2</td>
 </tr>
 <tr>
  <th>PSMCT32<br/>PSMCT24<br/>PSMCT16S<br/>PSMZ32<br/>PSMZ24<br/>PSMZ16S</th>
  <td></td>
  <td>PSMCT16<br/>PSMZ16</td>
 </tr>
</table>


The following are some examples of applicable and inapplicable combinations.


<table>
 <tr>
  <th>Pixel Format for</th>
  <th>Z Value Format for Z</th>
  <th>Combination</th>
 </tr>
 <tr>
  <td>Frame Buffer</td>
  <td>Buffer</td>
  <td></td>
 </tr>
 <tr>
  <td>PSMCT32</td>
  <td>PSMZ24</td>
  <td>Yes</td>
 </tr>
 <tr>
  <td>PSMCT32</td>
  <td>PSMZ16</td>
  <td>No</td>
 </tr>
 <tr>
  <td>PSMCT32</td>
  <td>PSMZ16S</td>
  <td>Yes</td>
 </tr>
 <tr>
  <td>PSMCT16</td>
  <td>PSMZ24</td>
  <td>No</td>
 </tr>
 <tr>
  <td>PSMCT16S</td>
  <td>PSMZ24</td>
  <td>Yes</td>
 </tr>
 <tr>
  <td>PSMCT16</td>
  <td>PSMZ16</td>
  <td>Yes</td>
 </tr>
</table>


- © SCEI

- 26-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 2.6. Texture Buffer


The texture buffer stores texture image data for drawing textured polygons. The TEX0 register stores settings
related to the texture buffer.
The height and width of textures in the buffer are specifiable separately, and independently of the width of the
buffer. They must be a power of 2, with a maximum size of 1024 texels.
The width of the buffer is a multiple of 64 texels, but it must be a multiple of 128 texels when PSMT8 or
PSMT4 is specified as the buffer storage format.


## 2.6.1. Starting Address of Texture Buffer


The starting address of the texture buffer in local memory is called the texture base pointer. It is specified in the
TBP0 field in the TEX0 register, in units of 64 words, that is, the starting address divided by 64.
When MIPMAP is performed, it is necessary to set the starting addresses and buffer widths for the textures of
two or more MIP levels. Texture settings of Level 0 (the maximum resolution) are stored in the TEX0 register
(as well as the case where MIPMAP is not used). Texture settings of Level 1 to 6 are specified in the MIPTBP1
and MIPBP2 registers.


## 2.6.2. Coordinate System of Texture Buffer


Coordinates in the texture buffer are specified as either texture coordinates (STQ) or texel coordinates (UV).
When performing perspective correction, texture coordinates must be used.


## Texture coordinates


The GS registers use texture coordinates specified in a two-dimensional homogeneous coordinate system (S,
T, Q) to achieve perspective correction.
The coordinate values stored in the texture buffer are indicated by the normalized texture coordinates (s, t)
showing the upper left point as (0.0, 0.0) and the lower right point as (1.0, 1.0) in a single-precision floating-
point representation.


1.0 s


0.0
0.0


A 1.0


1.0 1.0
t


Figure 2-4 Normalized Texture Coordinates


The texture coordinates (S, T, Q) use the results of the conversion of (s, t). For the calculation, see "3.4.10.
Perspective Correction".


- 27-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


Texel coordinates
Texel coordinates are two-dimensional coordinate values indicated by (U, V). The UV value is a fixed 16-bit
decimal value, where 1 texel is 1.0. The upper 12 bits are the unsigned integer part, and the lower 4 bits are
the fractional part.


16.0 U


0.0


0.0


A 16.0


16.0 16.0


V


Figure 2-5 Texture Coordinates (in case Texture Size


is 16x16)


Texels are centered on the position where the fractional parts of the texel coordinate values are 0.5 (the
center of the square in the grid in the figure).


U 1.0 2.0 3.0


V


1. 

2. 

3. 


Center of Texel


Figure 2-6 Texel Coordinate System and Center of Texel


- © SCEI

- 28-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 2.6.3. Texel Storage Format


There are 8 forms of Texel Storage Format for the texture buffer as shown below:


## PSMCT32


31


<table>
 <tr>
  <th>RGBA32</th>
 </tr>
 <tr>
  <td>31 24 23 0</td>
 </tr>
 <tr>
  <td>Not used RGB24</td>
 </tr>
 <tr>
  <td>31 16 15 0</td>
 </tr>
 <tr>
  <td>RGBA16 RGBA16</td>
 </tr>
 <tr>
  <td>31 24 23 16 15 8 7 0</td>
 </tr>
 <tr>
  <td>IDTEX8 IDTEX8 IDTEX8 IDTEX8</td>
 </tr>
 <tr>
  <td>31 24 23 0</td>
 </tr>
 <tr>
  <td>IDTEX8 Not used</td>
 </tr>
 <tr>
  <td>31 24 23 16 15 8 7 0</td>
 </tr>
 <tr>
  <td>IDTEX4 IDTEX4 IDTEX4 IDTEX4 IDTEX4 IDTEX4 IDTEX4 IDTEX4</td>
 </tr>
 <tr>
  <td>31 28 27 0</td>
 </tr>
 <tr>
  <td>IDTEX4 Not used</td>
 </tr>
 <tr>
  <td>31 28 27 24 23 0</td>
 </tr>
 <tr>
  <td>Not used IDTEX4 Not used</td>
 </tr>
</table>


0


## PSMCT24


## PSMCT16


## PSMT8


## PSMT8H


## PSMT4


## PSMT4HH


## PSMT4HL


there is no influence on the "not used" bits, it is possible to combine textures of two different modes (e.g.


Since


PSMCT24 and PSMT8H) in the same area.
The PSM field of the TEX0 register specifies which texel storage format is used.


- 29-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 2.7. CLUT Buffer


A CLUT (Color Look up Table) converts a texel value from an index to RGBA color data, when the color
format of the texel is IDTEX8 or IDTEX4. The CLUT buffer is the area where the CLUT is stored. Settings
related to the CLUT are stored in the TEX0 register or the TEX2 register.
The TEX2 register is a subset of the TEX0 register. Among the values set by the TEX0 register, it is possible to
change only CLUT-related information by changing the values in the TEX2 register.


## 2.7.1. CLUT Configuration


A CLUT is an array of two or more elements (CLUT entries) that describe color information, arranged in a
prescribed order. The number of CLUT entries depends on the texel color format; it is 16 for IDTEX4 and 256
for IDTEX8. The CLUT data format in local memory is the same as for pixels. For instance, CLUT entries
arranged as 16x16 in width and height are the same as 16x16 image data.


## 2.7.2. Starting Address


The starting address of the CLUT buffer in local memory is called the CLUT base pointer, and is specified in the
CBP field of the TEX0 or TEX2 register. The CLUT base pointer is in units of 64 words; that is, the value of
the address divided by 64 is stored in the CBP field.


## 2.7.3. CLUT Storage Mode


The CLUT buffer is a two-dimensional space like the other buffers. The CLUT storage mode determines how
CLUT entries are arranged in the two-dimensional space of the CLUT buffer. There are two CLUT storage
modes, CSM1 and CSM2. They are specified in the CSM field of the TEX0 register.


## CSM1


Data can be more efficiently read from local memory in CSM1 than in CSM2.
The CLUT entries are arranged in order in 16x16 rectangles in IDTEX8 (8 bits) and 8x2 in IDTEX4 (4 bits)
as shown below. The numbers in the figures show the corresponding index values (in hexadecimal).


Arrangement of CLUT in IDTEX4
X
Y
123
57


<table>
 <tr>
  <th>0</th>
  <td></td>
  <td></td>
  <td></td>
  <td>4</td>
  <td></td>
  <td>6</td>
  <td></td>
 </tr>
 <tr>
  <th>8</th>
  <td>9</td>
  <td>a</td>
  <td>b</td>
  <td>c</td>
  <td>d</td>
  <td>e</td>
  <td>f</td>
 </tr>
</table>


Arrangement of CLUT in IDTEX8
X


<table>
 <tr>
  <th rowspan="6">Y</th>
  <td>00</td>
  <td>01</td>
  <td>02</td>
  <td>03</td>
  <td>04</td>
  <td>05</td>
  <td>06</td>
  <td>07</td>
  <td>10</td>
  <td>11</td>
  <td>12</td>
  <td>13</td>
  <td>14</td>
  <td>15</td>
  <td>16</td>
  <td>17</td>
 </tr>
 <tr>
  <td>08</td>
  <td>09</td>
  <td>0a</td>
  <td>0b</td>
  <td>0c</td>
  <td>0d</td>
  <td>0e</td>
  <td>0f</td>
  <td>18</td>
  <td>19</td>
  <td>1a</td>
  <td>1b</td>
  <td>1c</td>
  <td>1d</td>
  <td>1e</td>
  <td>1f</td>
 </tr>
 <tr>
  <td>20</td>
  <td>21</td>
  <td>22</td>
  <td>23</td>
  <td>24</td>
  <td>25</td>
  <td>26</td>
  <td>27</td>
  <td>30</td>
  <td>31</td>
  <td>32</td>
  <td>33</td>
  <td>34</td>
  <td>35</td>
  <td>36</td>
  <td>37</td>
 </tr>
 <tr>
  <td>28</td>
  <td>29</td>
  <td>2a</td>
  <td>2b</td>
  <td>2c</td>
  <td>2d</td>
  <td>2e</td>
  <td>2f</td>
  <td>38</td>
  <td>39</td>
  <td>3a</td>
  <td>3b</td>
  <td>3c</td>
  <td>3d</td>
  <td>3e</td>
  <td>3f</td>
 </tr>
 <tr>
  <td>e0</td>
  <td>e1</td>
  <td>e2</td>
  <td>e3</td>
  <td>e4</td>
  <td>e5</td>
  <td>e6</td>
  <td>e7</td>
  <td>f0</td>
  <td>f1</td>
  <td>f2</td>
  <td>f3</td>
  <td>f4</td>
  <td>f5</td>
  <td>f6</td>
  <td>f7</td>
 </tr>
 <tr>
  <td>e8</td>
  <td>e9</td>
  <td>ea</td>
  <td>eb</td>
  <td>ec</td>
  <td>ed</td>
  <td>ee</td>
  <td>ef</td>
  <td>f8</td>
  <td>f9</td>
  <td>fa</td>
  <td>fb</td>
  <td>fc</td>
  <td>fd</td>
  <td>fe</td>
  <td>ff</td>
 </tr>
</table>


In CSM1, arrangement should be made from the start of the buffer (the position where X and Y are each 0).


- © SCEI

- 30-




SCE CONFIDENTIAL GS User's Manual Version 6.0


0 123 4 5 6 7 8 9 b c d e af


Y


## 2.7.4. CLUT Storage Format


The storage formats of individual CLUT entries are the same as those of the pixels in the frame buffer. When
the CLUT storage mode is CSM1, PSMCT32, PSMCT16 or PSMCT16S can be specified. When it is CSM2,
only PSMCT16 can be specified. They are specified in the CPSM field of the TEX0 register.


- 31-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


(This page is left blank intentionally)


- © SCEI

- 32-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 3. Drawing Function


- 33-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 3.1. Outline of Drawing Function


The GS receives the information necessary for drawing (vertex and drawing environment information) from the
host processor via the Host interface. It draws drawing primitives called GS primitives as processing units.
The drawing flow in the GS is shown below.


Host Interface


Drawing Environment


Vertex Information


<table>
 <tr>
  <th>Drawing</th>
  <th></th>
  <th></th>
  <th>Setup (preprocessing)</th>
 </tr>
 <tr>
  <td>Register</td>
  <td></td>
  <td></td>
  <td>DDA Set Value (Initial Value /</td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
  <td>Rasterizing (DDA)<br/>Scissoring</td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td>XY</td>
  <td>Z F RGBA STQ/UV</td>
 </tr>
 <tr>
  <td>To Each Process</td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
</table>


<table>
 <tr>
  <th colspan="3">Texture Mapping<br/>Texture Function</th>
 </tr>
 <tr>
  <td></td>
  <td>RGB</td>
  <td>A</td>
 </tr>
 <tr>
  <td colspan="2"></td>
  <td></td>
 </tr>
 <tr>
  <td></td>
  <td colspan="2">RGB A</td>
 </tr>
</table>


Texture
CLUT


A, Z


RGB


Gradient)


Coverage XYZRGBA....


Fogging Antialiasing


<table>
 <tr>
  <th>Pixel Test<br/>Alpha Test<br/>Destination Alpha Test<br/>Depth Test</th>
 </tr>
 <tr>
  <th>Alpha Blending</th>
 </tr>
 <tr>
  <th>Formatting</th>
 </tr>
 <tr>
  <th>XY Z RGB A</th>
 </tr>
</table>


Memory Interface


Figure 3-2 Block Diagram for Drawing Processing


- © SCEI

- 34-




SCE CONFIDENTIAL GS User's Manual Version 6.0


The processes shown in the figure are:


Setup (Preprocessing)


The gradient (e.g. shading coefficient) and the initial value of DDA necessary for drawing primitives are
calculated based on the vertex information received from the host.
If the primitive is a Triangle, the gradient of the RGBA value, Z value, texture value, and Fog value on the
three sides and on the scan line are calculated.


## Rasterizing (DDA)


The pixels of a primitive are generated by DDA (Digital Differential Algorithm). 8 or 16 pixels are generated
concurrently. The RGBA value, Z value, texture value and Fog value for each pixel are calculated from the
gradient obtained in the preprocessing stage, and are given to each of the parallel pixel pipelines.


## Texture Mapping


Textures are mapped to pixels. The pixel color is determined by applying the texture function to the Texture
CLUT RGBA value read from the memory block and the RGBA value calculated by DDA.


## Antialiasing


The alpha value is replaced with the pixel coverage calculated by DDA (proportion of the pixel occupied by
the theoretical edge of the primitive.) The edges of the primitive are smoothed when Alpha-blending is
implemented with this alpha value.


## Fogging


The RGB value and Fog color value output from the texture mapping block are blended according to the
Fog value of the pixel calculated by DDA.


Pixel Test


pixel
tests-alpha
alpha
depth
performed
by


Alpha-blending


Blending the RGB value of a pixel and the RGB value in the frame memory is implemented according to the
alpha value of the pixel or the alpha value in the frame memory.


## Formatting


The pixel value for drawing is converted into the data format of the frame buffer. Dithering and color
clamping are applied if necessary.


## Memory Interface


Read/write is performed to local memory in the chip. The operations are: writing drawing pixel values
(RGBA, Z) to the memory after a pixel operation, reading pixel values into the frame buffer from the
memory (used for pixel test and alpha-blending), and reading RGBA values for display from memory.


## Environment Registers


These registers store various parameters necessary for drawing. These values are referred to in each process
of drawing.


- 35-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 3.2. GS Primitive


## 3.2.1. Types of GS Primitives


The GS uses the following seven primitive types:
Point
A Point is an independent point that is drawn with 1 piece of vertex information.


3


1


2


## Line


A Line is an independent line that is drawn with 2 pieces of vertex information.


1


3


2


4


## LineStrip


A LineStrip is a series of continuous lines that share endpoints. The first line is drawn with 2 pieces of
vertex information. The succeeding lines are drawn with 1 piece of vertex information.


3


1


5


2


4


## Triangle


A Triangle is an independent triangle that is drawn with 3 pieces of vertex information.


1


2


3


4


5


6


- © SCEI

- 36-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## TriangleStrip


A TriangleStrip is a series of triangles that share sides. The first triangle is drawn with 3 pieces of vertex
information, and the succeeding ones are drawn whenever 1 piece of vertex information is added.


1


3


5


7


2


6


4


## TriangleFan


A TriangleFan is a series of triangles sharing one vertex. The first triangle is drawn with 3 pieces of vertex
information, and the succeeding triangles are drawn whenever 1 piece of vertex information is added.


2


3


4


5


6


7


1


## Sprite


A Sprite is an independent rectangle, drawn with 2 pieces of vertex information showing the endpoints of a
diagonal line.


1


3


4


2


## 3.2.2. Drawing Attributes


Besides their types, GS primitives have the following drawing attributes:


<table>
 <tr>
  <th></th>
  <td>Contents</td>
  <td></td>
 </tr>
 <tr>
  <th>Attribute</th>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th>IIP</th>
  <td>Method</td>
  <td>Flat/Gouraud</td>
 </tr>
 <tr>
  <th>TME</th>
  <td>Texture</td>
  <td>ON/OFF</td>
 </tr>
 <tr>
  <th>FGE</th>
  <td></td>
  <td>ON/OFF</td>
 </tr>
 <tr>
  <th>ABE</th>
  <td></td>
  <td>ON/OFF</td>
 </tr>
 <tr>
  <th>AA1</th>
  <td></td>
  <td>ON/OFF</td>
 </tr>
 <tr>
  <th>FST</th>
  <td>Texture Coordinates</td>
  <td>STQ/UV</td>
 </tr>
 <tr>
  <th>CTXT</th>
  <td>Context</td>
  <td>1/2</td>
 </tr>
 <tr>
  <th>FIX</th>
  <td>FIX</td>
  <td>ON/OFF</td>
 </tr>
</table>


- 37-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


Some drawing attributes are fixed for specific GS primitive types:
Point: Shading Method flat and Antialiasing Off
Sprite: Shading Method flat and Antialiasing Off


## 3.2.3. Outline of Drawing Procedure


The general drawing procedure is:


1. Set GS primitive type/drawing attribute (PRIM register)

2. Set vertex information (Vertex info setting registers)

3. Vertex Kick (XYZF2, XYZF3 registers, etc.)

4. Start drawing (XYZF2 register etc.)


1) Set GS Primitive Type and Drawing Attribute
The GS primitive type is specified by writing to the PRIM register. The condition of the vertex queue is also
initialized. The drawing attributes of the GS primitive can be set, and it is also possible to set GS primitive
drawing attributes in the PRMODE register. (See "3.2.6. Change of Drawing Attributes".)


2) Set Vertex Information
Vertex information includes the drawing coordinates, vertex color, texture coordinates, and Fog coefficient.
These values are set in the vertex information setting registers shown below.


<table>
 <tr>
  <th>Item</th>
  <td></td>
  <td>Vertex</td>
  <td></td>
 </tr>
 <tr>
  <th></th>
  <td>Name</td>
  <td>Kick</td>
  <td>Kick</td>
 </tr>
 <tr>
  <th>Coordinate values</th>
  <td>XYZ2</td>
  <td>Yes</td>
  <td>Yes</td>
 </tr>
 <tr>
  <th>Coordinate values and coefficient</th>
  <td>XYZF2</td>
  <td>Yes</td>
  <td>Yes</td>
 </tr>
 <tr>
  <th>Coordinate values</th>
  <td>XYZ3</td>
  <td>Yes</td>
  <td>No</td>
 </tr>
 <tr>
  <th>Coordinate values and coefficient</th>
  <td>XYZF3</td>
  <td>Yes</td>
  <td>No</td>
 </tr>
 <tr>
  <th>Color info and value of texture coordinates</th>
  <td></td>
  <td>No</td>
  <td>No</td>
 </tr>
 <tr>
  <th>ST of texture coordinates</th>
  <td>ST</td>
  <td>No</td>
  <td>No</td>
 </tr>
 <tr>
  <th>UV of texel coordinates</th>
  <td>UV</td>
  <td>No</td>
  <td>No</td>
 </tr>
 <tr>
  <th>Fog coefficient</th>
  <td>FOG</td>
  <td>No</td>
  <td>No</td>
 </tr>
</table>


1. Vertex Kick If the vertex information setting registers that have a Vertex Kick function are written to, the vertex information set up to that point is placed in the vertex queue, and the queue goes one step forward. This operation is called the Vertex Kick. The following figure shows the image of operation.

- © SCEI

- 38-




SCE CONFIDENTIAL


GS User's Manual Version 6.0


Vertex Information Setting Resisters


XYZ RGBA STQ/UV F


Copy


Vertex Queue


1st Vertex
XYZ, RGBA, STQ/UV, F


Move


2nd Vertex
XYZ, RGBA, STQ/UV, F


Differs depending
on the primitive
type.


Move


3rd Vertex
XYZ, RGBA, STQ/UV, F


Figure 3-3 Operation Image of Vertex Kick


4) Start Drawing (Drawing Kick)
When the necessary vertex information is arranged in the vertex queue, drawing begins.


## 3.2.4. Example of the Drawing Procedure


The following examples show the order of register setting for the drawing procedures for the GS primitive
types. The examples show the drawing sequences for non-textured GS primitives, and the RGBA value and the
coordinate values are given to each vertex.


## Point Drawing Procedure


[Register Setting Order]
PRIM
RGBAQ Vertex1
XYZ2
RGBAQ Vertex2 Setting
XYZ2


Setting
Vertex Kick & Drawing Kick (Vertex1)


Vertex Kick & Drawing Kick (Vertex2)


:


## Line Drawing Procedure


[Register Setting Order]
PRIM
RGBAQ
XYZ2


<table>
 <tr>
  <th>XYZ2</th>
  <th>Vertex Kick</th>
 </tr>
 <tr>
  <td>RGBAQ</td>
  <td>Vertex2 Setting</td>
 </tr>
</table>


Vertex1 Setting


Vertex Kick & Drawing Kick (Vertex1 & Vertex2)


:


- 39-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## LineStrip Drawing Procedure


[Register Setting Order]


<table>
 <tr>
  <th>PRIM</th>
 </tr>
 <tr>
  <td>RGBAQ</td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td>RGBAQ</td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td></td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td>RGBAQ</td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td>:</td>
 </tr>
</table>


<table>
 <tr>
  <th>RGBAQ<br/>XYZ2</th>
  <td></td>
  <td>Vertex1 Setting<br/>Vertex Kick</td>
 </tr>
 <tr>
  <th>RGBAQ</th>
  <td></td>
  <td>Vertex2</td>
 </tr>
 <tr>
  <th>XYZ2</th>
  <td></td>
  <td>Vertex Kick &amp;</td>
 </tr>
 <tr>
  <th>RGBAQ</th>
  <td></td>
  <td>Vertex3</td>
 </tr>
 <tr>
  <th>XYZ2</th>
  <td></td>
  <td>Vertex Kick &amp;</td>
 </tr>
 <tr>
  <th>RGBAQ</th>
  <td></td>
  <td>Vertex4 Setting</td>
 </tr>
</table>


Drawing Kick(Vertex1 & Vertex2)


Drawing Kick (Vertex2 & Vertex3)


Vertex Kick & Drawing Kick (Vertex3 & Vertex4)


## Triangle Drawing Procedure


[Register Setting Order]
Vertex1
RGBAQ Vertex2


<table>
 <tr>
  <th>PRIM</th>
 </tr>
 <tr>
  <td>RGBAQ</td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td></td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td></td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td>RGBAQ</td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td>RGBAQ</td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td>RGBAQ</td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td>:</td>
 </tr>
</table>


<table>
 <tr>
  <th></th>
  <th></th>
 </tr>
</table>


Setting


Vertex Kick


Setting


Vertex Kick


RGBAQ Vertex3 Setting


Vertex Kick & Drawing Kick
(Vertex1 & Vertex2 & Vertex3)


Vertex4 Setting


Vertex Kick


Vertex5 Setting


Vertex Kick


- © SCEI

- 40-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## TriangleStrip Drawing Procedure


[Register Setting Order]
Setting
Setting


<table>
 <tr>
  <th>PRIM</th>
 </tr>
 <tr>
  <td>RGBAQ</td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td>RGBAQ</td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td></td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td>RGBAQ</td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td>RGBAQ</td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td>RGBAQ</td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td>:</td>
 </tr>
</table>


<table>
 <tr>
  <th colspan="2">Vertex1 Setting</th>
  <td></td>
 </tr>
 <tr>
  <th></th>
  <td colspan="2">Vertex Kick</td>
 </tr>
 <tr>
  <th colspan="2">Vertex2 Setting</th>
  <td></td>
 </tr>
 <tr>
  <th></th>
  <td colspan="2">Vertex Kick</td>
 </tr>
 <tr>
  <th colspan="2">Vertex3 Setting</th>
  <td></td>
 </tr>
 <tr>
  <th></th>
  <td colspan="2">Vertex Kick &amp; Drawing Kick<br/>(Vertex1 &amp; Vertex2 &amp; Vertex3)</td>
 </tr>
 <tr>
  <th colspan="2">Vertex4 Setting</th>
  <td></td>
 </tr>
 <tr>
  <th></th>
  <td colspan="2">Vertex Kick &amp; Drawing Kick<br/>(Vertex2 &amp; Vertex3 &amp; Vertex4)</td>
 </tr>
 <tr>
  <th colspan="2">Vertex5 Setting</th>
  <td></td>
 </tr>
 <tr>
  <th></th>
  <td colspan="2">Vertex Kick &amp; Drawing Kick<br/>(Vertex3 &amp; Vertex4 &amp; Vertex5)</td>
 </tr>
 <tr>
  <th colspan="2">Vertex6 Setting</th>
  <td></td>
 </tr>
 <tr>
  <th></th>
  <td colspan="2">(Vertex4 &amp; Vertex5 &amp; Vertex6)</td>
 </tr>
</table>


RGBAQ
Setting


## TriangleFan Drawing Procedure


[Register Setting Order]
Vertex1 Setting
RGBAQ Vertex2 Setting


<table>
 <tr>
  <th>PRIM</th>
 </tr>
 <tr>
  <td>RGBAQ</td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td></td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td></td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td>RGBAQ</td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td>RGBAQ</td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td>RGBAQ</td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td>:</td>
 </tr>
</table>


<table>
 <tr>
  <th>Vertex Kick &amp; Kick</th>
 </tr>
 <tr>
  <td>(Vertex1 &amp; Vertex2 &amp; Vertex3)</td>
 </tr>
 <tr>
  <td>Vertex Kick &amp; Drawing Kick</td>
 </tr>
 <tr>
  <td>(Vertex1 &amp; Vertex3 &amp; Vertex4)</td>
 </tr>
 <tr>
  <td>Vertex Kick &amp; Kick</td>
 </tr>
 <tr>
  <td>(Vertex1 &amp; Vertex4 &amp; Vertex5)</td>
 </tr>
 <tr>
  <td>Vertex Kick &amp; Kick</td>
 </tr>
 <tr>
  <td>(Vertex1 &amp; Vertex5 &amp; Vertex6)</td>
 </tr>
</table>


<table>
 <tr>
  <th>Vertex Kick</th>
 </tr>
 <tr>
  <td></td>
 </tr>
</table>


Vertex Kick


RGBAQ Vertex3 Setting


Vertex4 Setting


Vertex5 Setting


Vertex6 Setting


- 41-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## Sprite Drawing Procedure


[Register Setting Order]


<table>
 <tr>
  <th>PRIM</th>
  <td></td>
 </tr>
 <tr>
  <th>RGBAQ</th>
  <td>Vertex1 Setting</td>
 </tr>
 <tr>
  <th>XYZ2</th>
  <td>Vertex Kick</td>
 </tr>
 <tr>
  <th>RGBAQ</th>
  <td>Vertex2 Setting</td>
 </tr>
 <tr>
  <th>XYZ2</th>
  <td>Vertex Kick &amp;<br/>&amp;</td>
 </tr>
 <tr>
  <th>RGBAQ</th>
  <td>Vertex3 Setting</td>
 </tr>
 <tr>
  <th>XYZ2</th>
  <td>Vertex Kick</td>
 </tr>
 <tr>
  <th>RGBAQ</th>
  <td>Vertex4 Setting</td>
 </tr>
 <tr>
  <th rowspan="2">XYZ2<br/>:</th>
  <td></td>
 </tr>
 <tr>
  <td>(Vertex3 &amp;</td>
 </tr>
</table>


<table>
 <tr>
  <th>PRIM</th>
 </tr>
 <tr>
  <td>RGBAQ</td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td>RGBAQ</td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td></td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td>RGBAQ</td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td>:</td>
 </tr>
</table>


Drawing
Vertex2)


Kick


Kick


Drawing
Vertex4)


## 3.2.5. Drawing Control


When drawing a GS primitive consisting of two or more polygons, such as TriangleStrip, it is possible to control
a polygon that is being drawn without changing the vertex string order. As a result, operations like normal clips
become easy.
To control drawing in this way, the vertex information is written to the XYZ3 register instead of the XYZ2
register and to the XYZF3 register instead of the XYZF2 register. Because of this, the Drawing Kick is not
performed at the Vertex Kick, and only the vertex queue is advanced. For this reason, the polygon is not drawn.
The figure below is an example of such register settings. In this example, only the 2nd triangle is not drawn.


[Register Setting Order]
RGBAQ


<table>
 <tr>
  <th>PRIM</th>
 </tr>
 <tr>
  <td>RGBAQ</td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td></td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td></td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td>RGBAQ</td>
 </tr>
 <tr>
  <td>XYZ3</td>
 </tr>
 <tr>
  <td>RGBAQ</td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td>RGBAQ</td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td>:</td>
 </tr>
</table>


Vertex1 Setting


<table>
 <tr>
  <th>Vertex Kick</th>
 </tr>
 <tr>
  <td>Vertex Kick</td>
 </tr>
 <tr>
  <td>Vertex Kick &amp; Kick</td>
 </tr>
 <tr>
  <td>(Vertex1 &amp; Vertex2 &amp; Vertex3)</td>
 </tr>
 <tr>
  <td>Vertex Kick (No Drawing Kick)</td>
 </tr>
 <tr>
  <td>Vertex Kick &amp; Drawing Kick</td>
 </tr>
 <tr>
  <td>(Vertex3 &amp; Vertex4 &amp; Vertex5)</td>
 </tr>
 <tr>
  <td>Vertex Kick &amp; Drawing Kick</td>
 </tr>
 <tr>
  <td>(Vertex4 &amp; Vertex5 &amp; Vertex6)</td>
 </tr>
</table>


Vertex2 Setting


RGBAQ Vertex3 Setting


Vertex4 Setting


Vertex5 Setting


Vertex6 Setting


- © SCEI

- 42-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 3.2.6. Change of Drawing Attributes


The drawing attributes of a GS primitive are specified in the PRIM register. However, if the PRMODE register
is used, it is possible to change drawing attributes while drawing a primitive consisting of two or more polygons
such as TriangleStrip.
The following is an example of performing Flat shading to only one of the triangles in the process of drawing a
TriangleStrip, to which Gouraud shading is specified.


[Register Setting Order]
Set to AC=1
Specify types of primitive (The attribute is ignored)
Set to IIP=1(Gouraud)


<table>
 <tr>
  <th>PRMODECONT</th>
 </tr>
 <tr>
  <td>PRIM</td>
 </tr>
 <tr>
  <td>PRMODE</td>
 </tr>
 <tr>
  <td></td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td>RGBAQ</td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td></td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td>PRMODE</td>
 </tr>
 <tr>
  <td>RGBAQ</td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td>RGBAQ</td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td>RGBAQ</td>
 </tr>
 <tr>
  <td>XYZ2</td>
 </tr>
 <tr>
  <td>:</td>
 </tr>
</table>


RGBAQ Vertex1 Setting
Vertex2 Setting


<table>
 <tr>
  <th>Vertex Kick</th>
 </tr>
 <tr>
  <td>Vertex Kick</td>
 </tr>
 <tr>
  <td>Vertex Kick &amp; Kick</td>
 </tr>
 <tr>
  <td>(Vertex1 &amp; Vertex2 &amp; Vertex3)</td>
 </tr>
 <tr>
  <td>Vertex Kick &amp; Kick</td>
 </tr>
 <tr>
  <td>(Vertex2 &amp; Vertex3 &amp; Vertex4)</td>
 </tr>
 <tr>
  <td>Vertex Kick &amp; Kick</td>
 </tr>
 <tr>
  <td>(Vertex3 &amp; Vertex4 &amp; Vertex5)</td>
 </tr>
 <tr>
  <td>Vertex Kick &amp; Drawing Kick</td>
 </tr>
 <tr>
  <td>(Vertex4 &amp; Vertex5 &amp; Vertex6)</td>
 </tr>
</table>


RGBAQ Vertex3 Setting


Drawing
IIP=0(Flat)


Reset to
Vertex4 Setting


Vertex5 Setting


Vertex6 Setting


## 3.2.7. Register Setting Order and Drawing Order


Since the pixel pipeline, where drawing is performed, is designed to maintain the input register setting order,
dependence between register setting and drawing is in register setting order. This means register setting will not
override drawing. Moreover, the order of texture data transmission (Host/Local transmission) and drawing
operation will not be reversed. However, when using the data converted in Host/Local transmission or
Local/Local transmission as texture or CLUT for the first time, the texture buffer must be disabled with the
TEXFLUSH register.


## 3.2.8. Enabled Vertex Information


Note that there may be unusable vertex information in a register, depending on the primitive type and drawing
attribute.
For instance, a Sprite consists of two pieces of vertex information, but it only needs one piece of vertex
information for the Z coordinate value and Fog information, since depth is not provided. Therefore, the Z
coordinate and Fog coordinate of the first vertex are ignored, and the setting for the second one takes effect.
Also, when doing Flat shading, one piece of vertex color information is sufficient, so the vertex color
information set immediately before each drawing kick becomes effective. By this method, it is possible to
perform proper drawing by specifying Flat shading even in case of TriangleStrip.


- 43-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 3.2.9. Primitive Drawing Rule


The rules for how the DDA generates pixels are shown for each primitive.
Point
The pixel closest to the specified XY coordinates is drawn.


Closest Pixel


×


Specified XY coordinates


Figure 3-4 Pixel Drawing Rule for Point


Line, LineStrip
By defining the area for each pixel as shown with small gray squares in Figure 3-5, the pixels located in the
areas where a line goes through are drawn. However, a pixel located at the endpoint of a line is not drawn.
Any line can be drawn without being broken. Even when continued lines that share endpoints are drawn,
they cannot be drawn doubly unless crossed.


Start point
is drawn.


Drawn
only once


End point
is not drawn.


Figure 3-5 Pixel Drawing Rule for Line


Pixel Drawn


© SCEI


Pixel Drawn


Figure 3-6 Pixel Drawing Rule for LineStrip


- 44-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## Triangle, TriangleStrip, TriangleFan


Draws the pixels in the three sides specified by the three vertices. When a side passes the center of a pixel,
drawing is performed if it is the left side, and is not performed if it is the right side. When a side parallel to
the X axis passes the center of a pixel, drawing is performed if it is the top side, and is not performed if it is
the bottom side.
Even when drawing triangles that share sides with the same vertices, neither a gap nor a double line can be
created. (Figure 3-7)


Drawn on
the Left Side.


Not Drawn on
the Right Side.


Drawn on
the Top Side.


Not Drawn on
the Bottom Side.


Figure 3-7 Pixel Drawing Rule for Triangle


- 45-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


Drawn
only once.


Figure 3-8 Pixel Drawing Rule for Side-Sharing Triangles


## Sprite


Draws pixels in a rectangular area with two vertices specified as diagonal points. When a side passes the
center of a pixel, drawing is performed if it is the top or left side and not performed if it is the bottom or
right side.


1st Diagonal Point


Drawn on
the Left Side.


1st Diagonal Point


2nd Diagonal Point


Drawn on the Top Side.


Not Drawn on the Right Side.


<table>
 <tr>
  <th></th>
  <th></th>
  <th></th>
  <th></th>
  <th></th>
  <th></th>
  <th></th>
  <th></th>
  <th></th>
  <th></th>
  <th></th>
  <th></th>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
</table>


Not Drawn on the Bottom Side.


2nd Diagonal Point


Figure 3-9 Pixel Drawing Rule for Sprite


- © SCEI

- 46-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 3.3. Drawing Environment


There are various parameters used for GS primitive drawing, such as texture information, in addition to the
drawing attributes set by the PRIM register. These are called the drawing environment. Once the drawing
environment is set, it remains in effect for multiple GS primitives until it is reset. Settings can be changed even
in the process of drawing a GS primitive consisting of two or more polygons, such as TriangleStrip.


## 3.3.1. Drawing Environment Setting Registers


The following are the drawing environment setting registers.


<table>
 <tr>
  <th>Name</th>
  <th>Contents</th>
 </tr>
 <tr>
  <td>XYOFFSET_1/2</td>
  <td>Offset value of vertex coordinates</td>
 </tr>
 <tr>
  <td>PRMODECONT</td>
  <td>PRIM attributes enabled/disabled</td>
 </tr>
 <tr>
  <td>TEX0_1/2</td>
  <td>Attributes of texture buffer and texture mapping</td>
 </tr>
 <tr>
  <td>TEX1_1/2</td>
  <td>Attributes of texture mapping</td>
 </tr>
 <tr>
  <td>TEX2_1/2</td>
  <td>CLUT entry</td>
 </tr>
 <tr>
  <td>CLAMP_1/2</td>
  <td>Wrap mode of texture mapping</td>
 </tr>
 <tr>
  <td>TEXCLUT</td>
  <td>CLUT setting</td>
 </tr>
 <tr>
  <td>SCANMSK</td>
  <td>Drawing control with Y coordinate of pixel</td>
 </tr>
 <tr>
  <td>MIPTBP1_1/2</td>
  <td>Base pointer for MIPMAP on each level</td>
 </tr>
 <tr>
  <td>MIPTBP2_1/2</td>
  <td>Base pointer for MIPMAP on each level</td>
 </tr>
 <tr>
  <td>TEXA</td>
  <td>Reference value when expanding Alpha value of TEX16 and<br/>TEX24</td>
 </tr>
 <tr>
  <td>FOGCOL</td>
  <td>Fogging distant color</td>
 </tr>
 <tr>
  <td>SCISSOR_1/2</td>
  <td>Scissoring area</td>
 </tr>
 <tr>
  <td>ALPHA_1/2</td>
  <td>Alpha-blending attributes</td>
 </tr>
 <tr>
  <td>DIMX</td>
  <td>Dither matrix</td>
 </tr>
 <tr>
  <td>DTHE</td>
  <td>Dithering enabled/disabled</td>
 </tr>
 <tr>
  <td>COLCLAMP</td>
  <td>Color clamp/mask</td>
 </tr>
 <tr>
  <td>TEST_1/2</td>
  <td>Pixel operation</td>
 </tr>
 <tr>
  <td>PABE</td>
  <td>Alpha-blending in pixel units enabled/disabled</td>
 </tr>
 <tr>
  <td>FBA_1/2</td>
  <td>Alpha correction value</td>
 </tr>
 <tr>
  <td>FRAME_1/2</td>
  <td>Frame buffer setting</td>
 </tr>
 <tr>
  <td>ZBUF_1/2</td>
  <td>Z buffer setting</td>
 </tr>
</table>


## 3.3.2. Two Context Drawing Environment


Some of the drawing environment registers have two registers for the same function, such as XYOFFSET_1
and XYOFFSET_2. The selection of the first or second can be made by the primitive attribute CTXT. Even in
drawing command strings containing drawing commands from two kinds of contexts intermixed in GS primitive
units, drawing can be performed without repeating register save/load.


- 47-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


The registers with two contexts are listed below.
Register
mapping
mapping
entry
Wrap
mapping
pointer
pointer
Scissoring
Alpha-blending
operation
Alpha
setting
setting


<table>
 <tr>
  <th>Name</th>
  <th>Contents</th>
 </tr>
 <tr>
  <td>XYOFFSET_1/2</td>
  <td>Offset value of Vertex coordinates</td>
 </tr>
 <tr>
  <td>TEX0_1/2</td>
  <td>Attributes of texture buffer and texture</td>
 </tr>
 <tr>
  <td>TEX1_1/2</td>
  <td>Attributes of texture</td>
 </tr>
 <tr>
  <td>TEX2_1/2</td>
  <td>CLUT</td>
 </tr>
 <tr>
  <td>CLAMP_1/2</td>
  <td>mode of texture</td>
 </tr>
 <tr>
  <td>MIPTBP1_1/2</td>
  <td>Base for MIPMAP on each level</td>
 </tr>
 <tr>
  <td>MIPTBP2_1/2</td>
  <td>Base for MIPMAP on each level</td>
 </tr>
 <tr>
  <td>SCISSOR_1/2</td>
  <td>area</td>
 </tr>
 <tr>
  <td>ALPHA_1/2</td>
  <td>attributes</td>
 </tr>
 <tr>
  <td>TEST_1/2</td>
  <td>Pixel</td>
 </tr>
 <tr>
  <td>FBA_1/2</td>
  <td>correction value</td>
 </tr>
 <tr>
  <td>FRAME_1/2</td>
  <td>Frame buffer</td>
 </tr>
 <tr>
  <td>ZBUF_1/2</td>
  <td>Z buffer</td>
 </tr>
</table>


<table>
 <tr>
  <th>TEX1_1 MMAG=0</th>
 </tr>
 <tr>
  <th>TEX1_2 MMAG=1</th>
 </tr>
 <tr>
  <th>PRIM Triangle, CTXT=0</th>
 </tr>
 <tr>
  <th>ST</th>
 </tr>
 <tr>
  <th>RGBAQ</th>
 </tr>
 <tr>
  <th>XYZF2</th>
 </tr>
 <tr>
  <th>ST</th>
 </tr>
 <tr>
  <th>RGBAQ</th>
 </tr>
 <tr>
  <th>XTZF2</th>
 </tr>
 <tr>
  <th>ST</th>
 </tr>
 <tr>
  <th>RGBAQ</th>
 </tr>
 <tr>
  <th>XYZF2</th>
 </tr>
 <tr>
  <th>PRIM Triangle, CTXT=1</th>
 </tr>
 <tr>
  <th>ST</th>
 </tr>
 <tr>
  <th>RGBAQ</th>
 </tr>
 <tr>
  <th>XYZF2</th>
 </tr>
 <tr>
  <th>ST</th>
 </tr>
 <tr>
  <th>RGBAQ</th>
 </tr>
 <tr>
  <th>XYZF2</th>
 </tr>
 <tr>
  <th>ST</th>
 </tr>
 <tr>
  <th>RGBAQ</th>
 </tr>
 <tr>
  <th>XYZF2</th>
 </tr>
</table>


The following is an example of switching the contexts by the primitive.
[Register Setting Order]
Point Sampling
Bilinear Sampling


Drawing Triangle with
Context 1 (Point Sampling)


Drawing Triangle with
Context 2 (Bilinear Sampling)


- © SCEI

- 48-




SCE CONFIDENTIAL GS User's Manual Version 6.0


Moreover, the contexts can be switched in the process of drawing a primitive.
[Register Setting Order]
Point Sampling
Bilinear Sampling


<table>
 <tr>
  <th>TEX1_1 MMAG=0</th>
 </tr>
 <tr>
  <th>TEX1_2 MMAG=1</th>
 </tr>
 <tr>
  <th>PRMODECONT AC=0</th>
 </tr>
 <tr>
  <th>PRIM TriangleStrip</th>
 </tr>
 <tr>
  <th>PRMODE CTXT=0</th>
 </tr>
 <tr>
  <th>ST</th>
 </tr>
 <tr>
  <th>RGBAQ</th>
 </tr>
 <tr>
  <th>XYZF2</th>
 </tr>
 <tr>
  <th>ST</th>
 </tr>
 <tr>
  <th>RGBAQ</th>
 </tr>
 <tr>
  <th>XYZF2</th>
 </tr>
 <tr>
  <th>ST</th>
 </tr>
 <tr>
  <th>RGBAQ</th>
 </tr>
 <tr>
  <th>XYZF2</th>
 </tr>
 <tr>
  <th>ST</th>
 </tr>
 <tr>
  <th>RGBAQ</th>
 </tr>
 <tr>
  <th>PRMODE CTXT=1</th>
 </tr>
 <tr>
  <th>XYZF2</th>
 </tr>
 <tr>
  <th>PRMODE CTXT=0</th>
 </tr>
 <tr>
  <th>ST</th>
 </tr>
 <tr>
  <th>RGBAQ</th>
 </tr>
 <tr>
  <th>XYZF2</th>
 </tr>
</table>


Drawing 1st Triangle with Context 1
(Point Sampling)


Drawing 2nd Triangle with Context 2
(Bilinear Sampling)


Drawing 3rd Triangle with Context 1
(Point Sampling)


- 49-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


<table>
 <tr>
  <th>3.4.<br/>3.4.1.<br/>The GS can<br/>Assuming<br/>the texture</th>
  <td>Mapping<br/>of Texture Mapping Process<br/>texture mapping for all drawing primitives.<br/>in the primitive drawing information, texture mapping is based on the coordinate values of<br/>to each vertex.</td>
 </tr>
 <tr>
  <th>Texture data</th>
  <td>be stored in the texture buffer in local before Texture data is</td>
 </tr>
 <tr>
  <th>transmitted</th>
  <td>the host the function between buffers. To obtain effects,</td>
 </tr>
 <tr>
  <th>however,</th>
  <td>created via the can be used as textures.</td>
 </tr>
 <tr>
  <th>3.4.2.<br/>The texture</th>
  <td>Mapping Process Flow<br/>flow is shown in Figure 3-9.</td>
 </tr>
 <tr>
  <th></th>
  <td>DDA</td>
 </tr>
</table>


RGBA U,V


Texture
Width / Height


S,T Q
Normalization Texel
Coordinate Calculation
s = S / Q
t = T / Q


LOD
Calculation


Texel Coordinate Calculation
u = s x USIZE
v = t x VSIZE


STQ/UV Selection


Memory Address Calculation


Address


Texture
Page Buffer


Bit Expansion


Texel Value


Wrap Mode


Fragment Color


Filtering
Point Sampling
Bilinear


Texel Color


Texture
MODULATE/DECAL/HIGHLIGHT


Address (Fractional Part)


Color Component


Texture
Buffer


Pixel Color


Figure 3-11 Texture Mapping Process Flow


- © SCEI

- 50-




SCE CONFIDENTIAL GS User's Manual Version 6.0


The address in the texture buffer is calculated by using the texture coordinate (STQ or UV) values of the pixel
output from the DDA, and then the texel is read from the texture buffer. Bit expansion of RGB values and
Alpha value, and reference to the CLUT, are made according to the texture information, and bilinear filter etc.
are applied according to the filter mode. The texel color from this result and the fragment color output from the
DDA are calculated according to the texture function mode, and become the pixel color.


<table>
 <tr>
  <th>Texture</th>
  <th>Buffer</th>
 </tr>
 <tr>
  <td>When</td>
  <td>is reference to local is made the texture buffer. To</td>
 </tr>
 <tr>
  <td></td>
  <td>reflect the made to local in it is to invalidate the texture</td>
 </tr>
 <tr>
  <td></td>
  <td>when:</td>
 </tr>
 <tr>
  <td>•</td>
  <td>to use the data transmitted from the host within local as texture data</td>
 </tr>
 <tr>
  <td>•</td>
  <td>to use the CLUT data stored in the CLUT buffer it into the</td>
 </tr>
 <tr>
  <td>•</td>
  <td>to use the created via the as textures</td>
 </tr>
 <tr>
  <td>To</td>
  <td>the texture buffer, write an value to the TEXFLUSH</td>
 </tr>
</table>


## 3.4.3. Texture Information Setting


Texture information is set in the TEX0_1 or TEX0_2 registers. The following values can be set; they should
correspond to the format of the texture data stored in the texture buffer.


Texture Size (TW, TH)


The texture size can be 1 x 1 texel (min.) to 1024 x 1024 texels (max.). Width (TW) and height (TH) are
specifiable independently as powers of 2. When performing bilinear or trilinear sampling, the texture size
must be 8 x 8 texels.
Assume USIZE and VSIZE represent the texture height and width in texel units. Then
USIZE = 2TW (texels)
VSIZE = 2TH (texels)
The TW and TH values are specified in the TW and TH fields in the register.


## Texel Storage Mode (PSM)


Can be one of the following: RGBA32, RGB24, RGBA16, IDTEX8, IDTEX4


Starting Address (TBP0: Texture Base Pointer)


The TBP0 field of the register represents the address in units of 64 words.
Address = TBP0 x 64 (words)


Texture Buffer Width (TBW)


The TBW field of the register represents the buffer width in units of 64 texels. It is used for address
conversion in the texture buffer, and it is the same value as the buffer width set when the data is transferred
from host to local buffer. It differs from the texture size TW.
Width = TBW x 64 (words)


Color Component (TCC)


The TCC field determines whether to use the A value of the texture during the texture function execution.


## Texture Function (TFX)


The TFX field represents the operation mode of the texture function.


When the texel storage format is IDTEX8 or IDTEX4, settings related to the CLUT buffer should be made in
settings.


- 51-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 3.4.4. Specification of Texture Coordinate Values


There are two types of texture coordinate values: the texture coordinate system (S, T, Q), and the texel
coordinate system (U, V). The FST field of the primitive drawing attribute determines which coordinate system
to use. The features of the two types of coordinate systems are:


<table>
 <tr>
  <th>Coordinate</th>
  <th>Texture Coordinate</th>
  <th>Texel Coordinate</th>
 </tr>
 <tr>
  <td>Type</td>
  <td>Two Dimensional Homogeneous<br/>Coordinate</td>
  <td>Two Dimensional Coordinate System</td>
 </tr>
 <tr>
  <td>Features</td>
  <td>When s and t (normalized texel<br/>coordinates) obtained from the<br/>division of S and T by Q are in the<br/>[0.0, 1.0] range, they show the whole<br/>texture.</td>
  <td>When the width in u direction is USIZE<br/>and the width in v direction is VSIZE in<br/>the texture, and U is [0.0, USIZE] and V<br/>is [0.0, VSIZE], they show the whole<br/>texture.</td>
 </tr>
 <tr>
  <td>Perspective<br/>Correction</td>
  <td>Possible by setting Q to the<br/>appropriate value. (See "3.4.10.<br/>Correction".)</td>
  <td>Impossible</td>
 </tr>
 <tr>
  <td>MIPMAP<br/>(LOD Value</td>
  <td>Possible<br/>(Fixed value in each primitive)</td>
  <td>Possible<br/>(Fixed value in each primitive)</td>
 </tr>
 <tr>
  <td>MIPMAP<br/>(Q Linear</td>
  <td>Possible</td>
  <td>Impossible</td>
 </tr>
</table>


The relationship between texture coordinates (S, T, Q) and texel coordinates (U, V) is as follows:


u = USIZE x S/Q
v = VSIZE x T/Q
u and v values that the GS can process are in the range of -2047 to +2047. Values exceeding this range are
clamped. This may cause irregularities in texture drawing when s and t values become large.
The GS performs processing by rounding down the lower 8 bits of the mantissa of 32-bit floating-point values
specified to S, T, and Q. This may cause irregularities to the texture on the screen when expanded.


## 3.4.5. Texture Wrap Modes


When trying to refer to the outside of the valid range of the texture, a processing method (wrap mode) can be
selected. This method enables tiling, the repetition of a small texture. Using antialiasing and bilinear filter in
inappropriately set wrap mode may cause a defect when drawing the boundary lines of the texture, since they
refer to the texels outside the primitive edges.
Assuming the texture width and height are USIZE and VSIZE, when the texel coordinate values (u, v) of a texel
to be processed are outside the range of (0, 0) - (USIZE, VSIZE), the texel coordinate values (u', v') are
calculated by a method specific to each wrap mode.
There are four wrap modes (REPEAT, CLAMP, REGION_CLAMP, and REGION_REPEAT), and they can
be set independently for the horizontal and vertical directions. Settings are made in the WMS and WMT fields
of the CLAMP register.


## REPEAT Mode


The original image is mapped repeatedly.
(Formulas)
u' = u % USIZE
v' = v % VSIZE


- © SCEI

- 52-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## CLAMP Mode


The outermost color of the texel is enlarged.
(Formulas)
u < 0.0: u' = 0.0
v < 0.0: v' = 0.0
u > USIZE: u' = USIZE
v > VSIZE: v' = VSIZE


## REGION_CLAMP Mode


The data of the rectangular area specified in the MINU, MINV, MAXU and MAXV areas in the CLAMP
register becomes effective, and the outermost texel color is enlarged outside the area in the same way as the
CLAMP mode.
(Formulas)
u < MINU: u' = MINU
v < MINV: v' = MINV
u > MAXU: u' = MAXU
v > MAXV: v' = MAXV
When MIPMAP is performed, on the levels other than MIPMAP0, clamping is performed within the range
MINU >>n, MINV >>n, MAXU >>n, and MAXV >>n in the texel coordinate system, assuming the level
value to be n.


## REGION_REPEAT Mode


The following operations are applied to the integer parts (uint, vint) of the texel coordinates, and the texel
coordinate values are calculated.
(Formulas)
u' = (uint & UMSK) | UFIX
v' = (vint & VMSK) | VFIX
When using the REGION_REPEAT mode, the pattern, by which a part of the texture area is repeated, can
be used, and a mosaic effect can be achieved.


Example of Original Texture Pattern (64 x 64)


UMSK=0x00f UFIX=0x020
VMSK=0x00f VFIX=0x010


UMSK=0x3f0 UFIX=0x000
VMSK=0x3f0 VFIX=0x000


Figure 3-12 REGION_REPEAT Mode Effect


- 53-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


UMSK, VMSK, UFIX, and VFIX are specified in the CLAMP_1 or CLAMP_2 register. They are the same
bits as the MINU, MINV, MAXU, and MAXV fields respectively, but are processed differently, according to
the wrap mode.
However, the following conditions exist to perform drawing correctly when the bilinear filter is used as a
filter for reading texels.
In the mask pattern of UMSK and VMSK, the bit position where the value is 1 should take 0 as the
corresponding bit values of UFIX and VFIX. This is shown in the formulas below.


UFIX & (1 << (iu + 1) - 1) = 0
VFIX & (1 << (iv + 1) - 1) = 0
(iu/iv shows the position of the most significant bit out of the bits with UMSK/VMSK equal to 1.)


## 3.4.6. Format Conversion


The GS performs calculations of 8 bits each for RGBA. When the color format of the texel value read from the
texture buffer is not RGBA32, the color format is converted as follows.


## CLUT Conversion


When the texel storage formats are IDTEX8 and IDTEX4, the actual color value is obtained via the Color
Look-up Table (CLUT) based on the texel value.


Color Value 0
Color Value 1
Color Value 2
:
:
:
Color Value 15


Texel Value
(IDTEX4)


Format
Conversion


Figure 3-13 Conversion from Texel Value (IDTEX4) to Color Value via CLUT


The CLUT is a color value table of 256 entries with IDTEX8 and 16 entries with IDTEX4. This table is
read temporarily from the local buffer to a temporary buffer and used. (This temporary buffer is different
from the texture page buffer, so the contents of the texture page buffer are not destroyed by using the
CLUT.)
Data load control from the CLUT to a temporary buffer is described in detail in "3.4.7.
CLUT Buffer Control".


CLUT


## RGB Bit Expansion


When the texel value or the color format of the color value converted via the CLUT is RGBA16, 5 bits each
for RGB are expanded into 8 bits as follows:


Input Luminance
Value (5bits)


E


D


C


B A


© SCEI


Output
Luminance
Value (8bits)


E


D


C


- 54-

- B

- A


0 00




SCE CONFIDENTIAL GS User's Manual Version 6.0


## Alpha Value Bit Expansion


When the color formats are RGBA16 and RGB24, the Alpha value of 8 bits is obtained as shown in the table
below based on the TEXA register.


## RGBA16


<table>
 <tr>
  <th></th>
  <td>A = 0 A = 1</td>
 </tr>
 <tr>
  <th>AEM = 0</th>
  <td>TA0 TA1</td>
 </tr>
 <tr>
  <th>AEM = 1</th>
  <td>R = G = B = 0 -&gt; 0 TA1<br/>R | G | B is not equal to 0 -&gt; TA0</td>
 </tr>
 <tr>
  <th colspan="2"></th>
 </tr>
 <tr>
  <th>AEM = 0</th>
  <td>TA0</td>
 </tr>
 <tr>
  <th>AEM = 1</th>
  <td>R = G = B = 0 -&gt; 0<br/>R | G | B is not equal to 0 -&gt; TA0</td>
 </tr>
</table>


## RGB24


## 3.4.7. CLUT Buffer Control


The Color Look-up Table (CLUT) for texture mapping is used to convert the texture's index value to a color
value after copying from the CLUT buffer in local memory to the temporary buffer.


Local Memory


CLUT Buffer
Color Value 0
Color Value 1


<table>
 <tr>
  <th>Color Value 2</th>
  <td></td>
  <td>Color Value 1</td>
  <td></td>
 </tr>
 <tr>
  <th>.</th>
  <td></td>
  <td>Color Value 2</td>
  <td></td>
 </tr>
 <tr>
  <th>.<br/>.</th>
  <td>Load</td>
  <td>.</td>
  <td>Conversion</td>
 </tr>
 <tr>
  <th>Color Value 15</th>
  <td></td>
  <td>.<br/>.</td>
  <td></td>
 </tr>
</table>


CSA


Temporary Buffer
.
.
.
.


Color Value 0
.
Color Value 15
.
.
.
.


CLUT2


CLUT3
.
.
.
.


Figure 3-14 Loading from CLUT Buffer to Temporary Buffer


When loading to the CLUT for the first time after CLUT data is stored in local memory, it is necessary to
invalidate the texture buffer by accessing the TEXFLUSH register.


- 55-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## Temporary Buffer for CLUT


The temporary buffer is a cache-like memory inside the GS. It has a 1 KB capacity, which is equivalent to the
entries of 256 colors in a 32-bit CLUT Entry Storage Format (CPSM) and 512 colors in a 16-bit CPSM. CSA
specifies which part of the temporary buffer is used.
Entries in the temporary buffer are arranged as follows:


<table>
 <tr>
  <th>0</th>
 </tr>
 <tr>
  <th>:<br/>:</th>
 </tr>
 <tr>
  <th>15</th>
 </tr>
 <tr>
  <th>16</th>
 </tr>
 <tr>
  <th>:<br/>:</th>
 </tr>
 <tr>
  <th>31</th>
 </tr>
 <tr>
  <th>32</th>
 </tr>
 <tr>
  <th>:<br/>:<br/>:</th>
 </tr>
 <tr>
  <th>511</th>
 </tr>
</table>


CSA=0


CSA=1


CSA=31


CPSM=PSMCT16,PSMCT16S:(0 to 31 can be specified in CSA)


CPSM=PSMCT32: (0 to 15 can be specified in CSA)


<table>
 <tr>
  <th colspan="2"></th>
 </tr>
 <tr>
  <td>256</td>
  <td>0</td>
 </tr>
 <tr>
  <td>:<br/>:</td>
  <td>:<br/>:</td>
 </tr>
 <tr>
  <td>271</td>
  <td>15</td>
 </tr>
 <tr>
  <td>272</td>
  <td>16</td>
 </tr>
 <tr>
  <td>:<br/>:</td>
  <td>:<br/>:</td>
 </tr>
 <tr>
  <td>303</td>
  <td>31</td>
 </tr>
 <tr>
  <td>304</td>
  <td>32</td>
 </tr>
 <tr>
  <td>:<br/>:<br/>:</td>
  <td>:<br/>:<br/>:</td>
 </tr>
 <tr>
  <td>511</td>
  <td>255</td>
 </tr>
</table>


CSA=0


CSA=1


CSA=15


- © SCEI

- 56-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## Load Operation


CLUT data is loaded from the CLUT buffer to the temporary buffer when the TEX0 or TEX2 register is
accessed. Only one set of CLUT data is loaded, and values in other areas of the temporary buffer are saved.


## CLUT Data Specification


The CLUT data in the source is specified with CBP, CPSM or PSM of the TEX0 or TEX2 register or may
occasionally be specified with TEXCLUT. When the CLUT storage mode in the CLUT buffer is CSM1, the
CLUT must be located at the buffer starting address (the upper left point). When it is CSM2, the CLUT
location in the buffer is specified with TEXCLUT.


## Loading Position in Temporary Buffer


CLUT data in the temporary buffer is loaded to the entry address (CSA multiplied by 16) with CSA in the
TEX0 or TEX2 register.


## Loading Condition


The condition of loading to the temporary buffer is specified with CLD.


<table>
 <tr>
  <th>CLD</th>
  <th>Transmission Control</th>
 </tr>
 <tr>
  <td>000</td>
  <td>CLUT data is not loaded. in the buffer is</td>
 </tr>
 <tr>
  <td>001</td>
  <td>CLUT data is always loaded.</td>
 </tr>
 <tr>
  <td>010</td>
  <td>CLUT data is loaded, and internal CBP0 is rewritten the value of CBP.</td>
 </tr>
 <tr>
  <td>011</td>
  <td>CLUT data is loaded, and internal CBP1 is rewritten the value of CBP.</td>
 </tr>
 <tr>
  <td>100</td>
  <td>CBP0 is with CBP, and if different, CLUT data is loaded.</td>
 </tr>
 <tr>
  <td>101</td>
  <td>CBP1 is with CBP, and if different, CLUT data is loaded.</td>
 </tr>
</table>


always
register
by
always
register
by
compared
compared
The internal registers CBP0 and CBP1 control loading. Since they remember which CLUT buffer the data
has been loaded from, the temporary buffer can be used as cache memory (only in CSM1).


- 57-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 3.4.8. Texel Sampling


Point sampling or bilinear sampling can be selected when performing filter processing during texel sampling.
The sampling method is specified in the drawing environment register, TEX1.


## Point Sampling


Texel coordinates (iu, iv) are calculated from the normalization texel coordinates (s, t) according to the
following formulas, and the color value of the texel is assumed to be the texture value.


iu = Integer Part of (s x USIZE) iv = Integer Part of (t x VSIZE)


Operations such as repetition and clamping are performed to iu and iv depending on the wrap mode.
U 1.0 2.0 3.0
V 1.0


1. 

2. 


Figure 3-15 Texel Selection with Point Sampling Filter


## Bilinear Sampling


The texture value is calculated by applying linear interpolation to four texel colors as below. First, texel
coordinates (iu0, iv0), (iu1, iv0), (iu0, iv1), and (iu1, iv1) are calculated from the normalization texel
coordinates (s, t) by the following formulas.


iu0 = Integer Part of (s x USIZE - 0.5) iu1 = iu0 + 1
iv0 = Integer Part of (t x VSIZE - 0.5) iv1 = iv0 + 1


The texture value is calculated by linear interpolation from the following formulas, assuming that the color
values of these four texels are (Ra, Ga, Ba, Aa), (Rb, Gb, Bb, Ab), (Rc, Gc, Bc, Ac), and (Rd, Gd, Bd, Ad)
respectively and that the decimal parts of (s x USIZE - 0.5) and (t x VSIZE - 0.5) are Alpha and Beta
respectively.


R = (1.0- α)(1.0-β)Ra + α (1.0-β)Rb + (1.0-α)β Rc + αβ Rd
G = (1.0- α)(1.0-β)Ga + α (1.0-β)Gb + (1.0-α)β Gc + αβ Gd
B = (1.0- α)(1.0-β)Ba + α (1.0-β)Bb + (1.0-α)β Bc + αβ Bd
A = (1.0- α)(1.0-β)Aa + α (1.0-β)Ab + (1.0-α)β Ac + αβ Ad


Operations such as repetition and clamping are performed to iu0, iv0, iu1, and iv1 depending on the
wrapping mode.


- © SCEI

- 58-


<table>
 <tr>
  <th></th>
  <th></th>
  <th></th>
  <th></th>
  <th></th>
  <th></th>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
</table>




SCE CONFIDENTIAL GS User's Manual Version 6.0


U


V 1.0


1. 2.0 3.0


a b


<table>
 <tr>
  <th></th>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th>c</th>
  <td></td>
  <td>d</td>
 </tr>
</table>


1. 


3.0


Figure 3-16 Texel Selection with Bilinear Filter


## 3.4.9. Texture Function


The colors (Rt, Gt, Bt, At) obtained from the texture are blended with the colors (Rf, Gf, Bf, Af) of the
fragment obtained from the primitive by the DDA and converted into output colors (Rv, Gv, Bv, Av). At this
time, according to the texture function specified by the TFX flag of the TEX0 register and the mode specified
by the TCC flag, the conversion operation in the following table is applied. When the TCC flag is RGBA and
the texel format is TEX 24 or TEX16, the value set in the TEXA register is used as the Alpha value.


<table>
 <tr>
  <th>Function</th>
  <th>TCC=RGB</th>
  <th>TCC=RGBA</th>
  <th>Description</th>
 </tr>
 <tr>
  <td>MODULATE</td>
  <td>Rv = Rt * Rf<br/>Gv = Gt * Gf<br/>Bv = Bt * Bf<br/>Av = Af</td>
  <td>Rv = Rt * Rf<br/>Gv = Gt * Gf<br/>Bv = Bt * Bf<br/>Av = At * Af</td>
  <td>Adjusts the brightness of the texture<br/>color according to the fragment color.<br/>When the value of the fragment color<br/>is 0x80, the brightness of the output<br/>color is the same as that of the original<br/>texture.</td>
 </tr>
 <tr>
  <td>DECAL</td>
  <td>Rv = Rt<br/>Gv = Gt<br/>Bv = Bt<br/>Av = Af</td>
  <td>Rv = Rt<br/>Gv = Gt<br/>Bv = Bt<br/>Av = At</td>
  <td>Outputs the texture color as is.</td>
 </tr>
 <tr>
  <td>HIGHLIGHT</td>
  <td>Rv = Rt * Rf + Af<br/>Gv = Gt * Gf + Af<br/>Bv = Bt * Bf + Af<br/>Av = Af</td>
  <td>Rv = Rt * Rf + Af<br/>Gv = Gt * Gf + Af<br/>Bv = Bt * Bf + Af<br/>Av = At + Af</td>
  <td>Adjusts the brightness of the texture<br/>color as in the case of MODULATE,<br/>then adds a highlight in pure white<br/>based on the alpha value of the<br/>fragment color. Suitable for<br/>translucent</td>
 </tr>
 <tr>
  <td>HIGHLIGHT2</td>
  <td>Rv = Rt * Rf + Af<br/>Gv = Gt * Gf + Af<br/>Bv = Bt * Bf + Af<br/>Av = Af</td>
  <td>Rv = Rt * Rf + Af<br/>Gv = Gt * Gf + Af<br/>Bv = Bt * Bf + Af<br/>Av = At</td>
  <td>Similar to HIGHLIGHT, but stores<br/>the alpha value of the texture color.<br/>Suitable for highlighting opaque<br/>polygons.</td>
 </tr>
</table>


However, the operator * means A*B = (A x B) >> 7, and the calculation result is clamped between 0 and 0xff.
(When the value of the fragment color is 0x80, the brightness of the texture corresponds to the brightness of the
output color.)


- 59-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 3.4.10. Perspective Correction


The GS has the ability to correct texture perspective distortion. This is done by giving the value, to which
appropriate preprocessing is performed in the texture coordinate system (S, T, Q), to the GS as the vertex
coordinate values in the texture space.
When drawing a three-dimensional primitive, the point (Xv, Yv, Zv) in the view coordinate system usually
shown in the rectangular parallelepiped is converted into the point (Xp, Yp, Zp) in the primitive coordinate
system shown in the truncated quadrangular-pyramid by transparency perspective conversion. In the primitive
coordinate system, a place close to the viewpoint is expanded, and a place far from the viewpoint is reduced and
distorted.


Z


Z


X


X


Y


Y


Primitive Coordinate System


View Coordinate System


Figure 3-17 Coordinate System in Transparency Perspective Conversion


This distortion is caused by transparency perspective conversion. Calculations for linear interpolation for DDA
etc. are made when drawing a primitive to the frame buffer. This is performed in the primitive coordinate
system. However, since the relation between texture and primitive is defined in the view coordinate system, a
positional deviation from the primitive is caused if the texture coordinate values are linear-interpolated as they
are when drawing the primitive. This is the so-called perspective distortion of the texture.
To avoid this, equivalent conversion is performed to the texture coordinates as well. Let the texture coordinates
of the primitive vertex in the view coordinate system (the world coordinate system or local coordinate system is
also OK) be (sv, tv) and the divisor in the transparency perspective conversion be W. The texture coordinates
(S, T, Q) in the primitive coordinate system are gained from the formulas below.


S = sv / W T = tv / W Q = 1 / W
Proper perspective correction can be achieved by giving these S, T, and Q to the GS. (After linear-interpolating
S, T, and Q, the GS calculates the normalization texel coordinate values from s = S/Q and t = T/Q.)
Moreover, perspective distortion is not generated for Sprites, where each fragment always takes the same Z
value. In such a case, texel coordinates for the vertex of each primitive can be specified directly. (Note that they
are not the normalization texel coordinates.)
Whether the homogeneous texture coordinates (S, T, Q) or texel coordinates (U, V) are used to specify the
texture coordinates is determined by the FST flag of the PRIM register or PRMODE register. Regarding the
primitive vertex, S and T values are set in the ST register and Q value is set in the Q area of the RGBAQ
register. Moreover, U and V values are set in the UV register.


- © SCEI

- 60-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 3.4.11. MIPMAP


MIPMAP textures can be set in seven stages from level 0 to level 6. The texture height and width should each
be a power of 2. When MIPMAP level 0 texture is 2m x 2n in sizing, levels 1 and 2 are set to 2m-1 x 2n-1 and 2m-2
x 2n-2 respectively in texture sizing. Sizing is set in succession by reducing the height and width by half, until
either the width or height reaches 1 for point sampling or 8 for bilinear sampling.
When MIPMAP is not performed, level 0 is used.


## Calculation of Texture Base Pointer


The user can set the base pointer of the MIPMAP texture at each level to the desired position in the same
way as texture level 0. However, because the page size changes according to the pixel storage format of the
texture, it is necessary to set the base pointer so that the textures do not overlap.
In the example below, the MIPMAP is composed assuming a texture of 512 x 512 texels is level 0 when the
format is PSMCT16.


<table>
 <tr>
  <th>MIPMAP</th>
  <th>Base Pointer</th>
  <th>Buffer</th>
  <th>Texture Width</th>
 </tr>
 <tr>
  <td>Level</td>
  <td></td>
  <td>Width</td>
  <td></td>
 </tr>
 <tr>
  <td>0</td>
  <td>7200</td>
  <td>8</td>
  <td>9</td>
 </tr>
 <tr>
  <td>1</td>
  <td>9248</td>
  <td>4</td>
  <td>Not</td>
 </tr>
 <tr>
  <td>2</td>
  <td>9760</td>
  <td>2</td>
  <td>Not</td>
 </tr>
 <tr>
  <td>3</td>
  <td>9888</td>
  <td>1</td>
  <td>Not</td>
 </tr>
 <tr>
  <td>4</td>
  <td>9920</td>
  <td>1</td>
  <td>Not</td>
 </tr>
 <tr>
  <td>5</td>
  <td>9928</td>
  <td>1</td>
  <td>Not</td>
 </tr>
 <tr>
  <td>6</td>
  <td>9930</td>
  <td>1</td>
  <td>Not</td>
 </tr>
</table>


The minimum value at base pointer intervals by which the textures of different MIPMAP levels are stored
changes depending on the texture format, texture size and ratio of height and width.


## Automatic Calculation of Base Pointer


The base pointer for texture levels 1 to 3 can be calculated automatically on the condition that texture format
and size must meet the following requirements.


- PSMCT32/PSMCT24/PSMT8H/PSMT4HL/PSMT4HH Width = Height = {32, 64, 128, 256, 512}

- PSMCT16/PSMT8/PSMT4 Width = Height = {32, 64, 128, 256, 512, 1024}


With automatic calculation, textures up to level 3 are stored in a continuous memory area, as in the examples
above. When the texture is loaded, it is necessary to store the texture at the position of the base pointer
calculated automatically.
The automatic calculation is executed when the TEX0 register is set after the MTBA field of the TEX1
register is set to 1.
For the texture of MIPMAP levels 1 to 3, the base pointer is specified in the fields TBP1 to TBP3 of the
MIPTBP1 register and the buffer width is specified in the field TBW1 to TBW3. As for the texture of
MIPMAP levels 4 to 6, the base pointer is specified in the fields TBP4 to TBP6 of the MIPTBP2 register and
the buffer width is specified in the field TBW4 to TBW6. Moreover, the maximum MIPMAP level used is
set to the MXL flag in the TEX1 register.


- 61-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 3.4.12. LOD Setting


The texture level and filter used for mapping are controlled by the value of the LOD (Level of Detail). There
are two methods of calculating the LOD, and they are specified in the LCM fields of the TEX1 register. One
method is to obtain the LOD from the Q value of each primitive vertex and the L and K fields of the TEX1
register according to the formula below, and the other is to specify the LOD in the K field only. When LCM is
set to 0, LOD is calculated from the Q value even if the texture coordinates are given by UV.


LCM = 0 :
LCM = 1 :


LOD = ( log2(1/Q) << L) + K
LOD = K


When the calculated LOD value is 0.0 or less, the level 0 texture is used, and the filter specified in the MMAG
field of the TEX1 register is used.
When the LOD value is more than 0.0, the level 0 to 6 textures are used depending on the LOD value, and the
filter specified by the MMIN flag of the TEX1 register is used.
The following table shows the relation between the MMIN flag and the filter used.


<table>
 <tr>
  <th>MMIN</th>
  <th>Level m Filer</th>
  <th>between Levels</th>
 </tr>
 <tr>
  <td>0</td>
  <td>Disabled</td>
  <td>Level 0</td>
 </tr>
 <tr>
  <td>1</td>
  <td>Disabled</td>
  <td>Level 0 bilinear</td>
 </tr>
 <tr>
  <td>2</td>
  <td>Point sampling</td>
  <td>Selection of m or m+1 by LOD</td>
 </tr>
 <tr>
  <td>3</td>
  <td>Point</td>
  <td>Linear LOD</td>
 </tr>
 <tr>
  <td>4</td>
  <td>Bilinear</td>
  <td>Selection of m or m+1 by LOD (round-off)</td>
 </tr>
 <tr>
  <td>5</td>
  <td>Bilinear</td>
  <td>Linear interpolation by LOD (trilinear)</td>
 </tr>
</table>


*m is the integer part of the LOD value.


The bilinear filter in each MIPMAP imposes almost no speed penalty, but the linear interpolation filter between
levels affects the drawing time.
Moreover, when the number of pixels for the height or width of the texture is four or less, correct images
cannot be obtained with bilinear and trilinear filters.


## Specifications of L and K


The weight of the MIPMAP filter is specified by L, and the position of MIPMAP level 0 is set by K.
Assuming the fragment position, where the ratio of the texel to the frame buffer pixel is 1 to 1, to be Z0 in
texture mapping, K is set by the following formula.


K = - ( log2(Z0 / h ) << L )


However, h is assumed to be the distance between the viewpoint and screen, and Q of each vertex of the
primitive is assumed to be h/Z. The relation between Z and LOD is shown in the figure below.


- © SCEI

- 62-




SCE CONFIDENTIAL GS User's Manual Version 6.0


Z2


Z1


Z0


Z


Viewpoint


LOD


0 1 2


Figure 3-18 LOD when L = 0 and K = - log2( Z0 / h )
When mapping the texture to the primitive parallel to the screen, it is appropriate to set L = 0, in which the
reduction on the display corresponds to the increase of LOD. However, when the primitive is inclined to the
screen, a value more than 0 is set to L.
Because the effect of MIPMAP changes with the texture pattern, appropriate L and K values should be set
according to the texture used. To control the MIPMAP levels individually in primitives units, the value of Q can
be directly controlled.


## 3.4.13. Texture Mapping Procedure


The following is an example of a register setting procedure for texture mapping.
general drawing should already be set.)


(The registers necessary for


1. Transmit the texture to the local buffer.

2. Transmit the CLUT to the local buffer.

3. Set the TEXCLUT register (if necessary).

4. Access the TEXFLUSH register (only once at first).

5. Set the TEXA register.

6. Set the TEX1_1 | TEX1_2 register.

7. Set the TEX0_1 | TEX0_2 register.

8. Set the MIPTBP1_1 | MIPTBP1_2 register. 9. Set the MIPTBP2_1 | MIPTBP2_2 register. (Repeat the following.) 10. Set the PRIM register. 11. Set the ST or UV register. 12. Set the RGBAQ register. 13. Set the XYZF register.

- 63-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 3.5. Fog Effect


The Fog effect is achieved by blending the color of each texel output from the texture function and the Fog
color. Let the output colors from the texture function be (Rv, Gv, Bv, Av); let the Fog coefficient, which has
been obtained by linear-interpolating the Fog value set for each vertex of the primitive, be F; and the Fog colors
written to the FOGCOL register be (Rfc, Gfc, Bfc). The blending operation is shown in the formulas below.


R = F * Rv + ( 0xff - F )* Rfc
G = F * Gv + ( 0xff - F )* Gfc
B = F * Bv + ( 0xff - F )* Bfc
A = Av


However, A*B = ( A x B) >> 8.


The Fog coefficient is stored in the F field of the XYZF or XYZF2 register. The Fog color is stored in the
FOGCOL register. Whether or not Fogging is performed is specified with the FGE flag of the PRIM register.


- © SCEI

- 64-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 3.6. Antialiasing


In the GS, Antialiasing can be performed for Line, LineStrip, Triangle, TriangleStrip, and TriangleFan. This
process is performed for each line or triangle.


## 3.6.1. Principle of Antialiasing


Antialiasing is achieved by calculating the coverage Cov (ratio of the area that covers the pixel) to each pixel at
the edge of the GS primitive, assuming this to be the Alpha value, and performing Alpha-blending to the
destination color (color distant from the primitive) and the primitive color.
The effect of Antialiasing is different depending on the combination of the AA1 flag to set Antialiasing, ABE
flag to set Alpha-blending, and the Alpha value calculated for the pixel. This is shown in the table below. Cov
shows the value of the coverage of the pixel. When the coverage is 100%, Cov = 0 x 80 (128 in decimal) is
obtained.


<table>
 <tr>
  <th>Flag AA1</th>
  <th>Flag ABE</th>
  <th>Alpha Value to be Output</th>
  <th>Effect Antialiasing</th>
  <th>Effect Blending</th>
 </tr>
 <tr>
  <td>0</td>
  <td>0</td>
  <td>A</td>
  <td>NO</td>
  <td>NO</td>
 </tr>
 <tr>
  <td>0</td>
  <td>1</td>
  <td>A</td>
  <td>NO</td>
  <td>YES</td>
 </tr>
 <tr>
  <td>1</td>
  <td>0</td>
  <td>Cov</td>
  <td>YES</td>
  <td>NO</td>
 </tr>
 <tr>
  <td>1</td>
  <td>1</td>
  <td>Cov (Pixel with A equal to 0x80)</td>
  <td>YES</td>
  <td>NO</td>
 </tr>
 <tr>
  <td>1</td>
  <td>1</td>
  <td>A (Pixel with A not equal to 0x80)</td>
  <td>NO</td>
  <td>YES</td>
 </tr>
</table>


## 3.6.2. Antialiasing Control


## Drawing Order


To perform Antialiasing appropriately, the part distant from the primitive should be drawn before drawing
the primitive. It is necessary to sort all the drawing primitives in positional order (far to close) to achieve
ideal Antialiasing.


## Write to Z Buffer


Since Antialiasing is performed to each line or triangle, how to deal with the parts shared by lines and
triangles becomes a problem. In the GS, to guarantee the connection of the image in the boundary part,
writing to the Z buffer is not performed for pixels with coverage Cov less than 0x80.
As for the line, Cov is always less than 0x80. Therefore, the Z buffer is not written when the AA1 flag is 1.
Antialiasing is controlled by the AA1 flag of the PRIM or PRMODE register. Moreover, it is necessary to
set the flags of the ALPHA register for blending, as shown below.


A = Cs (Source Color)


B = Cd (Destination Color)


C = As (Source Alpha)


D = Cd (Destination Color)


- 65-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 3.7. Pixel Test


The pixel test is the process of inspecting whether or not a pixel to be drawn meets specified conditions. It also
controls drawing processing for pixels that failed. The pixel test does not change the pixel value.
The pixel test consists of the following test items, which are executed in this order.


<table>
 <tr>
  <th>Pixel Test</th>
  <td>Test Contents</td>
 </tr>
 <tr>
  <th>Scissoring Test</th>
  <td>Test by the position where the pixel is drawn.</td>
 </tr>
 <tr>
  <th>Alpha Test</th>
  <td>Comparison test between the pixel's alpha value and standard value.</td>
 </tr>
 <tr>
  <th>Destination<br/>Alpha Test</th>
  <td>Test by the pixel's alpha value at the drawing destination in the frame buffer.</td>
 </tr>
 <tr>
  <th>Depth Test</th>
  <td>Comparison test between the pixel's Z value and the corresponding Z value in<br/>Z buffer.</td>
 </tr>
</table>


the point of view of GS internal processing, the scissoring test is carried out in the Rasterizing (DDA)


From


stage and not in the pixel test stage. For the convenience of the user, however, it is described in this section.


Processes succeeding the pixel test such as Alpha-blending are performed even to a pixel that failed the alpha
test, destination alpha test, or depth test, and the pixel is finally controlled in drawing when the pixel value is
written in the frame buffer and the Z buffer. It may appear as if the pixel test were performed after Alpha-
blending.


## 3.7.1. Scissoring Test


This test checks whether the coordinate values of the pixel to be drawn are in the rectangular (scissoring) area
specified in the window coordinate system. Pixels judged to be outside the scissoring area are not processed
further.


## Set Register


SCISSOR_1, SCISSOR_2


## Test Contents


The pixel is judged by the following formulas, assuming the pixel coordinates are (x, y). A pixel located on
the boundary of the scissoring area passes.


scissor_test(int x, y)
{
if (x < SCISSOR.SCAX0 || x > SCISSOR.SCAX1) return(FAIL);
if (y < SCISSOR.SCAY0 || y > SCISSOR.SCAY1) return(FAIL);
return(PASS);
}


The scissoring test cannot be omitted.


- © SCEI

- 66-




SCE CONFIDENTIAL GS User's Manual Version 6.0


<table>
 <tr>
  <th>3.7.2. Alpha Test</th>
 </tr>
 <tr>
  <td>The value of the and the standard value are is controlled</td>
 </tr>
 <tr>
  <td>if the does not meet the condition that was set.</td>
 </tr>
</table>


## Set Register


TEST_1, TEST_2 (ATE, ATST, AREF, and AFAIL fields)


## Test Contents


<table>
 <tr>
  <th>ATE</th>
  <th>ATST</th>
  <th>Test Contents</th>
 </tr>
 <tr>
  <td>0</td>
  <td>-</td>
  <td>No</td>
 </tr>
 <tr>
  <td rowspan="8">1</td>
  <td>NEVER</td>
  <td>All fail.</td>
 </tr>
 <tr>
  <td>ALWAYS</td>
  <td>All</td>
 </tr>
 <tr>
  <td>LESS</td>
  <td>Pixels less than AREF</td>
 </tr>
 <tr>
  <td>LEQUAL</td>
  <td>Pixels less than or to AREF</td>
 </tr>
 <tr>
  <td>EQUAL</td>
  <td>Pixels to AREF</td>
 </tr>
 <tr>
  <td>GEQUAL</td>
  <td>Pixels than or to AREF</td>
 </tr>
 <tr>
  <td>GREATER</td>
  <td>Pixels than AREF</td>
 </tr>
 <tr>
  <td>NOTEQUAL</td>
  <td>Pixels not to AREF</td>
 </tr>
</table>


<table>
 <tr>
  <th>Alpha Test Result</th>
  <th>AFAIL Setting</th>
  <th>RGB</th>
  <th>A</th>
  <th>Z</th>
 </tr>
 <tr>
  <td rowspan="4">Failed</td>
  <td>KEEP</td>
  <td>Controlled</td>
  <td>Controlled</td>
  <td>Controlled</td>
 </tr>
 <tr>
  <td>FB_ONLY</td>
  <td>Effective</td>
  <td>Effective</td>
  <td>Controlled</td>
 </tr>
 <tr>
  <td>ZB_ONLY</td>
  <td>Controlled</td>
  <td>Controlled</td>
  <td>Effective</td>
 </tr>
 <tr>
  <td>RGB_ONLY*</td>
  <td>Effective</td>
  <td>Controlled</td>
  <td>Controlled</td>
 </tr>
</table>


A pixel that failed the alpha test is not controlled completely in drawing, but can be specified in the AFAIL
field in the TEST register so that the RGB, A, and Z values are controlled individually. Details are as
follows.


only when the color format is RGBA32.
operation is made with FB_ONLY.


*RGB_ONLY is effective
In other formats,


Controlled values do not become effective even if they pass in other pixel tests after this.


- 67-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


<table>
 <tr>
  <th>3.7.3. Destination Alpha Test</th>
 </tr>
 <tr>
  <th>This test checks the value of the for in the frame buffer A</th>
 </tr>
 <tr>
  <th>failed is controlled in</th>
 </tr>
</table>


Set Register
TEST_1, TEST_2


(DATE and DATM fields)


Test Contents
The contents of the Destination Alpha Test depend on the pixel storage mode of the frame buffer.


Frame Buffer: PSMCT32 (RGBA32)
testing. (All pixels pass.)
pass.
pass.


<table>
 <tr>
  <th>DATE</th>
  <th>DATM</th>
  <th>Test Contents</th>
 </tr>
 <tr>
  <td>0</td>
  <td>-</td>
  <td>No</td>
 </tr>
 <tr>
  <td>1</td>
  <td>0</td>
  <td>Pixels with bit 7 of A set to 0</td>
 </tr>
 <tr>
  <td>1</td>
  <td>1</td>
  <td>Pixels with bit 7 of A set to 1</td>
 </tr>
 <tr>
  <td>Buffer:</td>
  <td>PSMCT16</td>
  <td></td>
 </tr>
 <tr>
  <td>DATE</td>
  <td>DATM</td>
  <td>Test Contents</td>
 </tr>
 <tr>
  <td>0</td>
  <td>-</td>
  <td>No (All</td>
 </tr>
 <tr>
  <td>1</td>
  <td>0</td>
  <td>Pixels with A set to 0</td>
 </tr>
 <tr>
  <td>1</td>
  <td>1</td>
  <td>Pixels with A set to 1</td>
 </tr>
</table>


Frame
(RGBA16)
testing.
pixels pass.)
pass.
pass.


Frame Buffer is PSMCT24 (RGB24)
Because the frame buffer does not have A value, all pixels pass regardless of the setting of DATE and
DATM.


## 3.7.4. Depth Test


compares
drawing pixel
corresponding
pixel
drawing.


Set Register
TEST_1, TEST_2


<table>
 <tr>
  <th>ZTE</th>
  <th>ZTST</th>
  <th>Test Contents</th>
 </tr>
 <tr>
  <td>0</td>
  <td>-</td>
  <td>(Disabled)</td>
 </tr>
 <tr>
  <td rowspan="4">1</td>
  <td>NEVER</td>
  <td>All pixels fail.</td>
 </tr>
 <tr>
  <td>ALWAYS</td>
  <td>All pixels pass.</td>
 </tr>
 <tr>
  <td>GEQUAL</td>
  <td>Pixels greater than or equal to Z buffer value pass.</td>
 </tr>
 <tr>
  <td>GREATER</td>
  <td>Pixels greater than Z buffer value pass.</td>
 </tr>
</table>


(ZTE and ZTST fields)


Test Contents
Setting ZTE to 0 is prohibited since it may cause a malfunction. To omit the depth test, set ZTE to 1 and
ZTST to ALWAYS, and the ZMSK field of the ZBUF register to 1 respectively. Due to these settings, the Z
buffer is neither accessed nor updated for any pixels. As a result, the operation becomes the same as the one
without the depth test.


- © SCEI

- 68-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 3.8. Alpha Blending


Blending calculation can be made between the depth test output color (Source Color) Cs and the frame buffer
pixel color (Destination Color) Cd.
Calculation of RGB values is performed. With Alpha value A, the source Alpha value (As) is written in the
frame buffer as is. However, when the FBA flag of the FBA register is set, a prescribed operation is performed.
Alpha-blending is set ON/OFF by the ABE flag of the PRIM or PRMODE register. When performing
antialiasing (setting AA1 to 1), the blending function operates automatically. Therefore, the ABE flag must
always be set to 0 (OFF).


## 3.8.1. Blending Setting


The formula for blending is shown below. A, B, C, and D in the formula can be set to take the value in the
following table with the flag of the ALPHA register. FIX is the fixed Alpha value set in the ALPHA register.


Output Color = (A - B) * C + D
However, X * Y = (X x Y) >> 7


<table>
 <tr>
  <th></th>
  <td>Selectable Color</td>
 </tr>
 <tr>
  <th>A</th>
  <td>Cs, Cd, 0</td>
 </tr>
 <tr>
  <th>B</th>
  <td>Cs, Cd, 0</td>
 </tr>
 <tr>
  <th>C</th>
  <td>As, Ad, FIX</td>
 </tr>
 <tr>
  <th>D</th>
  <td>Cs, Cd, 0</td>
 </tr>
</table>


When the Alpha value is 0x80, the multiplier to the source color becomes 1.0. An example of flag setting
applied to normal blending is shown as follows. When Antialiasing AA1 is performed, the same setting is made.


## Equivalent


Cd*(0x80
As)


Additionally,
settings
as subtraction
brightness


<table>
 <tr>
  <th>A</th>
  <th>B</th>
  <th>C</th>
  <th>D</th>
  <th>Formula</th>
 </tr>
 <tr>
  <td>Cd</td>
  <td>Cs</td>
  <td>0x80</td>
  <td>0</td>
  <td>Cd - Cs</td>
 </tr>
 <tr>
  <td>Cd</td>
  <td>0</td>
  <td>As</td>
  <td>Cd</td>
  <td>+</td>
 </tr>
 <tr>
  <td>0</td>
  <td>Cd</td>
  <td>As</td>
  <td>Cd</td>
  <td>-</td>
 </tr>
</table>


## A B


C


D


## Formula


Cs


Cd


As


Cd


Cs*As +
-


various


such


and


addition/decrease are possible.


In the RGBA16 mode, where Destination Alpha value is 1 bit only, Alpha is treated as 0 when A is 0 and as
0x80 when A is 1. In the RGB24 mode, Alpha is treated as 0x80.
The result of Alpha-blending is passed to the following Dithering processing as is, without being clamped.


## 3.8.2. PABE Flag


When the PABE flag is 1, the value of MSB of the Source Alpha determines whether to perform Alpha-blending
per pixel. Alpha-blending is ON when MSB is 1 and OFF when MSB is 0.


- 69-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 3.9. Writing to the Frame Buffer


The following processing is performed according to the pixel storage mode before the pixel value, the output
from the pixel pipeline, is written in the frame buffer.


<table>
 <tr>
  <th>RGB A</th>
 </tr>
 <tr>
  <th>Dithering A Correction Value</th>
 </tr>
 <tr>
  <th>Clamping</th>
 </tr>
 <tr>
  <th>Format Conversion</th>
 </tr>
</table>


Frame Buffer


Z Buffer


Figure 3-19 Write Process to Frame Buffer


## 3.9.1. Dithering


If the frame buffer color format is RGBA16, the RGB luminance values are each converted from 8 bits to 5 bits.
At this time, it is possible to decrease the Mach band by dithering.
Dithering is set ON/OFF by the DTHE register.
Dithering is performed by adding the offset value selected according to the coordinates of the pixel from the
dither matrix of 4 x 4 set in the DIMX register to each luminance value of the RGB of the pixel. It is shown in
the following formulas.


Rout = Rin + DIMX[Y%4][X%4]
Gout = Gin + DIMX[Y%4][X%4]
Bout = Bin + DIMX[Y%4][X%4]


X,Y : Window Coordinate Values of Pixel in Frame Buffer
Rin, Gin, Bin : Input Luminance Value (8 bits)
Rout, Gout, Bout : Output Luminance Value (8 bits)


Dither Matrix Setting Example


<table>
 <tr>
  <th>-4</th>
  <td>2</td>
  <td>-3</td>
  <td>3</td>
 </tr>
 <tr>
  <th>0</th>
  <td>-2</td>
  <td>1</td>
  <td>-1</td>
 </tr>
 <tr>
  <th>-3</th>
  <td>3</td>
  <td>-4</td>
  <td>2</td>
 </tr>
 <tr>
  <th>1</th>
  <td>-1</td>
  <td>0</td>
  <td>-2</td>
 </tr>
</table>


When the color format of the frame buffer is RGBA32 or RGB24, the result of dithering is not guaranteed.


- © SCEI

- 70-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 3.9.2. Color Clamp


Since the RGB value of a pixel occasionally exceeds the range of 0-255 after operations such as Alpha-blending,
the result is stored with 9 bits for each RGB value. It is possible to select clamping to set this value within the
range of 0-255, or to extract the lower 8 bits. Whether or not to perform clamping is determined by the
COLCLAMP register.


## 3.9.3. Alpha Value Correction


It is possible to correct the pixel alpha value with the value previously set in the FBA register.
when reusing the image data in the frame buffer as a texture.


This is done


RGBA32


FBA


bit 7
bit 6


OR


Input A Value


Output A Value


bit 0


RGBA16


FBA


OR Output A Value


bit 7


Input A Value


Figure 3-20 Alpha Value Correction


## 3.9.4. Format Conversion


Pixel color values are packed into a fixed number of bits, according to the pixel storage mode set by the
FRAME_1 or FRAME_2 register, and written in the frame buffer.


RGBA32 (PSMCT32) Mode


R


G


A
7


B


0 7


0 7


0 7


0


R


A


B


G


31


24 23


16 15


8 7


0


## RGB24 (PSMCT24) Mode


A
7


B GR
07 7 0 070


Not Used


B GR
23 16 15 8 7 0


- 71-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## RGBA16 (PSMCT16,PSMCT16S) Mode


A B GR
7 07 3 3 3 7 0 070


AB G
15 14 10 9


RR


5 4


0


## 3.9.5. Masking


The mask can be set so that only the prescribed bits are written when writing data to the frame buffer.
The value of the mask is specified in the FBMSK field of the FRAME_1 or FRAME_2 register. Since the bit
position of the FBMSK corresponds to the pixel value before format conversion, special care is needed when
the pixel format of the frame buffer is RGBA16. The relation between FBMSK and the frame buffer in the case
of RGBA16 is shown as follows.


FBMSK


31


24 23


19 15


11 7


3


0


Frame Buffer


AB GR
15 14 10 9 5 4 0


Regarding the Z buffer, it is possible to control only whether or not to write all the bits.
This is specified with the ZMSK of ZBUF_1 or ZBUF_2 register.


- © SCEI

- 72-




SCE CONFIDENTIAL GS User's Manual Version 6.0


memory
transmitting
arrangement
pixels
memory,
pixel storage
according
specification.
processes involving changes
pixel
(e.g.
Alpha
operation,
masking)
performed.
transferring
supported only
per pixel.


- 73-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 4.1. Transmission Parameters


When transmitting data, it is necessary to set the following parameters.


## 4.1.1. Destination Buffer / Source Buffer (BITBLTBUF Register)


For each buffer from/to which data is transmitted, the base pointer, buffer width, and pixel storage format are
set in the BITBLTBUF register.
When transmitting from host to local buffer, only the destination buffer is set. When transmitting from local
buffer to host, only the source buffer is set. When transmitting from local buffer to local buffer, both the
destination and source buffers are set.
The buffer base pointer is specified as a 14-bit unsigned integer in units of 64 words. The buffer width is
specified by a 6-bit unsigned integer in units of 64 pixels.
All pixel storage formats can be used, except that when transmitting from local buffer to host, pixel storage
formats PSMT4, PSMT4HL and PSMT4HH cannot be used. When transmitting from local buffer to local
buffer, the bit length of the pixels to be stored respectively in the source and destination buffer should be the
same.


## 4.1.2. Offset in Buffer (TRXPOS Register)


The TRXPOS register specifies the offset coordinates at the upper left point of the transmission area in the
transmission buffer and the pixel transmission direction. When transmitting from host to local buffer, only the
settings for the destination buffer are made. When transmitting from local buffer to host, only the settings for
the source buffer are made. And when transmitting from local buffer to local buffer, settings are made for both
the source and destination buffers.
The four figures below show the direction of the pixel transmission of the transfer data within the transmission
destination area. This setting is enabled only when transmitting from local buffer to local buffer, and when
transmitting from host to local buffer and local buffer to host, data is arranged in the direction from left to right
and top to bottom. The transmission starting point in the rectangular area varies according to the pixel
transmission direction.


Start


Start


Start


Start


1. Right to Left Top to Bottom

2. Right to Left Bottom to Top


0. Left to Right
Top to Bottom


1. Left to Right Bottom to Top


Figure 4-1 Pixel Transmission Direction


- © SCEI

- 74-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 4.1.3. Width and Height of Transmission Area (TRXREG Register)


The TRXREG register specifies the width and height of the transmission area.
When transmitting from host to local buffer, settings for the destination buffer are made. When transmitting
from local buffer to host, settings for the source buffer are made. And when transmitting from local buffer to
local buffer, settings are made for the destination buffer. (The width and height of the transmission area in the
destination and the source should be the same.)
When the coordinate value of the transmission area is 2048 or more, it wraps around at 2048. That is, if the
transmission start coordinates are (SAX, SAY), the coordinates of the pixel to be transmitted actually are (X, Y)
and the offset coordinates from the transmission start coordinates of the pixel are (RRX, RRY), then:


X = (SAX + RRX) % 2048
Y = (SAY + RRY) % 2048


The figure below shows the relation between transmission area parameters when the pixel transmission direction
is left to right and top to bottom.
Buffer Width
(SBW/DBW)
(SSAY/DSAY)


<table>
 <tr>
  <th>Address<br/>(SBP/DBP)</th>
 </tr>
 <tr>
  <td></td>
 </tr>
</table>


(SSAX/
DSAX)


RRH


RRW


Figure 4-2 Parameters to Show Transmission Area


## 4.1.4. Transmission Direction and Transmission Start (TRXDIR Register)


This register specifies the directions of transmission. The following three can be specified.


- Host to local buffer

- Local buffer to host

- Local buffer to local buffer


The GS starts transmission immediately after the TRXDIR register is accessed.


- 75-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 4.1.5. Limitations on Transmission Start Coordinates and Width


The table below shows the limitations on the start coordinates and transmission area width according to the
pixel storage format. These limitations do not apply to the transmission from local buffer to host.


<table>
 <tr>
  <th>Pixel</th>
  <th>Limitation on</th>
  <th>Limitation on</th>
 </tr>
 <tr>
  <td>Format</td>
  <td>Start X Coordinate</td>
  <td>Width</td>
 </tr>
 <tr>
  <td>PSMCT32<br/>PSMZ32</td>
  <td>No limitation</td>
  <td>Multiples of 2</td>
 </tr>
 <tr>
  <td>PSMCT24<br/>PSMZ24</td>
  <td>No limitation</td>
  <td>Multiples of 8</td>
 </tr>
 <tr>
  <td>PSMCT16<br/>PSMCT16S<br/>PSMZ16<br/>PSMZ16S</td>
  <td>No limitation</td>
  <td>Multiples of 4</td>
 </tr>
 <tr>
  <td>PSMT8<br/>PSMT8H</td>
  <td>Multiples of 2</td>
  <td>Multiples of 8</td>
 </tr>
 <tr>
  <td>PSMT4<br/>PSMT4HL<br/>PSMT4HH</td>
  <td>Multiples of 4</td>
  <td>Multiples of 8</td>
 </tr>
</table>


- © SCEI

- 76-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 4.2. Transmission Process


## 4.2.1. Transmission from Host to Local Buffer


Transmission of image data from host to local buffer is performed by writing data to the transmission data
register (HWREG) after setting the parameters described above.
Data is written to HWREG in 64-bit units. During data transmission, it is possible to access other registers and
to issue drawing primitives. It is guaranteed that in the GS, transmission data will not overtake a drawing
primitive that was issued before setting the transmission direction setting register (TRXDIR).
If the TRXDIR has been accessed before transmission is ended (before a fixed amount of data are written to
HWREG), transmission is ended, and a new transmission is started using the current values of BITBLTBUF,
TRXPOS and TRXREG.
To perform drawing using the transmitted data as a texture immediately after data transmission, it is necessary to
access the TEXFLUSH register before primitive drawing because the order of writing to local memory is not
guaranteed.


## 4.2.2. Transmission from Local Buffer to Host


<table>
 <tr>
  <th></th>
  <td>data from local buffer to host, it is to switch the transmission directions of</td>
 </tr>
 <tr>
  <th>the</th>
  <td>between the GS and host after checking the GS input FIFO is empty. The process is as follows:</td>
 </tr>
 <tr>
  <th>1.</th>
  <td>Set the transmission "4.1. Transmission</td>
 </tr>
 <tr>
  <th>2.</th>
  <td>Access the FINISH event occurs when data is to the</td>
 </tr>
 <tr>
  <th>3.</th>
  <td>Wait until the FINISH field of the CSR becomes 1.</td>
 </tr>
 <tr>
  <th>4.</th>
  <td>Set the FINISH field of the CSR to 0 and clear the event.</td>
 </tr>
 <tr>
  <th>5.</th>
  <td>Write 1 to the BUSDIR</td>
 </tr>
 <tr>
  <th></th>
  <td>transmission direction between the GS and host is</td>
 </tr>
 <tr>
  <th>6.</th>
  <td>Read data from Host interface.</td>
 </tr>
 <tr>
  <th>7.</th>
  <td>Write 0 to the BUSDIR after all data.</td>
 </tr>
 <tr>
  <th></th>
  <td>transmission direction is to the</td>
 </tr>
</table>


The host interface has a built-in FIFO for access to the general-purpose port register, and is usually put in the
direction from the host to the inside of the GS. It is necessary to use this FIFO in the reverse direction only in
the image transmission from the local buffer to the host. The general-purpose port register cannot be accessed
while the FIFO is in the reverse direction. However, the privileged port register can be accessed.
The Host FIFO requires the total data size of Local-Host transmission to be multiples of 128 bytes during DMA
transmission and multiples of 16 bytes during IO transmission.


## 4.2.3. Transmission from Local Buffer to Local Buffer


Image data transmission from local buffer to local buffer starts when the above-mentioned parameters are set
and the TRXDIR register is accessed.
per pixel (pixel
depth).
pixel depth
pixel storage
pixel


- 77-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


problem may
performing drawing immediately
performing
image
pixel
pixel
recently
during
image
page,
page
(equivalent
0).
phenomenon
present
problems
processing patterns.
transmitting image
drawing
primitive (using
buffer)
following sequence
problem:
image
page
dummy
Begin drawing
page
preventing
operation
alpha
by drawing
dummy primitive (such
primitive).
changing pixel
may
problem.


- © SCEI

- 78-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 4.3. Transmission Data Format


In transmitting the image data, the data is packed according to the following data format. Even if it is a
transmission to a buffer with a storage format that leaves blanks such as PSMCT24 and PSMT8H, padding is
unnecessary and the data packed as below can be transmitted.
Packed data is read after the transmission from local buffer to host.


## RGBA32/TEX32/Z32


63 55 47 39


<table>
 <tr>
  <th>Texel 1 Texel 0<br/>A1 B1 G1 R1 A0 B0 G0 R0</th>
 </tr>
 <tr>
  <td>Texel 3 Texel 2<br/>A3 B3 G3 R3 A2 B2 G2 R2</td>
 </tr>
 <tr>
  <td>RGB32/TEX24/Z24<br/>63 55 47 39 31 23 15 7 0</td>
 </tr>
 <tr>
  <td>Texel 2 Texel 1 Texel 0</td>
 </tr>
 <tr>
  <td>G2 R2 B1 G1 R1 B0 G0 R0</td>
 </tr>
 <tr>
  <td>Texel 4 Texel 3<br/>R5 B4 G4 R4 B3 G3 R3 B2</td>
 </tr>
 <tr>
  <td>RGBA16/TEX16/Z16<br/>63 47 31 15 0</td>
 </tr>
 <tr>
  <td>Texel 3 Texel 2 Texel 1 Texel 0<br/>A B3 G3 R3 A B2 G2 R2 A B1 G1 R1 A B0 G0 R0</td>
 </tr>
 <tr>
  <td>Texel 7 Texel 6 Texel 5 Texel 4<br/>A B7 G7 R7 A B6 G6 R6 A B5 G5 R5 A B4 G4 R4</td>
 </tr>
 <tr>
  <td>IDTEX8<br/>63 55 47 39 31 23 15 7 0</td>
 </tr>
 <tr>
  <td>Texel 7 Texel 6 Texel 5 Texel 4 Texel 3 Texel 2 Texel 1 Texel 0</td>
 </tr>
 <tr>
  <td>Texel 15 Texel 14 Texel 13 Texel 12 Texel 11 Texel 10 Texel 9 Texel 8</td>
 </tr>
 <tr>
  <td>IDTEX4<br/>63 59 55 51 ..... 15 11 7 3</td>
 </tr>
 <tr>
  <td>T15 T14 T13 T12 ..... T3 T2 T1 T0</td>
 </tr>
 <tr>
  <td>T31 T30 T29 T28 ..... T19 T18 T17 T16</td>
 </tr>
</table>


31


23


15


7


0


- 79-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


(This page is left blank intentionally)


- © SCEI

- 80-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 5. CRTC


- 81-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 5.1. CRTC Function


## 5.1.1. PCRTC Features


The GS's video output is performed in the PCRTC block as shown in Figure 5-1. The PCRTC has the
following functions:


• Image signals can be output with video clocks conforming to VESA standard (135MHz at maximum),
NTSC, and PAL by setting the standard video clock and the CRT mode.
• There are two independent rectangular area read output circuits. Input area, input pixel format, output
resolution, output size, output screen position, etc. can be set independently. Also, two output images can
be alpha-blended with BGColor and output using the built-in merge circuit (see Figure 5-2).
• There is a rectangular area write input circuit allowing a rectangular area in the output feedback image to be
written to an area in the frame or texture buffer. Also, RGB -> YCbCr conversion can be performed at
that time.


Clock NTSC/PAL


PCRTC


Sync Generator


PLL


Output Circuit


CRT 1
(Analog)


out1


DAC


Rectangular Area Read Output
Circuit 1


Rectangular Area Read Output
Circuit 2


Input Circuit


M
U
X


Rectangular Area Write Input
Circuit


<table>
 <tr>
  <th>Memory<br/>Controller</th>
  <td colspan="2">Register</td>
 </tr>
 <tr>
  <th>Frame</th>
  <td></td>
  <td rowspan="2">Memory<br/>Controller</td>
 </tr>
 <tr>
  <th>(ASIC DRAM)</th>
  <td></td>
 </tr>
</table>


Figure 5-1 PCRTC Block Diagram


The following is an example of PCRTC data flow.


- © SCEI

- 82-


Merge Circuit




SCE CONFIDENTIAL GS User's Manual Version 6.0


<table>
 <tr>
  <th>Sync Gen.</th>
  <td></td>
 </tr>
 <tr>
  <th colspan="2"></th>
 </tr>
 <tr>
  <th>Area Read<br/>Circuit 1</th>
  <td>Output</td>
 </tr>
 <tr>
  <th colspan="2">Circuit Area Read 2 Output</th>
 </tr>
 <tr>
  <th>1</th>
  <td></td>
 </tr>
 <tr>
  <th colspan="2">Area Write Input<br/>Circuit</th>
 </tr>
</table>


NTSC/PAL/VESA


<table>
 <tr>
  <th></th>
  <th colspan="2">Area Read</th>
 </tr>
 <tr>
  <td>0 1</td>
  <td></td>
  <td>Circuit 1<br/>Rectangular Area Read<br/>Circuit 2<br/>1<br/>Rectangular Area<br/>Circuit</td>
 </tr>
</table>


Frame Buffer


Merge Circuit


1


out1


CRT1
Output


out2


Figure 5-2 PCRTC Merge Circuit


As shown in the above figure, two rectangular area read output circuits read rectangular data in different sizes
and different pixel formats located in different areas of the frame buffer, arrange the data to arbitrary positions
with different resolutions, and output to the merge circuit. In the merge circuit, two output images are blended
at the set alpha value and are output as the CRT1 image.
In the above figure, the rectangular area read output circuit 1 controls the high-resolution text image in a single
buffer, and rectangular area read output circuit 2 controls the three-dimensional polygon image with a Z buffer
by RGBA 32-bit dual buffering. The two images are blended in the merge circuit, and a blended image of the
high-resolution text image and 24-bit full color three-dimensional image can be seen.
The rectangular write circuit can write a specified rectangular area in the CRT output image to a specified area in
the frame buffer at an arbitrary sampling rate (feedback write).


- 83-

- © SCEI




SCE CONFIDENTIAL


GS User's Manual Version 6.0


## 5.1.2. Corresponding Video Signal


The following are the display frequencies and typical resolutions that the GS supports.


<table>
 <tr>
  <th>Mode</th>
  <th>Resolution</th>
  <th>fv(Hz)</th>
  <th>fH(kHz)</th>
  <th>Remarks</th>
 </tr>
 <tr>
  <td>NTSC*</td>
  <td rowspan="2">256 x 448(224)<br/>320 x 448(224)<br/>384 x 448(224)<br/>512 x 448(224)<br/>640 x 448(224)</td>
  <td rowspan="2">59.940<br/>(59.82)</td>
  <td rowspan="2">15.734</td>
  <td rowspan="2">( ): Resolution in non-<br/>interlaced scanning</td>
 </tr>
 <tr>
  <td></td>
 </tr>
 <tr>
  <td>PAL*</td>
  <td rowspan="2">256 x 512(256)<br/>320 x 512(256)<br/>384 x 512(256)<br/>512 x 512(256)<br/>640 x 512(256)</td>
  <td rowspan="2">50.000<br/>(49.76)</td>
  <td rowspan="2">15.625</td>
  <td rowspan="2">( ): Resolution in non-<br/>interlaced scanning</td>
 </tr>
 <tr>
  <td></td>
 </tr>
 <tr>
  <td>VESA</td>
  <td>640 x 480<br/>800 x 600<br/>1024 x 768<br/>1280 x 1024</td>
  <td>59.940<br/>72.809<br/>75.000<br/>85.008<br/>56.250<br/>60.317<br/>72.188<br/>75.000<br/>85.061<br/>60.004<br/>70.069<br/>75.029<br/>84.997<br/>60.020<br/>75.025</td>
  <td>31.469<br/>37.861<br/>37.500<br/>43.269<br/>35.156<br/>37.879<br/>48.077<br/>46.875<br/>53.674<br/>48.363<br/>56.476<br/>60.023<br/>68.677<br/>63.981<br/>79.976</td>
  <td>Frequencies may vary<br/>within VESA standard<br/>(+/- 0.5%).<br/>Frequencies may vary<br/>within VESA standard<br/>(+/- 0.5%).<br/>Frequencies may vary<br/>within VESA standard<br/>(+/- 0.5%).<br/>Frequencies may vary<br/>within VESA standard<br/>(+/- 0.5%).</td>
 </tr>
 <tr>
  <td>DTV</td>
  <td>720 x 480</td>
  <td>59.94</td>
  <td>31.469</td>
  <td>480 P</td>
 </tr>
 <tr>
  <td></td>
  <td>1920 x 1080</td>
  <td>60.00</td>
  <td>33.750</td>
  <td>1080 I</td>
 </tr>
</table>


*Resolutions in NTSC and PAL modes are recommended to realize


the appropriate screen size.


- © SCEI

- 84-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 5.2. Video Output


## 5.2.1. Video Clock


Besides the system clock, the GS internally generates the standard video clock VCK, which defines the PCRTC
operation timing. The main settings related to the display area on the screen are done in VCK units. The VCK
frequency predetermined for each video mode is set by the kernel service.


## 5.2.2. Rectangular Area Read Output Circuit


This circuit reads the specified rectangular area in the frame buffer adjusting to CRT scanning, and outputs the
converted data to the display coordinates. It can display the contents of the read rectangular area magnified by
the specified coefficients (MAGH/MAGV).
The figure below shows the operation of this circuit.


Local Memory


FBP


DBY


Frame Memory


<table>
 <tr>
  <th>DBX</th>
  <th></th>
 </tr>
 <tr>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td colspan="2">FBW</td>
 </tr>
</table>


<table>
 <tr>
  <th></th>
 </tr>
 <tr>
  <th>X MAGV</th>
 </tr>
</table>


DX


Display
Coordinates


<table>
 <tr>
  <th colspan="2">DW+1</th>
 </tr>
 <tr>
  <td>DH+1</td>
  <td></td>
 </tr>
</table>


Figure 5-3 Read from Frame Buffer to PCRTC


The PCRTC is equipped with two independent rectangular area read output circuits. Each of them
independently has a DISPFB1/2 register (FBP, FBW, DBX, DBY, PSM) and DISPLAY1/2 register (DH, DW,
DX, DY, MAGH, MAGV), where the parameters in the above figure and the input pixel format can be set. The
two circuits function almost the same, but differ in the merging order and some of the input pixel format
settings in the merge circuit (described later in this document).
The unit of horizontal coefficients, DX, DW, and MAGH, which are related to the display coordinates, is VCK
(sub-pixel).


- 85-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## MAGH/MAGV


MAGH and MAGV decide the magnification of width and height respectively. MAGV actually decides the
magnification to the scanning line, and MAGH decides the magnification in units of sub-pixels (VCK).
The values of MAGH to obtain typical resolutions in NTSC/PAL mode are shown below.


<table>
 <tr>
  <th colspan="2">Horizontal Resolution (Pixel) MAGH Set Value<br/>256 9</th>
 </tr>
 <tr>
  <td>320</td>
  <td>7</td>
 </tr>
 <tr>
  <td>384</td>
  <td>6</td>
 </tr>
 <tr>
  <td>512</td>
  <td>4</td>
 </tr>
 <tr>
  <td>640</td>
  <td>3</td>
 </tr>
</table>


Other free resolutions can be set according to the value of MAGH.
The relation between the display pixels, DX and MAGH is as follows.


## MAGH


## VCK


DY


DX


MAGV


<table>
 <tr>
  <th></th>
  <th></th>
  <th></th>
 </tr>
</table>


DW


Figure 5-4 Relation between Display Pixels, DX and MAGH


## Read Format


The format of data read from the frame buffer by the rectangular area read output circuit can be selected
from the following four kinds, depending on the PSM field of the DISPFB register.


- © SCEI

- 86-




SCE CONFIDENTIAL GS User's Manual Version 6.0


PSMCT16/16S


15 10
31 26
A B


<table>
 <tr>
  <th></th>
  <th>B</th>
  <th>G</th>
  <th>R</th>
 </tr>
 <tr>
  <td>24</td>
  <td>16</td>
  <td>8</td>
  <td>0</td>
 </tr>
 <tr>
  <td>A</td>
  <td>B</td>
  <td>G</td>
  <td>R</td>
 </tr>
 <tr>
  <td>24</td>
  <td>16</td>
  <td>8</td>
  <td>0</td>
 </tr>
 <tr>
  <td>R1</td>
  <td>B0</td>
  <td>G0</td>
  <td>R0</td>
 </tr>
 <tr>
  <td>G2</td>
  <td>R2</td>
  <td>B1</td>
  <td>G1</td>
 </tr>
 <tr>
  <td>B3</td>
  <td>G3</td>
  <td>R3</td>
  <td>B2</td>
 </tr>
</table>


8
21


0
16


G


R


PSMCT24


24


16


8


0


PSMCT32


PS-GPU24


*Rectangular area read output circuit 2 does not support the PS-GPU24 format.
*Rectangular area setting of the PS-GPU24 should be the same as that of the PSMCT16.


## Interlace Read


In interlace mode, the reading method in the vertical direction differs depending on the value of the FFMD
field in the SMODE 2 register as follows.


0: FIELD Mode Read every other line from the start


1: FRAME Mode Read every line from the start


(+0,+2,+4.../+1,+3,+5...)
(+0,+1,+2,+3,+4,+5....)


## 5.2.3. Merge Circuit


The merge circuit synthesizes two image outputs of rectangular area read output circuits 1/2 and BGColor
(background color) and obtains two different output images, as shown in Figure 5-5.
MMOD AMOD
ALP


M
U
X


α


Merge Circuit


out1


xα


+


x (1 -α)


enable Alpha-Blending Circuit


out2


M
U
X


M
U
X


CRT 1
D (Analog)
A
C


Rectangular
Area
Output
NO.1


Rectangular
Area
Output
NO.2


BGColor


SLBG Rectangular Area Write Circuit


Figure 5-5 PCRTC Merge Circuit


- 87-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## OUT 2 Output


The output image of rectangular area read circuit 2 is output to OUT2. The BGColor register value is output
outside the rectangular area or when rectangular area read circuit 2 is in the enable-off state.
The alpha value from rectangular area read circuit 2 is output within the rectangular area and 0x00 is output
outside the area.


## OUT 1 Output


An alpha-blended image of the output image of rectangular area read circuit 1 and OUT2 or BGColor is
output to OUT1. OUT2 and BGColor is selected by the value of the SLBG field of the PMODE register.


SLBG=0: OUT2
SLBG=1: BGColor


The alpha used for blending is selected according to the value of the MMOD field of the PMODE register,
as follows.


MMOD=0: Alpha value from rectangular area read circuit 1*
MMOD=1: Value of ALP field in PMODE register


* When the alpha value from rectangular area read circuit 1 is selected, the alpha in the frame buffer is set


based on 0x80 =1.0. The value doubled first and then clamped with 0xff is used for blending.


The alpha output to OUT1 is selected according to the value of the AMOD field of the PMODE register, as
follows.


AMOD=0: Alpha value from rectangular area read circuit 1
AMOD=1: Alpha value from rectangular area read circuit 2


Assuming that the output of rectangular area read circuit 1 is Rs, Gs, and Bs, the output of rectangular area
read circuit 2/BGColor is Rd, Gd, and Bd, and the alpha value is A, the output (Ro,Go,Bo) of OUT1 is
calculated as follows:


Ro = Rs x A + Rd x (0xff - A)
Go = Rs x A + Gd x (0xff - A)
Bo = Rs x A + Bd x (0xff - A)
The above results are multiplied by 256(0x100) / 255(0xff).


## Data Format and Alpha Value


The Alpha value is handled as follows, based on the format of the data read from the frame buffer.
PSMCT32: 8-bit alpha value in 32-bit color is used.
PSMCT24: 0x80 is used.
PSMCT16: 0x80 is used if 1-bit alpha value in 16-bit color is 1.
0x00 is used if 1-bit alpha value in 16-bit color is 0.
PS-GPU24: 0x80 is used.


- © SCEI

- 88-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## Color Processing in PSMCT16(S) Mode


Since only 5 bits each are given to color data R, G and B in the PSMCT16(S) mode, the color is expanded to
8 bits.


bit4 bit3 bit2 bit1 bit0


Input Luminance
Value (5bits)


E DCBA


Output Luminance
Value (8bits)


E DCBA 000


bit7 bit6 bit5 bit4 bit3 bit2 bit1 bit0


- 89-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 5.3. Feedback Write


## 5.3.1. Rectangular Area Write Input Circuit


This circuit can write a specified rectangular area in the output feedback image to an arbitrary position in a local
buffer at an arbitrary sampling rate, as shown in the figure below.
The writing format is fixed to RGBA 32-bit (PSMCT32).
When inputting data, RGB→YCbCr conversion can be made.


Local Memory


EXBP


WDY


Frame Buffer
WDX


<table>
 <tr>
  <th colspan="2">WW+1</th>
 </tr>
 <tr>
  <td>WH+1</td>
  <td></td>
 </tr>
</table>


EXBW


SX Feedback Input Image


<table>
 <tr>
  <th colspan="2">(WW+1)*SMPH</th>
 </tr>
 <tr>
  <th>(WH+1)<br/>*SMPV</th>
  <td></td>
 </tr>
</table>


SY


1 / SMPH
1 / SMPV


SX


SY


Captured/SMPV


Captured/SMPH


Figure 5-6 Write from PCRTC to Frame Buffer


The rectangular area write input circuit can set the parameters shown in the figure above by means of the
EXTBUF register (EXBP, EXBW, WDX, WDY, FBIN, WFFMD, EMODA, EMODC) and the EXTDATA
register (SX, SY, WH, WW, SMPH, SMPV).


The unit of horizontal coefficients SX and SMPH, which relate to the input image, is VCK (sub-pixel).


## Selection of Input Image


The input image of the rectangular area write input circuit is selected according to the value of the FBIN
field in the EXTBUF register, as follows:


FBIN=0: OUT1 (Synthetic Image of Rectangular Area Read Circuits 1 and 2)
FBIN=1: OUT2 (Image of Rectangular Area Read Circuit 2)


- © SCEI

- 90-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## Processing at Data Write


The input Alpha data and color data perform the following processing according to the value of the
EMODA/EMODC field in the EXTBUF register.


Alpha Value Write Mode Setting (EMODA)
EMODA =0: A (Input Alpha Value)
EMODA =1: Y (Conversion Value of Input RGB)
EMODA =2: Y/2 (Conversion Value of Input RGB)
EMODA =3: 0


Color Value Write Mode Setting (EMODC)
EMODC =0: (R,G,B) (Direct Input of Input RGB)
EMODC =1: (Y,Y,Y) (Conversion Value of Input RGB: Y for All RGB)
EMODC =2: (Y,Cb,Cr) (YCbCr Conversion Value of Input RGB)
EMODC =3: (A, A, A) (Input Alpha Value: Alpha for All RGB)


Calculation of RGB->YCbCr conversion is as below:
Ey = 0.587 x Eg + 0.114 x Eb + 0.299 x Er
Ecb = -0.311 x Eg + 0.500 x Eb - 0.169 x Er
Ecr = -0.419 x Eg - 0.081 x Eb + 0.500 x Er


Er,Eg,Eb = R,G,B(0~0xff) / 255(0xff)


Y = (DBh) x Ey + (0x10)
Cr = (E0h) x Ecb + (0x80)
Cb = (E0h) x Ecr + (0x80)


Write Processing in Interlace Mode


For the input image of the rectangular area write input circuit, the writing method is decided depending on
the value of the WFFMD field of the EXTBUF register as follows.


WFFMD =0:
FIELD: Write to every other line from the start (+0,+2,+4.. / +1,+3,+5..)
WFFMD =1:
FRAME: Write to every line from the start (+0,+1,+2,+3,+4,+5....)


## Write Command


The register EXTWRITE to execute write commands is provided in addition to the registers to set addresses
and coordinates. Write processing of the feedback image is begun by writing 1 to this register and ended by
writing 0.
If write processing of the specified rectangular area ends, an interrupt (EXTWINT field in the CSR register)
occurs.
When the system is reset and FLUSH is caused during write processing, write processing is suspended.


- 91-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


(This page is left blank intentionally)


- © SCEI

- 92-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 6. Signal


- 93-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 6.1. Outline of Signal Processing


The GS can generate a signal (an interrupt) to the host, by writing data to the signal register. By using this
function in the drawing command string, it is possible to synchronize with the progress of drawing in the host.


The GS has three kinds of signal registers. Although it asserts interrupt signals to the host when signals are
generated, it can control them individually with the Interrupt Mask Register, IMR.


- © SCEI

- 94-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 6.2. Signal Register


The three kinds of signal registers of the GS are described below.


## 6.2.1. SIGNAL Register


register,
exception
generated immediately regardless
ongoing
drawing process.
register
updated,


operation
performed
register
clearing
interrupt
interrupt
generated
stops
drawing process
operations
general-purpose registers.
drawing process
operations
general-purpose registers
flag
register
interrupt
interrupt corresponding
operation
register
by setting
flag
register
interrupt
by clearing
flag
register again.


register,
exception
generated
ongoing drawing
processes,
flag
register
flag
register
interrupt
generated
By adding
operation


register,
register
stop
drawing


successively.


## 6.2.3. LABEL Register


If data is written to the LABEL register, the value of the LABELID field of the SIGLBLID register is updated.
An interrupt is not generated.


The drawing process does not stop by writing to the LABEL register.


- 95-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


(This page is left blank intentionally)


- © SCEI

- 96-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 7. Registers


This chapter describes the functions of the registers, which are accessible from the host, and the meanings of the
fields in the registers.


- 97-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 7.1. General Purpose Registers


The general purpose registers are mainly used to set vertex information, drawing environment and transmission
between buffers for the drawing primitives. All of them are write-only registers.


Among the registers that set the drawing environment, the registers with names in the format "XXX_1,
XXX_2" are designed to have two contexts. They consist of two sets of registers with the same functions.
Registers with two contexts are as shown below:


FRAME (FRAME_1, FRAME_2)
ZBUF (ZBUF_1, ZBUF_2)
TEX0 (TEX0_1, TEX0_2)
TEX1 (TEX1_1, TEX1_2)
TEX2 (TEX2_1, TEX2_2)
MIPTBP1 (MIPTBP1_1, MIPTBP1_2)
MIPTBP2 (MIPTBP2_1, MIPTBP2_2)
CLAMP (CLAMP_1, CLAMP_2)
TEST (TEST_1, TEST_2)
ALPHA (ALPHA_1, ALPHA_2)
XYOFFSET (XYOFFSET_1, XYOFFSET_2)
SCISSOR (SCISSOR_1, SCISSOR_2)
FBA (FBA_1, FBA_2)


- © SCEI

- 98-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## Explanatory Notes


Register Name


<table>
 <tr>
  <th>FOGCOL : Fog Color</th>
 </tr>
 <tr>
  <td>0x3d<br/>This register sets fogging ........ Details of<br/>Function<br/>BIT ASSIGN<br/>6 2 1 1 0 0 0<br/>3 3 6 5 8 7 0</td>
 </tr>
 <tr>
  <td>FCB FCG FCR</td>
 </tr>
 <tr>
  <td>FIELD</td>
 </tr>
 <tr>
  <td>Name Pos. Format Contents</td>
 </tr>
 <tr>
  <td>FCR 7:0 int0:8:0 Color</td>
 </tr>
 <tr>
  <td>FCG 15:8 int0:8:0 Color</td>
 </tr>
 <tr>
  <td>FCB 23:16 int0:8:0 Fog Color (BLUE)</td>
 </tr>
</table>


Outline


Address


Field Bit Position Field Data Field Name in
Name in the Field Format the Register


## Notation of the field bit position


63:48 Bit 63 to bit 48


15 Bit 15 only


Notation of the field data format
float 32 IEEE754-based 32-bit single-precision floating-point value.
int *:*:* Integer or fixed decimal value.
"*" indicates number of sign bits, number of integer bits, and number of decimal bits, left to
right.
(Notation example of "int")
int 1:11:4 16-bit fixed decimal value with 1 bit for sign, 11 bits for integer part and 4 bits for decimal
part.
int 0:8:0 8-bit integer with no sign, 8 bits for integer part and 0 bit for decimal part.
int 0:2:0 Bit field of 2-bit width with no sign.


- 99-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## ALPHA_1 / ALPHA_2 : Alpha Blending Setting


0x42 / 0x43


These registers define the blend function of Alpha Blending. ALPHA_1 sets Context 1 and ALPHA_2 sets
Context 2.


The basic form of the blending function is as follows:
Cv = (A-B)*C>>7 + D


(Cv = Output Color Value)


(A,B,D = Input Color Value)


(C = Input Alpha Value)


Each item of A/B/C/D in the function above is set by this register.


## BIT ASSIGN


3 0
9 2 7 6 5 4 3 2 1 0


<table>
 <tr>
  <th colspan="4">FIX D C B A</th>
 </tr>
 <tr>
  <th colspan="4">FIELD</th>
 </tr>
 <tr>
  <th>Name</th>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <th>A</th>
  <td>1:0</td>
  <td>int 0:2:0</td>
  <td>Specification of Input Color Value A<br/>00 Cs RGB value of the source is used.<br/>01 Cd RGB value in the frame buffer is used.<br/>10 0<br/>11 Reserved</td>
 </tr>
 <tr>
  <th>B</th>
  <td>3:2</td>
  <td>int 0:2:0</td>
  <td>Specification of Input Color Value B<br/>00 Cs RGB value of the source is used.<br/>01 Cd RGB value in the frame buffer is used.<br/>10 0<br/>11 Reserved</td>
 </tr>
 <tr>
  <th>C</th>
  <td>5:4</td>
  <td>int 0:2:0</td>
  <td>Specification of Input Alpha Value C<br/>00 As Alpha of the source is used.<br/>01 Ad Alpha in the frame buffer is used.<br/>10 FIX FIX-field value is used as Alpha.<br/>11 Reserved</td>
 </tr>
 <tr>
  <th>D</th>
  <td>7:6</td>
  <td>int 0:2:0</td>
  <td>Specification of Input Color Value D<br/>00 Cs RGB value of the source is used.<br/>01 Cd RGB value in the frame buffer is used.<br/>10 0<br/>11 Reserved</td>
 </tr>
 <tr>
  <th>FIX</th>
  <td>39:32</td>
  <td>int 0:8:0</td>
  <td>Fixed Alpha Value<br/>Referred to when the value of C above is FIX.<br/>(The is 0 - 255, and at the 128 1.0 is indicated.)</td>
 </tr>
</table>


- © SCEI

- 100-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## BITBLTBUF : Setting for Transmission between Buffers


0x50


This register stores buffer-related settings for transmission source and destination during transmission between
buffers.


BIT ASSIGN


6 5 5 4 4 2 3 2 2 1 1 0
1 6 3 8 5 2 9 4 1 6 3 0


<table>
 <tr>
  <th>DPSM</th>
  <th>DBW</th>
  <th>DBP</th>
  <th>SPSM</th>
  <th>SBW</th>
  <th>SBP</th>
 </tr>
</table>


FIELD


<table>
 <tr>
  <th>Name</th>
  <th>Pos.</th>
  <th>Format</th>
  <th>Contents</th>
 </tr>
 <tr>
  <td>SBP</td>
  <td>13:0</td>
  <td>int 0:14:0</td>
  <td>Source Buffer Base Pointer (SBP = Word Address/64)</td>
 </tr>
 <tr>
  <td>SBW</td>
  <td>21:16</td>
  <td>int 0:6:0</td>
  <td>Source Buffer Width (SBW = Width in Units of Pixels/64) (*1)</td>
 </tr>
 <tr>
  <td>SPSM</td>
  <td>29:24</td>
  <td>int 0:6:0</td>
  <td>Source Pixel Storage Format (*2, *3)<br/>000000 PSMCT32<br/>000001 PSMCT24<br/>000010 PSMCT16<br/>001010 PCMCT16S<br/>010011 PSMT8<br/>010100 PSMT4<br/>011011 PSMT8H<br/>100100 PSMT4HL<br/>101100 PSMT4HH<br/>110000 PSMZ32<br/>110001 PSMZ24<br/>110010 PSMZ16<br/>111010 PSMZ16S</td>
 </tr>
 <tr>
  <td>DBP</td>
  <td>45:32</td>
  <td>int 0:14:0</td>
  <td>Destination Buffer Base Pointer (DBP = Word Address/64)</td>
 </tr>
 <tr>
  <td>DBW</td>
  <td>53:48</td>
  <td>int 0:6:0</td>
  <td>Destination Buffer Width (DBW = Width in Units of Pixels/64) (*1)</td>
 </tr>
 <tr>
  <td>DPSM</td>
  <td>61:56</td>
  <td>int 0:6:0</td>
  <td>Destination Pixel Storage Format (*3)<br/>(Same as Source Pixel Format)</td>
 </tr>
</table>


- *1 Values must be in the range of 1 to 32.

- *2 Operation is undefined when values other than those in the table are set.

- *3 In local-local transmission, the formats specified in the SPSM and DPSM fields must have the same number of bits per pixel (pixel depth).

- 101-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## CLAMP_1 / CLAMP_2 : Texture Wrap Mode


0x08 / 0x09


These registers set the texture's wrap mode (repeating or clamping) for both S and T.
and CLAMP_2 sets Context 2.


CLAMP_1 sets Context 1


BIT ASSIGN


3 4 2 1
3 4 3 4 3 4 3


<table>
 <tr>
  <th colspan="4">W W<br/>MAXV MINV MAXU MINU M M<br/>T S</th>
 </tr>
 <tr>
  <th colspan="4">FIELD</th>
 </tr>
 <tr>
  <th>Name</th>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <th>WMS</th>
  <td>1:0</td>
  <td>int 0:2:0</td>
  <td>Wrap Mode in Horizontal (S) Direction<br/>00 REPEAT<br/>01 CLAMP<br/>10 REGION_CLAMP<br/>11 REGION_REPEAT</td>
 </tr>
 <tr>
  <th>WMT</th>
  <td>3:2</td>
  <td>int 0:2:0</td>
  <td>Wrap Mode in Vertical (T) Direction<br/>00 REPEAT<br/>01 CLAMP<br/>10 REGION_CLAMP<br/>11 REGION_REPEAT</td>
 </tr>
 <tr>
  <th>MINU</th>
  <td>13:4</td>
  <td>int 0:10:0</td>
  <td>Clamp Parameter in U Direction (*1)<br/>(In REGION_CLAMP Mode)<br/>Clamp Value of Lower Limit<br/>(In REGION_REPEAT Mode)<br/>UMSK Value</td>
 </tr>
 <tr>
  <th>MAXU</th>
  <td>23:14</td>
  <td>int 0:10:0</td>
  <td>Clamp Parameter in U Direction (*1)<br/>(In REGION_CLAMP Mode)<br/>Clamp Value of Upper Limit<br/>(In REGION_REPEAT Mode)<br/>UFIX Value</td>
 </tr>
 <tr>
  <th>MINV</th>
  <td>33:24</td>
  <td>int 0:10:0</td>
  <td>Clamp Parameter in V Direction (*2)<br/>(In REGION_CLAMP Mode)<br/>Clamp Value of Lower Limit<br/>(In REGION_REPEAT Mode)<br/>VMSK Value</td>
 </tr>
 <tr>
  <th>MAXV</th>
  <td>43:34</td>
  <td>int 0:10:0</td>
  <td>Clamp Parameter in V Direction (*2)<br/>(In REGION_CLAMP Mode)<br/>Clamp Value of Upper Limit<br/>(In REGION_REPEAT Mode)<br/>VFIX Value</td>
 </tr>
</table>


0 0 0
4 3 2 1 0


MAXU are not used when the value of WMS, the wrap
CLAMP. (Don't Care.)
and MAXV are not used when the value of WMT, the wrap
REPEAT or CLAMP. (Don't Care.)


- *1 Fields MINU and REPEAT or

- *2 Fields MINV


mode in horizontal direction, is


mode in vertical direction, is


- © SCEI

- 102-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## COLCLAMP : Color Clamp Control


0x46


This register stores settings as to whether clamping for the RGB value of the pixel is performed.


BIT ASSIGN


0
0


<table>
 <tr>
  <th></th>
  <th></th>
  <th>C</th>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td>L</td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td>A</td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td>M</td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td>P</td>
 </tr>
 <tr>
  <td>FIELD</td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td>Name Pos. Format</td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td>CLAMP 0 int 0:1:0<br/>0</td>
  <td>Clamping<br/>MASK</td>
  <td>Lower 8 bits are enabled (wraps around.)</td>
 </tr>
 <tr>
  <td>1</td>
  <td>CLAMP</td>
  <td>Clamped in [0,255] range.</td>
 </tr>
</table>


- 103-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## DIMX : Dither Matrix Setting


0x44


This register sets the value of the dither matrix.


BIT ASSIGN


<table>
 <tr>
  <th>6 6</th>
  <td>5 5</td>
  <td>5 5</td>
  <td>5 4</td>
  <td>4 4</td>
  <td>4 4</td>
  <td>3 3</td>
  <td>3 3</td>
 </tr>
 <tr>
  <th>2 0</th>
  <td>8 6</td>
  <td>4 2</td>
  <td>0 8</td>
  <td>6 4</td>
  <td>2 0</td>
  <td>8 6</td>
  <td>4 2</td>
 </tr>
 <tr>
  <th>DM</th>
  <td>DM</td>
  <td>DM</td>
  <td>DM</td>
  <td>DM</td>
  <td>DM</td>
  <td>DM</td>
  <td>DM</td>
 </tr>
 <tr>
  <th>33</th>
  <td>32</td>
  <td>31</td>
  <td>30</td>
  <td>23</td>
  <td>22</td>
  <td>21</td>
  <td>20</td>
 </tr>
</table>


<table>
 <tr>
  <th>3 2</th>
  <td>2 2</td>
  <td>2 2</td>
  <td>1 1</td>
  <td>1 1</td>
  <td>1 0</td>
  <td>0 0 0 0</td>
 </tr>
 <tr>
  <th>0 8</th>
  <td>6 4</td>
  <td>2 0</td>
  <td>8 6</td>
  <td>4 2</td>
  <td>0 8</td>
  <td>6 4 2 0</td>
 </tr>
 <tr>
  <th>DM</th>
  <td>DM</td>
  <td>DM</td>
  <td>DM</td>
  <td>DM</td>
  <td>DM</td>
  <td>DM DM</td>
 </tr>
 <tr>
  <th>13</th>
  <td>12</td>
  <td>11</td>
  <td>10</td>
  <td>03</td>
  <td>02</td>
  <td>01 00</td>
 </tr>
</table>


## FIELD


<table>
 <tr>
  <th>Name</th>
  <th>Pos.</th>
  <th>Format</th>
  <th>Contents</th>
 </tr>
 <tr>
  <td>DM00</td>
  <td>2:0</td>
  <td>int 1:2:0</td>
  <td>Dither Matrix (0,0)</td>
 </tr>
 <tr>
  <td>DM01</td>
  <td>6:4</td>
  <td>int 1:2:0</td>
  <td>Dither Matrix (0,1)</td>
 </tr>
 <tr>
  <td>DM02</td>
  <td>10:8</td>
  <td>int 1:2:0</td>
  <td>Dither Matrix (0,2)</td>
 </tr>
 <tr>
  <td>DM03</td>
  <td>14:12</td>
  <td>int 1:2:0</td>
  <td>Dither Matrix (0,3)</td>
 </tr>
 <tr>
  <td>DM10</td>
  <td>18:16</td>
  <td>int 1:2:0</td>
  <td>Dither Matrix (1,0)</td>
 </tr>
 <tr>
  <td>DM11</td>
  <td>22:20</td>
  <td>int 1:2:0</td>
  <td>Dither Matrix (1,1)</td>
 </tr>
 <tr>
  <td>DM12</td>
  <td>26:24</td>
  <td>int 1:2:0</td>
  <td>Dither Matrix (1,2)</td>
 </tr>
 <tr>
  <td>DM13</td>
  <td>30:28</td>
  <td>int 1:2:0</td>
  <td>Dither Matrix (1,3)</td>
 </tr>
 <tr>
  <td>DM20</td>
  <td>34:32</td>
  <td>int 1:2:0</td>
  <td>Dither Matrix (2,0)</td>
 </tr>
 <tr>
  <td>DM21</td>
  <td>38:36</td>
  <td>int 1:2:0</td>
  <td>Dither Matrix (2,1)</td>
 </tr>
 <tr>
  <td>DM22</td>
  <td>42:40</td>
  <td>int 1:2:0</td>
  <td>Dither Matrix (2,2)</td>
 </tr>
 <tr>
  <td>DM23</td>
  <td>46:44</td>
  <td>int 1:2:0</td>
  <td>Dither Matrix (2,3)</td>
 </tr>
 <tr>
  <td>DM30</td>
  <td>50:48</td>
  <td>int 1:2:0</td>
  <td>Dither Matrix (3,0)</td>
 </tr>
 <tr>
  <td>DM31</td>
  <td>54:52</td>
  <td>int 1:2:0</td>
  <td>Dither Matrix (3,1)</td>
 </tr>
 <tr>
  <td>DM32</td>
  <td>58:56</td>
  <td>int 1:2:0</td>
  <td>Dither Matrix (3,2)</td>
 </tr>
 <tr>
  <td>DM33</td>
  <td>62:60</td>
  <td>int 1:2:0</td>
  <td>Dither Matrix (3,3)</td>
 </tr>
</table>


- © SCEI

- 104-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## DTHE : Dither Control


0x45


This register stores settings for dithering (performed/not performed).
If the pixel storage mode of the frame buffer is PSMCT32 or PSMCT24, dithering should be turned off.


BIT ASSIGN


0
0
H


<table>
 <tr>
  <th></th>
  <th></th>
  <th>D</th>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td>T</td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td>E</td>
 </tr>
 <tr>
  <td>FIELD</td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td>Name Pos.</td>
  <td>Format</td>
  <td></td>
 </tr>
 <tr>
  <td>DTHE 0</td>
  <td>int 0:1:0<br/>0</td>
  <td>Control<br/>Dithering is not performed.</td>
 </tr>
 <tr>
  <td></td>
  <td>1</td>
  <td>Dithering is performed.</td>
 </tr>
</table>


- 105-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## FBA_1 / FBA_2 : Alpha Correction Value


0x4a / 0x4b


These registers set the fixed value of Alpha when writing to the frame buffer. FBA_1 sets Context 1 and FBA_2
sets Context 2.


The OR of the fixed value set by this register and the most significant bit of the Alpha value of the pixel is
written to the frame buffer.


BIT ASSIGN


0
0


<table>
 <tr>
  <th></th>
  <td></td>
  <td></td>
  <td></td>
  <td>F</td>
 </tr>
 <tr>
  <th></th>
  <td></td>
  <td></td>
  <td></td>
  <td>B</td>
 </tr>
 <tr>
  <th></th>
  <td></td>
  <td></td>
  <td></td>
  <td>A</td>
 </tr>
 <tr>
  <th>FIELD</th>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th>Name</th>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
  <td></td>
 </tr>
 <tr>
  <th>FBA</th>
  <td>0</td>
  <td>int 0:1:0</td>
  <td>MSB of value for</td>
  <td>to the frame buffer.</td>
 </tr>
 <tr>
  <th></th>
  <td></td>
  <td></td>
  <td>RGBA32 Mode:</td>
  <td>A = As</td>
 </tr>
 <tr>
  <th></th>
  <td></td>
  <td></td>
  <td>RGBA16 Mode:</td>
  <td>A = As | FBA&amp;0x01</td>
 </tr>
</table>


- © SCEI

- 106-




SCE CONFIDENTIAL GS User's Manual Version 6.0


<table>
 <tr>
  <th>FINISH : FINISH Event Occurrence Request</th>
 </tr>
 <tr>
  <th>0x61</th>
 </tr>
 <tr>
  <th>When the drawing process currently being performed is completed, this register makes a request for the</th>
 </tr>
 <tr>
  <th>occurrence of the FINISH event. Any data can be written.</th>
 </tr>
 <tr>
  <th>BIT ASSIGN</th>
 </tr>
</table>


- 107-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## FOG : Vertex Fog Value Setting


0x0a


This register sets the fog value of the vertex. When the fog value is 0, the fog effect is at the maximum (the
drawing color becomes the same as the distant color). When the fog value is 255, the fog effect is at the
minimum.
To completely eliminate the fog effect, fogging should be set off with the FGE flag of the PRIM register.


BIT ASSIGN


6 5
3 6


<table>
 <tr>
  <th>F</th>
  <td></td>
 </tr>
 <tr>
  <th colspan="2">FIELD</th>
 </tr>
 <tr>
  <th>Name</th>
  <td>Pos. Format Contents</td>
 </tr>
 <tr>
  <th>F</th>
  <td>63:56 int0:8:0 Fog Value</td>
 </tr>
</table>


- © SCEI

- 108-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## FOGCOL : Distant Fog Color Setting


0x3d


This register sets the distant color with RGB value when performing fogging.


BIT ASSIGN


1 1 0 2 0
3 6 5 8 7 0


<table>
 <tr>
  <th>FCB FCG FCR</th>
 </tr>
 <tr>
  <td></td>
 </tr>
 <tr>
  <td>Name Pos. Format Contents</td>
 </tr>
 <tr>
  <td>FCR 7:0 int 0:8:0 Fog Color (R)</td>
 </tr>
 <tr>
  <td>FCG 15:8 int 0:8:0 Fog Color (G)</td>
 </tr>
 <tr>
  <td>FCB 23:16 int 0:8:0 Color</td>
 </tr>
</table>


- 109-

- © SCEI




SCE CONFIDENTIAL


GS User's Manual Version 6.0


## FRAME_1 / FRAME_2 : Frame Buffer Setting


0x4c / 0x4d


These registers store various settings related to the frame buffer. FRAME_1 sets Context 1 and FRAME_2 sets
Context 2.


BIT ASSIGN


6
3


2 2 2 3 1 0 0
2 9 4 1 6 8 0


FBMSK


PSM FBW


FBP


FIELD


<table>
 <tr>
  <th>Name</th>
  <th>Pos.</th>
  <th>Format</th>
  <th>Contents</th>
 </tr>
 <tr>
  <td>FBP</td>
  <td>8:0</td>
  <td>int0:9:0</td>
  <td>Frame Buffer Base Pointer<br/>FBP = Word Address/2048</td>
 </tr>
 <tr>
  <td>FBW</td>
  <td>21:16</td>
  <td>int0:6:0</td>
  <td>Frame Buffer Width<br/>FBW = Width in Units of Pixels/64<br/>The effective of set value is 1 to 32</td>
 </tr>
 <tr>
  <td>PSM</td>
  <td>29:24</td>
  <td>int0:6:0</td>
  <td>Frame Buffer Pixel Storage Format<br/>000000 PSMCT32<br/>000001 PSMCT24<br/>000010 PSMCT16<br/>001010 PSMCT16S<br/>010011 reserved<br/>010100 reserved<br/>011011 reserved<br/>100100 reserved<br/>101100 reserved<br/>110000 PSMZ32<br/>110001 PSMZ24<br/>110010 PSMZ16<br/>111010 PSMZ16S</td>
 </tr>
 <tr>
  <td>FBMSK</td>
  <td>63:32</td>
  <td>int0:32:0</td>
  <td>Frame Buffer Drawing Mask (*)<br/>0 The corresponding bit in the frame buffer is updated.<br/>1 The corresponding bit in the frame buffer is not updated.</td>
 </tr>
</table>


* See "3.9.5. Masking". When the pixel storage format of the frame buffer is PSMCT16, PSMCT16S or


PSMZ16, note the bit relation between the mask pattern and frame buffer.


- © SCEI

- 110-




SCE CONFIDENTIAL GS User's Manual Version 6.0


<table>
 <tr>
  <th>HWREG</th>
  <th>: Data</th>
  <th>Port for</th>
  <th></th>
  <th>between Buffers</th>
 </tr>
 <tr>
  <td>This<br/>It is</td>
  <td>writes<br/>only</td>
  <td>the</td>
  <td>when host -&gt;</td>
  <td>0x54<br/>transmission between buffers is performed.<br/>registers are set effectively in terms</td>
 </tr>
 <tr>
  <td>of<br/>BIT</td>
  <td>no</td>
  <td>the<br/>is made.</td>
  <td>between</td>
  <td>is activated. When this register is written in other</td>
 </tr>
 <tr>
  <td>6</td>
  <td></td>
  <td></td>
  <td></td>
  <td>0</td>
 </tr>
 <tr>
  <td>3</td>
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
 </tr>
 <tr>
  <td>FIELD</td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td>Name</td>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
  <td></td>
 </tr>
 <tr>
  <td>DATA</td>
  <td>63:0</td>
  <td>int0:64:0</td>
  <td>Host -&gt; Local</td>
  <td>Transmission Data</td>
 </tr>
</table>


- 111-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## LABEL : LABEL Event Occurrence Request


0x62


This register updates the value of LBLID field in the SIGLBLID register. The ID field is composed of 32 bits,
and SIGLBLID is updated only in the bit position where the bit value corresponding to IDMSK is 1.


BIT ASSIGN


6 3 0
3 2 1 0


<table>
 <tr>
  <th colspan="4">IDMSK ID</th>
 </tr>
 <tr>
  <td colspan="4"></td>
 </tr>
 <tr>
  <td>Name</td>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <td>ID</td>
  <td>31:0</td>
  <td>int0:32:0</td>
  <td>Value to be written to SIGLBLID register</td>
 </tr>
 <tr>
  <td>IDMSK</td>
  <td>63:32</td>
  <td>int0:32:0</td>
  <td>Whether or not corresponding SIGLBLID bit is updated.<br/>0 Masked. (Not updated.)<br/>1 Not masked.</td>
 </tr>
</table>


- © SCEI

- 112-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## MIPTBP1_1 / MIPTBP1_2 : MIPMAP Information Setting (Level 1 to 3)


0x34 / 0x35
These registers set the buffer pointer and buffer width of Level 1 to Level 3 textures when performing
MIPMAP. MIPTBP1_1 sets Context 1 and MIPTBP1_2 sets Context 2.


BIT ASSIGN


5 5 4 3 3 2 1 1 1 0
9 4 3 0 9 4 3 0 9 4 3 0


<table>
 <tr>
  <th></th>
  <th>TBW3</th>
  <th>TBP3</th>
  <th>TBW2 TBP2 TBW1 TBP1</th>
 </tr>
 <tr>
  <td colspan="4">FIELD</td>
 </tr>
 <tr>
  <td>Name</td>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <td>TBP1</td>
  <td>13:0</td>
  <td>int0:14:0</td>
  <td>MIPMAP Level 1 Texture Base Pointer (*1)</td>
 </tr>
 <tr>
  <td>TBW1</td>
  <td>19:14</td>
  <td>int0:6:0</td>
  <td>MIPMAP Level 1 Texture Buffer Width (*2)</td>
 </tr>
 <tr>
  <td>TBP2</td>
  <td>33:20</td>
  <td>int0:14:0</td>
  <td>MIPMAP Level 2 Texture Base Pointer (*1)</td>
 </tr>
 <tr>
  <td>TBW2</td>
  <td>39:34</td>
  <td>int0:6:0</td>
  <td>MIPMAP Level 2 Texture Buffer Width (*2)</td>
 </tr>
 <tr>
  <td>TBP3</td>
  <td>53:40</td>
  <td>int0:14:0</td>
  <td>MIPMAP Level 3 Texture Base Pointer (*1)</td>
 </tr>
 <tr>
  <td>TBW3</td>
  <td>59:54</td>
  <td>int0:6:0</td>
  <td>MIPMAP Level 3 Texture Buffer Width (*2)</td>
 </tr>
</table>


- *1 The texture base pointer is set to the value of the word address divided by 64.

- *2 The buffer width is set to the value of the width (in units of texels) divided by 64. It is set to 1 if the width is


less than 64.


- 113-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## MIPTBP2_1 / MIPTBP2_2 : MIPMAP Information Setting (Level 4 to 6)


0x36 / 0x37
These registers set the buffer pointer and buffer width of Level 4 to Level 6 textures when performing
MIPMAP. MIPTBP2_1 sets Context 1 and MIPTBP2_2 sets Context 2.


BIT ASSIGN


5 3 4 33 2 1 11 0
9 4 3 0 9 4 3 0 9 4 3 0


<table>
 <tr>
  <th></th>
  <th>TBW6</th>
  <th>TBP6</th>
  <th>TBW5 TBP5 TBW4 TBP4</th>
 </tr>
 <tr>
  <td colspan="4">FIELD</td>
 </tr>
 <tr>
  <td>Name</td>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <td>TBP4</td>
  <td>13:0</td>
  <td>int0:14:0</td>
  <td>MIPMAP Level 4 Texture Base Pointer (*1)</td>
 </tr>
 <tr>
  <td>TBW4</td>
  <td>19:14</td>
  <td>int0:6:0</td>
  <td>MIPMAP Level 4 Texture Buffer Width (*2)</td>
 </tr>
 <tr>
  <td>TBP5</td>
  <td>33:20</td>
  <td>int0:14:0</td>
  <td>MIPMAP Level 5 Texture Base Pointer (*1)</td>
 </tr>
 <tr>
  <td>TBW5</td>
  <td>39:34</td>
  <td>int0:6:0</td>
  <td>MIPMAP Level 5 Texture Buffer Width (*2)</td>
 </tr>
 <tr>
  <td>TBP6</td>
  <td>53:40</td>
  <td>int0:14:0</td>
  <td>MIPMAP Level 6 Texture Base Pointer (*1)</td>
 </tr>
 <tr>
  <td>TBW6</td>
  <td>59:54</td>
  <td>int0:6:0</td>
  <td>MIPMAP Level 6 Texture Buffer Width (*2)</td>
 </tr>
</table>


- *1 The texture base pointer is set to the value of the word address divided by 64.

- *2 The buffer width is set to the value of the width (in units of texels) divided by 64. It is set to 1 if the width is


less than 64.


- © SCEI

- 114-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## PABE : Alpha Blending Control in Units of Pixels


0x49


This register sets whether Alpha Blending control in units of pixels is performed.
For details, see "3.8. Alpha Blending".


BIT ASSIGN


0
0
B


<table>
 <tr>
  <th></th>
  <th></th>
  <th></th>
  <th></th>
  <th>P</th>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td>A</td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td>E</td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td>Name</td>
  <td>Pos.</td>
  <td>Format</td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td>PABE</td>
  <td>0</td>
  <td>int0:1:0</td>
  <td></td>
  <td>Control in Units of Pixels</td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
  <td>0</td>
  <td>Not</td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
  <td>1</td>
  <td>Performed. is OFF for the where the A value</td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td>is MSB=0 and ON for the where the A value is MSB=1.)</td>
 </tr>
</table>


- 115-

- © SCEI




SCE CONFIDENTIAL


GS User's Manual Version 6.0


## PRIM : Drawing Primitive Setting


0x00


This register specifies the types of drawing primitives and various attributes, and initializes the contents of the
vertex queue.
Each of the IIP/TME/FGE/ABE/AA1/FST/CTXT/FIX fields is enabled only when AC=1 is set by the
PRMODECONT register.


1 0 0 0 0 0 0 0 0 0
0 9 8 7 6 5 4 3 2 0


<table>
 <tr>
  <th colspan="4">F T T C X S F T A A 1 E B A F E G T E M I I P PRIM<br/>I<br/>X</th>
 </tr>
 <tr>
  <th colspan="4">FIELD</th>
 </tr>
 <tr>
  <th>Name</th>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <th>PRIM</th>
  <td>2:0</td>
  <td>int0:3:0</td>
  <td>Types of Drawing Primitives<br/>000 Point<br/>001 Line<br/>010 Line Strip<br/>011 Triangle<br/>100 Triangle Strip<br/>101 Triangle Fan<br/>110 Sprite<br/>111 Prohibited</td>
 </tr>
 <tr>
  <th>IIP</th>
  <td>3</td>
  <td>int0:1:0</td>
  <td>Shading Method<br/>0 Flat Shading<br/>1 Gouraud Shading</td>
 </tr>
 <tr>
  <th>TME</th>
  <td>4</td>
  <td>int0:1:0</td>
  <td>Texture Mapping<br/>0 Texture Mapping OFF<br/>1 Texture ON</td>
 </tr>
 <tr>
  <th>FGE</th>
  <td>5</td>
  <td>int0:1:0</td>
  <td>Fogging<br/>0 Fogging OFF<br/>1 Fogging ON</td>
 </tr>
 <tr>
  <th>ABE</th>
  <td>6</td>
  <td>int0:1:0</td>
  <td>Alpha Blending<br/>0 Alpha Blending OFF<br/>1 ON</td>
 </tr>
 <tr>
  <th>AA1</th>
  <td>7</td>
  <td>int0:1:0</td>
  <td>1 Pass Antialiasing (*1)<br/>0 1 Pass Antialiasing OFF<br/>1 1 Pass Antialiasing ON</td>
 </tr>
 <tr>
  <th>FST</th>
  <td>8</td>
  <td>int0:1:0</td>
  <td>Method of Specifying Texture Coordinates (*2)<br/>0 STQ value is used (ST/RGBAQ Register is referred<br/>to.)<br/>1 UV value is used is referred</td>
 </tr>
 <tr>
  <th>CTXT</th>
  <td>9</td>
  <td>int0:1:0</td>
  <td>Context<br/>0 Environment of Context 1 is used.<br/>1 Environment of Context 2 is used.</td>
 </tr>
 <tr>
  <th>FIX</th>
  <td>10</td>
  <td>int0:1:0</td>
  <td>Fragment Value Control (RGBAFSTQ Change by DDA)<br/>0 Unfixed (Normal)<br/>1 Fixed</td>
 </tr>
</table>


BIT ASSIGN


- *1 When AA1=1 is set, note

- *2 When FST=1 is set,


(UV Register
to.)
Alpha Blending process. See "3.6. Antialiasing".
perspective correction is not performed.


- © SCEI

- 116-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## PRMODE : Setting for Attributes of Drawing Primitives


This register changes the attributes of drawing primitives. It is enabled only when AC=0 is set by the
PRMODECONT register.


BIT ASSIGN


1
0


<table>
 <tr>
  <th colspan="4">F C F A A F T I</th>
 </tr>
 <tr>
  <th colspan="4">I S A B G M I</th>
 </tr>
 <tr>
  <th colspan="4">X T T 1 E E E P</th>
 </tr>
 <tr>
  <th colspan="4">FIELD</th>
 </tr>
 <tr>
  <th>Name</th>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <th>IIP</th>
  <td>3</td>
  <td>int0:1:0</td>
  <td>Shading Method<br/>0 Flat Shading<br/>1 Gouraud Shading</td>
 </tr>
 <tr>
  <th>TME</th>
  <td>4</td>
  <td>int0:1:0</td>
  <td>Texture Mapping<br/>0 Texture Mapping OFF<br/>1 Texture ON</td>
 </tr>
 <tr>
  <th>FGE</th>
  <td>5</td>
  <td>int0:1:0</td>
  <td>Fogging<br/>0 Fogging OFF<br/>1 Fogging ON</td>
 </tr>
 <tr>
  <th>ABE</th>
  <td>6</td>
  <td>int0:1:0</td>
  <td>Alpha Blending<br/>0 Alpha Blending OFF<br/>1 ON</td>
 </tr>
 <tr>
  <th>AA1</th>
  <td>7</td>
  <td>int0:1:0</td>
  <td>1 Pass Antialiasing (*1)<br/>0 1 Pass Antialiasing OFF<br/>1 1 Pass Antialiasing ON</td>
 </tr>
 <tr>
  <th>FST</th>
  <td>8</td>
  <td>int0:1:0</td>
  <td>Method of Specifying Texture Coordinates (*2)<br/>0 STQ value (ST/RGBAQ register is referred to.)<br/>1 UV value is referred</td>
 </tr>
 <tr>
  <th>CTXT</th>
  <td>9</td>
  <td>int0:1:0</td>
  <td>Context<br/>0 Environment of Context 1 is used.<br/>1 Environment of Context 2 is used.</td>
 </tr>
 <tr>
  <th>FIX</th>
  <td>10</td>
  <td>int0:1:0</td>
  <td>Fragment Value Control (RGBAFSTQ Change by DDA)<br/>0 Unfixed (Normal)<br/>1 Fixed</td>
 </tr>
</table>


0 0 0 0 0 0 0
9 8 7 6 5 4 3
T
X


- *1 When AA1=1 is set, note Alpha Blending process. See "3.6. Antialiasing".

- *2 When FST=1 is set, perspective correction is not performed.

- 0x1b

- 117-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## PRMODECONT : Specification of Primitive Attribute Setting Method


0x1a


This register sets whether to use primitive attributes (IIP, TME, FGE, ABE, AA1, FST, CTXT, FIX) specified
by the PRMODE register or the PRIM register.


BIT ASSIGN


0
0


<table>
 <tr>
  <th></th>
  <td></td>
  <td></td>
  <td></td>
  <td>A</td>
 </tr>
 <tr>
  <th></th>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th>FIELD</th>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th>Name</th>
  <td>Pos.</td>
  <td>Format</td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th>AC</th>
  <td>0</td>
  <td>int0:1:0</td>
  <td></td>
  <td>that attributes</td>
 </tr>
 <tr>
  <th></th>
  <td></td>
  <td></td>
  <td>0</td>
  <td>PRMODE register</td>
 </tr>
 <tr>
  <th></th>
  <td></td>
  <td></td>
  <td>1</td>
  <td>PRIM</td>
 </tr>
</table>


- © SCEI

- 118-




SCE CONFIDENTIAL


GS User's Manual Version 6.0


## RGBAQ : Vertex Color Setting


0x01


This register sets the RGBA value of the vertex and the Q value of the normalized texture coordinates.
The Q value is not only used for calculating texture coordinates but also as the parameter to decide the LOD.


BIT ASSIGN


6
3


<table>
 <tr>
  <th colspan="4">Q A B G R</th>
 </tr>
 <tr>
  <th colspan="4">FIELD</th>
 </tr>
 <tr>
  <th>Name</th>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <th>R</th>
  <td>7:0</td>
  <td>int 0:8:0</td>
  <td>Luminance value of R element of vertex color</td>
 </tr>
 <tr>
  <th>G</th>
  <td>15:8</td>
  <td>int 0:8:0</td>
  <td>Luminance value of G element of vertex color</td>
 </tr>
 <tr>
  <th>B</th>
  <td>23:16</td>
  <td>int 0:8:0</td>
  <td>Luminance value of B element of vertex color</td>
 </tr>
 <tr>
  <th>A</th>
  <td>31:24</td>
  <td>int 0:8:0</td>
  <td>Alpha value of vertex (0x80 = 1.0)</td>
 </tr>
 <tr>
  <th>Q</th>
  <td>63:32</td>
  <td>float 32</td>
  <td>Normalized texture coordinates (Q) (*)</td>
 </tr>
</table>


2 3 1 0 0
2 1 4 3 6 5 8 7 0


* Negative values are acceptable. However, the vertices of one primitive must be the
the mantissa are rounded down.


same. The lower 8 bits of


- 119-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## SCANMSK : Raster Address Mask Setting


0x22


This register sets whether drawing is performed only to odd or even row by the row address of the frame buffer.
When drawing is prohibited, drawing efficiency decreases (1/2 in the worst case.)
The mask is only effective for drawing primitives.


BIT ASSIGN


0 0
1 0


<table>
 <tr>
  <th colspan="4">M<br/>S<br/>K</th>
 </tr>
 <tr>
  <th colspan="4"></th>
 </tr>
 <tr>
  <th>Name</th>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <th>MSK</th>
  <td>1:0</td>
  <td>int0:2:0</td>
  <td>00 Normal drawing (not masked.)<br/>01 Reserved.<br/>10 Drawing of pixel with even Y coordinate is prohibited.<br/>11 Drawing of pixel with odd Y coordinate is prohibited.</td>
 </tr>
</table>


- © SCEI

- 120-




SCE CONFIDENTIAL


GS User's Manual Version 6.0


## SCISSOR_1 / SCISSOR_2 : Setting for Scissoring Area


0x40 / 0x41


These registers specify the scissoring area. The coordinate values for the upper-left/lower-right points of the
enabled drawing area are specified by the window coordinate system. SCISSOR_1 sets Context 1 and
SCISSOR_2 sets Context 2.


BIT ASSIGN


5 4 4 2 3 1 0
8 8 2 2 6 6 0 0


SCAY1 SCAY0 SCAX1


SCAX0


## FIELD


<table>
 <tr>
  <th>Name</th>
  <th>Pos.</th>
  <th>Format</th>
  <th>Contents</th>
 </tr>
 <tr>
  <td>SCAX0</td>
  <td>10:0</td>
  <td>int 0:11:0</td>
  <td>X-coordinate value for upper-left point of enabled drawing area<br/>(Window coordinate system)</td>
 </tr>
 <tr>
  <td>SCAX1</td>
  <td>26:16</td>
  <td>int 0:11:0</td>
  <td>X-coordinate value for lower-right point of enabled drawing area<br/>(Window coordinate system)</td>
 </tr>
 <tr>
  <td>SCAY0</td>
  <td>42:32</td>
  <td>int 0:11:0</td>
  <td>Y-coordinate value for upper-left point of enabled drawing area<br/>(Window coordinate system)</td>
 </tr>
 <tr>
  <td>SCAY1</td>
  <td>58:48</td>
  <td>int 0:11:0</td>
  <td>Y-coordinate value for lower-right point of enabled drawing area<br/>coordinate</td>
 </tr>
</table>


- 121-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## SIGNAL : SIGNAL Event Occurrence Request


0x60


This register generates the SIGNAL event and updates the value of the SIGLBLID register. The ID field is
composed of 32 bits, and SIGLBLID is updated only in the bit position where the bit value corresponding to
IDMSK is 1.


BIT ASSIGN


6 3 0
3 2 1 0


<table>
 <tr>
  <th colspan="4">IDMSK ID</th>
 </tr>
 <tr>
  <td colspan="4"></td>
 </tr>
 <tr>
  <td>Name</td>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <td>ID</td>
  <td>31:0</td>
  <td>int0:32:0</td>
  <td>Value to be written to SIGLBLID</td>
 </tr>
 <tr>
  <td>IDMSK</td>
  <td>63:32</td>
  <td>int0:32:0</td>
  <td>Whether or not corresponding SIGLBLID bit is updated.<br/>0 Masked. (Not updated.)<br/>1 Not masked.</td>
 </tr>
</table>


- © SCEI

- 122-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## ST : Specification of Vertex Texture Coordinates


0x02


This register sets the S and T values of the vertex texture coordinates. The value Q is specified by the RGBAQ
register.
For the relation between the texture coordinate values S, T and Q and the texel, see "3.4.10. Perspective
Correction".


BIT ASSIGN


6 3 0
3 2 1 0


<table>
 <tr>
  <th colspan="4">T S</th>
 </tr>
 <tr>
  <td colspan="4"></td>
 </tr>
 <tr>
  <td>Name</td>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <td>S</td>
  <td>31:0</td>
  <td>float 32</td>
  <td>Texture Coordinate Value S (The lower 8 bits of the mantissa are<br/>rounded</td>
 </tr>
 <tr>
  <td>T</td>
  <td>63:32</td>
  <td>float 32</td>
  <td>Texture Coordinate Value T (The lower 8 bits of the mantissa are<br/>rounded</td>
 </tr>
</table>


- 123-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## TEST_1 / TEST_2 : Pixel Test Control


0x47 / 0x48


These registers perform settings related to the pixel test. TEST_1 sets Context 1 and TEST_2 sets Context 2.
For details, see "3.7. Pixel Test".


BIT ASSIGN


1 0 0 0
8 7 6 5 4 3 2 1 4 3 1 0
Z Z D D A A
T T A A F A
A AREF T T
S E T T I S E
T M E L T


FIELD


<table>
 <tr>
  <th>Name</th>
  <th>Pos.</th>
  <th>Format</th>
  <th>Contents</th>
 </tr>
 <tr>
  <td>ATE</td>
  <td>0</td>
  <td>int 0:1:0</td>
  <td>Alpha Test<br/>0 Alpha test OFF<br/>1 Alpha test ON</td>
 </tr>
 <tr>
  <td>ATST</td>
  <td>3:1</td>
  <td>int 0:3:0</td>
  <td>Alpha Test Method<br/>000 NEVER All pixels fail.<br/>001 ALWAYS All pixels pass.<br/>010 LESS Pixels with A less than AREF pass.<br/>011 LEQUAL Pixels with A less than or equal to<br/>AREF pass.<br/>100 EQUAL Pixels with A equal to AREF pass.<br/>101 GEQUAL Pixels with A greater than or equal to<br/>AREF pass.<br/>110 GREATER Pixels with A greater than AREF pass.<br/>111 NOTEQUAL Pixels with A not to AREF</td>
 </tr>
 <tr>
  <td>AREF</td>
  <td>11:4</td>
  <td>int 0:8:0</td>
  <td>Value to be and Referred to</td>
 </tr>
 <tr>
  <td>AFAIL</td>
  <td>13:12</td>
  <td>int 0:2:0</td>
  <td>Processing Method when Failed in Alpha Test<br/>00 KEEP Neither frame buffer nor Z buffer is updated.<br/>01 FB_ONLY Only frame buffer is updated.<br/>10 ZB_ONLY Only Z buffer is updated.<br/>11 RGB_ONLY frame-buffer RGB is</td>
 </tr>
 <tr>
  <td>DATE</td>
  <td>14</td>
  <td>int 0:1:0</td>
  <td>Destination Alpha Test<br/>0 Destination Alpha Test OFF<br/>1 Destination Alpha Test ON</td>
 </tr>
 <tr>
  <td>DATM</td>
  <td>15</td>
  <td>int 0:1:0</td>
  <td>Destination Alpha Test Mode<br/>0 Pixels with destination alpha equal to 0 pass.<br/>1 Pixels with destination to 1</td>
 </tr>
 <tr>
  <td>ZTE</td>
  <td>16</td>
  <td>int 0:1:0</td>
  <td>Depth Test<br/>0 Depth test OFF (not allowed)<br/>1 Depth test ON</td>
 </tr>
 <tr>
  <td>ZTST</td>
  <td>18:17</td>
  <td>int 0:2:0</td>
  <td>Depth Test Method<br/>00 NEVER All pixels fail.<br/>01 ALWAYS All pixels pass.<br/>10 GEQUAL Pixels with Z greater than or equal to<br/>Z buffer value pass.<br/>11 GREATER Pixels with Z greater than Z buffer<br/>value</td>
 </tr>
</table>


- © SCEI

- 124-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## TEX0_1 / TEX0_2 : Texture Information Setting


0x06 / 0x07


These registers set various kinds of information regarding the textures to be used. TEX0_1 sets Context 1 and
TEX0_2 sets Context 2.


BIT ASSIGN


6 6 6 5 5 5 5 5
3 1 0 6 5 4 1 0
C
CLD CSA S P
S


<table>
 <tr>
  <th></th>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th>C</th>
  <td></td>
  <td></td>
  <td>T</td>
  <td>T</td>
 </tr>
 <tr>
  <th></th>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th>M</th>
  <td>M</td>
  <td></td>
  <td>X</td>
  <td></td>
 </tr>
</table>


<table>
 <tr>
  <th>Name</th>
  <th>Pos.</th>
  <th>Format</th>
  <th>Contents</th>
 </tr>
 <tr>
  <td>TBP0</td>
  <td>13:0</td>
  <td>int0:14:0</td>
  <td>Texture Base Pointer<br/>TBP0 = Word Address/64</td>
 </tr>
 <tr>
  <td>TBW</td>
  <td>19:14</td>
  <td>int0:6:0</td>
  <td>Texture Buffer Width<br/>TBW0 = Width in Units of Texels/64</td>
 </tr>
 <tr>
  <td>PSM</td>
  <td>25:20</td>
  <td>int0:6:0</td>
  <td>Texture Pixel Storage Format<br/>000000 PSMCT32<br/>000001 PSMCT24<br/>000010 PSMCT16<br/>001010 PSMCT16S<br/>010011 PSMT8<br/>010100 PSMT4<br/>011011 PSMT8H<br/>100100 PSMT4HL<br/>101100 PSMT4HH<br/>110000 PSMZ32<br/>110001 PSMZ24<br/>110010 PSMZ16<br/>111010 PSMZ16S</td>
 </tr>
 <tr>
  <td>TW</td>
  <td>29:26</td>
  <td>int0:4:0</td>
  <td>Texture Width: Width = 2TW (max 210) (*1)</td>
 </tr>
 <tr>
  <td>TH</td>
  <td>33:30</td>
  <td>int0:4:0</td>
  <td>Texture = 2TH (max 210) (*1)</td>
 </tr>
 <tr>
  <td>TCC</td>
  <td>34</td>
  <td>int0:1:0</td>
  <td>Texture Color Component<br/>0 RGB<br/>1 RGBA value is At in</td>
 </tr>
 <tr>
  <td>TFX</td>
  <td>36:35</td>
  <td>int0:2:0</td>
  <td>Texture Function<br/>00 MODULATE<br/>01 DECAL<br/>10 HIGHLIGHT<br/>11 HIGHLIGHT2</td>
 </tr>
 <tr>
  <td>CBP</td>
  <td>50:37</td>
  <td>int0:14:0</td>
  <td>CLUT Buffer Base Pointer<br/>CBP = Word Address/64</td>
 </tr>
 <tr>
  <td>CPSM</td>
  <td>54:51</td>
  <td>int0:4:0</td>
  <td>CLUT Pixel Storage Format<br/>0000 PSMCT32<br/>0010 PSMCT16<br/>1010 PSMCT16S</td>
 </tr>
 <tr>
  <td>CSM</td>
  <td>55</td>
  <td>int0:1:0</td>
  <td>CLUT Storage Mode<br/>0 CSM1<br/>1 CSM2</td>
 </tr>
</table>


3 3
7 6 5 4 3


3 2
0 9


2 2
6 5


2 1
0 9


1 1
4 3


0
0


F C TH TW PSM TBW
C


TBP0


FIELD


- 125-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


<table>
 <tr>
  <th>Name</th>
  <th>Pos.</th>
  <th>Format</th>
  <th>Contents</th>
 </tr>
 <tr>
  <td>CSA</td>
  <td>60:56</td>
  <td>int0:5:0</td>
  <td>CLUT Entry Offset<br/>CSA = Offset/16<br/>In CSM2 mode, CSA=0 must be set.</td>
 </tr>
 <tr>
  <td>CLD</td>
  <td>63:61</td>
  <td>int0:3:0</td>
  <td>CLUT Buffer Load Control<br/>000 Temporary buffer contents are not changed.<br/>001 Load is performed to CSA position of buffer.<br/>010 Load is performed to CSA position of buffer and<br/>CBP is copied to CBP0. (*2)<br/>011 Load is performed to CSA position of buffer and<br/>CBP is copied to CBP1. (*2)<br/>100 If CBP0 !=CBP, load is performed and<br/>CBP is copied to CBP0. (*2)<br/>101 If CBP1 != CBP, load is performed and<br/>CBP is to CBP1.</td>
 </tr>
</table>


copied
(*2)
be 8 x 8 texels or more when using bilinear or trilinear sampling.
are internal registers of the GS.


- *1 Texture size must

- *2 CBP0 and CBP1

- © SCEI

- 126-




SCE CONFIDENTIAL


GS User's Manual Version 6.0


## TEX1_1 / TEX1_2 : Texture Information Setting


0x14 / 0x15


These registers set information on the sampling method of the textures. TEX1_1 sets Context 1 and TEX1_2
sets Context 2.


BIT ASSIGN


3 4 1 2 0 0 0 0 0
3 2 0 9 9 8 6 5 4 2 0
M M L
K L T MMIN M MXL C
B A M
A G


FIELD


<table>
 <tr>
  <th>Name</th>
  <th>Pos.</th>
  <th>Format</th>
  <th>Contents</th>
 </tr>
 <tr>
  <td>LCM</td>
  <td>0</td>
  <td>int0:1:0</td>
  <td>LOD Calculation Method<br/>0 Due to the formula (LOD = (log2(1/|Q|)&lt;&lt;L)+K)<br/>1 Fixed value (LOD = K)</td>
 </tr>
 <tr>
  <td>MXL</td>
  <td>4:2</td>
  <td>int0:3:0</td>
  <td>Maximum MIP Level (0-6)</td>
 </tr>
 <tr>
  <td>MMAG</td>
  <td>5</td>
  <td>int0:1:0</td>
  <td>Filter when Texture is Expanded (LOD &lt; 0)<br/>0 NEAREST<br/>1 LINEAR</td>
 </tr>
 <tr>
  <td>MMIN</td>
  <td>8:6</td>
  <td>int0:3:0</td>
  <td>Filter when Texture is Reduced (LOD &gt;= 0)<br/>000 NEAREST<br/>001 LINEAR<br/>010 NEAREST_MIPMAP_NEAREST<br/>011 NEAREST_MIPMAP_LINEAR<br/>100 LINEAR_MIPMAP_NEAREST<br/>101 LINEAR_MIPMAP_LINEAR</td>
 </tr>
 <tr>
  <td>MTBA</td>
  <td>9</td>
  <td>int0:1:0</td>
  <td>Base Address Specification of MIPMAP Texture of Level 1 or More<br/>0 Value specified by MIPTBP1 and MIPTBP2 is used.<br/>1 Base address of TBP1 - TBP3 is automatically set.<br/>"3.4.11.</td>
 </tr>
 <tr>
  <td>L</td>
  <td>20:19</td>
  <td>int0:2:0</td>
  <td>LOD Parameter Value L the section for</td>
 </tr>
 <tr>
  <td>K</td>
  <td>43:32</td>
  <td>int1:7:4</td>
  <td>LOD Parameter Value K the section for</td>
 </tr>
</table>


- 127-

- © SCEI




SCE CONFIDENTIAL


GS User's Manual Version 6.0


## TEX2_1 / TEX2_2 : Texture Information Setting


0x16 / 0x17


These registers set texture information. They are subsets of the TEX0 register. TEX2_1 sets Context 1 and
TEX2_2 sets Context 2.


BIT ASSIGN


6 6 6 55 5 5 5
3 1 0 6 5 4 1 0
C
S
M


<table>
 <tr>
  <th colspan="4">C C P CBP PSM<br/>CLD CSA S S<br/>M M</th>
 </tr>
 <tr>
  <td colspan="4">FIELD</td>
 </tr>
 <tr>
  <td>Name</td>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <td>PSM</td>
  <td>25:20</td>
  <td>int0:6:0</td>
  <td>Texture Pixel Storage Format<br/>000000 PSMCT32<br/>000001 PSMCT24<br/>000010 PSMCT16<br/>001010 PCMCT16S<br/>010011 PSMT8<br/>010100 PSMT4<br/>011011 PSMT8H<br/>100100 PSMT4HL<br/>101100 PSMT4HH<br/>110000 PSMZ32<br/>110001 PSMZ24<br/>110010 PSMZ16<br/>111010 PSMZ16S</td>
 </tr>
 <tr>
  <td>CBP</td>
  <td>50:37</td>
  <td>int0:14:0</td>
  <td>CLUT Buffer Base Pointer<br/>CBP=Word Address/64</td>
 </tr>
 <tr>
  <td>CPSM</td>
  <td>54:51</td>
  <td>int0:4:0</td>
  <td>CLUT Entry Storage Format<br/>0000 PSMCT32<br/>0010 PSMCT16<br/>1010 PSMCT16S</td>
 </tr>
 <tr>
  <td>CSM</td>
  <td>55</td>
  <td>int0:1:0</td>
  <td>CLUT Storage Mode<br/>0 CSM1<br/>1 CSM2</td>
 </tr>
 <tr>
  <td>CSA</td>
  <td>60:56</td>
  <td>int0:5:0</td>
  <td>CLUT Entry Offset<br/>CSA = Offset/16<br/>In CSM2 mode, CSA=0 must be set.</td>
 </tr>
 <tr>
  <td>CLD</td>
  <td>63:61</td>
  <td>int0:3:0</td>
  <td>CLUT Buffer Load Control<br/>000 CLUT buffer contents are not changed.<br/>001 Load is performed to CSA position of buffer.<br/>010 Load is performed to CSA position of buffer and<br/>CBP is copied to CBP0. (*)<br/>011 Load is performed to CSA position of buffer and<br/>CBP is copied to CBP1. (*)<br/>100 If CBP0 != CBP, load is performed and<br/>CBP is copied to CBP0. (*)<br/>101 If CBP1 != CBP, load is performed and<br/>CBP is to CBP1. (*)</td>
 </tr>
</table>


3 2
7 5 0


copied
registers of the GS. For details, see "3.4.7. CLUT Buffer Control".


- * CBP0 and CBP1 are internal

- © SCEI

- 128-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## TEXA : Texture Alpha Value Setting


0x3b


This register sets the Alpha value to be referred to when the Alpha value of the texture is not an 8-bit value. See
"3.4.6. Format Conversion".


BIT ASSIGN


3 1
9 2 5


<table>
 <tr>
  <th colspan="4">A<br/>TA1 E TA0<br/>M</th>
 </tr>
 <tr>
  <th colspan="4"></th>
 </tr>
 <tr>
  <th>Name</th>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <th>TA0</th>
  <td>7:0</td>
  <td>int0:8:0</td>
  <td>The "As" value referred to when A field is 0 in RGBA16 format or<br/>when the format is RGB24.</td>
 </tr>
 <tr>
  <th>AEM</th>
  <td>15</td>
  <td>int0:1:0</td>
  <td>Method of Expanding Texture Alpha<br/>0 Processed normally even when R=G=B=0.<br/>1 Treated as "Transparent" (A=0) when R=G=B=0.</td>
 </tr>
 <tr>
  <th>TA1</th>
  <td>39:32</td>
  <td>int0:8:0</td>
  <td>The "As" value referred to when A field is 1 in RGBA16 format.</td>
 </tr>
</table>


0 0
7 0


- 129-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## TEXCLUT : CLUT Position Specification


0x1c


This register specifies the CLUT position in the buffer when the CLUT storage mode is CSM=1 (CSM2 mode).
It is disabled when CSM=0 (CSM1 mode).


BIT ASSIGN


2
1


1 00 0
2 1 6 5 0


FIELD


- © SCEI

- 130-




SCE CONFIDENTIAL GS User's Manual Version 6.0


- 131-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## TRXDIR : Activation of Transmission between Buffers


0x53


This register specifies the transmission direction in the transmission between buffers, and activates transmission.
Appropriate settings must be made by the BITBLTBUF/TRXPOS/TRXREG before activating the
transmission.


BIT ASSIGN


0
1


<table>
 <tr>
  <th colspan="4">X<br/>D<br/>I<br/>R</th>
 </tr>
 <tr>
  <th colspan="4"></th>
 </tr>
 <tr>
  <th>Name</th>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <th>XDIR</th>
  <td>1:0</td>
  <td>int0:2:0</td>
  <td>Transmission direction is specified and transmission is started.<br/>00 Host -&gt; Local Transmission<br/>01 Local -&gt; Host Transmission<br/>10 Local -&gt; Local Transmission<br/>11 Transmission is deactivated.</td>
 </tr>
</table>


0
0


- © SCEI

- 132-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## TRXPOS : Specification of Transmission Areas in Buffers


0x51


This register specifies the position and scanning direction of the rectangular area in each buffer where buffer
transmission is performed. For details, see "4.1. Transmission Parameters".


BIT ASSIGN


6 5 5 4 2 3 1 0
0 9 8 8 2 2 6 6 0 0


<table>
 <tr>
  <th>DIR</th>
  <th>DSAY</th>
  <th>DSAX</th>
  <th></th>
  <th>SSAY</th>
  <th>SSAX</th>
 </tr>
</table>


FIELD


<table>
 <tr>
  <th>Name</th>
  <th>Pos.</th>
  <th>Format</th>
  <th>Contents</th>
 </tr>
 <tr>
  <td>SSAX</td>
  <td>10:0</td>
  <td>int0:11:0</td>
  <td>X Coordinate of Upper Left Point of Source Rectangular Area</td>
 </tr>
 <tr>
  <td>SSAY</td>
  <td>26:16</td>
  <td>int0:11:0</td>
  <td>Y Coordinate of Upper Left Point of Source Rectangular Area</td>
 </tr>
 <tr>
  <td>DSAX</td>
  <td>42:32</td>
  <td>int0:11:0</td>
  <td>X Coordinate of Upper Left Point in Destination Rectangular Area</td>
 </tr>
 <tr>
  <td>DSAY</td>
  <td>58:48</td>
  <td>int0:11:0</td>
  <td>Y Coordinate of Upper Left Point in Destination Rectangular Area</td>
 </tr>
 <tr>
  <td>DIR</td>
  <td>60:59</td>
  <td>int0:2:0</td>
  <td>Pixel Transmission Order (Enabled only in Local -&gt; Local<br/>Transmission.)<br/>00 Upper Left -&gt; Lower Right<br/>01 Lower Left -&gt; Upper Right<br/>10 Upper Right -&gt; Lower Left<br/>11 Lower Right -&gt; Upper Left</td>
 </tr>
</table>


Note that the set rectangular area wraps around when exceeding the buffer width.


- 133-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## TRXREG : Specification of Transmission Areas in Buffers


0x52


This register specifies the size of the rectangular area, where the transmission between buffers is implemented, in
units of pixels. The pixel mode must be the one set by the BITBLTBUF register.


BIT ASSIGN


3 4 0 1
3 2 1 0


RRH


## Format


int0:12:0
Transmission
int0:12:0
Transmission


- © SCEI

- 134-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## UV : Specification of Vertex Texture Coordinates


0x03


This register specifies the texel coordinate (UV) values of the vertex.
It is enabled when FST is set to 1 in the PRIM register. When FST=0, the value of the UV register is ignored.


BIT ASSIGN


1 2 0
9 6 3 0


<table>
 <tr>
  <th></th>
  <td></td>
  <td></td>
  <td></td>
  <td>V</td>
  <td>U</td>
 </tr>
 <tr>
  <th>FIELD</th>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th>Name</th>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th>U</th>
  <td>13:0</td>
  <td>int 0:10:4</td>
  <td>Texel</td>
  <td>(U)</td>
  <td></td>
 </tr>
 <tr>
  <th>V</th>
  <td>29:16</td>
  <td>int 0:10:4</td>
  <td>Texel</td>
  <td>(V)</td>
  <td></td>
 </tr>
</table>


- 135-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## XYOFFSET_1 / XYOFFSET_2 : Offset Value Setting


0x18 / 0x19


These registers set the offset value for converting from the primitive coordinate system to the window
coordinate system. XYOFFSET_1 sets Context 1 and XYOFFSET_2 sets Context 2.


BIT ASSIGN


4 3 1 0
7 2 5 0


<table>
 <tr>
  <th></th>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td>OFX</td>
 </tr>
 <tr>
  <th>FIELD</th>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th>Name</th>
  <td>Pos.</td>
  <td></td>
  <td>Contents</td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th>OFX</th>
  <td>15:0</td>
  <td>0:12:4</td>
  <td>Offset</td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th>OFY</th>
  <td>47:32</td>
  <td>0:12:4</td>
  <td>Offset</td>
  <td></td>
  <td></td>
 </tr>
</table>


- © SCEI

- 136-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## XYZ2 : Setting for Vertex Coordinate Values


0x05


This register sets the vertex coordinate values and moves the vertex queue a step forward. The coordinate
values X and Y are specified in the primitive coordinate system.
When the vertex information stored in the vertex queue reaches a fixed number, drawing is started. The method
of moving the vertex queue forward and the number of vertices at which drawing starts depend on the type of
the drawing primitive.


BIT ASSIGN


6 1 3 0
3 2 1 6 5 0


<table>
 <tr>
  <th>Z Y X</th>
 </tr>
 <tr>
  <td></td>
 </tr>
 <tr>
  <td>Name Pos. Format Contents</td>
 </tr>
 <tr>
  <td>X 15:0 int 0:12:4 Vertex Coordinate Value X (0 - 4095.9375)</td>
 </tr>
 <tr>
  <td>Y 31:16 int 0:12:4 Vertex Coordinate Value Y (0 - 4095.9375)</td>
 </tr>
 <tr>
  <td>Z 63:32 int 0:32:0 Vertex Coordinate Value Z</td>
 </tr>
</table>


- 137-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## XYZ3 : Setting for Vertex Coordinate Values (without Drawing Kick)


0x0d


This register sets the coordinate values of the vertex and moves the vertex queue a step forward. The X and Y
coordinate values are specified in the primitive coordinate system.
Drawing is not started.


BIT ASSIGN


6 1 3 0
3 2 1 6 5 0


<table>
 <tr>
  <th>Z Y X</th>
 </tr>
 <tr>
  <td>FIELD</td>
 </tr>
 <tr>
  <td>Name Pos. Format Contents</td>
 </tr>
 <tr>
  <td>X 15:0 int 0:12:4 Vertex Coordinate Value X -</td>
 </tr>
 <tr>
  <td>Y 31:16 int 0:12:4 Vertex Coordinate Value Y -</td>
 </tr>
 <tr>
  <td>Z 63:32 int 0:32:0 Vertex Coordinate Value Z</td>
 </tr>
</table>


- © SCEI

- 138-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## XYZF2 : Setting for Vertex Coordinate Values


0x04


This register sets the vertex coordinate values and the Fog coefficient, and then moves the vertex queue a step
forward. The X and Y coordinate values are specified in the primitive coordinate system.
When the vertex information stored in the vertex queue reaches a fixed number, drawing is started. The method
of moving the vertex queue forward and the number of vertices at which drawing starts depend on the type of
the drawing primitive.


BIT ASSIGN


6 5
3 6 5


<table>
 <tr>
  <th>F</th>
  <th></th>
  <th>Z</th>
  <th>Y X</th>
 </tr>
 <tr>
  <td colspan="4"></td>
 </tr>
 <tr>
  <td>Name</td>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <td>X</td>
  <td>15:0</td>
  <td>int0:12:4</td>
  <td>Vertex Coordinate Value X (0 - 4095.9375)</td>
 </tr>
 <tr>
  <td>Y</td>
  <td>31:16</td>
  <td>int0:12:4</td>
  <td>Vertex Coordinate Value Y (0 - 4095.9375)</td>
 </tr>
 <tr>
  <td>Z</td>
  <td>55:32</td>
  <td>int0:24:0</td>
  <td>Vertex Coordinate Value Z</td>
 </tr>
 <tr>
  <td>F</td>
  <td>63:56</td>
  <td>int0:8:0</td>
  <td>Coefficient</td>
 </tr>
</table>


3 1 0
2 1 6 5 0


- 139-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## XYZF3 : Setting for Vertex Coordinate Values (without Drawing Kick)


0x0c


This register sets the vertex coordinate values and the Fog coefficient, and then moves the vertex queue a step
forward.
Drawing is not started.


BIT ASSIGN


6 5 1 3 0
3 6 5 2 1 6 5 0


<table>
 <tr>
  <th>F</th>
  <td></td>
  <td>Z</td>
  <td>Y X</td>
 </tr>
 <tr>
  <th colspan="4">FIELD</th>
 </tr>
 <tr>
  <th>Name</th>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <th>X</th>
  <td>15:0</td>
  <td>int0:12:4</td>
  <td>Vertex Coordinate Value X (0 - 4095.9375)</td>
 </tr>
 <tr>
  <th>Y</th>
  <td>31:16</td>
  <td>int0:12:4</td>
  <td>Vertex Coordinate Value Y (0 - 4095.9375)</td>
 </tr>
 <tr>
  <th>Z</th>
  <td>55:32</td>
  <td>int0:24:0</td>
  <td>Vertex Coordinate Value Z</td>
 </tr>
 <tr>
  <th>F</th>
  <td>63:56</td>
  <td>int0:8:0</td>
  <td>Coefficient</td>
 </tr>
</table>


- © SCEI

- 140-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## ZBUF_1 / ZBUF_2 : Z Buffer Setting


0x4e / 0x4f


These registers make various settings regarding Z buffer. ZBUF_1 sets Context 1 and ZBUF_2 sets Context 2.


BIT ASSIGN


3
2
M
S


<table>
 <tr>
  <th colspan="2">Z Buffer Base Pointer (ZBP = Word Address/2048)</th>
 </tr>
 <tr>
  <td colspan="2">Z Value Storage Format</td>
 </tr>
 <tr>
  <td>0000</td>
  <td>PSMZ32</td>
 </tr>
 <tr>
  <td>0001</td>
  <td>PSMZ24</td>
 </tr>
 <tr>
  <td>0010</td>
  <td>PSMZ16</td>
 </tr>
 <tr>
  <td>1010</td>
  <td>PSMZ16S</td>
 </tr>
</table>


2 0
4 8 0


2
7


FIELD


## Name


ZBP 8:0
PSM 27:24


## Pos. Format Contents


int0:9:0
Buffer
int0:4:0
Value
ZMSK 32 int0:1:0 Z Value Drawing Mask
0 Z buffer is updated.
1 Z buffer is not updated regardless of the result of
test.
The buffer width of the Z buffer is not set since it is the same size as that of the frame buffer.


the depth


- 141-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 7.2. Privileged Registers


The privileged registers are as shown below:


• System Control/Status Register
• PCRTC Setting Register
• Event ID Register


Since the privileged registers and general-purpose registers are mapped to different spaces, their register
addresses may be duplicated. Also, unlike the general-purpose registers, which have write access only, some of
the privileged registers can have read/write access. For clarification, the following information is added to the
register names in this section:


(r/w) Read/Write


1. Write Only

- © SCEI

- 142-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## BGCOLOR (w) : Background Color Setting


This register sets the background color of the PCRTC with RGB value.


BIT ASSIGN


2
3


<table>
 <tr>
  <th>B G R</th>
 </tr>
 <tr>
  <td></td>
 </tr>
 <tr>
  <td>Name Pos. Format Contents</td>
 </tr>
 <tr>
  <td>R 7:0 int 0:8:0 Luminance of R element of color</td>
 </tr>
 <tr>
  <td>G 15:8 int 0:8:0 Luminance of G element of color</td>
 </tr>
 <tr>
  <td>B 23:16 int 0:8:0 Luminance of B element of color</td>
 </tr>
</table>


0x0e


1 1
6 5


0 0
8 7


0
0


- 143-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


<table>
 <tr>
  <th></th>
  <th>(w) :</th>
  <th>Host</th>
  <th></th>
  <th>Switching</th>
 </tr>
 <tr>
  <td>This</td>
  <td></td>
  <td>the</td>
  <td>of the</td>
  <td>0x44<br/>used for data transmission with the host.</td>
 </tr>
 <tr>
  <td>When</td>
  <td></td>
  <td>FIFO must</td>
  <td></td>
  <td>For details, see "4.2.2. Transmission from Local Buffer to Host".</td>
 </tr>
 <tr>
  <td>BIT</td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td></td>
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
  <td>0</td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td>D</td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td>I</td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td>R</td>
 </tr>
 <tr>
  <td>FIELD</td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td>Name</td>
  <td>Pos.</td>
  <td>Format</td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td>DIR</td>
  <td>0</td>
  <td>int 0:1:0</td>
  <td></td>
  <td>Direction of Interface</td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
  <td>0</td>
  <td>Host -&gt; Local</td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
  <td>1</td>
  <td>Local -&gt; Host</td>
 </tr>
</table>


- © SCEI

- 144-




SCE CONFIDENTIAL


GS User's Manual Version 6.0


## CSR (r/w) : System Status


0x40


This register sets the mode and obtains the status of the GS system.


2 3 1 1
1


1 1 00 00 0 0 0 0 0
4 3 6 5 4 3 2 9 8 6 5 4 3 2 1 0
F F N R F E V H F S
REV I I F E L D
E I
F L E S U 0 0 W S S I I
L E S I I I N G
N N N I N
S A
O D D T H T T T H L


<table>
 <tr>
  <th>Name</th>
  <th>Pos.</th>
  <th>Format</th>
  <th>Contents</th>
 </tr>
 <tr>
  <td>SIGNAL</td>
  <td>0</td>
  <td>int 0:1:0</td>
  <td>SIGNAL Event Control<br/>(Write)<br/>0 Nothing is done.<br/>1 Old event is cleared and event is enabled.<br/>(Read)<br/>0 SIGNAL event has not been generated.<br/>1 SIGNAL event has been</td>
 </tr>
 <tr>
  <td>FINISH</td>
  <td>1</td>
  <td>int 0:1:0</td>
  <td>FINISH Event Control<br/>(Write)<br/>0 Nothing is done.<br/>1 Event is enabled.<br/>(Read)<br/>0 FINISH event has not been generated.<br/>1 FINISH event has been</td>
 </tr>
 <tr>
  <td>HSINT</td>
  <td>2</td>
  <td>int 0:1:0</td>
  <td>HSync Interrupt Control<br/>(Write)<br/>0 Nothing is done.<br/>1 HSync interrupt is enabled.<br/>(Read)<br/>0 HSync interrupt has not been generated.<br/>1 has been</td>
 </tr>
 <tr>
  <td>VSINT</td>
  <td>3</td>
  <td>int 0:1:0</td>
  <td>VSync Interrupt Control<br/>(Write)<br/>0 Nothing is done.<br/>1 VSync interrupt is enabled.<br/>(Read)<br/>0 VSync interrupt has not been generated.<br/>1 VSync has been</td>
 </tr>
 <tr>
  <td>EDWINT</td>
  <td>4</td>
  <td>int 0:1:0</td>
  <td>Rectangular Area Write Termination Interrupt Control<br/>(Write)<br/>0 Nothing is done.<br/>1 Rectangular area write interrupt is enabled.<br/>(Read)<br/>0 Rectangular area write interrupt has not been generated.<br/>1 area write has been</td>
 </tr>
 <tr>
  <td>FLUSH</td>
  <td>8</td>
  <td>int 0:1:0</td>
  <td>Drawing Suspend and FIFO Clear (enabled during data write)<br/>0 Not flushed.<br/>1 Flushed.</td>
 </tr>
</table>


BIT ASSIGN


ID


FIELD


- 145-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


<table>
 <tr>
  <th>Name</th>
  <th>Pos.</th>
  <th>Format</th>
  <th>Contents</th>
 </tr>
 <tr>
  <td>RESET</td>
  <td>9</td>
  <td>int 0:1:0</td>
  <td>GS System Reset (enabled during data write)<br/>0 Not reset.<br/>1 Reset.</td>
 </tr>
 <tr>
  <td>NFIELD</td>
  <td>12</td>
  <td>int 0:1:0</td>
  <td>Output Value of NFIELD Output<br/>value is by at VSync.)</td>
 </tr>
 <tr>
  <td>FIELD</td>
  <td>13</td>
  <td>int 0:1:0</td>
  <td>Field Displayed Currently<br/>(In Interlace Mode)<br/>0 EVEN<br/>1 ODD<br/>(In Non-Interlace Mode)<br/>0 Equivalent to EVEN<br/>1 to ODD</td>
 </tr>
 <tr>
  <td>FIFO</td>
  <td>15:14</td>
  <td>int 0:2:0</td>
  <td>Host Interface FIFO Status<br/>00 Neither Empty nor Almost Full<br/>01 Empty<br/>10 Almost Full<br/>11 Reserved</td>
 </tr>
 <tr>
  <td>REV</td>
  <td>23:16</td>
  <td>int 0:8:0</td>
  <td>Revision No. of the GS</td>
 </tr>
 <tr>
  <td>ID</td>
  <td>31:24</td>
  <td>int 0:8:0</td>
  <td>ID of the GS</td>
 </tr>
</table>


(Output
updated
sampling
Equivalent
when data is written.


bit 6 should be set to 0


Bit 5 and


- © SCEI

- 146-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## DISPFB1 (w) : Setting for Rectangular Area Read Output Circuit 1


0x07


This register makes settings for the frame buffer regarding information on Rectangular Area Read Output
Circuit 1 for the PCRTC.


BIT ASSIGN


4 5 3 1 1 1 0 0 0
3 3 2 2 9 5 4 9 8 0


<table>
 <tr>
  <th colspan="2"></th>
  <td colspan="2">DBY DBX PSM FBW FBP</td>
 </tr>
 <tr>
  <th colspan="4">FIELD</th>
 </tr>
 <tr>
  <th>Name</th>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <th>FBP</th>
  <td>8:0</td>
  <td>int0:9:0</td>
  <td>Base Pointer</td>
 </tr>
 <tr>
  <th>FBW</th>
  <td>14:9</td>
  <td>int0:6:0</td>
  <td>Buffer Width</td>
 </tr>
 <tr>
  <th>PSM</th>
  <td>19:15</td>
  <td>int0:5:0</td>
  <td>Pixel Storage Format<br/>00000 PSMCT32<br/>00001 PSMCT24<br/>00010 PSMCT16<br/>01010 PSMCT16S<br/>10010 PS-GPU24</td>
 </tr>
 <tr>
  <th>DBX</th>
  <td>42:32</td>
  <td>int0:11:0</td>
  <td>X Position in Buffer of Upper Left Point of Rectangular Area<br/>(in units of pixels)</td>
 </tr>
 <tr>
  <th>DBY</th>
  <td>53:43</td>
  <td>int0:11:0</td>
  <td>Y Position in Buffer of Upper Left Point of Rectangular Area<br/>(in units of</td>
 </tr>
</table>


- 147-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## DISPFB2 (w) : Setting for Rectangular Area Read Output Circuit 2


0x09


This register makes settings for the frame buffer regarding information on Rectangular Area Read Output
Circuit 2 for the PCRTC.


BIT ASSIGN


4 5 3 1 1 1 0 0 0
3 3 2 2 9 5 4 9 8 0


<table>
 <tr>
  <th colspan="4">DBY DBX PSM FBW FBP</th>
 </tr>
 <tr>
  <th colspan="4">FIELD</th>
 </tr>
 <tr>
  <th>Name</th>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <th>FBP</th>
  <td>8:0</td>
  <td>int0:9:0</td>
  <td>Base Pointer</td>
 </tr>
 <tr>
  <th>FBW</th>
  <td>14:9</td>
  <td>int0:6:0</td>
  <td>Buffer Width</td>
 </tr>
 <tr>
  <th>PSM</th>
  <td>19:15</td>
  <td>int0:5:0</td>
  <td>Pixel Storage Format<br/>00000 PSMCT32<br/>00001 PSMCT24<br/>00010 PSMCT16<br/>01010 PSMCT16S</td>
 </tr>
 <tr>
  <th>DBX</th>
  <td>42:32</td>
  <td>int0:11:0</td>
  <td>X Position in Buffer of Upper Left Point of Rectangular Area<br/>units of</td>
 </tr>
 <tr>
  <th>DBY</th>
  <td>53:43</td>
  <td>int0:11:0</td>
  <td>Y Position in Buffer of Upper Left Point of Rectangular Area<br/>(in units of pixels)</td>
 </tr>
</table>


- © SCEI

- 148-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## DISPLAY1 (w) : Setting for Rectangular Area Read Output Circuit 1


This register makes settings for the display position on the screen regarding information on Rectangular Area
Read Output Circuit 1 for the PCRTC.


BIT ASSIGN


4 5 3 2 2 2 2 2
4 4 3 2 8 7 6 3 2


<table>
 <tr>
  <th colspan="4">M M<br/>DH DW A G A G DY DX<br/>V H</th>
 </tr>
 <tr>
  <th colspan="4">FIELD</th>
 </tr>
 <tr>
  <th>Name</th>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <th>DX</th>
  <td>11:0</td>
  <td>int 0:12:0</td>
  <td>X Position in the Area VCK</td>
 </tr>
 <tr>
  <th>DY</th>
  <td>22:12</td>
  <td>int 0:11:0</td>
  <td>Y Position in the Area Raster</td>
 </tr>
 <tr>
  <th>MAGH</th>
  <td>26:23</td>
  <td>int 0:4:0</td>
  <td>Magnification in H Direction<br/>0000 x 1<br/>0001 x 2<br/>0010 x 3<br/>0011 x 4<br/>:<br/>:<br/>1111 x 16</td>
 </tr>
 <tr>
  <th>MAGV</th>
  <td>28:27</td>
  <td>int 0:2:0</td>
  <td>Magnification in V Direction<br/>00 x 1<br/>01 x 2<br/>10 x 3<br/>11 x 4</td>
 </tr>
 <tr>
  <th>DW</th>
  <td>43:32</td>
  <td>int 0:12:0</td>
  <td>Area Width - 1 (in VCK units)</td>
 </tr>
 <tr>
  <th>DH</th>
  <td>54:44</td>
  <td>int 0:11:0</td>
  <td>Area - 1 (in Pixel units)</td>
 </tr>
</table>


0x08


1 0
2 1 0


- 149-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## DISPLAY2 (w) : Setting for Rectangular Area Read Output Circuit 2


This register makes settings for the display position on the screen regarding information on Rectangular Area
Read Output Circuit 2 for the PCRTC.


BIT ASSIGN


4 5 3 2 2 2
4 4 3 2 8 7 6 3 2


<table>
 <tr>
  <th colspan="4">M M<br/>DH DW A G A G DY DX<br/>V H</th>
 </tr>
 <tr>
  <th colspan="4">FIELD</th>
 </tr>
 <tr>
  <th>Name</th>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <th>DX</th>
  <td>11:0</td>
  <td>int 0:12:0</td>
  <td>X Position in the Area VCK</td>
 </tr>
 <tr>
  <th>DY</th>
  <td>22:12</td>
  <td>int 0:11:0</td>
  <td>Y Position in the Area Raster</td>
 </tr>
 <tr>
  <th>MAGH</th>
  <td>26:23</td>
  <td>int 0:4:0</td>
  <td>Magnification in H Direction<br/>0000 x 1<br/>0001 x 2<br/>0010 x 3<br/>0011 x 4<br/>:<br/>:<br/>1111 x 16</td>
 </tr>
 <tr>
  <th>MAGV</th>
  <td>28:27</td>
  <td>int 0:2:0</td>
  <td>Magnification in V Direction<br/>00 x 1<br/>01 x 2<br/>10 x 3<br/>11 x 4</td>
 </tr>
 <tr>
  <th>DW</th>
  <td>43:32</td>
  <td>int 0:12:0</td>
  <td>Area Width - 1 (in VCK units)</td>
 </tr>
 <tr>
  <th>DH</th>
  <td>54:44</td>
  <td>int 0:11:0</td>
  <td>Area - 1 (in Pixel units)</td>
 </tr>
</table>


0x0a


1 0
2 1 0


- © SCEI

- 150-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## EXTBUF (w) : Setting for Feedback Write Buffer


This register makes settings for the frame buffer used when feedback write is performed.


BIT ASSIGN


5 4 3 22 2 2 2 2 2 1
3 3 2 2 6 5 4 3 2 1 0 9
E E W
M M F B EXBW
WDY WDX O O F
D D M


<table>
 <tr>
  <th></th>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th></th>
  <td></td>
  <td>F</td>
 </tr>
 <tr>
  <th></th>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th></th>
  <td></td>
  <td>I</td>
 </tr>
 <tr>
  <th>C</th>
  <td>A</td>
  <td>D N</td>
 </tr>
</table>


<table>
 <tr>
  <th>Name</th>
  <th>Pos.</th>
  <th>Format</th>
  <th>Contents</th>
 </tr>
 <tr>
  <td>EXBP</td>
  <td>13:0</td>
  <td>int 0:14:0</td>
  <td>Base pointer of the buffer where data is written.<br/>EXBP=Word Address/64</td>
 </tr>
 <tr>
  <td>EXBW</td>
  <td>19:14</td>
  <td>int 0:6:0</td>
  <td>Width of the buffer where data is written.<br/>EXBW=Width in Pixel units/64</td>
 </tr>
 <tr>
  <td>FBIN</td>
  <td>21:20</td>
  <td>int 0:2:0</td>
  <td>Selection of Input Source<br/>00 OUT1<br/>01 OUT2</td>
 </tr>
 <tr>
  <td>WFFMD</td>
  <td>22</td>
  <td>int 0:1:0</td>
  <td>Interlace Mode<br/>0 FIELD (Written to every other raster.)<br/>1 FRAME (Written to every raster.)</td>
 </tr>
 <tr>
  <td>EMODA</td>
  <td>24:23</td>
  <td>int 0:2:0</td>
  <td>Method of Processing Input Alpha Value<br/>00 Input Alpha value is written as it is.<br/>01 Value converted from Input RGB to Luminance value Y is written.<br/>10 Value converted from Input RGB to Luminance value Y and<br/>reduced by half is written.<br/>11 0</td>
 </tr>
 <tr>
  <td>EMODC</td>
  <td>26:25</td>
  <td>int 0:2:0</td>
  <td>Method of Processing Input Color Value<br/>00 Input RGB is written as it is.<br/>01 Value converted from Input RGB to Luminance value Y is written<br/>to RGB respectively.<br/>10 Value converted from Input RGB to YCbCr is written.<br/>11 value is written to RGB</td>
 </tr>
 <tr>
  <td>WDX</td>
  <td>42:32</td>
  <td>int 0:11:0</td>
  <td>X Coordinate in the buffer of upper left point of the rectangular area where<br/>input image data is written.</td>
 </tr>
 <tr>
  <td>WDY</td>
  <td>53:43</td>
  <td>int 0:11:0</td>
  <td>Y Coordinate in the buffer of upper left point of the rectangular area where<br/>data is written.</td>
 </tr>
</table>


0x0b


1 1 0
4 3 0


EXBP


FIELD


- 151-

- © SCEI




SCE CONFIDENTIAL


GS User's Manual Version 6.0


## EXTDATA (w) : Feedback Write Setting


0x0c


This register sets the area to be read when feedback write is performed.


BIT ASSIGN


5 4 3 2
4 4 3


<table>
 <tr>
  <th></th>
  <td colspan="3">S<br/>WH WW M P SMPH SY SX<br/>V</td>
 </tr>
 <tr>
  <th colspan="4">FIELD</th>
 </tr>
 <tr>
  <th>Name</th>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <th>SX</th>
  <td>11:0</td>
  <td>int0:12:0</td>
  <td>X Coordinate of upper left point of the rectangular area where input<br/>is written VCK</td>
 </tr>
 <tr>
  <th>SY</th>
  <td>22:12</td>
  <td>int0:11:0</td>
  <td>Y Coordinate of upper left point of the rectangular area where input<br/>image is written (in Pixel units).</td>
 </tr>
 <tr>
  <th>SMPH</th>
  <td>26:23</td>
  <td>int0:4:0</td>
  <td>Sampling Rate in H Direction (in VCK units)<br/>0000 Every VCK<br/>0001 At intervals of 1 VCK<br/>0010 At intervals of 2 VCKs<br/>0011 At intervals of 3 VCKs<br/>:<br/>:<br/>1111 At intervals of 15 VCKs</td>
 </tr>
 <tr>
  <th>SMPV</th>
  <td>28:27</td>
  <td>int0:2:0</td>
  <td>Sampling Rate in V Direction<br/>00 Every H-Sync<br/>01 At intervals of 1 H-Sync<br/>10 At intervals of 2 H-Syncs<br/>11 At intervals of 3</td>
 </tr>
 <tr>
  <th>WW</th>
  <td>43:32</td>
  <td>int0:12:0</td>
  <td>Area Width - 1</td>
 </tr>
 <tr>
  <th>WH</th>
  <td>54:44</td>
  <td>int0:11:0</td>
  <td>Area - 1</td>
 </tr>
</table>


2 2 1 0
2 8 7 6 3 2 2 1 0


- © SCEI

- 152-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## EXTWRITE (w) : Feedback Write Function Control


This register controls the feedback write operation (start-stop).


BIT ASSIGN


<table>
 <tr>
  <th></th>
  <th>W</th>
 </tr>
 <tr>
  <td></td>
  <td>R</td>
 </tr>
 <tr>
  <td></td>
  <td>I</td>
 </tr>
 <tr>
  <td></td>
  <td>T</td>
 </tr>
 <tr>
  <td></td>
  <td>E</td>
 </tr>
 <tr>
  <td>FIELD</td>
  <td></td>
 </tr>
 <tr>
  <td>Name Pos. Format</td>
  <td></td>
 </tr>
 <tr>
  <td>WRITE 0 int 0:1:0<br/>0</td>
  <td>of Write<br/>Write to memory is completed in the current frame.</td>
 </tr>
 <tr>
  <td>1</td>
  <td>Write to memory is started from the next frame.</td>
 </tr>
</table>


0x0d


0
0


- 153-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## IMR (w) : Interrupt Mask Control


This register specifies whether various kinds of interrupts are masked.
All initial values after reset are 1 (masked).


BIT ASSIGN


0 0
4 3 2 1 0 9 8


<table>
 <tr>
  <th colspan="4">E V H F I S<br/>D S S N I</th>
 </tr>
 <tr>
  <th colspan="4">* * M M S</th>
 </tr>
 <tr>
  <th colspan="4">S S S M S<br/>K K K K S K</th>
 </tr>
 <tr>
  <th colspan="4">FIELD</th>
 </tr>
 <tr>
  <th>Name</th>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <th>SIGMSK</th>
  <td>8</td>
  <td>int 0:1:0</td>
  <td>SIGNAL Event Interrupt Mask<br/>0 Not masked.<br/>1 Masked.</td>
 </tr>
 <tr>
  <th>FINISHMSK</th>
  <td>9</td>
  <td>int 0:1:0</td>
  <td>FINISH Event Interrupt Mask<br/>0 Not masked.<br/>1 Masked.</td>
 </tr>
 <tr>
  <th>HSMSK</th>
  <td>10</td>
  <td>int 0:1:0</td>
  <td>HSync Interrupt Mask<br/>0 Not masked.<br/>1 Masked.</td>
 </tr>
 <tr>
  <th>VSMSK</th>
  <td>11</td>
  <td>int 0:1:0</td>
  <td>VSync Interrupt Mask<br/>0 Not masked.<br/>1 Masked.</td>
 </tr>
 <tr>
  <th>EDWMSK</th>
  <td>12</td>
  <td>int 0:1:0</td>
  <td>Rectangular Area Write Termination Interrupt Mask<br/>0 Not masked.<br/>1 Masked.</td>
 </tr>
</table>


0x41


1


should be set to 1.


- * Undefined bits

- © SCEI

- 154-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## PMODE (w) : PCRTC Mode Setting


0x00


This register makes various settings for the PCRTC.


BIT ASSIGN


0
6 5 8 7 6
S A


L M
B O
G D D


<table>
 <tr>
  <th>Name</th>
  <th>Pos.</th>
  <th>Format</th>
  <th>Contents</th>
 </tr>
 <tr>
  <td>EN1</td>
  <td>0</td>
  <td>int 0:1:0</td>
  <td>Read Circuit 1 ON/OFF<br/>0 OFF<br/>1 ON</td>
 </tr>
 <tr>
  <td>EN2</td>
  <td>1</td>
  <td>int 0:1:0</td>
  <td>Read Circuit 2 ON/OFF<br/>0 OFF<br/>1 ON</td>
 </tr>
 <tr>
  <td>CRTMD</td>
  <td>4:2</td>
  <td>int 0:3:0</td>
  <td>CRT Output Switching<br/>001</td>
 </tr>
 <tr>
  <td>MMOD</td>
  <td>5</td>
  <td>int 0:1:0</td>
  <td>Alpha Value Selection for Alpha Blending<br/>0 Alpha Value of Read Circuit 1<br/>1 ALP Value</td>
 </tr>
 <tr>
  <td>AMOD</td>
  <td>6</td>
  <td>int 0:1:0</td>
  <td>OUT1 Alpha Output Selection<br/>0 Alpha Value of Read Circuit 1<br/>1 Value of Read Circuit 2</td>
 </tr>
 <tr>
  <td>SLBG</td>
  <td>7</td>
  <td>int 0:1:0</td>
  <td>Alpha Blending Method Selection<br/>0 Blended with the output of Read Circuit 2.<br/>1 Blended with the color.</td>
 </tr>
 <tr>
  <td>ALP</td>
  <td>15:8</td>
  <td>int 0:8:0</td>
  <td>Fixed Value (0xff = 1.0)</td>
 </tr>
</table>


0 10
5 4 2 1 0
M E E
M CRTMD N N
O


2 1


FIELD


Always
Register
Alpha
background
Alpha
0.


- * Undefined bits should be set to

- 155-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## SIGLBLID (r/w) : Signal ID Value Read


0x48


This register obtains ID values of the SIGNAL event and LABEL event.


## BIT ASSIGN


6 3 0
3 2 1 0


<table>
 <tr>
  <th></th>
  <td></td>
  <td>LBLID</td>
  <td>SIGID</td>
 </tr>
 <tr>
  <th>FIELD</th>
  <td></td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th>Name</th>
  <td>Pos.</td>
  <td>Format</td>
  <td>Contents</td>
 </tr>
 <tr>
  <th>SIGID</th>
  <td>31:0</td>
  <td>int 0:32:0</td>
  <td>ID Value Set by SIGNAL Register</td>
 </tr>
 <tr>
  <th>LBLID</th>
  <td>63:32</td>
  <td>int 0:32:0</td>
  <td>ID Value Set by LABEL Register</td>
 </tr>
</table>


- © SCEI

- 156-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## SMODE2 (w) : Setting for Modes Related to Video Synchronization


0x02


This register makes settings related to PCRTC video synchronization.


BIT ASSIGN


0 0 0 0
3 2 1 0
D F I
P F N
M M T
S D


FIELD


<table>
 <tr>
  <th>Name</th>
  <th>Pos.</th>
  <th>Format</th>
  <th>Contents</th>
 </tr>
 <tr>
  <td>INT</td>
  <td>0</td>
  <td>int 0:1:0</td>
  <td>Interlace Mode Setting<br/>0 Non-Interlace Mode<br/>1 Interlace Mode</td>
 </tr>
 <tr>
  <td>FFMD</td>
  <td>1</td>
  <td>int 0:1:0</td>
  <td>Setting in Interlace Mode<br/>0 FIELD Mode (Read every other line.)<br/>1 FRAME Mode</td>
 </tr>
 <tr>
  <td>DPMS</td>
  <td>3:2</td>
  <td>int 0:2:0</td>
  <td>VESA DPMS Mode Setting<br/>00 On<br/>01 Stand-by<br/>10 Suspend<br/>11 Off</td>
 </tr>
</table>


- 157-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 7.3. Register List in Address Order


General Purpose Registers


<table>
 <tr>
  <th>Address</th>
  <th>Name</th>
  <th></th>
 </tr>
 <tr>
  <td>0x00</td>
  <td>PRIM</td>
  <td>Drawing primitive setting</td>
 </tr>
 <tr>
  <td>0x01</td>
  <td>RGBAQ</td>
  <td>Vertex color setting</td>
 </tr>
 <tr>
  <td>0x02</td>
  <td>ST</td>
  <td>Vertex texture coordinate setting (texture coordinates)</td>
 </tr>
 <tr>
  <td>0x03</td>
  <td>UV</td>
  <td>Vertex texture coordinate setting (texel coordinates)</td>
 </tr>
 <tr>
  <td>0x04</td>
  <td>XYZF2</td>
  <td>Vertex coordinate value setting</td>
 </tr>
 <tr>
  <td>0x05</td>
  <td>XYZ2</td>
  <td>Vertex coordinate value setting</td>
 </tr>
 <tr>
  <td>0x06</td>
  <td>TEX0_1</td>
  <td>Texture information setting</td>
 </tr>
 <tr>
  <td>0x07</td>
  <td>TEX0_2</td>
  <td>Texture information setting</td>
 </tr>
 <tr>
  <td>0x08</td>
  <td>CLAMP_1</td>
  <td>Texture wrap mode</td>
 </tr>
 <tr>
  <td>0x09</td>
  <td>CLAMP_2</td>
  <td>Texture wrap mode</td>
 </tr>
 <tr>
  <td>0x0a</td>
  <td>FOG</td>
  <td>Vertex fog value setting</td>
 </tr>
 <tr>
  <td>0x0c</td>
  <td>XYZF3</td>
  <td>Vertex coordinate value setting (without drawing kick)</td>
 </tr>
 <tr>
  <td>0x0d</td>
  <td>XYZ3</td>
  <td>Vertex coordinate value setting (without drawing kick)</td>
 </tr>
 <tr>
  <td>0x14</td>
  <td>TEX1_1</td>
  <td>Texture information setting</td>
 </tr>
 <tr>
  <td>0x15</td>
  <td>TEX1_2</td>
  <td>Texture information setting</td>
 </tr>
 <tr>
  <td>0x16</td>
  <td>TEX2_1</td>
  <td>Texture information setting</td>
 </tr>
 <tr>
  <td>0x17</td>
  <td>TEX2_2</td>
  <td>Texture information setting</td>
 </tr>
 <tr>
  <td>0x18</td>
  <td>XYOFFSET_1</td>
  <td>Offset value setting</td>
 </tr>
 <tr>
  <td>0x19</td>
  <td>XYOFFSET_2</td>
  <td>Offset value setting</td>
 </tr>
 <tr>
  <td>0x1a</td>
  <td>PRMODECONT</td>
  <td>Specification of primitive attribute setting method</td>
 </tr>
 <tr>
  <td>0x1b</td>
  <td>PRMODE</td>
  <td>Drawing primitive attribute setting</td>
 </tr>
 <tr>
  <td>0x1c</td>
  <td>TEXCLUT</td>
  <td>CLUT position setting</td>
 </tr>
 <tr>
  <td>0x22</td>
  <td>SCANMSK</td>
  <td>Raster address mask setting</td>
 </tr>
 <tr>
  <td>0x34</td>
  <td>MIPTBP1_1</td>
  <td>MIPMAP information setting (Level 1 - 3)</td>
 </tr>
 <tr>
  <td>0x35</td>
  <td>MIPTBP1_2</td>
  <td>MIPMAP information setting (Level 1 - 3)</td>
 </tr>
 <tr>
  <td>0x36</td>
  <td>MIPTBP2_1</td>
  <td>MIPMAP information setting (Level 4 - 6)</td>
 </tr>
 <tr>
  <td>0x37</td>
  <td>MIPTBP2_2</td>
  <td>MIPMAP information setting (Level 4 - 6)</td>
 </tr>
 <tr>
  <td>0x3b</td>
  <td>TEXA</td>
  <td>Texture alpha value setting</td>
 </tr>
 <tr>
  <td>0x3d</td>
  <td>FOGCOL</td>
  <td>Distant fog color setting</td>
 </tr>
 <tr>
  <td>0x3f</td>
  <td>TEXFLUSH</td>
  <td>Texture page buffer disabling</td>
 </tr>
 <tr>
  <td>0x40</td>
  <td>SCISSOR_1</td>
  <td>Scissoring area setting</td>
 </tr>
 <tr>
  <td>0x41</td>
  <td>SCISSOR_2</td>
  <td>Scissoring area setting</td>
 </tr>
 <tr>
  <td>0x42</td>
  <td>ALPHA_1</td>
  <td>Alpha blending setting</td>
 </tr>
 <tr>
  <td>0x43</td>
  <td>ALPHA_2</td>
  <td>Alpha blending setting</td>
 </tr>
 <tr>
  <td>0x44</td>
  <td>DIMX</td>
  <td>Dither matrix setting</td>
 </tr>
 <tr>
  <td>0x45</td>
  <td>DTHE</td>
  <td>Dither control</td>
 </tr>
 <tr>
  <td>0x46</td>
  <td>COLCLAMP</td>
  <td>Color clamp control</td>
 </tr>
 <tr>
  <td>0x47</td>
  <td>TEST_1</td>
  <td>Pixel test control</td>
 </tr>
 <tr>
  <td>0x48</td>
  <td>TEST_2</td>
  <td>Pixel test control</td>
 </tr>
 <tr>
  <td>0x49</td>
  <td>PABE</td>
  <td>Alpha blending control in pixel units</td>
 </tr>
 <tr>
  <td>0x4a</td>
  <td>FBA_1</td>
  <td>Alpha correction value</td>
 </tr>
 <tr>
  <td>0x4b</td>
  <td>FBA_2</td>
  <td>Alpha correction value</td>
 </tr>
 <tr>
  <td>0x4c</td>
  <td>FRAME_1</td>
  <td>Frame buffer setting</td>
 </tr>
 <tr>
  <td>0x4d</td>
  <td>FRAME_2</td>
  <td>Frame buffer setting</td>
 </tr>
 <tr>
  <td>0x4e</td>
  <td>ZBUF_1</td>
  <td>Z buffer setting</td>
 </tr>
 <tr>
  <td>0x4f</td>
  <td>ZBUF_2</td>
  <td>Z buffer setting</td>
 </tr>
 <tr>
  <td>0x50</td>
  <td>BITBLTBUF</td>
  <td>Setting for transmission between buffers</td>
 </tr>
 <tr>
  <td>0x51</td>
  <td>TRXPOS</td>
  <td>Specification for transmission area in buffers</td>
 </tr>
</table>


- © SCEI

- 158-




SCE CONFIDENTIAL


GS User's Manual Version 6.0


<table>
 <tr>
  <th>Address</th>
  <th>Name</th>
  <th></th>
 </tr>
 <tr>
  <td>0x52</td>
  <td>TRXREG</td>
  <td>for transmission area in buffers</td>
 </tr>
 <tr>
  <td>0x53</td>
  <td>TRXDIR</td>
  <td>Activation of transmission between buffers</td>
 </tr>
 <tr>
  <td>0x54</td>
  <td>HWREG</td>
  <td>Data for transmission between buffers</td>
 </tr>
 <tr>
  <td>0x60</td>
  <td>SIGNAL</td>
  <td>SIGNAL event occurrence</td>
 </tr>
 <tr>
  <td>0x61</td>
  <td>FINISH</td>
  <td>FINISH event occurrence</td>
 </tr>
 <tr>
  <td>0x62</td>
  <td>LABEL</td>
  <td>LABEL event occurrence</td>
 </tr>
</table>


<table>
 <tr>
  <th>Address</th>
  <th>Register Name</th>
  <th>Description</th>
 </tr>
 <tr>
  <td>0x00</td>
  <td>PMODE</td>
  <td>PCRTC mode</td>
 </tr>
 <tr>
  <td>0x02</td>
  <td>SMODE2</td>
  <td>Mode related to video synchronization</td>
 </tr>
 <tr>
  <td>0x07</td>
  <td>DISPFB1</td>
  <td>for area read circuit 1</td>
 </tr>
 <tr>
  <td>0x08</td>
  <td>DISPLAY1</td>
  <td>for area read circuit 1</td>
 </tr>
 <tr>
  <td>0x09</td>
  <td>DISPFB2</td>
  <td>for area read circuit 2</td>
 </tr>
 <tr>
  <td>0x0a</td>
  <td>DISPLAY2</td>
  <td>for area read circuit 2</td>
 </tr>
 <tr>
  <td>0x0b</td>
  <td>EXTBUF</td>
  <td>Feedback write buffer</td>
 </tr>
 <tr>
  <td>0x0c</td>
  <td>EXTDATA</td>
  <td>Feedback write</td>
 </tr>
 <tr>
  <td>0x0d</td>
  <td>EXTWRITE</td>
  <td>Feedback write control</td>
 </tr>
 <tr>
  <td>0x0e</td>
  <td>BGCOLOR</td>
  <td>color</td>
 </tr>
 <tr>
  <td>0x40</td>
  <td>CSR</td>
  <td>System status</td>
 </tr>
 <tr>
  <td>0x41</td>
  <td>IMR</td>
  <td>mask control</td>
 </tr>
 <tr>
  <td>0x44</td>
  <td>BUSDIR</td>
  <td>Host interface bus</td>
 </tr>
 <tr>
  <td>0x48</td>
  <td>SIGLBLID</td>
  <td>ID value read</td>
 </tr>
</table>


## Privileged Registers


- 159-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


(This page is left blank intentionally)


- © SCEI

- 160-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 8. Details of GS Local Memory


The GS uses non-linear address conversion rules when converting from a two-dimensional address to a one-
dimensional physical memory address to improve the efficiency of memory access. This chapter describes the
address conversion rules.


- 161-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 8.1. Memory Access Units


The GS local memory can be considered to be a linear address space of 32-bit word units, but it cannot be
accessed uniformly due to its complex configuration affected by the DRAM's physical structure.
Since accessible units vary according to the type of processing, 3 processing units are defined as follows:


## Page


Size: 8 Kbytes
FBP of FRAME (frame buffer) and ZBP of ZBUF (Z buffer) can point to page boundaries.
This is the same as a DRAM page. An access to a page does not result in a page break.


## Block


Size: 256 bytes
TBP0 of TEX0 and TBPn of MIPTBP1/2 (texture buffer), and DBP and SBP of BITBLTBUF (destination
and source buffers during transmission between buffers) point to block boundaries.


## Column


Size: 64 bytes (512 bits)
A column in a buffer is accessible in a single cycle.
Data can be read from and written to a column in the frame buffer and Z buffer in a single cycle (2048 bits
in total).


The relationships between page, block, and column are as follows from the point of view of memory
capacity:
Entire local memory = 512 pages = 16,384 blocks = 65,536 columns
1 page = 32 blocks
1 block = 4 columns
Each of page, block, and column corresponds to the pixels arranged in the form of a rectangle, in the two-
dimensional space in the buffer (i.e. when viewed as a drawn image or a texture image). The width and
height of the rectangle vary according to the pixel storage format.


<table>
 <tr>
  <th>Pixel Format</th>
  <th>x</th>
  <th>Block x</th>
  <th>Column x</th>
 </tr>
 <tr>
  <td>PSMCT32, PSMCT24,</td>
  <td>64 x 32</td>
  <td>8 x 8</td>
  <td>8 x 2</td>
 </tr>
 <tr>
  <td>PSMZ32, PSMZ24</td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td>PSMCT16, PSMCT16S,</td>
  <td>64 x 64 pixels</td>
  <td>16 x 8 pixels</td>
  <td>16 x 2 pixels</td>
 </tr>
 <tr>
  <td>PSMZ16, PSMZ16S</td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td>PSMT8</td>
  <td>128 x 64</td>
  <td>16 x 16</td>
  <td>16 x 4</td>
 </tr>
 <tr>
  <td>PSMT4</td>
  <td>128 x 128</td>
  <td>32 x 16</td>
  <td>32 x 4</td>
 </tr>
</table>


- © SCEI

- 162-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 8.2. Page Arrangement in Buffer


The following is the page arrangement order in a buffer in a two-dimensional space:


(e.g.) Frame Buffer (FBP=0 and FBW=10)
x


y


4


3


2


cc 9
cc 19


0


1


12


10


13


14


11


21


20


cc


Figure 8-1 Page Arrangement Order in Buffer


- 163-

- © SCEI


c c




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 8.3. Data Structure in a Page


The relationship between page, block, and column, such as block allocation in a page or pixel arrangement order
in a column, varies from one pixel storage format to another.
It is illustrated by pixel storage format below. Data with the same block number and column number in the
same page shows the same memory regardless of the pixel storage format.


## 8.3.1. PSMCT32/PSMCT24/PSMZ32/PSMZ24


Block Configuration in a Page (Numbers in the figures show block addresses.)


## PSMCT32, PSMCT24


64 pixels
121324252829


<table>
 <tr>
  <th>0</th>
  <td>1</td>
  <td>4</td>
  <td>5</td>
  <td>16</td>
  <td>17</td>
  <td>20</td>
  <td>21</td>
 </tr>
 <tr>
  <th>2</th>
  <td>3</td>
  <td>6</td>
  <td>7</td>
  <td>18</td>
  <td>19</td>
  <td>22</td>
  <td>23</td>
 </tr>
 <tr>
  <th>8</th>
  <td>9</td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th>10</th>
  <td>11</td>
  <td>14</td>
  <td>15</td>
  <td>26</td>
  <td>27</td>
  <td>30</td>
  <td>31</td>
 </tr>
</table>


## PSMZ32, PSMZ24


64 pixels


<table>
 <tr>
  <th>24</th>
  <td>25</td>
  <td>28</td>
  <td>29</td>
  <td>8</td>
  <td>9</td>
  <td>12</td>
  <td>13</td>
 </tr>
 <tr>
  <th>26</th>
  <td>27</td>
  <td>30</td>
  <td>31</td>
  <td>10</td>
  <td>11</td>
  <td>14</td>
  <td>15</td>
 </tr>
 <tr>
  <th>16</th>
  <td>17</td>
  <td>20</td>
  <td>21</td>
  <td>0</td>
  <td>1</td>
  <td>4</td>
  <td>5</td>
 </tr>
 <tr>
  <th>18</th>
  <td>19</td>
  <td>22</td>
  <td>23</td>
  <td>2</td>
  <td>3</td>
  <td>6</td>
  <td>7</td>
 </tr>
</table>


Column Configuration in
a Block


8 pixels


<table>
 <tr>
  <th colspan="2">COLUMN 0</th>
  <td rowspan="4">2 pixels<br/>Block</td>
 </tr>
 <tr>
  <th colspan="2">COLUMN 1</th>
 </tr>
 <tr>
  <th colspan="2">COLUMN 2</th>
 </tr>
 <tr>
  <th colspan="2">COLUMN 3</th>
 </tr>
</table>


Pixel Arrangement Order in a
Column
(Numbers in the figure show 32-bit word
addresses.)


8 pixels


size = 8x8 (pixels)
Column size = 8x2 (pixels)


0 1 458 912 13


236710111415


PSMT8H, PSMT4HH, and PSMT4HL have the same data structure as PSMCT32. Effective pixel data is put
in the position of bits 31 to 24, 31 to 27, and 27 to 24 respectively.


- © SCEI

- 164-




SCE CONFIDENTIAL


GS User's Manual Version 6.0


## 8.3.2. PSMCT16/PSMCT16S


Block Configuration in a Page (Numbers in the figures show block addresses.)


## PSMCT16


64 pixels


<table>
 <tr>
  <th>0</th>
  <td>2</td>
  <td>8</td>
  <td>10</td>
 </tr>
 <tr>
  <th>1</th>
  <td>3</td>
  <td>9</td>
  <td>11</td>
 </tr>
 <tr>
  <th>4</th>
  <td>6</td>
  <td>12</td>
  <td>14</td>
 </tr>
 <tr>
  <th>5</th>
  <td>7</td>
  <td>13</td>
  <td>15</td>
 </tr>
 <tr>
  <th>16</th>
  <td>18</td>
  <td>24</td>
  <td>26</td>
 </tr>
 <tr>
  <th>17</th>
  <td>19</td>
  <td>25</td>
  <td>27</td>
 </tr>
 <tr>
  <th>20</th>
  <td>22</td>
  <td>28</td>
  <td>30</td>
 </tr>
 <tr>
  <th>21</th>
  <td>23</td>
  <td>29</td>
  <td>31</td>
 </tr>
</table>


## PSMZ16


64 pixels


<table>
 <tr>
  <th>24</th>
  <td>26</td>
  <td>16</td>
  <td>18</td>
 </tr>
 <tr>
  <th>25</th>
  <td>27</td>
  <td>17</td>
  <td>19</td>
 </tr>
 <tr>
  <th>28</th>
  <td>30</td>
  <td>20</td>
  <td>22</td>
 </tr>
 <tr>
  <th>29</th>
  <td>31</td>
  <td>21</td>
  <td>23</td>
 </tr>
 <tr>
  <th>8</th>
  <td>10</td>
  <td>0</td>
  <td>2</td>
 </tr>
 <tr>
  <th>9</th>
  <td>11</td>
  <td>1</td>
  <td>3</td>
 </tr>
 <tr>
  <th>12</th>
  <td>14</td>
  <td>4</td>
  <td>6</td>
 </tr>
 <tr>
  <th>13</th>
  <td>15</td>
  <td>5</td>
  <td>7</td>
 </tr>
</table>


## PSMCT16S


64 pixels


<table>
 <tr>
  <th>0</th>
  <td>2</td>
  <td>16</td>
  <td>18</td>
 </tr>
 <tr>
  <th>1</th>
  <td>3</td>
  <td>17</td>
  <td>19</td>
 </tr>
 <tr>
  <th>8</th>
  <td>10</td>
  <td>24</td>
  <td>26</td>
 </tr>
 <tr>
  <th>9</th>
  <td>11</td>
  <td>25</td>
  <td>27</td>
 </tr>
 <tr>
  <th>4</th>
  <td>6</td>
  <td>20</td>
  <td>22</td>
 </tr>
 <tr>
  <th>5</th>
  <td>7</td>
  <td>21</td>
  <td>23</td>
 </tr>
 <tr>
  <th>12</th>
  <td>14</td>
  <td>28</td>
  <td>30</td>
 </tr>
 <tr>
  <th>13</th>
  <td>15</td>
  <td>29</td>
  <td>31</td>
 </tr>
</table>


Column Configuration in a Block
16 pixels


COLUMN 0


COLUMN 1


COLUMN 2


COLUMN 3


## PSMZ16S


64 pixels


<table>
 <tr>
  <th>24</th>
  <td>26</td>
  <td>8</td>
  <td>10</td>
 </tr>
 <tr>
  <th>25</th>
  <td>27</td>
  <td>9</td>
  <td>11</td>
 </tr>
 <tr>
  <th>16</th>
  <td>18</td>
  <td>0</td>
  <td>2</td>
 </tr>
 <tr>
  <th>17</th>
  <td>19</td>
  <td>1</td>
  <td>3</td>
 </tr>
 <tr>
  <th>28</th>
  <td>30</td>
  <td>12</td>
  <td>14</td>
 </tr>
 <tr>
  <th>29</th>
  <td>31</td>
  <td>13</td>
  <td>15</td>
 </tr>
 <tr>
  <th>20</th>
  <td>22</td>
  <td>4</td>
  <td>6</td>
 </tr>
 <tr>
  <th>21</th>
  <td>23</td>
  <td>5</td>
  <td>7</td>
 </tr>
</table>


2 pixels


8 pixels


Block size = 16x8 (pixels)
Column size = 16x2 (pixels)


- 165-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


Pixel Arrangement Order in a Column (Numbers in the figure show 32-bit word addresses.)


0 1 458 912 13 0 1 458 912 13


<table>
 <tr>
  <th>0</th>
  <td>1</td>
  <td>4</td>
  <td>5</td>
  <td>16</td>
  <td>17</td>
  <td>20</td>
  <td>21</td>
 </tr>
 <tr>
  <th>2</th>
  <td>3</td>
  <td>6</td>
  <td>7</td>
  <td>18</td>
  <td>19</td>
  <td>22</td>
  <td>23</td>
 </tr>
 <tr>
  <th>8</th>
  <td>9</td>
  <td>12</td>
  <td>13</td>
  <td>24</td>
  <td>25</td>
  <td>28</td>
  <td>29</td>
 </tr>
 <tr>
  <th>10</th>
  <td>11</td>
  <td>14</td>
  <td>15</td>
  <td>26</td>
  <td>27</td>
  <td>30</td>
  <td>31</td>
 </tr>
 <tr>
  <th></th>
  <td></td>
  <td>in a<br/>16</td>
  <td>Block</td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
</table>


8.3.3. PSMT8


Block Configuration in a Page (Numbers in the figure show block addresses.)
128 pixels


Column Configuration
pixels


COLUMN 0


4 pixels


COLUMN 1


<table>
 <tr>
  <th>COLUMN 2</th>
 </tr>
 <tr>
  <td>COLUMN 3</td>
 </tr>
</table>


16 pixels


- © SCEI

- 166-




SCE CONFIDENTIAL GS User's Manual Version 6.0


Pixel Arrangement Order in a Column (Numbers in the figures show 32-bit word addresses.)
Pixel arrangement order varies according to the column number in PSMT8.


COLUMNS 0 and 2


Bits 0-7


Bits 16-23


<table>
 <tr>
  <th>0 1 458 912 13 0 1 458 912 13</th>
 </tr>
 <tr>
  <td>236710111415 236710111415</td>
 </tr>
 <tr>
  <td>8 9 12 13 0 1 458 9 12 13 0 1 45</td>
 </tr>
 <tr>
  <td>10 11 14 15 236710111415 2367</td>
 </tr>
</table>


Bits 8-15


COLUMNS 1 and 3


Bits 0-7


Bits 24-31


Bits 16-23


<table>
 <tr>
  <th>8 9 12 13 0 1 458 9 12 13 0 1 45</th>
 </tr>
 <tr>
  <td>10 11 14 15 236710111415 2367</td>
 </tr>
 <tr>
  <td>0 1 458 912 13 0 1 458 912 13</td>
 </tr>
 <tr>
  <td>236710111415 236710111415</td>
 </tr>
</table>


Bits 8-15


Bits 24-31


- 167-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 8.3.4. PSMT4


Block Configuration in a Page (Numbers in the figure show block addresses.)
128 pixels


<table>
 <tr>
  <th>0</th>
  <td>2</td>
  <td>8</td>
  <td>10</td>
 </tr>
 <tr>
  <th>1</th>
  <td>3</td>
  <td>9</td>
  <td>11</td>
 </tr>
 <tr>
  <th>4</th>
  <td>6</td>
  <td>12</td>
  <td>14</td>
 </tr>
 <tr>
  <th>5</th>
  <td>7</td>
  <td>13</td>
  <td>15</td>
 </tr>
 <tr>
  <th>16</th>
  <td>18</td>
  <td>24</td>
  <td>26</td>
 </tr>
 <tr>
  <th>17</th>
  <td>19</td>
  <td>25</td>
  <td>27</td>
 </tr>
 <tr>
  <th>20</th>
  <td>22</td>
  <td>28</td>
  <td>30</td>
 </tr>
 <tr>
  <th>21</th>
  <td>23</td>
  <td>29</td>
  <td>31</td>
 </tr>
</table>


Column Configuration in a Block


<table>
 <tr>
  <th>COLUMN 0</th>
  <td rowspan="4">4 pixels<br/>16</td>
 </tr>
 <tr>
  <th>COLUMN 1</th>
 </tr>
 <tr>
  <th>COLUMN 2</th>
 </tr>
 <tr>
  <th>COLUMN 3</th>
 </tr>
</table>


32 pixels


16 pixels


- © SCEI

- 168-




### SCE CONFIDENTIAL GS User's Manual Version 6.0


Pixel Arrangement Order in a Column (Numbers in the figures show 32-bit word addresses.)


## Pixel arrangement order varies according to the column number in PSMT4.


COLUMNS 0 and 2
Bits 0-3


Bits 8-11


Bits 16-19


Bits 24-27


Bits 4-7


COLUMNS 1 and 3
Bits 0-3


Bits 12-15


Bits 8-11


Bits 20-23


Bits 16-19


Bits 28-31


Bits 24-27


Bits 4-7


Bits 12-15


Bits 20-23


Bits 28-31


- 169-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 8.4. Occupied Memory Area


8 (W) x 8 (H) in PSMCT32
Occupied block: 0 only


<table>
 <tr>
  <th>0</th>
  <td>1</td>
  <td>4</td>
  <td>5</td>
  <td>16</td>
  <td>17</td>
  <td>20</td>
  <td>21</td>
 </tr>
 <tr>
  <th>2</th>
  <td>3</td>
  <td>6</td>
  <td>7</td>
  <td>18</td>
  <td>19</td>
  <td>22</td>
  <td>23</td>
 </tr>
 <tr>
  <th>8</th>
  <td>9</td>
  <td>12</td>
  <td>13</td>
  <td>24</td>
  <td>25</td>
  <td>28</td>
  <td>29</td>
 </tr>
 <tr>
  <th>10</th>
  <td>11</td>
  <td>14</td>
  <td>15</td>
  <td>26</td>
  <td>27</td>
  <td>30</td>
  <td>31</td>
 </tr>
</table>


8 (W) x 16 (H) in PSMCT32
Occupied blocks: 0 and 2


<table>
 <tr>
  <th>0</th>
  <td>1</td>
  <td>4</td>
  <td>5</td>
  <td>16</td>
  <td>17</td>
  <td>20</td>
  <td>21</td>
 </tr>
 <tr>
  <th>2</th>
  <td>3</td>
  <td>6</td>
  <td>7</td>
  <td>18</td>
  <td>19</td>
  <td>22</td>
  <td>23</td>
 </tr>
 <tr>
  <th>8</th>
  <td>9</td>
  <td>12</td>
  <td>13</td>
  <td>24</td>
  <td>25</td>
  <td>28</td>
  <td>29</td>
 </tr>
 <tr>
  <th>10</th>
  <td>11</td>
  <td>14</td>
  <td>15</td>
  <td>26</td>
  <td>27</td>
  <td>30</td>
  <td>31</td>
 </tr>
</table>


32 (W) x 32 (H) in PSMCT32
Occupied blocks: 0 to 15


<table>
 <tr>
  <th>0</th>
  <td>1</td>
  <td>4</td>
  <td>5</td>
  <td>16</td>
  <td>17</td>
  <td>20</td>
  <td>21</td>
 </tr>
 <tr>
  <th>2</th>
  <td>3</td>
  <td>6</td>
  <td>7</td>
  <td>18</td>
  <td>19</td>
  <td>22</td>
  <td>23</td>
 </tr>
 <tr>
  <th>8</th>
  <td>9</td>
  <td>12</td>
  <td>13</td>
  <td>24</td>
  <td>25</td>
  <td>28</td>
  <td>29</td>
 </tr>
 <tr>
  <th>10</th>
  <td>11</td>
  <td>14</td>
  <td>15</td>
  <td>26</td>
  <td>27</td>
  <td>30</td>
  <td>31</td>
 </tr>
</table>


8 (W) x 32 (H) in PSMCT32
Occupied blocks: 0, 2, 8, and 10


<table>
 <tr>
  <th>0</th>
  <td>1</td>
  <td>4</td>
  <td>5</td>
  <td>16</td>
  <td>17</td>
  <td>20</td>
  <td>21</td>
 </tr>
 <tr>
  <th>2</th>
  <td>3</td>
  <td>6</td>
  <td>7</td>
  <td>18</td>
  <td>19</td>
  <td>22</td>
  <td>23</td>
 </tr>
 <tr>
  <th>8</th>
  <td>9</td>
  <td>12</td>
  <td>13</td>
  <td>24</td>
  <td>25</td>
  <td>28</td>
  <td>29</td>
 </tr>
 <tr>
  <th>10</th>
  <td>11</td>
  <td>14</td>
  <td>15</td>
  <td>26</td>
  <td>27</td>
  <td>30</td>
  <td>31</td>
 </tr>
</table>


- © SCEI

- 170-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 8.5. Pointing within a Page


A buffer starting address points to a block boundary (not a page boundary), when using a texture (TBP0 of
TEX0 or TBPn of MIPTBP1/2) or transferring data between buffers (SBP or DBP of BITBLTBUF). When it
points to a block within a page, blocks are arranged as follows from the point of view of the texture buffer:


- The block pointed to is located at the upper left point in a two-dimensional space

- The block arrangement order conforms to the one obtained when the start of a page is pointed to

- The block pointed to and the ones following configure a one-page rectangle (64 x 32 in PSMCT32)


Examples of block arrangement order are shown below. Light gray blocks show the next page, followed by the
dark gray page.


## Pointing to Block 1 in PSMCT32


64 pixels


<table>
 <tr>
  <th>1</th>
  <td>2</td>
  <td>5</td>
  <td>6</td>
  <td>17</td>
  <td>18</td>
  <td>21</td>
  <td>22</td>
  <td>1</td>
  <td>2</td>
  <td>5</td>
  <td>6</td>
  <td>17</td>
  <td>18</td>
  <td>21</td>
  <td>22</td>
 </tr>
 <tr>
  <th>3</th>
  <td>4</td>
  <td>7</td>
  <td>8</td>
  <td>19</td>
  <td>20</td>
  <td>23</td>
  <td>24</td>
  <td>3</td>
  <td>4</td>
  <td>7</td>
  <td>8</td>
  <td>19</td>
  <td>20</td>
  <td>23</td>
  <td>24</td>
 </tr>
 <tr>
  <th>9</th>
  <td>10</td>
  <td>13</td>
  <td>14</td>
  <td>25</td>
  <td>26</td>
  <td>29</td>
  <td>30</td>
  <td>9</td>
  <td>10</td>
  <td>13</td>
  <td>14</td>
  <td>25</td>
  <td>26</td>
  <td>29</td>
  <td>30</td>
 </tr>
 <tr>
  <th>11</th>
  <td>12</td>
  <td>15</td>
  <td>16</td>
  <td>27</td>
  <td>28</td>
  <td>31</td>
  <td>0</td>
  <td>11</td>
  <td>12</td>
  <td>15</td>
  <td>16</td>
  <td>27</td>
  <td>28</td>
  <td>31</td>
  <td>0</td>
 </tr>
 <tr>
  <th colspan="16">to Block 9 in PSMCT32<br/>64</th>
 </tr>
 <tr>
  <th>9</th>
  <td>10</td>
  <td>13</td>
  <td>14</td>
  <td>25</td>
  <td>26</td>
  <td>29</td>
  <td>30</td>
  <td>9</td>
  <td>10</td>
  <td>13</td>
  <td>14</td>
  <td>25</td>
  <td>26</td>
  <td>29</td>
  <td>30</td>
 </tr>
 <tr>
  <th>11</th>
  <td>12</td>
  <td>15</td>
  <td>16</td>
  <td>27</td>
  <td>28</td>
  <td>31</td>
  <td>0</td>
  <td>11</td>
  <td>12</td>
  <td>15</td>
  <td>16</td>
  <td>27</td>
  <td>28</td>
  <td>31</td>
  <td>0</td>
 </tr>
 <tr>
  <th>17</th>
  <td>18</td>
  <td>21</td>
  <td>22</td>
  <td>1</td>
  <td>2</td>
  <td>5</td>
  <td>6</td>
  <td>17</td>
  <td>18</td>
  <td>21</td>
  <td>22</td>
  <td>1</td>
  <td>2</td>
  <td>5</td>
  <td>6</td>
 </tr>
 <tr>
  <th>19</th>
  <td>20</td>
  <td>23</td>
  <td>24</td>
  <td>3</td>
  <td>4</td>
  <td>7</td>
  <td>8</td>
  <td>19</td>
  <td>20</td>
  <td>23</td>
  <td>24</td>
  <td>3</td>
  <td>4</td>
  <td>7</td>
  <td>8</td>
 </tr>
</table>


- 171-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 8.6. Pixel Storage Format Conversion


When transferring PSMT4/PSMT8/PSMT16 texture data as PSMCT32 data, use the following rules to convert
pixel storage format in the preprocessing stage. This allows textures of different modes to be packed together
and transferred at one time as PSMCT32 data.


## 8.6.1. PSMT4


The following is an example of converting 1-column PSMT4 data (32 x 4) to PSMCT32 (8 x 2).


1-column PSMT4 Data (Pixel numbers in the figure are arranged in scan line order.)


32 pixels


<table>
 <tr>
  <th>0 1</th>
  <td></td>
  <td></td>
  <td></td>
  <td>9</td>
  <td>10 11</td>
  <td>12 13</td>
  <td>14 15</td>
  <td>16 17</td>
  <td>18 19</td>
  <td>20 21</td>
  <td>22 23</td>
  <td>24 25</td>
  <td>26 27</td>
  <td>28 29</td>
  <td>30 31</td>
 </tr>
 <tr>
  <th>32 33</th>
  <td>34 35</td>
  <td>36 37</td>
  <td>38 39</td>
  <td>40 41</td>
  <td>42 43</td>
  <td>44 45</td>
  <td>46 47</td>
  <td>48 49</td>
  <td>50 51</td>
  <td>52 53</td>
  <td>54 55</td>
  <td>56 57</td>
  <td>58 59</td>
  <td>60 61</td>
  <td>62 63</td>
 </tr>
 <tr>
  <th>64 65</th>
  <td>66 67</td>
  <td>68 69</td>
  <td>70 71</td>
  <td>72 73</td>
  <td>74 75</td>
  <td>76 77</td>
  <td>78 79</td>
  <td>80 81</td>
  <td>82 83</td>
  <td>84 85</td>
  <td>86 87</td>
  <td>88 89</td>
  <td>90 91</td>
  <td>92 93</td>
  <td>94 95</td>
 </tr>
 <tr>
  <th>96 97</th>
  <td>98 99</td>
  <td>100 101</td>
  <td>102 103</td>
  <td>104 105</td>
  <td>106 107</td>
  <td>108 109</td>
  <td>110 111</td>
  <td>112 113</td>
  <td>114 115</td>
  <td>116 117</td>
  <td>118 119</td>
  <td>120 121</td>
  <td>122 123</td>
  <td>124 125</td>
  <td>126 127</td>
 </tr>
</table>


1-column PSMCT32 Data


<table>
 <tr>
  <th>A</th>
  <th>B</th>
  <th>C</th>
  <th>D</th>
  <th>E</th>
  <th>F</th>
  <th>G</th>
  <th>H</th>
 </tr>
 <tr>
  <td>I</td>
  <td>J</td>
  <td>K</td>
  <td>L</td>
  <td>M</td>
  <td>N</td>
  <td>O</td>
  <td>P</td>
 </tr>
</table>


- © SCEI

- 172-




SCE CONFIDENTIAL


## GS User's Manual Version 6.0


PSMCT32 pixels (A to P) correspond to PSMT4 pixels (0 to 127) as follows:


<table>
 <tr>
  <th></th>
  <td>31</td>
  <td>0</td>
  <td>and</td>
  <td>2</td>
  <td></td>
  <td></td>
  <td></td>
  <td>Bit</td>
  <td>0</td>
  <td>31</td>
  <td>1</td>
  <td>and</td>
  <td>3</td>
  <td></td>
  <td></td>
  <td></td>
  <td>Bit</td>
  <td>0</td>
 </tr>
 <tr>
  <th>A</th>
  <td>92</td>
  <td>24</td>
  <td>84</td>
  <td>16</td>
  <td>76</td>
  <td>88</td>
  <td>68</td>
  <td>00</td>
  <td>A</td>
  <td>88</td>
  <td>28</td>
  <td>80</td>
  <td>20</td>
  <td>72</td>
  <td>12</td>
  <td>64</td>
  <td>4</td>
  <td></td>
 </tr>
 <tr>
  <th>B</th>
  <td>93</td>
  <td>25</td>
  <td>85</td>
  <td>17</td>
  <td>77</td>
  <td>99</td>
  <td>69</td>
  <td>11</td>
  <td>B</td>
  <td>89</td>
  <td>29</td>
  <td>81</td>
  <td>21</td>
  <td>73</td>
  <td>13</td>
  <td>65</td>
  <td>5</td>
  <td></td>
 </tr>
 <tr>
  <th>C</th>
  <td>94</td>
  <td>26</td>
  <td>86</td>
  <td>18</td>
  <td>78</td>
  <td>10</td>
  <td>70</td>
  <td>22</td>
  <td>C</td>
  <td>90</td>
  <td>30</td>
  <td>82</td>
  <td>22</td>
  <td>74</td>
  <td>14</td>
  <td>66</td>
  <td>6</td>
  <td></td>
 </tr>
 <tr>
  <th>D</th>
  <td>95</td>
  <td>27</td>
  <td>87</td>
  <td>19</td>
  <td>79</td>
  <td>11</td>
  <td>71</td>
  <td>33</td>
  <td>D</td>
  <td>91</td>
  <td>31</td>
  <td>83</td>
  <td>23</td>
  <td>75</td>
  <td>15</td>
  <td>67</td>
  <td>7</td>
  <td></td>
 </tr>
 <tr>
  <th>E</th>
  <td>88</td>
  <td>28</td>
  <td>80</td>
  <td>20</td>
  <td>72</td>
  <td>12</td>
  <td>64</td>
  <td>44</td>
  <td>E</td>
  <td>92</td>
  <td>24</td>
  <td>84</td>
  <td>16</td>
  <td>76</td>
  <td>8</td>
  <td>68</td>
  <td>0</td>
  <td></td>
 </tr>
 <tr>
  <th>F</th>
  <td>89</td>
  <td>29</td>
  <td>81</td>
  <td>21</td>
  <td>73</td>
  <td>13</td>
  <td>65</td>
  <td>55</td>
  <td>F</td>
  <td>93</td>
  <td>25</td>
  <td>85</td>
  <td>17</td>
  <td>77</td>
  <td>9</td>
  <td>69</td>
  <td>1</td>
  <td></td>
 </tr>
 <tr>
  <th>G</th>
  <td>90</td>
  <td>30</td>
  <td>82</td>
  <td>22</td>
  <td>74</td>
  <td>14</td>
  <td>66</td>
  <td>66</td>
  <td>G</td>
  <td>94</td>
  <td>26</td>
  <td>86</td>
  <td>18</td>
  <td>78</td>
  <td>10</td>
  <td>70</td>
  <td>2</td>
  <td></td>
 </tr>
 <tr>
  <th>H</th>
  <td>91</td>
  <td>31</td>
  <td>83</td>
  <td>23</td>
  <td>75</td>
  <td>15</td>
  <td>67</td>
  <td>77</td>
  <td>H</td>
  <td>95</td>
  <td>27</td>
  <td>87</td>
  <td>19</td>
  <td>79</td>
  <td>11</td>
  <td>71</td>
  <td>3</td>
  <td></td>
 </tr>
 <tr>
  <th>I</th>
  <td>124124</td>
  <td>56</td>
  <td>11116</td>
  <td>48</td>
  <td>108108</td>
  <td>40</td>
  <td>100100</td>
  <td>32</td>
  <td>I</td>
  <td>120</td>
  <td>60</td>
  <td>112</td>
  <td>52</td>
  <td>104</td>
  <td>44</td>
  <td>96</td>
  <td>36</td>
  <td></td>
 </tr>
 <tr>
  <th>J</th>
  <td>125125</td>
  <td>57</td>
  <td>11117</td>
  <td>49</td>
  <td>109109</td>
  <td>41</td>
  <td>101101</td>
  <td>33</td>
  <td>J</td>
  <td>121</td>
  <td>61</td>
  <td>113</td>
  <td>53</td>
  <td>105</td>
  <td>45</td>
  <td>97</td>
  <td>37</td>
  <td></td>
 </tr>
 <tr>
  <th>K</th>
  <td>126126</td>
  <td>58</td>
  <td>11118</td>
  <td>50</td>
  <td>11110</td>
  <td>42</td>
  <td>102102</td>
  <td>34</td>
  <td>K</td>
  <td>122</td>
  <td>62</td>
  <td>114</td>
  <td>54</td>
  <td>106</td>
  <td>46</td>
  <td>98</td>
  <td>38</td>
  <td></td>
 </tr>
 <tr>
  <th>L</th>
  <td>127127</td>
  <td>59</td>
  <td>11119</td>
  <td>51</td>
  <td>111111</td>
  <td>43</td>
  <td>103103</td>
  <td>35</td>
  <td>L</td>
  <td>123</td>
  <td>63</td>
  <td>115</td>
  <td>55</td>
  <td>107</td>
  <td>47</td>
  <td>99</td>
  <td>39</td>
  <td></td>
 </tr>
 <tr>
  <th>M</th>
  <td>120120</td>
  <td>60</td>
  <td>11112</td>
  <td>52</td>
  <td>104104</td>
  <td>44</td>
  <td>96</td>
  <td>36</td>
  <td>M</td>
  <td>124</td>
  <td>56</td>
  <td>116</td>
  <td>48</td>
  <td>108</td>
  <td>40</td>
  <td>100</td>
  <td>32</td>
  <td></td>
 </tr>
 <tr>
  <th>N</th>
  <td>121121</td>
  <td>61</td>
  <td>11113</td>
  <td>53</td>
  <td>105105</td>
  <td>45</td>
  <td>97</td>
  <td>37</td>
  <td>N</td>
  <td>125</td>
  <td>57</td>
  <td>117</td>
  <td>49</td>
  <td>109</td>
  <td>41</td>
  <td>101</td>
  <td>33</td>
  <td></td>
 </tr>
 <tr>
  <th>O</th>
  <td>122122</td>
  <td>62</td>
  <td>11114</td>
  <td>54</td>
  <td>106106</td>
  <td>46</td>
  <td>98</td>
  <td>38</td>
  <td>O</td>
  <td>126</td>
  <td>58</td>
  <td>118</td>
  <td>50</td>
  <td>110</td>
  <td>42</td>
  <td>102</td>
  <td>34</td>
  <td></td>
 </tr>
 <tr>
  <th>P</th>
  <td>123123</td>
  <td>63</td>
  <td>11115</td>
  <td>55</td>
  <td>107107</td>
  <td>47</td>
  <td>99</td>
  <td>39</td>
  <td>P</td>
  <td>127</td>
  <td>59</td>
  <td>119</td>
  <td>51</td>
  <td>111</td>
  <td>43</td>
  <td>103</td>
  <td>35</td>
  <td></td>
 </tr>
</table>


-173-


© SCEI




SCE CONFIDENTIAL


GS User's Manual Version 6.0


## 8.6.2. PSMT8


The following is an example of converting 1-column PSMT8 data (16 x 4) to PSMCT32 (8 x 2).


1-column PSMT8 Data (Pixel numbers in the figure are arranged in scan line order.)


16 pixels


<table>
 <tr>
  <th>0 1</th>
  <td></td>
  <td></td>
  <td></td>
  <td>910</td>
  <td>11 12</td>
  <td>13 14 15</td>
 </tr>
 <tr>
  <th>16 17</th>
  <td>18 19</td>
  <td>20 21</td>
  <td>22 23 24</td>
  <td>25 26</td>
  <td>27 28</td>
  <td>29 30 31</td>
 </tr>
 <tr>
  <th>32 33</th>
  <td>34 35</td>
  <td>36 37</td>
  <td>38 39 40</td>
  <td>41 42</td>
  <td>43 44</td>
  <td>45 46 47</td>
 </tr>
 <tr>
  <th>48 49</th>
  <td>50 51</td>
  <td>52 53</td>
  <td>54 55 56</td>
  <td>57 58</td>
  <td>59 60</td>
  <td>61 62 63</td>
 </tr>
</table>


1-column PSMCT32 Data


<table>
 <tr>
  <th>A</th>
  <th>B</th>
  <th>C</th>
  <th>D</th>
  <th>E</th>
  <th>F</th>
  <th>G</th>
  <th>H</th>
 </tr>
 <tr>
  <td>I</td>
  <td>J</td>
  <td>K</td>
  <td>L</td>
  <td>M</td>
  <td>N</td>
  <td>O</td>
  <td>P</td>
 </tr>
</table>


PSMCT32 pixels (A to P) correspond


PSMT8 pixels (0 to 63) as follows:


to


COLUMNS 0 and 2


Bit 31


<table>
 <tr>
  <th>A 44</th>
  <td>8</td>
  <td>36</td>
  <td>0</td>
 </tr>
 <tr>
  <th>B 45</th>
  <td>9</td>
  <td>37</td>
  <td>1</td>
 </tr>
 <tr>
  <th>C 46</th>
  <td>10</td>
  <td>38</td>
  <td>2</td>
 </tr>
 <tr>
  <th>D 47</th>
  <td>11</td>
  <td>39</td>
  <td>3</td>
 </tr>
 <tr>
  <th>E 40</th>
  <td>12</td>
  <td>32</td>
  <td>4</td>
 </tr>
 <tr>
  <th>F 41</th>
  <td>13</td>
  <td>33</td>
  <td>5</td>
 </tr>
 <tr>
  <th>G 42</th>
  <td>14</td>
  <td>34</td>
  <td>6</td>
 </tr>
 <tr>
  <th>H 43</th>
  <td>15</td>
  <td>35</td>
  <td>7</td>
 </tr>
 <tr>
  <th>I 60</th>
  <td>24</td>
  <td>52</td>
  <td>16</td>
 </tr>
 <tr>
  <th>J 61</th>
  <td>25</td>
  <td>53</td>
  <td>17</td>
 </tr>
 <tr>
  <th>K 62</th>
  <td>26</td>
  <td>54</td>
  <td>18</td>
 </tr>
 <tr>
  <th>L 63</th>
  <td>27</td>
  <td>55</td>
  <td>19</td>
 </tr>
 <tr>
  <th>M 56</th>
  <td>28</td>
  <td>48</td>
  <td>20</td>
 </tr>
 <tr>
  <th>N 57</th>
  <td>29</td>
  <td>49</td>
  <td>21</td>
 </tr>
 <tr>
  <th>O 58</th>
  <td>30</td>
  <td>50</td>
  <td>22</td>
 </tr>
 <tr>
  <th>P 59</th>
  <td>31</td>
  <td>51</td>
  <td>23</td>
 </tr>
</table>


Bit 0


COLUMNS 1 and 3


Bit 31


<table>
 <tr>
  <th>40</th>
  <td>12</td>
  <td>32</td>
  <td>4</td>
 </tr>
 <tr>
  <th>41</th>
  <td>13</td>
  <td>33</td>
  <td>5</td>
 </tr>
 <tr>
  <th>42</th>
  <td>14</td>
  <td>34</td>
  <td>6</td>
 </tr>
 <tr>
  <th>43</th>
  <td>15</td>
  <td>35</td>
  <td>7</td>
 </tr>
 <tr>
  <th>44</th>
  <td>8</td>
  <td>36</td>
  <td>0</td>
 </tr>
 <tr>
  <th>45</th>
  <td>9</td>
  <td>37</td>
  <td>1</td>
 </tr>
 <tr>
  <th>46</th>
  <td>10</td>
  <td>38</td>
  <td>2</td>
 </tr>
 <tr>
  <th>47</th>
  <td>11</td>
  <td>39</td>
  <td>3</td>
 </tr>
 <tr>
  <th>56</th>
  <td>28</td>
  <td>48</td>
  <td>20</td>
 </tr>
 <tr>
  <th>57</th>
  <td>29</td>
  <td>49</td>
  <td>21</td>
 </tr>
 <tr>
  <th>58</th>
  <td>30</td>
  <td>50</td>
  <td>22</td>
 </tr>
 <tr>
  <th>59</th>
  <td>31</td>
  <td>51</td>
  <td>23</td>
 </tr>
 <tr>
  <th>60</th>
  <td>24</td>
  <td>52</td>
  <td>16</td>
 </tr>
 <tr>
  <th>61</th>
  <td>25</td>
  <td>53</td>
  <td>17</td>
 </tr>
 <tr>
  <th>62</th>
  <td>26</td>
  <td>54</td>
  <td>18</td>
 </tr>
 <tr>
  <th>63</th>
  <td>27</td>
  <td>55</td>
  <td>19</td>
 </tr>
</table>


A


B


C


D


E


F


G


H


I


J


K


L


M


N


O


P


Bit 0


- © SCEI

- 174-




SCE CONFIDENTIAL GS User's Manual Version 6.0


## 8.6.3. PSMT16


The following is an example of converting 1-column PSMT16 data (16 x 2) to PSMCT32 (8 x 2).


1-column PSMT16 Data (Pixel numbers in the figure are arranged in scan line order.)


16 pixels


<table>
 <tr>
  <th>1</th>
  <th></th>
  <th></th>
  <th></th>
  <th></th>
  <th></th>
  <th></th>
 </tr>
 <tr>
  <td>16 17</td>
  <td>18 19</td>
  <td>20 21</td>
  <td>22 23 24</td>
  <td>25 26</td>
  <td>27 28</td>
  <td>29 30 31</td>
 </tr>
</table>


1-column PSMCT32 Data


8 pixels


A


B


C


D


E


F


<table>
 <tr>
  <th colspan="2">I J K L M N O P<br/>pixels (A to P) correspond to<br/>Bit 31 Bit</th>
 </tr>
 <tr>
  <th>8</th>
  <td>0</td>
 </tr>
 <tr>
  <th>B 9</th>
  <td>1</td>
 </tr>
 <tr>
  <th>C 10</th>
  <td>2</td>
 </tr>
 <tr>
  <th>D 11</th>
  <td>3</td>
 </tr>
 <tr>
  <th>E 12</th>
  <td>4</td>
 </tr>
 <tr>
  <th>F 13</th>
  <td>5</td>
 </tr>
 <tr>
  <th>G 14</th>
  <td>6</td>
 </tr>
 <tr>
  <th>H 15</th>
  <td>7</td>
 </tr>
 <tr>
  <th>I 24</th>
  <td>16</td>
 </tr>
 <tr>
  <th>J 25</th>
  <td>17</td>
 </tr>
 <tr>
  <th>K 26</th>
  <td>18</td>
 </tr>
 <tr>
  <th>L 27</th>
  <td>19</td>
 </tr>
 <tr>
  <th>M 28</th>
  <td>20</td>
 </tr>
 <tr>
  <th>N 29</th>
  <td>21</td>
 </tr>
 <tr>
  <th>O 30</th>
  <td>22</td>
 </tr>
 <tr>
  <th>P 31</th>
  <td>23</td>
 </tr>
</table>


G


H


PSMT16 pixels (0 to 31) as follows:


- 175-

- © SCEI




SCE CONFIDENTIAL GS User's Manual Version 6.0


(This page is left blank intentionally)


- © SCEI

- 176-


