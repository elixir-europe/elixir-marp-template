# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A [Marp](https://github.com/marp-team/marp-cli) (Markdown Presentation Ecosystem) theme
implementing the official **ELIXIR slide template 2025** (.pptx). There is no application
code, build system, or test suite — this repo is a CSS theme (`elixir.css`), a starter deck
(`template.md`), and the brand assets they reference (`assets/`).

## Files

```
elixir.css       the theme: colors, fonts, layouts, slide classes
template.md      starter deck demonstrating every slide type/class
assets/theme/    logo, fonts, and decorative graphics referenced by elixir.css
assets/          demo/placeholder content referenced by template.md, plus other loose
                 images used ad hoc in decks built from this template (not part of the theme)
template.pdf     rendered export of template.md, kept as a visual reference
```

`elixir.css` and `assets/theme/` are coupled by relative path — the theme's `@font-face` and
`background-image: var(--elixir-...)` declarations reference files under `assets/theme/`
directly (e.g. `assets/theme/elixir-logo-standard.png`). If you rename or move a file in
`assets/theme/`, update the corresponding `url(...)` in `elixir.css`. Plain `assets/` has no
such coupling — `template.md` references files there (e.g. `assets/placeholder-image.svg`) by
plain relative path, and nothing in `elixir.css` depends on it.

## Commands

Render/export a deck with the Marp CLI (`npm install -g @marp-team/marp-cli`):

```bash
# HTML (fastest, no browser needed)
marp template.md --html --allow-local-files --no-stdin --theme-set elixir.css -o deck.html

# PDF (requires a local Chrome/Chromium/Edge/Firefox)
marp template.md --pdf --allow-local-files --no-stdin --theme-set elixir.css -o deck.pdf

# PowerPoint
marp template.md --pptx --allow-local-files --no-stdin --theme-set elixir.css -o deck.pptx
```

`--theme-set elixir.css` registers the theme so `theme: elixir` in a deck's YAML front matter
resolves. `--allow-local-files` is required because decks load images from `assets/`. There is
no lint/test command — validate changes by rendering `template.md` and visually inspecting the
output (or diffing against `template.pdf`).

## Theme architecture

`elixir.css` extends Marp's built-in `default` theme (`@import 'default';`) and layers ELIXIR
brand styling on top. Key structure to understand before editing:

- **CSS custom properties** at the top of `:root` centralize brand colors (`--elixir-navy`,
  `--elixir-orange`, etc.) and background-image URLs (`--elixir-logo`, `--elixir-title-art`,
  etc.). Prefer adding new brand colors/images here rather than hardcoding values inline.
- **Slide "classes"** select a layout via Marp's `<!-- _class: name -->` directive on a slide,
  matched in CSS as `section.name`. The four slide types, each with distinct background art and
  absolutely-positioned content regions, are:
  - `title` — opening slide (title text + speaker info, positioned via absolute offsets)
  - `divider` — full-navy section-break slide
  - `closing` — thank-you/contact slide
  - `no-logo` — a plain content slide with the footer logo and page number suppressed (for
    slides that need the full canvas for a large image)
  - Slides with none of these classes are regular content slides (heading + body, logo footer
    bottom-right, page number top-right).
- Because `title`/`divider`/`closing` position their content with `position: absolute`, editing
  them means keeping the paired positioning rules for both the container and its children (e.g.
  `section.title h1` and `section.title .speaker`) in sync — moving one without the other will
  misalign content.
- **Utility classes** usable directly in deck markdown: `.subtitle` (below an `h2`), `.highlight`/
  `<mark>` (orange emphasis), `.box`/`.box-blue`/`.box-orange` (callout boxes), `.columns`
  (multi-column flex layout; add `.img-col` to a column that should size to its content instead
  of stretching equally, e.g. one holding an image), `.small` (14px paragraph text), `.boxed`
  (bordered image).
- Brand sizing rules are encoded directly as fixed px values (not variables): H1 48px, H2 32px,
  H3 24px, body 22px, nested bullets 20px — matching the ELIXIR brand spec for Open Sans.

## Working on decks vs. the theme

- Editing `template.md`: keep the YAML front matter and `<!-- _class -->` comments intact when
  replacing placeholder content — they're what wires a slide to its layout.
- Editing `elixir.css`: changes affect every deck built against this theme. Check the slide
  types listed above (title/divider/closing/content/no-logo) in `template.md` after any change,
  since layout rules are position-absolute and easy to break for one slide type while fixing
  another.
- `assets/` (outside `assets/theme/`) holds miscellaneous graphics used in specific decks built
  from this template elsewhere — they are not referenced by `elixir.css` or `template.md` and
  can be added to/removed from freely without affecting the theme.
