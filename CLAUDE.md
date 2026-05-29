# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Alfred Cueva's personal academic website, served as a static site from GitHub Pages
(`alfred-cueva.github.io`). Forked from Jon Barron's academic website template
(https://jonbarron.info/). There is **no build step, no package manager, and no test
suite** — the site is hand-edited HTML/CSS and viewed directly in a browser
(open `index.html`, or `python3 -m http.server` from the repo root to preview locally).

Deployment is `git push` to the GitHub Pages repo; the live commit history is the
deploy log. `googlea3babd3d7df5ba87.html` is a Google Search Console verification
file — do not delete or rename it.

## Architecture

Everything user-facing lives in **`index.html`** (~580 lines) plus **`stylesheet.css`**.
The page is a single column built from stacked `<table>` elements (template convention —
keep using tables for new sections rather than introducing flexbox/grid, to stay
consistent). Each content section is its own `<table>` introduced by an `<h2>`:
`Research`, `Current Position`, `Experience`, `Projects`, `Services`. (`Technical Skills`
exists but is commented out.)

Entries within a section follow one repeating two-column row pattern:

```html
<tr>
  <td class="paper-img-td">           <!-- left 25%: thumbnail -->
    <img src="images/foo.gif" class="paper-img">   <!-- or <video class="paper-img"> -->
  </td>
  <td ...>                            <!-- right: text -->
    <span class="papertitle">Title</span>
    <p style="text-align:justify">Description…</p>
  </td>
</tr>
```

Key CSS classes (all defined in `stylesheet.css`): `.papertitle`, `.paper-img`,
`.paper-img-td`, `.company-logo` (Experience-section logos), `.name` (header name),
`span.highlight` (yellow highlight). Layout/spacing is a mix of these classes and
heavy inline `style="..."` attributes carried over from the template — match the
surrounding inline style when editing rather than refactoring it out.

Media (`images/`, `data/`) is referenced by relative path. Thumbnails are `.png`,
`.gif`, or autoplaying muted `.mp4`/`<video>`; resumes/theses/reports are PDFs in
`data/`. Square thumbnails use inline `style="aspect-ratio:1/1;object-fit:cover;"`.

The only JavaScript is an inline `<script>` at the bottom of `index.html`: a
`showFunFact()` function cycling a `funFacts` array into `#fun-fact`.

## Subproject directories (do not treat as part of the personal site)

`mipnerf/`, `mipnerf360/`, and `zipnerf/` are **self-contained, unrelated NeRF
project demo pages** (their own `index.html` + `css/`/`js/`/`img/`). They are not
linked from the main site and use Bootstrap, not `stylesheet.css`. Leave them alone
unless a task explicitly targets them.

## Editing conventions

- Commit messages are short and imperative, scoped to the file (e.g. "Revise personal
  bio in index.html"). Match that style.
- All external links use `target="_blank" rel="noopener noreferrer"`. Preserve this on
  any new outbound link.
- Content is frequently toggled via HTML comments (`<!-- ... -->`) rather than deleted —
  e.g. the Technical Skills table and individual project rows. Prefer commenting out
  over removing when hiding content, to keep it easy to restore.
