# zignite

ZAO research into **Ignite Radio** ([igniteradio.xyz](https://igniteradio.xyz/)) -
what they built, how it works, and what any of it means for us.

Research first. No product decision has been made.

## What we actually know

The homepage is a **JavaScript shell** - 3,816 bytes, and the only text that
renders without executing JS is the title. It has since been loaded properly in
a headless browser, and the app behind it read directly from its own public REST
API, its shipped JS bundle, and public chain RPC.

**The teardown is in [`research/ignite-radio-teardown.md`](research/ignite-radio-teardown.md).**
Raw API and on-chain snapshots are in `evidence/`.

Headline: a real, working platform with genuine creator supply, effectively zero
listeners, effectively zero revenue, and a tipping feature that is broken in
production. It runs on SQLite and a filesystem, and has no Move contract
deployed at all.

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
| 1 | What does the app look like past the JS shell? | answered - full product, SQLite backend |
| 2 | Tipping mechanics - token, fee, custody, artist payout | answered - bare SUI transfer, no contract, 0% fee, non-custodial, and currently broken |
| 3 | Where the catalogue comes from and who cleared it | answered - 45% creator upload, 42% re-hosted Audius, rights self-attested |
| 4 | Real usage - listeners, artists, tip volume | answered - 32 stations and 356 tracks vs 28 lifetime likes and a near-empty treasury; tip volume structurally unmeasurable |
| 5 | Who is behind it, and are they reachable | answered - operated by Harmony Hub, reachable by email, Discord dead |
| 6 | Compete / integrate / ignore | integrate, cheaply and specifically - see teardown |

Remaining unknowns are listed at the end of the teardown and stay written as
unknown.

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

Created 2026-08-12. Teardown completed 2026-08-12; all six questions answered.
The durable doc has **not** been filed into ZAOOS `research/music/` yet, because
ZAOOS is not cloned on this machine and the doc number could not be assigned.
