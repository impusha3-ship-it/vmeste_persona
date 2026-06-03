# vmeste_persona — Persona 5 design

Persona-5-styled fork of the Дима/Оля couple site. Same functionality as the
original (`impusha3-ship-it/vmeste`), redesigned look.

- **Live:** https://impusha3-ship-it.github.io/vmeste_persona/
- **Working file:** `C:\Users\impus\Downloads\index-v2.html` → copied to repo `index.html`, push `main`.
- **Database:** Firebase project `vmeste-a8b90`, **isolated node `/app2`** (original site uses `/app`). Ref in code: `ref(db, "app2")`. Realtime-DB rules grant read/write on both `app` and `app2`.
- **Checkpoint:** `Downloads/index-persona-v1.html` — frozen good state (2026-06-03).

## Design system

- **Palette:** red `#e60012` / black `#0a0a0a` / paper-white `#f7f3ea`. Secondary HUD accents: cyan `#27d3ff`, magenta `#ff3ea5`, velvet-blue `#1fc3ff`, lash-yellow `#ffd400`.
- **Per-person:** Дима = red, Оля = white/cyan.
- **Fonts (Google, Cyrillic):** Oswald (italic display/headings), Russo One (numbers), Montserrat (body).

## Architecture — override layers

The original cyber/neon theme is left intact; Persona is layered on top as
appended `<style>` blocks (later source order + `!important` win). Order:

1. `#p5-skin` — palette var remap, black panels w/ white border, slanted nav, red buttons, headings, decor recolor (star/halftone/scanlines), P5 page-transition slash, login calling-card base.
2. `#p5-ref` — torn-paper panels via `filter: drop-shadow` (NOT box-shadow — `clip-path` clips box-shadow), cyan/magenta numbers, question answers → speech bubbles.
3. `#p5-tier-chat` — tier lists, pet (Амням) palette, **question-of-day = tilted red "phone-screen" chat panels** (all cards in `#q-wrap`, alternating tilt).
4. `#p5-big` — ransom-note headings, app-icon achievement tiles, slanted nav menu blocks, calling-card login, star-halftone bg, **chat avatars w/ yellow corner**, Phantom mask + vertical "TAKE YOUR HEART" + barcode on login.

## Key gotchas

- `clip-path` clips both `box-shadow` AND `position:fixed` descendants. Hard offset shadows on clipped panels use `filter: drop-shadow(...)`. The Дима/Оля overlay is moved to `<body>` in JS so the login card's clip-path doesn't crop it (and `enterAs` hides it explicitly since it's no longer a `#login` child).
- When editing near a block's closing `</style>`, keep the `</style>` — dropping one swallows the following `<script>`.
- Verify visual changes (esp. SVG like the login mask) via the local preview server (`.claude/launch.json` → `downloads-static`) and screenshots before deploy.

## JS helpers (trailing `<script>`s)

`ransomize()` (per-letter cut-out headings + login monogram), star-halftone bg injection, move `#login-step-who` to body, inject Phantom mask SVG + vertical text + barcode footer into `#login`.

## Deploy

No `gh` CLI. Push via stored Windows credential. GitHub Pages enabled via API
(`git credential fill` → `Authorization: token` on api.github.com). Local clone:
`C:\Users\impus\vmeste-deploy\vmeste_persona`.
