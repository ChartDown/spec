# Product

## Platform

web

## Stack

Static HTML/CSS/JS, no build step, served by GitHub Pages from this repo (chartdown.github.io/spec). Same approach as `playground/index.html`, whose parser can render real charts on other pages.

## Users

- **Primary: worship team music directors and players.** A volunteer or staff MD prepares charts during the week, and the band reads them at rehearsal and on Sunday, often from a phone or tablet on a music stand. They already think in the Nashville Number System, or are learning it.
- Other working bands (cover bands, session players, songwriters) who use number charts.
- Developers who might build a ChartDown tool (they need the spec, the JSON form, and the conformance suite).

## Product Purpose

ChartDown is an open plain-text notation for Nashville Number System band charts: "Markdown for band charts". A chart is a `.chartdown` text file anyone can type, read, diff and share. Success for the launch (8 weeks): visitors try the playground, write and share their own charts, open issues, star the repo, and at least one third-party tool adopts the format.

## Positioning

- Music director cues are part of the notation: `> MD drums:` notes and `{bass: walk up}` cues target one player, so each player can see only their part.
- It is an open spec (CC0) with a formal grammar, a JSON form and a conformance suite, not an app's file format.
- Plain text first: readable with no software, transposable by any compatible tool because it is written in numbers.

## Operating Context

Chart prep at a desk during the week; reading on stage or in rehearsal under stage light; changing key on the day for a different singer; passing charts around by message or link.

## Capabilities and Constraints

- Current spec version 0.7 (draft). Whether launch is 0.x or 1.0 is undecided.
- Reference playground at https://chartdown.github.io/spec/playground/ renders, transposes, switches numbers/letters, and filters by player.
- Languages: US English and Mexican Spanish, both first-class (guides exist as `docs/guide.md` and `docs/guia.md`).
- Do not mention ChartDown App (the commercial app) on the language's pages.

## Brand Commitments

- Name: ChartDown. Tagline: "Markdown for band charts" / "Markdown para charts de banda".
- Voice: plain, short sentences; band vocabulary (MD, pass, push, stab, tag), no hype.

## Evidence on Hand

- Four original example songs in `examples/` (Open Road, Lantern Bay, Northbound, Syntax Tour).
- The spec, two guides, 142 conformance cases.
- No users, testimonials, star counts or third-party tools yet: do not invent any.
- Lyric landmarks must be original and at most 8 words; no copyrighted songs.

## Product Principles

1. The text is the product: every page should show real ChartDown text, not describe it.
2. Respect the band's language: use the words players already say at rehearsal.
3. Open by default: anyone can build on it, so lead to the spec and the suite, never to a store.
4. Bilingual as equals: English and Spanish readers get the same page.

## Accessibility & Inclusion

Readable on phones at a music stand; real text, not images of charts; respects reduced motion.
