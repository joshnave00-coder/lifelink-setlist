# Life Link Setlist

Private backup of the Life Link Church worship team rehearsal player, the Life Link Setlist app Josh Nave uses in Muse. Built Nov 2026.

## What is in here

- `index.html`: the app itself, one self-contained page. Set-list-first: it opens on the next service set list, shows each song in plan order with its scheduled key, and tapping a song expands it to versions, lyrics, and charts. It plays a whole set in order, follows lyrics while listening, supports tap-to-mark, and pitch-shifts tracks to the scheduled key. It saves nothing; that is the static copy. The live app in Josh's Muse is server-backed and saves markers and settings.
- `data/setlists.json`: the 12 recent set lists (Oct 11 to Nov 29, 2026) with Planning Center plan links and scheduled keys where verified. Keys not listed there should be read from the plan.
- `docs/life-link-setlist-build-guide.md`: the full guide for rebuilding this app in Muse from Planning Center, written for a band member (Holli Kelly, drums). It has a paste-ready prompt, the set lists, the feature list, and the gotchas.

## What is intentionally not in here

The audio recordings (122 files) and chart PDFs (43 files) are excluded on purpose. They are licensed through Planning Center (Multitracks audio, SongSelect charts) and are not ours to redistribute, even in a private repo. The app references them by filename only, for example `all-my-life-live-e.mp3`.

## Restoring the media

Sign in to Planning Center Services with your own login, open a plan at `https://services.planningcenteronline.com/plans/PLAN_ID`, open a song in the plan, go to its Files tab, and download the audio files and chart PDFs from Arrangement Attachments and Key Attachments. Place the files beside `index.html` using the exact filenames the app references. The song record's Files panel alone misses files, so always use the plan item's Files tab.

## Notes

- Worthy Of It All has no downloadable audio. The app uses its editor-text lyrics and links its three SongSelect chart PDFs.
- Gratitude and Believe For It full mixes are over 17 MB each.
- A band position heading in Planning Center holds multiple people under one heading.
- The plan shows each song's scheduled key; read it there instead of asking.
- If a song has no lyrics in Planning Center, ask the person for the words rather than leaving a gap.
