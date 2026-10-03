# ChartDown Specification

**Version 0.3 (draft)** · File extension: `.chartdown` · Encoding: UTF-8

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
| `chartdown` | Version of this spec the file was written for | `0.3` |
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

An unknown name is a warning, and the note is shown to everyone so nothing
is lost. Implementations may add roles in later versions.

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

```
1:3 4:1        four beats in 4/4: three of 1, one of 4
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
| `x` | No chord (N.C.) |
| `%` | Repeat the previous bar |
| `=` | Continue the previous chord, tied across the barline |

## 8. Expression marks

Marks attach to a chord. Order inside a token:

`'` (push) · `<` `>` (hold) · chord · `:beats` · `!` and `~`

| Mark | Where | Meaning | Example |
|---|---|---|---|
| `'` | before | **Push**: play early, an eighth note ahead | `'4` |
| `<` `>` | around | **Hold**: a long sustained chord (diamond) | `<1>` |
| `!` | after | **Stab**: short, accented, cut off | `1!` |
| `~` | after | **Fermata**: hold until cued | `<1>~` |

Several marks can combine: `'4:2!`.

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
3. Number display uses `-` for minor (`6-`). Letter display uses `m` (`Em`).
4. Use flat spellings in keys of F, Bb, Eb, Ab, Db, Gb. Use sharps otherwise.

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
use       = "@use" name NL
keychg    = "@key" KEY NL
row       = [ "|:" | "|" ] bar { "|" bar } [ ":|" [ "x" N ] | "|" ] NL
bar       = { [ ending ] | cue | lyric | token }
ending    = "[" N { ("," | "-") N } "]"
cue       = "{" [ roles ":" ] text "}"
lyric     = '"' text '"'
token     = "%" | "=" | "x" | chord-token
chord-tok = [ "'" ] [ "<" ] chord [ ">" ] [ ":" beats ] { "!" | "~" }
chord     = root [ quality ] [ "/" root ]
root      = [ "b" | "#" ] DIGIT(1-7)  |  LETTER(A-G) [ "b" | "#" ]
```

## 16. Roadmap

Ideas under consideration, not yet part of the format:

- **Layers:** a separate file of MD notes that sits on top of a chart without
  changing it, anchored to section names and bar numbers.
- **Custom instruments:** declaring extra instrument roles in the header.
- **JSON form:** a standard JSON shape of a parsed chart for tools.
- Pickup bars, tempo or meter changes inside a song, rehearsal marks.

## 17. Changelog

- **0.3** `chartdown` version header, `ccli` header removed, lyric landmarks
  limited to 8 words with a parser warning, roadmap updated.
- **0.2** MD notes per instrument, per-instrument bar cues, views, minor keys,
  `feel` header, `@key` key changes, beat-count checks, unknown-directive
  warnings.
- **0.1** First draft.
