# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Alfred Cueva's personal academic website, a static site served by GitHub Pages from
`alfred-cueva/alfred-cueva.github.io`. Pushing to `master` deploys. Forked from Jon Barron's
template (https://jonbarron.info/), which is all `README.md` says.

There is no build step, package manager, linter, or test suite. Preview with
`python3 -m http.server` from the repo root, then open http://localhost:8000.

`googlea3babd3d7df5ba87.html` is a Google Search Console verification file. Do not delete or
rename it.

## Architecture

The whole site is `index.html` (~810 lines) plus `stylesheet.css`.

### Layout

A single column (max 1000px) of stacked `<table>`s, one or two per section. Visible sections,
in order: header (bio, contact links, rotating headshot), `Timeline`, `News`, `Research`,
`Work Experience`, `Projects`, `Services`. `Current Position`, `Technical Skills`, the SNU rows
of Work Experience, and one project row are commented out.

Use tables for new sections to match the template. The one exception is the Timeline: a
horizontal flexbox strip (`.timeline-*` classes) inside a table cell that becomes a vertical
grid below 700px. The first `.timeline-item` gets a "current" badge from CSS `:first-child`,
so list order decides what is shown as current.

### Entry pattern (Research, Work Experience, Projects)

```html
<tr>
  <td class="paper-img-td">                         <!-- left 25%: thumbnail -->
    <img src="images/foo.gif" alt="..." class="paper-img">
  </td>
  <td style="padding:20px;width:75%;vertical-align:middle">
    <span class="papertitle">Title</span>
    <br>
    <em>Author, <strong>Alfred Cueva</strong>, Author</em>   <!-- * equal contrib, &dagger; -->
    <br>
    <a href="..." target="_blank" rel="noopener noreferrer">[arXiv]</a> |
    <a href="..." target="_blank" rel="noopener noreferrer">[Website]</a>
    <br>
    <strong>CoRL 2026<br>RSS 2026 - X Workshop <span class="wine-accent">(Oral)</span></strong>
    <p style="text-align:justify">Description</p>
    <p style="color:#666;font-size:12px;margin-top:8px;"><em>Tool · Tool · Tool</em></p>
  </td>
</tr>
```

Work Experience rows use `<img class="company-logo">` on the left and company link, role in
`.papertitle`, then dates in `<em>` on the right. Every Research, Work Experience, and Projects
entry ends with the gray `·`-separated tech-tags line.

Thumbnails are `.png`/`.gif` or `<video autoplay loop muted playsinline>`. Recent Research
thumbnails use `aspect-ratio:664/560;object-fit:cover;`, others `aspect-ratio:1/1`.

### One fact, several places

Content is duplicated across sections, so a single update usually touches several:

- Paper accepted: a `News` line, the venue `<strong>` block of its `Research` entry, and
  possibly the project links under its `Timeline` item.
- New role: the bio paragraphs, a `Timeline` item, a `News` line, and a `Work Experience` row.

### JavaScript

One inline `<script>` at the bottom of `index.html`:

- `showFunFact()`: the header button puts a random `funFacts` entry into `#fun-fact`.
- `zoomLens()`: hover magnifier for `.zoom-wrap` thumbnails. Each needs the same video twice:
  a `.zoom-base` video and a `.zoom-mag` copy inside `.zoom-lens`. Runs only on fine pointers.
- `rotateProfilePic()`: cycles `#profile-pic` through `profileImages` every 10s with a fade.
  Alternate headshots live in `images/alternative_headpics/`.

### Styling

`stylesheet.css` holds the template's base styles plus site additions: `span.wine-accent`
(crimson emphasis for awards and the "looking for opportunities" line), the `.timeline-*`
block, `.paper-img*`, `.company-logo`, `#profile-circle`, and `.zoom-*`. The green in the
Research bullets (inline `#1a9e5c`) matches the timeline's `--tl-accent`. Inline `style="..."`
is everywhere; match the surrounding inline style rather than refactoring it out.

### Phone layout (max-width 700px)

A media query at the end of `stylesheet.css` stacks the header (`.intro-row`, headshot above
the bio) and every entry row (thumbnail above text). It also turns justified text left-aligned.
It relies on markup details, so new content must follow them:

- The left cell of an entry must have `class="paper-img-td"`. The text cell is styled through
  `.paper-img-td + td`.
- Write justified text as `style="text-align:justify"` with no space. It is matched by the
  attribute selector `[style*="text-align:justify"]`.
- Inline styles on these cells are overridden with `!important`, so their inline widths and
  padding only apply on desktop.

### Assets

Media lives in `images/`, PDFs (resume, thesis, report) in `data/`, both referenced by relative
path. GitHub Pages paths are case-sensitive and many files end in `.PNG`, so match the exact
filename case.

## Unrelated subdirectories

`mipnerf/`, `mipnerf360/`, and `zipnerf/` are self-contained NeRF project pages with their own
Bootstrap assets. Nothing on the site links to them. Leave them alone unless a task targets them.

## Editing conventions

- External links use `target="_blank" rel="noopener noreferrer"`. Keep this on new links.
- Hide content by commenting it out (`<!-- ... -->`) rather than deleting it, so it is easy to
  restore.
