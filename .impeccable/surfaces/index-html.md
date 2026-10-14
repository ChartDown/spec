---
version: 1
slug: "index-html"
primary_target: "index.html"
related_targets: []
---

# Landing (chartdown.github.io/spec)

Mode: Persuade. Audience: worship team MDs and players first, then other working bands, then tool builders. Action: try ChartDown on the page and open the playground; secondary: read the guide/spec, share a chart in Discussions, star on GitHub. Proof: real ChartDown text rendered live by the reference parser (playground/index.html), the spec, 142 conformance cases. Constraints: US English + Mexican Spanish as equals; never mention ChartDown App; no invented users, stars or quotes; original example songs only. Build path: code-led (no image generation available).

## Direction contract

THESIS: The page is the stage floor at soundcheck. The chart is the set list taped down at your feet, and every player's spot is marked with spike tape. It refuses the open-source split hero (code left, render right, feature cards below).

OWN-WORLD: Scuffed charcoal stage-floor paint as the ground; torn off-white gaffer tape strips carry every block of copy; Sharpie marker for chord numbers and MD notes; road-case stencil caps for headlines; one spike-tape color per player (drums red, bass yellow, keys blue, guitar green, vocals pink) used as marks with labels, never color alone. Mono only for real .chartdown source.

STORY: The visitor sees a real chart taped to the floor, steps onto their spike mark and sees only their cues, changes the key, sees it is just text they can type, then opens the playground or reads the guide.

FIRST VIEWPORT: Stencil wordmark and the tagline written on tape top-left, nav as tape tabs with EN/ES. Dominant: a large taped set list sheet with the live chart (Open Road), slightly skewed. Beside it a tape strip holding the editable source. Along the bottom edge, the spike marks row (Band, Drums, Bass, Keys, Guitar, Vocals) and a key tape; primary action "Open the playground" as a bold tape strip in the first viewport.

FORM: Gaffer-tape set list and stage plot, IMPECCABLE'S PICK (my list position 1), seed 0d8fe95e. Signature interaction: choosing a spike mark re-tapes the chart for that player; typing in the source re-renders the sheet live.

FINISH: unreviewed and undocumented is unfinished; this build ends with the finish review, the verdict, DESIGN.md, and every shipping raster carrying its provenance

## Cited adaptations

- Chord numbers are set in Big Shoulders Display 900, not marker: Permanent Marker's numeral 1 reads as a bracket at chart size, and the numbers are the most-read glyphs. Marker stays for MD notes, titles and readouts.
- The editable source is a dashed field on the floor, not tape: a textarea needs its input affordance.
- Long copy (lede, section text, facts) sits on the floor, not tape, for reading comfort.
