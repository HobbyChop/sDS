<p align="center"><img src="sDS.png" width="500"></p>

# sDS

An mGB-style MIDI synthesizer for the Nintendo DS and DS Lite. Your DS
becomes the instrument: send it MIDI from a keyboard, sequencer, or DAW
through the mDS slot-2 cartridge, and it plays the notes on the DS sound chip
with pulse, sample, noise, and FM voices.

Made by hobbychop.

## Getting started

1. Copy `sds_synth.nds` onto your slot-1 flashcart and launch it.
2. Put the mDS cartridge in the slot-2 port.
3. Press A to detect the cart. The top screen shows when it is connected.
4. Plug MIDI into the mDS and play.

The top screen is your monitor: connection, mode, the channel map, and a
live scope of the audio output. The bottom screen is the LAB, where you
shape the sound live with the d-pad.

## Playing it

Switch between two modes with B.

Mono is the classic mGB layout: nine separate instruments (MIDI channels
1-9), each one monophonic. The default voices are spread across the channels:

- Channels 1 and 2: pulse
- Channel 3: sample
- Channel 4: noise
- Channels 5 and 6: FM
- Channels 7, 8, 9: pulse, sample, FM

Send a Program Change on a channel to change its voice (0 pulse, 1 sample,
2 noise, 3 FM), so any channel can be any voice. The LAB follows along.

Poly turns the whole synth into one instrument that plays chords. Choose its
voice (pulse, sample, noise, or FM) in the LAB, by Program Change, or with
CC 21.

Channel 10 is always a General MIDI drum kit in both modes. The note number
picks the drum (36 kick, 38 snare, 42 closed hat, 46 open hat, and so on);
the DRUMS page in the LAB sets the overall drum level.

Pitch bend defaults to two semitones and honours the standard RPN bend-range
message for wider ranges; note velocity sets loudness.

## Buttons

- A: detect the cart
- B: switch between Mono and Poly
- Start: open the PRESETS page
- D-pad up and down: pick a setting in the LAB
- D-pad left and right: change the value
- L and R: switch LAB page (channels, DRUMS, PRESETS)
- X, Y, Select: on the PRESETS page, save / load / rename a slot

## Shaping the sound

Every setting in the LAB also responds to a MIDI control change, so you can
play it by hand or automate it from your sequencer. The controls below shape
the current instrument.

Performance

- CC 7: Volume
- CC 10: Pan
- CC 1: Vibrato depth
- CC 76: Vibrato rate
- CC 77: Pitch sweep (64 = off)

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
- CC 21: Voice type

Panic and reset

- CC 120: All sound off
- CC 121: Reset controllers
- CC 123: All notes off

## Presets

The PRESETS page (press Start) holds 32 slots, each a snapshot of the whole
synth: all nine channels, the poly instrument, and the drum level.

- Up and down pick a slot; left and right jump by 8.
- X saves the current synth into the slot; Y loads it back.
- Select renames the slot (up and down change the letter, left and right move
  the cursor, Select again to finish).

Presets are written to your SD card as `sds_presets.dat`, so they survive a
power-off. If the card cannot be opened they still work for the session.

## License

Released under CC BY-NC-SA 4.0. You are free to use, change, and share sDS for
non-commercial purposes, with attribution, under the same license. Selling sDS
or mDS units is not allowed.

You can make, perform, record, and sell music with it. The license only
restricts selling the device or the software itself.

The names sDS and mDS are reserved by the author. For commercial use, get in
touch through hobbychop.com.
