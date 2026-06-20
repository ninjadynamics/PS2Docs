# PS2Docs

A curated shelf of **PlayStation 2 development documentation** — the official Sony
hardware manuals, a quick-reference index into them, the TM2 texture-format spec,
and a field-tested handbook of lessons learned shipping a real PS2 renderer.

If you are doing homebrew, emulation, reverse-engineering, or porting work on the
PS2, this is meant to be the folder you keep open in the other tab.

---

## 📕 The handbook

**[The PS2 Programming Language — Lessons Learned (So Far)](LESSONS_LEARNED.md)**
&nbsp;·&nbsp; [📄 A4 PDF](LESSONS_LEARNED.pdf)

The PS2 was famously so quirky that the pile of workarounds you accumulate
practically becomes its own language. This is that pile, written down: 18 chapters
plus appendices covering the GS, EE, IOP, SPU2, ps2gl/ps2stuff, the EE software
clipper, video modes, texture formats, audio streaming, memory cards, and the
emulator-vs-hardware traps that eat days.

It is the *scar-tissue layer* on top of the Sony manuals — the rules that only
became obvious after ps2gl, the GS, SPU2, SIF RPC, PCSX2, ps2link, and real
hardware disagreed with one another. Each chapter gives you a rule you can apply,
the failure mode that makes you reach for it, and a pointer to the relevant Sony
manual chapter.

- **[Read it as Markdown →](LESSONS_LEARNED.md)** (best on GitHub)
- **[Download the PDF →](LESSONS_LEARNED.pdf)** (A4, clickable table of contents,
  each chapter on its own page)

---

## 🔖 References

| Document | What it is |
|---|---|
| **[BOOKMARKS.md](BOOKMARKS.md)** | An index into the Sony manuals below — which manual to open for which problem. |
| **[ps2textures.md](ps2textures.md)** | The **TM2 / `TIM2`** texture container format (header, picture, mipmap, the `GsTex` bitfield, and the `PSM`/`CPSM`/`CSM`/`TFX` enums). Reformatted from [OpenKh](https://openkh.dev/common/tm2.html). |

---

## 📚 Official Sony manuals

The PS2 technical reference manuals (SCE Confidential, archived). The **GS User's
Manual** is the one you'll open most; the **EE Overview** is the closest thing to
a "system architecture" tour — start there.

| Manual | Pages | Covers |
|---|---:|---|
| [GS_Users_Manual.pdf](GS_Users_Manual.pdf) | 177 | **Graphics Synthesizer** — registers, pixel-storage modes (PSMCT32/16/8/4, CLUT), `TEXFLUSH`, blending/alpha, scissor, GIF packet formats, display environment |
| [GS_Users_Manual_Supplement.pdf](GS_Users_Manual_Supplement.pdf) | 22 | GS errata / additions |
| [EE_Overview_Manual.pdf](EE_Overview_Manual.pdf) | 65 | **System architecture** — EE core + VU0/VU1 + GIF + VIF + DMAC + IPU and how they connect |
| [EE_Users_Manual.pdf](EE_Users_Manual.pdf) | 219 | EE peripherals — DMAC, GIF, VIF, timers, INTC, scratchpad |
| [EE_Core_Users_Manual.pdf](EE_Core_Users_Manual.pdf) | 181 | The R5900 CPU core (pipeline, caches, MMU, COP0) |
| [EE_Core_Instruction_Set_Manual.pdf](EE_Core_Instruction_Set_Manual.pdf) | 409 | R5900 + MMI (multimedia) instruction set |
| [VU_Users_Manual.pdf](VU_Users_Manual.pdf) | 369 | VU0/VU1 architecture (registers, pipelines, micro mode) |
| [vu-instruction-manual.pdf](vu-instruction-manual.pdf) | 129 | VU micro-instruction set |
| [SPU2_Overview_Manual.pdf](SPU2_Overview_Manual.pdf) | 80 | **SPU2 sound processor** — 2 cores × 24 ADPCM voices @ 48 kHz, 2 MB local RAM; PITCH/VOLL/VOLR/ADSR/KON, mixing, reverb |

---

## Credits & attribution

- The **TM2 format** documentation ([ps2textures.md](ps2textures.md)) is reproduced
  and reformatted from the **[OpenKh](https://openkh.dev/common/tm2.html)** project.
  The reverse-engineering is theirs; please cite and consult the upstream page for
  the authoritative version.
- The **Sony manuals** are archival copies of SCE developer documentation,
  collected here for study and preservation.
- The **handbook** distills lessons from real PS2 porting work by Ninja Dynamics.

Corrections and additions welcome.
