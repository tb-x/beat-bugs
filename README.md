# Beat Bugs

A first music maker for young kids. The board is a garden bed with eight beats across and one row for each bug. Tap a
square to put a bug there, press play, and a light sweeps across the board: every bug it reaches hops and sings its
sound. Together they make a little loop that keeps playing while you add and take away bugs.

One board, no levels, bug voices recorded with ElevenLabs (synthesized ones as a fallback). Play it at
[tb-x.github.io/beat-bugs](https://tb-x.github.io/beat-bugs/).

It works offline and can be added to the Home Screen: the first visit stores everything it needs, and later visits
start without internet.

## The bugs

| Row | Bug | Sound |
| --- | --- | --- |
| top, tall | Bee | hums a note: tap high in the square for a high note, low for a low one (five notes that always sound nice together) |
| 2 | Cricket | a quick chirp (like a hi-hat) |
| 3 | Ladybird | a clap |
| 4 | Frog | a low "rib-bit" (like a bass) |
| 5 | Beetle | a deep boom (like a big drum) |

## How it plays

- Tap an empty square to put a bug there. It pops in and sings once so you hear what you added.
- Tap a bug to send it away. In the bee row, tapping at a different height moves the bee to that note.
- Tap the bug at the start of a row to hear its sound.
- The big green button plays and stops the loop. The light shows which beat is playing, and the bugs on it hop, open
  their mouths and send up a note.
- The snail, rabbit and cheetah buttons choose slow, medium or fast. The speed changes straight away, even mid-loop.
- The dice fills the garden with a surprise pattern and starts playing.
- The bin clears the garden (the bugs fly away). For five seconds it turns into a blue "bring them back" button.
- The garden and the speed are remembered for next time. On the first visit a simple beat is already there, so
  pressing play does something straight away.
- Nothing to read and nothing to get wrong: any pattern is a song.

## Running it

Serve the folder and open it in a browser:

```bash
python -m http.server 5233
```

Then go to `http://localhost:5233/`. It's also on the root server (port 5200) at `/beat-bugs/`. Opening `index.html`
straight from disk works too. Adding `?tbxtest` to the address keeps it silent (for testing).

## Tuning knobs

All at the top of the script in `index.html`:

- `STEPS`: beats across the board (8, one bar of eighth notes).
- `TEMPOS`: the three speeds (76, 100 and 128 beats a minute) and their pictures.
- `MELODY`: the bee row's five notes, bottom to top (G, A, C, D, E).
- `ROWS`: which bugs there are and in what order.
- `SURPRISES`: the dice patterns, one line of text per bug (`x` = a bug, `0`-`4` = a bee note, `.` = empty).
- `START_PATTERN`: the beat already there on the first visit.
- `UNDO_MS`: how long the "bring them back" button stays (5 s).
- `LOOKAHEAD`, `TICK_MS`: how far ahead the sounds are queued (0.15 s, topped up every 25 ms).
- `USE_CLIPS`, `ASSET_V`, `BEE_BASE`, `CLIP_TRIM`, `CLIP_CUT`: for the recorded clips (see Sounds below).

## Ideas for the next version

- More bug rows that scroll in, or a second garden (a pond with frogs and fish, a kitchen with pots and spoons).
- Two bars (16 beats) for older kids.
- A "copy me" game: a bug plays a short rhythm and you build it on the board.

## Sounds

The bugs' voices and the "flying away" sound are short recordings made with ElevenLabs (below). Each loads as
`assets/<name>.mp3?v=ASSET_V`, and any sound whose clip can't load falls back to its synthesized Web Audio version
(the Blast Off pattern). `USE_CLIPS = false` plays everything synthesized. There's no music track: the kids' pattern
is the music.

Three takes of each were made on 2026-10-24 and the best one kept. They were picked by measuring, not by ear (sound
stays off while testing): a quick start, a clean decay and, for the bee, a steady note. Two knobs tidy them up in the
browser instead of re-recording:

- `CLIP_CUT` plays only part of a clip, with a 20 ms fade at the end. The cricket take had several chirps spread over
  0.4 s, so only its first chirp (70 ms) is used. The frog and the bee are cut to 0.35 s and 0.32 s so they stay on
  their beat.
- `CLIP_TRIM` evens out the volumes. The beetle and the clap came out very quiet (peaks around −20 dB) and are turned
  up about 15 dB. The frog came out loud and is turned down. Mixed, every clip peaks at about −10 dB.

**The bee's tune.** All five bee notes come from one clip played faster. The hum measures MIDI 64.65 (just above E4,
median over the clip, steady within about ±0.2 of a semitone), which is set in `BEE_BASE`. The lanes were moved down
to G, A, C, D, E (`MELODY` 67–76), so the clip plays at 1.15× to 1.93× its speed rather than up to 2.8×. If the top
notes sound chipmunky on a real device, record a second, higher bee and use it for the top lanes.

## Audio credits

All made with the ElevenLabs **sound effects** model (`text_to_sound_effects`, not marked beta) on the paid
**Creator** plan, on 2026-10-24. Paid-plan audio can be used commercially and needs no credit line. The audio is
part of this game only and is **not licensed for reuse**.

| File | What it is | Prompt | Length | Take kept |
| --- | --- | --- | --- | --- |
| `assets/bug-beetle.mp3` | the beetle's boom | "single deep soft kick drum hit, warm and round, cartoon, very short, no reverb, starts immediately" | 0.5 s | 3 of 3 |
| `assets/bug-ladybird.mp3` | the ladybird's clap | "one single bright hand clap, dry, no reverb, starts immediately" | 0.5 s | 1 of 3 |
| `assets/bug-cricket.mp3` | the cricket's chirp | "one short cricket chirp, three quick bright clicks, close up, dry, starts immediately" | 0.5 s (first 70 ms used) | 2 of 3 |
| `assets/bug-frog.mp3` | the frog's rib-bit | "one cute cartoon frog ribbit, low and bouncy, two quick croaks, dry, starts immediately" | 0.6 s (0.35 s used) | 1 of 3 |
| `assets/bug-bee.mp3` | the bee's hummed note | "a cartoon bumblebee humming one steady musical note, cute and buzzy, no pitch change, starts immediately" | 0.6 s (0.32 s used) | 1 of 3 |
| `assets/sfx-whoosh.mp3` | bugs flying away (clear, dice) | "a little swarm of cartoon bugs fluttering away quickly, light whoosh of wings, playful" | 1 s | 2 of 3 |

The 18 takes cost about 130 credits.
