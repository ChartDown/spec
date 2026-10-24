# Changelog

## 0.9 (draft)
- A tempo can say what one beat is: `tempo: 58 q.`, `@tempo 87 q`, with `h`, `h.`, `q`, `q.`, `e`, `e.`, `s`, and `?` when it isn't known yet. Without a unit, 6/8, 9/8 and 12/8 count the dotted quarter note and every other meter its bottom number. Tempos can have decimals. A tool converts tempos to eighth notes per minute to compare or play them (spec 11.3).
- JSON form: `rows[].tempoUnit` and `rows[].tempoEpm` (Appendix A).
- Conformance: 8 new cases for beat units (156 in all).
- Guides, syntax tour and playground: tempo units; the playground shows a tempo with its unit as written.
- Version 0.9 in the spec, examples, guides, landing and playground.
- New example: Amazing Grace (public domain hymn, 3/4, six verses with `@use`). The playground's example list shows it and the syntax tour, and opens with it; the made-up songs stay in `examples/` but leave the list.

## 0.8 (draft)
- A count on `@use` plays the copied section several times: `@use Bridge 2 3x` (or `x3`). Each time through plays it as written, so its own repeats, endings and cues for one pass start over; after a D.S. or D.C. jump it plays once (spec 5.2). MD notes for one pass in that section need no repeat of their own (spec 9.1).
- JSON form: `sections[].times` (Appendix A).
- Conformance: 6 new cases for `@use` counts (148 in all).
- Guides and the syntax tour: `@use ... 2x`. Playground: reads the count, shows it as `×N` on the section and in the song form, and accepts `chartdown: 0.8`.
- Version 0.8 in the spec, examples, guides, landing and playground.

## 0.7 (draft)
- Chord quality vocabulary: canonical forms (`m7`, `maj7`, `m7b5`, `dim7`, `7#9`, `9sus`, ...) and aliases (`-7`, `M7`, `Δ`, `ø`, `°7`, `+`, `sus4`, parentheses). A quality outside the vocabulary passes through as written (spec 6).
- `numbering: 6m` header: in a minor key, numbers count from the relative major, so the tonic is `6m`. `1m` stays the default (spec 3).
- Formal grammar in ABNF (spec 15). Directive names in any case; a `@key` it cannot read is a warning.
- JSON form: `numbering`, `rows[].minor`, and `quality` in its canonical form (Appendix A).
- Conformance: 19 new cases (142 in all), and the existing cases updated for the new JSON fields.
- Guides: `numbering: 6m` and common quality spellings. Syntax tour: quality aliases.
- Playground: quality aliases, `numbering: 6m`, any-case directives.
- Playground: print or save a PDF of the chart as the current view and key show it; new chart, open and download a `.chartdown` file; the draft stays in the browser; interface in English and Spanish.
- Guides and the new-chart template: chord letters (`G | C D | Em`) are a fine way to start; the chart still shows in numbers.
- Playground: New, Open and Download sit in the editor's menu bar, Copy text and Copy JSON at its foot, and the examples below it.

## 0.6 clarifications
- Conformance suite: 123 cases in `conformance/`, one file per spec section, checked by `node conformance/run.mjs` (spec Appendix B). Cases come from the ChartDown App parser and were checked against the playground.
- Spec rules that were left open: a header with no closing line, unknown versions (3), unknown names in cues and role names in the JSON form (4.1), `@use` with its own rows (5.2), meter and tempo across sections (11.2, 11.3), minor qualities in number display, minor-key spelling and transposed key names (12), warning lines and order (13).
- Playground: warnings for an unknown version, a header with no closing line, an unknown name in a cue and `@use` with its own rows; warnings carry their line; JSON roles use their spec names; minor keys spell like their relative major; transposed key changes take their common name; `6m7` shows as `6-7` in numbers.

## 0.6 (draft)
- Meter and tempo changes: `@meter N/D`, `@tempo N`, and `[m:N/D]` for one bar (spec 11.2, 11.3).
- Tempo marks `{rit}`, `{accel}`, `{a tempo}` as reserved words (spec 9).
- Cues and MD notes for one pass of a repeat: `{2x drums: in}`, `> MD 2x bass: ...` (spec 9.1).
- Repeat counts written `:| 3x`. The older form `:| x3` still works.
- Navigation marks on bars: `{segno}`, `{coda}`, `{to coda}`, `{fine}`, D.S. and D.C. jumps (spec 10.1). Charts from 0.4 and 0.5 parse the same; a mark that was free text now has its meaning, and an incomplete jump is only a warning.
- JSON form of a parsed chart (spec Appendix A).
- New example: Northbound (D.S. al Coda, 6/8 bridge, 2/4 bar, cues for one pass).
- Playground: play order, meter and tempo marks, pass tags, Copy JSON.
- Syntax tour rewritten to show every feature of the spec, one section each, named after its spec section.

## 0.5 (draft)
- Pickup bars: `[p:N]` at the start of a section's first bar (spec 5.3).
- Partial rests and stops: `x:beats`. Tokens without `:beats` share what is left of the bar (spec 5.1).
- Sixteenth-note push `''`, next to the eighth-note push `'` (spec 8).
- Unison riffs: `R[ ... ]` (spec 7.1).
- Playground: pickup bars drawn narrower and numbered as bar 0, riff and short-push rendering.
- Roadmap: navigation (segno, coda, D.S., D.C.) redesigned and deferred; tempo and meter changes, rehearsal marks.

## 0.4 (draft)
- New standard instrument roles: saxophone (`sax`), trumpet (`tpt`), trombone (`tbn`), violin (`vln`).
- New `instruments` header field to declare custom instrument roles.
- Playground: View menu and labels for the new instruments.

## 0.3 (draft)
- New `chartdown` header field declares the spec version of a file.
- Removed the `ccli` header field.
- Lyric landmarks limited to 8 words; parsers warn above the limit.
- Roadmap: layers, custom instruments, JSON form.
- Examples replaced with original songs written for this repository.

## 0.2 (draft)
- MD notes for specific instruments, per-instrument bar cues and views.
- Minor keys, `feel` header, `@key` key changes, beat-count checks.

## 0.1 (draft)
- First draft.
