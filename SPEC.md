# ChartDown Specification

**Version 0.8 (draft)** · File extension: `.chartdown` · Encoding: UTF-8

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
(use `@use` to avoid retyping, see 5.2), or jump back to it with the navigation
marks in 10.1.

Blank lines are ignored. A line beginning with `//` is a comment and is not
rendered.

## 3. Header

The header sits between two lines of three dashes, at the very top of the file.
Each line is `name: value`. Names are case-insensitive.

| Name | Meaning | Example |
|---|---|---|
| `chartdown` | Version of this spec the file was written for | `0.8` |
| `instruments` | Extra instrument roles used in this file (see 4.1) | `cello, banjo` |
| `title` | Song title | `Open Road` |
| `artists` | Writers or artists | `A, B` |
| `key` | Home key. Add `m` for a minor key | `G`, `Bb`, `F#`, `Em` |
| `tempo` | Beats per minute at the start (see 11.3 for changes) | `72` |
| `meter` | Time signature at the start (see 11.2 for changes) | `4/4`, `3/4`, `6/8`, `12/8` |
| `feel` | Groove or style, free text | `Slow shuffle` |
| `numbering` | How numbers count in a minor key: `1m` (default) or `6m` | `6m` |

Unknown names are kept and may be shown by a renderer. If `key` is missing,
`C` is assumed. If `meter` is missing, `4/4` is assumed.

If the closing `---` line is missing, the header ends at the first section line
(`##`), with a warning on line 1.

**Versions.** A parser SHOULD warn, on the `chartdown` line, when the file
declares a version it does not read, and then read the file anyway. A file with
no `chartdown` line is read as the latest version the parser knows.

**Minor keys.** In a minor key, `1` is the tonic. The tonic chord is `1m` and
the relative major is `b3`. Example: in `key: Em`, `1m` is Em and `b3` is G.

**`numbering: 6m`.** Some players number a minor-key song from its relative
major, so the tonic chord is `6m`. A file that says `numbering: 6m` counts
numbers that way in every minor key, including keys set with `@key`: in Em,
`6m` is Em, `1` is G, `4` is C and `3` is B. Major keys are not affected.
Letters mean the same in both, and so does the JSON form (Appendix A), which
gives each chord's distance from the tonic. Any other value is a warning, and
`1m` is used.

## 4. Sections and notes

- `## Name` starts a section. The name is free text.
- `> text` adds a note to the current section.
- `> MD: text` is a **music director note** for the whole band.
- `> MD <instruments>: text` is an MD note for specific players.
- `> MD 2x: text` or `> MD 2x <instruments>: text` is an MD note for one pass
  of the section's repeat (see 9.1).

```
## Chorus
> MD: Open it up a little. Don't go all out yet.
> MD drums: Light groove. Hi-hat or ride, no crashes.
> MD bass, keys: Root notes only.
> MD 2x drums: Add the crash on the second pass.
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
nothing is lost. The same goes for a cue (see 9): `{kazoo: in}` is a warning,
and the whole text `kazoo: in` becomes a cue for everyone.

In the JSON form (Appendix A), a role is written with its name in the Role
column (`everyone`, `guitar`, `vocals`, ...), and a custom role with its id.

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
Beats are counted in the units of the top number of the meter in effect for
that bar (4 in 4/4, 3 in 3/4, 6 in 6/8, 12 in 12/8; see 11.2). If any chord in
a bar has `:beats`, the beats SHOULD add up to the meter's top number. A
renderer SHOULD warn when they do not.

Tokens in the same bar that have no `:beats` share the beats that are left over
evenly. This also applies to rests (`x`) and riffs (see 7 and 7.1).

```
1:3 4:1        four beats in 4/4: three of 1, one of 4
1! x:3         one beat of 1 (stab), then three beats of rest
```

### 5.2 Reusing a section

`@use Name` as the only content line of a section copies the rows of the
earlier section called `Name`. The new section keeps its own notes. If the key
changed since the original, the copied rows use the current key. They keep the
meter and tempo they had in the original.

A section with `@use` and rows of its own keeps its rows, with a warning on the
`@use` line. A `@use` of a name with no earlier section is a warning on that
line.

```
## Chorus 2
> MD: A touch bigger.
@use Chorus
```

A count after the name plays the copied rows that many times in a row:
`@use Bridge 2 3x` (or `x3`, as in 10). Each time, the rows play exactly as
written in the original: their own repeats, endings and cues for one pass
(see 9.1) start over. So a section that already repeats plays its repeated
bars count times its own repeat, and its other bars count times.

```
## Bridge 2
|: 6m | 4 :| 2x
1 | 5

## Bridge 3
> MD: Build every time.
@use Bridge 2 2x
```

Bridge 3 plays `6m | 4`, `6m | 4`, `1 | 5`, then all of it again: `6m | 4`
four times and `1 | 5` twice. The last word of the line is a count when it is
a number with `x` (`2x`, `x2`); the rest is the section name. `0x` is read as
`1x`, with a warning. After a D.S. or D.C. jump, the section plays once, the
same way repeats play their last pass only (see 10.1).

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

**Quality** (written directly after the root, up to `/` or the end of the chord)

A quality is a base, an extension, alterations, `sus` and additions, in that
order, each optional:

| Part | Canonical text |
|---|---|
| Base | `m` (minor), `dim`, `aug` |
| Extension | `6`, `7`, `9`, `11`, `13`, `69`, `maj7`, `maj9`, `maj11`, `maj13` |
| Alterations | `b5`, `#5`, `b9`, `#9`, `b11`, `#11`, `b13`, `#13`, and `alt` |
| Suspension | `sus` (a fourth), `sus2` |
| Additions | `add2`, `add4`, `add9`, `add11`, `add13` |

`5` alone is a power chord. Examples: `m7`, `maj7`, `m7b5`, `dim7`, `7#9`,
`9sus`, `madd9`, `mmaj7`, `13#11`.

**Aliases.** Before a quality is read, these spellings take their canonical
form:

| Written | Canonical |
|---|---|
| `-`, `min`, `mi` at the start | `m` (`-7` is `m7`) |
| `ø`, `ø7` | `m7b5` |
| `°`, `o` / `°7`, `o7` | `dim` / `dim7` |
| `+` at the start | `aug` |
| `Δ` alone | `maj7` |
| `Δ`, `M` or `ma` before a number | `maj` (`M7` is `maj7`, `Δ9` is `maj9`) |
| `sus4` | `sus` |
| `(` and `)` | dropped (`7(b9)` is `7b9`) |

A parser SHOULD give a quality in its canonical form, in the JSON form too. A
quality that does not fit this vocabulary passes through as written, with no
warning, and a renderer shows it as written.

**Reading roots.** A number root is one digit, so in `57` the root is `5` and
the quality is `7`. After a letter, `b` or `#` belongs to the root, so `Bb9` is
Bb with a `9`, not B with a `b9`.

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

- **Tempo marks:** `{rit}`, `{accel}`, `{a tempo}`. Renderers draw these as
  tempo marks. A lasting tempo change uses `@tempo` (see 11.3).
- **Navigation marks:** `{segno}`, `{coda}`, `{to coda}` and the jumps listed
  in 10.1.

Dynamics, tempo marks and navigation marks are **reserved words**: they are
case-insensitive, and dots and spaces inside them are ignored (`{D.S. al Coda}`
and `{ds al coda}` are the same mark). Any other text is a cue.

### 9.1 Cues for one pass

Inside a repeat, a cue or an MD note can apply to one pass only. Write the pass
as a number followed by `x`, the same way a repeat counts passes (`:| 3x`, see
10), before the instruments. Several passes are separated by commas.

```
|: 1 {2x drums: in} | 4 | 5 {1x,3x: stop} | 1 :| 3x
```

- `{2x: text}` is for everyone on the second pass. `{2x drums: text}` is for
  the drummer on the second pass.
- `> MD 2x: text` and `> MD 2x bass: text` apply to a pass of the repeat in
  that section. In a section that plays a copy more than once (`@use Name 3x`,
  see 5.2), they apply to that time through the copy.
- The pass only counts as a pass when a `:` follows it, with or without
  instruments in between. `{2x louder}` is ordinary cue text.
- A pass on a bar outside any repeat, or on a note in a section with no repeat
  and no `@use` count, is a warning.

A quoted string `"like this"` attaches a **lyric landmark** to the bar. Keep it
short: a few words that tell the band where they are. A landmark SHOULD be at
most **8 words**, and a parser SHOULD warn when it is longer (see 14).

```
{mf} 6m 5 4 1 "You are good" | 6m 5 4 1
```

## 10. Repeats, endings and navigation

| Syntax | Meaning |
|---|---|
| `\|:` at the start of a row | Repeat starts at the first bar |
| `:\|` at the end of a row | Repeat ends at the last bar |
| `:\| 3x` | Play the repeated bars three times in total (default is 2). `:\| x3` is the same (0.5 and earlier) |
| `[1]` at the start of a bar | First ending |
| `[2]` at the start of a bar | Second ending |
| `[1,2]` | An ending used on several passes |

```
|: 1 | 4 | 1 | [1] 5 :| 2x
[2] 5 4 5 1 |
```

A `:|` with no `|:` before it in the same section repeats from the first bar of
the section. An ending covers its bar and the bars after it, up to the end of
its row or the next ending.

### 10.1 Navigation

Navigation marks are written as reserved words in curly braces (see 9) and
attach to a bar, so a jump can land in the middle of a section.

| Mark | Where it acts | Meaning |
|---|---|---|
| `{segno}` | start of the bar | The place a D.S. jumps back to |
| `{coda}` | start of the bar | The place a "to coda" jumps forward to |
| `{to coda}` | end of the bar | Jump to `{coda}`, only after a D.S. or D.C. |
| `{fine}` | end of the bar | The end, only after a D.S. or D.C. |
| `{D.S.}` | end of the bar | Jump back to `{segno}` and play to the end |
| `{D.S. al coda}` | end of the bar | Jump back to `{segno}`, then take the coda |
| `{D.S. al fine}` | end of the bar | Jump back to `{segno}`, then stop at `{fine}` |
| `{D.C.}`, `{D.C. al coda}`, `{D.C. al fine}` | end of the bar | Same, from the first bar of the song |

```
## Verse
{segno} 1 | 4 | 6m | 5

## Chorus
4 | 1 | 5 {to coda} | 6m
4 | 1 | 5 | 5 {D.S. al coda}

## Outro
{coda} 4 | 5 | <1>~
```

Play order: Verse, Chorus, then back to the Verse, the Chorus up to
`{to coda}`, and the Outro.

- `{to coda}` and `{fine}` are ignored until the jump has been taken.
- A song has at most one jump. After it, each repeat is played once, taking
  its last ending.
- A jump with no target (a D.S. with no `{segno}`, "al coda" with no `{to coda}`
  or `{coda}`, "al fine" with no `{fine}`), a second jump, or a mark used twice
  is a warning. The chart still renders as written.
- A renderer SHOULD be able to show the expanded play order.

## 11. Key, meter and tempo changes

### 11.1 Key

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

### 11.2 Meter

`@meter N/D` on its own line changes the meter from the next row onward, like
`@key`, and stays in effect across sections until the next `@meter`. For a single bar in another meter, start that bar with `[m:N/D]`. The
bar after it goes back to the meter in effect.

```
## Bridge
@meter 6/8
1 | 4 | 5 | 1
@meter 4/4
4 | 5 | [m:2/4] 5 | 1
```

A pickup (see 5.3) counts in the meter in effect, and a bar MUST NOT have both
`[p:N]` and `[m:N/D]`. Renderers SHOULD mark meter changes.

### 11.3 Tempo

`@tempo N` on its own line changes the tempo (beats per minute) from the next
row onward, and stays in effect across sections until the next `@tempo`. For gradual changes use the tempo marks `{rit}`, `{accel}` and
`{a tempo}` (see 9). Renderers SHOULD mark tempo changes.

## 12. Numbers and letters

A renderer SHOULD be able to show a chart in either style.

1. Convert every chord to a scale degree relative to the current key. With
   `numbering: 6m`, numbers in a minor key count from its relative major (see
   3), and letters shown as numbers use the same count.
2. To show letters, apply the display key (the written key unless transposed).
   A transposed display moves key changes by the same interval.
3. Riff notes (see 7.1) convert the same way as chord roots.
4. Number display uses `-` for minor (`6-`), in every minor quality (`6m7`
   shows as `6-7`). Qualities that start with `maj` are not minor. Letter
   display uses `m` (`Em`).
5. Use flat spellings in keys of F, Bb, Eb, Ab, Db, Gb. Use sharps otherwise.
   A minor key spells like its relative major: Dm like F (flats), Em like G
   (sharps).
6. A transposed key takes its common name. Major: C, Db, D, Eb, E, F, F#, G,
   Ab, A, Bb, B. Minor: Cm, C#m, Dm, Ebm, Em, Fm, F#m, Gm, G#m, Am, Bbm, Bm.

Degree to semitone (major scale): 1=0, 2=2, 3=4, 4=5, 5=7, 6=9, 7=11.
`b` lowers by one semitone, `#` raises by one.

## 13. Errors

A parser MUST NOT stop on bad input. It SHOULD:

- skip what it cannot read,
- report a warning with the line number,
- keep rendering everything else.

A warning carries the line it comes from (0 only when no line applies). The
order of warnings is not significant, and their wording is up to each parser.

## 14. Lyrics and rights

ChartDown is for structure, not for storing full lyrics. Quoted landmarks are
intended to be short (8 words at most, see 9), and parsers SHOULD warn above
that limit. Users are responsible for respecting the rights of
songwriters and publishers, and for checking their own license (for example
CCLI) before copying lyrics into any tool.

## 15. Grammar

This grammar uses ABNF (RFC 5234). Literal strings in quotes are
case-insensitive in ABNF; here they are **case-sensitive** unless a rule says
otherwise. A parser reads a file line by line, and trims spaces and tabs at
both ends of each line first.

**Lines**

```
file       = [ header ] *line
header     = "---" NL *( field NL ) ( "---" NL / &section )
field      = name *WSP ":" *WSP value / blank
name       = ( ALPHA / "_" ) *( ALPHA / DIGIT / "_" / "-" )   ; any case
line       = ( blank / comment / section / note / directive / row ) NL
blank      = ""
comment    = "//" *CHAR            ; only at the start of a line
section    = "##" *"#" *WSP text
note       = ">" [ " " ] ( md-note / text )
md-note    = "MD" [ 1*WSP [ passes *WSP ] [ roles ] ] *WSP ":" *WSP text   ; MD in any case
directive  = "@use" 1*WSP text [ 1*WSP count ]   ; directive names in any case
           / "@key" 1*WSP letter-root [ "m" ]
           / "@meter" 1*WSP 1*DIGIT *WSP "/" *WSP 1*DIGIT
           / "@tempo" 1*WSP number
```

Directive names are read in any case (`@Key`, `@TEMPO`). A line that starts
with `@` and is not one of these directives, or a directive with a value it
cannot read, is a warning (see 13). Any other line is a row, and a row before the first section is a
warning.

**Rows and bars**

```
row        = [ "|:" / "|" ] bar *( "|" bar ) [ ":|" *WSP [ count ] / "|" ]
count      = 1*DIGIT *WSP "x" / "x" *WSP 1*DIGIT         ; x in any case
bar        = [ pickup ] [ meter ] [ ending ] *( token / cue / lyric )
pickup     = "[p:" number "]"
meter      = "[m:" 1*DIGIT *WSP "/" *WSP 1*DIGIT "]"
ending     = "[" 1*DIGIT *( ( "," / "-" ) 1*DIGIT ) "]"
cue        = "{" *WSP cue-body *WSP "}"
lyric      = DQUOTE *( %x20-21 / %x23-7E / UTF8-non-ascii ) DQUOTE
```

- A repeat count only follows `:|` or ends a `@use` line (5.2). Elsewhere, `x3`
  is a token that cannot be read (a warning).
- Pickup, meter and ending open their bar, in that order. Later in the bar
  they cannot be read.
- Cues and lyric landmarks can sit anywhere in a bar. Tokens are separated by
  spaces, and a riff `R[ ... ]` is one token.
- An empty bar between two `|` is ignored.

**Tokens and chords**

```
token      = "%" / "=" / rest / riff / chord-tok
rest       = ( "x" / "X" ) [ ":" number ]
riff       = "R[" *WSP riff-note *( 1*WSP riff-note ) *WSP "]" [ ":" number ]
riff-note  = ( root / "x" ) [ ":" number ]
chord-tok  = [ "''" / "'" ] ( chord / "<" chord ">" ) [ ":" number ] *( "!" / "~" )
chord      = root quality [ "/" root ]
root       = number-root / letter-root
number-root = [ "b" / "#" ] %x31-37               ; one digit, 1 to 7
letter-root = %x41-47 [ "b" / "#" ]               ; A to G, uppercase
quality    = *( %x21-2E / %x30-7E / UTF8-non-ascii ) ; anything up to "/", read as in 6
number     = 1*DIGIT [ "." 1*DIGIT ]
```

- After a letter, `b` or `#` belongs to the root (see 6).
- A token that fits none of these rules is a warning, and the rest of the bar
  is still read.

**Cues**

```
cue-body   = reserved / [ passes *WSP ] [ roles *WSP ":" *WSP ] text
passes     = 1*DIGIT "x" *( *WSP "," *WSP 1*DIGIT "x" )
roles      = role *( ( "," / "/" / "&" / 1*WSP ) role )
role       = standard role name (see 4.1) / id declared in the instruments header
```

A cue whose text, compared without case, dots or spaces, is one of these words
is a reserved word, not a cue:

| Kind | Words |
|---|---|
| Dynamics (9) | `pp`, `p`, `mp`, `mf`, `f`, `ff`, `cresc`, `decresc` |
| Tempo marks (9) | `rit`, `accel`, `a tempo` |
| Navigation (10.1) | `segno`, `coda`, `to coda`, `fine`, `D.S.`, `D.S. al Coda`, `D.S. al Fine`, `D.C.`, `D.C. al Coda`, `D.C. al Fine` |

So `{D.S. AL CODA}`, `{ds al coda}` and `{D.S.al Coda}` are the same mark.

## 16. Roadmap

Ideas under consideration, not yet part of the format:

- **Layers:** a separate file of MD notes that sits on top of a chart without
  changing it, anchored to section names and bar numbers.
- Rehearsal marks, a `capo` header, hairpins (crescendo and decrescendo
  wedges).

## 17. Changelog

- **0.8** A count on `@use` plays the copied section several times:
  `@use Bridge 2 3x` (5.2). JSON form: `sections[].times`. In 0.7 the count
  was read as part of the name, so the section was not found (a warning).
- **0.7** Chord quality vocabulary with canonical forms and aliases (6). The
  `numbering: 6m` header for minor keys counted from the relative major (3).
  A formal grammar (15). Directive names in any case. JSON form: `numbering`
  and `rows[].minor`.
- **0.6 clarifications** Conformance suite (Appendix B). Rules that were left
  open: a header with no closing line (3), unknown versions (3), unknown names
  in cues and role names in the JSON form (4.1), `@use` with its own rows
  (5.2), meter and tempo across sections (11.2, 11.3), minor qualities in
  number display, minor-key spelling and transposed key names (12), warning
  lines and order (13). Charts read the same as before.
- **0.6** Meter and tempo changes (`@meter`, `[m:N/D]`, `@tempo`, 11.2 and
  11.3), tempo marks (`{rit}`, `{accel}`, `{a tempo}`), cues and MD notes for
  one pass (`2x`, 9.1), repeat counts written `:| 3x` (`x3` still works),
  navigation marks (segno, coda, D.S., D.C., fine, 10.1), and the JSON form
  (Appendix A).
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

## Appendix A. JSON form

A tool that exchanges parsed charts as JSON SHOULD use this shape, so that
apps, converters and tests agree on what a chart means. Field order does not
matter. Producers MAY add fields, and consumers MUST ignore fields they do not
know.

```json
{
  "chartdown": "0.8",
  "meta": { "title": "Open Road", "key": "G", "tempo": "76" },
  "key": "G",
  "minor": false,
  "numbering": "1m",
  "sections": [
    {
      "name": "Chorus",
      "use": null,
      "times": null,
      "notes": [
        { "md": true, "roles": ["drums"], "passes": [2], "text": "Add the crash." }
      ],
      "rows": [
        {
          "key": "G", "minor": false, "meter": "4/4", "tempo": 76,
          "bars": [
            {
              "number": 1, "beats": 4, "pickup": false, "meter": null,
              "repeatStart": false, "repeatEnd": false, "times": null,
              "ending": null,
              "dynamic": "mf", "tempoMark": null, "nav": ["segno"],
              "cues": [ { "roles": ["drums"], "passes": [], "text": "in" } ],
              "lyric": "Take the open road",
              "items": [
                { "type": "chord", "root": "4", "degree": 5, "quality": "",
                  "bass": null, "bassDegree": null, "beats": 4,
                  "push": 0, "hold": false, "stab": false, "fermata": false }
              ]
            }
          ]
        }
      ]
    }
  ],
  "playOrder": [ { "section": 0, "bar": 1, "pass": 1 } ],
  "warnings": [ { "line": 12, "message": "Can't read \"4zz\"" } ]
}
```

| Field | Meaning |
|---|---|
| `meta` | Every header field, as written (strings) |
| `key`, `minor` | Home key root and whether it is minor |
| `numbering` | `1m` or `6m`, from the header (see 3) |
| `sections[].use`, `times` | The section named by `@use`, and the count after it (see 5.2); `null` when there is none |
| `rows[].key`, `minor`, `meter`, `tempo` | Key, minor or not, meter and tempo in effect for the row (after `@key`, `@meter`, `@tempo`) |
| `bars[].number` | Bar number in its section: 0 for a pickup, then 1, 2, ... |
| `bars[].beats` | Length of the bar in beats (see 5.1, 5.3, 11.2) |
| `bars[].meter` | The one-bar meter from `[m:N/D]`, or `null` |
| `bars[].times` | Total passes on the bar that ends a repeat, otherwise `null` |
| `bars[].ending` | The passes written in `[ ]` on that bar (`[1, 2]`), or `null` |
| `bars[].nav` | Navigation marks, normalized: `segno`, `coda`, `tocoda`, `fine`, `ds`, `dsalcoda`, `dsalfine`, `dc`, `dcalcoda`, `dcalfine` |
| `cues[].passes`, `notes[].passes` | Passes from 9.1, empty for every pass |
| `items[].type` | `chord`, `rest` (`x`), `riff`, `repeat` (`%`) or `tie` (`=`) |
| `root`, `bass` | As written (number or letter) |
| `quality` | In its canonical form (see 6), or as written when it is not in the vocabulary |
| `degree`, `bassDegree` | Semitones above the row's tonic, 0 to 11 (see 12), whatever the numbering |
| `beats` | Share of the bar, after the rules in 5.1 |
| `push` | 0 none, 1 eighth (`'`), 2 sixteenth (`''`) |
| riff `notes` | `{ "root", "degree", "beats" }`, or `{ "rest": true, "beats" }` |
| `playOrder` | Bars in the order they are played, after repeats, endings and navigation (see 10 and 10.1). `section` is an index into `sections`, `bar` is a bar number |
| `warnings` | Every warning, with its line number (0 when it has none) |

## Appendix B. Conformance

The `conformance/` folder holds the official test cases: one file per section
of this spec, each case a short chart and what a correct parser makes of it.
A case checks the JSON form (Appendix A) of the chart. Cases for section 12
check the chords as shown in numbers or letters, and apply only to renderers.

A parser conforms to this version when it passes every case.
`conformance/README.md` describes the format and how to run the cases against
any implementation.

