# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

This repo's purpose is **`PageLayout.html`**: a reference layout page for Tamarind Learning showing how page content gets wrapped by the site chrome (header/nav/footer) built on the **63bits Page Builder** template system. It is the canonical template — **every custom page built for this site must be built based on `PageLayout.html`**, reusing its `.app` shell and asset includes as-is and replacing only the content inside the wrapper described below.

There is no build step, package manager, server, or test suite — it's plain HTML/CSS/jQuery served directly (e.g. via IIS, given the `TamarindLearningHtml` folder name and Windows environment). Open `PageLayout.html` (or any page built from it) directly in a browser, or serve the folder statically, to preview.

## Structure

- `PageLayout.html` — the template/reference page. It's a 63bits Page Builder export: a generic `.app` shell (header/nav/footer) wrapping one `.t63-section` → `.js-section--container` → **`<div class="js-plain-html-wrap">`**, the content wrapper. The wrapper ships empty (just a `<!-- Page Content Goes Here -->` placeholder comment) — copy it and drop the new page's content in directly.
- `css/style.css` — main site stylesheet (~8800 lines) for the `.app`/`.t63-*` shell (header, nav, footer, buttons, layout primitives).
- `css/themes/theme.css` — CSS custom properties (`--color-*`) consumed by `style.css`; this is where the site's color palette is defined.
- `css/BootstrapUtilities.css` — small set of extra Bootstrap-style utility classes not covered by the vendored Bootstrap build.
- `css/fonts.css` — `@font-face` declarations for the fonts in `fonts/`.
- `plugins/` — vendored third-party libraries, loaded via plain `<script>`/`<link>` tags (no bundler): jQuery, Bootstrap, Font Awesome, Slick, Splide, malihu custom scrollbar, jquery-appear, and `63bits-pageBuilder` (the template engine's own JS/CSS, `template63.js` / `template63.css`).
- `images/`, `fonts/` — static assets.

## Editing conventions

- **Building a new page**: Start from a copy of `PageLayout.html`. Keep the `.app` shell (header/nav/footer), all `<head>` asset includes, and the `.t63-section` / `.js-section--container` scaffolding untouched. **All page content must be placed inside `<div class="js-plain-html-wrap">`** — replace the `<!-- Page Content Goes Here -->` placeholder with the new page's content; don't add content outside this wrapper.
- **Content edits**: Content inside `.js-plain-html-wrap` is expected to be self-contained (its own class namespace and scoped `<style>` block, ids/classes prefixed per-page, e.g. a `#tl-<page>` root with `--tl-*` CSS variables and `tl-<page>-*` classes) so it doesn't collide with the shell's `t63-*`/`js-*` styles. Pick a page-specific prefix and scope custom CSS under a single root id/class.
- **Shell/template classes**: Classes prefixed `t63-` come from the 63bits Page Builder template (`template63.css`/`.js`); classes prefixed `js-` are JS behavior hooks (event binding, not styling) wired up in `plugins/63bits-pageBuilder/js/template63.js` and inline `<script>` blocks — e.g. `js-page-section`, `js-app-nav`, `js-app-hamburger`, `js-plain-html-wrap`. Don't repurpose `js-` classes for styling.
- **Colors/theme**: Global theme colors are CSS variables in `css/themes/theme.css` (`--color-primary`, `--color-*` badge/form/text variants). Change palette here rather than hardcoding hex values in `style.css`, except within a page's own self-contained block inside `.js-plain-html-wrap`, which may define its own `--tl-*`-style variable set.
- **Container width & section background**: The outer `<section class="t63-section js-page-section">` and its ancestors sit **above** `.js-plain-html-wrap` and are not to be targeted with JavaScript from inside the wrapper — there is no `$('.js-page-section')` script pattern anymore. Content width and background are handled entirely with markup/CSS placed **inside** `.js-plain-html-wrap` itself (e.g. inline styles or a scoped `<style>` block setting `width`/`max-width`/`background-color` on the page's own root element). Don't add scripts that reach outside the wrapper to restyle shell elements.
- **Page-specific CSS & JS**: Don't add new files to `css/` or new `<script src>` includes in `<head>`/before `</body>` for page-specific styling/behavior. All CSS and JS unique to a page belongs **inline, inside `.js-plain-html-wrap`** — a scoped `<style>` block and any needed `<script>` blocks live alongside the page's content markup, scoped to the page's own root id/class. This keeps each page fully self-contained and independent from other pages built off the same `PageLayout.html`.
- **Known broken references**: `PageLayout.html` links `plugins/jquery-confirm/jquery-confirm.min.css`, `plugins/fancybox/jquery.fancybox.css`, and `plugins/fancybox/style.css`, and loads `plugins/jquery-confirm/jquery-confirm.min.js` — but the `jquery-confirm` and `fancybox` plugin folders do not exist in this repo. These are dangling references from the template; either add the missing vendor files or remove the tags if working in that area.
