# Map Pins — original design notes (now built)

**This has been built.** See `../../map-pins-modal.html`, opened from the Sheet popover's GM-only **Pins** button (`index.html`'s `openMapPinsModal`). The right-click "Open Room Notes" jump described below also shipped, in `overlay.html` + `gm-notes-modal.html`. This file is kept as-is for historical context on the original sketch, not as a to-do list — see those files' own comments for how the shipped version actually works (it differs in a few places, notably calibration being a manual test-pin step rather than trusted math, and the jump using a broadcast channel to reach an already-open GM Notes modal).

The rest of this file is the original sketch, unedited:

This is a sketch for a possible future feature, not a working file — Crawl-Bear-Classics keeps every feature self-contained inside its own HTML file's `<script type="module">` block (see `CLAUDE.md`), so this would need to live inside a new modal (e.g. `map-pins-modal.html`, opened the same way `gm-notes-modal.html` and `rulebook-data-builder.html` are) rather than as a standalone `.js` file.

It bulk-creates numbered room-pin labels on the current scene from a JSON export produced by the "Cartographer's Pins" tool (a small companion web tool: upload a Dungeon Alchemist map export, click to drop numbered pins matching the adventure's area numbers, export their positions as percentages of the image). Matches this repo's existing conventions: SDK imported from `esm.sh` the same way `index.html` does, and the `com.claude.monster-sheet/` metadata prefix already used by `gm-notes-modal.html` and `rulebook-data-builder.html`, so pin data stays in the same metadata namespace as the rest of DCC Suite.

```js
import OBR, { buildLabel } from "https://esm.sh/@owlbear-rodeo/sdk@2";

const METADATA_KEY = "com.claude.monster-sheet/room-pin";

/**
 * pins.json comes straight out of the Cartographer's Pins tool:
 * [{ doc, level, id, title, x, y }, ...] where x/y are 0-100 percentages
 * of the map IMAGE's own width/height (not the scene's pixel grid).
 */
async function importPins(pins, mapImageItem) {
  // mapImageItem = the OBR image item already placed on the map layer for
  // this scene (e.g. via OBR.scene.items.getItems(isImage), picked by name).
  //
  // An image item's `position` is its top-left in scene units, and its
  // rendered size is item.image.width/height * item.scale.x/y. Calibrate
  // this against one known pin before trusting a full batch — worth
  // double-checking against whatever Owlbear SDK version esm.sh resolves
  // to at build time, since anchor semantics have shifted across versions.
  const originX = mapImageItem.position.x;
  const originY = mapImageItem.position.y;
  const fullWidth = mapImageItem.image.width * mapImageItem.scale.x;
  const fullHeight = mapImageItem.image.height * mapImageItem.scale.y;

  const items = pins.map((pin) =>
    buildLabel()
      .position({
        x: originX + (pin.x / 100) * fullWidth,
        y: originY + (pin.y / 100) * fullHeight,
      })
      .plainText(pin.id) // shows "1-4" etc. right on the map
      .backgroundColor(levelColor(pin.level))
      .pointerDirection("DOWN")
      .metadata({ [METADATA_KEY]: { doc: pin.doc, level: pin.level, id: pin.id, title: pin.title } })
      .layer("NOTE") // own layer so the whole set can be hidden together
      .build()
  );

  await OBR.scene.items.addItems(items);
}

function levelColor(level) {
  const colors = {
    "The Citadel": "#1c7a52",
    "The Dungeons": "#6b5b8a",
    "The Creation Vats": "#1c6d7a",
    "The Overland Map": "#96701f",
    "Enter the Emerald Titan": "#a83f2b",
  };
  return colors[level] || "#1c7a52";
}

// A right-click action on a pin, so a GM can jump straight to that room's
// entry instead of getting Owlbear's generic label menu:
OBR.contextMenu.create({
  id: "com.claude.monster-sheet/open-room-pin",
  icons: [
    {
      icon: "/icon.svg", // reuse the suite's existing icon, or ship a small one
      label: "Open room notes",
      filter: { every: [{ key: ["metadata", METADATA_KEY], value: undefined, operator: "!=" }] },
    },
  ],
  onClick(context) {
    const room = context.items[0]?.metadata[METADATA_KEY];
    if (!room) return;
    // gm-notes-modal.html's notes are plain { id, title, content } markdown
    // strings under OBR.scene.setMetadata — the simplest hookup is finding
    // the note whose content contains `Area ${room.id} –` and scrolling the
    // GM Notes modal to that heading, rather than building a second parallel
    // lookup structure.
  },
});
```

## What's still open

- Whether this becomes its own modal or a panel inside `gm-notes-modal.html` (which already renders markdown and could scroll to a heading on request).
- The exact scroll-to-heading hookup into GM Notes' existing note viewer, once one exists to hook into.
- Calibrating the image-to-scene coordinate math against a real placed background image, since Owlbear's anchor behavior isn't something this sketch has been run against yet.

~~Ask for this to be built out as a real modal when you're ready — it's a genuine feature addition to DCC Suite, not just a content drop, so it's worth doing deliberately against the current `esm.sh` SDK version and the app's build-stamp/version conventions.~~ Done — see the note at the top of this file.
