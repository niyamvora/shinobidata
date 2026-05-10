# Brand assets

| File | What | Use |
|---|---|---|
| `logo-light.svg` / `logo-light.png` | Logo for light backgrounds (dark glyph) | README header in light mode, light website surfaces |
| `logo-dark.svg` / `logo-dark.png` | Logo for dark backgrounds (light glyph) | README header in dark mode (via `<picture media="(prefers-color-scheme: dark)">`) |
| `logo-512.png` | 512×512 square logo | Connector listing icon (Anthropic + OpenAI directories), favicon source, social previews |

## TODO

- [ ] **Social preview image** (1280×640 PNG). What shows when the GitHub link is shared on Twitter / Slack / Discord. Set via repo Settings → Social preview. Most repos skip this; doing it is one of the highest-leverage 30-minute investments for marketing.
- [ ] **Hero GIF** (10–15 seconds). A user in Claude asking "how concentrated am I in tech?" and getting the narrated answer. Add to README top.
- [ ] **OG-banner.png** for the README hero — wide, brand-aligned, with the tagline visible.

## Usage rules (for community contributors)

- Do **not** modify the logo glyphs. Color tweaks for backgrounds are fine; glyph edits are not.
- Do **not** combine the logo with another brand's logo without checking with us — keep the consent screen pattern (Claude × ShinobiData) as the canonical co-branding example.
- For social cards / blog posts using the logo, link back to <https://shinobidata.com> as a courtesy.

## License

Logos and other brand assets are © 2026 ShinobiData. Permission is granted to use them for the purpose of describing, reviewing, or integrating with the ShinobiData MCP. All other uses (merchandise, derivative branding, training data) require written permission — email [support@shinobidata.com](mailto:support@shinobidata.com).

The MIT license that covers the rest of this repository (docs, examples, configuration) **does not** extend to the brand assets in this directory.
