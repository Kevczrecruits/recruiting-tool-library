# recruiting tool library

a living index of useful free web software and ai tools for recruiting work — sourcing, outreach, research, decks, and docs. kept as its own repo, separate from kevin's other github projects.

## objective

keep one organized, shareable home for tools worth remembering (chatgpt, claude, gemini, notebooklm, canva, gamma, figma, smallpdf, clay, crust data, …) so kevin — or any agent working with him — can browse, search, and extend the list without digging through chat history. the list is meant to keep growing over time.

## what's here

| file | role |
|---|---|
| `index.html` | the tracker ui — glassmorphism cards, live search, category filters, an add-tool helper that generates csv rows, and a one-click csv export |
| `tools.csv` | **source of truth** — one row per tool: `name,url,category,description,cost,tags,notes,icon` |
| `icons/` | one official logo per tool — the canonical files are `icons/<slug>.png` (e.g. `icons/granola.png`), kept locally |
| `icons/icons-N.js` | the same logos embedded as base64 data URIs, loaded by the page (see note below) — rendered on each card at 52px |
| `README.md` | this file — goal, history, and handoff notes |

to view the tracker: open `index.html` in a browser, or serve the folder / enable github pages. served over http(s) the page reads `tools.csv` live; opened from `file://` it falls back to the embedded `TOOL_DATA` copy.

## work done so far (oct 4, 2026)

- built locally at `~/workspace/recruiting-tool-library/` — `index.html`, `tools.csv` (source of truth), and this readme
- seeded with **18 tools** across **7 categories**: ai assistants, research & notebooks, design & decks, pdf & docs, sourcing & data, productivity, build & host
- github push **pending, not done**: repo creation is blocked (github app lacks repo-create permission, 403 — same limitation as the sep 28 childcare repo). kevin needs to create the empty `recruiting-tool-library` repo himself and grant the muse app access, then the files can be pushed.
- seed list: chatgpt, claude, gemini, notebooklm, canva, gamma, figma, smallpdf, clay, crust data, linkedin, github, notion, granola, v0, vercel, perplexity, google scholar
- tracker ui built in the "gen x softclub glassmorphism" style: soft light gradient + faint grid mesh, frosted-glass cards, obsidian-navy pills, electric-cyan status dots, space grotesk / space mono type

## how to add a tool

1. append one row to `tools.csv`, keeping the header order (`name,url,category,description,cost,tags,notes,icon`). quote any field that contains a comma. drop the tool's official logo into `icons/` as `icons/<slug>.png` (lowercase name, letters/numbers only — the add-tool form suggests this automatically) and put that path in the `icon` column.

**icon plumbing note:** the GitHub file API used for pushes cannot transport binary PNGs, so the repo carries the logos as base64 data URIs in `icons/icons-N.js` (each file stays under ~100KB for the push transport). `index.html` loads those scripts and resolves each tool's `icon` path to its data URI by slug; if no embedded icon matches, it falls back to the path itself. to add a logo for a new tool: save the official PNG locally as `icons/<slug>.png`, then append `"<slug>": "data:image/png;base64,..."` to the smallest `icons-N.js` (or ask an agent to regenerate them). icons must be the tool's real published logo pulled from its official site — never invented.
2. refresh `index.html` — when served over http(s) it picks the csv up automatically.
3. if you used the ui's **+ add tool** form, paste the generated row into `tools.csv` so the source stays complete (the form also offers a full-csv export).
4. when opening from `file://`, also update the embedded `TOOL_DATA` array in `index.html` to match.

## agent / llm handoff

another agent picking this up should:

1. read `tools.csv` first — it is the source of truth. do not trust the embedded `TOOL_DATA` copy alone; it can lag.
2. keep `category` values consistent with the existing seven (`AI Assistants`, `Research & Notebooks`, `Design & Decks`, `PDF & Docs`, `Sourcing & Data`, `Productivity`, `Build & Host`). add a new category only deliberately — the ui builds its filter pills from whatever categories exist.
3. keep `description` to one terse, use-case-first line.
4. `cost` is one of: `Free`, `Freemium`, `Paid`.
5. `tags` are comma-separated, lowercase, no spaces after commas.
6. after editing, update the embedded `TOOL_DATA` array in `index.html` with the same rows so `file://` opens stay current, then commit and push. visual experiments go on a feature branch first (e.g. `add-tool-icons`) — `main` stays the safe copy until the branch is merged; deleting the branch is the undo.
7. never invent urls — use the tool's real homepage, verified, not guessed.
