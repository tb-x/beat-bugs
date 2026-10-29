# Beat Bugs

A first music maker for young kids. The board is a garden bed with eight beats across and seven rows, each with a
creature that makes one sound. Tap a square to put a creature there, press play, and a light sweeps across the board:
every creature it reaches hops and sings its sound. Together they make a little loop that keeps playing while you add
and take away creatures.

Four places to play in (a meadow, a forest, a lake and a band of real instruments), each with its own players
and colours, plus thirteen more instruments and two singers to mix in. Any creature or instrument can go in any row, and a row can even
play a sound you record yourself. All the voices and instruments were recorded with ElevenLabs (synthesized ones as
a fallback). Play it at [tb-x.github.io/beat-bugs](https://tb-x.github.io/beat-bugs/).

It works offline and can be added to the Home Screen: the first visit stores everything it needs, and later visits
start without internet.

## The creatures

The top two rows are tall and play tunes: tap high in the square for a high note, low for a low one (five notes that
always sound nice together, so the two tunes never clash). The five rows below are the drums.

| Row | 🌼 Meadow | 🌲 Forest | 🌊 Lake | 🥁 Band | Like a… |
| --- | --- | --- | --- | --- | --- |
| 1, tall (tune) | Bee hums | Songbird tweets | Fish blows a bubble | Piano | melody |
| 2, tall (tune) | Songbird tweets | Bee hums | Frog rib-bits | Guitar | second tune |
| 3 | Cricket chirps | Woodpecker knocks | Dragonfly flutters | Hi-hat | hi-hat |
| 4 | Ladybird claps | Squirrel cracks a nut | Beaver slaps the water | Snare drum | clap |
| 5 | Frog rib-bits | Owl hoots | Duck quacks | Bass guitar | bass |
| 6 | Beetle booms | Bear drums on a log | Turtle plops in | Big drum | big drum |
| 7 | Dragonfly flutters | Cricket chirps | Ladybird claps | Tambourine | shaker |

Each place has five creatures of its own, so rows 2 and 7 borrow one from another place (the band's come from the
extra instruments). The second tune row and the bottom drum row start empty on the first visit, and a garden saved by the
five-row version keeps its pattern, creatures and recordings, with the two new rows added empty.

Below the places, the picker has more instruments that don't have a place of their own, one row each:

- 🎵 xylophone, tambourine, triangle and maracas
- 🎻 guitar, violin, cello, double bass and harp
- 🎺 trumpet, trombone, saxophone and flute
- 🎤 the microphone (my own sound), a man singing "la" and a woman singing "la"

Every instrument and singer has a face and hops on its beat like the creatures. The singers sing the tune in a tune
row like any instrument.

## How it plays

- Tap an empty square to put a creature there. It pops in and sings once so you hear what you added.
- Tap a creature to send it away. In a tune row, tapping at a different height moves it to that note.
- The flower, tree, wave and drum buttons switch the whole board to the meadow, forest, lake or band: new players
  and colours, same beat.
- **Mix and match:** tap the creature at the start of a row (it has a little yellow swap badge) to open the picker,
  with every creature, instrument and singer, plus a microphone. Tap one to hear it; it takes over that row and keeps the
  row's beat. The green tick closes the picker. A frog playing the tune or a bear on the hi-hat is allowed, and funny.
- **My own sound:** pick the microphone, then tap the big red button and make a sound ("boom!", a clap, a laugh). It
  stops by itself when you go quiet, or tap again to stop. The silence is trimmed off and the level evened out, and
  that row now plays your sound, as a tune in the top two rows. The play button hears it again, the bin deletes it.
- The big green button plays and stops the loop. The light shows which beat is playing, and the creatures on it hop,
  open their mouths and send up a note.
- The snail, rabbit and cheetah buttons choose slow, medium or fast. The speed changes straight away, even mid-loop.
- The dice fills the garden with a surprise pattern and starts playing.
- The bin clears the garden (the creatures fly away). For five seconds it turns into a blue "bring them back" button.
- The garden, the place, the creature in each row, the recordings and the speed are remembered for next time. On the
  first visit a simple beat is already there, so pressing play does something straight away.
- Nothing to read and nothing to get wrong: any pattern is a song.

## The microphone and privacy

- The browser asks before the microphone is first used; a parent says yes. It only works on https (GitHub Pages) or
  `localhost`, not on the laptop's home-network address. Where it can't work, the microphone card is greyed out with
  a 🚫.
- A recording is at most 1.5 seconds. It **stays on the device**: it is kept in the browser's local storage (about 16 kHz,
  roughly 15–50 KB a row), is never uploaded or sent anywhere, and the game's telemetry only notes that a recording was
  made and how long it was. The bin in the picker deletes it, and so does clearing the browser's site data.
- The microphone is switched off as soon as each recording stops. (On iPhones the sound can get quieter while the
  microphone is on; switching it off straight away is meant to bring the volume back. Worth checking on a real phone.)

## Running it

Serve the folder and open it in a browser:

```bash
python -m http.server 5233
```

Then go to `http://localhost:5233/`. It's also on the root server (port 5200) at `/beat-bugs/`. Opening `index.html`
straight from disk works too (without the microphone). Adding `?tbxtest` to the address keeps it silent and never
touches the microphone (a made-up "boo" stands in for a recording), for testing.

## Tuning knobs

All at the top of the script in `index.html`:

- `STEPS`: beats across the board (8, one bar of eighth notes). `ROW_COUNT`: seven rows, the first `TUNE_ROWS` (2)
  of them tune rows, each `TUNE_TALL` (1.9) times as tall as a drum row. On a phone held sideways the drum rows come
  out about 38 px tall; a smaller `TUNE_TALL` gives them more room but makes the tune notes harder to hit.
- `TEMPOS`: the three speeds (76, 100 and 128 beats a minute) and their pictures.
- `MELODY`: the tune rows' five notes, bottom to top (G, A, C, D, E).
- `CREATURES`: every creature, with its clip and how it's played (see Sounds below).
- `SCENES`: the four places, their colours and which player goes in each row (the picker groups the creatures by
  their own place, so a borrowed one still shows up only once). `EXTRAS`: the picker rows of instruments that have
  no place of their own (🎵, 🎻, 🎺). `VOICES`: the 🎤 row, the microphone and the two singers.
- `SURPRISES`: the dice patterns, one line of text per row (`x` = a creature, `0`-`4` = a tune note, `.` = empty).
- `START_PATTERN`: the beat already there on the first visit.
- `UNDO_MS`: how long the "bring them back" button stays (5 s).
- `LOOKAHEAD`, `TICK_MS`: how far ahead the sounds are queued (0.15 s, topped up every 25 ms).
- `USE_CLIPS`, `ASSET_V`, `WHOOSH_TRIM`: the recorded clips.
- `REC_MAX`, `REC_KEEP`, `REC_SILENCE`, `REC_RATE`: the microphone (2 s at most, 1.5 s kept, stops 0.45 s after the
  voice goes quiet, stored at about 16 kHz).

## Ideas for the next version

- More places (a kitchen with pots and spoons, a farm, the sea).
- Two bars (16 beats) for older kids.
- A "copy me" game: a creature plays a short rhythm and you build it on the board.

## Sounds

The creatures' voices and the "flying away" sound are short recordings made with ElevenLabs (below). Each loads as
`assets/<name>.mp3?v=ASSET_V`, and any sound whose clip can't load falls back to its synthesized Web Audio version
(the Blast Off pattern). `USE_CLIPS = false` plays everything synthesized. There's no music track: the kids' pattern
is the music. Everything goes through a limiter at the end, so seven rows landing on the same beat get squashed a
little instead of crackling.

Three takes of each were made (on 2026-10-24, and on 2026-10-29 for the strings, brass, woodwind, maracas and
singers) and the best one kept. They were picked by measuring, not by ear (sound
stays off while testing): a quick start, a clean decay and, for the singers, a steady note. Each creature in
`CREATURES` tidies its clip up in the browser instead of re-recording:

- `cut` plays only part of a clip, with a 20 ms fade at the end. Several takes had more than one hit spread out (the
  cricket's chirps, the duck's two quacks) or a gap at the start (the songbird, the woodpecker, the turtle), so only one
  short hit is used.
- `trim` evens out the volumes. The beetle, clap, bear and beaver came out very quiet (peaks around −20 to −25 dB) and
  are turned up a lot; the frog, fish and duck came out loud and are turned down. Mixed, every clip peaks at about
  −9 dB (the knocks and cracks −4 to −8 dB, which sounds about as loud for such short hits).
- `base` is the note a singer's or instrument's clip is on, measured from the clip (MIDI: bee 64.65, songbird 87.1,
  owl 60.42, fish 70.3, piano 72.1, bass guitar 41.7, guitar 55.1, xylophone 82.1, trumpet 68.9, triangle 70.5,
  violin 81.05, cello 54.5, double bass 35.8, harp 73.07, trombone 47.35, saxophone 60.95, flute 72.16, man singing
  57.8, woman singing 67.2), and `octave` moves the tune up or down for very high or low ones (the bass and trombone
  play two octaves down, the double bass three). The 2026-10-29 notes were measured twice, by autocorrelation and by a
  harmonic product spectrum, which agreed within 0.3 semitones except for the trombone, where they disagreed by
  exactly an octave; an octave error only moves the register, never puts a note out of tune. The tune rows play
  each note by speeding the clip up or slowing it down, so with these settings everything plays at 0.63× to 1.93× its
  speed. The instruments all hold a steady note, so the piano, guitar, xylophone, trumpet and the 2026-10-29
  instruments play the cleanest tunes; the singers wobble a little more (they have some vibrato). The bee and
  the owl hold a steady note; the songbird's tweet slides down about six semitones and the fish's bubble rises, so their
  tunes are rougher. Creatures without a note (and recordings) play the tune around their own sound.

## Audio credits

All made with the ElevenLabs **sound effects** model (`text_to_sound_effects`, not marked beta) on the paid
**Creator** plan, on 2026-10-24 (the last ten rows on 2026-10-29). The singers are sound effects too, not a voice
model, so no voice was cloned. Paid-plan audio can be used commercially and needs no credit line. The audio is
part of this game only and is **not licensed for reuse**.

| File | What it is | Prompt | Length | Take kept |
| --- | --- | --- | --- | --- |
| `assets/bug-beetle.mp3` | the beetle's boom | "single deep soft kick drum hit, warm and round, cartoon, very short, no reverb, starts immediately" | 0.5 s | 3 of 3 |
| `assets/bug-ladybird.mp3` | the ladybird's clap | "one single bright hand clap, dry, no reverb, starts immediately" | 0.5 s | 1 of 3 |
| `assets/bug-cricket.mp3` | the cricket's chirp | "one short cricket chirp, three quick bright clicks, close up, dry, starts immediately" | 0.5 s (first 70 ms used) | 2 of 3 |
| `assets/bug-frog.mp3` | the frog's rib-bit | "one cute cartoon frog ribbit, low and bouncy, two quick croaks, dry, starts immediately" | 0.6 s (0.35 s used) | 1 of 3 |
| `assets/bug-bee.mp3` | the bee's hummed note | "a cartoon bumblebee humming one steady musical note, cute and buzzy, no pitch change, starts immediately" | 0.6 s (0.32 s used) | 1 of 3 |
| `assets/sfx-whoosh.mp3` | creatures flying away (clear, dice, places) | "a little swarm of cartoon bugs fluttering away quickly, light whoosh of wings, playful" | 1 s | 2 of 3 |
| `assets/forest-bird.mp3` | the songbird's tweet | "a little songbird whistling one clear steady musical note, a single short tweet, no pitch change, close up, dry, starts immediately" | 0.6 s (0.2 s used) | 1 of 3 |
| `assets/forest-woodpecker.mp3` | the woodpecker's knock | "a woodpecker tapping on a tree three times very fast, dry knocks on wood, close up, starts immediately" | 0.5 s (90 ms used) | 2 of 3 |
| `assets/forest-squirrel.mp3` | the squirrel cracking a nut | "a single sharp crack of a nut being cracked open, cartoon, dry, starts immediately" | 0.5 s (80 ms used) | 2 of 3 |
| `assets/forest-owl.mp3` | the owl's hoo | "one soft owl hoot, a single hoo, warm and low, close up, dry, starts immediately" | 0.8 s (0.25 s used) | 3 of 3 |
| `assets/forest-bear.mp3` | the bear's log drum | "one deep hollow log drum hit, a big wooden thump, warm, dry, starts immediately" | 0.5 s (0.35 s used) | 3 of 3 |
| `assets/lake-fish.mp3` | the fish's bubble | "a single cute bubble bloop underwater, one round musical blip, starts immediately" | 0.5 s (0.16 s used) | 1 of 3 |
| `assets/lake-dragonfly.mp3` | the dragonfly's flutter | "a dragonfly's wings buzzing in one short quick flutter, close up, dry, starts immediately" | 0.5 s (0.14 s used) | 3 of 3 |
| `assets/lake-beaver.mp3` | the beaver's tail slap | "a beaver slapping its tail on the water once, a short sharp splash slap, starts immediately" | 0.5 s (0.2 s used) | 2 of 3 |
| `assets/lake-duck.mp3` | the duck's quack | "one single cartoon duck quack, short, close up, dry, starts immediately" | 0.5 s (first quack, 0.13 s) | 1 of 3 |
| `assets/lake-turtle.mp3` | the turtle's plop | "a pebble plopping into a pond, one deep round plop, starts immediately" | 0.5 s (80 ms used) | 1 of 3 |
| `assets/band-piano.mp3` | the piano (C5) | "a single piano note, middle C, clean and clear, one key struck, no reverb, starts immediately" | 0.8 s (0.45 s used) | 3 of 3 |
| `assets/band-hihat.mp3` | the hi-hat | "a drummer hitting a closed hi-hat cymbal once with a stick, tight crisp tick, loud, close microphone, studio drum sample" | 0.5 s (0.12 s used) | 5 of 6 |
| `assets/band-snare.mp3` | the snare drum | "a drummer hitting a snare drum once with a stick, loud sharp crack with snare rattle, close microphone, studio drum sample" | 0.5 s (0.25 s used) | 5 of 6 |
| `assets/band-bass.mp3` | the bass guitar (F2) | "one single bass guitar note, plucked, deep and round, steady pitch, dry, starts immediately" | 0.6 s (0.35 s used) | 2 of 3 |
| `assets/band-kick.mp3` | the big drum | "one bass drum kick, deep and punchy, dry, no reverb, starts immediately" | 0.5 s (0.25 s used) | 2 of 3 |
| `assets/band-guitar.mp3` | the guitar (G3) | "one single acoustic guitar note, plucked string, clear and warm, steady pitch, dry, starts immediately" | 0.8 s (0.45 s used) | 1 of 3 |
| `assets/band-xylophone.mp3` | the xylophone (A♯5) | "one single xylophone note, a bright wooden bar struck once with a mallet, dry, starts immediately" | 0.6 s (0.22 s used) | 3 of 3 |
| `assets/band-trumpet.mp3` | the trumpet (A4) | "one single short trumpet note, bright and steady pitch, dry, starts immediately" | 0.6 s (0.3 s used) | 1 of 3 |
| `assets/band-tambourine.mp3` | the tambourine | "one tambourine hit, a single jingly shake, dry, starts immediately" | 0.5 s (0.13 s used) | 1 of 3 |
| `assets/band-triangle.mp3` | the triangle (B♭4) | "one triangle ding, a single bright metallic ring, starts immediately" | 0.8 s (0.3 s used) | 3 of 3 |
| `assets/band-maracas.mp3` | the maracas | "one maracas shake, a single quick rattle, dry, starts immediately" | 0.5 s (0.2 s used) | 1 of 3 |
| `assets/band-violin.mp3` | the violin (A5) | "one single violin note, bowed, clear and bright, steady pitch, short, dry, starts immediately" | 0.8 s (0.35 s used) | 2 of 3 |
| `assets/band-cello.mp3` | the cello (about F♯3) | "one single cello note, bowed, warm and deep, steady pitch, short, dry, starts immediately" | 0.8 s (0.45 s used) | 2 of 3 |
| `assets/band-doublebass.mp3` | the double bass (about C2) | "one single double bass note, plucked pizzicato, deep and round, steady pitch, dry, starts immediately" | 0.6 s (0.4 s used) | 1 of 3 |
| `assets/band-harp.mp3` | the harp (C♯5) | "one single harp string plucked, a clear bright note, steady pitch, dry, starts immediately" | 0.8 s (0.45 s used) | 3 of 3 |
| `assets/band-trombone.mp3` | the trombone (B2 or B3) | "one single short trombone note, warm and brassy, steady pitch, no slide, dry, starts immediately" | 0.6 s (0.4 s used) | 1 of 3 |
| `assets/band-saxophone.mp3` | the saxophone (C♯4) | "one single short saxophone note, warm and smooth, steady pitch, dry, starts immediately" | 0.6 s (0.45 s used) | 2 of 3 |
| `assets/band-flute.mp3` | the flute (C5) | "one single short flute note, soft and clear, steady pitch, dry, starts immediately" | 0.6 s (0.45 s used) | 2 of 3 |
| `assets/sing-man.mp3` | a man singing "la" (B♭3) | "a man singing one single short note "laa", warm baritone voice, steady pitch, clear, dry, no music, starts immediately" | 0.8 s (0.5 s used) | 2 of 3 |
| `assets/sing-woman.mp3` | a woman singing "la" (G4) | "a woman singing one single short note "laa", clear soprano voice, steady pitch, dry, no music, starts immediately" | 0.8 s (0.5 s used) | 1 of 3 |

The meadow's 18 takes cost about 130 credits. The forest's and lake's 30 takes (about 16 s of audio) should have cost
about 200 more; that's an estimate from their length, because other work on the account the same day makes the
balance impossible to split. The band's 36 takes cost about 250 credits (measured on the balance): the first three
hi-hat and snare takes, with shorter prompts ("one closed hi-hat hit, crisp and short, dry, starts immediately", "one
snare drum hit, punchy and crisp, dry, no reverb, starts immediately"), came out almost silent (−55 to −75 dB), so
both were made again with the prompts above. Even those came out quiet (about −40 dB) and are turned up about 30 dB.
The 2026-10-29 batch was 30 takes (about 20 s of audio) and cost about 240 credits by the balance. The harps all came
out quiet (about −22 to −25 dB) and are turned up; the woman's "la" is spikier than the man's, so it's set about 4 dB
louder than its peak alone suggests.
