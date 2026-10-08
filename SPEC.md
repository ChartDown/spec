# ChartDown Specification

**Version 0.5 (draft)** · File extension: `.chartdown` · Encoding: UTF-8

This specification is released under CC0 1.0 Universal. Anyone may implement,
extend, or redistribute it, with no permission needed.

ChartDown is an independent design. The only thing it shares with other chart
formats is the Nashville Number System, a public musical convention.

## 1. Goals

1. Easy to type, and easy to read as raw text.
2. Describe what a band needs: the form, the bars, the chords, and how the
   music director (MD) wants every part played.
3. **Make MD directions readable in every section, for every player.** A
   drummer can read only the drum notes. The MD can read only the cues.
4. Work for any song: major or minor, any meter, key changes.
5. Work with numbers or letters, and convert between them.
6. Stay small. Unknown things are warnings, never crashes.

## 2. File structure

A file has an optional header, then one or more sections.

```
---
title: Song Title
key: G
---

## Section name
> MD: A note for the band.
> MD drums: A note only for the drummer.
1 | 4 | 5 | 1
```

Sections appear in **play order**. If a section plays twice, write it twice
(use `@use` to avoid retyping, see 5.2).

Blank lines are ignored. A line beginning with `//` is a comment and is not
rendered.

## 3. Header

The header sits between two lines of three dashes, at the very top of the file.
Each line is `name: value`. Names are case-insensitive.

| Name | Meaning | Example |
|---|---|---|
| `chartdown` | Version of this spec the file was written for | `0.5` |
| `instruments` | Extra instrument roles used in this file (see 4.1) | `cello, banjo` |
| `title` | Song title | `Open Road` |
| `artists` | Writers or artists | `A, B` |
| `key` | Home key. Add `m` for a minor key | `G`, `Bb`, `F#`, `Em` |
| `tempo` | Beats per minute | `72` |
| `meter` | Time signature | `4/4`, `3/4`, `6/8`, `12/8` |
| `feel` | Groove or style, free text | `Slow shuffle` |

Unknown names are kept and may be shown by a renderer. If `key` is missing,
`C` is assumed. If `meter` is missing, `4/4` is assumed.

**Minor keys.** In a minor key, `1` is the tonic. The tonic chord is `1m` and
the relative major is `b3`. Example: in `key: Em`, `1m` is Em and `b3` is G.

## 4. Sections and notes

- `## Name` starts a section. The name is free text.
- `> text` adds a note to the current section.
- `> MD: text` is a **music director note** for the whole band.
- `> MD <instruments>: text` is an MD note for specific players.

```
## Chorus
> MD: Open it up a little. Don't go all out yet.
> MD drums: Light groove. Hi-hat or ride, no crashes.
> MD bass, keys: Root notes only.
```

### 4.1 Instruments (roles)

`<instruments>` is one or more names separated by commas, `/`, `&` or spaces.

| Role | Accepted names |
|---|---|
| everyone | `all`, `band`, `everyone` (also the default when no name is given) |
| drums | `drums`, `drum`, `perc` |
| bass | `bass` |
| keys | `keys`, `piano`, `pad`, `synth` |
| guitar | `gtr`, `guitar`, `eg`, `ag` |
| vocals | `vox`, `vocals`, `lead`, `bgv`, `bgvs` |
| saxophone | `sax`, `saxophone` |
| trumpet | `tpt`, `trumpet` |
| trombone | `tbn`, `trombone` |
| violin | `vln`, `violin` |

**Custom instruments.** Any other instrument can be used if the header declares
it in `instruments:`, a comma-separated list of lowercase ids (letters, digits
and `-`). A declared id works everywhere a role does: `> MD cello: ...` and
`{cello: in}`. Renderers show it with its id capitalized unless they know a
better label.

An undeclared, unknown name is a warning, and the note is shown to everyone so
nothing is lost.

### 4.2 Views (what players read)

A renderer SHOULD offer these views, because they are the point of the format:

- **Everyone:** all notes, with a visible tag for notes meant for specific
  players.
- **Per instrument:** only notes for that instrument, plus notes for everyone.
- **MD cues only:** for each section, the MD notes, plus every bar-level
  dynamic and cue (see 9), listed by bar number, with no chord grid.

Notes marked `MD` SHOULD be visually distinct from plain `>` notes.

## 5. Bars

Any other line inside a section is a **row** of bars. A row is one printed line
of the chart. Bars in a row are separated by `|`. The leading and trailing `|`
are optional.

```
1 | 4 | 5 | 1
```

### 5.1 Chords in a bar

Chords inside one bar are separated by spaces and **share the bar evenly**.

```
1 4 | 6m 5
```

To split unevenly, add `:beats` to a chord. Beats may be fractional.
Beats are counted in the units of the meter's top number (4 in 4/4, 3 in 3/4,
6 in 6/8, 12 in 12/8). If any chord in a bar has `:beats`, the beats SHOULD add
up to the meter's top number. A renderer SHOULD warn when they do not.

Tokens in the same bar that have no `:beats` share the beats that are left over
evenly. This also applies to rests (`x`) and riffs (see 7 and 7.1).

```
1:3 4:1        four beats in 4/4: three of 1, one of 4
1! x:3         one beat of 1 (stab), then three beats of rest
```

### 5.2 Reusing a section

`@use Name` as the only content line of a section copies the rows of the
earlier section called `Name`. The new section keeps its own notes. If the key
changed since the original, the copied rows use the current key.

```
## Chorus 2
> MD: A touch bigger.
@use Chorus
```

### 5.3 Pickup bars

A bar that starts with `[p:N]` is a **pickup** (anacrusis): an incomplete bar
of `N` beats that leads into the first full bar. `N` counts in the same units
as `:beats` (see 5.1) and MUST be greater than 0 and less than the meter's top
number.

- A pickup MUST be the first bar of a section. Anywhere else it is a warning.
- The chords in a pickup share its `N` beats. If any has `:beats`, they SHOULD
  add up to `N` (not to the meter's top number).
- A pickup is **bar 0**. The first full bar is bar 1, and the pickup is not
  counted in a section's bar count.
- A renderer SHOULD draw a pickup narrower than a full bar, in proportion to
  `N`.

```
## Intro
> MD gtr: Pickup on beat 3.
[p:2] 5 | 1 | 4 | 5 | 1
```

## 6. Chords

A chord is `root` + optional `quality` + optional `/bass`.

**Root**
- Number: `1` to `7`, optionally with `b` or `#` in front (`b7`, `#4`).
- Letter: `A` to `G`, optionally with `b` or `#` after (`Bb`, `F#`).

A file may mix numbers and letters. Numbers are relative to the current key.

**Quality** (written directly after the root)

| Text | Meaning |
|---|---|
| (none) | major |
| `m` or `-` | minor (`-` is normalized to `m`) |
| `7`, `maj7`, `m7` | sevenths |
| `sus`, `sus4`, `sus2` | suspended |
| `add9`, `6`, `9`, `dim`, `aug` | other qualities |

Qualities are passed through as written. A renderer should not reject
qualities it does not recognize.

**Slash chords:** `1/3`, `G/B`. The bass note uses the same number or letter
rules as the root.

**Examples:** `1`, `6m`, `5sus`, `4add9`, `b7`, `1/3`, `Em`, `Bb/D`.

## 7. Special bars and rests

| Token | Meaning |
|---|---|
| `x` | No chord (N.C.) for a full bar |
| `x:beats` | No chord for part of a bar (a partial rest or stop), for example `1! x:3` |
| `R[ ... ]` | Unison riff: a line played in single notes (see 7.1) |
| `%` | Repeat the previous bar |
| `=` | Continue the previous chord, tied across the barline |

### 7.1 Unison riffs

`R[ notes ]` marks a line that the band plays in unison, as single notes
instead of chords. It is a token like a chord: it shares its bar with the other
tokens, or takes `:beats` after the closing bracket.

Each note is a root as in 6 (a scale degree such as `3` or `#4`, or a letter
such as `Bb`), with no quality and no bass. A note may take `:beats`, and `x`
inside the brackets is a rest. Notes without `:beats` share the riff's beats
evenly.

```
> MD gtr, tpt: Unison riff in bar 2.
1 | R[ 1 3 4 #4 5 ] | 1 | R[ 5:1 b7:1 x:2 ]
```

A riff says what is played, not who plays it. Name the players in an MD note or
a cue (see 4 and 9). Octave and exact rhythm are outside the scope of the
format.

## 8. Expression marks

Marks attach to a chord. Order inside a token:

`'` or `''` (push) · `<` `>` (hold) · chord · `:beats` · `!` and `~`

| Mark | Where | Meaning | Example |
|---|---|---|---|
| `'` | before | **Push**: play early, an eighth note ahead | `'4` |
| `''` | before | **Short push**: play early, a sixteenth note ahead | `''4` |
| `<` `>` | around | **Hold**: a long sustained chord (diamond) | `<1>` |
| `!` | after | **Stab**: short, accented, cut off | `1!` |
| `~` | after | **Fermata**: hold until cued | `<1>~` |

Pushes are note values (eighth, sixteenth), whatever the meter. Several marks
can combine: `'4:2!`.

## 9. Dynamics and cues

Text in curly braces `{ }` attaches to the bar it appears in.

- **Dynamics:** `{pp}`, `{p}`, `{mp}`, `{mf}`, `{f}`, `{ff}`, `{cresc}`,
  `{decresc}`. Renderers draw these as dynamic marks.
- **Cues for everyone:** any other text, for example `{watch me}`.
- **Cues for one instrument:** `{instrument: text}`, using the roles in 4.1.

```
{p} 1 {drums: in} | 4 | 5 {bass: walk up} | 1
```

A quoted string `"like this"` attaches a **lyric landmark** to the bar. Keep it
short: a few words that tell the band where they are. A landmark SHOULD be at
most **8 words**, and a parser SHOULD warn when it is longer (see 14).

```
{mf} 6m 5 4 1 "You are good" | 6m 5 4 1
```

## 10. Repeats and endings

| Syntax | Meaning |
|---|---|
| `\|:` at the start of a row | Repeat starts at the first bar |
| `:\|` at the end of a row | Repeat ends at the last bar |
| `:\| x3` | Play the repeated bars three times in total (default is 2) |
| `[1]` at the start of a bar | First ending |
| `[2]` at the start of a bar | Second ending |
| `[1,2]` | An ending used on several passes |

```
|: 1 | 4 | 1 | [1] 5 :| x2
[2] 5 4 5 1 |
```

## 11. Key changes

`@key X` on its own line changes the key from the next row onward, and stays
in effect until the next `@key`. Numbers after it are relative to the new key.
Renderers SHOULD mark the change.

```
## Last chorus
> MD: Up a whole step. Count it in on the last bar.
@key E
1 | 4 | 5 | 1
```

Add `m` for a minor key: `@key Am`.

## 12. Numbers and letters

A renderer SHOULD be able to show a chart in either style.

1. Convert every chord to a scale degree relative to the current key.
2. To show letters, apply the display key (the written key unless transposed).
   A transposed display moves key changes by the same interval.
3. Riff notes (see 7.1) convert the same way as chord roots.
4. Number display uses `-` for minor (`6-`). Letter display uses `m` (`Em`).
5. Use flat spellings in keys of F, Bb, Eb, Ab, Db, Gb. Use sharps otherwise.

Degree to semitone (major scale): 1=0, 2=2, 3=4, 4=5, 5=7, 6=9, 7=11.
`b` lowers by one semitone, `#` raises by one.

## 13. Errors

A parser MUST NOT stop on bad input. It SHOULD:

- skip what it cannot read,
- report a warning with the line number,
- keep rendering everything else.

## 14. Lyrics and rights

ChartDown is for structure, not for storing full lyrics. Quoted landmarks are
intended to be short (8 words at most, see 9), and parsers SHOULD warn above
that limit. Users are responsible for respecting the rights of
songwriters and publishers, and for checking their own license (for example
CCLI) before copying lyrics into any tool.

## 15. Grammar (informal)

```
file      = [ header ] { section }
header    = "---" NL { key ":" value NL } "---" NL
section   = "##" name NL { note | use | keychg | row | comment }
note      = ">" [ "MD" [ roles ] ":" ] text NL
roles     = role { ("," | "/" | "&" | " ") role }
role      = standard role name | id declared in the instruments header
use       = "@use" name NL
keychg    = "@key" KEY NL
row       = [ "|:" | "|" ] bar { "|" bar } [ ":|" [ "x" N ] | "|" ] NL
bar       = [ pickup ] { ending | cue | lyric | token }
pickup    = "[p:" beats "]"
ending    = "[" N { ("," | "-") N } "]"
cue       = "{" [ roles ":" ] text "}"
lyric     = '"' text '"'
token     = "%" | "=" | rest | riff | chord-tok
rest      = "x" [ ":" beats ]
riff      = "R[" riff-note { riff-note } "]" [ ":" beats ]
riff-note = ( root | "x" ) [ ":" beats ]
chord-tok = [ "''" | "'" ] [ "<" ] chord [ ">" ] [ ":" beats ] { "!" | "~" }
chord     = root [ quality ] [ "/" root ]
root      = [ "b" | "#" ] DIGIT(1-7)  |  LETTER(A-G) [ "b" | "#" ]
```

## 16. Roadmap

Ideas under consideration, not yet part of the format:

- **Navigation:** segno, coda, D.S. and D.C. for compact one-page charts. It
  needs bar-level markers, an "al coda" jump that only happens on the second
  pass, and rules for how a renderer expands the jumps.
- **Tempo and meter changes inside a song,** and rehearsal marks.
- **Layers:** a separate file of MD notes that sits on top of a chart without
  changing it, anchored to section names and bar numbers.
- **JSON form:** a standard JSON shape of a parsed chart for tools.

## 17. Changelog

- **0.5** Pickup bars (`[p:N]`, 5.3). Partial rests (`x:beats`) and the rule
  that tokens without `:beats` share what is left of the bar (5.1). Sixteenth
  push (`''`). Unison riffs (`R[ ... ]`, 7.1).
- **0.4** New standard roles: saxophone (`sax`), trumpet (`tpt`), trombone
  (`tbn`), violin (`vln`). New `instruments` header to declare custom roles.
- **0.3** `chartdown` version header, `ccli` header removed, lyric landmarks
  limited to 8 words with a parser warning, roadmap updated.
- **0.2** MD notes per instrument, per-instrument bar cues, views, minor keys,
  `feel` header, `@key` key changes, beat-count checks, unknown-directive
  warnings.
- **0.1** First draft.
