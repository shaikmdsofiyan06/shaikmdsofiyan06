# Setup Guide

## 1. Create the special profile repo
On GitHub, create a new repository named **exactly** `shaikmdsofiyan06` (same as your username).
GitHub auto-detects this and shows its `README.md` on your profile page.

## 2. Upload the files
Push everything in this folder to that repo, keeping the structure:

```
shaikmdsofiyan06/
├── README.md
├── assets/
│   ├── banner.svg
│   ├── banner-light.svg
│   ├── lanyard.svg
│   ├── stats.svg
│   ├── langs.svg
│   └── trophies.svg
└── .github/workflows/github-snake.yml
```

(`build/` is optional — it's just the generator scripts in case you want to regenerate
the SVGs later with new details or real stats.)

## 3. Enable the snake Action
- Go to **Settings → Actions → General** in the repo and make sure Actions are enabled,
  and under "Workflow permissions" select **Read and write permissions**.
- Go to the **Actions** tab and manually run "Generate Snake Animation" once
  (`workflow_dispatch`), or just push — it also runs on push to `main`.
- It commits `github-snake.svg` and `github-snake-dark.svg` to a new `output` branch.
  The README already points at those paths, so the snake section will populate
  automatically after the first run.

## 4. Dark/light banner auto-switch
`README.md` already uses a `<picture>` tag with `prefers-color-scheme` so
`banner.svg` (dark) shows for dark-mode viewers and `banner-light.svg` for
light-mode viewers automatically — no action needed.

## 5. Updating your real stats
`assets/stats.svg`, `assets/langs.svg`, and `assets/trophies.svg` currently hold
placeholder numbers — GitHub's public API was rate-limited when these were built,
so live numbers couldn't be pulled. Edit the values at the top of `build/gen_cards.py`
and re-run `python3 gen_cards.py` to regenerate them with your real stats, then
commit the updated SVGs.

## 6. Cache-busting
Every local asset in `README.md` is referenced with a `?v=1` query string
(e.g. `assets/banner.svg?v=1`). Bump that number any time you replace an asset
so GitHub's cached copy doesn't hide your update.

## 7. Notes
- Everything animates with pure **SMIL/CSS** — no JavaScript — so nothing gets
  stripped when GitHub renders it.
- Project rows in the README currently link to your profile generically since
  no repo URLs were given — swap in the real repo links whenever you have them.
- Hover effects on the tech pills are included in the SVG's CSS, but GitHub
  renders these images via `<img>`, which doesn't forward `:hover` — so they
  won't trigger on GitHub itself (this is a platform limitation, not a bug).
  They *will* work if you open the SVG directly in a browser tab.
