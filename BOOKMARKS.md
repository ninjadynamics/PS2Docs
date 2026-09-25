# PS2 Hardware Manuals — Bookmarks

Official Sony PlayStation 2 developer manuals (SCE Confidential, leaked/archived). These are the PS2 analog to the Dreamcast Holly manual. For HyperSolar's PS2 work the **GS User's Manual** is the one you'll open most (the GS quirks in [`PS2/LESSONS.md`](../../../PS2/LESSONS.md) trace back to it); the **EE Overview** is the closest thing to a "system architecture" tour.

## What's here

| File | Covers | Most relevant for |
|---|---|---|
| `GS_Users_Manual.pdf` (177p) | **Graphics Synthesizer** — registers, texture formats (PSMCT32/16/8/4, CLUT), `TEXFLUSH`, blending/alpha, scissor, the GIF packet formats (PACKED/REGLIST/IMAGE), display environment | textures, blending, GS-OOM, the AR/display setup — the core graphics reference |
| `GS_Users_Manual_Supplement.pdf` (22p) | GS errata / additions to the above | corner cases |
| `EE_Overview_Manual.pdf` (65p) | **System architecture**: EE core + VU0/VU1 + GIF + VIF + DMAC + IPU and how they connect | the big picture; start here |
| `EE_Users_Manual.pdf` (219p) | EE peripherals — **DMAC, GIF, VIF, timers, INTC**, scratchpad | DMA paths, ps2gl/VIF interaction, GS-OOM |
| `EE_Core_Users_Manual.pdf` (181p) | The R5900 CPU core (pipeline, caches, MMU, COP0) | CPU-side perf |
| `EE_Core_Instruction_Set_Manual.pdf` (409p) | R5900 + MMI (multimedia) instruction set | hand-tuning / intrinsics |
| `VU_Users_Manual.pdf` (369p) | VU0/VU1 architecture (registers, pipelines, microMode). **Ch 3.3.5** (per-instruction flag table — ABS/MAX/MINI/FTOI/MOVE set NO flags) and **Ch 3.4** (hazards: Q/ACC/I have no interlocks; `waitq` stalls both pipes; same-pair semantics) arbitrated the VU1 clip renderer's vcl bugs | geometry / the clippers (EE software + VU1 microcode) |
| `vu-instruction-manual.pdf` (129p) | VU micro-instruction set (per-instruction latency/flag detail) | VU microcode |
| `SPU2_Overview_Manual.pdf` (80p) | **SPU2 sound processor** — 2 cores × 24 ADPCM voices @ 48 kHz, 2 MB local RAM; voice PITCH/VOLL/VOLR/ADSR/KON registers, the pitch math (`0x1000` = native @ 48 kHz, range −12..+2 oct), mixing, reverb | PS2 audio backend (`audio.c`/`playstation2.c` `ps2_audio_*`), see [`PS2/plans/done/AUDIO_PLAN.md`](../../../PS2/plans/done/AUDIO_PLAN.md) |

## Notes

- **GS User's Manual pp95/107, pp145–148; EE User's Manual p28/p87** —
  separate SIGNAL command acknowledgement, FINISH drawing completion,
  CSR event/field bits, DISPFB publication and vblank start/end. A pending
  VSYNC semaphore token does not prove that scanout is still blank. VIF FLUSH
  is not a GS raster-completion fence. These sections support the
  [presentation investigation](../../../todo/done/good-enough/PS2_SCREEN_TEARING_INVESTIGATION.md),
  where Bruno closed tearing on September 23 while stalls remain unresolved.
  The report notes the p95 flag-value typo and
  uses the CSR table/operational sequence for polarity.
- **GS User's Manual p53, pp113/127, pp162–171; GS Supplement section 1.4** —
  REGION_CLAMP shifts with mip level; MIPTBP block bases and TBW must match
  the uploaded PSMCT32 page/block geometry. The retained entrance shape uses
  one eight-page owner and level offsets 0/128/144/148 blocks. Source-byte
  count alone does not price stride holes, transfer-tail rows or allocator
  slots. See [the mip plan](../../../todo/done/good-enough/PS2_ENTRANCE_MIPS.md) for the
  admitted shape, address proof and lack of proven performance neutrality.
- **VU User's Manual instruction hazards and XGKICK ordering** — inspect
  generated pairs, relative branch reach, flag delays and output ownership.
  A warning-bearing successful assembler exit or a source-only fence label
  is insufficient. See [the generated-code lesson](../../../PS2/LESSONS.md#validate-generated-vu-branches-and-data-hazards-not-assembler-exit-status)
  and the per-module guards under `PS2/lib/ps2gl/vu1/`.
- **VU User's Manual, macro-mode interlocks and aligned transfers (pp210–214,
  236/239)** — use with the current `ps2s/vu0_math.h` ownership/alignment
  contract. Macro-mode helpers are separate from VU1 microprograms and from
  obsolete `NO_ASM`/`NO_VU0_VECTORS` class branches. The tested numerical
  corpus does not establish universal EE/VU identity or a speedup; the grouped
  eye-transform candidate passed its oracle but was rejected and removed in
  September 21 promotion. See [the math lesson](../../../PS2/LESSONS.md#ordered-ee-arithmetic-and-vu-batching-have-separate-correctness-and-cost-gates).
- **EE User's Manual, VIF/DMAC and GIF ordering** — read alongside actual
  `glFlush`, packet `Send` and frame completion paths. Geometry construction,
  source writeback, DMA submission, command acknowledgement and GS raster
  completion are separate events. The September 12 prepared HUD borrows
  frame-owned arrays after construction returns; see
  [the packet-lifetime lesson](../../../PS2/LESSONS.md#cached-and-borrowed-packets-need-separate-construction-and-dma-lifetimes)
  and [counter coverage](../../../PS2/LESSONS.md#compare-completed-work-and-counter-coverage-before-claiming-a-renderer-gain).
- **EE User's Manual, DMAC source-chain reference transfers** — use alongside
  the actual ps2gl `XferVectors`/`packet.Ref()` implementation when checking
  client-array lifetime. The September 8 exhaust candidate reset shared frame
  scratch and overwrote queued skybox data; only the frame owner may recycle
  it. This is a source/hardware finding, not a new manual claim. See
  [the buffer-lifetime lesson](../../../PS2/LESSONS.md#a-frame-scratch-cursor-belongs-to-the-frame-not-a-draw).
- **GS User's Manual Ch 3, Drawing Function (Z test / Z buffer)** — `ZTST`
  defines the tie rule. Use ALWAYS for truly depth-independent overlays; use
  LEQUAL with Z writes masked for intentional coplanar overlays; use LESS when
  an equal-depth fragment must preserve the earlier surface. Physical Y offsets
  are not a substitute for choosing the semantic tie rule.
- **GS User's Manual p139 (XYZF2), p60 (STQ); EE User's Manual p154
  (GIF packed extraction)** — XY has four fractional bits; packed XYZF2 drops
  the four low Z bits produced by FTOI4. LEQUAL resolves equal raster depth,
  not all independently triangulated coplanar surfaces. Read handbook
  **chapters 8 and 9 together** before changing decal depth: the confirmed
  entrance adapter combines original-plane slope, active GS units, clipping
  before division and explicit STQ, with read-only scene depth. The tolerance
  and its acceptance come from source/hardware evidence, not a guarantee in
  the manual. See [the decal lesson](../../../PS2/LESSONS.md#coplanar-decals-need-a-raster-depth-tolerance-not-just-a-shared-plane).
- **SPU2 Overview Manual, local RAM + voice transfer/loop sections** — chunk
  size, encoded-file stride, DMA length, half-ring size and SPU addresses form
  one contract. HyperSolar's current source uses 1024-block chunks (16 KiB
  halves; 32 KiB/channel rings at `0x1F0000`/`0x1F8000`). September 8 playback
  was confirmed after replacing old 2048-block USB music; stale files caused
  a 0.597-second stereo offset. This deployment finding comes from source and
  hardware evidence, not the manual. See `PS2/LESSONS.md`, "Deploy music assets
  with their streamer format", and handbook Chapter 28. Refill stress testing
  remains separate.
- [`COPETTI_PS2_ARCHITECTURE.md`](COPETTI_PS2_ARCHITECTURE.md) (next to this file) is Rodrigo Copetti's "PlayStation 2 Architecture — A Practical Analysis" (offline Markdown copy of <https://www.copetti.org/writings/consoles/playstation-2/>) — a readable end-to-end tour (EE/VU/GS/SPU2/IOP/OS/anti-piracy); friendlier than the EE Overview as a first read.
- `ps2textures.htm` (next to this file) is a community write-up on GS texture formats — quicker than the GS manual for the format table.
- The fuller set (MIPS calling conventions, third-party perf/normal-mapping write-ups) lives in the upstream repo `DarrenRainey/PS2-Programming-Docs` if needed.
