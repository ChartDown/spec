# ChartDown quick guide

This guide takes you from a blank file to a full band chart in a few minutes.
For every rule, see the [specification](../SPEC.md).

## 1. Start with a header

```
---
chartdown: 0.6
title: Open Road
key: G
tempo: 76
meter: 4/4
---
```

`key` is the home key. In the Nashville Number System, `1` is that key, so in G,
`1` = G, `4` = C, `5` = D and `6m` = Em. For a minor key write `key: Em`.

## 2. Add sections and bars

A section starts with `##`. Each line below it is one row of bars, separated by `|`.

```
## Verse 1
1 | 4 1 | 6m 5 | 4 1
```

Chords in the same bar share it evenly, so `4 1` is two beats of 4 and two of 1.
For an uneven split use beats: `1:3 4:1`.

## 3. Write chords

| You write | Meaning |
|---|---|
| `1` `4` `5` | Major chords |
| `6m` or `6-` | Minor |
| `5sus` `4add9` `1maj7` | Other qualities |
| `1/3` | Slash chord (1 over the 3rd in the bass) |
| `b7` `#4` | Chords outside the key |
| `G` `Em` `C/E` | Letters work too |

## 4. Add the music director's notes

This is what makes ChartDown different.

```
## Chorus
> MD: Open it up a little.
> MD drums: Light groove, no crashes.
> MD bass, keys: Root notes only.
```

- `> MD:` is for the whole band.
- `> MD drums:` is only for the drummer. Roles: `drums`, `bass`, `keys`, `gtr`, `vox`, `sax`, `tpt` (trumpet), `tbn` (trombone), `vln` (violin).
- Other instruments: declare them in the header, `instruments: cello, banjo`, then write `> MD cello: ...`.
- A cue on one bar: `{drums: in}` or `{bass: walk up}`.

A ChartDown app can show each player only the notes for their instrument.

## 5. Show how to play it

| You write | Meaning |
|---|---|
| `{p}` `{mf}` `{f}` `{cresc}` | Dynamics |
| `'4` | Push: play it early (an eighth note) |
| `''4` | Short push: a sixteenth note early |
| `1!` | Stab: short and accented |
| `<1>` | Hold the chord for the bar |
| `=` | Keep holding into the next bar |
| `<1>~` | Fermata: hold until cued |
| `x` | No chord |
| `1! x:3` | Stop: hit on beat 1, then three beats of rest |
| `R[ 1 3 4 #4 5 ]` | Unison riff: single notes instead of chords |
| `%` | Repeat the previous bar |

A pickup (a short bar that leads into the first full bar) starts with `[p:beats]`:
`[p:2] 5 | 1 | 4 | 5`. It is bar 0, and the full bars are counted from 1.
Say who plays a riff in an MD note: `> MD gtr, tpt: Unison riff in bar 2.`

## 6. Repeats, endings and reuse

```
|: 6m | 4 | 1 | [1] 5 :| 2x
[2] 5 4 5 5 |
```

To play a section again without retyping it:

```
## Chorus 2
> MD: A touch bigger.
@use Chorus
```

To change key from here on: `@key A`. Meter and tempo work the same way:
`@meter 6/8` and `@tempo 72`. For one bar in another meter, start it with
`[m:2/4]`. Gradual tempo changes: `{rit}`, `{accel}`, `{a tempo}`.

### Cues for one pass

Inside a repeat, put the pass before the instruments, the same way you count
the repeat:

```
|: 1 {2x drums: in} | 4 | 5 {1x,3x: stop} | 1 :| 3x
> MD 2x bass: Walk it up on the second pass.
```

### Navigation

Write the marks the band already knows, in curly braces on a bar: `{segno}`,
`{coda}`, `{to coda}`, `{fine}`, `{D.S. al coda}`, `{D.C. al fine}` and so on.

```
## Verse
{segno} 1 | 4 | 6m | 5

## Chorus
4 | 1 | 5 {to coda} | 5 {D.S. al coda}

## Outro
{coda} 4 | 5 | <1>~
```

`{to coda}` only counts after the jump back. A ChartDown app can show the full
play order.

## 7. Lyric landmarks

A short quote tells the band where they are: `1 "Every mile" | 4 1`.
Keep it to 8 words or fewer. ChartDown is not for full lyrics.

## 8. Try it

Open `playground/index.html` in your browser, pick an example and edit it. The chart
updates as you type, and warnings show the line number of anything it can't read.
