# Model Context Protocol for Scientific Software

Slides for **PyHEP.dev 2026** (8 September 2026) by Giordon Stark (UC Santa Cruz / SCIPP):
*"Model Context Protocol for Scientific Software: Building AI Interfaces That Actually
Work."* Title and abstract: [`title-abstract.md`](title-abstract.md).

**▶ View the deck:** https://kratsg.github.io/2026-09-08-pyhepdev-mcp/

A 15-minute introduction to MCP for scientific software developers, argued from a working
deployment: six MCP-layer projects, four credential services, and one gateway/broker
platform, all in production on the UChicago ATLAS Analysis Facility. The limiting factor is
rarely the model; the hard part is giving it reliable, secure access to the tools, data,
and domain knowledge scientists already use. The talk covers practical patterns for
building MCP servers around existing Python libraries, handling authentication without
exposing user credentials, and designing interfaces that stay useful across LLM providers
and experiments.

The research behind every slide, with commit-level citations, is in
[`mcp-design-talk.md`](mcp-design-talk.md).

## What's here

| Path | What it is |
|------|------------|
| `presentation.html` | The deck (reveal.js 6.0.1, loaded from CDN — no build step) |
| `styles.css` | UChicago-maroon theme (CSS variables, `pt` font sizing) |
| `title-abstract.md` | The submitted talk title and abstract |
| `mcp-design-talk.md` | The full research dossier the slides are built from |
| `images/` | Committed figures (the title hero is reused AI-generated art) |
| `.agents/skills/` | The `revealjs` build skill used to author the deck |
| `.github/workflows/deploy.yml` | Auto-publishes to GitHub Pages on push to `main` |

## Structure

A main line of ~14 slides (about a minute each), with **backup / deep-dive** slides in
vertical stacks (down-arrow): what MCP is (+ a minimal server), the production stack, five
design principles, what breaks at platform scale (+ the negative-lesson catalogue), the
end-to-end tool-call trace, interoperability (+ a scorecard against the MCP spec), and a
closing "when is an MCP appropriate" (+ the checklist). Slides aim for 40–50 words of
visible text; the depth lives in speaker notes and the backups.

## View it

- **Online:** the GitHub Pages link above (always reflects `main`).
- **Locally:** open `presentation.html` in a browser. It pulls reveal.js from a CDN, so you
  need a network connection the first time.
- **Speaker notes:** press **`S`** in the browser (allow the popup) for speaker view —
  current + next slide, notes, and a timer. Every slide has notes.
- **Build stamp:** a small footer reads `dev` locally and `<short-sha> · <date>` on the
  deployed site, so you can tell which version is live.

## Edit it

Edit `presentation.html` directly (one slide / a few slides at a time), or use the
in-browser editor from the reveal.js skill:

```bash
node .agents/skills/revealjs/scripts/edit-html.js presentation.html
```

After editing, check for content overflow and review screenshots (Node deps live under
`.agents/skills/revealjs/node_modules`, gitignored — run `npm install` there if missing):

```bash
# flag any slide whose content exceeds 1280×720
node .agents/skills/revealjs/scripts/check-overflow.js presentation.html

# screenshot every slide (export mode disables animations)
.agents/skills/revealjs/node_modules/.bin/decktape reveal "presentation.html?export" \
  output.pdf --screenshots --screenshots-directory "screenshots/$(date +%Y%m%d_%H%M%S)"
```

> Heads-up: media-heavy slides (video / large images) can render *scaled-down* in the
> decktape export — that's a capture artifact. They render correctly in a real browser;
> trust `check-overflow.js` (it reports no overflow) over the decktape thumbnail.

## Deploy

Pushing to `main` triggers `.github/workflows/deploy.yml`, which publishes
`presentation.html` (as `index.html`) + `styles.css` + `images/` to GitHub Pages, injects
the build stamp, and attaches a best-effort `slides.pdf`. One-time setup: repo **Settings →
Pages → Source: GitHub Actions**.

## Credits

Slides AI-assisted by Claude (Anthropic). The title illustration is AI-generated (reused
from the Nikhef colloquium deck this repo was scaffolded from). All quoted code and commit
references are from the real repositories under github.com/maniaclab and github.com/kratsg.
