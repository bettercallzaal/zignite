# zignite

ZAO research into **Ignite Radio** ([igniteradio.xyz](https://igniteradio.xyz/)) -
what they built, how it works, and what any of it means for us.

Research first. No product decision has been made.

## What we actually know

Everything below came from a raw fetch of the homepage on 2026-08-12. It is the
site's own words, quoted, because that is all that is currently verified:

> Sui's #1 hit music station. Stream live channels, discover artists, and tip
> your favorites - powered by the Sui blockchain.

That is the whole verified set. The homepage is a **JavaScript shell** - 3,816
bytes, and the only text that renders without executing JS is the title. So the
channels, the artist pages, the tipping flow and the token mechanics are all
still unknown, and nothing in this repo should claim otherwise until someone has
actually loaded the app.

## Why ZAO is looking

It is our thesis on someone else's chain. Live channels, artist discovery and
direct fan-to-artist tipping is close to what The ZAO, WaveWarZ and ZAOstock are
each reaching for from different directions. They chose **Sui**; we are on Base
and Solana.

The interesting questions are therefore not "should we copy this" but:

- What does Sui give them that Base or Solana would not?
- Does the tipping actually reach artists, and what does it cost per tip?
- Where does the music come from - licensed, uploaded, or artist-submitted?
- Is there a real audience, or a landing page and a roadmap?
- Is the honest move to compete, to integrate, or to leave it alone?

## Open questions

| # | Question | Status |
|---|---|---|
| 1 | What does the app look like past the JS shell? | unanswered |
| 2 | Tipping mechanics - token, fee, custody, artist payout | unanswered |
| 3 | Where the catalogue comes from and who cleared it | unanswered |
| 4 | Real usage - listeners, artists, tip volume | unanswered |
| 5 | Who is behind it, and are they reachable | unanswered |
| 6 | Compete / integrate / ignore | blocked on 1-5 |

## How to work in here

- **Quote sources, do not paraphrase them.** `WebFetch` returns a small model's
  summary of a page, not the page - never quote from it. Use `curl` plus an HTML
  strip, an official API, or `/browse` for anything JS-rendered.
- **Mark every claim FULL / PARTIAL / FAILED** by how well it was actually
  fetched, per `.claude/rules/research-grounding.md` in ZAOOS.
- **An unknown stays written as "unknown."** Do not fill a gap with something
  plausible.

## Where the findings end up

Working material lives here. The durable finding belongs in ZAOOS
`research/music/` as a numbered doc, because research is the one thing that
never graduates out of the monorepo - it is the institutional memory across
every ZAO product. This repo is the workbench; ZAOOS is the record.

## Status

Created 2026-08-12. Nothing researched yet beyond the tagline above.
