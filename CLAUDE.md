# Crawl Bear Classics

DCC Suite: an Owlbear Rodeo (virtual tabletop) extension for Dungeon Crawl Classics RPG — monster/PC character sheets, initiative tracker, dice roller with tumbling-die animation, and a shared roll log. Static site, no framework, no build step, hosted on GitHub Pages.

## Commands

No build, test, or lint tooling — plain static HTML/CSS/JS files served as-is.
- Quick UI check: open `index.html` directly in a browser (OBR-dependent features like token sync, broadcasts, and the dice modal won't work outside Owlbear Rodeo).
- Full functional test: add this repo's GitHub Pages URL (or a local tunnel) as a custom extension in Owlbear Rodeo, per `manifest.json`.

## Workflow

- All project files live in this local folder: `/Users/guydelanoye/Documents/GitHub/Crawl-Bear-Classics`. Always read from and save changes here.
- This is a local Git repo, remote `origin` on GitHub. After committing, run `git push` automatically (plain push to the current branch's tracked upstream — no force, no `--no-verify`) so GitHub Pages redeploys without the user needing to sync manually via GitHub Desktop.
- After making a meaningful change, commit it to git with a clear, descriptive message so every version is tracked automatically, then push. Small logical changes = small separate commits, not one giant commit at the end.
- Do not create commits for exploratory reads or failed attempts — only for changes actually saved to the project.

## Architecture

<!-- Where things live, so Claude doesn't have to rediscover it every session. -->
- `manifest.json`: Owlbear Rodeo extension manifest. `action.popover` → `index.html` (the main panel), `background_url` → `overlay.html` (headless).
- `index.html`: The entire app (~10.5k lines) — all CSS, markup, and JS logic in one `<script type="module">`. Four tabs: Sheet (monster/PC editor), Tracker (initiative), Dice (roller), Log (shared roll history). Uses `@owlbear-rodeo/sdk` for everything — scene item storage (`OBR.scene.items`), sync/messaging (`OBR.broadcast`), and modals (`OBR.modal`). No backend; character data is persisted as JSON metadata directly on OBR tokens.
- `overlay.html`: Invisible background page. Listens for the `dice-roll` broadcast channel and opens a fullscreen `OBR.modal` pointing at `dice-modal.html` to show the tumble animation over the map.
- `dice-modal.html`: The dice-tumble animation itself — each die shape drawn via CSS `clip-path`, animated, self-closes on a timer.
- `_headers.txt`: CORS header (`Access-Control-Allow-Origin: *`) for GitHub Pages hosting, needed since OBR loads these pages cross-origin.
- `icon.svg`: Extension icon referenced by `manifest.json`.

## Conventions

<!-- Team/personal standards that aren't obvious from the code itself. -->
- Sheet logic aims for close fidelity to the DCC RPG rulebook — auto-calculated fields (Crit die, Deed Die, Action Dice, saves, thief skills, etc.) cite the specific table/page they implement. Preserve those citations and the underlying rule when editing this logic.
- Comments explain *why*, not *what* (e.g. why a stale value falls back silently instead of erroring, why a migration only runs under specific conditions). Keep that pattern — don't restate obvious code in comments.
- No separate CSS/JS files — new UI or logic goes into the relevant section of `index.html` alongside the existing style block / script module, matching the file's existing organization.

## Repo etiquette

<!-- Branching, committing, versioning habits — since you copy new versions into this folder. -->
- Branch naming: work happens directly on `main`; no branching convention observed.
- Commit style: version bump commits are titled with just the version number (e.g. `1.31.0`), matching `manifest.json`'s `version` field bumped in the same commit. Feature/fix commits use short imperative or descriptive titles (e.g. `Spells`, `feat: add styles for poison status and weapons`).
- Push after every commit (see Workflow above) — GitHub Desktop is no longer the sync path.

## Notes

<!-- Anything surprising, off-limits, or easy to get wrong. -->
- `manifest.json`'s `action.icon`, `action.popover`, and `background_url` are hardcoded absolute GitHub Pages URLs (`https://ostendpirate.github.io/Crawl-Bear-Classics/...`), not relative paths — Owlbear Rodeo fetches the live published site, so changes aren't visible in OBR until pushed and deployed, not just saved locally.
- Metadata keys on OBR scene items (e.g. `com.claude.monster-sheet/dcc-data`) are the actual data schema/contract — renaming or restructuring one is a breaking change for any token saved under the old key. Existing code already carries one-time migration shims for past schema changes (e.g. the old single `equipment` blob → split Armor/Weapons/Gear/Food/Treasure fields); follow that pattern rather than silently dropping old data.
- `index.html` is very large (~10.5k lines); grep/search rather than reading it linearly.
