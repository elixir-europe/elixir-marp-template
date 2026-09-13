# ELIXIR Marp template

A Marp (Markdown Presentation Ecosystem) theme built from the official
**ELIXIR slide template 2025** (.pptx). Colors, fonts, table styling, and logo
placement follow that source template.

## Files

```
elixir.css       the theme (colors, fonts, layouts)
template.md      starter deck showing every slide type/class
assets/theme/    logo, fonts, and decorative graphics referenced by elixir.css
assets/          demo/placeholder content referenced by template.md
```

## Requirements

- [Marp CLI](https://github.com/marp-team/marp-cli): `npm install -g @marp-team/marp-cli`
- A Chrome/Chromium/Edge/Firefox install, for PDF/PNG export only (HTML export
  needs no browser)

## Quick start

1. Copy this whole folder. Keep `assets/` alongside `template.md` and
   `elixir.css` (the theme references the logo files by relative path).
2. Edit `template.md` — replace the placeholder content slide-by-slide,
   keeping the front matter and `<!-- _class -->` comments.
3. Export:

```bash
# HTML (fastest, no browser needed)
marp template.md --html --allow-local-files --no-stdin --theme-set elixir.css -o deck.html

# PDF
marp template.md --pdf --allow-local-files --no-stdin --theme-set elixir.css -o deck.pdf

# PowerPoint
marp template.md --pptx --allow-local-files --no-stdin --theme-set elixir.css -o deck.pptx
```

`--theme-set elixir.css` registers the theme file so `theme: elixir` in the
markdown front matter resolves correctly. `--allow-local-files` is required
because the deck loads images from `assets/`.
