# Repository Guidelines

## Project Structure

This repository builds a GitHub profile README from Markdown and SVG assets. `README.md` composes the graphics at the repository root (`ascii.svg`, `stats.svg`, `streak.svg`, `langs.svg`, `year.svg`, and `hd-*.svg`). Python utilities live in `scripts/`: `generate_stats.py` updates contribution and language graphics, `make_portrait.py` creates the ASCII portrait, and `embed_portrait_font.py` embeds its font. Fonts and their license are in `scripts/fonts/`; source images and other design assets are under `assets/` and `.github/assets/`. The scheduled stats workflow is `.github/workflows/stats.yml`.

## Build and Development

There is no package build step. Use Python 3 and the standard library to regenerate stats:

```sh
GITHUB_TOKEN=<token> python3 scripts/generate_stats.py
```

Set `GH_LOGIN` to choose the GitHub account (default: `tanishworks`) or `OUT_DIR` to change the output directory. The portrait tool needs Pillow, NumPy, OpenCV, `rembg`, and `onnxruntime`; see its help with `python3 scripts/make_portrait.py --help`. After generating `ascii.svg`, run `python3 scripts/embed_portrait_font.py` to embed the subset font.

## Style and Asset Conventions

Keep scripts dependency-light where possible and follow the existing Python style: four-space indentation, descriptive `snake_case` names, and standard-library imports grouped before third-party imports. Preserve the shared monochrome palette, typography, and dimensions when editing SVG graphics. Keep generated graphics checked into the repository so the README works without runtime generation.

## Validation

No standalone test suite is configured. For script changes, run the relevant generator with suitable inputs and inspect the resulting SVG. For README or graphic changes, preview the README rendering and check both light and dark themes. Stats generation makes a GitHub GraphQL API request and requires a token; avoid committing generated changes unless they are intended.

## Commits and Pull Requests

Recent history uses short, imperative summaries; automated stats updates use `stats: refresh profile graphics`. Follow that pattern, for example `portrait: improve crop handling`. Pull requests should explain the visible change, list affected assets or scripts, and include a rendered screenshot for layout or appearance changes. Link a related issue when one exists.

## Secrets and Configuration

Never commit `GITHUB_TOKEN` or other credentials. Configure it in the shell for local generation; GitHub Actions supplies its token through repository secrets and workflow permissions.
