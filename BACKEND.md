# PS2 Backend

This document covers the **PlayStation 2 rendering and audio backend** that HyperSolar
is built on: a set of locally maintained forks of the standard PS2 homebrew libraries
plus a custom IOP music-streaming module, all layered on top of an otherwise stock
`ps2sdk` install.

It has two purposes:

1. **Getting started** — how to fetch and build the forks and the `musicstream` IOP
   module locally, on top of an already-installed `ps2sdk`, so the project links the
   forks instead of the toolchain-installed libraries.
2. **A detailed delta report** — exactly what was changed in each fork and *why*. The
   audience for the report half is maintainers of the PS2 SDK ecosystem
   (`ps2gl` / `ps2stuff`); the changes are described in library terms, with
   game-specific detail included only where it explains an observed requirement.

## Repositories

The backend lives in three public forks / repos (all wired into this project as git
submodules under `PS2/lib/` — see `.gitmodules`):

| Component | Role | Upstream | Fork / repo |
| --- | --- | --- | --- |
| **ps2gl** | OpenGL-ish GS rendering layer | [`ps2dev/ps2gl`](https://github.com/ps2dev/ps2gl) | [`ninjadynamics/ps2gl`](https://github.com/ninjadynamics/ps2gl) |
| **ps2stuff** | Low-level GS / DMA / memory helpers (ps2gl depends on it) | [`ps2dev/ps2stuff`](https://github.com/ps2dev/ps2stuff) | [`ninjadynamics/ps2stuff`](https://github.com/ninjadynamics/ps2stuff) |
| **musicstream** | Custom IOP module: zero-EE-work SPU2 music streamer | — (new) | [`ninjadynamics/musicstream`](https://github.com/ninjadynamics/musicstream) |

The `ps2gl` and `ps2stuff` deltas are documented in **[Fork Delta Report](#fork-delta-report-ps2gl-and-ps2stuff)**;
the IOP streamer is documented in **[musicstream — IOP Music Streamer](#musicstream--iop-music-streamer)**.

## Getting Started

### Prerequisites

- A working **ps2dev / `ps2sdk`** install with the EE + IOP toolchains on `PATH`
  (`$PS2DEV/ee/bin`, `$PS2DEV/iop/bin`, `$PS2DEV/dvp/bin`, `$PS2SDK/bin`). The root
  `Makefile` exports `PS2DEV` / `PS2SDK` and prepends these for the PS2 targets.
- The forks may already be installed under `$PS2SDK/ports` (stock `-lps2gl` /
  `-lps2stuff`). That's fine — the build **prefers the local fork archives when they
  exist** and only falls back to the installed copies otherwise (see below).

### 1. Fetch the forks

They are submodules; a recursive clone (or update) pulls them:

```sh
git submodule update --init --recursive PS2/lib/ps2stuff PS2/lib/ps2gl PS2/lib/musicstream
```

### 2. Build `ps2stuff` (CMake, out-of-source)

`ps2stuff` is CMake-built; its archive must land at `PS2/lib/ps2stuff/build/libps2stuff.a`:

```sh
cd PS2/lib/ps2stuff
cmake -S . -B build
cmake --build build
```

### 3. Build `ps2gl` (plain make, in-tree)

`ps2gl` builds against the **sibling `ps2stuff` fork's headers** — its `Makefile`
puts `-I../ps2stuff/include` ahead of `$(PS2SDK)/ports/include` so it compiles
against the fork (e.g. `CMemManager::GetMemInfo`). The archive lands in-tree at
`PS2/lib/ps2gl/libps2gl.a`:

```sh
cd PS2/lib/ps2gl
make
```

### 4. How the project picks up the forks

The root `Makefile` auto-detects the fork archives and, when present, puts the fork
include dir ahead of `$(PS2SDK)/ports/include` and links the fork archive directly
instead of `-lps2gl` / `-lps2stuff`:

```make
ifneq ($(wildcard PS2/lib/ps2gl/libps2gl.a),)
  PS2_PS2GL_INC  := -IPS2/lib/ps2gl/include
  PS2_PS2GL_LINK := PS2/lib/ps2gl/libps2gl.a
else
  PS2_PS2GL_LINK := -lps2gl            # toolchain-installed fallback
endif
```

(`ps2stuff` is detected the same way via `PS2/lib/ps2stuff/build/libps2stuff.a`.) The
**build canaries** in each fork — a one-shot `[ CANARY ] ... MODIFIED LOCAL ...` print
from `GS::Init()` — exist precisely to confirm at runtime which library actually got
linked (see the report).

### 5. The `musicstream` IOP module builds automatically

`musicstream.irx` is built by the project build itself: the root `Makefile` invokes
`make -C PS2/lib/musicstream` (inheriting the exported `PS2DEV` / `PS2SDK` / `PATH`),
then embeds the resulting IRX into the EE ELF and `SifExecModuleBuffer`-loads it at
runtime. No separate step is required for a normal `make ps2`. To build it standalone:

```sh
cd PS2/lib/musicstream
make            # -> musicstream.irx (needs the ps2dev IOP toolchain on PATH)
```

### 6. Build and run

From the project root:

```sh
make ps2          # build the EE ELF (pulls in the fork archives + musicstream.irx)
make ps2-run      # run under PCSX2
make ps2-hw       # deploy to real hardware via ps2client
```

---

## Fork Delta Report: ps2gl and ps2stuff

This report summarizes the local deltas in two PS2 library forks and explains the technical reason each change exists. The intended audience is maintainers of the PS2 SDK ecosystem, especially `ps2gl` and `ps2stuff`.

The downstream project using these forks is HyperSolar, but the library changes below are described in library terms. Game-specific details are included only where they explain an observed requirement or a compatibility constraint.

## High-Level Summary

Two nested forks were reviewed:

- `PS2/lib/ps2gl`
- `PS2/lib/ps2stuff`

Both nested repositories are clean worktrees.

`ps2gl` baseline and delta:

- Clone baseline: `3edd2e158c9fb4c3960657c4f313280acb73c9d5`
- Baseline subject: `inital changes for raylib test`
- Current commit: `15d411f`
- Commits after clone: 15
- Net delta: 13 files changed, 887 insertions, 17 deletions

`ps2stuff` baseline and delta:

- Clone baseline: `c8541baad8f1ad5462bf5955c66f51a15fcf20e2`
- Baseline subject: `Merge pull request #7 from ps2dev/cmake`
- Current commit: `4c475f7`
- Commits after clone: 4
- Net delta: 4 files changed, 53 insertions, 0 deletions

The local changes fall into these broad categories:

1. Build proof canaries.
2. GS memory usage query APIs.
3. Runtime display mode and raster offset control.
4. Centered viewport scaling for overscan/screen fit.
5. GS edge anti-aliasing exposure.
6. GS dither control through the draw environment.
7. Per-texture CLUT ownership for PSMT8 textures.
8. PSMT8 convenience uploaders.
9. Manual GS mipmap upload for PSMCT16 and PSMT8 textures.
10. Manual mip texture lifetime/release tracking.
11. A local Python replacement for the old VU `gasp` preprocessing step.

The most upstream-relevant changes are probably:

- `CMemManager::GetMemInfo()` and a ps2gl wrapper.
- `CDisplayEnv::SendDisplayPos()`.
- Runtime display offset and video mode control in ps2gl.
- Exposing GS `PRIM.AA1` through ps2gl.
- Exposing GS `DTHE` through the ps2gl draw environment.
- Fixing/modernizing CLUT ownership for multiple simultaneous PSMT8 textures.
- A real mipmap path using `TEX1` plus `MIPTBP1/2`.

## ps2stuff Delta

### Files Changed

```text
10  0  include/ps2s/displayenv.h
9   0  include/ps2s/gsmem.h
8   0  src/gs.cpp
26  0  src/gsmem.cpp
```

### 1. Build Canary

File:

- `src/gs.cpp`

Change:

- Adds `#include <stdio.h>`.
- Prints a one-shot canary from `GS::Init()`.

Current behavior:

```text
[ CANARY ] Initializing MODIFIED LOCAL ps2stuff [2026.06.04 12:25]
```

Why it exists:

- The downstream build can accidentally resolve to the toolchain-installed `libps2stuff` instead of the local fork.
- `GS::Init()` runs once through ps2gl initialization, so this proves which library was linked.

Upstream relevance:

- Diagnostic only. This should not be upstreamed as-is.

### 2. GS Memory Usage Query

Files:

- `include/ps2s/gsmem.h`
- `src/gsmem.cpp`

New API:

```cpp
void CMemSlotList::AccumMemInfo(int& total, int& used, int& largestFree);
void CMemManager::GetMemInfo(int& total, int& used, int& largestFreeSlot);
```

Behavior:

- Walks locked slots and all unlocked slot lists.
- Accumulates total carved pages.
- Counts bound or locked slots as used.
- Tracks the largest single free slot.

Implementation shape:

```cpp
void CMemSlotList::AccumMemInfo(int& total, int& used, int& largestFree)
{
    tSlotIter curSlot = Slots.begin();
    for (; curSlot != Slots.end(); curSlot++) {
        int len = (*curSlot)->GetPageLength();
        total += len;
        if ((*curSlot)->IsBound() || (*curSlot)->IsLocked())
            used += len;
        else if (len > largestFree)
            largestFree = len;
    }
}

void CMemManager::GetMemInfo(int& total, int& used, int& largestFreeSlot)
{
    total = used = largestFreeSlot = 0;

    LockedSlots.AccumMemInfo(total, used, largestFreeSlot);

    tSlotListIter curList = SlotLists.begin();
    for (; curList != SlotLists.end(); curList++)
        (*curList)->AccumMemInfo(total, used, largestFreeSlot);
}
```

Why it exists:

- `ps2stuff` already owns the GS memory slot allocator.
- `ps2gl` can print allocation state, but the application needed numeric values for debug/profiling overlays and memory-fit decisions.
- The largest free slot is more useful than total free pages because a texture allocation must fit in one compatible slot.

Upstream relevance:

- This is a generally useful query API.
- It does not alter allocation behavior.
- It mirrors existing `PrintAllocation()` traversal but returns data instead of printing it.

Potential API refinement:

- Return a struct rather than three reference parameters.
- Include counts in bytes as well as pages, or document the 8 KB page unit explicitly.
- Consider reporting per-PSM free-slot information if callers need to reason about texture formats.

### 3. DISPLAY-only Register Push

File:

- `include/ps2s/displayenv.h`

New API:

```cpp
inline void SendDisplayPos(void)
{
    using namespace GS::ControlRegs;
    *(uint64_t*)display2 = *(uint64_t*)&gsrDisplay2;
}
```

Behavior:

- Writes only `DISPLAY2`.
- Leaves `PMODE`, `DISPFB`, and `BGCOLOR` untouched.

Why it exists:

- Runtime screen-centering controls need to shift the output raster without re-sending unrelated display state.
- The full `SendSettings()` path writes more registers than necessary and can visibly disturb the overscan/background area during live adjustment.

Upstream relevance:

- A narrow display-position update is generally useful.
- The hardcoded `DISPLAY2` assumption matches ps2gl's current display path, but a more general upstream API could choose DISPLAY1 or DISPLAY2 based on the active read circuit.

Potential API refinement:

```cpp
void SendDisplayPos(tReadCircuit circuit);
```

or:

```cpp
void SendDisplay1Pos();
void SendDisplay2Pos();
```

## ps2gl Delta

### Files Changed

```text
12   2   Makefile
18   0   include/GL/ps2gl.h
10   0   include/ps2gl/displaycontext.h
16   0   include/ps2gl/drawcontext.h
5    0   include/ps2gl/glcontext.h
51   0   include/ps2gl/texture.h
4    1   src/base_renderer.cpp
71   0   src/displaycontext.cpp
101  3   src/drawcontext.cpp
12   0   src/glcontext.cpp
20   0   src/gsmemory.cpp
272  11  src/texture.cpp
295  0   vu1/gasp.py
```

### 1. Build Integration with Sibling ps2stuff

File:

- `Makefile`

Change:

```make
EE_INCS += -I./include -I./vu1 -I../ps2stuff/include -I$(PS2SDK)/ports/include
```

Why it exists:

- The ps2gl fork calls APIs added to the sibling ps2stuff fork, notably `CMemManager::GetMemInfo()` and `CDisplayEnv::SendDisplayPos()`.
- The sibling fork headers must appear before the toolchain-installed ports headers.

Upstream relevance:

- The exact relative include path is local-fork specific.
- The underlying issue is relevant: if ps2gl adds APIs that rely on ps2stuff changes, both libraries need versioned/coordinated updates or feature checks.

### 2. VU Preprocessor Replacement

Files:

- `Makefile`
- `vu1/gasp.py`

Change:

- Adds `vu1/gasp.py`.
- Changes the VU preprocessing step from `gasp` to `python3 vu1/gasp.py`.
- Gates VU regeneration behind `REBUILD_VU1=1`; normal builds expect checked-in `.vsm` files.

Supported `gasp.py` subset:

- `.include`
- `.macro/.endm`
- `.assignc`
- `.aif/.aelse/.aendi`
- `.arepeat/.aendr`
- `.equ`
- backslash macro parameters
- `\@`
- `\&var`

Why it exists:

- The old `gasp` tool is an external dependency that may be missing on modern setups.
- The fork needed a reproducible way to regenerate VU sources when intentionally requested, while keeping normal builds stable.

Upstream relevance:

- A Python replacement for the required macro subset may be useful, but it should be treated as a compatibility tool rather than a full assembler preprocessor.
- Gating regeneration is sensible because VU output churn is hard to review.

Risk:

- Future VCL macro usage outside this implemented subset can break regeneration or silently emit different code.

### 3. Build Canary

File:

- `src/glcontext.cpp`

Change:

- `pglInit()` prints a one-shot canary.

Current behavior:

```text
[ CANARY ] Welcome to MODIFIED LOCAL ps2gl! [2026.06.18 14:38]
```

Why it exists:

- Proves the local ps2gl archive was linked instead of the toolchain-installed one.

Upstream relevance:

- Diagnostic only. This should not be upstreamed as-is.

### 4. GS Memory Info Wrapper

Files:

- `include/GL/ps2gl.h`
- `src/gsmemory.cpp`

New C API:

```c
extern void pglGetGsMemInfo(int* total, int* used, int* largestFreeSlot);
```

Implementation:

```cpp
void pglGetGsMemInfo(int* total, int* used, int* largestFreeSlot)
{
    int t = 0, u = 0, l = 0;
    GS::CMemArea::GetMemManager().GetMemInfo(t, u, l);
    if (total)
        *total = t;
    if (used)
        *used = u;
    if (largestFreeSlot)
        *largestFreeSlot = l;
}
```

Why it exists:

- Exposes the new ps2stuff memory query through ps2gl's C API.
- Lets C applications or raylib-style platform layers query live GS memory use.

Upstream relevance:

- Generally useful.
- Should be documented as page counts, not bytes.
- Depends on an upstream ps2stuff memory-query addition.

### 5. Runtime Display Mode Reconfiguration

Files:

- `include/GL/ps2gl.h`
- `include/ps2gl/displaycontext.h`
- `src/displaycontext.cpp`

New C API:

```c
extern void pglSetVideoMode(int interlaced, int overscan_mode, int screen_x, int screen_y);
```

Behavior:

- Reuses current framebuffer memory.
- Updates ps2gl display interlace state.
- Calls `CDisplayEnv::SetDisplayMode(...)`.
- Reprograms `FB2`.
- Calls `SetDisplay2(...)` using different magnification for interlaced and progressive modes.
- Sends display settings.

Important mode handling:

```cpp
if (interlaced)
    DisplayEnv->SetDisplay2(width, height * 2, screenX, screenY, 4, 1);
else
    DisplayEnv->SetDisplay2(width, height, screenX, screenY, 2, 1);
```

Why it exists:

- Applications may need to switch between interlaced and progressive GS layouts at runtime.
- The existing display setup path was initialization-oriented.

The 480p fix:

- The progressive path uses full buffer height and `magH = 2`.
- The code comment states the previous `magH = 4` behavior made the image exactly twice too wide on real hardware, while PCSX2 normalized it enough to hide the bug.

Upstream relevance:

- Runtime mode switching is generally useful.
- The 480p magnification correction is hardware-relevant.

Risk:

- This does not itself rebuild GS memory layouts; it assumes the current frame buffers match the requested mode.
- Applications that change buffer dimensions or PSM still need to rebuild draw/display buffers appropriately.

### 6. Runtime DISPLAY Offset

Files:

- `include/GL/ps2gl.h`
- `include/ps2gl/displaycontext.h`
- `src/displaycontext.cpp`

New C API:

```c
extern void pglSetDisplayOffset(int screen_x, int screen_y);
```

Behavior:

- Recomputes `DISPLAY2` for the current mode and current framebuffer dimensions.
- Calls `SendDisplayPos()` to write only DISPLAY2.

Why it exists:

- Live raster centering should not require a full display-state resend.

Upstream relevance:

- Useful if paired with a generalized ps2stuff `SendDisplayPos()` API.

Risk:

- Current implementation follows the ps2gl read-circuit-2 assumption.

### 7. Centered Viewport Scale

Files:

- `include/GL/ps2gl.h`
- `include/ps2gl/drawcontext.h`
- `src/drawcontext.cpp`

New C API:

```c
extern void pglSetViewportScale(float sx, float sy);
```

Behavior:

- Adds persistent `VpScaleX` and `VpScaleY` fields to `CImmDrawContext`.
- Folds the scale into `GSScale`.
- Recomputes drawenv scissor to a centered sub-rectangle.
- Persists across `SetDrawBuffers()`.
- Bounds scale to `0 < s <= 1`.

Why it exists:

- Some applications need render-side screen fit for TV overscan.
- Generic logical `glViewport` behavior can be awkward on PS2 interlaced modes because the draw buffer may be a half-height field buffer while higher layers think in full logical screen dimensions.
- Applying scale in ps2gl's draw transform keeps units tied to the actual draw buffer.

Relevant implementation behavior:

```cpp
int vpw = (int)(Width  * sx + 0.5f);
int vph = (int)(Height * sy + 0.5f);
GSScale.set_scale(cpu_vec_xyz(vpw / 2.0f, -1 * vph / 2.0f, -1 * (float)maxDepthValue / 2.0f));
DrawEnv->SetScissorArea((Width - vpw) / 2, (Height - vph) / 2, vpw, vph);
```

Upstream relevance:

- Potentially useful as an explicit ps2gl screen-fit primitive.
- Should be documented as a centered scale/squish, not a general viewport implementation.

Risk:

- Scaling above 1.0 is intentionally rejected in the local fork because downstream CPU clipping happens before this scale. A general upstream implementation should define expected zoom semantics carefully.

### 8. GS Edge Anti-Aliasing

Files:

- `include/GL/ps2gl.h`
- `include/ps2gl/glcontext.h`
- `include/ps2gl/drawcontext.h`
- `src/base_renderer.cpp`
- `src/drawcontext.cpp`
- `src/glcontext.cpp`

New C API/capability:

```c
#define PGL_EDGE_AA 3
pglEnable(PGL_EDGE_AA);
pglDisable(PGL_EDGE_AA);
```

Behavior:

- Adds `EdgeAAIsEnabled` draw-context state.
- Adds immediate and display-list state setters.
- Adds renderer context change tracking.
- Sets `GS::tPrim.aa1` in giftag generation.

Core render change:

```cpp
bool edgeAA = drawContext.GetEdgeAAEnabled();
GS::tPrim prim = { prim_type : primType, iip : smoothShading, tme : useTexture, fge : 0, abe : alpha, aa1 : edgeAA, fst : 0, ctxt : 0, fix : 0 };
```

Why it exists:

- The GS exposes primitive edge anti-aliasing through `PRIM.AA1`, but ps2gl did not provide application-level control.

Upstream relevance:

- Generally useful.
- The implementation follows ps2gl's existing custom `pglEnable`/`pglDisable` pattern.

Important limitation:

- This smooths primitive edges. It does not filter textures and does not solve texture shimmer/minification.

### 9. GS Dither Toggle

File:

- `src/drawcontext.cpp`

New C symbol:

```c
extern "C" void pgl_enable_dither(int enable);
```

Behavior:

- Calls `EnableDither()` or `DisableDither()` on the active `CDrawEnv`.
- Calls `DrawEnvChanged()` so the setting is re-sent through ps2gl's normal drawenv path.

Implementation:

```cpp
extern "C" void pgl_enable_dither(int enable)
{
    CImmDrawContext& dc = pGLContext->GetImmDrawContext();
    if (enable)
        dc.GetDrawEnv().EnableDither();
    else
        dc.GetDrawEnv().DisableDither();
    pGLContext->DrawEnvChanged();
}
```

Why it exists:

- Dithering is useful when rendering to 16-bit framebuffers.
- ps2stuff already has DIMX/DTHE support in `CDrawEnv`, but ps2gl did not expose a public toggle.
- The setting must live in the drawenv object. A raw register poke would be overwritten when ps2gl re-sends drawenv state.

Upstream relevance:

- Generally useful.
- Should be promoted to a declared public API if upstreamed.
- Naming should probably match ps2gl style, for example `pglEnableDither(int)` or a `PGL_DITHER` capability.

### 10. Per-Texture CLUT Ownership

Files:

- `include/ps2gl/texture.h`
- `src/texture.cpp`

Core change:

- Adds `CMMTexture::OwnClut`.
- `CTexManager::SetCurClut(...)` creates a `CMMClut` owned by the currently bound texture.
- `CTexManager::UseCurTexture(...)` prefers the texture-owned CLUT and falls back to the manager-global `CurClut`.
- `CMMTexture::~CMMTexture()` deletes `OwnClut`.

Why it exists:

- The original manager-global CLUT model breaks when more than one PSMT8 texture is resident.
- With a single global CLUT, the latest `glColorTable(...)` effectively changes the palette used by other indexed textures.
- Per-texture CLUT ownership makes PSMT8 textures self-contained.

Representative logic:

```cpp
CMMClut* clut = CurTexture->GetOwnClut();
if (clut == NULL)
    clut = CurClut;
clut->Load(renderPacket);
CurTexture->SetClut(*clut);
```

and:

```cpp
CMMClut* newClut = new CMMClut(clut, numEntries);
if (CurTexture)
    CurTexture->SetOwnClut(newClut);
CurClut = newClut;
```

Upstream relevance:

- This looks like a real correctness fix for multiple simultaneous indexed textures.
- It should be reviewed for display-list behavior and ownership/lifetime edge cases.

Risk:

- Correctness depends on `glColorTable(...)` being called while the intended texture is bound.
- `CurClut` becomes a non-owning fallback pointer. The lifetime assumptions should be made explicit if upstreamed.

### 11. PSMT8 Convenience Upload

File:

- `src/texture.cpp`

New C symbol:

```c
extern "C" unsigned int pgl_create_index8(const void* indices, int w, int h, const void* clut);
```

Behavior:

- Generates a texture name.
- Uploads index data via `glTexImage2D(... GL_COLOR_INDEX, GL_UNSIGNED_BYTE ...)`.
- Uploads a 256-entry RGBA CLUT via `glColorTable(...)`.
- Sets default repeat wrapping, linear filtering, and modulate env.

Why it exists:

- Provides a simple C entry point for embedded PSMT8 data plus precomputed CLUT data.
- Avoids requiring callers to duplicate the exact ps2gl `glTexImage2D` and `glColorTable` sequence.

Assumptions:

- `clut` is a 256-entry RGBA table.
- `clut` is 16-byte aligned.
- In the downstream asset pipeline, CLUT data is already in GS CSM1 storage order.

Upstream relevance:

- Useful as a helper, but the assumptions should be documented.
- If ps2gl wants to stay OpenGL-like, this might belong as utility API rather than core GL wrapper behavior.

### 12. Manual PSMCT16 Mipmaps

Files:

- `include/ps2gl/texture.h`
- `src/texture.cpp`

New C symbol:

```c
extern "C" unsigned int pgl_create_mip16(
    void** levels,
    const int* lw,
    const int* lh,
    int count,
    int kbias,
    int min_filter
);
```

New `CMMTexture` support:

```cpp
void SetMipLevels(int mxl, int kbias, int minFilter);
void LockGsSlot();
void UnlockGsSlot();
```

Behavior:

- Creates a base GL texture.
- Uploads level 0 as normal PSMCT16 texture data.
- Uploads levels 1..N as separate resident `CMMTexture` objects.
- Locks each mip level's GS slot so the LRU allocator cannot evict it.
- Sets base texture `TEX1` fields:
  - `MXL`
  - `MMIN`
  - `K`
  - `LCM = 0`
  - `L = 0`
  - `MMAG = 1`
- Packs and writes `MIPTBP1_1` and `MIPTBP2_1` through a raw GIF PATH3 A+D DMA packet.

Why it exists:

- The GS has native mipmap support through `TEX1` and `MIPTBP1/2`.
- ps2gl does not expose a working mipmap upload path.
- Mipmaps are needed for large minified textured geometry such as floors/terrain.

Important implementation facts:

- Mip levels do not need to be contiguous in GS memory because `MIPTBP1/2` hold explicit base pointers.
- ps2stuff/ps2gl already re-emit `TEX1` with texture state, so per-texture `MXL`, `MMIN`, and `K` work naturally.
- ps2stuff/ps2gl do not normally emit `MIPTBP1/2`, so the fork writes them manually once.
- Mip levels are sampled but never directly drawn; without locking, ps2gl's allocator can evict them.

Raw MIPTBP write shape:

```cpp
static void pgl_send_miptbp(u64 miptbp1, u64 miptbp2)
{
    static u64 pkt[6] __attribute__((aligned(64)));
    pkt[0] = 0x1000000000008002ULL;
    pkt[1] = 0x0EULL;
    pkt[2] = miptbp1; pkt[3] = 0x34;
    pkt[4] = miptbp2; pkt[5] = 0x36;

    FlushCache(0);
    u32 madr = (u32)((u64)pkt) & 0x1FFFFFFF;
    *(volatile u32*)0x1000A010 = madr;
    *(volatile u32*)0x1000A020 = 3;
    *(volatile u32*)0x1000A000 = 0x101;
    while (*(volatile u32*)0x1000A000 & 0x100) ;
}
```

Upstream relevance:

- Real mipmap support is broadly valuable.
- The implementation proves a workable path, but the raw DMA/register write should probably be integrated into ps2gl's packet/state system before upstreaming.

Key limitation:

- `MIPTBP1/2` are global GS state. This implementation assumes exactly one live mipped texture owns those registers, unless a later draw re-sends the correct MIPTBP.

### 13. Manual PSMT8 Mipmaps

File:

- `src/texture.cpp`

New C symbol:

```c
extern "C" unsigned int pgl_create_index8_mip(
    const void** levels,
    const int* lw,
    const int* lh,
    int count,
    const void* clut,
    int kbias,
    int min_filter
);
```

Behavior:

- Creates base level through `pgl_create_index8(...)`.
- Uploads mip levels as resident `kPsm8` `CMMTexture`s.
- Locks those levels.
- Computes PSMT8 TBW as `ceil(width / 128) * 2`, minimum 2.
- Sets base `TEX1`.
- Writes `MIPTBP1/2`.

Why it exists:

- PSMT8 is valuable for GS memory pressure.
- Paletted textures still need mipmaps for minified surfaces.
- The GS has one CLUT per texture; all PSMT8 mip levels sample through the base CLUT.

Upstream relevance:

- Same as PSMCT16 mip support.
- Should probably be part of a general mipmap API rather than a one-off C helper.

### 14. Mip Registry and Deep Release

File:

- `src/texture.cpp`

New C symbol:

```c
extern "C" void pgl_delete_mips(unsigned int baseId);
```

Behavior:

- Keeps a small registry mapping base texture id to hidden mip-level `CMMTexture*` objects.
- On delete:
  - unlocks every hidden mip texture's GS slot
  - deletes each hidden `CMMTexture`
  - clears the registry entry

Why it exists:

- The base GL texture id does not own or know about the hidden mip-level textures.
- Plain `glDeleteTextures()` or higher-level texture unloads only release the base level.
- Locked mip levels would otherwise leak GS slots.
- Releasing old mip levels before creating new ones also preserves the current global-MIPTBP ownership model.

Upstream relevance:

- If mip levels become first-class ps2gl texture state, this registry should be replaced by normal texture ownership.
- In the local fork it is a practical release valve for the manual mipmap design.

Risk:

- Registry capacity is fixed at 4.
- Multiple simultaneous mipped textures remain unsafe unless MIPTBP is resent per texture use.

## Cross-Library Dependencies

### Memory Query Chain

```text
application
-> ps2gl: pglGetGsMemInfo()
-> ps2stuff: CMemManager::GetMemInfo()
-> ps2stuff: CMemSlotList::AccumMemInfo()
```

This requires both forks. ps2gl provides the C-facing wrapper; ps2stuff provides the allocator traversal.

### Display Offset Chain

```text
application
-> ps2gl: pglSetDisplayOffset()
-> ps2gl: CDisplayContext::SetDisplayOffset()
-> ps2stuff: CDisplayEnv::SetDisplay2()
-> ps2stuff: CDisplayEnv::SendDisplayPos()
-> GS DISPLAY2
```

This also requires both forks. Without ps2stuff's DISPLAY-only write, ps2gl cannot provide the no-full-state-resend behavior.

### Mipmap Chain

```text
application texture loader
-> ps2gl: pgl_create_mip16() or pgl_create_index8_mip()
-> ps2gl: CMMTexture::SetMipLevels()
-> ps2gl: pgl_send_miptbp()
-> GS TEX1 + MIPTBP1/2
```

Release:

```text
application texture unload
-> ps2gl: pgl_delete_mips()
-> hidden CMMTexture slots are unlocked and deleted
-> base texture is deleted separately
```

The design is functional, but a more upstreamable version should likely make mip levels owned by the base texture.

## Downstream CPU Clipping Workaround

The downstream renderer also carries a CPU software clipper. This is not a ps2gl fork change, but it explains an important library-facing constraint: some applications need to disable ps2gl clipping and submit already-clipped triangles.

Observed requirement:

- Large or custom world-space geometry can cross the near plane or GS guard band.
- With `PGL_CLIPPING` disabled, raw triangles outside the guard band can produce visible corruption.
- With `PGL_CLIPPING` enabled, the ps2gl VU clipping path can reject or mishandle some application-generated passes.
- The application therefore clips on the EE CPU and emits safe eye-space triangles with `PGL_CLIPPING` disabled.

The clipper:

- Uses Sutherland-Hodgman clipping.
- Clips against five planes: near, left, right, bottom, top.
- Uses a conservative guard band (`SH_NDC_LIMIT = 5.5f`).
- Has a fast all-inside test to avoid running the full clipper for most triangles.
- Emits clipped polygons as triangle fans.
- Starts `glBegin(GL_TRIANGLES)` lazily only after the first surviving triangle.

The relevant constants are:

```c
#define SH_NDC_LIMIT     5.5f
#define SH_NEAR_Z        1.0f
#define SH_RAYLIB_NEAR_Z 0.05f
#define SH_CLIP_PLANES   5
#define SH_MAX_VERTS     16
```

Current downstream implementation:

```c
 * Software clipper — verbatim from PS2/render.c
 * ============================================================ */

typedef struct {
    float x, y, z;
    float u, v;
} ClipVert;

SHBasis SHBasisFromAngles(float yaw, float pitch) {
    float   cy = cosf(yaw), sy = sinf(yaw);
    float   cp = cosf(pitch), sp = sinf(pitch);
    SHBasis b;
    b.right   = (Vector3){cy, 0.0f, -sy};
    b.up      = (Vector3){sp * sy, cp, sp * cy};
    b.forward = (Vector3){sy * cp, -sp, cy * cp};
    return b;
}

static inline float plane_dist4(const ClipVert *v, const float pl[4]) {
    return pl[0] * v->x + pl[1] * v->y + pl[2] * v->z + pl[3];
}

static inline void lerp_vert(ClipVert *o, const ClipVert *va, const ClipVert *vb, float t) {
    o->x = va->x + (vb->x - va->x) * t;
    o->y = va->y + (vb->y - va->y) * t;
    o->z = va->z + (vb->z - va->z) * t;
    o->u = va->u + (vb->u - va->u) * t;
    o->v = va->v + (vb->v - va->v) * t;
}

static int clip_poly(const ClipVert *in, int n, ClipVert *out, const float pl[4]) {
    int             count   = 0;
    const ClipVert *prev    = &in[n - 1];
    float           prev_d  = plane_dist4(prev, pl);
    int             prev_in = (prev_d >= 0.0f);

    for (int i = 0; i < n; i++) {
        const ClipVert *cur    = &in[i];
        float           cur_d  = plane_dist4(cur, pl);
        int             cur_in = (cur_d >= 0.0f);

        if (prev_in != cur_in && count < SH_MAX_VERTS) {
            float denom = prev_d - cur_d;
            float t     = (fabsf(denom) > 1e-6f) ? (prev_d / denom) : 0.0f;
            lerp_vert(&out[count++], prev, cur, t);
        }
        if (cur_in && count < SH_MAX_VERTS) out[count++] = *cur;

        prev    = cur;
        prev_d  = cur_d;
        prev_in = cur_in;
    }
    return count;
}

static int clip_triangle(
    const ClipVert tri[3], ClipVert *out, const float planes[SH_CLIP_PLANES][4]
) {
    int need_clip = 0;
    for (int p = 0; p < SH_CLIP_PLANES; p++) {
        const float *pl = planes[p];
        if (plane_dist4(&tri[0], pl) < 0.0f || plane_dist4(&tri[1], pl) < 0.0f ||
            plane_dist4(&tri[2], pl) < 0.0f) {
            need_clip = 1;
            break;
        }
    }
    if (!need_clip) {
        out[0] = tri[0];
        out[1] = tri[1];
        out[2] = tri[2];
        return 3;
    }

    ClipVert  buf_a[SH_MAX_VERTS], buf_b[SH_MAX_VERTS];
    ClipVert *src = buf_a, *dst = buf_b;
    int       count = 3;

    buf_a[0] = tri[0];
    buf_a[1] = tri[1];
    buf_a[2] = tri[2];

    for (int p = 0; p < SH_CLIP_PLANES; p++) {
        count = clip_poly(src, count, dst, planes[p]);
        if (count < 3) return 0;
        ClipVert *tmp = src;
        src           = dst;
        dst           = tmp;
    }

    for (int i = 0; i < count; i++)
        out[i] = src[i];
    return count;
}

static void projection_factors(float fovy, float aspect, float *px, float *py) {
    static float cached_fovy   = -1.0f;
    static float cached_aspect = -1.0f;
    static float cached_px     = 1.0f;
    static float cached_py     = 1.0f;
    if (fovy != cached_fovy || aspect != cached_aspect) {
        cached_py     = 1.0f / tanf((fovy * PI / 180.0f) * 0.5f);
        cached_px     = cached_py / aspect;
        cached_fovy   = fovy;
        cached_aspect = aspect;
    }
    *px = cached_px;
    *py = cached_py;
}

static inline ClipVert to_eye(float lx, float ly, float lz, float u, float v, const SHBasis *b) {
    ClipVert cv;
    cv.x = lx * b->right.x + ly * b->right.y + lz * b->right.z;
    cv.y = lx * b->up.x + ly * b->up.y + lz * b->up.z;
    cv.z = lx * b->forward.x + ly * b->forward.y + lz * b->forward.z;
    cv.u = u;
    cv.v = v;
    return cv;
}

static inline ClipVert to_eye_rot_y(float x, float y, float z, float u, float v,
                                    const SHBasis *b, Vector3 eye,
                                    float rot_c, float rot_s) {
    /* Identity Y-rotation (rot_s == 0, rot_c == 1) is the common case — every
       per-element mesh (boxes, particles, jets, splashes, reticles, sun, flare)
       goes through DrawMeshSHNear with no rotation. Skip the 4 mults + 2 adds;
       only the skybox/floor pass a real rot. (sinf is exactly 0 only at angle 0,
       where cosf is exactly 1, so the fast path is also bit-correct there.) */
    float rx, rz;
    if (rot_s == 0.0f) { rx = x; rz = z; }
    else { rx = x * rot_c + z * rot_s; rz = -x * rot_s + z * rot_c; }
    return to_eye(rx - eye.x, y - eye.y, rz - eye.z, u, v, b);
}

static inline ClipVert to_eye_rot_y_floor_uv(float x, float y, float z,
                                             const SHBasis *b, Vector3 eye,
                                             float rot_c, float rot_s) {
    float rx, rz;
    if (rot_s == 0.0f) { rx = x; rz = z; }
    else { rx = x * rot_c + z * rot_s; rz = -x * rot_s + z * rot_c; }
    return to_eye(rx - eye.x, y - eye.y, rz - eye.z,
                  rx / FLOOR_TILE_W, rz / FLOOR_TILE_L, b);
}

static inline void emit_eye_vert(const ClipVert *cv, float u_off, float v_off) {
    glTexCoord2f(cv->u + u_off, cv->v + v_off);
    glVertex3f(-cv->x, cv->y, -cv->z);
}

static void DrawMeshSHNearRotY(
    Mesh mesh, unsigned int texId, const SHBasis *basis, float fovy, float aspect, Vector3 eye_pos,
    float u_off, float v_off, float near_z, float rot_c, float rot_s, bool floor_uv
) {
    float px, py;
    projection_factors(fovy, aspect, &px, &py);
    if (near_z < SH_RAYLIB_NEAR_Z) near_z = SH_RAYLIB_NEAR_Z;

    const float planes[SH_CLIP_PLANES][4] = {
        {0.0f, 0.0f, 1.0f, -near_z},    {-px, 0.0f, SH_NDC_LIMIT, 0.0f},
        {px, 0.0f, SH_NDC_LIMIT, 0.0f}, {0.0f, -py, SH_NDC_LIMIT, 0.0f},
        {0.0f, py, SH_NDC_LIMIT, 0.0f},
    };

    const float          *verts     = mesh.vertices;
    const float          *texc      = mesh.texcoords;
    const unsigned short *indices   = mesh.indices;
    int                   has_tex   = (texId > 0 && (texc != NULL || floor_uv));
    int                   began     = 0;
    int                   tri_count = mesh.triangleCount;

    for (int t = 0; t < tri_count; t++) {
        int i0, i1, i2;
        if (indices) {
            i0 = indices[t * 3 + 0];
            i1 = indices[t * 3 + 1];
            i2 = indices[t * 3 + 2];
        } else {
            i0 = t * 3 + 0;
            i1 = t * 3 + 1;
            i2 = t * 3 + 2;
        }

        int      v0i = i0 * 3, v1i = i1 * 3, v2i = i2 * 3;
        ClipVert tri[3];
        if (floor_uv) {
            tri[0] = to_eye_rot_y_floor_uv(verts[v0i], verts[v0i + 1], verts[v0i + 2],
                                           basis, eye_pos, rot_c, rot_s);
            tri[1] = to_eye_rot_y_floor_uv(verts[v1i], verts[v1i + 1], verts[v1i + 2],
                                           basis, eye_pos, rot_c, rot_s);
            tri[2] = to_eye_rot_y_floor_uv(verts[v2i], verts[v2i + 1], verts[v2i + 2],
                                           basis, eye_pos, rot_c, rot_s);
        } else if (has_tex) {
            tri[0] = to_eye_rot_y(verts[v0i], verts[v0i + 1], verts[v0i + 2],
                                  texc[i0 * 2], texc[i0 * 2 + 1], basis,
                                  eye_pos, rot_c, rot_s);
            tri[1] = to_eye_rot_y(verts[v1i], verts[v1i + 1], verts[v1i + 2],
                                  texc[i1 * 2], texc[i1 * 2 + 1], basis,
                                  eye_pos, rot_c, rot_s);
            tri[2] = to_eye_rot_y(verts[v2i], verts[v2i + 1], verts[v2i + 2],
                                  texc[i2 * 2], texc[i2 * 2 + 1], basis,
                                  eye_pos, rot_c, rot_s);
        } else {
            tri[0] = to_eye_rot_y(verts[v0i], verts[v0i + 1], verts[v0i + 2],
                                  0.0f, 0.0f, basis, eye_pos, rot_c, rot_s);
            tri[1] = to_eye_rot_y(verts[v1i], verts[v1i + 1], verts[v1i + 2],
                                  0.0f, 0.0f, basis, eye_pos, rot_c, rot_s);
            tri[2] = to_eye_rot_y(verts[v2i], verts[v2i + 1], verts[v2i + 2],
                                  0.0f, 0.0f, basis, eye_pos, rot_c, rot_s);
        }

        /* Fast all-inside test before the general clipper. The 5 planes reduce
           to: a vertex is inside iff  z >= near_z  &&  NDC*z >= |px*x|  &&
           NDC*z >= |py*y|  (the guard-band pairs ±px/±py collapse to abs). That's
           ~9 mults for the whole triangle vs ~60 for per-plane plane_dist4 — and
           the common case (triangle fully on-screen) is exactly this path. Only
           triangles that actually straddle a plane fall through to clip_triangle. */
        ClipVert clipped[SH_MAX_VERTS];
        int      count;
        int      all_in = 1;
        for (int k = 0; k < 3; k++) {
            float nz = SH_NDC_LIMIT * tri[k].z;
            if (tri[k].z < near_z ||
                nz < fabsf(px * tri[k].x) ||
                nz < fabsf(py * tri[k].y)) { all_in = 0; break; }
        }
        if (all_in) {
            clipped[0] = tri[0];
            clipped[1] = tri[1];
            clipped[2] = tri[2];
            count      = 3;
        } else {
            count = clip_triangle(tri, clipped, planes);
        }
        if (count < 3) continue;

        if (!began) {
            glMatrixMode(GL_MODELVIEW);
            glPushMatrix();
            glLoadIdentity();
            if (has_tex) {
                glEnable(GL_TEXTURE_2D);
                glBindTexture(GL_TEXTURE_2D, texId);
            }
            glBegin(GL_TRIANGLES);
            began = 1;
        }

        for (int j = 1; j < count - 1; j++) {
            emit_eye_vert(&clipped[0], u_off, v_off);
            emit_eye_vert(&clipped[j], u_off, v_off);
            emit_eye_vert(&clipped[j + 1], u_off, v_off);
        }
    }

    if (began) {
        glEnd();
        if (has_tex) glDisable(GL_TEXTURE_2D);
        glPopMatrix();
    }
}

void DrawMeshSHNear(
    Mesh mesh, unsigned int texId, const SHBasis *basis, float fovy, float aspect, Vector3 eye_pos,
    float u_off, float v_off, float near_z
) {
    DrawMeshSHNearRotY(mesh, texId, basis, fovy, aspect, eye_pos,
                       u_off, v_off, near_z, 1.0f, 0.0f, false);
}

void DrawMeshSH(
    Mesh mesh, unsigned int texId, const SHBasis *basis, float fovy, float aspect, Vector3 eye_pos,
    float u_off, float v_off
) {
    DrawMeshSHNear(mesh, texId, basis, fovy, aspect, eye_pos, u_off, v_off, SH_NEAR_Z);
}

static void DrawMeshSHRotYCS(
    Mesh mesh, unsigned int texId, const SHBasis *basis, float fovy, float aspect, Vector3 eye_pos,
    float u_off, float v_off, float rot_c, float rot_s
) {
    DrawMeshSHNearRotY(mesh, texId, basis, fovy, aspect, eye_pos,
                       u_off, v_off, SH_NEAR_Z, rot_c, rot_s, false);
}

static void DrawFloorCapMeshSHRotYCS(
    Mesh mesh, unsigned int texId, const SHBasis *basis, float fovy, float aspect, Vector3 eye_pos,
    float u_off, float v_off, float rot_c, float rot_s
) {
    DrawMeshSHNearRotY(mesh, texId, basis, fovy, aspect, eye_pos,
                       u_off, v_off, SH_NEAR_Z, rot_c, rot_s, true);
}
```

Library-facing takeaway:

- Applications may need a supported way to submit pre-clipped geometry while disabling ps2gl's internal clipping.
- `PGL_CLIPPING` already provides the switch, but the behavior and hazards around disabling it should be documented.
- A future ps2gl clipping path improvement could reduce the need for this workaround, but the workaround is currently practical and predictable.

## Upstreaming Notes

Likely good candidates:

- ps2stuff memory info query.
- ps2gl `pglGetGsMemInfo()`.
- DISPLAY-only position push, generalized beyond DISPLAY2 if needed.
- Runtime display offset API.
- GS edge AA exposure.
- GS dither exposure through drawenv state.
- Per-texture CLUT ownership fix.

Potential candidates with design work:

- Runtime video mode switching.
- Centered viewport scale/screen fit.
- PSMT8 upload helper.
- Full mipmap support.

Local-only / not upstream candidates as-is:

- Canary prints.
- Relative `../ps2stuff/include` include path.
- Fixed-size local mip registry.
- Raw one-shot MIPTBP DMA write as public behavior without ps2gl state integration.

The largest architectural issue is mipmap state. The current fork proves that GS mipmaps work through `TEX1` plus `MIPTBP1/2`, but global `MIPTBP` ownership prevents multiple simultaneous mipped textures unless ps2gl learns to bind/re-emit MIPTBP as part of texture state.

The second-largest issue is indexed texture ownership. The per-texture CLUT change appears to fix a real correctness problem and is much closer to being upstreamable, but it should be reviewed carefully for display-list and texture-deletion edge cases.


## musicstream — IOP Music Streamer

Repo: [`ninjadynamics/musicstream`](https://github.com/ninjadynamics/musicstream)
(submodule at `PS2/lib/musicstream`).

`musicstream.irx` is a small, self-contained **PlayStation 2 IOP module** that streams
stereo music to the SPU2 with **zero EE per-frame work**: the EE only sends
`play` / `stop` / `vol` / `pause` over SIF RPC, and the IOP does all the file reading
and SPU feeding itself. It is unrelated to the `ps2gl` / `ps2stuff` deltas above — it is
new code, not a fork — but it is part of the same PS2 backend and is built and embedded
by the same project build.

### Why it exists

`ps2sdk`'s `ps2snd` streaming (`sndStreamOpen`) **deadlocks the IOP** when it opens a
file to play: it performs the device mount/open inside its own blocking RPC, which
collides with the USB / cdvd driver threads. `musicstream` avoids this entirely by
owning the stream loop on a dedicated IOP thread and **never doing file I/O inside an
IRQ or the RPC handler**.

### How it works

- **Stereo = two SPU2 voices** (22 = L, 23 = R), each a **2-half ring** in SPU2 RAM.
  The input is chunk-interleaved `[L:CHUNK][R:CHUNK]…`; one CHUNK fills one half.
- A **stream thread** polls the play cursor (`sceSdGetAddr … NAX`) every ~4 ms and
  refills the half the cursor just left with the next file chunk via
  `sceSdVoiceTrans`. The ring self-loops on the SPU block flags (`0x06` start /
  `0x03` end) so playback is gapless; the file rewinds at EOF when looping.
- **The rule that beats `ps2snd`:** the blocking file read + SPU DMA happen ONLY on the
  stream thread. The RPC handler just hands off a command and returns; the open is async
  and self-retries (for removable-media mount latency), so the EE never blocks.
- Uses the same `libsd` (`sceSd*`) as the rest of the audio path — it drives only its
  two voices and never calls `sceSdInit`, so it coexists with EE-side SFX.

### Audio format

Raw **PS-ADPCM**, 16-byte blocks, stereo, chunk-interleaved, no VAG header, 48 kHz
native (pitch `0x1000`). `MUS_CHUNK_BLOCKS` (default 2048 = a 32 KB half per channel,
~1.2 s at 48 kHz) **must match how the file was encoded** — in this project it is kept
in lockstep with the `Makefile`'s `PS2_MUSIC_CHUNK` (the `pcm2adpcm --stereo` arg), since
the SPU2 ring half size is exactly one file chunk.

### RPC interface

The shared EE/IOP header [`musicstream_rpc.h`](https://github.com/ninjadynamics/musicstream/blob/master/musicstream_rpc.h)
defines the SID, command numbers, and request/response struct. EE side:

```c
SifRpcClientData_t cd;
MusRpc rpc __attribute__((aligned(64)));
sceSifBindRpc(&cd, MUSICSTREAM_RPC_SID, 0);          /* retry until cd.server */
/* play: */
snprintf(rpc.path, sizeof(rpc.path), "mass0:/track.adpcm");
rpc.loop = 1;
sceSifCallRpc(&cd, MUS_RPC_PLAY, 0, &rpc, sizeof(rpc), &rpc, sizeof(rpc), 0, 0);
```

The device is whatever the IOP can `open()` through iomanX (e.g. `mass0:` via the
BDM/USB stack, or `cdfs:`); load the device drivers before binding.

### Build & integration

Standalone: `make` in `PS2/lib/musicstream` produces `musicstream.irx` (needs the
ps2dev IOP toolchain on `PATH`). In this project the root `Makefile` builds it via
`make -C PS2/lib/musicstream`, embeds it into the EE ELF as `musicstream_irx` /
`musicstream_irx_size`, and `SifExecModuleBuffer`-loads it at runtime alongside the
stock `iomanX` / `usbd` / `bdm` / `usbmass_bd` / `bdmfs_fatfs` / `cdfs` IRX. The whole
subsystem can be compiled out with `PS2_MUSIC_OFF` (`hypersolar.h`), which stubs the
`ps2_music_*` RPC calls and skips loading the module.
