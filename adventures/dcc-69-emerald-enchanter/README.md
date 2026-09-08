# DCC #69: The Emerald Enchanter

Adventure content for Dungeon Crawl Classics #69 (plus its bonus follow-up, "The Emerald Enchanter Strikes Back"), prepped for use with DCC Suite.

- **`DCC69_The_Emerald_Enchanter.md`** — clean Markdown transcription of the adventure PDF, restructured with headers per area and stat blocks pulled into blockquotes, with inline "Party of 3" scaling callouts for a wizard/thief/cleric trio. Import this directly through GM Notes' existing file-import feature (`gm-notes-modal.html` already accepts `.md`/`.markdown`/`.txt`) — no code changes needed to use it as-is.
- **`emerald_enchanter_areas.json`** — all 45 areas across both documents, extracted as structured data (`doc`, `level`, `id`, `title`, `blurb`). Feeds the Cartographer's Pins tool below, and is reusable by any future in-app feature that needs to look up a room by number without re-parsing the markdown.
- **`map-pins-notes.md`** — the original design notes and code sketch for the "map pins" modal, kept for context now that it's built as `../../map-pins-modal.html` (opened from the Sheet popover's GM-only **Pins** button).

## The map-pin workflow

1. Rebuild the PDF's maps in Dungeon Alchemist (there's no way to carry over exact coordinates from the old PDF layout — this part is manual).
2. Export the map image, then place your numbered room pins with **Cartographer's Pins**: https://claude.ai/code/artifact/2ee2bd22-e980-42c7-b1a9-7744042039cf — upload the image, pick a room from the list (loaded from `emerald_enchanter_areas.json`'s data), click to place, drag to nudge, copy the resulting JSON.
3. In Owlbear, place that same map image on the scene, open DCC Suite's **Pins** button (GM-only), pick the image, paste in that JSON, calibrate with a test pin if the coordinates land off, then Import. Right-click any placed pin afterward for an "Open Room Notes" jump straight to that area's `### Area <id> –` heading in GM Notes.
