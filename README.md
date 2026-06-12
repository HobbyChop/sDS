# sDS - mGB-style DS MIDI synth (v0.6)

**sDS** is an **mGB-style MIDI synthesizer for the Nintendo DS / DS Lite**.
The DS itself is the synth: feed it MIDI through the
[mDS slot-2 cart](../rp2040/) and it plays the notes on its own sound
hardware - pulse, sample, noise and FM voices.

It is the **baseline ROM** that ships with the mDS, and the reference other
slot-2 MIDI apps build on. Builds to `sds_synth.nds`.

> Made by **hobbychop** - [hobbychop.com](https://hobbychop.com)

## At a glance

| | |
|---|---|
| **Platform** | Nintendo DS / DS Lite (runs from a slot-1 flashcart; DSi+ dropped slot-2) |
| **MIDI in** | via the mDS slot-2 cart over the `"STDS"` v2 protocol (event ring + clock mirror) |
| **Engine** | 4 voice types: PULSE (PSG square), SAMPLE (10 wavetables), NOISE (LFSR), FM (2-op wavetable) |
| **Voices** | PSG square ch 8-13, PSG noise ch 14-15, PCM ch 0-7 (SAMPLE + FM + drums share the pool) |
| **Modes** | MONO (6 monophonic instruments, classic mGB multitimbral) / POLY (one polyphonic instrument) |
| **Drums** | GM percussion kit on MIDI channel 10 (procedural, both modes) |
| **Control** | live on the DS (d-pad LAB editor) and over MIDI (full CC map) |
| **Build** | devkitARM + libnds (calico) |
| **Version** | v0.6 (see `source/version.h`) |

## Voices

| Voice  | DS hardware          | Pool | Notes |
|--------|----------------------|------|-------|
| PULSE  | PSG square ch 8-13   | 6    | hardware duty; soft-knee on loud notes |
| NOISE  | PSG noise ch 14-15   | 2    | LFSR; CC19 picks one of 8 rate "octaves" |
| SAMPLE | PCM ch 0-7 (shared)  | 8*   | looped single-cycle wave, 10 types (saw/square/tri/sine/pulse25/pulse12/dblsine/organ/softsq/bright) |
| FM     | PCM ch 0-7 (shared)  | 8*   | 2-op wavetable: ratio, depth, modulator self-feedback, baked at note-on |

\* SAMPLE, FM and the drum one-shots share the 8 PCM channels via a per-pool
LRU allocator. Voices retire at sub-LSB envelope level (no click).

## Modes

Toggle with **B**.

- **MONO** (default): MIDI channels 1-6 are six independent monophonic
  instruments (1,2 PULSE / 3 SAMPLE / 4 NOISE / 5,6 FM). Classic mGB feel,
  multitimbral.
- **POLY**: the whole synth becomes **one polyphonic instrument** whose voice
  type you choose (FM / PULSE / SAMPLE / NOISE). Pick it in the LAB, via
  Program Change, or CC 21.

**Channel 10** is always a General-MIDI drum kit in both modes (note number
picks the drum: 36 kick, 38 snare, 42 closed hat, 46 open hat, and so on).

## Controls

On the DS:

| Key    | Action |
|--------|--------|
| A      | detect / re-probe the cart |
| B      | toggle MONO / POLY |
| START  | panic (all sound off) |
| d-pad  | drive the LAB (up/down select param, left/right change value) |
| L / R  | (MONO) switch which channel the LAB edits |

**Top screen** = cart status + live MIDI monitor. **Bottom screen** = the
instrument **LAB**: a button-driven parameter editor with a live filled
**ADSR envelope plot**. The LAB and the MIDI CC map drive the same params.

## MIDI CC map (summary)

| CC | Param         | CC  | Param             |
|---:|---------------|----:|-------------------|
| 1  | vibrato depth | 73  | attack            |
| 76 | vibrato rate  | 75  | decay             |
| 7  | volume        | 70  | sustain           |
| 10 | pan           | 72  | release           |
| 16 | pulse duty    | 17  | FM depth          |
| 18 | FM ratio      | 79  | FM feedback       |
| 19 | noise rate    | 20  | sample wave       |
| 21 | poly type (POLY) | 120/121/123 | all sound off / reset CC / all notes off |

Pitch bend is +/-2 semitones. Program Change selects the POLY voice type (or,
in MONO on a SAMPLE channel, the wave). Full detail, channel map and the
extension guide: [docs/08-synth-app.md](../docs/08-synth-app.md).

## Build & flash

Needs **devkitARM + libnds + calico**. From a devkitPro msys2 shell:

```
cd ds-rom
make
```

Output: **`sds_synth.nds`**. Copy it to your slot-1 flashcart, insert the mDS
in slot-2, and feed MIDI into the cart. The launcher banner shows the version.

## Version

**v0.6** - pre-1.0 while the synth is validated on hardware and the drum / FM
voices are tuned by ear. The version is defined once in
[`source/version.h`](source/version.h) (`SDS_VERSION`), shown on the
top-screen banner, and mirrored in the ROM's launcher subtitle (`Makefile`).

## License

Licensed under **[CC BY-NC-SA 4.0](../LICENSE)** (Attribution-NonCommercial-
ShareAlike). You may use, modify, and share sDS for **non-commercial**
purposes, with attribution and under the same license; **selling sDS, mDS
units, or derivatives is not permitted**.

**Intent:** use sDS however you like, including to make, perform, record, and
**sell music** - the license does not cover what you create with it. The
restriction is only on commercial dealing in the device / ROM itself: no
selling mDS units or kits, no manufacturing for sale, no selling modified
firmware or hardware.

The "sDS" / "mDS" names are reserved by the author (trademark is separate from
this copyright license). For commercial use, contact the author via
[hobbychop.com](https://hobbychop.com).
