# Data format

Everything the page shows comes from `data/site.json` plus the Markdown files it points to. The full contract is `data/site.schema.json`; this page explains it in plain words.

```
data/site.json
├── project        name, title, repo, team, trackerName      → header, hero, ticket plate
├── stages         ["Analyse", "Mockup", …, "Released"]      → the progress boat on every ticket
├── departments[]  the islands on the map                    → Map, area pages
├── tickets[]      what you worked on                        → ticket cards, ticket pages, map routes
├── bugs[]         bugs you found                            → Bugs page + review/release lanes
├── concepts[]     things you asked about                    → Concepts page (searchable)
├── timeline[]     newest day first                          → Log + "Latest" on the map
├── repos[]        other repos, each with a notes file       → one nav link per repo
├── upcoming[]     optional "Up next" list                   → Map
├── meeting        optional meeting page                     → "Meeting" nav button, Present mode
└── dailyLog       Markdown file                             → Day by day
```

## Status values

Departments and modules use the same three states:

| Value | Shown as | Meaning |
|---|---|---|
| `"worked"` | Worked in | You wrote code here |
| `"investigating"` | Looking into | Mockup, reading, planning |
| `""` | Not yet | Nothing yet |

## project

| Field | Used for |
|---|---|
| `name` | Hero: "Where I've worked in **name**" |
| `title` | Browser tab and brand (default `"<name> Atlas"`) |
| `repo`, `team` | Hero eyebrow and the summary card |
| `eyebrow`, `lead` | Override the hero eyebrow and intro text |
| `trackerName` | Label for each ticket's `tracker` field (Jira, Linear, GitHub…) |
| `ticketPrefix` | For your own reference and the update skill |

## departments[]

```json
{
  "id": "catalog", "name": "Catalog", "icon": "book", "pigment": "honey", "place": [3, 1],
  "status": "worked",
  "about": "Books, authors, editions, prices.",
  "modules": [["file_import", "worked"], ["authors", ""]],
  "tickets": ["NB-421"],
  "touched": [{ "ticket": "NB-421", "lines": ["app/services/catalog/file_reader.rb (new)"] }]
}
```

- `icon`: receipt, people, cloche, boxes, card, plug, screen, chart, sliders, chat, key, pipes, mail, book, cart, truck, flag.
- `pigment`: coral, pink, honey, sage, mint, sky, peach, lilac, ice, orchid, sand, teal. Leave it out and the department gets the next one in turn.
- `place`: `[column, row]` on wide screens (4 columns). Leave it out to fill in reading order.
- Keep `tickets` in sync with each ticket's `areas`: the map draws a route through every area a ticket touches.

## tickets[]

Required: `id`, `title`, `oneLine`, `status`, `areas`, `notes`, `now`, `done`, `next`.

- `now` is the current stage, `done` the stages finished; both use names from `stages`.
- `notes` is the Markdown file for the "My notes" tab; `extra` adds more tabs (`{ "label", "file" }`).
- `illo` picks an illustration: `messages`, `notes` or `scan` (with `illoText` for the scanned page's heading). Leave it out to draw a route through the ticket's areas.
- `dod`: definition-of-done steps, each `done`, `doing` or `todo`.
- `walkthrough`: a before → after comparison with screenshots. `angles`: a two-column table of edge cases, plus an optional `sub` table.
- `was`: a previous ticket id, so old links keep working.

## bugs[]

Each bug says whether it has reached `review` and `release`. Add `pr` and `when` to label it on the lanes diagram.

## timeline[]

Newest day first. Each item is `[ticketId, text]`; use `""` as the ticket for a general entry.

## meeting

Optional. When present, a **Meeting** button appears in the nav. Each ticket gets rows of *Decide / Propose / Fallback* with an input for what was decided. Press **P** to present (prep sections hide).

Decisions are saved per `collection`:

- inside a claude.ai artifact, in the artifact's shared database;
- anywhere else, in the viewer's browser (`localStorage`).

Delete the `meeting` key after the meeting and the button goes away.

## Images

Paths are relative to `index.html`, e.g. `assets/import-after-1.svg`. Give `w` and `h` when you know them to avoid layout jumps.
