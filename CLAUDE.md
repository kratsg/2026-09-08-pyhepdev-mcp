# CLAUDE.md — working on this repo

This repo is a **reveal.js talk**: "Model Context Protocol for Scientific Software:
Building AI Interfaces That Actually Work" (PyHEP.dev 2026, 2026-09-07, Giordon Stark,
University of Chicago). 15 minutes. It is **public** and auto-deploys to GitHub Pages.
Conference page: https://indico.nikhef.nl/event/7873/contributions/31381/
Read `README.md` for the audience-facing overview and `title-abstract.md` for the
submitted title/abstract; this file is for whoever edits it next.

It was scaffolded from `kratsg/2026-07-01-nikhef-colloquium` (reusing that deck's
`styles.css`, `images/`, skills, and Pages workflow), then fully rebuilt around new
research: a staged multi-agent analysis of the whole AF MCP ecosystem, written up in
**`mcp-design-talk.md`** (committed — the citation-grade source for every claim on a
slide). The per-repo research reports live in `research/` (**local only, gitignored**).

## How to work on it

- Authored with the **project-local skill** in `.agents/skills/revealjs/` — build
  mechanics (scaffold, overflow check, screenshots, in-browser editor) and design
  conventions. Read `.agents/skills/revealjs/SKILL.md` before structural changes.
- It's already scaffolded. **Don't re-scaffold.** Edit `presentation.html` incrementally
  (one or a few slides at a time) — never rewrite the whole file at once.

## The edit → verify loop (do this every change)

1. Edit `presentation.html` / `styles.css`.
2. `node .agents/skills/revealjs/scripts/check-overflow.js presentation.html` — must report
   **no overflow** (slides are fixed 1280×720; content must fit).
3. Screenshot with decktape and **look at every changed slide** (see README for the command).
4. Watch for code blocks wrapping mid-line in the screenshots — the overflow checker does
   not flag ragged `<pre>` wraps; shorten the code lines instead of shrinking fonts.

## Content rules (match these)

- **15 minutes, ~14 main slides, about a minute each.** Aim for **40–50 words of visible
  text per slide** — no blobs of prose. Depth goes into speaker notes and the vertical
  backup slides, not onto the main line.
- **Voice:** Giordon's — plain, direct, first person, honest about failures. The framing
  language comes from `title-abstract.md` ("the limiting factor is rarely the model",
  "tools, data, and domain knowledge scientists already use", "interoperable tool
  interfaces are likely to matter more than model-specific integrations"). Don't
  reintroduce hype.
- **Evidence-backed only.** Every technical claim on a slide traces to
  `mcp-design-talk.md` (which carries file:line and commit citations). Don't invent
  numbers or soften the negative lessons — the "tried X, discovered Y, ended with Z"
  stories are the talk's strength. Code on slides is quoted from the real repos,
  abridged only (label "abridged"/"sketch" when reformatted).
- **Vertical stacks (down-arrow within a stack, right-arrow between):**
  `what-is-mcp(+build)`, `p-credentials(+custodians)`, `platform-scale(+lessons)`,
  `trace(+steps)`, `interop(+spec)`, `close(+checklist)`. Backups are marked in HTML
  banner comments (never put a literal `<!-- ... -->` inside another comment — it closes
  the outer one and leaks text onto every slide). Keep that structure if adding slides.
- **Em-dash policy (humanized):** prose uses commas/colons/periods. Em-dashes only in
  real quotations and "Name — role" lists.
- **Speaker notes:** every slide has an `<aside class="notes">`, conversational, first
  person, spoken cadence, straight apostrophes.

## Theme & mechanics

- **Theme:** UChicago Maroon (`#800000`) on warm off-white, teal (`#1F6F78`) / slate
  accents. All in `styles.css` CSS variables. **Font sizes in `pt`**, never px/em/rem.
- **reveal 6.0.1** from CDN. Plugins: **Notes only**. `slideNumber: 'c/t'`, fixed
  `width: 1280, height: 720`. No chart library; diagrams are `.node`/`.flow-arrow`
  HTML+CSS. Reusable components in `styles.css`: `.card`, `.callout`, `.tag`, `.stat`,
  `.node`, `.kicker`, `.shot`.
- **The trace slide's animated gateway diagram** (`.pdiag` CSS + the GSAP "gateway
  pulse" script at the bottom of `presentation.html`) is the mcp-portal landing
  animation, ported via R. Gardner's WLCG OTF12 deck (robrwg/2026-08-25-WLCG-OTF12; the
  original is af-mcp-platform `portal/src/lib/gatewayPulse.ts`). It plays only while
  the slide is on screen, is skipped under `?export` (decktape shows the static
  diagram, correctly) and under prefers-reduced-motion, and needs the GSAP CDN script
  tag that precedes reveal.js.
- **Build stamp:** fixed top-right element `id="buildstamp"` reads `dev` locally;
  `deploy.yml` rewrites it to `<short-sha> · <date>` at publish time. Keep the `id` and
  the `dev` default so the sed anchor keeps working.
- Node deps for the skill scripts live in `.agents/skills/revealjs/node_modules`
  (gitignored); run `npm install` there if the overflow checker can't find puppeteer,
  then `npx puppeteer browsers install chrome` once.

## Sources (what feeds the slides)

- `mcp-design-talk.md` — committed research dossier: per-project analysis, cross-project
  patterns, ~40 negative lessons with commits, spec comparison, principles, checklist.
- `research/` — **gitignored** intermediate reports (one per repo + synthesis + spec
  baseline + live-gateway snapshot). Regenerate rather than commit.
- The analyzed repos live locally under `~` (rucio-mcp, ami-mcp, af-jupyterlab-mcp,
  af-filesystem-mcp, atlas-search-mcp-bridge, af-credentials, {krb5,voms,condor}-token-service,
  af-mcp-platform, flux_apps) — link, don't commit. Known defects found during the
  research are filed as issues on the maniaclab repos, not tracked here.

## Commits & deploy

- Commit/push to **`main`** (no feature branches needed). Conventional Commits.
- End commit messages with `Assisted-by: Claude (Anthropic)` (Giordon's convention — not
  Co-Authored-By).
- Pushing `main` auto-deploys via `.github/workflows/deploy.yml`. Verify the run is green
  and the live site updated. Pages source: **Settings → Pages → GitHub Actions** (one-time).
