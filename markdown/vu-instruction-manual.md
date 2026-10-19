

# Playstation 2 Vector Unit Instruction Manual


by BigBoss


Layout changes and additions by Jules (jules@mouthshut.net)


Updates


1/7/2003 - Initial Version




## Vector Units : Introduction


### GENERAL


The VU0 is the first of two Vector Processing Units in PS2. It's designed to operate in two modes : as the
second coprocessor for the 5900 core or independently as a microprocessor. Operating as a coprocessor the
VU0 utilises either the upper or the lower instruction in each 64-bit microcode stored in it's instruction
cache. Operating as an independent microprocessor the VU0 receives data fed from DMA to VIF0 and finally to
the VU0 instruction or data cache. In
microprocessor mode the VU0 utilises both upper and lower microcode instructions.


The VU1 is an independent microproseccor operating in parallel to the CPU. It executes the program in its
instruction cache on demand of a VIF1 program. VU1 programms use both upper and lower microcode instructions
and and has additional commands to the VU0, because of the EPU and the path to the GIF.


### REGISTERS


The VU0 has 32 128 bit floating point registers , 16 32 bit integer data registers and 16 control registers.


### FLOATING POINT REGISTERS


The floating point registers are 128 bits wide and each is divided in 4
fields of 32 bits : x , y , z and w.


### FLOATING POINT REGISTERS FIELD LAYOUT


Word 0 Word 1 Word 2 Word 3
___________________________________________________________________
|31 0|63 32|95 64|127 96|


- | x | y | z | w |

- | | | | |


### VU0F00 AND VU1F00 REGISTERS


Word 0 Word 1 Word 2 Word 3
___________________________________________________________________
|31 0|63 32|95 64|127 96|


- | | | | |

- | 0.0 | 0.0 | 0.0 | 1.0 |




The Floating registers can be represented in 4 fixed point formats:
Mode Meaning
Fixed 0 fixed point 0 bits
Fixed 4 fixed point 4 bits
Fixed 12 fixed point 12 bits
Fixed 15 fixed point 15 bits


### INTEGER DATA REGISTERS


The Integer data registers are 32 bits wide but only the lower halfword is used as the integer registers
work as pointers to the instruction or data cache.


### INTEGER CONTROL REGISTERS


The Integer control registers are:
Status flag
MAC flag
clipping flag
reserved
R
I
Q
reserved
reserved
reserved
TPC
CMSAR0
FBRST
VPU_STAT
reserved
CMSAR1


### Status Flag


Bits : 32
Bits used : lower 12
Instructions : FSAND
FSEQ
FSOR
FSSET


rt,12 bit immediate
rt,12 bit immediate
rt,12 bit immediate
12 bit immediate




#### MAC flag


Bits : 32
Bits used : lower 16
Instructions : FMAND
FMEQ
FMOR


rt,rs
rt,rs
rt,rs


### Clipping flag


Bits : 32
Bits used : lower 24
Instructions : FCAND
FCEQ
FCGET
FCOR
FCSET


rt,24 bit immediate
rt,24 bit immediate
rt
rt,24 bit immediate
24 bit immediate


### R register


Bits : 32
Bits used : 32
Instructions : RINIT
RGET
RNEXT
RXOR


R,Frs.e
Frt,R
Frt,R
R,Frs


### I register


Bits : 32
Bits used : 32 (floating
Instructions : ADDI
SUBI
MULI
MADDI
MSUBI
ADDAI
SUBAI
MULAI
MADDAI
MSUBAI
MAXII
MINII


point)
Upper
Upper
Upper
Upper
Upper
Upper
Upper
Upper
Upper
Upper
Upper
Upper




#### Q register


Bits : 32
Bits used : 32 (floating point)
Instructions : ADDQ Upper
SUBQ Upper
MULQ Upper
MADDQ Upper
MSUBQ Upper
ADDAQ Upper
SUBAQ Upper
MULAQ Upper
MADDAQ Upper
MSUBAQ Upper
DIV Q,rt,rs Lower
SQRT Q,rs Lower
RSQRT Q,rs Lower


TPC ????? 2 Bytes used
FBRST ????? sceGSreset sets the bits in this disabling break
VPU-STAT ?????
CMSAR0 ?????
CMSAR1 ?????




##### Reading the manual


It can be quite confusing reading the manual for the first time so here is a quick introduction to
understanding the manual and basics of VU.


First lets remember that there are 32 x 128 bit general purpose registers, each of these 128 bits registers
is split into 4 x 32 bits (floats). These 32 bit parts of the 128 bit register is accessed by X,Y,Z,W (like
a vector).


### Register:


Bit: 128 96 64 32 0


| W | Z | Y | X |


Register are named VF00 - VF31, VF00 is static and has the values; X = 0.0f, Y = 0.0f, Z = 0.0f, W = 1.0f


Now lets look at some documentation.


<table>
 <tr>
  <th>Add broadcast</th>
  <th></th>
  <th></th>
  <th>ADDy</th>
  <th>ADDz ADDw</th>
 </tr>
 <tr>
  <td>|31|30|29|28|27|26|25|24|23|22|21|20</td>
  <td>16|15</td>
  <td>11|10</td>
  <td>06|05</td>
  <td>02|01|00|</td>
 </tr>
 <tr>
  <td>| I| E| M| D| T| 0| 0| x| y| z| w|</td>
  <td>|</td>
  <td>|</td>
  <td>|</td>
  <td>ADDbc | bc |</td>
 </tr>
 <tr>
  <td>| | 0| 0| dest |</td>
  <td>|</td>
  <td>|</td>
  <td>|</td>
  <td>0000 | |</td>
 </tr>
 <tr>
  <td>Format: ADDx.dest fd,fs,ft</td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td>ADDy.dest fd,fs,ft</td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td>ADDz.dest fd,fs,ft</td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td>ADDw.dest fd,fs,ft</td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td>Purpose:</td>
  <td></td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td>To add up to 4 fields with the</td>
  <td>field</td>
  <td></td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td>Description: if dest &amp; .x == true :</td>
  <td>fd.x &lt;- fs.x +</td>
  <td>ft.bc</td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td>if dest &amp; .y == true :</td>
  <td>fd.y &lt;- fs.y +</td>
  <td>ft.bc</td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td>if dest &amp; .z == true :</td>
  <td>fd.z &lt;- fs.z +</td>
  <td>ft.bc</td>
  <td></td>
  <td></td>
 </tr>
 <tr>
  <td>if dest &amp; .w == true :</td>
  <td>fd.w &lt;- fs.w +</td>
  <td>ft.bc</td>
  <td></td>
  <td></td>
 </tr>
</table>




First, lets look at description, "if dest & .x == true", this means that if we can do


addy.x $vf3x, $vf4x, $vf5y


This performs $vf3.x (x part of vf3) = $vf4.x (x part of vf4) + $vf5.y (y part of vf5)


But what the description also saids, is that we can combine different dests


addy.xyzw $vf3xywz, $vf4xywz, $vf5y


This performs


$vf3.x = $vf4.x + $vf5.y
$vf3.y = $vf4.y + $vf5.y
$vf3.z = $vf4.z + $vf5.y
$vf3.w = $vf4.w + $vf5.y


Or ..


addz.xw $vf3xw, $vf4xw, $vf5z


Which performs


$vf3.x = $vf4.x + $vf5.z
$vf3.w = $vf4.w + $vf5.z


You can also just write this instruction like this and still make sense because of the last z on $vf5


addz.xw $vf3, $vf4, $vf5z


As you can see VU is very flexible and as you learn more and read others code you will learn quite a few
tricks which will result in fast VU code.


### VU0 INSTRUCTION NOTE


This manaul covers VU1 instructions, but the VU1 instructions which are avaliable on VU0 are noted on the
following pages. Just remember that you need to add a V as the first letter of the instruction, like
VU1 Instruction: addy.x $vf3x, $vf4x, $vf5y
VU0 Instruction: vaddy.x $vf3x, $vf4x, $vf5y




#### UPPER INSTRUCTION AVAILABILITY IN VU UNITS


Instruction VU0 VU0 VU1
coprocessor microprocessor


### Instruction VU0 coprocessor


### VU0 VU1 microprocessor


ADD.dest


1. 


Q.dest


e.dest


ADDA.dest


1. 


Q.dest


e.dest


SUB.dest


1. 


Q.dest


e.dest


SUBA.dest


1. 


Q.dest


e.dest


MUL.dest


1. 


Q.dest


e.dest


MULA.dest


1. 


Q.dest


e.dest


MADD.dest


1. 


Q.dest


e.dest


MADDA.dest


1. Yes


Q.dest Yes


e.dest Yes


Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes


Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes


Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes


MSUB.dest


1. 


I.dest


e.dest


MSUBA.dest


1. Yes


Q.dest Yes


e.dest Yes


OPMULA.xyz Yes
OPMSUB.xyz
ABS.dest
MAX.dest


1. 


e.dest


MINI.dest


1. 


e.dest


CLIPw.dest
ITOF0.dest


1. 


12.dest Yes


15.dest Yes


FTOI0.dest Yes


1. Yes


12.dest Yes


15.dest Yes


NOP Yes


Yes
Yes
Yes
Yes
Yes


Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes


Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes


Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes




#### LOWER INSTRUCTION AVAILABILITY IN VU UNITS


VU0


VU0


Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes


Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
Yes
No
No
No
No
No
No
No
No
No
No
No
No
No
No
No
No
No




## UPPER VU1 INSTRUCTION ENCODING


### UPPER VU1 INSTRUCTION LAYOUT


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | I| E| M| D| T| 0| 0| x| y| z| w| ft | fs | fd | OPCODE | bc |

- | flags | 0| 0| dest | | | | | |


FLAG USAGE
i Floating point value in lower microcode
e Unknown
m Unknown
d Breakpoint. The code breaks after the current instruction executes
t Unknown


UPPER VU1 INSTRUCTION FLAGS




#### Absolute ABS


____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | I| E| M| D| T| 0| 0| x| y| z| w| ft | fs | ABS | SPECIAL | bc |

- | | 0| 0| dest | | | 00111 | 1111 | 01 |

- Format: ABS.dest fs,ft


Purpose:


To calculate the absolute values of up to 4 fields


Description: if dest & .x == true : fs.x <- abs(ft.x)
if dest & .y == true : fs.y <- abs(ft.y)
if dest & .z == true : fs.z <- abs(ft.z)
if dest & .w == true : fs.w <- abs(ft.w)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




##### Add ADD


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 00|


- | I| E| M| D| T| 0| 0| x| y| z| w| ft | fs | fd | ADD |

- | | 0| 0| dest | | | | 101000 |


Format: ADD.dest fd,fs,ft


Purpose:


To add up to 4 matching fields


Description: if dest & .x == true : fd.x <- fs.x + ft.x
if dest & .y == true : fd.y <- fs.y + ft.y
if dest & .z == true : fd.z <- fs.z + ft.z
if dest & .w == true : fd.w <- fs.w + ft.w


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




##### Add I ADDI


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 00|


- | I| E| M| D| T| 0| 0| x| y| z| w| 0 | fs | fd | ADDI |

- | | 0| 0| dest | 00000 | | | 100010 |


Format: ADDI.dest fd,fs,I


Purpose:


To add up to 4 fields with the I register


Description: if dest & .x == true : fd.x <- fs.x + I
if dest & .y == true : fd.y <- fs.y + I
if dest & .z == true : fd.z <- fs.z + I
if dest & .w == true : fd.w <- fs.w + I


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




##### Add Q ADDQ


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 00|


- | I| E| M| D| T| 0| 0| x| y| z| w| 0 | fs | fd | ADDQ |

- | | 0| 0| dest | 00000 | | | 100000 |


Format: ADDQ.dest fd,fs,Q


Purpose:


To add up to 4 fields with the Q register


Description: if dest & .x == true : fd.x <- fs.x + Q
if dest & .y == true : fd.y <- fs.y + Q
if dest & .z == true : fd.z <- fs.z + Q
if dest & .w == true : fd.w <- fs.w + Q


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




##### Add broadcast ADDx ADDy ADDz ADDw


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | I| E| M| D| T| 0| 0| x| y| z| w| ft | fs | fd | ADDbc | bc |

- | | 0| 0| dest | | | | 0000 | |


Format: ADDx.dest fd,fs,ft
ADDy.dest fd,fs,ft
ADDz.dest fd,fs,ft
ADDw.dest fd,fs,ft


Purpose:


To add up to 4 fields with the broadcast field


Description: if dest & .x == true : fd.x <- fs.x + ft.bc
if dest & .y == true : fd.y <- fs.y + ft.bc
if dest & .z == true : fd.z <- fs.z + ft.bc
if dest & .w == true : fd.w <- fs.w + ft.bc


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




##### Add accumulator ADDA


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | I| E| M| D| T| 0| 0| x| y| z| w| ft | fs | ADDA | SPECIAL | bc |

- | | 0| 0| dest | | | 01010 | 1111 | 00 |


Format: ADDA.dest ACC,fs,ft


Purpose:


To add up to 4 matching fields


Description: if dest & .x == true : ACC.x <- fs.x + ft.x
if dest & .y == true : ACC.y <- fs.y + ft.y
if dest & .z == true : ACC.z <- fs.z + ft.z
if dest & .w == true : ACC.w <- fs.w + ft.w


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




##### Add accumulator I ADDAI


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | I| E| M| D| T| 0| 0| x| y| z| w| 0 | fs | ADDAI | SPECIAL | bc |

- | | 0| 0| dest | 00000 | | 01000 | 1111 | 10 |


Format: ADDAI.dest ACC,fs,I


Purpose:


To add up to 4 fields with the I register


Description: if dest & .x == true : ACC.x <- fs.x + I
if dest & .y == true : ACC.y <- fs.y + I
if dest & .z == true : ACC.z <- fs.z + I
if dest & .w == true : ACC.w <- fs.w + I


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




##### Add accumulator Q ADDAQ


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | I| E| M| D| T| 0| 0| x| y| z| w| 0 | fs | ADDAQ | SPECIAL | bc |

- | | 0| 0| dest | 00000 | | 01000 | 1111 | 00 |


Format: ADDAQ.dest ACC,fs,Q


Purpose:


To add up to 4 fields with the Q register


Description: if dest & .x == true : ACC.x <- fs.x + Q
if dest & .y == true : ACC.y <- fs.y + Q
if dest & .z == true : ACC.z <- fs.z + Q
if dest & .w == true : ACC.w <- fs.w + Q


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




##### Add accumulator broadcast ADDAx ADDAy ADDAz ADDAw


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | I| E| M| D| T| 0| 0| x| y| z| w| ft | fs | ADDAbc | SPECIAL | bc |

- | | 0| 0| dest | | | 00000 | 1111 | 00 |


Format: ADDAx.dest ACC,fs,ft
ADDAy.dest ACC,fs,ft
ADDAz.dest ACC,fs,ft
ADDAw.dest ACC,fs,ft


Purpose:


To add up to 4 fields with the broadcast field


Description: if dest & .x == true : ACC.x <- fs.x + ft.bc
if dest & .y == true : ACC.y <- fs.y + ft.bc
if dest & .z == true : ACC.z <- fs.z + ft.bc
if dest & .w == true : ACC.w <- fs.w + ft.bc


### Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Clip CLIPw


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | I| E| M| D| T| 0| 0| x| y| z| w| ft | fs | CLIPw | SPECIAL | bc |

- | | 0| 0| dest | | | 00111 | 1111 | 11 |


Format: CLIPw.dest ft,fs


Purpose:


To update the status flag about the clipping condition of the destination values


Description:


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Floating to integer FTOI0 FTOI4 FTOI12 FTOI15


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | I| E| M| D| T| 0| 0| x| y| z| w| ft | fs | FTOI | SPECIAL | bc |

- | | 0| 0| dest | | | 00101 | 1111 | |


Format: FTOI0.dest It,fs
FTOI4.dest It,fs
FTOI12.dest It,fs
FTOI15.dest It,fs


Purpose:


To convert a floating point value to a fixed point (integer) number of 0, 4, 12 or 15
decimal digits


Description: if dest & .x == true : ft <- FixedPoint0/4/12/15(fs.x)
elseif dest & .y == true : ft <- FixedPoint0/4/12/15(fs.y)
elseif dest & .z == true : ft <- FixedPoint0/4/12/15(fs.z)
elseif dest & .w == true : ft <- FixedPoint0/4/12/15(fs.w)


### Restrictions:


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1. It is primarily used with the Random


number generator
The broadcast field specifies the number of
Broadcast field Meaning


<table>
 <tr>
  <th>00</th>
  <th>0</th>
  <th>decimal digits</th>
 </tr>
 <tr>
  <td>01</td>
  <td>4</td>
  <td>decimal digits</td>
 </tr>
 <tr>
  <td>10</td>
  <td>12</td>
  <td>decimal digits</td>
 </tr>
 <tr>
  <td>11</td>
  <td>15</td>
  <td>decimal digits</td>
 </tr>
</table>


decimal digits




#### Integer to floating point ITOF0 ITOF4 ITOF12 ITOF15


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | I| E| M| D| T| 0| 0| x| y| z| w| ft | fs | ITOF | SPECIAL | bc |

- | | 0| 0| dest | | | 00100 | 1111 | |


Format: ITOF0.dest ft,fs
ITOF4.dest ft,fs
ITOF12.dest ft,fs
ITOF15.dest ft,fs


Purpose:


To convert a fixed point (integer) value of 0, 4, 12 or 15 decimal digits to a floating
point number


Description: if dest & .x == true : ft.x <- FixedPoint0/4/12/15(fs)
elseif dest & .y == true : ft.y <- FixedPoint0/4/12/15(fs)
elseif dest & .z == true : ft.z <- FixedPoint0/4/12/15(fs)
elseif dest & .w == true : ft.w <- FixedPoint0/4/12/15(fs)


### Restrictions:


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1. It is primarily used with the Random


number generator
The broadcast field specifies the number of
Broadcast field Meaning


<table>
 <tr>
  <th>00</th>
  <th>0</th>
  <th>decimal digits</th>
 </tr>
 <tr>
  <td>01</td>
  <td>4</td>
  <td>decimal digits</td>
 </tr>
 <tr>
  <td>10</td>
  <td>12</td>
  <td>decimal digits</td>
 </tr>
 <tr>
  <td>11</td>
  <td>15</td>
  <td>decimal digits</td>
 </tr>
</table>


decimal digits




#### Multiply and add MADD


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 00|


- | I| E| M| D| T| 0| 0| x| y| z| w| ft | fs | fd | MADD |

- | | 0| 0| dest | | | | 101001 |


Format: MADD.dest fd,fs,ft


Purpose:


To multiply up to 4 matching fields and add the products with matching fields of the
accumulator


Description: if dest & .x == true : fd.x <- ACC.x + (fs.x * ft.x)
if dest & .y == true : fd.y <- ACC.y + (fs.y * ft.y)
if dest & .z == true : fd.z <- ACC.z + (fs.z * ft.z)
if dest & .w == true : fd.w <- ACC.w + (fs.w * ft.w)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Multiply and add I MADDI


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 00|


- | I| E| M| D| T| 0| 0| x| y| z| w| 0 | fs | fd | MADDI |

- | | 0| 0| dest | 00000 | | | 100011 |


Format: MADDI.dest fd,fs,I


Purpose:


To multiply up to 4 fields with the I register and add the products with matching fields
of the accumulator


Description: if dest & .x == true : fd.x <- ACC.x + (fs.x * I)
if dest & .y == true : fd.y <- ACC.y + (fs.y * I)
if dest & .z == true : fd.z <- ACC.z + (fs.z * I)
if dest & .w == true : fd.w <- ACC.w + (fs.w * I)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Multiply and add Q MADDQ


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 00|


- | I| E| M| D| T| 0| 0| x| y| z| w| 0 | fs | fd | MADDQ |

- | | 0| 0| dest | 00000 | | | 100001 |


Format: MADDQ.dest fd,fs,Q


Purpose:


To multiply up to 4 fields with the Q register and add the products with matching fields
of the accumulator


Description: if dest & .x == true : fd.x <- ACC.x + (fs.x * Q)
if dest & .y == true : fd.y <- ACC.y + (fs.y * Q)
if dest & .z == true : fd.z <- ACC.z + (fs.z * Q)
if dest & .w == true : fd.w <- ACC.w + (fs.w * Q)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Multiply and add broadcast MADDx MADDy MADDz MADDw


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | I| E| M| D| T| 0| 0| x| y| z| w| ft | fs | fd | MADDbc | bc |

- | | 0| 0| dest | | | | 0010 | |


Format: MADDx.dest fd,fs,ft
MADDy.dest fd,fs,ft
MADDz.dest fd,fs,ft
MADDw.dest fd,fs,ft


Purpose:


To multiply up to 4 fields with the broadcast field and add the products with matching
fields of the accumulator


Description: if dest & .x == true : fd.x <- ACC.x + (fs.x * ft.bc)
if dest & .y == true : fd.y <- ACC.y + (fs.y * ft.bc)
if dest & .z == true : fd.z <- ACC.z + (fs.z * ft.bc)
if dest & .w == true : fd.w <- ACC.w + (fs.w * ft.bc)


### Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Multiply and add accumulator MADDA


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | I| E| M| D| T| 0| 0| x| y| z| w| ft | fs | MADDA | SPECIAL | bc |

- | | 0| 0| dest | | | 01010 | 1111 | 01 |


Format: MADDA.dest ACC,fs,ft


Purpose:


To multiply up to 4 fields and add the products with matching fields in the accumulator


Description: if dest & .x == true : ACC.x <- ACC.x + (fs.x * ft.x)
if dest & .y == true : ACC.y <- ACC.y + (fs.y * ft.y)
if dest & .z == true : ACC.z <- ACC.z + (fs.z * ft.z)
if dest & .w == true : ACC.w <- ACC.w + (fs.w * ft.w)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Multiply and add accumulator I MADDAI


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | I| E| M| D| T| 0| 0| x| y| z| w| 0 | fs | MADDAI | SPECIAL | bc |

- | | 0| 0| dest | 00000 | | 01000 | 1111 | 11 |


Format: MADDAI.dest ACC,fs,I


Purpose:


To multiply up to 4 fields with the I register and add the products with matching fields
in the accumulator


Description: if dest & .x == true : ACC.x <- ACC.x + (fs.x * I)
if dest & .y == true : ACC.y <- ACC.y + (fs.y * I)
if dest & .z == true : ACC.z <- ACC.z + (fs.z * I)
if dest & .w == true : ACC.w <- ACC.w + (fs.w * I)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Multiply and add accumulator Q MADDAQ


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | I| E| M| D| T| 0| 0| x| y| z| w| 0 | fs | MADDAQ | SPECIAL | bc |

- | | 0| 0| dest | 00000 | | 01000 | 1111 | 01 |


Format: MADDAI.dest ACC,fs,Q


Purpose:


To multiply up to 4 fields with the Q register and add the products with matching fields
in the accumulator


Description: if dest & .x == true : ACC.x <- ACC.x + (fs.x * Q)
if dest & .y == true : ACC.y <- ACC.y + (fs.y * Q)
if dest & .z == true : ACC.z <- ACC.z + (fs.z * Q)
if dest & .w == true : ACC.w <- ACC.w + (fs.w * Q)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Multiply and add accumulator broadcast MADDAx MADDAy MADDAz


### MADDAw


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | I| E| M| D| T| 0| 0| x| y| z| w| ft | fs | MADDAbc | SPECIAL | bc |

- | | 0| 0| dest | | | 01000 | 1111 | |


Format: MADDAx.dest ACC,fs,ft
MADDAy.dest ACC,fs,ft
MADDAz.dest ACC,fs,ft
MADDAw.dest ACC,fs,ft


### Purpose:


To multiply up to 4 fields with the broadcast field and add the products with matching
fields in the accumulator


Description: if dest & .x == true : ACC.x <- ACC.x + (fs.x * ft.x)
if dest & .y == true : ACC.y <- ACC.y + (fs.y * ft.y)
if dest & .z == true : ACC.z <- ACC.z + (fs.z * ft.z)
if dest & .w == true : ACC.w <- ACC.w + (fs.w * ft.w)


### Restrictions:


None


Operation:


Exceptions:


### Programming notes:


This instruction is applicable to both VU0 and VU1




#### Maximum


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 00|


- | I| E| M| D| T| 0| 0| x| y| z| w| ft | fs | fd | MAX |

- | | 0| 0| dest | | | | 101011 |


### MAX


Format: MAX.dest fd,fs,ft


Purpose:


To find the maximum value between up to 4 matching fields


Description: if dest & .x == true : fd.x <- max(fs.x : ft.bc)
if dest & .y == true : fd.y <- max(fs.y : ft.bc)
if dest & .z == true : fd.z <- max(fs.z : ft.bc)
if dest & .w == true : fd.w <- max(fs.w : ft.bc)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Maximum I MAXI


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 00|


- | I| E| M| D| T| 0| 0| x| y| z| w| 0 | fs | fd | MAXI |

- | | 0| 0| dest | 00000 | | | 011101 |


Format: MAXI.dest fd,fs,I


Purpose:


To find the maximum value between up to 4 fields and the I register


Description: if dest & .x == true : fd.x <- max(fs.x : I)
if dest & .y == true : fd.y <- max(fs.y : I)
if dest & .z == true : fd.z <- max(fs.z : I)
if dest & .w == true : fd.w <- max(fs.w : I)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Maximum broadcast MAXx MAXy MAXz MAXw


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | I| E| M| D| T| 0| 0| x| y| z| w| ft | fs | fd | MAXbc | bc |

- | | 0| 0| dest | | | | 0100 | |


Format: MAXx.dest fd,fs,ft
MAXy.dest fd,fs,ft
MAXz.dest fd,fs,ft
MAXw.dest fd,fs,ft


Purpose:


To find the maximum value between up to 4 fields and the I register


Description: if dest & .x == true : fd.x <- max(fs.x : ft.bc)
if dest & .y == true : fd.y <- max(fs.y : ft.bc)
if dest & .z == true : fd.z <- max(fs.z : ft.bc)
if dest & .w == true : fd.w <- max(fs.w : ft.bc)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Minimum MINI


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 00|


- | I| E| M| D| T| 0| 0| x| y| z| w| ft | fs | fd | MINI |

- | | 0| 0| dest | | | | 101111 |


Format: MINI.dest fd,fs,ft


Purpose:


To find the minimum value between up to 4 matching fields


Description: if dest & .x == true : fd.x <- mini(fs.x : ft.x)
if dest & .y == true : fd.y <- mini(fs.y : ft.y)
if dest & .z == true : fd.z <- mini(fs.z : ft.z)
if dest & .w == true : fd.w <- mini(fs.w : ft.w)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Minimum I MINII


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 00|


- | I| E| M| D| T| 0| 0| x| y| z| w| 0 | fs | fd | MINII |

- | | 0| 0| dest | 00000 | | | 011111 |


Format: MINII.dest fd,fs,ft


Purpose:


To find the minimum value between up to 4 fields and the I register


Description: if dest & .x == true : fd.x <- mini(fs.x : I)
if dest & .y == true : fd.y <- mini(fs.y : I)
if dest & .z == true : fd.z <- mini(fs.z : I)
if dest & .w == true : fd.w <- mini(fs.w : I)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Minimum broadcast MINIx MINIy MINIz MINIw


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | I| E| M| D| T| 0| 0| x| y| z| w| ft | fs | fd | MINIbc | bc |

- | | 0| 0| dest | | | | 0101 | |


Format: MINIx.dest fd,fs,ft
MINIy.dest fd,fs,ft
MINIz.dest fd,fs,ft
MINIw.dest fd,fs,ft


Purpose:


To find the minimum value between up to 4 fields and the I register


Description: if dest & .x == true : fd.x <- mini(fs.x : ft.bc)
if dest & .y == true : fd.y <- mini(fs.y : ft.bc)
if dest & .z == true : fd.z <- mini(fs.z : ft.bc)
if dest & .w == true : fd.w <- mini(fs.w : ft.bc)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




##### Multiply and substract MSUB


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 00|


- | I| E| M| D| T| 0| 0| x| y| z| w| ft | fs | fd | MSUB |

- | | 0| 0| dest | | | | 101101 |


Format: MSUB.dest fd,fs,ft


Purpose:


To multiply up to 4 matching fields and substract the products from matching fields of
the accumulator


Description: if dest & .x == true : fd.x <- ACC.x - (fs.x * ft.x)
if dest & .y == true : fd.y <- ACC.y - (fs.y * ft.y)
if dest & .z == true : fd.z <- ACC.z - (fs.z * ft.z)
if dest & .w == true : fd.w <- ACC.w - (fs.w * ft.w)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Multiply and substract I MSUBI


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 00|


- | I| E| M| D| T| 0| 0| x| y| z| w| 0 | fs | fd | MSUBI |

- | | 0| 0| dest | 00000 | | | 100111 |


Format: MSUBI.dest fd,fs,ft


Purpose:


To multiply up to 4 fields with the I register and substract the products from matching
fields of the accumulator


Description: if dest & .x == true : fd.x <- ACC.x - (fs.x * I)
if dest & .y == true : fd.y <- ACC.y - (fs.y * I)
if dest & .z == true : fd.z <- ACC.z - (fs.z * I)
if dest & .w == true : fd.w <- ACC.w - (fs.w * I)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Multiply and substract Q MSUBQ


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 00|


- | I| E| M| D| T| 0| 0| x| y| z| w| 0 | fs | fd | MSUBQ |

- | | 0| 0| dest | 00000 | | | 100101 |


Format: MSUBQ.dest fd,fs,ft


Purpose:


To multiply up to 4 fields with the Q register and substract the products from matching
fields of the accumulator


Description: if dest & .x == true : fd.x <- ACC.x - (fs.x * Q)
if dest & .y == true : fd.y <- ACC.y - (fs.y * Q)
if dest & .z == true : fd.z <- ACC.z - (fs.z * Q)
if dest & .w == true : fd.w <- ACC.w - (fs.w * Q)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Multiply and substract broadcast MSUBx MSUBy MSUBz MSUBw


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | I| E| M| D| T| 0| 0| x| y| z| w| ft | fs | fd | MSUBbc | bc |

- | | 0| 0| dest | | | | 0011 | |


Format: MSUBx.dest fd,fs,ft
MSUBy.dest fd,fs,ft
MSUBw.dest fd,fs,ft
MSUBw.dest fd,fs,ft


Purpose:


To multiply up to 4 fields with the broadcast field and substract the products from
matching fields of the accumulator


Description: if dest & .x == true : fd.x <- ACC.x - (fs.x * ft.bc)
if dest & .y == true : fd.y <- ACC.y - (fs.y * ft.bc)
if dest & .z == true : fd.z <- ACC.z - (fs.z * ft.bc)
if dest & .w == true : fd.w <- ACC.w - (fs.w * ft.bc)


### Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Multiply and substract accumulator MSUBA


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | I| E| M| D| T| 0| 0| x| y| z| w| ft | fs | MSUBA | SPECIAL | bc |

- | | 0| 0| dest | | | 01011 | 1111 | 01 |


Format: MSUBA.dest fd,fs,ft


Purpose:


To multiply up to 4 fields and substract the products from matching fields in the
accumulator


Description: if dest & .x == true : ACC.x <- ACC.x - (fs.x * ft.bc)
if dest & .y == true : ACC.y <- ACC.y - (fs.y * ft.bc)
if dest & .z == true : ACC.z <- ACC.z - (fs.z * ft.bc)
if dest & .w == true : ACC.w <- ACC.w - (fs.w * ft.bc)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Multiply and substract accumulator I MSUBAI


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | I| E| M| D| T| 0| 0| x| y| z| w| 0 | fs | MSUBAI | SPECIAL | bc |

- | | 0| 0| dest | 00000 | | 01001 | 1111 | 11 |


Format: MSUBAI.dest fd,fs,ft


Purpose:


To multiply up to 4 fields with the I register and substract the products from matching
fields in the accumulator


Description: if dest & .x == true : ACC.x <- ACC.x - (fs.x * I)
if dest & .y == true : ACC.y <- ACC.y - (fs.y * I)
if dest & .z == true : ACC.z <- ACC.z - (fs.z * I)
if dest & .w == true : ACC.w <- ACC.w - (fs.w * I)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Multiply and substract accumulator Q MSUBAQ


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | I| E| M| D| T| 0| 0| x| y| z| w| 0 | fs | MSUBAQ | SPECIAL | bc |

- | | 0| 0| dest | 00000 | | 01001 | 1111 | 01 |


Format: MSUBAQ.dest fd,fs,ft


Purpose:


To multiply up to 4 fields with the Q register and substract the products from matching
fields in the accumulator


Description: if dest & .x == true : ACC.x <- ACC.x - (fs.x * Q)
if dest & .y == true : ACC.y <- ACC.y - (fs.y * Q)
if dest & .z == true : ACC.z <- ACC.z - (fs.z * Q)
if dest & .w == true : ACC.w <- ACC.w - (fs.w * Q)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Multiply and substract accumulator broadcast MSUBAx MSUBAy MSUBAz MSUBAw


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | I| E| M| D| T| 0| 0| x| y| z| w| 0 | fs | MSUBAbc | SPECIAL | bc |

- | | 0| 0| dest | 00000 | | 00011 | 1111 | |


Format: MSUBAx.dest fd,fs,ft
MSUBAy.dest fd,fs,ft
MSUBAz.dest fd,fs,ft
MSUBAw.dest fd,fs,ft


Purpose:


To multiply up to 4 fields with the broadcast field and substract the products from
matching fields in the accumulator


Description: if dest & .x == true : ACC.x <- ACC.x - (fs.x * ft.bc)
if dest & .y == true : ACC.y <- ACC.y - (fs.y * ft.bc)
if dest & .z == true : ACC.z <- ACC.z - (fs.z * ft.bc)
if dest & .w == true : ACC.w <- ACC.w - (fs.w * ft.bc)


### Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Multiply MUL


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 00|


- | I| E| M| D| T| 0| 0| x| y| z| w| ft | fs | fd | MUL |

- | | 0| 0| dest | | | | 101010 |


Format: MUL.dest fd,fs,ft


Purpose:


To multiply up to 4 matching fields


Description: if dest & .x == true : fd.x <- fs.x * ft.x
if dest & .y == true : fd.y <- fs.y * ft.y
if dest & .z == true : fd.z <- fs.z * ft.z
if dest & .w == true : fd.w <- fs.w * ft.w


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Multiply I MULI


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 00|


- | I| E| M| D| T| 0| 0| x| y| z| w| 0 | fs | fd | MULI |

- | | 0| 0| dest | 00000 | | | 011110 |


Format: MULI.dest fd,fs,ft


Purpose:


To multiply up to 4 fields with the I register


Description: if dest & .x == true : fd.x <- fs.x * I)
if dest & .y == true : fd.y <- fs.y * I)
if dest & .z == true : fd.z <- fs.z * I)
if dest & .w == true : fd.w <- fs.w * I)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Multiply Q MULQ


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 00|


- | I| E| M| D| T| 0| 0| x| y| z| w| 0 | fs | fd | MULQ |

- | | 0| 0| dest | 00000 | | | 011100 |


Format: MULQ.dest fd,fs,ft


Purpose:


To multiply up to 4 fields with the Q register


Description: if dest & .x == true : fd.x <- fs.x * Q)
if dest & .y == true : fd.y <- fs.y * Q)
if dest & .z == true : fd.z <- fs.z * Q)
if dest & .w == true : fd.w <- fs.w * Q)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Multiply broadcast MULx MULy MULz MULw


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | I| E| M| D| T| 0| 0| x| y| z| w| ft | fs | fd | MULbc | bc |

- | | 0| 0| dest | | | | 0110 | |


Format: MULx.dest fd,fs,ft
MULy.dest fd,fs,ft
MULz.dest fd,fs,ft
MULw.dest fd,fs,ft


Purpose:


To multiply up to 4 fields with the broadcast field


Description: if dest & .x == true : fd.x <- fs.x * ft.bc)
if dest & .y == true : fd.y <- fs.y * ft.bc)
if dest & .z == true : fd.z <- fs.z * ft.bc)
if dest & .w == true : fd.w <- fs.w * ft.bc)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




##### No operation NOP


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | I| E| M| D| T| 0| 0| x| y| z| w| 0 | 0 | NOP | SPECIAL | bc |

- | | 0| 0| 0| 0| 0| 0| 00000 | 00000 | 01011 | 1111 | 11 |

- Format: NOP


Purpose:


To perform no operation


Description:


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




##### Outer product pre increment OPMULA


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | I| E| M| D| T| 0| 0| x| y| z| w| ft | fs | OPMULA | SPECIAL | bc |

- | | 0| 0| 1| 1| 1| 0| | | 01011 | 1111 | 10 |


Format: OPMULA.xyz ACC,fs,ft


Purpose:


To calculate the outer product pre increment


Description: ACC.x <- (fs.y * ft.z) + (fs.z * ft.y)
ACC.y <- (fs.z * ft.x) + (fs.x * ft.z)
ACC.z <- (fs.x * ft.y) + (fs.y * ft.x)


### Restrictions:


None


Operation:


Exceptions:


### Programming notes:


This instruction is applicable to both VU0 and VU1




#### Outer product post decrement OPMSUB


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 00|


- | I| E| M| D| T| 0| 0| x| y| z| w| ft | fs | fd | OPMSUB |

- | | 0| 0| 1| 1| 1| 0| | | | 011011 |

- Format: OPMSUB.xyz fd,fs,ft


### Purpose:


To calculate the outer product post decrement


Description: fd.x <- (fs.y * ft.z) - (fs.z * ft.y)
fd.y <- (fs.z * ft.x) - (fs.x * ft.z)
fd.z <- (fs.x * ft.y) - (fs.y * ft.x)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Substract


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 00|


- | I| E| M| D| T| 0| 0| x| y| z| w| ft | fs | fd | SUB |

- | | 0| 0| dest | | | | 101100 |


### SUB


Format: SUB.dest fd,fs,ft


Purpose:


To substract up to 4 matching fields


Description: if dest & .x == true : fd.x <- fs.x - ft.x
if dest & .y == true : fd.y <- fs.y - ft.y
if dest & .z == true : fd.z <- fs.z - ft.z
if dest & .w == true : fd.w <- fs.w - ft.w


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Substract I SUBI


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 00|


- | I| E| M| D| T| 0| 0| x| y| z| w| 0 | fs | fd | SUBI |

- | | 0| 0| dest | 00000 | | | 100110 |


Format: SUBI.dest fd,fs,Q


Purpose:


To substract the I register from up to 4 fields


Description: if dest & .x == true : fd.x <- fs.x - I
if dest & .y == true : fd.y <- fs.y - I
if dest & .z == true : fd.z <- fs.z - I
if dest & .w == true : fd.w <- fs.w - I


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Substract Q SUBQ


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 00|


- | I| E| M| D| T| 0| 0| x| y| z| w| 0 | fs | fd | SUBQ |

- | | 0| 0| dest | 00000 | | | 100100 |


Format: SUBQ.dest fd,fs,Q


Purpose:


To substract the Q register from up to 4 fields


Description: if dest & .x == true : fd.x <- fs.x - Q
if dest & .y == true : fd.y <- fs.y - Q
if dest & .z == true : fd.z <- fs.z - Q
if dest & .w == true : fd.w <- fs.w - Q


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Substract broadcast SUBx SUBy SUBz SUBw


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | I| E| M| D| T| 0| 0| x| y| z| w| ft | fs | fd | SUBbc | bc |

- | | 0| 0| dest | | | | 0001 | |


Format: SUBx.dest fd,fs,ft
SUBy.dest fd,fs,ft
SUBz.dest fd,fs,ft
SUBw.dest fd,ft,fs


Purpose:


To substract the broadcast field from up to 4 fields


Description: if dest & .x == true : fd.x <- fs.x - ft.bc
if dest & .y == true : fd.y <- fs.y - ft.bc
if dest & .z == true : fd.z <- fs.z - ft.bc
if dest & .w == true : fd.w <- fs.w - ft.bc


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Substract accumulator SUBA


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | I| E| M| D| T| 0| 0| x| y| z| w| ft | fs | SUBA | SPECIAL | bc |

- | | 0| 0| dest | | | 01011 | 1111 | 00 |


Format: SUBA.dest ACC,fs,ft


Purpose:


To substract up to 4 matching fields to the accumulator


Description: if dest & .x == true : ACC.x <- fs.x - ft.x
if dest & .y == true : ACC.y <- fs.y - ft.y
if dest & .z == true : ACC.z <- fs.z - ft.z
if dest & .w == true : ACC.w <- fs.w - ft.w


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Substract accumulator I SUBAI


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | I| E| M| D| T| 0| 0| x| y| z| w| 0 | fs | SUBAI | SPECIAL | bc |

- | | 0| 0| dest | 00000 | | 01001 | 1111 | 10 |


Format: SUBAI.dest ACC,fs,I


Purpose:


To substract the I register from up to 4 matching fields to the accumulator


Description: if dest & .x == true : ACC.x <- fs.x - I
if dest & .y == true : ACC.y <- fs.y - I
if dest & .z == true : ACC.z <- fs.z - I
if dest & .w == true : ACC.w <- fs.w - I


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Substract accumulator Q SUBAQ


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | I| E| M| D| T| 0| 0| x| y| z| w| 0 | fs | SUBAQ | SPECIAL | bc |

- | | 0| 0| dest | 00000 | | 01001 | 1111 | 00 |


Format: SUBAQ.dest ACC,fs,Q


Purpose:


To substract the Q register from up to 4 matching fields to the accumulator


Description: if dest & .x == true : ACC.x <- fs.x - Q
if dest & .y == true : ACC.y <- fs.y - Q
if dest & .z == true : ACC.z <- fs.z - Q
if dest & .w == true : ACC.w <- fs.w - Q


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Substract accumulator broadcast SUBAx SUBAy SUBAz SUBAw


_____________________________________________________________________________________________
|31|30|29|28|27|26|25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | I| E| M| D| T| 0| 0| x| y| z| w| ft | fs | SUBAbc | SPECIAL | bc |

- | | 0| 0| dest | | | 00001 | 1111 | |


Format: SUBAx.dest ACC,fs,ft
SUBAy.dest ACC,fs,ft
SUBAz.dest ACC,fs,ft
SUBAw.dest ACC,ft,fs


Purpose:


To substract up to matching 4 fields to the accumulator


Description: if dest & .x == true : ACC.x <- fs.x - ft.bc
if dest & .y == true : ACC.y <- fs.y - ft.bc
if dest & .z == true : ACC.z <- fs.z - ft.bc
if dest & .w == true : ACC.w <- fs.w - ft.bc


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




## LOWER VU1 INSTRUCTION ENCODING


### LOWER VU1 INSTRUCTION LAYOUT


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | OPCODE | x| y| z| w| rt | rs | rd | SPECIAL | bc |

- | | t.e | s.e | | | | | |


ft.e and fs.e : 00 x
01 y
11 w




#### Branch B


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 00|


- | B | x| y| z| w| 0 | 0 | offset |

- | 0100000 | 0| 0| 0| 0| 00000 | 00000 | |

- Format: B offset


Purpose:


To unconditionally branch to a VU instruction address


Description: branch -> PC + offset + 8


Restrictions:


None


Operation:


Exceptions:


Programming notes:


The offset refers to doublewords in the VU instruction memory.
This instruction is applicable to both VU0 and VU1




#### Branch and link BAL


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 00|


- | BAL | x| y| z| w| It | 0 | offset |

- | 0100001 | 0| 0| 0| 0| | 00000 | |

- Format: BAL It, offset


Purpose:


To unconditionally branch to a VU instruction address and link to an IR


Description: branch -> PC + offset + 8
It -> PC + 16


Restrictions:


None


Operation:


Exceptions:


Programming notes:


The offset refers to doublewords in the VU instruction memory.
This instruction is applicable to both VU0 and VU1




#### Divide DIV


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | SPECIAL | x| y| z| w| ft | fs | DIV | SPECIAL | bc |

- | 1000000 | t.e | s.e | | | 01110 | 1111 | 00 |


Format: DIV Q,ft.e,fs.e


Purpose:


To divide a field of a VU FPR with a field of a VU FPR and store the result in the Q
register


Description: Q <- ft.e/fs.e


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Archtangent EATAN


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | SPECIAL | x| y| z| w| 0 | fs | EATAN | SPECIAL | bc |

- | 1000000 | 0| 0| s.e | 00000 | | 11111 | 1111 | 01 |

- Format: EATAN P, fs.e


Purpose:


To calculate the archtangent of a single field in a VU FPR and store it in the P register


Description: P <- arch(fs.e)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable only to VU1




#### Archtangent xy EATANxy


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | SPECIAL | x| y| z| w| 0 | fs | EATAN | SPECIAL | bc |

- | 1000000 | 1| 1| 0| 0| 00000 | | 11101 | 1111 | 00 |

- Format: EATANxy P, fs


Purpose:


To calculate the archtangent of two fields in a VU FPR and store it in the P register


Description: P <- arch(fs.x, fs.y)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable only to VU1




#### Archtangent xz EATANxz


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | SPECIAL | x| y| z| w| 0 | fs | EATAN | SPECIAL | bc |

- | 1000000 | 1| 0| 1| 0| 00000 | | 11101 | 1111 | 01 |

- Format: EATANxy P, fs


Purpose:


To calculate the archtangent of two fields in a VU FPR and store it in the P register


Description: P <- arch(fs.x, fs.z)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable only to VU1




#### Exponent EEXP


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | SPECIAL | x| y| z| w| 0 | fs | EEXP | SPECIAL | bc |

- | 1000000 | 0| 0| s.e | 00000 | | 11111 | 1111 | 10 |

- Format: EEXP P, fs.e


Purpose:


To calculate the exponent of a single field of a VU FPR and store it in the P register


Description: P <- exp(fs.e)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable only to VU1




#### Length ELENG


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | SPECIAL | x| y| z| w| 0 | fs | ELENG | SPECIAL | bc |

- | 1000000 | 1| 1| 1| 0| 00000 | | 11100 | 1111 | 10 |

- Format: ELENG P, fs


Purpose:


To calculate the length for the values in a VU FPR


Description: P <- len(fs)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable only to VU1




#### Reciprocal root ERCPR


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | SPECIAL | x| y| z| w| 0 | fs | ERCPR | SPECIAL | bc |

- | 1000000 | 0| 0| s.e | 00000 | | 11110 | 1111 | 10 |

- Format: ERCPR P, fs.e


Purpose:


To calculate the reciprocal root for a single field of a VU FPR and store it in the P
register


Description: P <- rec.root(fs.e)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable only to VU1




#### Reverse length ERLENG


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | SPECIAL | x| y| z| w| 0 | fs | ERLENG | SPECIAL | bc |

- | 1000000 | 1| 1| 1| 0| 00000 | | 11100 | 1111 | 11 |


Format: ERLENG P, fs.xyz


Purpose:


To calculate the reverse length for the xyz fields of a VU FPR and store it in the P
register


Description: P <- rev.len(fs.xyz)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable only to VU1




##### Reverse square root add ERSADD


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | SPECIAL | x| y| z| w| 0 | fs | ERSADD | SPECIAL | bc |

- | 1000000 | 1| 1| 1| 0| 00000 | | 11100 | 1111 | 01 |

- Format: ERSADD P,fs


Purpose:


To calculate the reverse square root of the addition of the xyz fields of a VU FPR and
store it in the P register


Description: P <- rev.sq.root(fs.x + fs.y + fs.z)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable only to VU1




##### Square root ESQRT


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | SPECIAL | x| y| z| w| 0 | fs | ESQRT | SPECIAL | bc |

- | 1000000 | 0 0| s.e | 00000 | | 11110 | 1111 | 01 |

- Format: ESQRT P, fs.e


Purpose:


To calculate the square root of a single field of a VU FPR and store it in the P register


Description: P <- sq.root(fs.e)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable only to VU1




##### Square root add ESADD


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | SPECIAL | x| y| z| w| 0 | fs | ESADD | SPECIAL | bc |

- | 1000000 | 1| 1| 1| 0| 00000 | | 11100 | 1111 | 00 |


Format: ESADD P, fs.xyz


Purpose:


To calculate the square root of the sum of the xyz fields of a VU FPR and store it in
the P register


Description: P <- sq.root(fs.x + fs.y + fs.z)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable only to VU1




#### Sin SIN


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | SPECIAL | x| y| z| w| 0 | fs | ESIN | SPECIAL | bc |

- | 1000000 | 0| 0| s.e | 00000 | | 11111 | 1111 | 00 |

- Format: ESIN P, fs.e


Purpose:


To calculate the sin of a single field of VU FPR and store it in the P register


Description: P <- sin(fs.e)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable only to VU1




##### Square root ESQRT


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | SPECIAL | x| y| z| w| 0 | fs | ESQRT | SPECIAL | bc |

- | 1000000 | 0| 0| s.e | 00000 | | 11110 | 1111 | 00 |

- Format: ESQRT P, fs.e


Purpose:


To calculate the square root of a single field of VU FPR and store it in the P register


Description: P <- sq.root(fs.e)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable only to VU1




##### Sum ESUM


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 02|01|00|


- | SPECIAL | x| y| z| w| 0 | fs | ESUM | SPECIAL | bc |

- | 1000000 | 1| 1| 1| 1| 00000 | | 11101 | 1111 | 10 |

- Format: ESUM P,fs


Purpose:


To calculate the sum of all the fields of a VU FPR and store it in the P register


Description: P <- fs.x + fs.y + fs.z + fs.w


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable only to VU1




#### Clipping AND FCAND


_____________________________________________________________________________________________
|31 25|24|23 00|


- | FCAND | x| immediate |

- | 0010010 | 0| |


Format: FCAND VI01,imm24


Purpose:


To perform a logical AND between the VI01 register and the 24 bit immediate and update
the clipping flag


Description: clipping flag(VI01 AND imm24)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Clipping equality


### FCEQ


_____________________________________________________________________________________________
|31 25|24|23 00|


- | FCEQ | x| immediate |

- | 0010000 | 0| |


Format: FCEQ VI01,imm24


Purpose:


To perform an equality check between the VI01 register and the 24 bit immediate and
only update the clipping flag on equality


Description: clipping flag(VI01 EQ imm24)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### FCGET


### Clipping transfer


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 00|


- | FCGET | x| y| z| w| It | 0 | 0 |

- | 0011100 | 0| 0| 0| 0| | 00000 | 00000000000 |


Format: FCGET It


Purpose:


To move the clipping flag to a VU IR


Description: It <- clipping flag


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Clipping OR


### FCOR


_____________________________________________________________________________________________
|31 25|24|23 00|


- | FCOR | x| immediate |

- | 0010011 | 0| |


Format: FCOR VI01,imm24


Purpose:


To perform an OR between the VI01 register and the 24 bit immediate and update the
clipping flag


Description: clipping flag(VI01 OR imm24)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Clipping set


### FCSET


_____________________________________________________________________________________________
|31 25|24|23 00|


- | FCSET | x| immediate |

- | 0010001 | 0| |


Format: FCSET imm24


Purpose:


To set the clipping flag to the value of the 24 bit immediate


Description: clipping flag = imm24


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### FMAND


### MAC AND


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 00|


- | FMAND | x| y| z| w| It | Is | 0 |

- | 0011010 | 0| 0| 0| 0| | | 00000000000 |


Format: FMAND It,Is


Purpose:


To AND the values in two VU IRs and move the result to the MAC flag


Description: MAC <- It AND Is


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### FMEQ


### MAC equality


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 00|


- | FMEQ | x| y| z| w| It | Is | 0 |

- | 0011000 | 0| 0| 0| 0| | | 00000000000 |


Format: FMEQ It,Is


Purpose:


To compare the values in two VU IRs and only move the result to the MAC flag on
eqauality


Description: MAC eq(It,Is)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### FMOR


### MAC OR


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 00|


- | FMOR | x| y| z| w| It | Is | 0 |

- | 0011011 | 0| 0| 0| 0| | | 00000000000 |


Format: FMOR It,Is


Purpose:


To OR the values two VU IRs and move the result to the MAC flag


Description: MAC == It OR Is


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Status AND FSAND


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 00|


- | FSAND | x| y| z| w| It | 0 | immediate |

- | 0010110 | 0| 0| 0| *| | 00000 | 12th bit is in dest.w |

- Format: FSAND It,imm12


Purpose:


To AND the value in a VU IR with a 12 bit immediate and move the result to the status
flag


Description: STATUS == It OR imm12


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




##### FSEQ


### Status equality


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 00|


- | FSEQ | x| y| z| w| It | 0 | immediate |

- | 0010100 | 0| 0| 0| *| | 00000 | 12th bit is in dest.w |


Format: FSEQ It, imm12


Purpose:


To compare the value in a VU IR with a 12 bit immediate and only move the result to the
status flag on equality


Description: STATUS eq(It, imm12)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### FSOR


### Status OR


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 00|


- | FSOR | x| y| z| w| It | 0 | immediate |

- | 0010111 | 0| 0| 0| *| | 00000 | 12th bit is in dest.w |


Format: FSOR It, imm12


Purpose:


To OR the value in a VU IR with a 12 bit immediate and move the result to the status
flag


Description: STATUS == It OR imm12


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Status set FSSET


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 00|


- | FSSET | x| y| z| w| 0 | 0 | immediate |

- | 0010101 | 0| 0| 0| *| 00000 | 00000 | 12th bit is in dest.w |

- Format: FSSET imm12


Purpose:


To set the status flag to the value of the 12bit immediate


Description: STATUS == imm12


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




##### Integer add


### IADD


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 00|


- | SPECIAL | x| y| z| w| It | Is | Id | IADD |

- | 1000000 | 0| 0| 0| 0| | | | 110000 |


Format: IADD Id, Is, It


Purpose:


To add two VU IRs and store the result in a VU IR


Description: Id = Is + It


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Integer add immediate


### IADDI


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 00|


- | SPECIAL | x| y| z| w| It | Is | imm | IADDI |

- | 1000000 | 0| 0| 0| 0| | | | 110010 |


Format: IADDI It, Is, imm5


Purpose:


To add a VU IRs with a 5 bit immediate and store the result in a VU IR


Description: It = Is + imm5


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Integer add immediate unsigned


### IADDIU


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 00|


- | IADDIU | x| y| z| w| It | Is | immediate |

- | 0001000 | * * * *| | | the upper 4 bits are in dest |


Format: IADDIU It, Is, imm12


Purpose:


To add a VU IRs with an unsigned 12 bit immediate and store the result in a VU IR


Description: It = Is + imm12


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Integer AND


### IAND


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 00|


- | SPECIAL | x| y| z| w| It | Is | Id | IADD |

- | 1000000 | 0| 0| 0| 0| | | | 110100 |


Format: IAND Id, Is, It


Purpose:


To AND two VU IRs and store the result in a VU IR


Description: Id = Is AND It


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Integer branch on equal


### IBEQ


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 00|


- | IBEQ | x| y| z| w| It | Is | offset |

- | 0101000 | 0| 0| 0| 0| | | |


Format: IBEQ It, Is, offset


Purpose:


To branch to a VU instruction memory offset if two VU IRs are equal


Description: PC <- PC + offset + 8


Restrictions:


None


Operation:


Exceptions:


Programming notes:


The offset refers to doublewords in the VU instruction memory.
This instruction is applicable to both VU0 and VU1




#### Integer branch on greater or equal to zero


### IBGEZ


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 00|


- | IBGEZ | x| y| z| w| 0 | Is | offset |

- | 0101111 | 0| 0| 0| 0| 00000 | | |


Format: IBGEZ Is, offset


Purpose:


To branch to a VU instruction memory offset if a VU IR is greater or equal to zero


Description: if Is >=0
PC <- PC + offset + 8


Restrictions:


None


Operation:


Exceptions:


Programming notes:


The offset refers to doublewords in the VU instruction memory.
This instruction is applicable to both VU0 and VU1




#### Integer branch on greater than zero


### IBGTZ


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 00|


- | IBGTZ | x| y| z| w| 0 | Is | offset |

- | 0101101 | 0| 0| 0| 0| 00000 | | |


Format: IBGEZ Is, offset


Purpose:


To branch to a VU instruction memory offset if a VU IR is greater than zero


Description: if Is >0
PC <- PC + offset + 8


Restrictions:


None


Operation:


Exceptions:


Programming notes:


The offset refers to doublewords in the VU instruction memory.
This instruction is applicable to both VU0 and VU1




#### Integer branch on less or equal to zero


### IBLEZ


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 00|


- | IBLEZ | x| y| z| w| 0 | Is | offset |

- | 0101110 | 0| 0| 0| 0| 00000 | | |


Format: IBLEZ Is, offset


Purpose:


To branch to a VU instruction memory offset if a VU IR is less or equal to zero


Description: if Is <=0
PC <- PC + offset + 8


Restrictions:


None


Operation:


Exceptions:


Programming notes:


The offset refers to doublewords in the VU instruction memory.
This instruction is applicable to both VU0 and VU1




#### Integer branch on not equal


### IBNE


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 00|


- | IBNE | x| y| z| w| It | Is | offset |

- | 0101001 | 0| 0| 0| 0| | | |


Format: IBNE It, Is, offset


Purpose:


To branch to a VU instruction memory offset if two VU IR are not equal


Description: if Is =| It
PC <- PC + offset + 8


Restrictions:


None


Operation:


Exceptions:


Programming notes:


The offset refers to doublewords in the VU instruction memory.
This instruction is applicable to both VU0 and VU1




#### Integer load word


### ILW


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 00|


- | ILW | x| y| z| w| ft | base | offset |

- | 0000100 | dest | | | |


Format: ILW.dest ft, offset(base)


Purpose:


To load up to 4 fields of a VU FPR with a word from the VU data cache


Description: fs.dest <- offset(base)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


The offset refers to doublewords in the VU data memory.
This instruction is applicable to both VU0 and VU1




#### Integer load word register


### ILWR


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 00|


- | SPECIAL | x| y| z| w| ft | base | ILWR | SPECIAL |

- | 1000000 | dest | | | 01111 | 111110 |


Format: ILWR.dest ft, base


Purpose:


To load up to 4 fields of a VU FPR with a word from the VU data cache


Description: fs.dest <- [base]


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Integer OR


### IOR


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 00|


- | SPECIAL | x| y| z| w| It | Is | Id | IOR |

- | 1000000 | 0| 0| 0| 0| | | | 110011 |


Format: IOR Id, Is, It


Purpose:


To OR two VU IRs and store the result in a VU IR


Description: Id = Is OR It


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Integer substract


### ISUB


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 00|


- | SPECIAL | x| y| z| w| It | Is | Id | ISUB |

- | 1000000 | 0| 0| 0| 0| | | | 110001 |


Format: ISUB Id, Is, It


Purpose:


To substract two VU IRs and store the result in a VU IR


Description: Id = Is - It


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Integer substract immediate unsigned


### ISUBIU


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 00|


- | ISUBIU | x| y| z| w| It | Is | immediate |

- | 0001001 | * * * *| | | the upper 4 bits are in dest |


Format: IADDIU It, Is, imm12


Purpose:


To substract an unsigned 12 bit immediate from a VU IR and store the result in a VU IR


Description: It = Is + imm12


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Integer store word


### ISW


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 00|


- | ISW | x| y| z| w| ft | base | offset |

- | 0000101 | dest | | | |


Format: ISW.dest ft, offset(base)


Purpose:


To store up to 4 fields of a VU FPR in the VU data cache


Description: offset(base) <- fs.dest


Restrictions:


None


Operation:


Exceptions:


Programming notes:


The offset refers to doublewords in the VU data memory.
This instruction is applicable to both VU0 and VU1




#### Integer store word register


### ISWR


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 00|


- | SPECIAL | x| y| z| w| ft | base | ISWR | SPECIAL |

- | 1000000 | dest | | | 01111 | 111111 |


Format: ISWR.dest ft, base


Purpose:


To store up to 4 fields of a VU FPR in the VU data cache


Description: [base] <- fs.dest


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Jump and link register


### JALR


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 00|


- | JALR | x| y| z| w| It | Is | 0 |

- | 0100101 | 0| 0| 0| 0| | | 00000000000 |


Format: JALR It, Is


Purpose:


To branch to the VU instruction memory offset of a VU IR and link to a VU IR


Description: PC <- It
Is <- PC + 8


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Jump register


### JR


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 00|


- | JR | x| y| z| w| 0 | Is | 0 |

- | 0100100 | 0| 0| 0| 0| 00000 | | 00000000000 |


Format: JALR Is


Purpose:


To branch to the VU instruction memory offset of a VU IR


Description: PC <- Is


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Load quadraword


### LQ


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 00|


- | LQ | x| y| z| w| ft | base | offset |

- | 0000000 | dest | | | |


Format: LQ.dest ft, offset(base)


Purpose:


To load up to 4 fields of a VU FPR with adjacent words in the VU data cache


Description: fs.dest <- offset(base)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


The offset refers to doublewords in the VU data memory.
This instruction is applicable to both VU0 and VU1




#### Load quadraword pre decrement


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 00|


- | SPECIAL | x| y| z| w| ft | base | LQD | SPECIAL |

- | 1000000 | dest | | | 01101 | 111110 |


Format: LQD.dest ft, --base


Purpose:


To load up to 4 fields of a VU FPR with VU data pre decrement


Description: base == base - 8
ft.dest <- [base]


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1


### LQD




#### Load quadraword post increment


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 00|


- | SPECIAL | x| y| z| w| ft | base | LQI | SPECIAL |

- | 1000000 | dest | | | 01101 | 111100 |


Format: LQI.dest ft, base++


Purpose:


To load up to 4 fields of a VU FPR with VU data post increment


Description: ft.dest <- [base]
base == base + 8


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1


### LQI




#### Move from integer register


### MFIR


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 00|


- | SPECIAL | x| y| z| w| ft | Is | MFIR | SPECIAL |

- | 1000000 | dest | | | 01111 | 111101 |


Format: MFIR.dest ft, Is


Purpose:


To load up to 4 fields of a VU FPR with the value of an IR


Description: ft.dest <- Is


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Move floating point registers


### MOVE


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 00|


- | SPECIAL | x| y| z| w| ft | fs | MOVE | SPECIAL |

- | 1000000 | dest | | | 01100 | 111100 |


Format: MOVE.dest ft, fs


Purpose:


To move up to 4 fields of a VU FPR to the corresponding fields of a VU FPR


Description: if dest & .x == true : ft.x <- fs.x
if dest & .y == true : ft.y <- fs.y
if dest & .z == true : ft.z <- fs.z
if dest & .w == true : ft.w <- fs.w


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Move and rotate per word


### MR32


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 00|


- | SPECIAL | x| y| z| w| ft | fs | MR32 | SPECIAL |

- | 1000000 | dest | | | 01100 | 111101 |


Format: MOVE.dest ft, fs


Purpose:


To move up to 4 fields of a VU FPR to rotated fields of a VU FPR


Description: if dest & .x == true : fs.x -> ft.y
if dest & .y == true : fs.y -> ft.z
if dest & .z == true : fs.z -> fs.w
if dest & .w == true : fs.w -> fs.x


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Move to integer register


### MTIR


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 00|


- | SPECIAL | x| y| z| w| It | fs | MTIR | SPECIAL |

- | 1000000 | 0| 0| fs.e| | | 01111 | 111100 |


Format: MTIR.e It, fs.e


Purpose:


To move a field of a VU FPR to an IR


Description: It <- fs.dest


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### RGET


### R move


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 00|


- | SPECIAL | x| y| z| w| ft | 0 | RGET | SPECIAL |

- | 1000000 | dest | | 00000 | 10000 | 111101 |


Format: RGET.dest ft, R


Purpose:


To load up to 4 fields of a VU FPR with the R register


Description: fs.dest <- R


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### RINIT


### R initialise


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 00|


- | SPECIAL | x| y| z| w| 0 | fs | RINIT | SPECIAL |

- | 1000000 | 0| 0| fs.e| 00000 | | 10000 | 111110 |


Format: RINIT.e R, fs.e


Purpose:


To initialise the random generator using a field of a VU FPR as matrix


Description: R <- Random(fs.e)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### RNEXT


### R next


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 00|


- | SPECIAL | x| y| z| w| ft | 0 | RNEXT | SPECIAL |

- | 1000000 | dest | | 00000 | 10000 | 111100 |


Format: RNEXT.dest ft, R


Purpose:


To load up to 4 fields of a VU FPR with the next random number from the R register


Description: ft.dest <- R


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Reverse square root


### RSQRT


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 00|


- | SPECIAL | x| y| z| w| ft | fs | RSQRT | SPECIAL |

- | 1000000 | ft.e| fs.e| | | 01110 | 111110 |


Format: RSQRT Q, ft.e, fs.e


Purpose:


To calculate ft.e/sqrt(fs.e)


Description: Q <- ft.e/sqrt(fs.e)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### RXOR


### R XOR


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 00|


- | SPECIAL | x| y| z| w| 0 | fs | RXOR | SPECIAL |

- | 1000000 | 0| 0| fs.e| 00000 | | 10000 | 111111 |


Format: RXOR.e R, fs.e


Purpose:


To XOR the value in the R register and a field of a VU FPR and store the result in the
R register


Description: R <- R XOR fs.e


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Store quadraword


### SQ


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 00|


- | SQ | x| y| z| w| ft | base | offset |

- | 0000001 | dest | | | |


Format: SQ.dest ft, offset(base)


Purpose:


To store up to 4 fields of a VU FPR to adjacent words in the VU data cache


Description: offset(base) <- fs.dest


Restrictions:


None


Operation:


Exceptions:


Programming notes:


The offset refers to doublewords in the VU data memory.
This instruction is applicable to both VU0 and VU1




#### Store quadraword pre decrement


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 00|


- | SPECIAL | x| y| z| w| base | ft | SQD | SPECIAL |

- | 1000000 | dest | | | 01101 | 111111 |


Format: SQD.dest ft, --base


Purpose:


To store up to 4 fields of a VU FPR to the VU data cache pre decrement


Description: base == base - 8
[base] <- ft.dest


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1


### SQD




#### Store quadraword post increment


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 00|


- | SPECIAL | x| y| z| w| base | ft | SQI | SPECIAL |

- | 1000000 | dest | | | 01101 | 111101 |


Format: SQI.dest ft, base++


Purpose:


To store up to 4 fields of a VU FPR to the VU data cache post increment


Description: [base] <- ft.dest
base == base + 8


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1


### SQI




#### Square root


### SQRT


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 00|


- | SPECIAL | x| y| z| w| ft | 0 | SQRT | SPECIAL |

- | 1000000 | ft.e| 0| 0| | 00000 | 01110 | 111101 |


Format: SQRT Q, ft.e


Purpose:


To calculate the square root of the value in a single field of a VU FPR


Description: Q <- sqrt(ft.e)


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Wait P operation


### WAITP


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 00|


- | SPECIAL | x| y| z| w| 0 | 0 | WAITP | SPECIAL |

- | 1000000 | 0| 0| 0| 0| 00000 | 00000 | 11110 | 111111 |


Format: WAITP


Purpose:


To postpone VU operation until the operation of the instruction that writes to the P
register is concluded


Description:


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Wait Q operation


### WAITQ


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 00|


- | SPECIAL | x| y| z| w| 0 | 0 | WAITQ | SPECIAL |

- | 1000000 | 0| 0| 0| 0| 00000 | 00000 | 01110 | 111111 |


Format: WAITQ


Purpose:


To postpone VU operation untill the operation of the instruction that writes to the Q
register is concluded


Description:


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is applicable to both VU0 and VU1




#### Initialise GIF path


### XGKICK


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 00|


- | SPECIAL | x| y| z| w| 0 | Is | XGKICK | SPECIAL |

- | 1000000 | 0| 0| 0| 0| 00000 | | 11011 | 111100 |


Format: XGKICK Is


Purpose:


To initialise the VU1 path to the GIF


Description:


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is only applicable to VU1




#### GIF path handling


### XITOP


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 00|


- | SPECIAL | x| y| z| w| It | 0 | XITOP | SPECIAL |

- | 1000000 | 0| 0| 0| 0| | 00000 | 11010 | 111101 |


Format: XITOP It


Purpose:


To control the VU1 path to the GIF


Description:


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is only applicable to VU1




#### GIF path handling


### XTOP


_____________________________________________________________________________________________
|31 25|24|23|22|21|20 16|15 11|10 06|05 00|


- | SPECIAL | x| y| z| w| It | 0 | XTOP | SPECIAL |

- | 1000000 | 0| 0| 0| 0| | 00000 | 11010 | 111100 |


Format: XTOP It


Purpose:


To control the VU1 path to the GIF


Description:


Restrictions:


None


Operation:


Exceptions:


Programming notes:


This instruction is only applicable to VU1




VU Vector Unit
VU0 Vector Unit 0
VU1 Vector Unit 1
FPR Floating point register
IR Integer register
dest destination fields
fs floating point source register
ft floating point target register
fd floating point destination register
Is integer source register
It integer target register
Id integer destination register
imm immediate
bc broadcast field
.e field selector


### LEGEND


