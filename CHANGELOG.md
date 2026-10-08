# Changelog

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
