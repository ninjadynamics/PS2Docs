

# EE Overview


Copyright © 2002 Sony Computer Entertainment Inc.
All Rights Reserved.
SCE Confidential




SCE CONFIDENTIAL EE Overview Version 6.0


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


The EE Overview is supplied pursuant to and subject to the terms of the Sony Computer Entertainment
PlayStation® license agreements.


The EE Overview is intended for distribution to and use by only Sony Computer Entertainment licensed
Developers and Publishers in accordance with the PlayStation® license agreements.


Unauthorized reproduction, distribution, lending, rental or disclosure to any third party, in whole or in part, of
this book is expressly prohibited by law and by the terms of the Sony Computer Entertainment PlayStation®
license agreements.


Ownership of the physical property of the book is retained by and reserved by Sony Computer Entertainment.
Alteration to or deletion, in whole or in part, of the book, its presentation, or its contents is prohibited.


The information in the EE Overview is subject to change without notice. The content of this book is
Confidential Information of Sony Computer Entertainment.


® and PlayStation® are registered trademarks, and GRAPHICS SYNTHESIZERTM and


© SCEI


- 2-




SCE CONFIDENTIAL EE Overview Version 6.0


## About This Manual


The "EE Overview" introduces the development concept and main points of the functions and operation of the
Emotion Engine, the CPU of the PlayStation 2.


- Chapter 1 "Architecture Policy" describes the processing and features of the Emotion Engine and Graphics
Synthesizer, which allow the PlayStation 2 to implement high-speed real-time three-dimensional graphics, an
important characteristic of home entertainment software.
- Chapter 2 "Architecture Overview" introduces the functions and operations of the blocks which make up
the Emotion Engine.
- Chapter 3 "Functional Overview" describes the data flow between the blocks of the Emotion Engine and
from the Emotion Engine to the Graphics Synthesizer.


## Changes Since Release of 5th Edition


Since release of the 5th Edition of the EE Overview Manual, the following changes have been made.
Note that each of these changes is indicated by a revision bar in the margin of the affected page.


## Ch. 2: Architecture Overview


• A correction has been made to the description for Figure 2-11, in section 2.4. IPU Image Data
Processor, on page 45.


## Ch. 3: Functional Overview


- A correction has been made to section 3.3.1. Data Transfer Route, on page 60.

- 3-

- © SCEI




SCE CONFIDENTIAL EE Overview Version 6.0


(This page is left blank intentionally)


- © SCEI

- 4-




SCE CONFIDENTIAL EE Overview Version 6.0


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




SCE CONFIDENTIAL EE Overview Version 6.0


(This page is left blank intentionally)


- © SCEI

- 6-




SCE CONFIDENTIAL


EE Overview Version 6.0


## Contents


1. Architecture Policy ..................................................................................................................................................................9 1.1. Main Points of Architecture Policy .............................................................................................................................10 1.2. Expansion of Bandwidth ..............................................................................................................................................12 1.3. Geometry Engines in Parallel.......................................................................................................................................14 1.4. Data Decompression/Unpack.....................................................................................................................................16 1.5. Memory Architecture ....................................................................................................................................................17

2. Architecture Overview..........................................................................................................................................................21 2.1. EE Block Configuration ...............................................................................................................................................22 2.2. EE Core: CPU................................................................................................................................................................24 2.2.1. EE Core Features...................................................................................................................................................24 2.2.2. Memory Map...........................................................................................................................................................25 2.2.3. Instruction Set Overview ......................................................................................................................................26 2.3. VPU: Vector Operation Processor..............................................................................................................................35 2.3.1. VPU Architecture...................................................................................................................................................35 2.3.2. VPU0........................................................................................................................................................................38 2.3.3. VPU1........................................................................................................................................................................38 2.3.4. VIF: VPU Interface................................................................................................................................................39 2.3.5. Operation Mode and Programming Model........................................................................................................39 2.3.6. VPU Instruction Set Overview.............................................................................................................................40 2.4. IPU: Image Data Processor..........................................................................................................................................45 2.5. GIF: GS Interface..........................................................................................................................................................46 2.6. SIF: Sub-CPU Interface ................................................................................................................................................47 3. Functional Overview.............................................................................................................................................................49 3.1. Data Transfer via DMA................................................................................................................................................50 3.1.1. Sliced Transfer ........................................................................................................................................................50 3.1.2. Chain Mode Transfer.............................................................................................................................................50 3.1.3. Interleave Transfer.................................................................................................................................................54 3.1.4. Stall Control ............................................................................................................................................................54 3.1.5. MFIFO ....................................................................................................................................................................55 3.2. Data Transfer to VPU...................................................................................................................................................56 3.2.1. VIF Overview.........................................................................................................................................................56 3.2.2. VIF Packet...............................................................................................................................................................56 3.2.3. VIFcode Structure..................................................................................................................................................57 3.2.4. Data Transfer by UNPACK..................................................................................................................................58 3.2.5. Double Buffering ...................................................................................................................................................59 3.3. Data Transfer to GS......................................................................................................................................................60 3.3.1. Data Transfer Route ..............................................................................................................................................60 3.3.2. Data Format............................................................................................................................................................60 3.3.3. PACKED Mode.....................................................................................................................................................62 3.3.4. REGLIST Mode.....................................................................................................................................................62

- © SCEI

- 7-




SCE CONFIDENTIAL EE Overview Version 6.0


3.3.5. IMAGE Mode.........................................................................................................................................................62
3.4. Image Decompression by IPU .....................................................................................................................................63


- © SCEI

- 8-




SCE CONFIDENTIAL EE Overview Version 6.0


## 1. Architecture Policy


- 9-

- © SCEI




SCE CONFIDENTIAL EE Overview Version 6.0


## 1.1. Main Points of Architecture Policy


## Cutting-edge Process for Consumers


A characteristic of a home entertainment computer (a consumer video game console) is that its functions and
performance cannot be changed during its life. Changing functions and performance brings profit to neither
the developer nor the user. With this in mind, the PlayStation 2 is designed to have the highest performance
by adopting the latest technology and the most advanced manufacturing technology from the early stages, to
secure a long product life with performance at the point of sale kept unchanged.


## Silicon for Emotion


High-quality computer graphics require a huge amount of calculation. In addition, high-quality
entertainment software requires a large amount of calculation, not only for beautiful graphics but also for
logical inference and simulation of physical phenomena. The PlayStation 2 has sufficient resources to
produce this level of computer graphics, along with these additional elements.


## Fast Rendering


One of the most advanced manufacturing technologies for improving performance in computer graphics is
embedded DRAM, equipped with both an operation circuit and memory. By using embedded DRAM for
the rendering engine, the bandwidth between memory and processor expands dramatically. This eliminates a
bottleneck in pixel fill rate, which has been a problem with rendering engines up to now, and improves
drawing performance dynamically.


## Multi Path Geometry


Geometry performance is decreased relative to the improved drawing performance. To increase
performance and distribute the load, the architecture allows parallel geometry engines, and allows two or
more processors to share the same rendering engine by timesharing. This is unlike the previous architecture,
in which the rendering engines are in parallel.


## On-demand Data Decompression


The performance of memory is decreased relative to the improved processor performance. To make
effective use of low-capacity, low-speed memory, data is placed in memory in a compressed state, and is
decompressed and generated as necessary. High-resolution textures and modeling data, which use a lot of
memory, are normally kept in main memory in a compressed state and decompressed and generated by
means of a special circuit as necessary.


Stall Control and Memory FIFO


A huge amount of intermediate data (display lists) is continually transferred from the geometry engine to the
rendering engine. To control this data flow without imposing a load on the processor, an MFIFO (Memory
FIFO) mechanism is provided. This allows synchronized data transfers from the geometry engine to
memory and from memory to the rendering engine by using memory as a buffer.


## Application-Specific Processors


Video game applications inevitably use regular processes such as coordinate conversion and image
processing. Besides the processing load itself, context-switching overhead places a heavy load on the CPU.
For these reasons, many small-scale sub-processors are applied to these regular processes to share CPU
processing.


- © SCEI

- 10-




SCE CONFIDENTIAL EE Overview Version 6.0


## Intelligent Data Transport


Distributed processing by increasing sub-processors requires synchronization and arbitration controls. To
ensure that these controls are not a load on the CPU, all the instructions (programs) to the sub-processors
are sent along with data by DMA transfer through main memory.


## Data Path Buffering


In a UMA (Unified Memory Architecture) system with many sub-processors, competition for bus access
creates a bottleneck. Therefore, a small-capacity buffer memory is embedded in each sub-processor. The
results of processing are temporarily collected there and then collectively DMA-transferred to main memory.
As a result, burst transfer becomes central to bus access. Transmission efficiency should improve as well.


- 11-

- © SCEI




<table>
 <tr>
  <th>SCE CONFIDENTIAL</th>
  <th>EE Overview Version 6.0</th>
 </tr>
 <tr>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td>1.2. Expansion of Bandwidth<br/>Embedded DRAM<br/>Since performance of the rendering engine is determined by<br/>performance is maximized by using embedded DRAM in<br/>chip as the rendering circuit) and by providing multiple</td>
  <td>access to the frame buffer (pixel fill rate),<br/>GS (the frame buffer is embedded in the same<br/>engines to draw several pixels in parallel.</td>
 </tr>
 <tr>
  <td rowspan="3">Total 2048bit<br/>PXE<br/>PXE<br/>Ctrl<br/>Host PXE<br/>Interface ....<br/>PXE</td>
  <td rowspan="3">width<br/>Frame<br/>Buffer CRTC<br/>Video</td>
 </tr>
 <tr>
 </tr>
 <tr>
 </tr>
 <tr>
  <td>Figure 1-1 Speedup in Rendering<br/>Complete 128-bit Data Bus<br/>The processor has a 128-bit width data bus and registers.<br/>floating-point coprocessor registers are 128 bits wide. All</td>
  <td>by Embedded DRAM<br/>CPU's general-purpose registers (GPR) and<br/>processors are connected via a 128-bit bus.</td>
 </tr>
 <tr>
  <td>CPU<br/>128 bit<br/>GPR0 128 bit<br/>GPR1<br/>....<br/>GPR31<br/>128 bit</td>
  <td>Co-Processor<br/>128 bit<br/>VF00<br/>VF01<br/>....<br/>VF31<br/>128 bit</td>
 </tr>
 <tr>
  <td>Main Bus</td>
  <td>128 bit<br/>System</td>
 </tr>
 <tr>
  <td>Figure 1-2</td>
  <td>Bus</td>
 </tr>
</table>


- © SCEI

- 12-




SCE CONFIDENTIAL EE Overview Version 6.0


## Parallel 128-bit Integer Operation


A multimedia instruction set is implemented. It uses the 128-bit wide GPRs (integer registers) in parallel by
dividing them into fields of 8 bits x 16, 16 bits x 8, 32 bits x 4, and 64 bits x 2. The following example shows
execution of 16-parallel 8-bit addition.


128bit


rs a0 a1 a2 a3 a4 a5 a6 a7 a8 a9 a10 a11 a12 a13 a14 a15
++++++++++++++++
rt b0 b1 b2 b3 b4 b5 b6 b7 b8 b9 b10 b11 b12 b13 b14 b15
8bit PADDB (Parallel Add Byte)
rd c0 c1 c2 c3 c4 c5 c6 c7 c8 c9 c10 c11 c12 c13 c14 c15
SQ (Store Quad Word)
Main Memory


Figure 1-3 128-bit Parallel Processing by Multimedia Instruction


## Parallel 128-bit Floating Operation


The 128-bit floating-point registers are divided into four 32-bit floating-point fields. Four FMACs (floating-
point multiply-add ALUs) are provided for four fields to perform operations in parallel. The following
example shows the execution of four parallel 32-bit multiplications.


MUL MUL MUL MUL


FMAC FMAC FMAC FMAC


1 cycle
(Throughput)


VFc .x


VFc .y


VFc .z


VFc.w


128 bit


Figure 1-4 4-Parallel Floating-Point Operation


- 13-

- © SCEI




SCE CONFIDENTIAL EE Overview Version 6.0


## 1.3. Geometry Engines in Parallel


## Principle


To improve geometry performance relative to drawing performance, an architecture is implemented with
two geometry engines connected in parallel to one rendering engine. One of the geometry engines consists
of the CPU, with a high degree of flexibility, and a vector operation unit (VPU0) as a coprocessor to perform
complex irregular geometry processing, including physical simulation. The other engine is structured with a
programmable vector operation unit (VPU1) to perform simple, repetitive geometry processing such as
background and distant views.
The transfer right between the display lists from each geometry engine is arbitrated, and the display lists are
supplied to the rendering engine asynchronously.


2 Geometry Engines


<table>
 <tr>
  <th>CPU</th>
  <td rowspan="2"></td>
  <td rowspan="2">GIF<br/>Arbiter</td>
 </tr>
 <tr>
  <th>Vector<br/>Engine</th>
 </tr>
 <tr>
  <th>Vector<br/>Engine</th>
  <td colspan="2"></td>
 </tr>
</table>


Rendering Engine


GS


<table>
 <tr>
  <th>Rendering</th>
  <td></td>
  <td>Frame</td>
 </tr>
 <tr>
  <th>Logic</th>
  <td></td>
  <td>Buffer</td>
 </tr>
</table>


Figure 1-5 Parallel Geometry Engines


## Dual Context


The display lists supplied from the geometry engines have a context that includes status data such as texture
page and drawing mode. To eliminate the need for setting context information again, two contexts are
maintained in the GS, corresponding to the two geometry engines, VPU0 and VPU1. This is the dual
context mechanism.


GIF


Arbiter


Context
Information


Display
List


GC: Graphic Context


GS


GC0


SEL Rendering Frame
Logic Buffer


GC1


Figure 1-6 Rendering Engine with Dual Context


- © SCEI

- 14-




SCE CONFIDENTIAL EE Overview Version 6.0


## Data Path


Of the two geometry engines, the higher-priority VPU1 is directly connected to the GS, and the lower-
priority CPU+VPU0 is connected to the GS through the main bus. Because data transfer from the lower-
priority geometry engine might be suspended, generated display lists are buffered temporarily in main
memory. The corresponding DMA channels can monitor each other's transfer address so that the buffer
does not overflow.


GS


Arbiter


1st Priority


2nd Priority


CPU VPU0


VPU1


Main Bus
128bit


MFIFO


Main Memory


Figure 1-7 Typical Data Paths


## Application-Specific Path


The two geometry paths seem to the programmer to be two independent paths. That is, it is possible to
divide graphic processing of the application into two and allocate a portion to each geometry engine.
In general, a high-speed geometry engine (VPU1) takes charge of regular processing such as background and
distant view, and a geometry engine with a high degree of flexibility (CPU+VPU0) takes charge of complex
irregular processing including physical simulation. Simple lighting calculations and transparency perspective
conversions can be executed in VPU1, and the CPU does not have to participate in them directly.


Non-fixed, emotional, and creative operation


EE Core


VPU0


Rendering
Engine


VPU1


Fixed, routine operation


Figure 1-8 Processing Allocation of Geometry Engines


- 15-

- © SCEI




SCE CONFIDENTIAL EE Overview Version 6.0


## 1.4. Data Decompression/Unpack


## Image Decompression


High-resolution texture data requiring a large amount of memory is stored in main memory in a compressed
state, and is decompressed with a special decompression processor (IPU) when used. The decompressed
texture data is returned to main memory temporarily and transferred to the GS.


Display List


Compressed Image


Main
Memory Texture
Decompresser Arbiter GS


IPU


Display List


Figure 1-9 Image Data Decompression


## Geometry Data Unpack


Modeling data is packed into an optimal bit width in data units, maintained in main memory, and
automatically unpacked by the VIF when sent to the geometry engine (VPU). As a result, the data size in
main memory is reduced, and the load on the VPU can be reduced.


<table>
 <tr>
  <th>VPU<br/>VU 32bit<br/>VUMEM<br/>X Y Z 1.0</th>
 </tr>
 <tr>
  <th>VIF unpack</th>
 </tr>
 <tr>
  <th>Main X Y Z<br/>Memory 8/16bit</th>
 </tr>
</table>


Figure 1-10 Geometry Data Unpack


- © SCEI

- 16-




SCE CONFIDENTIAL EE Overview Version 6.0


## 1.5. Memory Architecture


## Hybrid UMA


To correct the problems with UMA (Unified Memory Architecture), each processor has a high-speed, small
capacity cache or working memory for exclusive use, and is connected to the large capacity shared memory
through the high-speed memory.
By storing the data read from or written to memory in 4-qword units, the cache speeds up the second and
succeeding accesses to the nearby addresses and decreases the frequency of accesses to the main memory.
Access to the main memory is made only when


- the data attempted to be read is not in the cache (cache miss)

- the data written to the cache is not reflected in memory (dirty) and the cache space is required to be freed to access other addresses (cache out).


Data is transferred between the cache and main memory as burst access every 4-qword block (cache line) to
improve the bus efficiency.


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
  <td>Engine</td>
  <td>Engine</td>
 </tr>
 <tr>
  <th>Local Cache</th>
  <td>Local Cache</td>
  <td>Local Cache</td>
 </tr>
 <tr>
  <th></th>
  <td></td>
  <td>Main Bus</td>
 </tr>
 <tr>
  <th></th>
  <td>Burst Access</td>
  <td></td>
 </tr>
</table>


Main Memory


Figure 1-11 Shared Main Memory and Local Cache


## CPU Cache


The CPU has an instruction cache (I-Cache) and a data cache (D-Cache). The data cache has the ability to
load a necessary word from a cache line first (sub-block ordering) and to permit a hazard-free cache-line hit
while a previous load is still in process (hit-under-miss). Since this hit-under-miss effect is similar to the
prefetch (PREF) instruction, it is effective when the address to be accessed is known in advance.


<table>
 <tr>
  <th>Cache</th>
  <th>Size</th>
  <th>Way</th>
  <th>Line Size</th>
  <th>Sub-block</th>
  <th>Hit-under-miss</th>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
  <td>Ordering</td>
  <td></td>
 </tr>
 <tr>
  <td>Instruction</td>
  <td>16 KB</td>
  <td>2-way</td>
  <td>4 qwords</td>
  <td>No</td>
  <td>No</td>
 </tr>
 <tr>
  <td>Data</td>
  <td>8 KB</td>
  <td>2-way</td>
  <td>4 qwords</td>
  <td>Yes</td>
  <td>Yes</td>
 </tr>
</table>


The output from the cache is also buffered in the Write Back Buffer (WBB). The WBB is a FIFO of 8
qwords. Write requests are stored here, and then written to memory according to the state of the main bus.


- 17-

- © SCEI




SCE CONFIDENTIAL


EE Overview Version 6.0


## Uncached Access


In applications primarily designed for computer graphics, writing display lists to memory is the major
process. The display lists are calculated from the three-dimensional data just read from memory. When
processing a one-way data flow like this, the use of cache may be a disadvantage. Furthermore, in some
cases (e.g. when writing hardware registers and writing data which should be DMA-transferred), it is
preferable that written data be reflected in the main memory immediately.
Therefore, a mode that does not use cache ( uncached mode) is provided. To speed up reading while writing
synchronously, an uncached accelerated mode that uses a special-purpose buffer (UCAB: uncached
accelerated buffer) is also available. The UCAB (in size 8 qwords) speeds up continuous data reading from
the adjoining addresses.


<table>
 <tr>
  <th>UnCached</th>
 </tr>
 <tr>
  <th>GPR</th>
 </tr>
</table>


Cached


## GPR


D Cache


## WBB


Memory


## WBB


Memory


UnCached Accelarated


GPR


WBB UCAB


Memory


Figure 1-12 Three Memory Access Modes


- © SCEI

- 18-




SCE CONFIDENTIAL EE Overview Version 6.0


## Scratchpad RAM


A general-purpose high-speed internal memory (Scratchpad RAM: SPR) useable as a working memory for
the CPU is embedded, in addition to the data cache. DMA transfer between main memory and the SPR can
be performed in parallel with SPR access from the CPU. Main memory access overhead can be hidden from
the program by using the SPR as a double buffer.


<table>
 <tr>
  <th>SPR #1 #2 pseudo-</th>
  <td colspan="7">Calc Calc Calc</td>
 </tr>
 <tr>
  <th></th>
  <td colspan="2"></td>
  <td>read</td>
  <td colspan="2">Calc</td>
  <td>write</td>
  <td>read</td>
 </tr>
 <tr>
  <th>DMA</th>
  <td colspan="7">read Calc write read Calc</td>
 </tr>
 <tr>
  <th>External</th>
  <td>read</td>
  <td></td>
  <td>read</td>
  <td>write</td>
  <td>read</td>
  <td>write</td>
  <td>read</td>
 </tr>
</table>


CPU


<table>
 <tr>
  <th>#1</th>
  <th>#2</th>
 </tr>
 <tr>
  <td colspan="2">DMA</td>
 </tr>
 <tr>
  <td></td>
  <td>External<br/>Memory</td>
 </tr>
</table>


Memory


1. Architecture

2. Scheduling


Figure 1-13 Double Buffering with SPR


## List processor DMA


Display lists are not always located in consecutive areas in memory. In most cases they can be arranged
discontinuously by adopting a linked list structure. To negate the need for data sorting when transferring
non-continuous data between processors, the DMAC can trace data lists according to the tag information
(DMAtag) in the data. This releases the CPU from simple memory copying and increases efficiency in using
the cache.


<table>
 <tr>
  <th>DMA Start Address<br/>Reference CONT<br/>Matrix<br/>Texture<br/>REF<br/>REF<br/>Reference NEXT</th>
  <th>Matrix<br/>Texture</th>
 </tr>
 <tr>
  <td>Vertex Next Object</td>
  <td>Vertex</td>
 </tr>
</table>


Figure 1-14 List Processing with DMAC


- 19-

- © SCEI




SCE CONFIDENTIAL EE Overview Version 6.0


(This page is left blank intentionally)


- © SCEI

- 20-




SCE CONFIDENTIAL EE Overview Version 6.0


## 2. Architecture Overview


- 21-

- © SCEI




SCE CONFIDENTIAL EE Overview Version 6.0


## 2.1. EE Block Configuration


The block diagram and main specifications of the EE are shown below.


<table>
 <tr>
  <th>cop1 EE Core CPU</th>
  <th></th>
  <th>VPU0 cop2</th>
  <th>VPU1</th>
 </tr>
 <tr>
  <td>FPU</td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td></td>
  <td>128</td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
  <td>GIF GS</td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td>MEM MEM</td>
  <td>MEM MEM</td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td>Controller</td>
  <td></td>
  <td>VIF</td>
  <td>VIF</td>
 </tr>
 <tr>
  <td>128</td>
  <td></td>
  <td>128</td>
  <td>128</td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
</table>


Timer SIO


10ch DMA
Controller


IPU Memory I/O
Interface Interface


External Memory


Figure 2-1 EE Block Diagram


Peripherals


- © SCEI

- 22-




SCE CONFIDENTIAL


EE Overview Version 6.0


## Main Specifications


<table>
 <tr>
  <th>Block</th>
  <th>Name</th>
  <th>Contents</th>
 </tr>
 <tr>
  <td rowspan="4">CPU</td>
  <td>Core</td>
  <td>2-way superscalar<br/>Data bus 128 bits (64 bits x 2)<br/>Internal bus 128 bits<br/>Internal 128 bits x 32</td>
 </tr>
 <tr>
  <td>CACHE</td>
  <td>I-Cache 16 KB 2-way set associative<br/>D-Cache 8 KB 2-way set associative with<br/>line lock<br/>Scratchpad RAM (SPR) 16 KB</td>
 </tr>
 <tr>
  <td>MMU</td>
  <td>48-double-entry TLB<br/>32-bit address conversion</td>
 </tr>
 <tr>
  <td>Instruction set</td>
  <td>64 bits, conforms to MIPS III (partly to MIPS IV)<br/>128-bit parallel multimedia instruction set<br/>3-operand multiply/multiply-add calculation instruction<br/>Interrupt enable/disable instruction</td>
 </tr>
 <tr>
  <td rowspan="2">Coprocessors</td>
  <td>FPU</td>
  <td>32-bit single-precision floating-point multiply-add arithmetic<br/>logical unit<br/>32-bit divide calculator</td>
 </tr>
 <tr>
  <td>VPU0</td>
  <td>32-bit single-precision floating-point multiply-add arithmetic<br/>logical unit x 4<br/>32-bit single-precision floating-point divide calculator x 1<br/>Data unpacking function (VIF)<br/>Programmable LIW DSP<br/>Internal bus 128 bits</td>
 </tr>
 <tr>
  <td>Coordinate<br/>engine</td>
  <td>VPU1</td>
  <td>32-bit single-precision floating-point multiply-add arithmetic<br/>logical unit x 5<br/>32-bit single-precision floating-point divide calculator x 2<br/>Data unpacking function (VIF)<br/>Programmable LIW DSP<br/>Internal bus (data) 128 bits</td>
 </tr>
 <tr>
  <td>Image engine</td>
  <td>IPU</td>
  <td>MPEG2 video layer decoding/bit stream decoding/IDCT/CSC<br/>(Color Conversion)/Dither/ VQ (Vector Quantization)</td>
 </tr>
 <tr>
  <td rowspan="6">Built-in devices</td>
  <td>DMAC</td>
  <td>10ch (transfer between memory and I/O, memory and SPR)</td>
 </tr>
 <tr>
  <td>DRAMC</td>
  <td>RDRAM controller</td>
 </tr>
 <tr>
  <td>INTC</td>
  <td>2 types: INT0 (for interrupt from each device)/INT1 (for<br/>from</td>
 </tr>
 <tr>
  <td>TIMER</td>
  <td>16 bits x 4</td>
 </tr>
 <tr>
  <td>GIF</td>
  <td>256-byte FIFO embedded<br/>Data formatting function<br/>Arbitration (PATH1, 2 and 3)</td>
 </tr>
 <tr>
  <td>SIF</td>
  <td>32-bit (address/data 128-byte FIFO embedded</td>
 </tr>
 <tr>
  <td colspan="2">Main bus</td>
  <td>128 bits</td>
 </tr>
</table>


- 23-

- © SCEI




SCE CONFIDENTIAL EE Overview Version 6.0


## 2.2. EE Core: CPU


## 2.2.1. EE Core Features


The EE Core is a processor that implements the superscalar 64-bit MIPS IV instruction set architecture. In
particular, 128-bit parallel processing for multimedia applications has been greatly expanded.
The EE Core is composed of the CPU, a floating-point execution unit (Coprocessor 1), an instruction cache, a
data cache, scratchpad RAM, and a tightly coupled vector operation unit (Coprocessor 2).
The CPU has two pipelines and can decode two instructions in each cycle. Instructions are executed and
completed in order. However, since data cache misses are not blocked and a single cache miss does not stall the
pipelines, a load miss or non-cached load completion may occur out of order. Completion of Multiply, Multiply-
Add, Divide, Prefetch, and Coprocessor instructions may also occur out of order. The above features are
summarized as follows:


- 2-way superscalar pipelines

- 128-bit (64 bits x 2) data path and 128-bit system bus

- Instruction set - 64-bit instruction set conforming to MIPS III and partly conforming to MIPS IV (Prefetch instruction and conditional move instructions) - Non-blocking load instructions - Three-operand Multiply and Multiply-Add instructions - 128-bit multimedia instructions (Parallel processing of 64 bits x 2, 32 bits x 4, 16 bits x 8, or 8 bits x 16) • On-chip caches and scratchpad RAM - Instruction cache: 16 KB, 2-way set associative - Data cache: 8 KB, 2-way set associative (with a write back protocol) - Data scratchpad RAM: 16 KB - Data cache line lock function - Prefetch function • MMU - 48-double-entry full-set-associative address translation look-aside buffer (TLB)

- © SCEI

- 24-




SCE CONFIDENTIAL EE Overview Version 6.0


## 2.2.2. Memory Map


<table>
 <tr>
  <th>1fc0_0000 Boot ROM (Max. 4MB)<br/>Reserved</th>
  <th>ffff_ffff</th>
  <th rowspan="2">NO Mount</th>
 </tr>
 <tr>
  <td></td>
  <td>8000_0000</td>
 </tr>
 <tr>
  <td>1200_0000 GS Registers<br/>1000_0000 EE Registers</td>
  <td></td>
  <td rowspan="2">Extend<br/>Main Memory<br/>(Max. 1GB)</td>
 </tr>
 <tr>
  <td></td>
  <td>4000_0000</td>
 </tr>
 <tr>
  <td>(Max. 256MB)</td>
  <td></td>
  <td rowspan="2">NO Mount</td>
 </tr>
 <tr>
  <td></td>
  <td>2000_0000</td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td rowspan="2">System</td>
 </tr>
 <tr>
  <td></td>
  <td></td>
 </tr>
</table>


ffff_ffff


Physical Memory


Figure 2-2 EE Core Memory Map


KSEG1
System


KSEG0
System


## Kernel Mode


c000_0000


a000_0000


8000_0000


- 25-

- © SCEI




SCE CONFIDENTIAL EE Overview Version 6.0


## 2.2.3. Instruction Set Overview


The EE Core has an instruction set consisting of the MIPS III instruction set, part of the MIPS IV instruction
set, 128-bit multimedia instructions, three-operand multiply instructions, I1 pipe operation instructions, and
others. The EE Core instructions are listed below.


## Integer Add/Subtract


<table>
 <tr>
  <th>Instruction</th>
  <th>Fuction</th>
  <th>Level</th>
 </tr>
 <tr>
  <td>ADD</td>
  <td>Add Word</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>ADDI</td>
  <td>Add Immediate Word</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>ADDIU</td>
  <td>Add Immediate Unsigned Word</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>ADDU</td>
  <td>Add Unsigned Word</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>DADD</td>
  <td>Doubleword Add</td>
  <td>MIPS III</td>
 </tr>
 <tr>
  <td>DADDI</td>
  <td>Doubleword Add Immediate</td>
  <td>MIPS III</td>
 </tr>
 <tr>
  <td>DADDIU</td>
  <td>Doubleword Add Immediate Unsigned</td>
  <td>MIPS III</td>
 </tr>
 <tr>
  <td>DADDU</td>
  <td>Doubleword Add Unsigned</td>
  <td>MIPS III</td>
 </tr>
 <tr>
  <td>DSUB</td>
  <td>Doubleword Subtract</td>
  <td>MIPS III</td>
 </tr>
 <tr>
  <td>DSUBU</td>
  <td>Doubleword Subtract Unsigned</td>
  <td>MIPS III</td>
 </tr>
 <tr>
  <td>SUB</td>
  <td>Subtract Word</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>SUBU</td>
  <td>Subtract Unsigned Word</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>PADDB</td>
  <td>Parallel Add Byte</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PADDH</td>
  <td>Parallel Add Halfword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PADDSB</td>
  <td>Parallel Add with Signed Saturation Byte</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PADDSH</td>
  <td>Parallel Add with Signed Saturation Halfword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PADDSW</td>
  <td>Parallel Add with Signed Saturation Word</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PADDUB</td>
  <td>Parallel Add with Unsigned Saturation Byte</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PADDUH</td>
  <td>Parallel Add with Unsigned Saturation Halfword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PADDUW</td>
  <td>Parallel Add with Unsigned Saturation Word</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PADDW</td>
  <td>Parallel Add Word</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PADSBH</td>
  <td>Parallel Add/Subtract Halfword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PSUBB</td>
  <td>Parallel Subtract Byte</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PSUBH</td>
  <td>Parallel Subtract Halfword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PSUBSB</td>
  <td>Parallel Subtract with Signed Saturation Byte</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PSUBSH</td>
  <td>Parallel Subtract with Signed Saturation Halfword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PSUBSW</td>
  <td>Parallel Subtract with Signed Saturation Word</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PSUBUB</td>
  <td>Parallel Subtract with Unsigned Saturation Byte</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PSUBUH</td>
  <td>Parallel Subtract with Unsigned Saturation Halfword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PSUBUW</td>
  <td>Parallel Subtract with Unsigned Saturation Word</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PSUBW</td>
  <td>Parallel Subtract Word</td>
  <td>128-bit MMI</td>
 </tr>
</table>


- © SCEI

- 26-




SCE CONFIDENTIAL


## Integer Multiply/Divide


<table>
 <tr>
  <th>Instruction</th>
  <th>Function</th>
  <th>Level</th>
 </tr>
 <tr>
  <td>DIV</td>
  <td>Divide Word</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>DIV1</td>
  <td>Divide Word Pipeline 1</td>
  <td>EE Core</td>
 </tr>
 <tr>
  <td>DIVU</td>
  <td>Divide Unsigned Word</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>DIVU1</td>
  <td>Divide Unsigned Word Pipeline 1</td>
  <td>EE Core</td>
 </tr>
 <tr>
  <td>MULT</td>
  <td>Multiply Word</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>MULTU</td>
  <td>Multiply Unsigned Word</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>MULT1</td>
  <td>Multiply Word Pipeline 1</td>
  <td>EE Core</td>
 </tr>
 <tr>
  <td>MULTU1</td>
  <td>Multiply Unsigned Word Pipeline 1</td>
  <td>EE Core</td>
 </tr>
 <tr>
  <td>PDIVBW</td>
  <td>Parallel Divide Broadcast Word</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PDIVUW</td>
  <td>Parallel Divide Unsigned Word</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PDIVW</td>
  <td>Parallel Divide Word</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PMULTH</td>
  <td>Parallel Multiply Halfword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PMULTUW</td>
  <td>Parallel Multiply Unsigned Word</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PMULTW</td>
  <td>Parallel Multiply Word</td>
  <td>128-bit MMI</td>
 </tr>
</table>


## Integer Multiply-Add


<table>
 <tr>
  <th>Instruction</th>
  <th>Function</th>
  <th>Level</th>
 </tr>
 <tr>
  <td>MADD</td>
  <td>Multiply-Add word</td>
  <td>EE Core</td>
 </tr>
 <tr>
  <td>MADD1</td>
  <td>Multiply-Add word Pipeline 1</td>
  <td>EE Core</td>
 </tr>
 <tr>
  <td>MADDU</td>
  <td>Multiply-Add Unsigned word</td>
  <td>EE Core</td>
 </tr>
 <tr>
  <td>MADDU1</td>
  <td>Multiply-Add Unsigned word Pipeline 1</td>
  <td>EE Core</td>
 </tr>
 <tr>
  <td>PHMADH</td>
  <td>Parallel Horizontal Multiply-Add Halfword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PHMSBH</td>
  <td>Parallel Horizontal Multiply-Subtract Halfword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PMADDH</td>
  <td>Parallel Multiply-Add Halfword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PMADDUW</td>
  <td>Parallel Multiply-Add Unsigned Word</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PMADDW</td>
  <td>Parallel Multiply-Add Word</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PMSUBH</td>
  <td>Parallel Multiply-Subtract Halfword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PMSUBW</td>
  <td>Parallel Multiply-Subtract Word</td>
  <td>128-bit MMI</td>
 </tr>
</table>


## Floating-Point


<table>
 <tr>
  <th>Instruction</th>
  <th>Function</th>
  <th>Level</th>
 </tr>
 <tr>
  <td>ADD.S</td>
  <td>Floating Point Add</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>ADDA.S</td>
  <td>Floating Point Add to Accumulator</td>
  <td>EE Core</td>
 </tr>
 <tr>
  <td>MADD.S</td>
  <td>Floating Point Multiply-Add</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>MADDA.S</td>
  <td>Floating Point Multiply and Add to Accumulator</td>
  <td>EE Core</td>
 </tr>
 <tr>
  <td>MUL.S</td>
  <td>Floating Point Multiply</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>MULA.S</td>
  <td>Floating Point Multiply to Accumulator</td>
  <td>EE Core</td>
 </tr>
 <tr>
  <td>MSUB.S</td>
  <td>Floating Point Multiply and Subtract</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>MSUBA.S</td>
  <td>Floating Point Multiply and Subtract from Accumulator</td>
  <td>EE Core</td>
 </tr>
 <tr>
  <td>SUB.S</td>
  <td>Floating Point Subtract</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>SUBA.S</td>
  <td>Floating Point Subtract to Accumulator</td>
  <td>EE Core</td>
 </tr>
</table>


EE Overview Version 6.0


- 27-

- © SCEI




SCE CONFIDENTIAL


## Shift


<table>
 <tr>
  <th>Instruction</th>
  <th>Function</th>
  <th>Level</th>
 </tr>
 <tr>
  <td>DSRA</td>
  <td>Doubleword Shift Right Arithmetic</td>
  <td>MIPS III</td>
 </tr>
 <tr>
  <td>DSLL</td>
  <td>Doubleword Shift Left Logical</td>
  <td>MIPS III</td>
 </tr>
 <tr>
  <td>DSLL32</td>
  <td>Doubleword Shift Left Logical Plus 32</td>
  <td>MIPS III</td>
 </tr>
 <tr>
  <td>DSLLV</td>
  <td>Doubleword Shift Left Logical Variable</td>
  <td>MIPS III</td>
 </tr>
 <tr>
  <td>DSRA32</td>
  <td>Doubleword Shift Right Arithmetic Plus 32</td>
  <td>MIPS III</td>
 </tr>
 <tr>
  <td>DSRAV</td>
  <td>Doubleword Shift Right Arithmetic Variable</td>
  <td>MIPS III</td>
 </tr>
 <tr>
  <td>DSRL</td>
  <td>Doubleword Shift Right Logical</td>
  <td>MIPS III</td>
 </tr>
 <tr>
  <td>DSRL32</td>
  <td>Doubleword Shift Right Logical Plus 32</td>
  <td>MIPS III</td>
 </tr>
 <tr>
  <td>DSRLV</td>
  <td>Doubleword Shift Right Logical Variable</td>
  <td>MIPS III</td>
 </tr>
 <tr>
  <td>SLL</td>
  <td>Shift Word Left Logical</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>SLLV</td>
  <td>Shift Word Left Logical Variable</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>SRA</td>
  <td>Shift Word Right Arithmetic</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>SRAV</td>
  <td>Shift Word Right Arithmetic Variable</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>SRL</td>
  <td>Shift Word Right Logical</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>SRLV</td>
  <td>Shift Word Right Logical Variable</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>PSLLH</td>
  <td>Parallel Shift Left Logical Halfword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PSLLVW</td>
  <td>Parallel Shift Left Logical Variable Word</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PSLLW</td>
  <td>Parallel Shift Left Logical Word</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PSRAH</td>
  <td>Parallel Shift Right Arithmetic Halfword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PSRAVW</td>
  <td>Parallel Shift Right Arithmetic Variable Word</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PSRAW</td>
  <td>Parallel Shift Right Arithmetic Word</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PSRLH</td>
  <td>Parallel Shift Right Logical Halfword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PSRLVW</td>
  <td>Parallel Shift Right Logical Variable Word</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PSRLW</td>
  <td>Parallel Shift Right Logical Word</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>QFSRV</td>
  <td>Quadword Funnel Shift Right Variable</td>
  <td>128-bit MMI</td>
 </tr>
</table>


## Logical


<table>
 <tr>
  <th>Instruction</th>
  <th>Function</th>
  <th>Level</th>
 </tr>
 <tr>
  <td>AND</td>
  <td>And</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>ANDI</td>
  <td>And Immediate</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>NOR</td>
  <td>Not Or</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>OR</td>
  <td>Or</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>ORI</td>
  <td>Or Immediate</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>XOR</td>
  <td>Exclusive OR</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>XORI</td>
  <td>Exclusive OR Immediate</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>PAND</td>
  <td>Parallel And</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PNOR</td>
  <td>Parallel Not Or</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>POR</td>
  <td>Parallel Or</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PXOR</td>
  <td>Parallel Exclusive OR</td>
  <td>128-bit MMI</td>
 </tr>
</table>


- © SCEI

- 28-

- EE Overview Version 6.0




SCE CONFIDENTIAL


## Compare


<table>
 <tr>
  <th>Instruction</th>
  <th>Function</th>
  <th>Level</th>
 </tr>
 <tr>
  <td>SLTI</td>
  <td>Set on Less Than Immediate</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>SLTIU</td>
  <td>Set on Less Than Immediate Unsigned</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>SLTU</td>
  <td>Set on Less Than Unsigned</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>PCEQB</td>
  <td>Parallel Compare for Equal Byte</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PCEQH</td>
  <td>Parallel Compare for Equal Halfword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PCEQW</td>
  <td>Parallel Compare for Equal Word</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PCGTB</td>
  <td>Parallel Compare for Greater Than Byte</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PCGTH</td>
  <td>Parallel Compare for Greater Than Halfword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PCGTW</td>
  <td>Parallel Compare for Greater Than Word</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>C.EQ.S</td>
  <td>Floating Point Compare (Equal)</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>C.F.S</td>
  <td>Floating Point Compare (False)</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>C.LE.S</td>
  <td>Floating Point Compare (Less than or Equal)</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>C.LT.S</td>
  <td>Floating Point Compare (Less than)</td>
  <td>MIPS I</td>
 </tr>
</table>


Min/Max


<table>
 <tr>
  <th>Instruction</th>
  <th>Function</th>
  <th>Level</th>
 </tr>
 <tr>
  <td>PMAXH</td>
  <td>Parallel Maximize Halfword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PMAXW</td>
  <td>Parallel Maximize Word</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PMINH</td>
  <td>Parallel Minimize Halfword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PMINW</td>
  <td>Parallel Minimize Word</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>MAX.S</td>
  <td>Floating Point Maximum</td>
  <td>EE Core</td>
 </tr>
 <tr>
  <td>MIN.S</td>
  <td>Floating Point Minimum</td>
  <td>EE Core</td>
 </tr>
</table>


## Data Format Conversion


<table>
 <tr>
  <th>Instruction</th>
  <th>Function</th>
  <th>Level</th>
 </tr>
 <tr>
  <td>PEXT5</td>
  <td>Parallel Extend Upper from 5 bits</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PPAC5</td>
  <td>Parallel Pack to 5 bits</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>CVT.S.W</td>
  <td>Fixed point Convert to Single Floating Point</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>CVT.W.S</td>
  <td>Floating point Convert to Word Fixed-Point</td>
  <td>MIPS I</td>
 </tr>
</table>


EE Overview Version 6.0


- 29-

- © SCEI




SCE CONFIDENTIAL


## Reordering


<table>
 <tr>
  <th>Instruction</th>
  <th>Function</th>
  <th>Level</th>
 </tr>
 <tr>
  <td>PCPYH</td>
  <td>Parallel Copy Halfword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PCPYLD</td>
  <td>Parallel Copy Lower Doubleword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PCPYUD</td>
  <td>Parallel Copy Upper Doubleword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PEXCH</td>
  <td>Parallel Exchange Center Halfword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PEXCW</td>
  <td>Parallel Exchange Center Word</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PEXEH</td>
  <td>Parallel Exchange Even Halfword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PEXEW</td>
  <td>Parallel Exchange Even Word</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PEXTLB</td>
  <td>Parallel Extend Lower from Byte</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PEXTLH</td>
  <td>Parallel Extend Lower from Halfword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PEXTLW</td>
  <td>Parallel Extend Lower form Word</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PEXTUB</td>
  <td>Parallel Extend Upper from Byte</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PEXTUH</td>
  <td>Parallel Extend Upper from Halfword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PEXTUW</td>
  <td>Parallel Extend Upper from Word</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PINTEH</td>
  <td>Parallel Interleave Even Halfword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PINTH</td>
  <td>Parallel Interleave Halfword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PPACB</td>
  <td>Parallel Pack to Byte</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PPACH</td>
  <td>Parallel Pack to Halfword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PPACW</td>
  <td>Parallel Pack to Word</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PREVH</td>
  <td>Parallel Reverse Halfword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PROT3W</td>
  <td>Parallel Rotate 3 Words</td>
  <td>128-bit MMI</td>
 </tr>
</table>


## Others


<table>
 <tr>
  <th>Instruction</th>
  <th>Function</th>
  <th>Level</th>
 </tr>
 <tr>
  <td>PABSH</td>
  <td>Parallel Absolute Halfword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PABSW</td>
  <td>Parallel Absolute Word</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PLZCW</td>
  <td>Parallel Leading Zero or One Count Word</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>ABS.S</td>
  <td>Floating Point Absolute Value</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>NEG.S</td>
  <td>Floating Point Negate</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>RSQRT.S</td>
  <td>Floating Point Reciprocal Root</td>
  <td>MIPS IV</td>
 </tr>
 <tr>
  <td>SQRT.S</td>
  <td>Floating Point Square Root</td>
  <td>MIPS II</td>
 </tr>
</table>


EE Overview Version 6.0


- © SCEI

- 30-




SCE CONFIDENTIAL


## Register-Register Transfer


<table>
 <tr>
  <th>Instruction</th>
  <th>Function</th>
  <th>Level</th>
 </tr>
 <tr>
  <td>MFHI</td>
  <td>Move from HI Register</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>MFLO</td>
  <td>Move from LO Register</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>MOVN</td>
  <td>Move Conditional on Not Zero</td>
  <td>MIPS IV</td>
 </tr>
 <tr>
  <td>MOVZ</td>
  <td>Move Conditional on Zero</td>
  <td>MIPS IV</td>
 </tr>
 <tr>
  <td>MTHI</td>
  <td>Move to HI Register</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>MTLO</td>
  <td>Move to LO Register</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>MFHI1</td>
  <td>Move From HI1 Register</td>
  <td>EE Core</td>
 </tr>
 <tr>
  <td>MFLO1</td>
  <td>Move From LO1 Register</td>
  <td>EE Core</td>
 </tr>
 <tr>
  <td>MTHI1</td>
  <td>Move To HI1 Register</td>
  <td>EE Core</td>
 </tr>
 <tr>
  <td>MTLO1</td>
  <td>Move to LO1 Register</td>
  <td>EE Core</td>
 </tr>
 <tr>
  <td>PMFHI</td>
  <td>Parallel Move From HI Register</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PMFHL</td>
  <td>Parallel Move from HI/LO Register</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PMFLO</td>
  <td>Parallel Move from LO Register</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PMTHI</td>
  <td>Parallel Move To HI Register</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PMTHL</td>
  <td>Parallel Move To HI/LO Register</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>PMTLO</td>
  <td>Parallel Move To LO Register</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>MFC1</td>
  <td>Move Word from Floating Point</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>MOV.S</td>
  <td>Floating Point Move</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>MTC1</td>
  <td>Move Word to Floating Point</td>
  <td>MIPS I</td>
 </tr>
</table>


Load from Memory


<table>
 <tr>
  <th>Instruction</th>
  <th>Function</th>
  <th>Level</th>
 </tr>
 <tr>
  <td>LB</td>
  <td>Load Byte</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>LBU</td>
  <td>Load Byte Unsigned</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>LD</td>
  <td>Load Doubleword</td>
  <td>MIPS III</td>
 </tr>
 <tr>
  <td>LDL</td>
  <td>Load Doubleword Left</td>
  <td>MIPS III</td>
 </tr>
 <tr>
  <td>LDR</td>
  <td>Load Doubleword Right</td>
  <td>MIPS III</td>
 </tr>
 <tr>
  <td>LH</td>
  <td>Load Halfword</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>LHU</td>
  <td>Load Halfword Unsigned</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>LUI</td>
  <td>Load Upper Immediate</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>LW</td>
  <td>Load Word</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>LWL</td>
  <td>Load Word Left</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>LWR</td>
  <td>Load Word Right</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>LWU</td>
  <td>Load Word Unsigned</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>LQ</td>
  <td>Load Quadword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>LWC1</td>
  <td>Load Word to Floating Point</td>
  <td>MIPS I</td>
 </tr>
</table>


- 31-

- EE Overview Version 6.0

- © SCEI




SCE CONFIDENTIAL


## Store in Memory


<table>
 <tr>
  <th>Instruction</th>
  <th>Function</th>
  <th>Level</th>
 </tr>
 <tr>
  <td>SB</td>
  <td>Store Byte</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>SD</td>
  <td>Store Doubleword</td>
  <td>MIPS III</td>
 </tr>
 <tr>
  <td>SDL</td>
  <td>Store Doubleword Left</td>
  <td>MIPS III</td>
 </tr>
 <tr>
  <td>SDR</td>
  <td>Store Doubleword Right</td>
  <td>MIPS III</td>
 </tr>
 <tr>
  <td>SH</td>
  <td>Store Halfword</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>SW</td>
  <td>Store Word</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>SWL</td>
  <td>Store Word Left</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>SWR</td>
  <td>Store Word Right</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>SQ</td>
  <td>Store Quadword</td>
  <td>128-bit MMI</td>
 </tr>
 <tr>
  <td>SWC1</td>
  <td>Store Word from Floating Point</td>
  <td>MIPS I</td>
 </tr>
</table>


Special Data Transfer


<table>
 <tr>
  <th>Instruction</th>
  <th>Function</th>
  <th>Level</th>
 </tr>
 <tr>
  <td>MFSA</td>
  <td>Move from Shift Amount Register</td>
  <td>EE Core</td>
 </tr>
 <tr>
  <td>MTSA</td>
  <td>Move to Shift Amount Register</td>
  <td>EE Core</td>
 </tr>
 <tr>
  <td>MTSAB</td>
  <td>Move Byte Count to Shift Amount Register</td>
  <td>EE Core</td>
 </tr>
 <tr>
  <td>MTSAH</td>
  <td>Move Halfword Count to Shift Amount Register</td>
  <td>EE Core</td>
 </tr>
 <tr>
  <td>MFBPC</td>
  <td>Move from Breakpoint Control Register</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>MFCO</td>
  <td>Move from System Control Coprocessor</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>MFDAB</td>
  <td>Move from Data Address Breakpoint register</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>MFDABM</td>
  <td>Move from Data Address Breakpoint Mask Register</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>MFDVB</td>
  <td>Move from Data value Breakpoint Register</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>MFDVBM</td>
  <td>Move from Data Value Breakpoint Mask Register</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>MFIAB</td>
  <td>Move from Instruction Address Breakpoint Register</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>MFIABM</td>
  <td>Move from Instruction Address Breakpoint Mask Register</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>MFPC</td>
  <td>Move from Performance Counter</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>MFPS</td>
  <td>Move from Performance Event Specifier</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>MTBPC</td>
  <td>Move to Breakpoint Control Register</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>MTCO</td>
  <td>Move to System Control Coprocessor</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>MTDAB</td>
  <td>Move to Data Address Breakpoint Register</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>MTDABM</td>
  <td>Move to Data Address Breakpoint Mask Register</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>MTDVB</td>
  <td>Move to Data Value Breakpoint Register</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>MTDVBM</td>
  <td>Move to Data Value Breakpoint Mask Register</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>MTIAB</td>
  <td>Move to Instruction Address Breakpoint Register</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>MTIABM</td>
  <td>Move to Instruction Address Mask Breakpoint Register</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>MTPC</td>
  <td>Move to Performance Counter</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>MTPS</td>
  <td>Move to Performance Event Specifier</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>CFC1</td>
  <td>Move Control Word from Floating Point</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>CTC1</td>
  <td>Move Control Word to Floating Point</td>
  <td>MIPS I</td>
 </tr>
</table>


EE Overview Version 6.0


- © SCEI

- 32-




SCE CONFIDENTIAL


## Conditional Branch and Jump


<table>
 <tr>
  <th>Instruction</th>
  <th>Function</th>
  <th>Level</th>
 </tr>
 <tr>
  <td>BEQ</td>
  <td>Branch on Equal</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>BEQL</td>
  <td>Branch on Equal Likely</td>
  <td>MIPS II</td>
 </tr>
 <tr>
  <td>BGEZ</td>
  <td>Branch on Greater Than or Equal to Zero</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>BGEZL</td>
  <td>Branch on Greater Than or Equal to Zero Likely</td>
  <td>MIPS II</td>
 </tr>
 <tr>
  <td>BGTZ</td>
  <td>Branch on Greater Than Zero</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>BGTZL</td>
  <td>Branch on Greater Than Zero Likely</td>
  <td>MIPS II</td>
 </tr>
 <tr>
  <td>BLEZ</td>
  <td>Branch on Less Than or Equal to Zero</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>BLEZL</td>
  <td>Branch on Less Than or Equal to Zero Likely</td>
  <td>MIPS II</td>
 </tr>
 <tr>
  <td>BLTZ</td>
  <td>Branch on Less Than Zero</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>BLTZL</td>
  <td>Branch on Less Than Zero Likely</td>
  <td>MIPS II</td>
 </tr>
 <tr>
  <td>BNE</td>
  <td>Branch on Not Equal</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>BNEL</td>
  <td>Branch on Not Equal Likely</td>
  <td>MIPS II</td>
 </tr>
 <tr>
  <td>BC0F</td>
  <td>Branch on Coprocessor 0 False</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>BC0FL</td>
  <td>Branch on Coprocessor 0 False Likely</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>BC0T</td>
  <td>Branch on Coprocessor 0 True</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>BC0TL</td>
  <td>Branch on Coprocessor 0 True Likely</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>BC1F</td>
  <td>Branch on FP False</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>BC1FL</td>
  <td>Branch on FP False Likely</td>
  <td>MIPS II</td>
 </tr>
 <tr>
  <td>BC1T</td>
  <td>Branch on FP True</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>BC1TL</td>
  <td>Branch on FP True Likely</td>
  <td>MIPS II</td>
 </tr>
 <tr>
  <td>BC2F</td>
  <td>Branch on Coprocessor 2 False</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>BC2FL</td>
  <td>Branch on Coprocessor 2 False Likely</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>BC2T</td>
  <td>Branch on Coprocessor 2 True</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>BC2TL</td>
  <td>Branch on Coprocessor 2 True Likely</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>J</td>
  <td>Jump</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>JR</td>
  <td>Jump Register</td>
  <td>MIPS I</td>
 </tr>
</table>


<table>
 <tr>
  <th>Instruction</th>
  <th>Function</th>
  <th>Level</th>
 </tr>
 <tr>
  <td>BGEZAL</td>
  <td>Branch on Greater Than or Equal to Zero and Link</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>BGEZALL</td>
  <td>Branch on Greater Than or Equal to Zero and Link Likely</td>
  <td>MIPS II</td>
 </tr>
 <tr>
  <td>BLTZAL</td>
  <td>Branch on Less Than Zero and Link</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>BLTZALL</td>
  <td>Branch on Less Than Zero and Link Likely</td>
  <td>MIPS II</td>
 </tr>
 <tr>
  <td>JAL</td>
  <td>Jump and Link</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>JALR</td>
  <td>Jump and Link Register</td>
  <td>MIPS I</td>
 </tr>
</table>


## Subroutine Call


EE Overview Version 6.0


- 33-

- © SCEI




SCE CONFIDENTIAL EE Overview Version 6.0


## Break and Trap


<table>
 <tr>
  <th>Instruction</th>
  <th>Function</th>
  <th>Level</th>
 </tr>
 <tr>
  <td>BREAK</td>
  <td>Breakpoint</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>SYSCALL</td>
  <td>System Call</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>TEQ</td>
  <td>Trap if Equal</td>
  <td>MIPS II</td>
 </tr>
 <tr>
  <td>TEQI</td>
  <td>Trap if Equal Immediate</td>
  <td>MIPS II</td>
 </tr>
 <tr>
  <td>TGE</td>
  <td>Trap if Greater or Equal</td>
  <td>MIPS II</td>
 </tr>
 <tr>
  <td>TGEI</td>
  <td>Trap if Greater or Equal Immediate</td>
  <td>MIPS II</td>
 </tr>
 <tr>
  <td>TGEIU</td>
  <td>Trap if Greater or Equal Immediate Unsigned</td>
  <td>MIPS II</td>
 </tr>
 <tr>
  <td>TGEU</td>
  <td>Trap if Greater or Equal Unsigned</td>
  <td>MIPS II</td>
 </tr>
 <tr>
  <td>TLT</td>
  <td>Trap if Less Than</td>
  <td>MIPS II</td>
 </tr>
 <tr>
  <td>TLTI</td>
  <td>Trap if Less Than Immediate</td>
  <td>MIPS II</td>
 </tr>
 <tr>
  <td>TLTIU</td>
  <td>Trap if Less Than Immediate Unsigned</td>
  <td>MIPS II</td>
 </tr>
 <tr>
  <td>TLTU</td>
  <td>Trap if Less Than Unsigned</td>
  <td>MIPS II</td>
 </tr>
 <tr>
  <td>TNE</td>
  <td>Trap if Not Equal</td>
  <td>MIPS II</td>
 </tr>
 <tr>
  <td>TNEI</td>
  <td>Trap if Not Equal Immediate</td>
  <td>MIPS II</td>
 </tr>
 <tr>
  <td>ERET</td>
  <td>Exception Return</td>
  <td>MIPS III</td>
 </tr>
</table>


## Others


<table>
 <tr>
  <th>Instruction</th>
  <th>Function</th>
  <th>Level</th>
 </tr>
 <tr>
  <td>SYNC.stype</td>
  <td>Synchronize Shared Memory</td>
  <td>MIPS II</td>
 </tr>
 <tr>
  <td>PREF</td>
  <td>Prefetch</td>
  <td>MIPS IV</td>
 </tr>
 <tr>
  <td>DI</td>
  <td>Disabled Interrupt</td>
  <td>MIPS I</td>
 </tr>
 <tr>
  <td>EI</td>
  <td>Enabled Interrupt</td>
  <td>MIPS I</td>
 </tr>
</table>


- © SCEI

- 34-




SCE CONFIDENTIAL EE Overview Version 6.0


## 2.3. VPU: Vector Operation Processor


The EE has two on-chip vector operation processors with the same architecture, VPU0 and VPU1, for floating-
point vector operation indispensable to geometry processing.
VPU0 is connected to the EE Core via a 128-bit coprocessor bus. The operation resources and registers for
VPU0 can be used directly from the EE Core by using coprocessor instructions and not by using the main bus.
VPU1 is directly connected to the rendering engine, the GS, via the GIF (Graphics Synthesizer Interface Unit).
Display lists generated in VPU1 are not transferred to the GS via the main bus.
VPU0 and VPU1 each have a packet expansion engine called VIF (VPU Interface Unit) at the front end. They
are named VIF0 and VIF1 respectively.


VPU1


FDIV


VIF1


Main Memory


128


Figure 2-3 VPU-Related Block Diagram


## 2.3.1. VPU Architecture


The 2 VPUs basically have the same architecture, consisting of the VU, VU Mem (data memory for VU), and
VIF (compressed-data decompression engine). The VU is a processor unit consisting of several FMACs
(Floating-point Multiply-Add ALUs), FDIV (Floating-point Divide Calculator), 32 four-parallel floating-point
registers, 16 integer registers, and a Micro Mem (program memory). It loads data from the VU Mem in 128-bit
units (single-precision floating-point x 4), performs operations according to microprograms placed in the Micro
Mem, and stores the results in the VU Mem.
Microprograms use a 64-bit-long LIW (Long Instruction Word) instruction set, and can concurrently execute
floating-point multiply-add operations in the Upper 32-bit field (Upper instruction field) and floating-point
divide or integer operations in the Lower 32-bit field (Lower instruction field).


- 35-

- © SCEI




SCE CONFIDENTIAL EE Overview Version 6.0


<table>
 <tr>
  <th colspan="3">Micro instruction fetch unit<br/>Vector Unit : VU Micro Mem<br/>4 KBytes or 16 KBytes<br/>63 64 0<br/>bold line : 128 bits Upper Instruction Lower Instruction<br/>32 32</th>
  <th>16<br/>16</th>
  <th>special<br/>registers<br/>VI16~VI31<br/>COP2<br/>control<br/>registers</th>
  <th rowspan="3">32<br/>CFC2<br/>32<br/>CTC2</th>
 </tr>
 <tr>
  <td colspan="4">QMFC2 127 floating 0 FMACw Upper FMACz Execution FMACy Unit FMACx FDIV RANDU/etc Lower Execution Unit<br/>/ SQC2<br/>QMTC2 registers LSU IALU EFU BRU<br/>/ LQC2 VF00~VF31<br/>(COP2 data registers)</td>
  <td>integer<br/>registers<br/>VI00~VI15</td>
 </tr>
 <tr>
  <td colspan="4">16</td>
  <td></td>
 </tr>
</table>


Vector Processing Unit :
VPU


VU Mem


4 KBytes or 16 KBytes


External Units


Figure 2-4 VU Block Diagram


VIF


Following are brief descriptions of the VPU units.


## FMAC


This unit handles add/subtract, multiply, and multiply-add of the floating-point numbers. FMACx, FMACy,
FMACz, and FMACw are mounted to execute four-element vector operations efficiently. The latency of
instructions which use the FMAC has been unified at four cycles to increase the efficiency of pipeline
processing.


## FDIV


This unit performs self-synchronous type floating-point divide/square root operations. FDIV operations
differ from others in latency, so the results are stored in the Q register.


## LSU


This unit controls loading and storing to and from VU Mem.
Load/Store must be performed in units of 128 bits, but can be


masked in units of x, y, z and w fields.


## IALU


This unit performs 16-bit integer operations.
Loop counter operations and load/store address calculations are performed in conjunction with the integer
register.


## BRU


This unit controls jump and conditional branch.


## RANDU


This unit generates random numbers. Random numbers are generated by the M sequence and stored in the
R register.


- © SCEI

- 36-




SCE CONFIDENTIAL EE Overview Version 6.0


## EFU


This is an elementary function unit, which executes operations such as exponential and trigonometric
functions. This unit is mounted only on VU1. Operation results are stored in the P register.


## Floating-Point Registers


32 128-bit floating-point registers (VF00 - VF31) are mounted. Each register can be divided into 4 fields of
x, y, z, and w, and is equivalent to a vector of four single-precision floating-point numbers. VF00 is a
constant register.


## Integer Registers


Sixteen 16-bit integer registers (VI00 - VI15) are mounted. These registers are used as loop counters, and
used for load/store address calculations. VI00 is a constant register.


## VU Mem


This is data memory for the VU's exclusive use. Memory capacity is 4 Kbytes for VU0 and 16 Kbytes for
VU1. This memory is connected to the LSU at a width of 128 bits, and addresses are aligned on qword
boundaries.
Address
0x0000 w z y x
0x0010 w z y x
:
:


0x0ff0 w z y x


:
Implemented on VU1 only
:


0x3ff0 w z y x


Figure 2-5 VU Mem Memory Map


Furthermore, VU1 registers are mapped to addresses 0x4000 to 0x43ff in VU0.


## Micro Mem


This is on-chip memory, which stores microinstruction programs. Memory capacity is 4 Kbytes in VU0 and
16 Kbytes in VU1.
Address
0x0000 Upper Lower
0x0008 Upper Lower
:
:


0x0ff8 Upper Lower


:
Implemented on VU1 only
:


0x3ff8 Upper Lower


Figure 2-6 Micro Mem Memory Map


- 37-

- © SCEI




SCE CONFIDENTIAL EE Overview Version 6.0


## 2.3.2. VPU0


VPU0 has a macro mode, which operates according to coprocessor instructions from the EE Core, and a micro
mode, which operates independently according to microprograms stored in the Micro Mem. Almost all the
instructions used in micro mode are also defined as coprocessor instructions, and are executable directly from
the EE Core. Similarly, VPU0 registers can be referred to directly from the EE Core with coprocessor transfer
instructions.
VPU0 is tightly coupled with the EE Core as mentioned above, and takes charge of relatively small-sized
processing.


<table>
 <tr>
  <th rowspan="2">EE Core</th>
  <th></th>
  <th colspan="2" rowspan="2">Upper Exec. Unit Lower Exec.Unit<br/> FMAC FDIV</th>
 </tr>
 <tr>
  <td></td>
 </tr>
 <tr>
  <td>Interface</td>
  <td></td>
  <td colspan="2">Floating 128bit   </td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td>Micro Mem<br/>Integer 4KB<br/>Regs</td>
  <td>VU Mem<br/>4KB</td>
 </tr>
 <tr>
  <td colspan="2">Main Bus</td>
  <td colspan="2">VIF<br/>VPU0</td>
 </tr>
</table>


Figure 2-7 VPU0 Block Diagram


## 2.3.3. VPU1


VPU1 operates only in micro mode. VPU1 has a larger Micro Mem and VU Mem than VPU0, and is equipped
with an EFU. It is also directly connected to the GIF, and has additional synchronization control instructions
such as transfer to the GIF. Furthermore, it structures double buffers in VU Mem and has additional functions
to perform data transfer and operations in parallel.
As mentioned above, VPU1 operates autonomously as a geometry engine independently of the EE Core. High-
speed processing is possible with VPU1, but because of the limits of complexity of what it can process, it divides
processing of standard three-dimensional graphics.
VPU1 operation results are transferred from VU Mem1 to the GS via the GIF, with the highest priority.


- © SCEI

- 38-




SCE CONFIDENTIAL EE Overview Version 6.0


<table>
 <tr>
  <th></th>
  <th></th>
  <th></th>
 </tr>
</table>


64


Upper Exec.Unit


Lower Exec.Unit


GIF


128 FMAC FDIV Path1 Path2 Path3


128 bit
Floating
Regs


32 32 128


128 128


128


Micro Mem
16KB


VU Mem
16KB


Integer
Regs


VIF


VPU1


Main Bus


Figure 2-8 VPU1 Block Diagram


## 2.3.4. VIF: VPU Interface


The VIF functions as a preprocessor for the VPU. The VIF unpacks the packed vertex data, based on the
specification of the tag (VIFtag) at the start of the data, and transfers it to the data memory (VU Mem) of the
VPU. As a result, in addition to reducing the data size in main memory, the VIF removes the load in data
formatting from the VPU, which has low degree of programming freedom.
The VIF also stores microprograms in Micro Mem and transfers DIRECT data to the GIF according to the
VIFtag specification.


## 2.3.5. Operation Mode and Programming Model


The VU has two execution modes, micro mode and macro mode. In micro mode, the VU functions as a stand-
alone processor and executes microprograms stored in Micro Mem. VU1 operates in this mode. In macro
mode, the VU executes macroinstructions as COP2 (Coprocessor 2) of the EE Core. VU0 operates primarily in
this mode.
Microinstructions are LIW (Long Instruction Word) instructions of 32 bits x 2, and can concurrently execute an
Upper instruction, which uses the upper 32 bits of the instruction word, and a Lower instruction, which uses the
lower 32 bits of the instruction word. The Upper instruction controls the FMAC, and the Lower instruction
controls operations which use the FDIV/EFU/LSU/BRU and integer registers. In the Upper instruction, 4
FMACs are operable concurrently with 1 instruction, and a four-dimensional vector calculation can be made in 1
cycle (throughput).


<table>
 <tr>
  <th colspan="2">Upper Instruction Lower Instruction</th>
 </tr>
 <tr>
  <td>MUL VF01,VF02,VF03</td>
  <td>SQ VF04, VI01</td>
 </tr>
 <tr>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td>FMAC<br/>FMAC<br/>FMAC<br/>FMAC</td>
  <td>FDIV LSU<br/>VU Mem</td>
 </tr>
</table>


Figure 2-9 Upper Instruction and Lower Instruction


- 39-

- © SCEI




SCE CONFIDENTIAL EE Overview Version 6.0


<table>
 <tr>
  <th></th>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th>+ add</th>
  <td>4</td>
  <td>1</td>
 </tr>
 <tr>
  <th>divide</th>
  <td>7</td>
  <td>7</td>
 </tr>
 <tr>
  <th>4 x 4 matrix * 4-row vector</th>
  <td>8</td>
  <td>4</td>
 </tr>
 <tr>
  <th>4 x 4 matrix * 4 x 4 matrix</th>
  <td>20</td>
  <td>16</td>
 </tr>
 <tr>
  <th>1 vertex * vector +</th>
  <td>19</td>
  <td>8</td>
 </tr>
</table>


Some microinstructions do not have macroinstruction equivalents. Macro mode cannot execute the Upper
instruction and Lower instruction at the same time, either. However, macroinstructions can execute the
CALLMS instruction, which executes a microinstruction program in Micro Mem like a subroutine, and the
COP2 data transfer instruction, which transfers data to the VU registers.


<table>
 <tr>
  <th></th>
  <th>Micro Mode (VU1)</th>
  <th>Macro Mode (VU0)</th>
 </tr>
 <tr>
  <td></td>
  <td>as a stand-alone</td>
  <td>as a of the EE Core</td>
 </tr>
 <tr>
  <td>Operation<br/>code</td>
  <td>64-bit-long LIW instruction</td>
  <td>32-bit MIPS COP2 instruction</td>
 </tr>
 <tr>
  <td>Instruction set</td>
  <td>Upper instruction + Lower instruction<br/>(Can be specified concurrently)<br/>EFU instruction<br/>External unit control instruction</td>
  <td>Upper instruction<br/>Lower instruction (partial)<br/>VCALLMS, VCALLMSR instruction<br/>COP2 transfer instruction</td>
 </tr>
 <tr>
  <td>Total number<br/>of instructions</td>
  <td>127 instructions</td>
  <td>90 instructions</td>
 </tr>
 <tr>
  <td>EFU</td>
  <td>Usable as an</td>
  <td>Not</td>
 </tr>
 <tr>
  <td>Register</td>
  <td>Floating-point register: 32 x 128 bits<br/>Integer register: 16<br/>Special register: ACC, I, Q, R (, P)</td>
  <td>Floating-point register: 32 x 128 bits<br/>Integer register: 16<br/>Special register: ACC, I, Q, R<br/>Control register: 16</td>
 </tr>
</table>


## 2.3.6. VPU Instruction Set Overview


VPU microinstructions/macroinstructions are listed below.
Floating-Point Operation
Microinstruction


<table>
 <tr>
  <th colspan="2">Microinstruction</th>
  <td>Macro-instruction</td>
  <td>Function</td>
 </tr>
 <tr>
  <th>Upper</th>
  <td>Lower</td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th>ABS</th>
  <td>-</td>
  <td>VABS</td>
  <td>absolute</td>
 </tr>
 <tr>
  <th>ADD</th>
  <td>-</td>
  <td>VADD</td>
  <td>addition</td>
 </tr>
 <tr>
  <th>ADDA</th>
  <td>-</td>
  <td>VADDA</td>
  <td>ADD output to ACC</td>
 </tr>
 <tr>
  <th>ADDAbc</th>
  <td>-</td>
  <td>VADDAbc</td>
  <td>ADD output to ACC broadcast bc field</td>
 </tr>
 <tr>
  <th>ADDAi</th>
  <td>-</td>
  <td>VADDAi</td>
  <td>ADD output to ACC broadcast I register</td>
 </tr>
 <tr>
  <th>ADDAq</th>
  <td>-</td>
  <td>VADDAq</td>
  <td>ADD output to ACC broadcast Q register</td>
 </tr>
 <tr>
  <th>ADDbc</th>
  <td>-</td>
  <td>VADDbc</td>
  <td>ADD broadcast bc field</td>
 </tr>
 <tr>
  <th>ADDi</th>
  <td>-</td>
  <td>VADDi</td>
  <td>ADD broadcast I register</td>
 </tr>
 <tr>
  <th>ADDq</th>
  <td>-</td>
  <td>VADDq</td>
  <td>ADD broadcast Q register</td>
 </tr>
 <tr>
  <th>-</th>
  <td>DIV</td>
  <td>VDIV</td>
  <td>floating divide</td>
 </tr>
 <tr>
  <th>MADD</th>
  <td>-</td>
  <td>VMADD</td>
  <td>MUL and ADD</td>
 </tr>
 <tr>
  <th>MADDA</th>
  <td>-</td>
  <td>VMADDA</td>
  <td>MUL and ADD output to ACC</td>
 </tr>
 <tr>
  <th>MADDAbc</th>
  <td>-</td>
  <td>VMADDAbc</td>
  <td>MUL and ADD output to ACC broadcast bc field</td>
 </tr>
 <tr>
  <th>MADDAi</th>
  <td>-</td>
  <td>VMADDAi</td>
  <td>MUL and ADD output to ACC broadcast I register</td>
 </tr>
 <tr>
  <th>MADDAq</th>
  <td>-</td>
  <td>VMADDAq</td>
  <td>MUL and ADD output to ACC broadcast Q register</td>
 </tr>
 <tr>
  <th>MADDbc</th>
  <td>-</td>
  <td>VMADDbc</td>
  <td>MUL and ADD broadcast bc field</td>
 </tr>
</table>


- © SCEI

- 40-




SCE CONFIDENTIAL


EE Overview Version 6.0


<table>
 <tr>
  <th colspan="2">Microinstruction</th>
  <td>Macro-instruction</td>
  <td>Function</td>
 </tr>
 <tr>
  <th>Upper</th>
  <td>Lower</td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th>MADDi</th>
  <td>-</td>
  <td>VMADDi</td>
  <td>MUL and ADD broadcast I register</td>
 </tr>
 <tr>
  <th>MADDq</th>
  <td>-</td>
  <td>VMADDq</td>
  <td>MUL and ADD broadcast Q register</td>
 </tr>
 <tr>
  <th>MAX</th>
  <td>-</td>
  <td>VMAX</td>
  <td>maximum</td>
 </tr>
 <tr>
  <th>MAXbc</th>
  <td>-</td>
  <td>VMAXbc</td>
  <td>MAX broadcast bc field</td>
 </tr>
 <tr>
  <th>MAXi</th>
  <td>-</td>
  <td>VMAXi</td>
  <td>MAX broadcast I register</td>
 </tr>
 <tr>
  <th>MINI</th>
  <td>-</td>
  <td>VMINI</td>
  <td>minimum</td>
 </tr>
 <tr>
  <th>MINIbc</th>
  <td>-</td>
  <td>VMINIbc</td>
  <td>MINI broadcast bc field</td>
 </tr>
 <tr>
  <th>MINIi</th>
  <td>-</td>
  <td>VMINIi</td>
  <td>MINI broadcast I register</td>
 </tr>
 <tr>
  <th>MSUB</th>
  <td>-</td>
  <td>VMSUB</td>
  <td>MUL and SUB</td>
 </tr>
 <tr>
  <th>MSUBA</th>
  <td>-</td>
  <td>VMSUBA</td>
  <td>MUL and SUB output to ACC</td>
 </tr>
 <tr>
  <th>MSUBAbc</th>
  <td>-</td>
  <td>VMSUBAbc</td>
  <td>MUL and SUB output to ACC broadcast bc field</td>
 </tr>
 <tr>
  <th>MSUBAi</th>
  <td>-</td>
  <td>VMSUBAi</td>
  <td>MUL and SUB output to ACC broadcast I register</td>
 </tr>
 <tr>
  <th>MSUBAq</th>
  <td>-</td>
  <td>VMSUBAq</td>
  <td>MUL and SUB output to ACC broadcast Q register</td>
 </tr>
 <tr>
  <th>MSUBbc</th>
  <td>-</td>
  <td>VMSUBbc</td>
  <td>MUL and SUB broadcast bc field</td>
 </tr>
 <tr>
  <th>MSUBi</th>
  <td>-</td>
  <td>VMSUBi</td>
  <td>MUL and SUB broadcast I register</td>
 </tr>
 <tr>
  <th>MSUBq</th>
  <td>-</td>
  <td>VMSUBq</td>
  <td>MUL and SUB broadcast Q register</td>
 </tr>
 <tr>
  <th>MUL</th>
  <td>-</td>
  <td>VMUL</td>
  <td>multiply</td>
 </tr>
 <tr>
  <th>MULA</th>
  <td>-</td>
  <td>VMULA</td>
  <td>MUL output to ACC</td>
 </tr>
 <tr>
  <th>MULAbc</th>
  <td>-</td>
  <td>VMULAbc</td>
  <td>MUL output to ACC broadcast bc field</td>
 </tr>
 <tr>
  <th>MULAi</th>
  <td>-</td>
  <td>VMULAi</td>
  <td>MUL output to ACC broadcast I register</td>
 </tr>
 <tr>
  <th>MULAq</th>
  <td>-</td>
  <td>VMULAq</td>
  <td>MUL output to ACC broadcast Q register</td>
 </tr>
 <tr>
  <th>MULbc</th>
  <td>-</td>
  <td>VMULbc</td>
  <td>MUL broadcast bc field</td>
 </tr>
 <tr>
  <th>MULi</th>
  <td>-</td>
  <td>VMULi</td>
  <td>MUL broadcast I register</td>
 </tr>
 <tr>
  <th>MULq</th>
  <td>-</td>
  <td>VMULq</td>
  <td>MUL broadcast Q register</td>
 </tr>
 <tr>
  <th>OPMSUB</th>
  <td>-</td>
  <td>VOPMSUB</td>
  <td>outer product MSUB</td>
 </tr>
 <tr>
  <th>OPMULA</th>
  <td>-</td>
  <td>VOPMULA</td>
  <td>outer product MULA</td>
 </tr>
 <tr>
  <th>-</th>
  <td>RSQRT</td>
  <td>VRSQRT</td>
  <td>floating reciprocal square-root</td>
 </tr>
 <tr>
  <th>-</th>
  <td>SQRT</td>
  <td>VSQRT</td>
  <td>floating square-root</td>
 </tr>
 <tr>
  <th>SUB</th>
  <td>-</td>
  <td>VSUB</td>
  <td>subtraction</td>
 </tr>
 <tr>
  <th>SUBA</th>
  <td>-</td>
  <td>VSUBA</td>
  <td>SUB output to ACC</td>
 </tr>
 <tr>
  <th>SUBAbc</th>
  <td>-</td>
  <td>VSUBAbc</td>
  <td>SUB output to ACC broadcast bc field</td>
 </tr>
 <tr>
  <th>SUBAi</th>
  <td>-</td>
  <td>VSUBAi</td>
  <td>SUB output to ACC broadcast I register</td>
 </tr>
 <tr>
  <th>SUBAq</th>
  <td>-</td>
  <td>VSUBAq</td>
  <td>SUB output to ACC broadcast Q register</td>
 </tr>
 <tr>
  <th>SUBbc</th>
  <td>-</td>
  <td>VSUBbc</td>
  <td>SUB broadcast bc field</td>
 </tr>
 <tr>
  <th>SUBi</th>
  <td>-</td>
  <td>VSUBi</td>
  <td>SUB broadcast I register</td>
 </tr>
 <tr>
  <th>SUBq</th>
  <td>-</td>
  <td>VSUBq</td>
  <td>SUB broadcast Q register</td>
 </tr>
</table>


- 41-

- © SCEI




SCE CONFIDENTIAL


EE Overview Version 6.0


Format Conversion
Microinstruction


<table>
 <tr>
  <th colspan="2">Microinstruction</th>
  <th>Macro-instruction</th>
  <th>Function</th>
 </tr>
 <tr>
  <td>Upper</td>
  <td>Lower</td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td>FTOI0</td>
  <td>-</td>
  <td>VFTOI0</td>
  <td>float to integer, fixed point 0 bit</td>
 </tr>
 <tr>
  <td>FTOI12</td>
  <td>-</td>
  <td>VFTOI12</td>
  <td>float to integer, fixed point 12 bits</td>
 </tr>
 <tr>
  <td>FTOI15</td>
  <td>-</td>
  <td>VFTOI15</td>
  <td>float to integer, fixed point 15 bits</td>
 </tr>
 <tr>
  <td>FTOI4</td>
  <td>-</td>
  <td>VFTOI4</td>
  <td>float to integer, fixed point 4 bits</td>
 </tr>
 <tr>
  <td>ITOF0</td>
  <td>-</td>
  <td>VITOF0</td>
  <td>integer to float, fixed point 0 bit</td>
 </tr>
 <tr>
  <td>ITOF12</td>
  <td>-</td>
  <td>VITOF12</td>
  <td>integer to float, fixed point 12 bits</td>
 </tr>
 <tr>
  <td>ITOF15</td>
  <td>-</td>
  <td>VITOF15</td>
  <td>integer to float, fixed point 15 bits</td>
 </tr>
 <tr>
  <td>ITOF4</td>
  <td>-</td>
  <td>VITOF4</td>
  <td>integer to float, fixed point 4 bits</td>
 </tr>
</table>


Integer Operation
Microinstruction


<table>
 <tr>
  <th colspan="2">Microinstruction</th>
  <td>Macro-instruction</td>
  <td>Function</td>
 </tr>
 <tr>
  <th>Upper</th>
  <td>Lower</td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th>-</th>
  <td>IADD</td>
  <td>VIADD</td>
  <td>integer ADD</td>
 </tr>
 <tr>
  <th>-</th>
  <td>IADDI</td>
  <td>VIADDI</td>
  <td>integer ADD immediate</td>
 </tr>
 <tr>
  <th>-</th>
  <td>IADDIU</td>
  <td>-</td>
  <td>integer ADD immediate unsigned</td>
 </tr>
 <tr>
  <th>-</th>
  <td>IAND</td>
  <td>VIAND</td>
  <td>integer AND</td>
 </tr>
 <tr>
  <th>-</th>
  <td>IOR</td>
  <td>VIOR</td>
  <td>integer OR</td>
 </tr>
 <tr>
  <th>-</th>
  <td>ISUB</td>
  <td>VISUB</td>
  <td>integer SUB</td>
 </tr>
 <tr>
  <th>-</th>
  <td>ISUBIU</td>
  <td>-</td>
  <td>integer SUB immediate unsigned</td>
 </tr>
</table>


Elementary Function Operation
Microinstruction


<table>
 <tr>
  <th colspan="2">Microinstruction</th>
  <td>Macro-instruction</td>
  <td>Function</td>
 </tr>
 <tr>
  <th>Upper</th>
  <td>Lower</td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th>-</th>
  <td>EATAN</td>
  <td>-</td>
  <td>Elementary-function ArcTAN</td>
 </tr>
 <tr>
  <th>-</th>
  <td>EATANxy</td>
  <td>-</td>
  <td>Elementary-function ArcTAN y/x</td>
 </tr>
 <tr>
  <th>-</th>
  <td>EATANxz</td>
  <td>-</td>
  <td>Elementary-function ArcTAN z/x</td>
 </tr>
 <tr>
  <th>-</th>
  <td>EEXP</td>
  <td>-</td>
  <td>Elementary-function Exponential</td>
 </tr>
 <tr>
  <th>-</th>
  <td>ELENG</td>
  <td>-</td>
  <td>Elementary-function Length</td>
 </tr>
 <tr>
  <th>-</th>
  <td>ERCPR</td>
  <td>-</td>
  <td>Elementary-function Reciprocal</td>
 </tr>
 <tr>
  <th>-</th>
  <td>ERLENG</td>
  <td>-</td>
  <td>Elementary-function Reciprocal Length</td>
 </tr>
 <tr>
  <th>-</th>
  <td>ERSADD</td>
  <td>-</td>
  <td>Elementary-function Reciprocal Square and ADD</td>
 </tr>
 <tr>
  <th>-</th>
  <td>ERSQRT</td>
  <td>-</td>
  <td>Elementary-function Reciprocal Square-root</td>
 </tr>
 <tr>
  <th>-</th>
  <td>ESADD</td>
  <td>-</td>
  <td>Elementary-function Square and ADD</td>
 </tr>
 <tr>
  <th>-</th>
  <td>ESIN</td>
  <td>-</td>
  <td>Elementary-function SIN</td>
 </tr>
 <tr>
  <th>-</th>
  <td>ESQRT</td>
  <td>-</td>
  <td>Elementary-function Square-root</td>
 </tr>
 <tr>
  <th>-</th>
  <td>ESUM</td>
  <td>-</td>
  <td>Elementary-function Sum</td>
 </tr>
</table>


Register-Register Transfer
Microinstruction


<table>
 <tr>
  <th colspan="2">Microinstruction</th>
  <td>Macro-instruction</td>
  <td>Function</td>
 </tr>
 <tr>
  <th>Upper</th>
  <td>Lower</td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th>-</th>
  <td>MFIR</td>
  <td>VMFIR</td>
  <td>move from integer register</td>
 </tr>
 <tr>
  <th>-</th>
  <td>MFP</td>
  <td>-</td>
  <td>move from P register</td>
 </tr>
 <tr>
  <th>-</th>
  <td>MOVE</td>
  <td>VMOVE</td>
  <td>move floating register</td>
 </tr>
 <tr>
  <th>-</th>
  <td>MR32</td>
  <td>VMR32</td>
  <td>move rotate 32 bits</td>
 </tr>
 <tr>
  <th>-</th>
  <td>MTIR</td>
  <td>VMTIR</td>
  <td>move to integer register</td>
 </tr>
</table>


- © SCEI

- 42-




SCE CONFIDENTIAL


EE Overview Version 6.0


## Load/Store


<table>
 <tr>
  <th colspan="2">Microinstruction</th>
  <td>Macro-instruction</td>
  <td>Function</td>
 </tr>
 <tr>
  <th>Upper</th>
  <td>Lower</td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th>-</th>
  <td>ILW</td>
  <td>-</td>
  <td>integer load word</td>
 </tr>
 <tr>
  <th>-</th>
  <td>ILWR</td>
  <td>VILWR</td>
  <td>integer load word register</td>
 </tr>
 <tr>
  <th>-</th>
  <td>ISW</td>
  <td>-</td>
  <td>integer store word</td>
 </tr>
 <tr>
  <th>-</th>
  <td>ISWR</td>
  <td>VISWR</td>
  <td>integer store word register</td>
 </tr>
 <tr>
  <th>-</th>
  <td>LQ</td>
  <td>-</td>
  <td>Load Quadword</td>
 </tr>
 <tr>
  <th>-</th>
  <td>LQD</td>
  <td>VLQD</td>
  <td>Load Quadword with pre-decrement</td>
 </tr>
 <tr>
  <th>-</th>
  <td>LQI</td>
  <td>VLQI</td>
  <td>Load Quadword with post-increment</td>
 </tr>
 <tr>
  <th>-</th>
  <td>SQ</td>
  <td>-</td>
  <td>Store Quadword</td>
 </tr>
 <tr>
  <th>-</th>
  <td>SQD</td>
  <td>VSQD</td>
  <td>Store Quadword with pre-decrement</td>
 </tr>
 <tr>
  <th>-</th>
  <td>SQI</td>
  <td>VSQI</td>
  <td>Store Quadword with post-increment</td>
 </tr>
</table>


Flag Operation


<table>
 <tr>
  <th colspan="2">Microinstruction</th>
  <td>Macro-instruction</td>
  <td>Function</td>
 </tr>
 <tr>
  <th>Upper</th>
  <td>Lower</td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th>-</th>
  <td>FCAND</td>
  <td>-</td>
  <td>flag-operation clipping flag AND</td>
 </tr>
 <tr>
  <th>-</th>
  <td>FCEQ</td>
  <td>-</td>
  <td>flag-operation clipping flag EQ</td>
 </tr>
 <tr>
  <th>-</th>
  <td>FCGET</td>
  <td>-</td>
  <td>flag-operation clipping flag get</td>
 </tr>
 <tr>
  <th>-</th>
  <td>FCOR</td>
  <td>-</td>
  <td>flag-operation clipping flag OR</td>
 </tr>
 <tr>
  <th>-</th>
  <td>FCSET</td>
  <td>-</td>
  <td>flag-operation clipping flag set</td>
 </tr>
 <tr>
  <th>-</th>
  <td>FMAND</td>
  <td>-</td>
  <td>flag-operation MAC flag AND</td>
 </tr>
 <tr>
  <th>-</th>
  <td>FMEQ</td>
  <td>-</td>
  <td>flag-operation MAC flag EQ</td>
 </tr>
 <tr>
  <th>-</th>
  <td>FMOR</td>
  <td>-</td>
  <td>flag-operation MAC flag OR</td>
 </tr>
 <tr>
  <th>-</th>
  <td>FSAND</td>
  <td>-</td>
  <td>flag-operation status flag AND</td>
 </tr>
 <tr>
  <th>-</th>
  <td>FSEQ</td>
  <td>-</td>
  <td>flag-operation status flag EQ</td>
 </tr>
 <tr>
  <th>-</th>
  <td>FSOR</td>
  <td>-</td>
  <td>flag-operation status flag OR</td>
 </tr>
 <tr>
  <th>-</th>
  <td>FSSET</td>
  <td>-</td>
  <td>flag-operation set status flag</td>
 </tr>
</table>


Branching


<table>
 <tr>
  <th colspan="2">Microinstruction</th>
  <td>Macro-instruction</td>
  <td>Function</td>
 </tr>
 <tr>
  <th>Upper</th>
  <td>Lower</td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th>-</th>
  <td>B</td>
  <td>-</td>
  <td>branch (PC relative address)</td>
 </tr>
 <tr>
  <th>-</th>
  <td>BAL</td>
  <td>-</td>
  <td>branch and link (PC relative address)</td>
 </tr>
 <tr>
  <th>-</th>
  <td>IBEQ</td>
  <td>-</td>
  <td>integer branch on equal</td>
 </tr>
 <tr>
  <th>-</th>
  <td>IBGEZ</td>
  <td>-</td>
  <td>integer branch on greater than or equal to zero</td>
 </tr>
 <tr>
  <th>-</th>
  <td>IBGTZ</td>
  <td>-</td>
  <td>integer branch on greater than 0</td>
 </tr>
 <tr>
  <th>-</th>
  <td>IBLEZ</td>
  <td>-</td>
  <td>integer branch on less than or equal to zero</td>
 </tr>
 <tr>
  <th>-</th>
  <td>IBLTZ</td>
  <td>-</td>
  <td>integer branch on less than zero</td>
 </tr>
 <tr>
  <th>-</th>
  <td>IBNE</td>
  <td>-</td>
  <td>integer branch on not equal</td>
 </tr>
 <tr>
  <th>-</th>
  <td>JALR</td>
  <td>-</td>
  <td>jump and link register (absolute address)</td>
 </tr>
 <tr>
  <th>-</th>
  <td>JR</td>
  <td>-</td>
  <td>jump register (absolute address)</td>
 </tr>
</table>


- 43-

- © SCEI




SCE CONFIDENTIAL EE Overview Version 6.0


Random Numbers
Microinstruction


<table>
 <tr>
  <th colspan="2">Microinstruction</th>
  <td>Macro-instruction</td>
  <td>Function</td>
 </tr>
 <tr>
  <th>Upper</th>
  <td>Lower</td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th>-</th>
  <td>RGET</td>
  <td>VRGET</td>
  <td>random-unit get R register</td>
 </tr>
 <tr>
  <th>-</th>
  <td>RINIT</td>
  <td>VRINIT</td>
  <td>random-unit init R register</td>
 </tr>
 <tr>
  <th>-</th>
  <td>RNEXT</td>
  <td>VRNEXT</td>
  <td>random-unit next M sequence</td>
 </tr>
 <tr>
  <th>-</th>
  <td>RXOR</td>
  <td>VRXOR</td>
  <td>random-unit XOR register</td>
 </tr>
</table>


Others
Microinstruction


<table>
 <tr>
  <th colspan="2">Microinstruction</th>
  <td>Macro-instruction</td>
  <td>Function</td>
 </tr>
 <tr>
  <th>Upper</th>
  <td>Lower</td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th>CLIP</th>
  <td>-</td>
  <td>VCLIP</td>
  <td>clipping</td>
 </tr>
 <tr>
  <th>NOP</th>
  <td>-</td>
  <td>VNOP</td>
  <td>no operation</td>
 </tr>
 <tr>
  <th>-</th>
  <td>WAITP</td>
  <td>-</td>
  <td>wait P register</td>
 </tr>
 <tr>
  <th>-</th>
  <td>WAITQ</td>
  <td>VWAITQ</td>
  <td>wait Q register</td>
 </tr>
 <tr>
  <th>-</th>
  <td>XGKICK</td>
  <td>-</td>
  <td>eXternal-unit GPU2 Interface Kick</td>
 </tr>
 <tr>
  <th>-</th>
  <td>XITOP</td>
  <td>-</td>
  <td>eXternal-unit read ITOP register</td>
 </tr>
 <tr>
  <th>-</th>
  <td>XTOP</td>
  <td>-</td>
  <td>eXternal-unit read TOP register</td>
 </tr>
</table>


- © SCEI

- 44-




SCE CONFIDENTIAL EE Overview Version 6.0


## 2.4. IPU: Image Data Processor


The IPU implements decompression of two-dimensional images, such as texture data and video data. The IPU
decompresses the data, using MPEG2 or a subset of MPEG2, or converts the data, using VQ (Vector
Quantization). Which layer to use depends on the purpose and the property of the image.


IPU


Macro-Block Decoder
Zig-zag IDCT
Scan


VLD


IQ CSC VQ


Local Buffer
Memory


FIFO


128


Figure 2-10 IPU Block Diagram


In decoding MPEG2 bit streams, the IPU decodes macro blocks and the EE Core performs motion
compensation via software by using multimedia instructions. For CSC (Color Space Conversion), the IPU is in
charge.


EE Core
128 bit ALU


Rendering
Engine


IPU
(IDCT)


IPU
(CSC)


SPR SPR
Buffer#0 Buffer#1


RGB
Decoded
Image


Bit Reference Decoded
Stream Image Image


Main Memory


Figure 2-11 Decoding Process Flow for Motion Compensation


- 45-

- © SCEI




SCE CONFIDENTIAL EE Overview Version 6.0


## 2.5. GIF: GS Interface


As a front end to the GS, the GIF formats data based on the specifications of a tag (GIFtag) at the start of the
display list packet, and then transfers the formatted data to the GS as a drawing command. Data is input to the
GIF from VU Mem1 via PATH1, from VIF1 via PATH2, and from main memory via PATH3. The GIF also
plays a role in data path arbitration.
PATH1 is assigned to the transfer of display lists processed in VPU1. PATH2 is assigned to the data directly
transferable to the rendering engine, e.g. online textures. PATH3 is assigned to the transfer of display lists
which have been generated by the EE Core and VPU0 and stored temporarily in main memory. The order of
priority is PATH1, PATH2, and PATH3.


G S


Buffer
64
Packing


<table>
 <tr>
  <th>Logic</th>
  <th>GS</th>
  <th>Regs</th>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
 </tr>
</table>


GIF


128 PATH1


VU1 VU Mem1


PATH3
PATH2


128


128


VIF1 GIF
FIFO


Figure 2-12 Data Paths to GS


- © SCEI

- 46-




SCE CONFIDENTIAL EE Overview Version 6.0


## 2.6. SIF: Sub-CPU Interface


The Sub-CPU (IOP) controls sound output and I/O to and from storage devices. It adopts an LMA
configuration with memory independent of the EE. The SIF is the interface to exchange data between these
processors. The DMA controllers (DMACs) for the IOP and EE operate in cooperation through the
bidirectional FIFO (SFIFO) in the SIF.


<table>
 <tr>
  <th>IOP IOP</th>
  <th>SIF</th>
  <th>EE</th>
  <th>EE</th>
 </tr>
 <tr>
  <td>Core DMAC</td>
  <td>Control Regs .</td>
  <td>Core</td>
  <td>DMAC</td>
 </tr>
 <tr>
  <td>32</td>
  <td></td>
  <td>128</td>
  <td></td>
 </tr>
 <tr>
  <td></td>
  <td>unpack</td>
  <td></td>
  <td></td>
 </tr>
</table>


IOP-Memory


Main Memory
(MEM)


Figure 2-13 EE-IOP Interface


Data is transmitted in units called packets. A tag (DMATag) is attached to each packet, containing a memory
address in the IOP memory space, a memory address in the EE memory space, and the data size. The IOP-
DMAC reads the IOP memory address and data size from the tag, and transmits the packet with its tag to the
SIF. The EE-DMAC reads the packet from the SIF, interprets the first word as a tag, reads the EE memory
address and data size from the tag, and decompresses the data to the specified memory address. These transfer
operations are performed by the DMACs to avoid generating unnecessary interrupts of the CPU.


IOP Memory


128 bit


Packet A


Packet B


Packet C


128 bit


<table>
 <tr>
  <th>Tag</th>
 </tr>
 <tr>
  <th>Tag</th>
 </tr>
 <tr>
  <th>Tag</th>
 </tr>
</table>


Source
Chain
DMA


SIF


Destination
Chain


DMA


EE Memory


128 bit


Packet A


Packet B


Packet C


Figure 2-14 SIF Data Flow


- 47-

- © SCEI




SCE CONFIDENTIAL EE Overview Version 6.0


(This page is left blank intentionally)


- © SCEI

- 48-




SCE CONFIDENTIAL EE Overview Version 6.0


## 3. Functional Overview


- 49-

- © SCEI




SCE CONFIDENTIAL EE Overview Version 6.0


## 3.1. Data Transfer via DMA


Data is transferred between main memory, peripheral processors, and scratchpad memory (SPR) via DMA.
The unit of data transfer is a quadword (128 bits = qword). In data transfer to and from peripheral processors,
data is divided into blocks (slices) of 8 qwords.
On some of the channels, Chain mode is available. This mode performs processing such as switching transfer
addresses according to the tag (DMAtag) in the transfer data. This not only reduces processing such as data
sorting before transfer, but also enables data exchange between peripheral processors through the mediation of
main memory without the EE Core. At such times, the stall control function, which mutually synchronizes
transfer, is available. For the GIF channel, memory FIFO function to use the ring buffer in main memory is
also provided.


## 3.1.1. Sliced Transfer


Except for the data transfer between the SPR and main memory, DMA transfer is performed by slicing the data
every 8 qwords and arbitrating the transfer requests from each channel. A channel releases the bus right
temporarily whenever transfer of one slice is completed, and it continues transferring if there are no requests
from others. This sliced-transfer mechanism not only enables two or more transfer processes to be executed in
parallel but also allows the EE Core to access main memory during the transfer process. The following figure
illustrates DMA transfers performed concurrently on Channel A and B.


ch-A DREQ


ch-A DREQ


ch-A DREQ


8 qword


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
  <th>CPU ch-A ch-B</th>
  <td>ch-B ch-A CPU ch-A</td>
  <td>ch-B</td>
 </tr>
 <tr>
  <th></th>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th>Stall</th>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th></th>
  <td></td>
  <td></td>
 </tr>
</table>


Stall


8 qword


8 qword


Figure 3-1 Example of Sliced Transfer


## 3.1.2. Chain Mode Transfer


## Source Chain Mode


Source Chain Mode is used for DMA transfer from memory to peripherals. In this mode, transfer address
and transfer data size are specified according to the tag data (DMAtag) in the packet. The DMAC repeats
transfer processing while tracing the tags in memory, and ends a series of transfers at the point where
transfer of the tag with the end instruction finishes.
The DMAtag is 128-bit data with the following structure. ID is a field in which details of the transfer
operation are specified. Eight types in the table below can be specified.


- © SCEI

- 50-




SCE CONFIDENTIAL EE Overview Version 6.0


127


<table>
 <tr>
  <th colspan="4">(Arbitrary)</th>
 </tr>
 <tr>
  <th>63</th>
  <td></td>
  <td>32 31</td>
  <td>24 15 0</td>
 </tr>
 <tr>
  <th colspan="4">Address Specification ADDR ID/FLG Data Size QWC</th>
 </tr>
 <tr>
  <th>ID</th>
  <td>Transfer Data Position</td>
  <td>Next Tag Position</td>
  <td>Operation</td>
 </tr>
 <tr>
  <th>cnt</th>
  <td>Next to tag</td>
  <td>Next to transfer data</td>
  <td>Transfers the data following the tag and<br/>to the data.</td>
 </tr>
 <tr>
  <th>next</th>
  <td>Next to tag</td>
  <td>Specified in tag</td>
  <td>Transfers the data following the tag and<br/>to the</td>
 </tr>
 <tr>
  <th>ref</th>
  <td>Specified in tag</td>
  <td>Next to tag</td>
  <td>Transfers the data at the specified</td>
 </tr>
 <tr>
  <th>refs</th>
  <td>Specified in tag</td>
  <td>Next to tag</td>
  <td>Transfers the data at the specified<br/>position while applying stall control.</td>
 </tr>
 <tr>
  <th>refe</th>
  <td>Specified in tag</td>
  <td>(None)</td>
  <td>Transfers the data at the specified<br/>and ends transfer.</td>
 </tr>
 <tr>
  <th>call</th>
  <td>Next to tag</td>
  <td>Specified in tag</td>
  <td>Transfers the data following the tag,<br/>stores the next address, and jumps to the</td>
 </tr>
 <tr>
  <th>ret</th>
  <td>Next to tag</td>
  <td>Position stored<br/>when call was</td>
  <td>Transfers the data following the tag and<br/>jumps to the position stored when call<br/>was</td>
 </tr>
 <tr>
  <th>end</th>
  <td>Next to tag</td>
  <td>(None)</td>
  <td>Transfers the data following the tag and<br/>ends transfer.</td>
 </tr>
</table>


64


proceeds
succeeding
jumps
specified position.
position.
position
specified position.
specified.
these IDs appropriately according to the data


Data transfers can be performed most efficiently by using
structures in memory. The following is an example.


- 51-

- © SCEI




SCE CONFIDENTIAL EE Overview Version 6.0


TADR


TADR


<table>
 <tr>
  <th></th>
 </tr>
 <tr>
  <th>(3)REF ADDR=ADDR1</th>
 </tr>
 <tr>
  <th>(6) DATA 0</th>
 </tr>
 <tr>
  <th>(4) DATA 1</th>
 </tr>
 <tr>
  <th>(2) DATA 2</th>
 </tr>
</table>


(1)NEXT ADDR=ADDR2


(2)
DATA 0


ADDR0


1


1. END ADDR=-

2.  DATA 1


ADDR1


3


2


(3)NEXT ADDR=ADDR1
(4) DATA 2


ADDR2


ADDR0


1. 


ADDR1


1. 


ADDR2


1


TADR


next tag


## ADDR0 (2)CALL ADDR=ADDR2 ADDR1


(1)CALL ADDR=ADDR1
(7)CALL ADDR=ADDR2


(3)
DATA 1 1


1. DATA 0


3


(6) RET ADDR=-


## (11)END ADDR=-


(4)(9) RET ADDR=-


ADDR2


(5)(10)


2


4


DATA 2


ref tag


call tag/ret tag


Figure 3-2 Source Chain DMA Tags Showing Data Structures


- © SCEI

- 52-




SCE CONFIDENTIAL EE Overview Version 6.0


## Destination Chain Mode


Destination Chain Mode is used to transfer data from peripherals to memory. The tag (DMAtag) bearing the
destination address and packet length is placed at the start of the transfer packet. This enables the peripheral
side to control the address where data is stored.
The Destination Chain tag is 128-bit data with the following structure, and is classified into three types as
shown in the table below.
127 64


<table>
 <tr>
  <th>(Arbitrary)</th>
 </tr>
 <tr>
  <td>63 32 31 24 15 0</td>
 </tr>
 <tr>
  <td>Address Specification ADDR ID/FLG Data Size QWC</td>
 </tr>
 <tr>
  <td>ID Destination Address</td>
 </tr>
 <tr>
  <td>cnt in Stores the data the at the address.</td>
 </tr>
 <tr>
  <td>cnts Specified in tag Stores the data following the tag at the specified address while<br/>stall control.</td>
 </tr>
 <tr>
  <td>end Specified in tag Stores the data following the tag at the specified address and ends<br/>transfer.</td>
 </tr>
</table>


applying
The following is an example.


<table>
 <tr>
  <th colspan="2">SPR SADDR</th>
 </tr>
 <tr>
  <td>CNT ADDR=ADDR1,QWC=8</td>
  <td rowspan="4"></td>
 </tr>
 <tr>
  <td>DATA 0</td>
 </tr>
 <tr>
  <td>CNT ADDR=ADDR0,QWC=8</td>
 </tr>
 <tr>
  <td>DATA 1</td>
 </tr>
 <tr>
  <td>IOP Memory</td>
  <td></td>
 </tr>
 <tr>
  <td>CNT ADDR=ADDR1,QWC=8</td>
  <td rowspan="4">SDMA</td>
 </tr>
 <tr>
  <td>DATA 0</td>
 </tr>
 <tr>
  <td>CNT ADDR=ADDR0,QWC=8</td>
 </tr>
 <tr>
  <td>DATA 1</td>
 </tr>
</table>


Start Address Register


D.Chain
DMA


ADDR0


ADDR1


Destination Chain DMA from SPR


Main Memory


DATA 1


DATA 0


D.Chain
DMA


SFIFO


ADDR0


ADDR1


Destination Chain DMA from SIF


Main Memory


DATA 1


DATA 0


Figure 3-3 Destination Chain DMA to Transfer Data to Specified Address


- 53-

- © SCEI




SCE CONFIDENTIAL EE Overview Version 6.0


## 3.1.3. Interleave Transfer


Interleave mode is available for DMA transfer between main memory and SPR. This mode processes data in
such a way that a small rectangular area is cut out from or fitted into the two-dimensional data (image data)
allocated in memory.
Figure 3-4 illustrates an example of cutting out a small rectangular area (TW, TH) from a rectangular area (FW,
FH).
Fw


Fh


Th


Source
Image Tw


Destination


T0
T1
T2
:
:
SPR


<table>
 <tr>
  <th></th>
  <th>Tw</th>
  <th>T0</th>
 </tr>
 <tr>
  <td>Fw</td>
  <td></td>
  <td>skip</td>
 </tr>
 <tr>
  <td></td>
  <td>Tw</td>
  <td>T1</td>
 </tr>
 <tr>
  <td>Fw</td>
  <td></td>
  <td>skip</td>
 </tr>
 <tr>
  <td></td>
  <td>Tw</td>
  <td>T2</td>
 </tr>
 <tr>
  <td>Fw</td>
  <td></td>
  <td>skip</td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td>:</td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td>:</td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td>Main</td>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td>memory</td>
 </tr>
</table>


Figure 3-4 Cutting Out a Small Rectangular Area in Interleave Mode


## 3.1.4. Stall Control


When a transfer from a peripheral to memory and a transfer from memory to another peripheral are performed
concurrently, they can be synchronized through the stall address register (D_STADR). The channel that handles
the DMA transfer to memory is called the source channel. The channel that handles the DMA transfer from
memory is called the drain channel. The value of D_STADR is updated as transfer processing on the source
channel side advances, but transfer processing on the drain channel side stalls at the address immediately
preceding the D_STADR address. This mechanism is called stall control.


- © SCEI

- 54-




SCE CONFIDENTIAL EE Overview Version 6.0


Main Memory/SPR


Source
DMA


<table>
 <tr>
  <th>Peripheral0</th>
  <th>8 qwords<br/>8 qwords</th>
  <th>Peripheral1</th>
 </tr>
 <tr>
  <td></td>
  <td>8 qwords<br/>8 qwords</td>
  <td></td>
 </tr>
</table>


Drain
DMA


D_STADR


Figure 3-5 Synchronization between DMA Transfers by Stall Control


## 3.1.5. MFIFO


A FIFO function can be implemented by using a ring buffer and the DMA tag set in main memory when
transferring data from the scratchpad memory to the VIF1/GIF. This is called MFIFO (MemoryFIFO).


EE Core


SPR


## VIF1/GIF


Main BUS


D_RBOR


Dd_TADR


D_RBSR


D8_MADR


Main Memory


Ring Buffer (MFIF0)


Figure 3-6 Memory FIFO (MFIFO)


- 55-

- © SCEI




SCE CONFIDENTIAL EE Overview Version 6.0


## 3.2. Data Transfer to VPU


The EE has two built-in VPUs. These floating-point vector processors execute matrix operations, coordinate
conversion, transparency perspective conversion, and so forth, at high speed. Data is DMA-transferred to the
VPU through the VIF. The header information (VIFcode) embedded in the transfer data specifies how to
process the data in the VPU. This is the mechanism of DMA transfer to the VPU.


## 3.2.1. VIF Overview


The VIF is an interface unit, which decompresses the DMA-transferred data in packets and transfers it to the
VPU memory. The VIF is designed to set the decompression method and destination memory address of the
data according to the VIFcode included in the VIF packet. It enables the VPU to perform operations
independently of the EE Core by transferring VIF packets of vector data, VIF packets of microinstruction
program, and VIF packets to give an instruction to activate a microinstruction program.
The data types the VIF can decompress and transfer to the VU Mem are one- to four-dimensional vectors
consisting of 8-bit/16-bit/32-bit elements, and a four-dimensional vector of 16-bit color type with RGBa:


5.5.5.1. In addition, the VIF can transfer microinstruction code to be transferred to the Micro Mem. VIF1 can


also transfer data to the GS via the GIF.


## 3.2.2. VIF Packet


According to the 32-bit VIFcode in the transferred data, the VIF decompresses the following data and writes
memory and registers in the VU. The VIFcode and the following data string are called the VIF packet. Several
VIF packets can exist in 1 DMA packet as shown in the figure below.


## DMA packet example (When DMAtag is transferred)


←MSB 128 bits LSB→
DMAtag


<table>
 <tr>
  <th>data</th>
  <td>VIFcode0</td>
  <td colspan="2"></td>
 </tr>
 <tr>
  <th>data</th>
  <td>data</td>
  <td>VIFcode2</td>
  <td>VIFcode1</td>
 </tr>
 <tr>
  <th>data</th>
  <td>data</td>
  <td>data</td>
  <td>data</td>
 </tr>
 <tr>
  <th>data</th>
  <td>data</td>
  <td>data</td>
  <td>data</td>
 </tr>
 <tr>
  <th>data</th>
  <td>data</td>
  <td>data</td>
  <td>data</td>
 </tr>
 <tr>
  <th>data</th>
  <td>data</td>
  <td>data</td>
  <td>VIFcode3</td>
 </tr>
 <tr>
  <th>data</th>
  <td>data</td>
  <td>VIFcode4</td>
  <td>data</td>
 </tr>
 <tr>
  <th>data</th>
  <td>data</td>
  <td>data</td>
  <td>data</td>
 </tr>
 <tr>
  <th>VIFcode5</th>
  <td>data</td>
  <td>data</td>
  <td>data</td>
 </tr>
</table>


## DMA packet example (When DMAtag is not transferred)


←MSB 128 bits LSB→


<table>
 <tr>
  <th>--</th>
  <td>--</td>
  <td colspan="2">DMAtag</td>
 </tr>
 <tr>
  <th>VIFcode2</th>
  <td>VIFcode1</td>
  <td>data</td>
  <td>VIFcode0</td>
 </tr>
 <tr>
  <th>data</th>
  <td>data</td>
  <td>data</td>
  <td>data</td>
 </tr>
 <tr>
  <th>data</th>
  <td>data</td>
  <td>data</td>
  <td>data</td>
 </tr>
 <tr>
  <th>data</th>
  <td>data</td>
  <td>data</td>
  <td>data</td>
 </tr>
 <tr>
  <th>data</th>
  <td>VIFcode3</td>
  <td>data</td>
  <td>data</td>
 </tr>
 <tr>
  <th>VIFcode4</th>
  <td>data</td>
  <td>data</td>
  <td>data</td>
 </tr>
 <tr>
  <th>data</th>
  <td>data</td>
  <td>data</td>
  <td>data</td>
 </tr>
 <tr>
  <th>data</th>
  <td>data</td>
  <td>data</td>
  <td>data</td>
 </tr>
 <tr>
  <th>--</th>
  <td>--</td>
  <td>VIFcode5</td>
  <td>data</td>
 </tr>
</table>


- © SCEI

- 56-




SCE CONFIDENTIAL EE Overview Version 6.0


VIF packets included in the above DMA packets


data VIFcode0
VIFcode5


<table>
 <tr>
  <th>VIFcode1</th>
  <td colspan="3"></td>
 </tr>
 <tr>
  <th>data</th>
  <td>data</td>
  <td>......data x 12</td>
  <td>VIFcode2</td>
 </tr>
 <tr>
  <th>data</th>
  <td>data</td>
  <td>......data x 2</td>
  <td>VIFcode3</td>
 </tr>
 <tr>
  <th>data</th>
  <td>data</td>
  <td>......data x 3</td>
  <td>VIFcode4</td>
 </tr>
</table>


## 3.2.3. VIFcode Structure


The VIFcode is 32 bits in length, consisting of the CMD field (8 bits), the
IMMEDIATE field (16 bits) as shown in the figure below.
31 24 23 16 15


NUM field (8 bits), and the


CMD


NUM IMMEDIATE


0


The CMD field gives the VIF instructions on the operation and the decompression method of the following
data. The meanings of the NUM and IMMEDIATE fields change according to the value of the CMD field.


<table>
 <tr>
  <th>Category</th>
  <th>CMD Name</th>
  <th>Function</th>
  <th>Following data</th>
 </tr>
 <tr>
  <td rowspan="7">Data transfer</td>
  <td>UNPACK</td>
  <td>Decompresses data and writes to VU Mem.</td>
  <td>Packed vector data</td>
 </tr>
 <tr>
  <td>STCYCL</td>
  <td>Sets CYCLE value.</td>
  <td>None</td>
 </tr>
 <tr>
  <td>OFFSET</td>
  <td>Sets OFFSET value (VIF1 only).</td>
  <td>None</td>
 </tr>
 <tr>
  <td>STMOD</td>
  <td>Sets MODE value.</td>
  <td>None</td>
 </tr>
 <tr>
  <td>STMASK</td>
  <td>Sets MASK value.</td>
  <td>Mask pattern</td>
 </tr>
 <tr>
  <td>STROW</td>
  <td>Sets Row register value.</td>
  <td>Row-completion<br/>data</td>
 </tr>
 <tr>
  <td>STCOL</td>
  <td>Sets Col register value.</td>
  <td>Column-completion<br/>data</td>
 </tr>
 <tr>
  <td rowspan="7">Micro-program<br/>execution</td>
  <td>MPG</td>
  <td>Loads a microprogram.</td>
  <td>Microinstruction<br/>program</td>
 </tr>
 <tr>
  <td>FLUSHE</td>
  <td>Waits for end of a microprogram.</td>
  <td>None</td>
 </tr>
 <tr>
  <td>FLUSH</td>
  <td>Waits for end of a microprogram and end of<br/>GIF (PATH1 /PATH2) transfer. (VIF1 only)</td>
  <td>None</td>
 </tr>
 <tr>
  <td>FLUSHA</td>
  <td>Waits for end of a microprogram and end of<br/>GIF transfer. (VIF1 only)</td>
  <td>None</td>
 </tr>
 <tr>
  <td>MSCAL</td>
  <td>Activates a</td>
  <td>None</td>
 </tr>
 <tr>
  <td>MSCNT</td>
  <td>Executes a</td>
  <td>None</td>
 </tr>
 <tr>
  <td>MSCALF</td>
  <td>FLUSH and activates a microprogram.</td>
  <td>None</td>
 </tr>
 <tr>
  <td rowspan="2">Double<br/>buffering</td>
  <td>BASE</td>
  <td>Sets BASE value. (VIF1 only)</td>
  <td>None</td>
 </tr>
 <tr>
  <td>ITOP</td>
  <td>Sets ITOPS value.</td>
  <td>None</td>
 </tr>
 <tr>
  <td>GS data</td>
  <td>DIRECT</td>
  <td>Transfers data to GIF</td>
  <td>GS data</td>
 </tr>
 <tr>
  <td>transfer</td>
  <td>DIRECTHL</td>
  <td>Transfers data to GIF</td>
  <td>GS data</td>
 </tr>
 <tr>
  <td>(VIF1 only)</td>
  <td>MSKPATH3</td>
  <td>Masks transfer via PATH3 to GIF.</td>
  <td>None</td>
 </tr>
 <tr>
  <td rowspan="2">Others</td>
  <td>NOP</td>
  <td>No</td>
  <td>None</td>
 </tr>
 <tr>
  <td>MARK</td>
  <td>Sets MARK value.</td>
  <td>None</td>
 </tr>
</table>


- 57-

- © SCEI




SCE CONFIDENTIAL EE Overview Version 6.0


## 3.2.4. Data Transfer by UNPACK


The most general data transfer via the VIF is data transfer to VU Mem by using the VIFcode UNPACK.
transfer data following the VIFcode is packed data; 8 bits x 4 elements and 32 bits x 3 elements, for example.
The VIF decompresses the packed data to vector data of 32 bits x 4 elements and writes it to the VU Mem.
this time, VU Mem area left blank can be filled with a VPU register value (supplementation), and a constant
offset value can be added to the transfer data (addition).
The list of packing formats is shown as follows.


<table>
 <tr>
  <th>Format</th>
  <th>Data</th>
  <th>No. of elements</th>
 </tr>
 <tr>
  <td></td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td>S-32</td>
  <td>32 bits</td>
  <td>1</td>
 </tr>
 <tr>
  <td>S-16</td>
  <td>16 bits</td>
  <td>1</td>
 </tr>
 <tr>
  <td>S-8</td>
  <td>8 bits</td>
  <td>1</td>
 </tr>
 <tr>
  <td>V2-32</td>
  <td>32 bits</td>
  <td>2</td>
 </tr>
 <tr>
  <td>V2-16</td>
  <td>16 bits</td>
  <td>2</td>
 </tr>
 <tr>
  <td>V2-8</td>
  <td>8 bits</td>
  <td>2</td>
 </tr>
 <tr>
  <td>V3-32</td>
  <td>32 bits</td>
  <td>3</td>
 </tr>
 <tr>
  <td>V3-16</td>
  <td>16 bits</td>
  <td>3</td>
 </tr>
 <tr>
  <td>V3-8</td>
  <td>8 bits</td>
  <td>3</td>
 </tr>
 <tr>
  <td>V4-32</td>
  <td>32 bits</td>
  <td>4</td>
 </tr>
 <tr>
  <td>V4-16</td>
  <td>16 bits</td>
  <td>4</td>
 </tr>
 <tr>
  <td>V4-8</td>
  <td>8 bits</td>
  <td>4</td>
 </tr>
 <tr>
  <td>V4-5</td>
  <td>5+5+5+1 bits</td>
  <td>4</td>
 </tr>
</table>


The


At


- © SCEI

- 58-




SCE CONFIDENTIAL EE Overview Version 6.0


## 3.2.5. Double Buffering


VPU1 supports double buffering, which sets two buffer areas in the VU Mem and enhances throughputs by
simultaneously transferring data to VU Mem and performing microprogram operations.


TOPS


Following data transfer


Buffer A


Buffer B


Transferred data processing


Buffer A


TOP


Transferred data processing


Buffer B


Following data transfer


Figure 3-7 Double Buffering in VU Mem


Double buffer addresses can be set with the VIF1_BASE and VIF1_OFST registers. These can be reflected in
the VIF1_TOPS register and the TOP register of VU1 by taking appropriate steps.
By setting the FLG bit in the VIFcode UNPACK, data can be transferred to the double buffers according to the
relative specification based on the address shown by the TOPS register. When a microprogram reads data from
double buffers, it reads the TOP register value using the XTOP instruction and accesses the data in the buffer
accordingly.
The values of TOPS and TOP are replaced whenever a microprogram is activated. So it is possible to process
transferred data with a microprogram while transferring data to two buffers alternately, by repeating data transfer
and microprogram activation.


- 59-

- © SCEI




SCE CONFIDENTIAL EE Overview Version 6.0


general
paths
They


• PATH1 PATH1 is a data transfer path from VPU1 data memory (VU Mem1) to the GS. When VU1
executes the XGKICK instruction, transfer processing via this path is performed.
• PATH2 PATH2 is a data transfer path between the FIFO inside the VPU1 VIF and the GIF. This path
is used when executing the DIRECT/DIRECT_HL instruction in the VIF and when
transferring data from the GS to main memory by using the image data transfer function of the
GS.
• PATH3 PATH3 is a direct data transfer path from the EE main bus to the GIF. This path is used when
transferring data from main memory or the SPR to the GS.


## Priority and Timing


The three general data transfer paths are prioritized as PATH1>PATH2>PATH3. Whenever transfer of the
GS packet (described later in this document) ends in each path, transfer requests from other paths are
checked. If there is a request, transfer processing is performed according to priority.


## Access to GS Privileged Register


The privileged registers of the GS are directly mapped to the I/O space of the EE core, and are accessible
without using the GIF, regardless of the state of the general data transfer paths. The GIF monitors access to
the privileged registers. When the transfer direction switching register (BUSDIR) is accessed, the GIF
switches data transfer direction accordingly.


## 3.3.2. Data Format


## GS Packet


The basic unit of data transferred by the GIF is a GS primitive consisting of header information (GIFtag)
and following data. However, transfer processing is performed in units of GS packets in which several GS
primitives are gathered. The last GS primitive in the GS packet is shown by the termination information
(EOP=1) in the GIFtag.


- © SCEI

- 60-




SCE CONFIDENTIAL EE Overview Version 6.0


GIFtag(EOP=0)


DATA


GS Primitive


GIFtag(EOP=0)


DATA


GIFtag(EOP=1)


DATA


GS Primitive


GS Primitive


GS Packet


Figure 3-8 GS Packet Structure


The above data structure is common to any data transfer path. For PATH2 and PATH3, however, the
VIFcode and DMATag are put in front of the GS packet.
It is necessary to align the GIFtag and data on a 128-bit boundary in memory.


## GIFtag


The GIFtag has a 128-bit fixed length, and specifies the size and structure of the following data and the data
format (mode). The structure of the GIFtag is as follows:
1
2 6 6 6 5 5 5 4 4 1 1 0
7 4 3 0 9 8 7 7 6 5 4 0
(max


<table>
 <tr>
  <th colspan="3">N F P P E<br/>REGS (max 16) G R E L G M R I R E P O NLOOP</th>
 </tr>
 <tr>
  <th>Name</th>
  <td>Pos.</td>
  <td>Contents</td>
 </tr>
 <tr>
  <th>NLOOP</th>
  <td>14:0</td>
  <td>Repeat count (GS primitive data size)</td>
 </tr>
 <tr>
  <th>EOP</th>
  <td>15</td>
  <td>Termination information (End of Packet)</td>
 </tr>
 <tr>
  <th>PRE</th>
  <td>46</td>
  <td>PRIM field enabled</td>
 </tr>
 <tr>
  <th>PRIM</th>
  <td>57:47</td>
  <td>Data to be set to the PRIM register of GS</td>
 </tr>
 <tr>
  <th>FLG</th>
  <td>59:58</td>
  <td>Data format<br/>00 PACKED mode<br/>01 REGLIST mode<br/>10 IMAGE mode<br/>11 Disabled (Same operation as the IMAGE mode)</td>
 </tr>
 <tr>
  <th>NREG</th>
  <td>63:60</td>
  <td>Number of register descriptors (Number of register descriptors in REGS<br/>field)</td>
 </tr>
 <tr>
  <th>REGS</th>
  <td>127:64</td>
  <td>(4 bits x 16 max.)</td>
 </tr>
</table>


Register descriptor
The value of the NLOOP field shows the data size of GS primitive, but the unit varies depending
data format.


on the


- 61-

- © SCEI




SCE CONFIDENTIAL EE Overview Version 6.0


## 3.3.3. PACKED Mode


PACKED mode formats (packs) vertex coordinate values, texture coordinate values, and color values generated
as vector data of 32 bits x 4 elements adjusting to the corresponding bit fields of the GS registers, and writes
them to the GS registers. The register descriptors put in the REGS field of the GIFtag correspond to every
qword in the following data, and show the data format and the register where the data is written. The following
9 types of register descriptors are available:


<table>
 <tr>
  <th>Name</th>
  <th>Data</th>
  <th>Destination</th>
 </tr>
 <tr>
  <td>PRIM</td>
  <td>and attribute of</td>
  <td>PRIM</td>
 </tr>
 <tr>
  <td>RGBAQ</td>
  <td>Vertex color</td>
  <td>RGBAQ</td>
 </tr>
 <tr>
  <td>ST</td>
  <td>Vertex texture coordinates</td>
  <td>ST</td>
 </tr>
 <tr>
  <td>UV</td>
  <td>Vertex texture coordinates coordinate</td>
  <td>UV</td>
 </tr>
 <tr>
  <td>XYZF2</td>
  <td>Vertex coordinate values + coefficient</td>
  <td>XYZF2/XYZF3</td>
 </tr>
 <tr>
  <td>XYZ2</td>
  <td>Vertex coordinate values</td>
  <td>XYZ2/XYZ3</td>
 </tr>
 <tr>
  <td>FOG</td>
  <td>coefficient</td>
  <td>FOG</td>
 </tr>
 <tr>
  <td>A+D</td>
  <td>set value</td>
  <td></td>
 </tr>
 <tr>
  <td>NOP</td>
  <td></td>
  <td>None</td>
 </tr>
</table>


## 3.3.4. REGLIST Mode


REGLIST mode transfers data strings formatted in such a way that they can be written to the GS register as they
are. The data following the GIFtag is considered to be data strings of 64 bits x 2 as they are, and the register
descriptors put in the REGS field of the GIFtag show to which register the data is written.


## 3.3.5. IMAGE Mode


IMAGE mode transfers image data by means of the host-local transfer function of the GS. The data following
the GIFtag is considered to be data strings of 64 bits x 2 and is written to the HWREG register of the GS
consecutively.


- © SCEI

- 62-




SCE CONFIDENTIAL EE Overview Version 6.0


## 3.4. Image Decompression by IPU


The IPU (Image Processing Unit) is an image data processor whose main functions are bit stream
decompression and macro block decoding of MPEG2. Compressed data in main memory is decoded,
decompressed, and written back again to main memory. The decoded images are transferred to the GS and used
as moving picture image data and texture data.
Figure 3-9 illustrates the basic processing flow of the IPU.


<table>
 <tr>
  <th>Input data BS128</th>
  <td>RAW8</td>
  <td></td>
  <td>RGB32</td>
 </tr>
 <tr>
  <th>↓</th>
  <td colspan="3">↓ ↓</td>
 </tr>
 <tr>
  <th>Processing VLC</th>
  <td>CSC</td>
  <td>→</td>
  <td>Dither</td>
 </tr>
 <tr>
  <th>↓</th>
  <td colspan="3">↓ ↓</td>
 </tr>
 <tr>
  <th>Output data CODE16</th>
  <td>RGB32</td>
  <td></td>
  <td>RGB16</td>
 </tr>
</table>


- → IDCT → → VQ ↓ ↓ RAW16 INDX4


Figure 3-9 IPU Processing Flow


The IPU has the following basic functions:


- MPEG2 macro block layer decoding

- MPEG2 bit stream decoding

- Bit stream decompression


The IPU has the following additional post-processing functions.


- YCbCr → RGB color conversion (CSC)

- 4 x 4 ordered dither

- Vector quantization (VQ)


The IPU handles the following data formats:
pixels (A8+R8+G8+B8)
pixels (A1+R5+G5+B5)
Unsigned
pixels
Singed
pixels (Only
effective.)
Unsigned
pixels


<table>
 <tr>
  <th>Name</th>
  <th>Contents</th>
  <th>Width</th>
 </tr>
 <tr>
  <td>BS128</td>
  <td>MPEG2 bit stream subset</td>
  <td>128 bits</td>
 </tr>
 <tr>
  <td>RGB32</td>
  <td>RGBA</td>
  <td>32 bits</td>
 </tr>
 <tr>
  <td>RGB16</td>
  <td>RGBA</td>
  <td>16 bits</td>
 </tr>
 <tr>
  <td>RAW8</td>
  <td>8-bit YCbCr</td>
  <td>8 bits</td>
 </tr>
 <tr>
  <td>RAW16</td>
  <td>16-bit YCbCr lower 9 bits are</td>
  <td>16 bits</td>
 </tr>
 <tr>
  <td>INDX4</td>
  <td>4-bit index</td>
  <td>4 bits</td>
 </tr>
</table>


- 63-

- © SCEI




SCE CONFIDENTIAL EE Overview Version 6.0


The following commands are available:
Input
decoding


<table>
 <tr>
  <th>Name</th>
  <td>Contents</td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <th>BCLR</th>
  <td>FIFO initialization command</td>
  <td>-</td>
  <td>-</td>
 </tr>
 <tr>
  <th>IDEC</th>
  <td>Intra decoding command</td>
  <td>BS128</td>
  <td>RGB32/RGB16</td>
 </tr>
 <tr>
  <th>BDEC</th>
  <td>Block command</td>
  <td>BS128</td>
  <td>RAW16</td>
 </tr>
 <tr>
  <th>VDEC</th>
  <td>Variable-length data decoding command</td>
  <td>BS128</td>
  <td>Variable-length code +<br/>code</td>
 </tr>
 <tr>
  <th>FDEC</th>
  <td>data command</td>
  <td>BS128</td>
  <td>data</td>
 </tr>
 <tr>
  <th>SETIQ</th>
  <td>IQ table command</td>
  <td>RAW8</td>
  <td>-</td>
 </tr>
 <tr>
  <th>SETVQ</th>
  <td>VQ table command</td>
  <td>RGB16</td>
  <td>-</td>
 </tr>
 <tr>
  <th>CSC</th>
  <td>Color conversion command</td>
  <td>RAW8</td>
  <td>RGB32/RGB16</td>
 </tr>
 <tr>
  <th>PACK</th>
  <td>Format conversion command</td>
  <td>RGB32</td>
  <td>RGB16/INDX4</td>
 </tr>
 <tr>
  <th>SETTH</th>
  <td>Threshold command</td>
  <td>-</td>
  <td>-</td>
 </tr>
</table>


Fixed-length
decoding
setting
setting
space
setting
Other functional features are as follows.


In decoding an MPEG2 bit stream, motion compensation (MC) is not
performed in the IPU, but in the EE core, by using multimedia
instructions.
The alpha plane (transparency plane) is generated from the decoded
luminance value according to a fixed rule. This is useful in effectively
cutting out the texture pattern when decoding the bit stream without
the stencil pattern (transparent pixel mask pattern).


- Motion Compensation (MC)

- Automatic Generation of Alpha

- © SCEI

- 64-


