---
name: ChartDown
description: Markdown for band charts. The page is the stage floor at soundcheck, with the chart taped down at your feet.
colors:
  floor: "#1b1c1e"
  floor-raised: "#242528"
  floor-line: "#34353a"
  chalk: "#eeeae0"
  chalk-dim: "#aaa69c"
  tape: "#efece4"
  tape-edge: "#d9d5ca"
  paper: "#f4f2ec"
  gaff: "#2b2c30"
  ink: "#16171a"
  ink-soft: "#4a4a4f"
  spike-drums: "#e5432d"
  spike-bass: "#f3c316"
  spike-keys: "#2f7de1"
  spike-gtr: "#2ead5b"
  spike-vox: "#e85a9c"
  spike-all: "#eeeae0"
typography:
  display:
    fontFamily: "Stencil, Shoulders, system-ui, sans-serif"
    fontSize: "clamp(2.4rem, 1.5rem + 3.6vw, 5.4rem)"
    fontWeight: 800
    lineHeight: 0.9
    letterSpacing: "0.01em"
  wordmark:
    fontFamily: "Stencil, Shoulders, system-ui, sans-serif"
    fontSize: "clamp(1.9rem, 1.4rem + 1.6vw, 2.6rem)"
    fontWeight: 800
    lineHeight: 0.9
    letterSpacing: "0.02em"
  headline-marker:
    fontFamily: "Marker, 'Comic Sans MS', cursive"
    fontSize: "clamp(2.2rem, 1.3rem + 3.2vw, 4.1rem)"
    fontWeight: 400
    lineHeight: 1.08
  title-marker:
    fontFamily: "Marker, 'Comic Sans MS', cursive"
    fontSize: "clamp(1.7rem, 1.2rem + 1.4vw, 2.4rem)"
    fontWeight: 400
    lineHeight: 1
  chord:
    fontFamily: "Shoulders, system-ui, sans-serif"
    fontSize: "clamp(1.5rem, 1.1rem + 1vw, 2rem)"
    fontWeight: 900
    lineHeight: 1.1
    letterSpacing: "0.01em"
  note-marker:
    fontFamily: "Marker, 'Comic Sans MS', cursive"
    fontSize: "1.02rem"
    fontWeight: 400
    lineHeight: 1.3
  lede:
    fontFamily: "Archivo, system-ui, -apple-system, 'Segoe UI', sans-serif"
    fontSize: "clamp(1.15rem, 1rem + 0.5vw, 1.45rem)"
    fontWeight: 400
    lineHeight: 1.4
  body:
    fontFamily: "Archivo, system-ui, -apple-system, 'Segoe UI', sans-serif"
    fontSize: "1.0625rem"
    fontWeight: 400
    lineHeight: 1.55
  action:
    fontFamily: "Archivo, system-ui, -apple-system, 'Segoe UI', sans-serif"
    fontSize: "1.12rem"
    fontWeight: 600
    lineHeight: 1
  label-caps:
    fontFamily: "Shoulders, system-ui, sans-serif"
    fontSize: "0.8rem"
    fontWeight: 800
    lineHeight: 1
    letterSpacing: "0.06em"
  cue:
    fontFamily: "Archivo, system-ui, sans-serif"
    fontSize: "0.78rem"
    fontWeight: 700
    lineHeight: 1.15
  source:
    fontFamily: "'JetBrains Mono', ui-monospace, Menlo, monospace"
    fontSize: "0.84rem"
    fontWeight: 400
    lineHeight: 1.6
rounded:
  none: "0px"
  chip: "2px"
  control: "3px"
  field: "4px"
  plot: "6px"
spacing:
  xs: "6px"
  sm: "12px"
  md: "22px"
  gutter: "clamp(16px, 4vw, 56px)"
  hero-gap: "clamp(24px, 3.4vw, 56px)"
  section: "clamp(52px, 6.5vw, 92px)"
components:
  tape-strip:
    backgroundColor: "{colors.tape}"
    textColor: "{colors.ink}"
    rounded: "{rounded.none}"
    padding: "0.2em 1.05em 0.25em"
  button-go:
    backgroundColor: "{colors.spike-bass}"
    textColor: "{colors.ink}"
    typography: "{typography.action}"
    rounded: "{rounded.none}"
    padding: "16px 30px 17px"
  nav-tab:
    backgroundColor: "#303136"
    textColor: "{colors.chalk}"
    rounded: "{rounded.none}"
    padding: "10px 16px"
  nav-tab-hover:
    backgroundColor: "#45464c"
    textColor: "{colors.chalk}"
  lang-tab-active:
    backgroundColor: "{colors.tape}"
    textColor: "{colors.ink}"
    padding: "9px 13px"
  taped-sheet:
    backgroundColor: "{colors.paper}"
    textColor: "{colors.ink}"
    rounded: "{rounded.none}"
    padding: "clamp(22px, 2.6vw, 38px) clamp(20px, 2.6vw, 40px)"
  gaff-corner:
    backgroundColor: "{colors.gaff}"
    width: "92px"
    height: "30px"
  section-badge:
    backgroundColor: "{colors.ink}"
    textColor: "{colors.paper}"
    rounded: "{rounded.none}"
    padding: "3px 9px 2px"
  who-tag:
    textColor: "{colors.ink}"
    typography: "{typography.label-caps}"
    rounded: "{rounded.chip}"
    padding: "4px 6px 3px"
  cue-chip:
    backgroundColor: "{colors.spike-bass}"
    textColor: "{colors.ink}"
    typography: "{typography.cue}"
    rounded: "{rounded.chip}"
    padding: "3px 6px"
  spike-mark:
    textColor: "{colors.chalk}"
    rounded: "{rounded.control}"
    padding: "8px 10px 8px 6px"
  spike-mark-hover:
    backgroundColor: "{colors.floor-raised}"
  key-tape:
    backgroundColor: "{colors.tape}"
    textColor: "{colors.ink}"
    padding: "5px 14px"
  key-button:
    textColor: "{colors.ink}"
    padding: "9px 10px 8px"
    width: "42px"
  source-field:
    backgroundColor: "rgba(0,0,0,0.28)"
    textColor: "{colors.chalk}"
    typography: "{typography.source}"
    rounded: "{rounded.field}"
    padding: "14px 16px"
  stage-plot:
    backgroundColor: "{colors.floor-raised}"
    rounded: "{rounded.plot}"
---

# Design System: ChartDown

## Overview

**Creative North Star: "The Stage Floor at Soundcheck"**

The page is a scuffed, charcoal-painted stage at soundcheck. The chart is a set list taped down at the players' feet; every block of copy that needs to be found fast is written on a strip of torn off-white gaffer tape; chord numbers and MD notes are written in by hand; headlines are road-case stencil. Each player's position is marked with a colored spike-tape X, and those five colors are the only saturated hues in the world.

It is a dark, low-light, material world. Depth comes from physical things lying on a floor (tape, paper, gaff corners) with real drop shadows and slight skew, never from floating cards or glass. Long reading copy sits directly on the floor in chalk, for comfort; tape is reserved for labels, headlines, commands and the primary action. The chart sheet is the hero object and is live: real ChartDown text rendered by the reference parser, re-taped when the player or key changes.

The world rejects the open-source split hero (code left, render right, feature cards below). It also explicitly stands apart from `playground/index.html`, which is an older light-themed reference tool and is not governed by this file.

**Key Characteristics:**
- Charcoal floor ground with a fractal "scuff" texture and a soft top-right pool of light.
- Torn-edge tape strips (CSS mask with jagged left/right ends) carrying headlines, labels and the primary action.
- Paper sheets held down by four rotated black gaff corners.
- Five spike-tape role colors, always paired with a label and the X mark, never color alone.
- Four hand-feel faces with strict jobs: stencil caps, marker, condensed numerals, plain sans; mono only for machine text.
- Small rotations (between -1.6deg and 1.3deg) on tape and paper; nothing is perfectly square except the controls' hit areas.

## Colors

A near-black stage floor, warm off-white tape and chalk, black ink, and five spike-tape colors that each belong to exactly one player.

### Primary
- **Bass Spike Yellow** (spike-bass): the system's action color. The "Open the playground" tape, focus rings, the caret, text selection, the highlighter under the lede's key phrase, the active-source border and the "A" key callout. It is also the bass player's mark.

### Secondary (the spike set)
- **Drums Spike Red** (spike-drums): drummer's mark and cues; also the ChartDown wordmark X and the warnings border on the sheet.
- **Keys Spike Blue** (spike-keys): keys player's mark and cues.
- **Guitar Spike Green** (spike-gtr): guitar player's mark and cues.
- **Vocals Spike Pink** (spike-vox): vocalist's mark and cues.
- **Whole-Band Chalk** (spike-all): the "Everyone" / MD mark; the same value as chalk.

### Neutral
- **Stage Floor Charcoal** (floor): the page ground, scrollbar track, theme color.
- **Raised Floor** (floor-raised): the stage-plot riser and hover wash under spike marks.
- **Floor Seam** (floor-line): hairlines, dashed source-field border, footer rule, fact-list dividers.
- **Chalk** (chalk): all text on the floor (14.2:1 on floor).
- **Dim Chalk** (chalk-dim): secondary floor text, legends, captions (7.0:1 on floor).
- **Gaffer Tape White** (tape): the tape strip surface, multiplied with fine noise.
- **Tape Edge** (tape-edge): the slightly darker tape tone for edges and separators.
- **Set List Paper** (paper): the chart sheet, the syntax "road case" paper and the JSON card.
- **Black Gaff** (gaff): the corner strips that pin paper down.
- **Sharpie Ink** (ink): all text on tape and paper, bar lines, section badges.
- **Faded Ink** (ink-soft): readout labels, lyric landmarks, secondary paper text.

### Named Rules
**The One Player, One Color Rule.** Each spike color means one player and nothing else. Never use drums red for "danger", keys blue for "link", or green for "success"; the only allowed borrowings are the yellow action role and the red wordmark X, both already in the build.

**The Mark And Label Rule.** A role color never appears alone. Every spike color is accompanied by the X mark and the player's name (spike buttons, stage-plot positions), or by the name written into the chip ("Drums: in").

**The Floor Is Not A Palette Rule.** No new background hues. Surfaces are floor, raised floor, tape, paper or gaff.

## Typography

**Display Font:** Big Shoulders Stencil Display, served as "Stencil" (fallback Big Shoulders Display, system-ui)
**Numeral / Label Font:** Big Shoulders Display, served as "Shoulders" (100-900 variable)
**Hand Font:** Permanent Marker, served as "Marker" (fallback Comic Sans MS, cursive)
**Body Font:** Archivo (100-900 variable, fallback system-ui)
**Mono Font:** JetBrains Mono

All five are self-hosted in `assets/fonts/` (OFL 1.1, latin subset), loaded with `font-display: swap`; Marker and Stencil are preloaded.

**Character:** Road-case stencil for what is stamped on the gear, marker for what the MD wrote by hand, condensed heavy numerals for the numbers the band reads from six feet away, and a plain sans for anything meant to be read at length.

### Hierarchy
- **Display** (Stencil 800, uppercase, clamp 2.4rem to 5.4rem, line-height 0.9): section headlines, max 16ch, `text-wrap: balance`. The closing headline goes larger (clamp 2.8rem to 6rem, 13ch).
- **Wordmark** (Stencil 800, uppercase, clamp 1.9rem to 2.6rem): "ChartDown" beside the red spike X.
- **Headline marker** (Marker 400, clamp 2.2rem to 4.1rem, line-height 1.08): the tagline, written on a tape strip.
- **Title marker** (Marker 400, clamp 1.7rem to 2.4rem): the song title on the sheet; readout values (1.3rem).
- **Chord** (Shoulders 900, clamp 1.5rem to 2rem, line-height 1.1): chord numbers and letters on the sheet, and the transposition strips (up to 3rem). Qualities are a superscript at 0.55em, slash bass at 0.7em.
- **Note marker** (Marker 400, 1.02rem, line-height 1.3): MD notes on the sheet, stage-plot tape notes, syntax descriptions.
- **Lede** (Archivo 400, clamp 1.15rem to 1.45rem, line-height 1.4, max 30ch): the hero sentence, on the floor.
- **Body** (Archivo 400, 1.0625rem, line-height 1.55): section subcopy runs a touch larger (clamp 1.08rem to 1.25rem) at max 62ch.
- **Label caps** (Shoulders 700-900, uppercase, 0.76rem to 1rem, letter-spacing 0.05em to 0.08em): readout labels, section badges, who-tags, stage-plot names, fact keys.
- **Cue** (Archivo 700, 0.78rem): text inside cue chips.
- **Source** (JetBrains Mono 400-600, 0.8rem to 1.02rem): the editable `.chartdown` source, commands, JSON, inline code.

### Named Rules
**The Numbers Are Not Handwriting Rule.** Chord numbers are always set in Shoulders 900, never in Marker: Marker's numeral 1 reads as a bracket at chart size, and the numbers are the most-read glyphs on the page.

**The Mono Is Machine Text Rule.** JetBrains Mono appears only for literal text a machine reads: `.chartdown` source, shell commands, JSON and inline syntax. Never for labels, headings or decoration.

**The Read It On The Floor Rule.** Paragraph copy (lede, subcopy, facts) sits on the floor in Archivo. Marker and tape are for short, glanceable writing.

## Layout

Full-bleed floor with a fluid side gutter (spacing.gutter, 16px to 56px). The hero is a two-column grid, 5fr pitch to 7fr board, gap spacing.hero-gap; the board holds the taped sheet with the spike-mark and key controls along its lower edge, like the downstage lip. Sections stack with fluid vertical padding (spacing.section) and left-aligned headlines; there is no centered marketing column.

The stage plot is an absolutely positioned diagram (aspect ratio 16 / 7.2) with players at their real stage positions: drums upstage center, bass and keys at the wings, guitar, vocals and MD downstage, and a dashed chalk "front of stage" edge.

Responsive behavior:
- **At 1080px and below:** the hero collapses to one column and reorders as tagline, lede, sheet, actions, source. The stage plot becomes a 2-column grid of positions (bass/drums, keys/gtr, vox/md). The builders section stacks.
- **At 640px and below:** the header stacks, the GitHub tab hides, the sheet loses its skew, gaff corners shrink to 70 by 24px, lyric landmarks hide, chord size drops to 1.3rem, the actions move above the sheet, and the stage plot re-centers drums and vocals on their own rows.

Rhythm: 6px inside control groups, 12px between related items, 22px between hero stack items, fluid section padding between bands.

## Elevation & Depth

Depth is physical: things lie on the floor and cast short, soft, doubled drop shadows (a tight contact shadow plus a wider ambient one), applied with `filter: drop-shadow` so they follow torn tape edges. There are no floating cards, no hover lifts beyond 2px, and no glass or blur surfaces. Texture does the rest: a fixed fractal "scuff" over the floor and fine noise multiplied into tape and paper.

### Shadow Vocabulary
- **Tape on floor** (`filter: drop-shadow(0 2px 1.5px rgba(0,0,0,.45)) drop-shadow(0 10px 12px rgba(0,0,0,.28))`): every tape strip.
- **Sheet on floor** (`filter: drop-shadow(0 3px 2px rgba(0,0,0,.4)) drop-shadow(0 24px 30px rgba(0,0,0,.45))`): the hero set list; the road-case paper and JSON card use the same pair at 20/26px and 18/22px.
- **Gaff sheen** (`box-shadow: inset 0 1px 0 rgba(255,255,255,.08)`): the top edge of gaff corners.
- **Cue chip** (`box-shadow: 0 1px 2px rgba(0,0,0,.25)`): cues sitting on paper.

### Named Rules
**The Lying Flat Rule.** Every raised object is something that could physically lie on a stage. If it would not be taped down, it does not get a shadow.

## Shapes

Tape has no corners at all: its left and right ends are torn by an SVG mask (9px torn ends on tape, 6px on nav tabs, 7px on gaff corners), and top/bottom edges are straight. Paper is square-cornered. Rounding is reserved for small functional pieces: chips and who-tags (2px), spike buttons and inline code (3px), the source field (4px), and the stage-plot riser (6px).

Hand-drawn forms recur: the spike X (two crossed tape bars), a loose marker ring drawn around the selected spike mark or key, a diamond for held chords, a fermata arc, and double bar lines for repeats. Slight rotation is part of the form language: tape from -1.6deg to 1.3deg, the sheet at 0.7deg, the road case at -0.4deg, gaff corners at about 32 to 38deg.

## Components

### Tape strip
The world's carrier for short copy. Tape-white surface with noise multiplied in, ink text, torn left and right ends, wrapped in the tape-on-floor shadow and given a small rotation. Used for the tagline, the key row, stage-plot notes, the conformance command and the "Written" number strip.

### Primary action ("Open the playground")
- **Shape:** a tape strip in bass yellow, torn ends, padding 16px 30px 17px, Archivo 600 1.12rem with an arrow glyph drawn as inline SVG.
- **Hover:** rises 2px and tilts -0.6deg (0.25s, ease); active returns to rest.
- **Secondary:** plain chalk text links with a 2px underline at 40% chalk that turns bass yellow on hover.

### Navigation (tape tabs)
- Dark tape tabs (`#303136` with noise screened in) with 6px torn ends, Archivo 600 0.92rem chalk text, alternately rotated -1.2deg and 0.9deg.
- **Hover:** lighter tape (`#45464c`), straightens and lifts 1px.
- **Language toggle (EN / ES):** two tabs; the pressed one turns to tape white with ink text. Both languages are equal; neither is styled as the default.
- **Mobile:** the header stacks, GitHub hides, tabs keep their tape form.

### Taped set list sheet (signature)
Paper surface, four black gaff corners at the corners, skewed 0.7deg. Inside: a marker song title with a 3px ink rule beneath, stencil-ish caps readouts (Key, Tempo, Time) with marker values; ink section badges (Shoulders 900 caps on ink, rotated -1deg); bars separated by 3px ink lines, repeat bars with 7px double lines; chord numbers in Shoulders 900; italic Archivo lyric landmarks in faded ink; MD notes in marker with a who-tag; cue chips above the chords. Parser warnings appear as a red dashed box, never as an alert.

### Cue chip and who-tag
- **Cue chip:** a 2px-rounded chip with the player's name written into the text ("Bass: walk up"); Archivo 700 0.78rem in ink. A whole-band cue is a solid bass-yellow chip; a player's cue is white with a 2px inset ring in that player's spike color, so the text stays ink on white (spike colors fail AA as a ground for small text).
- **Who-tag:** a 2px ink-outlined label in Shoulders caps with a 9px square swatch of the player's color; the whole-band "MD" tag is solid ink with paper text.

### Spike marks (signature control)
A row of toggle buttons: a 22px spike X in the player's color plus the player's name in chalk. Pressed state rotates the X 90deg and scales it 1.12, and draws a loose chalk marker ring around the button. Hover gives a raised-floor wash. Choosing a mark re-tapes the sheet so only that player's notes and cues (plus whole-band ones) remain.

### Key tape
A single tape strip holding segmented key buttons separated by 2px ink rules: "Numbers" first in Archivo 600, then keys in Shoulders 800. The pressed key gets an ink marker ring drawn on; focus uses a drums-red inset outline on the tape. Changing key flips the chord glyphs in place.

### Source field
A dashed 2px floor-seam box on a 28% black wash, JetBrains Mono 0.84rem, bass-yellow caret. Hover darkens the dash; focus turns it to a solid bass-yellow border. It is deliberately not tape: an input must look like an input.

### Stage plot
A raised-floor riser with faint vertical board seams, a 3px border, 6px corners and a dashed chalk downstage edge. Each position is a spike mark with a caps name and a tape note holding the real `.chartdown` line in mono plus a marker gloss.

### Facts list
Plain floor rows separated by floor-seam hairlines: a Shoulders caps key (8.5em wide) linked into the repo, and a dim-chalk description.

## Do's and Don'ts

### Do:
- **Do** put every short, glanceable label, headline-on-tape and the primary action on a torn tape strip with the tape-on-floor shadow and a rotation under 2deg.
- **Do** pair every spike color with the X mark and the player's name.
- **Do** set chord numbers in Shoulders 900 and MD notes in Marker.
- **Do** render real ChartDown text through the reference parser wherever a chart is shown.
- **Do** keep long copy on the floor in Archivo at 62ch or narrower.
- **Do** use the shared ease (`cubic-bezier(.16,1,.3,1)`) and let the tape-laying reveal (0.5s clip from the left) be the one authored entrance; everything else is a state transition under 0.4s.
- **Do** honor `prefers-reduced-motion` by removing all animation and transition.
- **Do** use the bass-yellow 3px focus ring with 3px offset on the floor; inset it on tape and tabs.
- **Do** give English and Spanish identical layout and weight.

### Don't:
- **Don't** use a role color for status, links or decoration unrelated to that player.
- **Don't** set chord numbers in Marker.
- **Don't** use mono for anything a machine does not read.
- **Don't** put paragraphs on tape, or make the source field look like tape.
- **Don't** add floating cards, glass, gradients-as-surfaces or new background hues; surfaces are floor, raised floor, tape, paper and gaff.
- **Don't** square up the world: a page of perfectly aligned, unrotated tape reads as a template, not a stage.
- **Don't** style the separate `playground/index.html` reference tool from this file.
