# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Static marketing website for ITS Industrial Technology Supply Co., Ltd. (Samutsakhon, Thailand). Plain HTML/CSS/vanilla JS — no build step, package manager, linter, or tests. Each page is a single self-contained `.html` file with inline `<style>` and `<script>`.

## Running locally

Open the folder with the VS Code **Live Server** extension and serve `index.html` (or open the file directly in a browser). All asset paths are relative (`images/...`), so pages must be served from the repo root.

External dependencies load from CDNs: Google Fonts (Inter, Barlow Condensed) and Tabler Icons webfont (`ti ti-*` classes, https://tabler.io/icons).

## Architecture

- [index.html](index.html) — full-screen split landing page ("Select Portal"). Two `<a class="panel">` halves link to the two division sites. No JS.
- [ITS_Industrial_Automation_OIT.html](ITS_Industrial_Automation_OIT.html) — Industrial Automation division site (~2900 lines).
- [ITS_OilGas.html](ITS_OilGas.html) — Oil & Gas division site; a smaller copy of the same template (home / projects / contact).

The two division files share the same structure, design tokens, and JS. They are independent copies, so a shared change (nav, footer, tokens, contact details) must be made in both files.

### In-file SPA pattern (division pages)

- Each "page" is a `<main id="page-<name>" class="page">`; only the one with `.active` is shown.
- Navigation calls `showPage('<name>')` (without the `page-` prefix) from `onclick` handlers in the desktop nav, dropdowns, and the mobile drawer. There is no URL routing/hash state.
- **Adding a page** requires: a new `<main id="page-X" class="page">`, a footer placeholder `<div id="X-footer"></div>` inside it, adding `'X-footer'` to the `footerIds` array in the script, and nav links in both the desktop nav and the mobile nav.
- The shared footer is a template string `footerHTML` in the bottom `<script>`, injected into every `*-footer` div by the `footerIds` loop. Edit footer content there, not in the markup.
- Theming is driven by CSS custom properties in `:root` at the top of the `<style>` block (`--clr-*`, `--font-*`, `--space-*`). Brand accent is `--clr-orange: #FF8800`; prefer tokens over hard-coded colors.
- `.product-card[role="button"]` elements get Enter/Space keyboard handling from the script; keep `role="button"` + `tabindex` on clickable cards.

## Conventions

- Files are heavily commented with boxed section headers (`/* ==== ... ==== */`, `<!-- ==== ... ==== -->`); follow that style when adding sections.
- Images live in `images/`; partner/brand logos use `<div class="logo-box logo-box--img"><img ...></div>`.

## Working rules

- Keep each page as a single self-contained HTML file. Don't split CSS/JS into separate files unless asked.
- Any shared change (nav, footer, tokens, contact info) must be applied to BOTH division files in the same edit.
- Site copy is [English / Thai / both] — match existing tone.
- Deploys via `git push` to github.com/ponkarl/ITS-website-. Don't push without asking.

## Launch checklist

- Remove the `<meta name="robots" content="noindex, nofollow">` tag (marked PREVIEW ONLY) from all three HTML files.
- Set up redirects from the old site's pages (FlowComputer.html, Overview.html, ApplicationRef.html, PowerSupply.html, Raycap.html, Inhand.html, Oring.html, ControlMaestro.html, Contact.html, Flowmeter.html) to the matching new pages.
- Confirm all items in the "ITS Website — Open Questions for Management" doc are answered.
