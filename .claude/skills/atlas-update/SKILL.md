---
name: atlas-update
description: Keep this Code Atlas current. Use at the end of any task that changes a ticket's status, notes, branch, areas touched, bugs found or concepts asked about, or when the user says "update the atlas".
---

# Updating the atlas

The atlas is a static site: `index.html` renders `data/site.json` and the Markdown files in `notes/`. Your job is to keep the data true to what actually happened. Never edit `index.html` to change content.

## 1. Gather what changed

Read, don't guess:

- `git log --since="<data.updated>" --date=iso --stat` in the project repo the atlas describes (ask the user for its path once, then remember it).
- `git branch --show-current` and `git rev-parse --short HEAD` for the ticket being worked on.
- The ticket's notes file in `notes/`, and anything the user told you in this conversation (decisions, who asked, what's blocked).

## 2. Update the notes first

Each ticket has `notes/<ID>.md` with these sections (create the file from this shape if it's new):

```
# <emoji> <ID> · <title>
## 🎯 What it is        one paragraph + a small ASCII diagram
## ✅ What I've done    checklist, newest last
## 🧭 Decisions         table: date · decision · who
## ⚠️ Possible blockers
## 🧠 Things I asked about   one line + tiny picture per concept
```

Add a dated entry to `notes/daily-log.md` (newest day first): what was done per ticket, and what you're waiting on and from whom.

## 3. Update `data/site.json`

| Something happened | Change |
|---|---|
| New ticket | Add to `tickets[]` (id, title, oneLine, status, areas, notes, now, done, next); add its id to every department in `areas` |
| Stage moved | `now` = new stage, previous one appended to `done` |
| Code written in an area | Department and module status → `"worked"`; add a `touched` entry with the files |
| Read or mocked an area | Status → `"investigating"` (never downgrade `"worked"`) |
| New branch/commit | `branch`, `commit` |
| Bug found | Append to `bugs[]` with the next `n` |
| User asked "what is X?" | Append to `concepts[]` with one line + a tiny ASCII picture |
| Anything notable | Add an item to today's `timeline[]` entry (create it at the top if missing) |

Always set `updated` to today (YYYY-MM-DD).

## 4. Check before you finish

- The JSON parses, and it validates against `data/site.schema.json`:
  `python -c "import json,jsonschema; jsonschema.validate(json.load(open('data/site.json')), json.load(open('data/site.schema.json')))"`
- Every id in a department's `tickets` exists in `tickets[]`, and every ticket's `areas` lists that department.
- `timeline` is newest first.
- Every `notes`, `extra[].file` and image path exists.
- No secrets, customer data or anything the user marked private.

Then tell the user in one line what changed on the atlas.
