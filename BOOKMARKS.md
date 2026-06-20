# PS2 Hardware Manuals — Bookmarks

Official Sony PlayStation 2 developer manuals (SCE Confidential, leaked/archived). These are the PS2 analog to the Dreamcast Holly manual. The **GS User's Manual** is the one you'll open most (the GS quirks in [`LESSONS_LEARNED.md`](LESSONS_LEARNED.md) trace back to it); the **EE Overview** is the closest thing to a "system architecture" tour.

## What's here

| File | Covers | Most relevant for |
|---|---|---|
| `GS_Users_Manual.pdf` (177p) | **Graphics Synthesizer** — registers, texture formats (PSMCT32/16/8/4, CLUT), `TEXFLUSH`, blending/alpha, scissor, the GIF packet formats (PACKED/REGLIST/IMAGE), display environment | textures, blending, GS-OOM, the AR/display setup — the core graphics reference |
| `GS_Users_Manual_Supplement.pdf` (22p) | GS errata / additions to the above | corner cases |
| `EE_Overview_Manual.pdf` (65p) | **System architecture**: EE core + VU0/VU1 + GIF + VIF + DMAC + IPU and how they connect | the big picture; start here |
| `EE_Users_Manual.pdf` (219p) | EE peripherals — **DMAC, GIF, VIF, timers, INTC**, scratchpad | DMA paths, ps2gl/VIF interaction, GS-OOM |
| `EE_Core_Users_Manual.pdf` (181p) | The R5900 CPU core (pipeline, caches, MMU, COP0) | CPU-side perf |
| `EE_Core_Instruction_Set_Manual.pdf` (409p) | R5900 + MMI (multimedia) instruction set | hand-tuning / intrinsics |
| `VU_Users_Manual.pdf` (369p) | VU0/VU1 architecture (registers, pipelines, microMode) | geometry / the software clipper |
| `vu-instruction-manual.pdf` (129p) | VU micro-instruction set | VU microcode |
| `SPU2_Overview_Manual.pdf` (80p) | **SPU2 sound processor** — 2 cores × 24 ADPCM voices @ 48 kHz, 2 MB local RAM; voice PITCH/VOLL/VOLR/ADSR/KON registers, the pitch math (`0x1000` = native @ 48 kHz, range −12..+2 oct), mixing, reverb | PS2 audio backend / SPU2 voice control (see [`LESSONS_LEARNED.md`](LESSONS_LEARNED.md) chapters 15–16) |

## Notes

- [`ps2textures.md`](ps2textures.md) is a write-up of the TM2 PlayStation 2 texture format (reformatted from [OpenKh](https://openkh.dev/common/tm2.html)) — quicker than the GS manual for the format/CLUT tables.
- The fuller set (MIPS calling conventions, third-party perf/normal-mapping write-ups) lives in the upstream repo `DarrenRainey/PS2-Programming-Docs` if needed.
