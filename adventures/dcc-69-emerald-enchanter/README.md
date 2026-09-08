# DCC #69: The Emerald Enchanter

Adventure content for Dungeon Crawl Classics #69 (plus its bonus follow-up, "The Emerald Enchanter Strikes Back"), prepped for use with DCC Suite.

- **`DCC69_The_Emerald_Enchanter.md`** — clean Markdown transcription of the adventure PDF, restructured with headers per area and stat blocks pulled into blockquotes, with inline "Party of 3" scaling callouts for a wizard/thief/cleric trio. Import this directly through GM Notes' existing file-import feature (`gm-notes-modal.html` already accepts `.md`/`.markdown`/`.txt`) — no code changes needed to use it as-is.
- **`emerald_enchanter_areas.json`** — all 45 areas across both documents, extracted as structured data (`doc`, `level`, `id`, `title`, `blurb`). Feeds the Cartographer's Pins tool below, and is reusable by any future in-app feature that needs to look up a room by number without re-parsing the markdown.
- **`map-pins-notes.md`** — design notes and a code sketch for a possible future "map pins" modal (numbered room markers on a battle map, linked back to the matching note). Not wired into the app — see that file for what's still open.

## The map-pin workflow

1. Rebuild the PDF's maps in Dungeon Alchemist (there's no way to carry over exact coordinates from the old PDF layout — this part is manual).
2. Export the map image, then place your numbered room pins with **Cartographer's Pins**: https://claude.ai/code/artifact/2ee2bd22-e980-42c7-b1a9-7744042039cf — upload the image, pick a room from the list (loaded from `emerald_enchanter_areas.json`'s data), click to place, drag to nudge, copy the resulting JSON.
3. That JSON is the input `map-pins-notes.md`'s sketch expects, once that feature actually gets built into DCC Suite.
