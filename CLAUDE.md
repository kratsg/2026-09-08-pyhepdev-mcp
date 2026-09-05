# CLAUDE.md — working on this repo

This repo is a **reveal.js talk**: "The Facility Is the Context — agentic computing
infrastructure for analysis at scale" (Nikhef interview colloquium, Amsterdam, 2026-07-01,
Giordon Stark, UC Santa Cruz / SCIPP). It is **public** and auto-deploys to GitHub Pages.
Read `README.md` for the audience-facing overview; this file is for whoever edits it next.

It was rebuilt from `kratsg/throughput-computing-2026-slides` (reusing that deck's `styles.css`,
`images/`, and Pages workflow), then restructured to be **vision-first** and **less
ATLAS-centric** (ATLAS = current employer; CMS/DUNE/Belle II = informal collaboration on these
agentic ideas).

## How to work on it

- Authored with the **project-local skills** in `.agents/skills/`:
  - `revealjs/` — build mechanics (scaffold, overflow check, screenshots, in-browser editor)
    and design conventions. Read `.agents/skills/revealjs/SKILL.md` before structural changes.
  - `grill-me/` — the interview-the-user skill used to scope the talk.
- It's already scaffolded. **Don't re-scaffold.** Edit `presentation.html` incrementally
  (one or a few slides at a time) — never rewrite the whole file at once.

## The edit → verify loop (do this every change)

1. Edit `presentation.html` / `styles.css`.
2. `node .agents/skills/revealjs/scripts/check-overflow.js presentation.html` — must report
   **no overflow** (slides are fixed 1280×720; content must fit).
3. Screenshot with decktape and **look at every changed slide** (see README for the command).
4. **Known gotcha:** slides with video or large images render *scaled-down in decktape's
   export* — a capture artifact, not a real problem. Confirm via `check-overflow.js`; the
   browser render is fine.

## Conventions (match these)

- **Theme:** UChicago Maroon (`#800000`) on warm off-white, teal (`#1F6F78`) / slate accents.
  All in `styles.css` CSS variables. **Font sizes in `pt`**, never px/em/rem (fixed-size slides).
- **reveal 6.0.1** from CDN. Plugins: **Notes only** (`dist/plugin/notes.js`). `slideNumber: 'c/t'`,
  fixed `width: 1280, height: 720`. No Chart.js — charts/bars are plain HTML+CSS (the cost
  comparison on `agenda-equity-cost` is div bars, not a chart library).
- **Build stamp:** a fixed bottom-left element `id="buildstamp"` reads `dev` locally; `deploy.yml`
  rewrites it to `<short-sha> · <date>` at publish time. Keep the `id` and the `dev` default so
  the sed anchor in the workflow keeps working.
- **Vision-first, layered depth.** A broad main line + optional `<!-- DEEP-DIVE -->` / `<!-- BACKUP -->`
  slides. Those markers live **in HTML comments** — never put a literal `<!-- ... -->` inside
  another comment (it closes the outer one and leaks text onto every slide).
- **Vertical stacks (down-arrow within a section, right-arrow between):** vocab(+watts),
  the architecture run, and most agenda items group a main slide with its deep-dive/backup —
  `gateway(+topology)`, `identity(+deepdive)`, `ssh(+deepdive)`, `sandbox(+policy, +trust-ladder)`,
  `pixi(+cluster-as-code)`, `equity(+cost)`, `openweb(+skills-commons)`, `iris-osg(+map)`.
  Keep that structure if adding slides.
- **Em-dash policy (humanized):** prose uses commas/colons/periods. Keep em-dashes only for
  real quotations, the "— Giordon" attribution, and "Name — role" lists. Don't reintroduce
  prose em-dashes.
- **Honesty tone:** the deck is deliberate about "deployed vs aspirational" — production for
  ATLAS, "informal / in conversation" for CMS/DUNE/Belle II, "partner" for ESCAPE. Don't
  inflate these. Citations must be verified before going on-slide.
- **Boxes:** cards for parallel items, `.callout` for "the point" / open questions. Box-rich
  but not over-boxed.
- **Speaker notes:** every slide has an `<aside class="notes">`. Keep them conversational
  (first person, spoken cadence), straight apostrophes/commas, not em-dashes.

## Images & diagrams

- Live in `images/` and are committed (incl. the web-friendly `jupyter-mcp-demo.mp4`).
- Two hand-authored SVG diagrams: `NEW-af-mcp-arch.svg` (gateway/broker) and `NEW-pixi-pack.svg`
  (login → pack → worker). **SVG gotcha:** a standalone `.svg` loaded via `<img>` is parsed as
  strict XML — use literal UTF-8 glyphs (·, —, →) or numeric entities, **never** HTML named
  entities like `&middot;`/`&mdash;` (they're undefined in XML and break the render).
- AI-generated images must be credited on-slide; the title uses the reused `title-hero.png`.

## Commits & deploy

- Commit/push to **`main`** (no feature branches needed). Conventional Commits.
- End commit messages with `Assisted-by: Claude (Anthropic)` (Giordon's convention — not
  Co-Authored-By).
- Pushing `main` auto-deploys via `.github/workflows/deploy.yml`. Verify the run is green and
  the live site updated. Pages source: **Settings → Pages → GitHub Actions** (one-time).

## Source material (LOCAL ONLY — gitignored on purpose)

These inputs live in the working tree but are **not committed**, and must stay that way —
this repo is public and they carry internal detail (collaborators, Slack/email threads, ops
internals) beyond what's on the slides:

- `nikhef_colloquium_research.md` — the vetted research dossier the deck's notes point to.
- `nikhef_colloquium_title_abstract.md` — title/abstract drafts.
- `af-agentic-ai-research.md` — earlier Slack/email research.
- `talk-notes.md`, `TODO.md` — Giordon's personal notes.
- PDFs (`*.pdf`) and checked-out repos (`marketplace/`, `rucio-mcp/`, `ami-mcp/`,
  `jupyter-mcp-server/`, `af-docs/`, `flux_apps/`) — link, don't commit.

## Getting Slack/email-sourced facts

For anything that lives only in Giordon's Slack or email (real outputs, adoption numbers,
quotes), don't fabricate — hand Giordon a copy-paste prompt to run in **Claude App** (which has
those connectors) and fold the result back in. The dossier was produced that way.
