# Tamarind Learning — Page Templates

Static HTML/CSS/jQuery pages built on the **63bits Page Builder** template system. No build step — just plain files served as-is (e.g. via IIS).

## Building a new page

1. **Copy the template.** Duplicate `PageLayout.html` and rename it (e.g. `AboutUs.html`).
2. **Leave the shell alone.** Don't modify the `.app` wrapper (header/nav/footer), anything in `<head>`, or the `.t63-section` → `.js-section--container` scaffolding.
3. **Add your content inside `<div class="js-plain-html-wrap">`**, replacing the `<!-- Page Content Goes Here -->` placeholder. All page content goes inside this div and nowhere else.
4. **Make the page self-contained**, inside the wrapper:
   - Give it a unique root id/class prefix (e.g. `#tl-about`).
   - Add a scoped `<style>` block using your own class prefix and `--tl-*` variables.
   - Add any page-specific `<script>` blocks inline, scoped to your root element.
   - Don't add new files to `css/` or new `<script src>`/`<link>` tags to `<head>`.
5. **Control width/background from inside the wrapper** (inline styles or your scoped `<style>` block) — don't write JS that reaches out to style shell elements.
6. **Preview** by opening the file in a browser, or serving the folder statically.

See [`Pricing.html`](Pricing.html) for a worked example. Full conventions are in [`CLAUDE.md`](CLAUDE.md).

## Publishing

These files are for local development/preview only — the live site is served through the 63bits Page Builder, not by deploying this folder to IIS.

To publish a page:

1. Open the finished page in a browser and copy the HTML generated **inside `<div class="js-plain-html-wrap">`** (i.e. just your content — not the `.app` shell, `<head>`, or `.t63-section` scaffolding around it).
2. In the Page Builder, open the target page and paste that HTML into its **Markup** section.
3. Save/publish from the Page Builder as usual.

Only the wrapper's contents ever get pasted in — the shell markup in `PageLayout.html` exists so you can preview the page in context locally, but it's the Page Builder's own template that renders the header/nav/footer in production.
