# DCC #69: The Emerald Enchanter

Adventure content for Dungeon Crawl Classics #69 (plus its bonus follow-up, "The Emerald Enchanter Strikes Back"), prepped for use with DCC Suite.

- **`DCC69_The_Emerald_Enchanter.md`** — clean Markdown transcription of the adventure PDF, restructured with headers per area and stat blocks pulled into blockquotes, with inline "Party of 3" scaling callouts for a wizard/thief/cleric trio. Import this directly through GM Notes' existing file-import feature (`gm-notes-modal.html` already accepts `.md`/`.markdown`/`.txt`) — no code changes needed to use it as-is.
- **`emerald_enchanter_areas.json`** — all 45 areas across both documents, extracted as structured data (`doc`, `level`, `id`, `title`, `blurb`). This is the "Rooms" list Map Pins expects in step 3 below, and is reusable by any future in-app feature that needs to look up a room by number without re-parsing the markdown.
- **`map-pins-notes.md`** — the original design notes and code sketch for the "map pins" modal, kept for context now that it's built as `../../map-pins-modal.html` (opened from the Sheet popover's GM-only **Pins** button).

## The map-pin workflow

1. Rebuild the PDF's maps in Dungeon Alchemist (there's no way to carry over exact coordinates from the old PDF layout — this part is manual).
2. Export the map image and place it on your Owlbear scene the normal way.
3. Open DCC Suite's **Pins** button (GM-only) → pick that image in step 1 → paste `emerald_enchanter_areas.json`'s contents into step 2 (Rooms) and Load → click each room's actual spot directly on the map image shown in step 3, one at a time → **Import Pins**. No separate tool or JSON round-trip needed — pin placement happens right there against the real map image.
4. Right-click any placed pin afterward for an "Open Room Notes" jump straight to that area's `### Area <id> –` heading in GM Notes.
