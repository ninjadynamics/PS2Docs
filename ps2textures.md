# TM2 — The PlayStation 2 Texture Format

> **Source & attribution.** This document is a re-edited, Markdown version of the
> TM2 reference published by the **OpenKh** project at
> <https://openkh.dev/common/tm2.html>. The structural facts, field tables, and
> register layouts are theirs; the prose framing here is rewritten for this
> collection. Credit for the original reverse-engineering goes to the OpenKh
> contributors — please consult the upstream page for the authoritative version.

## What TM2 is, and why it's in this collection

TM2 (the file magic is the ASCII string `TIM2`) is the texture container used by
a great many PlayStation 2 titles — not only the Kingdom Hearts games OpenKh
documents it for. If you are reading the rest of this collection's GS material,
TM2 is worth knowing because it is, in effect, **a serialized snapshot of the GS
texture registers**: a TM2 picture stores not just pixels and an optional palette,
but the `GsTex` register value that tells the hardware where the texture and its
CLUT live in GS local memory, how the pixels are packed (`PSM`), and how the
texture function blends. Reading TM2 is therefore a fast, concrete way to learn
the GS texture model — the same `PSM`, `CPSM`, `CSM`, and `TFX` fields that the GS
User's Manual describes in the abstract appear here as plain bytes in a file.

A TM2 file is a flat container of one or more **pictures**. Each picture may
optionally carry a Color Look-Up Table (CLUT/palette) and/or multiple mipmap
levels. (The mipmap sub-structure is only partially understood upstream; that gap
is preserved below rather than guessed at.)

## File structure

### File header

A TM2 file opens with a small fixed header, immediately followed by the first
picture. To reach picture *N*, walk the picture list using each picture's
`TotalSize` field (see [Picture](#picture)).

| Offset | Type       | Description                                  |
|-------:|------------|----------------------------------------------|
| `0x00` | `int`      | Magic code, always `TIM2`                     |
| `0x04` | `byte`     | File format revision (usually `4`)            |
| `0x05` | `byte`     | Format (usually `0`)                          |
| `0x06` | `short`    | Picture count                                 |
| `0x08` | `int`      | Reserved                                      |
| `0x0C` | `int`      | Reserved                                      |
| `0x10` | `Picture*` | List of pictures (first picture starts here)  |

To pick a specific picture, step forward from the first picture header by each
header's `TotalSize`:

```c
Picture* GetPicture(Picture* firstPicture, int index) {
    off_t offset = (off_t)firstPicture;
    while (index-- > 0)
        offset += ((Picture*)offset)->TotalSize;
    return (Picture*)offset;
}
```

### Picture

Each picture begins with the header below. If `Mipmap count` is greater than 1, a
[Mipmap](#mipmap) structure follows the header. After the (optional) mipmap block
comes the bitmap (`Image size` bytes), and finally the optional palette
(`Clut size` bytes).

| Offset | Type    | Description                                       |
|-------:|---------|---------------------------------------------------|
| `0x00` | `int`   | Total size in bytes used by the picture           |
| `0x04` | `int`   | CLUT size in bytes used by the palette            |
| `0x08` | `int`   | Image size in bytes used by the bitmap            |
| `0x0C` | `short` | Header size                                       |
| `0x0E` | `short` | Number of colors used by the CLUT                 |
| `0x10` | `byte`  | Picture format                                    |
| `0x11` | `byte`  | Mipmap count                                      |
| `0x12` | `byte`  | CLUT color type (see [Color type](#color-type))   |
| `0x13` | `byte`  | Image color type (see [Color type](#color-type))  |
| `0x14` | `short` | Image width in pixels                             |
| `0x16` | `short` | Image height in pixels                            |
| `0x18` | `long`  | `GsTex` register (see [GsTex](#gstex))            |
| `0x20` | `long`  | `GsTex` register (second word; see [GsTex](#gstex)) |
| `0x28` | `int`   | GS flags register (`GsReg`)                       |
| `0x2C` | `int`   | GS CLUT register (`GsClut`)                       |

### Mipmap

Present only when `Mipmap count > 1`. The exact purpose of these fields is not yet
documented upstream.

| Offset  | Type     | Description           |
|--------:|----------|-----------------------|
| `0x00`  | `int`    | `Miptbp` register     |
| `0x04`  | `int`    | `Miptbp` register     |
| `0x08`  | `int`    | `Miptbp` register     |
| `0x0C`  | `int`    | `Miptbp` register     |
| `0x10`  | `int[8]` | Array of sizes        |

## Registers

### GsTex

The 64-bit `GsTex` value packs the GS texture-binding state. Bit offsets and
widths:

| Bit  | Count | Name   | Description                                                                 |
|-----:|------:|--------|-----------------------------------------------------------------------------|
| `0`  | 14    | `TBP0` | Texture buffer location. Multiply by `0x100` for the raw VRAM pointer.       |
| `14` | 6     | `TBW`  | Texture buffer width.                                                        |
| `20` | 6     | `PSM`  | Pixel storage format (see [PSM](#psm--pixel-storage-mode)).                  |
| `26` | 4     | `TW`   | `log2(texture width)`.                                                       |
| `30` | 4     | `TH`   | `log2(texture height)`.                                                      |
| `34` | 1     | `TCC`  | `1` if the texture or the CLUT contains an alpha channel.                    |
| `35` | 2     | `TFX`  | Texture function (see [TFX](#tfx--texture-function)).                        |
| `37` | 14    | `CBP`  | CLUT buffer location. Multiply by `0x100` for the raw VRAM pointer.          |
| `51` | 4     | `CPSM` | CLUT storage format (see [CPSM](#cpsm--clut-pixel-storage-mode)).            |
| `55` | 1     | `CSM`  | CLUT storage mode (see [CSM](#csm--clut-storage-mode)).                      |
| `56` | 5     | `CSA`  | CLUT entry offset. Mostly used by 4-bit images.                             |
| `61` | 3     | `CLD`  | CLUT load control. Purpose unknown.                                         |

### GsReg

Undocumented upstream.

### GsClut

Undocumented upstream.

## Types

### PSM — Pixel Storage Mode

Defines how pixels are arranged within each 32-bit word of GS local memory.

| Value | Name       | Description                                                           |
|------:|------------|-----------------------------------------------------------------------|
| `0`   | `PSMCT32`  | RGBA32, 32 bits per pixel.                                             |
| `1`   | `PSMCT24`  | RGB24, 24 bits per pixel (upper 8 bits unused).                       |
| `2`   | `PSMCT16`  | RGBA16 unsigned; two pixels packed into 32 bits, little-endian.       |
| `10`  | `PSMCT16S` | RGBA16 signed; two pixels packed into 32 bits, little-endian.         |
| `19`  | `PSMT8`    | 8-bit indexed; 4 pixels per 32-bit word.                              |
| `20`  | `PSMT4`    | 4-bit indexed; 8 pixels per 32-bit word.                             |
| `27`  | `PSMT8H`   | 8-bit indexed; upper 24 bits unused.                                  |
| `26`  | `PSMT4HL`  | 4-bit indexed; upper 24 bits unused.                                  |
| `44`  | `PSMT4HH`  | 4-bit indexed; bits 4–7 evaluated, the rest discarded.               |
| `48`  | `PSMZ32`   | 32-bit Z buffer.                                                      |
| `49`  | `PSMZ24`   | 24-bit Z buffer (upper 8 bits unused).                                |
| `50`  | `PSMZ16`   | 16-bit unsigned Z buffer; two pixels per 32 bits, little-endian.      |
| `58`  | `PSMZ16S`  | 16-bit signed Z buffer; two pixels per 32 bits, little-endian.        |

### CPSM — CLUT Pixel Storage Mode

Storage format of the palette itself.

| Value | Name       | Description             |
|------:|------------|-------------------------|
| `0`   | `PSMCT32`  | 32-bit color palette.   |
| `1`   | `PSMCT24`  | 24-bit color palette.   |
| `2`   | `PSMCT16`  | 16-bit color palette.   |
| `10`  | `PSMCT16S` | 16-bit color palette.   |

### CSM — CLUT Storage Mode

| Mode   | Description                                                                                  |
|--------|----------------------------------------------------------------------------------------------|
| `CSM1` | Pixels are stored swizzled every `0x20` bytes. Faster for the PS2 to render.                  |
| `CSM2` | Pixels are stored sequentially. Simpler, but the PS2 GPU consumes the CLUT more slowly.       |

### Color type

The `Clut color type` / `Image color type` fields use these values:

| Value | Description              |
|------:|--------------------------|
| `0`   | Undefined                |
| `1`   | 16-bit RGBA (`A1B5G5R5`) |
| `2`   | 32-bit RGB (`X8B8G8R8`)  |
| `3`   | 32-bit RGBA (`A8B8G8R8`) |
| `4`   | 4-bit indexed            |
| `5`   | 8-bit indexed            |

### TFX — Texture Function

| Value | Description |
|------:|-------------|
| `0`   | Modulate    |
| `1`   | Decal       |
| `2`   | Hilight     |
| `3`   | Hilight 2   |

---

*Reproduced and reformatted from the OpenKh wiki entry
[TM2 (PlayStation 2 texture format)](https://openkh.dev/common/tm2.html). For
corrections or newly-documented fields, refer to the upstream source.*
