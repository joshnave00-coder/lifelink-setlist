# Life Link Setlist: exact build spec

This document is the complete specification for rebuilding Josh Nave's Life Link Setlist app exactly as it is built. It is written so that another person's Muse, pointed at this repo, can build a perfect replica in their own Muse. The reference implementation is `index.html` in this repo: a working static copy of the app. Build the replica to match it screen for screen, then read the "Live app differences" section for the two things a fresh build must add (saving markers and settings, which need a server-backed build in their own Muse).

## 1. The prompt to paste to your Muse

Paste this exactly, then follow up with anything it asks. Make sure your Muse can see this whole repo, especially this file, `data/songs.json`, `data/setlists.json`, and `index.html`.

> Build me a rehearsal player app called Life Link Setlist for the Life Link Church worship team, as an exact replica of the app in this repo. Read docs/life-link-setlist-build-spec.md and follow it exactly: same layout, same theme, same graphics, same pictures, same behavior. Use data/songs.json as the song catalog (29 songs, 122 recordings, lyrics, charts, album art) and data/setlists.json as the plan list. Use index.html as the reference implementation: render it and match it screen for screen. Pull the set lists, keys, recordings, lyrics, and charts from Planning Center Services at services.planningcenteronline.com, signing in with my Planning Center account. Build it as a server-backed app in my Muse so my markers and settings save, which is how Josh's live copy works.

## 2. What the app is

A rehearsal tool for the Life Link Church worship team, not a general music player. It opens on the next service set list. Each song shows its scheduled key from the Planning Center plan. Tapping a song expands it to its audio versions, lyrics, and charts. It plays one version, plays a whole set in order, follows lyrics while listening, lets the singer tap to mark sections, and pitch-shifts any track to the scheduled key. Josh's live copy holds 29 songs, 122 recordings, follow-along lyrics for 17 songs, and 43 chart PDFs.

## 3. Design tokens (exact)

Copy these values, do not approximate them.

- Page background: `#f1f2f4`. Cards and song rows: white `#ffffff`.
- Gradient (buttons, key pills, play circles, header rule): teal `#22d6e4` to blue `#0399e5`, left to right.
- Title text: `#171717`. Body text: `#1a1a1a`. Secondary text: `#383838`. Muted text: `#6b6b6b`. Borders and dividers: `#e6e6e6`.
- Player screen (dark): background `#0e1316`, panels `#161d21` and `#20292e`.
- Font: `"Helvetica Neue", Helvetica, Arial, system-ui, -apple-system, sans-serif` everywhere.
- Corner radii: pill buttons and key chips `999px`; round play buttons and avatars `50%`; cards `16px`; inner panels `12px`; small controls `8px` and `6px`; large sheets `24px`.
- Logo: the real Life Link logo, `lifelinkchurch.com` mark (cross emblem plus "LifeLink CHURCH.COM" wordmark). It is embedded in `index.html` as a PNG data URI (MD5 `c1f4807168fbd1d5e2857316904836e2`, 300x150). Use that exact image, centered in the header, not a redraw.
- Title under the logo: "WORSHIP TEAM" in uppercase, bold, large. A short gradient rule sits directly under the title.

## 4. Home screen: the set view

This is the first screen and the one the app opens on. It opens on the next service set list by date, not on the newest or on a library.

- Header: centered Life Link logo, then "WORSHIP TEAM", then the gradient rule.
- Set navigator card: a white rounded card with a round previous button on the left and a round next button on the right. Center shows the short date (for example "Sun, Nov 15"), the full date underneath (for example "Sunday, November 15, 2026"), and a gradient "NEXT SERVICE" pill. There is also a set dropdown to jump to any loaded set.
- Song rows, one per song in plan order: a white rounded card with a grey number badge on the left (the position in the set), the song title in bold, a sub-line reading "N tracks · lyrics · chart" (with the real counts for that song), a single gradient pill on the right reading "Key of X" (the scheduled key for that set), and a right chevron. Tapping a row expands it in place.
- Below the songs: a "Run the whole set" card with a gradient button, and a "Play this set offline" card with a save button and a remove button.
- Footer hint text: "Tap a song to see its tracks, keys, lyrics and chart. Tap a track to open it in the player." plus a "Player & songs" link.

## 5. Expanded song

Tapping a song row expands it to show, in this order:

- Version rows: every audio version of the song (full mix and part tracks). Each row has a gradient play circle, the version label (for example "Full mix", "Tenor part", "Live version"), a sub-line (for example "Full mix, key of C"), and a download button. One version carries a "Set default" badge: the version the set uses by default (the `defaultId` for that song in that set).
- A lyrics row (Aa icon) that opens the follow-along lyrics for the song.
- Chart rows: one per chart PDF, with a chart icon, the chart label (for example "SongSelect Chart in D"), and "PDF · N pages". Tapping one opens the PDF viewer.
- The expanded song keeps the same key pill and layout as the collapsed row.

## 6. Player page ("Player & songs")

Tapping a track opens the player page. It has, top to bottom:

- A round spinning album art disc showing the song's cover picture and album credits underneath (artist and album, for example "Pat Barrett · Pat Barrett").
- The track title and sub-line (version label and key).
- A seek bar with thin marker ticks on it for the song's markers.
- Transport row: shuffle, previous, back 10 seconds, big gradient play/pause, forward 10 seconds, next, repeat.
- A speed and volume row.
- "Lyrics" and "Lyrics PDF" buttons.
- Collapsible sections in exactly this order: Live lyrics, Key and transpose, Song map, Song library.
  - Live lyrics: the follow-along lyrics for the current track (see section 7).
  - Key and transpose: shows the track key and the scheduled key, with real pitch shifting that keeps the tempo unchanged (a pitch-shifter worklet; transpose does not speed up or slow down the track).
  - Song map: tap-to-mark bubbles, one per section (Verse 1, Chorus, Bridge, and so on). Tapping a bubble while listening sets a marker there. Markers are drawn as thin hash ticks on the seek bar, and tapping a tick jumps playback to it.
  - Song library: the same track rows as the expanded song view, grouped by version, with download buttons.
- A full-screen follow-along lyrics view (see section 7), a words sheet for songs with editor-text lyrics, a PDF viewer for charts, and a mobile mini player pinned to the bottom (small art, title, sub-line, back, play, forward, next) that stays visible while browsing.
- Run the whole set: starting a set opens a four-beat count-in card (with dots, title, status, and a stop button), then a running banner while the set plays through in order.

## 7. Follow-along lyrics

- Lyrics are stored as timed lines: each line has a time, a section name, and the line text. Times are fractions of the track duration (0.0 to 1.0), not seconds.
- While a track plays, the current line is highlighted and the view follows it. Tapping any lyric line jumps playback to that part of the track.
- Every song must have lyrics. If Planning Center has no lyrics file for a song, ask the person for the words rather than leaving a gap.
- Worthy Of It All has no downloadable audio. The live app carries its editor-text lyrics as a words sheet and links its three SongSelect chart PDFs.

## 8. Behavior rules (exact, do not improvise)

- Markers belong to the song, not to one recording. A marker carries to another version of the same song only when that version's total length is within 5 seconds of the marked version. Further apart counts as a different version and keeps its own markers.
- Tap-to-mark: tapping a verse or chorus bubble while listening sets the marker at the current playback position. There is no automatic time marking.
- Pitch-shift: any track can be shifted to the scheduled key, and the tempo must not change.
- Run the whole set plays the set's songs in plan order, each on its set default version, with the four-beat count-in first.
- Offline: "Play this set offline" saves the set's audio for offline playback, with a visible save state and a remove button.
- Search: the song library is searchable by title.
- Set navigation: previous and next buttons move between loaded sets, and the set dropdown jumps to any set.
- Section order inside a song can be reordered by drag handle in the live app, and the order saves automatically.
- The live app saves markers, section order, transpose settings, and default picks per person. The static copy in this repo saves nothing; that is the one deliberate difference (see section 12).

## 9. Data model

All catalog data lives in `data/songs.json`. The shapes below are exact; use them as the schema.

- `songs`: 29 entries. Each has `name`, `versions` (an array of track ids), `art` (`file` and `credit`, for example `{"file": "build-my-life.jpg", "credit": "Pat Barrett · Pat Barrett"}`), and `charts` (an array of `{"file", "label", "pages"}`). Two songs have no album art (He Shall Be Praised and Sovereign); they show a letter tile with the song's first letter instead of a cover. Worthy Of It All is not in this catalog as an audio song; see section 10.
- `tracks`: 122 entries keyed by version id. Each has `title`, `base` (song name), `label` (one of Full mix, Full version, Live version, Original version, Transposed version, Demo, Octave Down demo, Tenor part, Alto part, Soprano part, Singers part, Bass part), `sub` (for example "Full mix, key of C"), `key` ("Key of C"), `keyName` ("C"), `dur` (length in seconds), `file` (for example "build-my-life-full-c.m4a"), and `lyricKey` (the key into `lyricSets`, or null). Track counts by label: Full mix 1, Full version 11, Live version 16, Original version 2, Transposed version 1, Demo 2, Octave Down demo 1, Tenor part 27, Alto part 21, Soprano part 21, Singers part 18, Bass part 1. Track counts by key: C 12, D 35, E 42, E flat 1, G 32.
- `sets`: 11 dated sets embedded in the static copy, Oct 18 through Nov 29, 2026. Each has `dateISO`, `dateLabel`, `shortLabel`, and `songs`, where each song entry has `title`, `note`, `bridgeTag` (true only for Sovereign on Nov 8, which is bridge tag only), `keyChips` (for example `["Key of E"]`), and `defaultId` (the version id the set plays by default).
- `lyricSets`: 17 entries keyed by `lyricKey`. Each is an array of `[timeFraction, section, line]`, for example `[0.02, "Verse 1", "Worthy of every song we could ever sing"]`. Times are fractions of the track duration.

## 10. Media inventory

- Audio: 122 recordings. Filenames follow the pattern `<song>-<version>-<key>.m4a` (for example `build-my-life-full-c.m4a`, `all-my-life-live-e.mp3`). They are not in this repo; restore them from Planning Center per the README. Worthy Of It All has no downloadable audio.
- Charts: 43 chart PDFs in total. 40 are file references in `data/songs.json` (26 songs have charts), and the other 3 are Worthy Of It All's SongSelect chart links. They are not in this repo; restore them from the plan item's Files tab in Planning Center, never from the song record's Files panel alone, which misses files.
- Album art: 24 cover pictures, one per song that has art (27 songs list art; some share a cover). They are embedded in `index.html` as JPEG data URIs, byte for byte, so the static copy needs no separate image files. A fresh build should use the same pictures: filenames are in `data/songs.json` under each song's `art.file`, and the bytes are the data URIs in `index.html`.
- Logo: embedded in `index.html` as a PNG data URI (MD5 `c1f4807168fbd1d5e2857316904836e2`). Use that exact image.

## 11. Set lists

Load the set lists from Planning Center. The verified plan list is in `data/setlists.json`: 12 plans, Oct 11 through Nov 29, 2026, each with its plan link and verified scheduled keys. The embedded set list in the static copy has 11 dated sets (Oct 18, Oct 21, Oct 25, Oct 28, Nov 1, Nov 8, Nov 11, Nov 15, Nov 18, Nov 22, Nov 29); if the live plans differ from that snapshot, the plans win.

- Nov 18, 2026, plan 92984955: Good Plans was removed from this set on Nov 13. Do not put it back.
- Nov 8, 2026, plan 92891802: Sovereign appears as bridge tag only.
- Nov 5, 2026, plan 92705223 (Man Camp, Gilbert Thursday): So So Good opens and closes the set.
- A band position heading in Planning Center holds multiple people under one heading; it is one slot, not separate slots.
- The plan shows each song's scheduled key; read it there instead of asking the person.

## 12. Live app differences (what a fresh build must add)

The bundled `index.html` is a static copy: it saves nothing. Josh's live app is server-backed and saves per-person markers, section order, transpose settings, and default picks. A replica built in someone else's Muse must be built server-backed the same way, so their markers and settings save. Everything else about the replica must match the static copy exactly.

## 13. What not to build

Do not build transposed chord charts, part blend, record-yourself, rehearsal recordings inside a set, plan notes in the player, changed-since-practiced flags, a one-page set sheet, a karaoke section, or a Stem Mixer. Josh declined all of these; they are not missing features.

## 14. Verification checklist (do this before calling it done)

- Render `index.html` from this repo at phone width (390 px) and compare screenshots against the fresh build at the same width: header (centered logo, WORSHIP TEAM, gradient rule), set navigator card, song rows with number badges and key pills, Run the whole set card, offline card. They must match.
- Open a song and compare the expanded view: version rows with gradient play circles, Set default badge, lyrics row, chart rows.
- Open the player and compare: spinning disc with cover and credits, seek bar with marker ticks, transport row, speed and volume row, Lyrics and Lyrics PDF buttons, and the four collapsible sections in order (Live lyrics, Key and transpose, Song map, Song library).
- Count: 29 songs, 122 recordings, 11 dated sets in the snapshot (12 plans in `data/setlists.json`), lyrics for 17 songs, 43 chart PDFs.
- Test: play a track and pitch-shift it to the scheduled key; the tempo must not change. Tap a verse bubble while listening; a thin hash tick must appear on the seek bar at that spot. Switch to another version of the same song within 5 seconds of length; the marker must carry over. Switch to a version more than 5 seconds different in length; it must keep its own markers. Run the whole set; the four-beat count-in must play first. Tap a lyric line; playback must jump to that part.
- Confirm no broken images anywhere: every cover renders, and the two songs without art show letter tiles.

## 15. Gotchas

- Worthy Of It All has no downloadable audio. Use its editor-text lyrics and link its three SongSelect chart PDFs.
- Gratitude and Believe For It full mixes are large, over 17 MB each; do not assume every file is small.
- The recordings and charts live on the plan items, not on the song records. Always use the plan item's Files tab (Arrangement Attachments and Key Attachments).
- If a song has no lyrics in Planning Center, ask the person for the words rather than leaving a gap.
- The app should keep working as new plans are added; the set-list section should let the person pick a date and get that set's songs and keys.

Written Nov 15, 2026, from Josh Nave's Life Link Setlist app (live copy in his Muse) and its static copy in this repo. Questions go to Josh.
