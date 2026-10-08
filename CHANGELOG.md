# Changelog

## 0.6 (draft)
- Meter and tempo changes: `@meter N/D`, `@tempo N`, and `[m:N/D]` for one bar (spec 11.2, 11.3).
- Tempo marks `{rit}`, `{accel}`, `{a tempo}` as reserved words (spec 9).
- Cues and MD notes for one pass of a repeat: `{2x drums: in}`, `> MD 2x bass: ...` (spec 9.1).
- Repeat counts written `:| 3x`. The older form `:| x3` still works.
- Navigation marks on bars: `{segno}`, `{coda}`, `{to coda}`, `{fine}`, D.S. and D.C. jumps (spec 10.1). Charts from 0.4 and 0.5 parse the same; a mark that was free text now has its meaning, and an incomplete jump is only a warning.
- JSON form of a parsed chart (spec Appendix A).
- New example: Northbound (D.S. al Coda, 6/8 bridge, 2/4 bar, cues for one pass).
- Playground: play order, meter and tempo marks, pass tags, Copy JSON.

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
