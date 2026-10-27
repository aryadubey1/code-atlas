# 🧭 Code Atlas

A living, hand-drawn-feeling map of a codebase you're learning: the areas you've worked in, the tickets that took you there, the bugs you found and the concepts you had to look up. It's one static page plus one JSON file, and it's kept up to date by [Claude Code](https://claude.com/claude-code) with the included skill.

It started as a private tracker for a new job and is shared here with fictional demo data (**Northwind Books**, an online bookstore).

> 📸 Screenshots coming soon. Open the live demo or run it locally (below) to see it.

## What's in it

| Page | What it shows |
|---|---|
| **Map** | Every area of the codebase as a floating island, coloured by *worked in / looking into / not yet*. Pick a ticket to light up the route it takes across the map. |
| **Tickets** | One card per ticket with its progress boat, definition of done, before → after walkthrough, edge-case table and your notes (Markdown, with tabs). |
| **Bugs** | Bugs you found, plus a lanes diagram of which ones have reached review and release. |
| **Concepts** | Everything you asked "what is X?" about, each with one line and a tiny ASCII picture. Searchable. |
| **Log / Day by day** | What changed each day, what you waited on and who gave the direction. Handy for retros. |
| **Meeting** *(optional)* | A decide / propose / fallback sheet per ticket, with screenshots and a Present mode (press **P**). |

## Quick start

```bash
git clone https://github.com/aryadubey1/code-atlas.git
cd code-atlas
python -m http.server 8000
# open http://localhost:8000
```

It needs a web server (any static one) because the page loads `data/site.json` with `fetch`. Opening `index.html` straight from disk won't load the data.

## Make it yours

1. Replace `data/site.json` with your own project. Start by changing `project`, `departments` and `tickets`.
2. Put your ticket notes in `notes/` and screenshots in `assets/`.
3. Delete the demo notes and images you don't need.

The format is documented in [docs/data-format.md](docs/data-format.md), and [data/site.schema.json](data/site.schema.json) is the full JSON Schema. Your editor will validate against it because `site.json` points at it.

## Keep it current with Claude Code

The repo ships a skill at `.claude/skills/atlas-update/SKILL.md`. Open Claude Code in this folder (or copy the skill into your own `~/.claude/skills/`) and, after a work session, say:

```
update the atlas
```

Claude reads your git log, branch and notes. It then updates the ticket notes, the daily log and `site.json`: stages, areas touched, bugs, concepts and timeline. Finally it checks the result against the schema.

## Deploy to GitHub Pages

1. Push the repo to GitHub.
2. **Settings → Pages → Build and deployment**: *Deploy from a branch*, pick your branch and `/ (root)`.
3. Your atlas is live at `https://<user>.github.io/code-atlas/`.

Meeting decisions are saved in each viewer's browser on a static host. Inside a claude.ai artifact they go to the artifact's shared database instead.

> ⚠️ A public atlas is public. Keep company code, names and customer data out of anything you publish.

## Tech

Plain HTML, CSS and JavaScript in a single file, with no build step. [marked](https://marked.js.org/) and [DOMPurify](https://github.com/cure53/DOMPurify) render the notes. The watercolour sea is two canvases; islands, emblems and illustrations are SVG generated from each id, so the same data always draws the same map. It respects `prefers-reduced-motion`.

## License

[MIT](LICENSE) © Arya Dubey
