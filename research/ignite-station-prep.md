# Ignite Radio - station prep sheet

**Prepared:** 2026-08-14
**For:** standing up a WaveWarZ station and a ZAO station on igniteradio.xyz
**State:** igniteradio.xyz is DOWN as of writing (HTTP 000, ~12s timeout, repeated).
Nothing below can be executed until it returns. Everything is prepped so it is a
short job when it does.

Context: Ignite Radio sponsors WaveWarZ and is part of the onchain music family.
The teardown is ZAOOS `research/music/2269-ignite-radio-teardown/`.

---

## The two constraints that decide the shape

Both read from their shipped Studio code, not assumed.

**1. One owner = one station.** The create handler catches a server error code
`OWNER_HAS_CHANNEL` and responds *"You already have a channel - opening it."* A single
account can never hold both stations. Two stations means **two owner accounts**. Sign-in
accepts plain email as well as wallet, so a second account is trivial.

**2. You cannot self-serve an AM station.** The create payload is:

```js
{ id, owner_id, name, genre, frequency, artwork_url, banner_url, songs_json, links_json, is_live }
```

There is **no `band` field**, and the client hardcodes `band:"FM"` on every station it
puts into the directory. That is why all 32 existing stations are FM and **zero are AM**.

Correction worth recording: the "540-1600 kHz" figures are decorative step markers on the
CPM Collabs page (step 01 "TUNE IN" at 540, step 06 "MINT" at 1600). They are not a real
AM dial that can be claimed.

## So AM is a per-track dial, not a station

The Podcasts and Audiobooks pages sweep **every** station's `songs_json` and filter on
`contentType`:

```js
.filter(n => n.contentType === "audiobook" || String(n.category||"").toLowerCase() === "audiobook")
if (a.contentType === "podcast") { ... }
```

Dials: `Music` / `Instrumental` / `Podcast` / `Audiobook`. FM is Music; AM is Beats &
Spoken.

**The shape is two stations, not four.** Each publishes music tracks (FM dial) *and*
instrumental/podcast/audiobook tracks (AM dials). Asking for "AM and FM for both brands"
is satisfied by two stations, correctly tagged.

## The opening

Platform-wide there are 338 music tracks but only **13 instrumentals and 3 podcasts**.
The AM side is effectively unoccupied, and ZAO has more spoken-word catalogue sitting
idle than the whole platform has published.

---

## Station 1 - WaveWarZ

| Field | Proposed |
|---|---|
| Name | WaveWarZ Radio |
| Genre | Afrobeats (their genre list is free text; "Hip Hop" or "Alternative" also fit) |
| Frequency | **104.9** (free) - alternates: 101.1, 105.1, 99.5 |
| Bio | Live music battles you can trade. The artists who fight for the wave, on rotation between rounds. |
| Links | wavewarz.com, the WaveWarZ Farcaster/X |

**FM fill - zero uploads needed.** Studio has *"Import catalog from Audius - pull your
existing Audius tracks straight into this channel, audio and all"*
(`GET /api/import/audius/preview?handle=` then `POST /api/import/audius`):

| Audius handle | Tracks | Artists |
|---|---|---|
| `@WaveWarzAfrica` | 10 | N3M3SIS, kiyomi, Shikulu Bantu - incl. "COMMANDER ZAAL" |
| `@BennyJ504WaveWarz` | 25 | BennyJ504 - already runs a station on Ignite |

Note `nemesis100` and `nemesis6` already hold Ignite profiles, so some of these artists
are on both platforms already.

## Station 2 - ZAO

| Field | Proposed |
|---|---|
| Name | ZAO Radio |
| Genre | Mixed / Community |
| Frequency | **100.0** (free; off-grid evens are already in use - 90.0 and 108.0 both exist) - alternates: 100.1, 96.5, 107.7 |
| Bio | The ZAO - independent artists, builders and the people who show up. Music on FM, the workshops and rooms on AM. |
| Links | thezao.com, zabalgamez.com, the ZAO Farcaster |

**FM fill:** `@thezaodao` - 2 tracks by IMan Afrikah ("COC V2", "BetterCallZaal"). Thin,
so this station leans on AM.

**AM fill:** the ZABAL GAMEZ workshop catalogue - see below.

---

## The AM catalogue - 28 workshops, extraction in progress

`data/recaps.json` in the zabalgames repo lists **32 Season 1 recordings**, of which
**28 carry a YouTube URL**. All are type `workshop`, each with a full transcript, summary,
takeaways and topic list already written - so episode descriptions are a copy-paste, not
a writing job.

Extraction pipeline, validated:

```bash
yt-dlp -f 18 -x --audio-format mp3 --audio-quality 128K --no-update -q "<url>"
```

Measured on the first one (Ceci / Unlock Protocol): **24 seconds, 29.7 MB, 32 minutes** -
comfortably inside Ignite's **MP3 only, 100 MB** cap. 28 items runs in roughly ten
minutes and lands about 800 MB.

One wrinkle worth knowing: YouTube's SABR experiment is currently hiding audio-only
formats from the installed yt-dlp (2026-02-21, about six months stale), so format `18`
(360p mp4) is fetched and the audio extracted locally. Updating yt-dlp would likely
restore the smaller audio-only path, but that is a change to Zaal's tooling and other
lanes use it, so it was left alone.

Upload each as `contentType: podcast` so they land on the AM/Podcasts dial.

**The 4 recordings with no video** - these need a source before they can go up:

| Recording | Date | Transcript |
|---|---|---|
| ZABAL Gamez AMA with The Farcaster Intern | 2026-06-22 | no |
| Selling merch onchain | 2026-06-21 | no |
| Farcaster Batches, and the builders behind it | 2026-06-20 | no |
| Bonfires + a vibe-coding masterclass | 2026-06-06 | yes |

---

## Setup checklist, per station

1. Sign in (email or wallet) - **a separate account per station**, because of
   `OWNER_HAS_CHANNEL`.
2. Accept the **Uploader Agreement** - one-time, blocks publishing until accepted:
   *"I own or have the rights to everything I upload."*
3. Set artist name, bio, genre, frequency.
4. Upload artwork + **banner at 2000x800**.
5. First track - **MP3 only, 100 MB max**.
6. Optional per track: **ISRC** (12 chars; DistroKid/TuneCore/CD Baby assign one free)
   and **songwriter splits**, which must total 100.
7. Grab the Studio **referral link** - listeners arriving through it are credited to the
   station.

## Two things to decide before pulling the trigger

**Audius re-hosting.** All 10 `@WaveWarzAfrica` tracks are `is_downloadable: false` with
no license set. Ignite's importer copies audio onto their own server regardless - that is
how 42% of their catalogue got there. It is ZAO's own catalogue so it is Zaal's call, but
it is the same re-hosting question already raised in the founder-share, and it is better
decided than defaulted into.

**Frequency is a vanity field.** Weakly validated - existing stations include `90.0`,
`108.0` and literally `702.0`. 32 taken, 71 grid slots free. 88.1-91.7 is packed solid;
everything from 91.9 up is open.

## Next, once they are back up

- Confirm `OWNER_HAS_CHANNEL` really is server-enforced by attempting a second station on
  one account (it is a caught client-side error code, so the server behaviour is inferred,
  not observed).
- Test `GET /api/import/audius/preview?handle=WaveWarzAfrica` to see exactly what the
  importer offers before committing.
- Ask the founder directly whether a genuine AM band entry is possible - that is a partner
  conversation, not a self-serve action, and it pairs with the founder-share that is still
  pending delivery.
