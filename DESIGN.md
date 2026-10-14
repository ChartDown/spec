---
name: ChartDown
description: Markdown for band charts. A stage lit scene by scene; the chart is always paper.
source: "ChartDown – Spec design system, https://claude.ai/artifact/7FuYpSAnB2AxP2XZJQ2VSi"
colors:
  ink: "#16212c"
  ink-muted: "#566574"
  paper: "#fbfcfd"
  ground: "#eef1f4"
  rule: "#dde3e9"
  md: "#b8322a"
  dynamic: "#1f4fa8"
  gel-red: "#ef5a48"
  gel-amber: "#f5b23e"
  gel-blue: "#3450e0"
  gel-pink: "#f4b3cb"
typography:
  display: "Barlow Condensed 600/700/800, self-hosted"
  text: "Barlow 400/500/600, self-hosted"
  mono: "JetBrains Mono, only for ChartDown source and commands"
---

# ChartDown landing design

The landing follows the **ChartDown – Spec** design system. Its brand book is the source of truth for the logo, ink, the two pencils (`md` red for the music director, `dynamic` blue for dynamics and links), type and components. This file records what the landing adds.

## Scenes and gels

Each scene owns a full-bleed field, like a stage light with a colored gel. Text color follows the field:

| Field | Text | Used for |
|---|---|---|
| ink | `#e6edf3`, secondary `#98a8b8` | Hero, Open, footer |
| gel-red `#ef5a48` | ink (4.8:1) | Every player hears it differently |
| paper | ink | Write it once |
| gel-amber `#f5b23e` | ink (8.8:1) | Every player reads their part |
| gel-blue `#3450e0` | paper (6.1:1) | The key changes |
| gel-pink `#f4b3cb` | ink (9.4:1) | Join |

The chart sheet is always paper with an ink border and a hard offset shadow, so the chart reads the same on every field. The pencils (`md`, `dynamic`) appear only on paper.

## Type

- Display: Barlow Condensed 700 for headlines (`.mega` up to 144px, `.big` up to 112px, 88px inside pinned scenes), 800 for the role name, the split-flap chords and the stats.
- Text: Barlow 400 to 600. Pills and labels at 13 to 16px, 600.
- Mono: JetBrains Mono, only for ChartDown source and shell commands.

## Motion

- A small scroll engine in the page script, no libraries. `.pin` scenes are `--len` viewports tall with a sticky stage; each frame passes the scroll progress (0 to 1) to the scene.
- The page color follows the scene in the middle of the screen (0.7s ease).
- The stage drawing draws itself on load (stroke-dashoffset, staggered).
- Join cards and the 142 counter animate once, when they enter.
- `prefers-reduced-motion`, or a screen narrower than 760px or shorter than 600px: nothing pins, nothing animates, every scene shows its finished state.

## Components on the page

Header rack (paper boxes with an ink border), pill labels, buttons (solid and outline), the dark editor with two-pencil highlighting, the chart sheet (from the playground parser), role and key toggles (pills, pressed state filled with the text color), the split-flap row, stat strip, command boxes with Copy, join cards.
