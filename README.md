# sDS

An mGB-style MIDI synthesizer for the Nintendo DS and DS Lite. Your DS
becomes the instrument: send it MIDI from a keyboard, sequencer, or DAW
through the mDS slot-2 cartridge, and it plays the notes on the DS sound chip
with pulse, sample, noise, and FM voices.

Made by hobbychop, hobbychop.com. Version 0.6.

## Getting started

1. Copy `sds_synth.nds` onto your slot-1 flashcart and launch it.
2. Put the mDS cartridge in the slot-2 port.
3. Press A to detect the cart. The top screen shows when it is connected.
4. Plug MIDI into the mDS and play.

The top screen is your monitor: connection, mode, and incoming MIDI. The
bottom screen is the LAB, where you shape the sound live with the d-pad.

## Playing it

Switch between two modes with B.

Mono is the classic mGB layout: six separate instruments, one per MIDI
channel, each playing one note at a time.

- Channels 1 and 2: pulse
- Channel 3: sample
- Channel 4: noise
- Channels 5 and 6: FM

Poly turns the whole synth into one instrument that plays chords. Choose its
voice (pulse, sample, noise, or FM) in the LAB, by Program Change, or with
CC 21.

Channel 10 is always a General MIDI drum kit in both modes. The note number
picks the drum (36 kick, 38 snare, 42 closed hat, 46 open hat, and so on).

Pitch bend covers two semitones, and note velocity sets loudness.

## Buttons

- A: detect the cart
- B: switch between Mono and Poly
- Start: panic, silence everything
- D-pad up and down: pick a setting in the LAB
- D-pad left and right: change the value
- L and R: in Mono, choose which channel the LAB edits

## Shaping the sound

Every setting in the LAB also responds to a MIDI control change, so you can
play it by hand or automate it from your sequencer. The controls below shape
the current instrument.

Performance

- CC 7: Volume
- CC 10: Pan
- CC 1: Vibrato depth
- CC 76: Vibrato rate

Envelope

- CC 73: Attack
- CC 75: Decay
- CC 70: Sustain
- CC 72: Release

Tone

- CC 16: Pulse duty
- CC 17: FM depth
- CC 18: FM ratio
- CC 79: FM feedback
- CC 19: Noise rate
- CC 20: Sample wave
- CC 21: Poly voice type

Panic and reset

- CC 120: All sound off
- CC 121: Reset controllers
- CC 123: All notes off

## Building it yourself

You need devkitARM and libnds. From a devkitPro msys2 shell:

```
cd ds-rom
make
```

That produces `sds_synth.nds`.

## License

Released under CC BY-NC-SA 4.0. You are free to use, change, and share sDS for
non-commercial purposes, with attribution, under the same license. Selling sDS
or mDS units is not allowed.

You can make, perform, record, and sell music with it. The license only
restricts selling the device or the software itself.

The names sDS and mDS are reserved by the author. For commercial use, get in
touch through hobbychop.com.
