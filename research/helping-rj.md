# How ZAO can actually help Rj

**Written:** 2026-08-15
**Frame:** Ignite Radio sponsors WaveWarZ. This is reciprocity, not evaluation.
**Background:** ZAOOS `research/music/2269-ignite-radio-teardown/` and
`research/ignite-station-prep.md` in this repo.

---

## Who Rj is, from the evidence

| | |
|---|---|
| Handle | `rjsmithy`, display name "Rj" |
| Station | **R&Rj**, 106.1 FM, genre Mixed, created 2026-06-08 |
| Bio | *"Let's make our own story in the stars!"* / *"Broadcasting live on R&Rj"* |
| Catalogue | 6 original tracks - Sacred Flame (Nessy + Rj), Easy Now, Glitch, Lighthouse, Something in the distance, Beauty in the feathers |
| Reach | 3 unique listeners, 12 likes, 3 fanclub members |
| Community | joined 4 other stations' fanclubs - one of the most active members on the platform |
| Profile | **unverified, and every social link is blank** - x, instagram, discord, website all empty |

**She is more than a station owner there.** The platform's AI concierge is named after
her and is hardcoded into the app chrome on every page:

> `const xh = "Lady Rj"` - *"Hey, I'm Lady Rj, your Ignite guide. Ask me anything -
> claiming your channel, uploading music, going live, or finding your way around the
> dial."*

So Rj is the front-of-house personality of Ignite Radio. Helping her is helping the
platform, and vice versa.

One thing this doc does **not** claim: her exact relationship to Harmony Hub, the entity
named as operator in Ignite's Terms. The contact addresses there belong to Harmony Hub
accounts, not to `rjsmithy`. Whether Rj is the owner, a co-founder, or the host is
**unknown** and worth simply asking rather than assuming.

---

## 1. Right now: her site has been down for days, and she may not know

**Unreachable on every check from 2026-08-14 through 2026-08-16.** This is not a blip.

```
DNS        igniteradio.xyz -> 18.204.152.241   (resolves fine, AWS)
TCP        port 443 OPEN, port 80 OPEN         (the box is up)
HTTP       no response, connection times out    (the app is wedged)
harmonyhub.love (same operator, different host) -> HTTP 200 in 0.32s
```

**The server is accepting connections but the application process is not answering.**
That is a hung process, not DNS, not a dead host, not a firewall. It almost certainly
clears with a restart of the app process.

Two things make this worse than it looks: their whole platform is one process on one box
with a SQLite file behind it, so there is no second instance to absorb this. And their
own `/api/health` tracks `lastCrashAt` and `crashes24h`, which means crashes are a known,
recurring thing they already instrument for.

**This is the single most useful thing ZAO can say to Rj today**, and it costs nothing.
It is also the kind of message that proves a partner is actually paying attention.

A multi-day outage nobody reported is itself the clearest possible evidence of the
audience problem in section 3: if a platform can be down for days without anyone raising
it, almost nobody was trying to listen. That is worth understanding gently rather than
saying out loud - but it is the thing to fix.

## 2. Then: the things that are broken and cost her money

All verified against their live build, all in `research/founder-share.md`, **all still
undelivered.**

- **The Tip button cannot work.** The modal binds to `station?.owner_id`, which the public
  API never returns, so it always falls through to *"this station owner hasn't linked a
  Sui wallet yet."* Tipping is in their own marketing copy and it has never fired for
  anyone, on any platform. No point has ever been tipped because it cannot be.
- **A static `x-api-key` ships in their client bundle**, attached to write endpoints
  including `POST /api/stations`, `/api/profiles` and `/api/claim`. The value stays out of
  every repo and gets handed over privately.
- **Tips are unmeasurable by design** - bare SUI transfers with no contract, memo or
  event - so `totalEarnings` reads 0 for every artist on the platform and structurally
  always will. Every artist there, Rj included, sees zero earnings whether or not anyone
  supported them. This is the highest-value item and it is a *design suggestion*, not a
  bug report: keep the transfer, add an event.

**Delivery has become time-sensitive for a reason that is our fault, not hers.** That
founder note is sitting in a public repo (`bettercallzaal/ZAOOS` is public) and has been
readable since 2026-08-13. Publication overtook disclosure. The remedy is speed, not
redaction - public git history cannot be un-fetched.

## 3. The real help: audience, because that is her actual constraint

The teardown's core finding is that Ignite's problem is not product or technology. It is
that almost nobody is listening. Across the whole platform, 72 fanclub memberships resolve
to **13 distinct people**, and the operator's own two accounts hold 34 of those. Rj's own
station has 3 listeners and 3 club members - two of whom are operator accounts.

She does not need a better player. She needs people.

That is exactly what a sponsorship should buy in the other direction, and it is what ZAO
actually has:

- **Embed the Ignite player on ZAO surfaces.** They ship an Embed Builder specifically for
  this. zabalgamez.com, thezao.com, a ZAOstock page - each one is a real front door.
- **ZAOstock, Oct 3, Franklin St Parklet.** A live event with an audience is the single
  biggest audience-transfer opportunity on the calendar. Ignite is a WaveWarZ sponsor;
  putting the dial in front of that crowd is straightforward reciprocity.
- **The newsletter and /zabal casts.** One honest "here is where our artists are
  broadcasting" beats any amount of platform polish.
- **Cross-follow.** Her fanclub has 3 members. ZAO people joining costs nothing and moves
  a number that is currently indistinguishable from zero.

## 4. Supply that pulls audience

Covered in `research/ignite-station-prep.md`. The short version: a WaveWarZ station and a
ZAO station, seeded from Audius with no uploads (`@WaveWarzAfrica` 10 tracks,
`@BennyJ504WaveWarz` 25), plus the 28 ZABAL GAMEZ workshops into the **AM side, which is
effectively empty** - 3 podcast tracks exist platform-wide.

Two ZAO stations broadcasting is a bigger vote of confidence than any message.

## 5. Small things she can fix herself in ten minutes

Worth mentioning gently, because each one silently leaks the audience she does have:

- **Her own profile has no links at all** - x, instagram, discord and website are all
  blank. Anyone who finds R&Rj has nowhere to go next.
- **She is unverified** on her own platform (1 of 53 profiles is verified).
- **The Discord invite is dead.** `discord.gg/igniteradio` returns
  `{"message": "Unknown Invite", "code": 10006}` from Discord's own API. It is in the
  footer and the nav of every page, so every interested visitor hits a wall.
- **`@igniteradio_sui` could not be confirmed to exist.** Two methods failed. Marked
  unverified rather than deleted, since logged-out X blocks are common - but worth her
  checking.

## What not to do

- **Do not open with the full defect list.** She is one person and her server is currently
  wedged. Lead with the outage, then the tip button, and let the rest follow when there is
  appetite for it.
- **Do not build it for them.** Offer the finding and the fix; the code is theirs.
- **Do not lead with the Harmony Hub observations.** The seeded demo profiles marked
  `isVerified: true` with fabricated earnings, and the placeholder provenance hash, are
  real but they are about the parent product, they are marked PARTIAL, and they are the
  wrong opening move with a sponsor. That conversation happens later, privately, or not at
  all.
- **Do not treat any of this as leverage.** They sponsor WaveWarZ. Help is the point.

## The order

1. Tell her the site is down, with the diagnosis above. Today.
2. Send the founder note privately - tip button, API key, the tip-event suggestion.
3. Offer the audience: embed, ZAOstock, newsletter, cross-follow.
4. Stand up the two ZAO stations once the site is back.
5. Ask, rather than assume, what she actually needs - this whole document is inference
   from the outside, and she knows things none of it can.
