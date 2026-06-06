# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

This is **not a software project** — it is a local **reference data archive for Path of Exile 2**. There is no build, test, or lint step. The work here is reading, extracting, and answering questions from saved game data.

The owner is a PoE2 player (Thai speaker — questions may come in Thai; answer in Thai when asked in Thai) who uses this folder to look up game mechanics and plan builds.

## Where to get data — web first

**Prefer fetching the live web over reading the local `.html` dumps.** The owner explicitly wants latest data + low token cost. The local files were saved from these same sources and go stale.

- **Game data** → `https://poe2db.tw/us/` (the source of every `.html` here). WebFetch the relevant page.
- **The build** → `https://maxroll.gg/poe2/build-guides/spirit-walker-twisters` (Spirit Walker Twisters).

**Why web-first saves tokens:** WebFetch converts the page and returns only the answer to your prompt (~hundreds of tokens). `Read`-ing a local dump loads the entire raw HTML into context — the big files here are 700KB–2.8MB ≈ 200k+ tokens each (some too large to read whole). That's roughly a 100–300× difference.

Fall back to the local files (via **`Grep`**, not whole-file `Read`) only when offline, when the page won't fetch, or for an exact string lookup. The small hand-made build sheet (`poe2 change gear level/index.html`, ~21KB) is fine to `Read` directly.

## Contents

- **`*.html` (top level)** — Saved web pages from **PoE2DB** (`poe2db.tw`, English/`us` locale): the Path of Exile 2 data wiki. Each file is one category dump: weapon types (Wands, Crossbows, Spears, Daggers…), armour slots (Body Armours, Boots, Belts…), `Gems`/`Skill Gems`/`Support Gems`/`Spirit Gems`, `Unique_item`, `Modifiers`, `Keywords`, currency, and league/encounter mechanics (Breach, Delirium, Ritual, Expedition, Trial of the Sekhemas…). These are large (some multi‑MB) Bootstrap‑styled HTML pages.
- **`Modifiers/`** — A focused set of per‑item‑type **affix (modifier) pool** dumps from PoE2DB (e.g. `Bows`, `Quarterstaves`, `Two Hand Maces`, `Gloves_str_int`). Use these to answer "what mods can roll on X". Note some duplicates exist (`Bows` / `Bows2`, `Two Hand Maces` / `Two Hand Maces2`).
- **`poe2 change gear level/index.html`** — NOT a PoE2DB dump. A hand‑authored, self‑contained Thai **build/gearing sheet** ("Spirit Walker Twister — HC Gearing Sheet"): a styled single‑page planner. Edit this directly if asked to update the build plan.

## How to Grep the local PoE2DB HTML (offline fallback)

When you do fall back to the local dumps (see "web first" above), `Grep` for a targeted class or stat string rather than reading the whole file. Key CSS class hooks:

- `explicitMod` / `implicitMod` — affix lines on items and in modifier pools.
- `property` — base item stats (damage, armour, requirements).
- `item_currency`, `whiteitem`, `StackableCurrency` — currency/item entries.
- `PassiveSkills`, `passive-icon-frame` — passive/keystone data.
- `KeywordPopups` — inline tooltip definitions of game keywords.
- Rarity colour classes follow PoE conventions (`colourDefault`, unique/rare frames).

Because these files are large, prefer `Grep` with a targeted class or stat string over reading a whole file. There are no `data-name`/`ItemLevel` attributes — tier and item‑level context lives in surrounding table text, not structured attributes.
