# ga-legislation

Georgia bills and resolutions, and statewide ballot measures, published by [VoteGA.org](https://votega.org). The bills come from the Georgia General Assembly's own web service (legis.ga.gov). A General Assembly is a two-year *biennium* made up of a regular session plus any special sessions the Governor convenes; each session is archived separately (see below).

Files are updated automatically whenever the source data changes in the [votega.org](https://github.com/Votega/votega.org) repository.

---

## Files

Pick the format that fits how you work — the CSV and Markdown files are flattened views of the same data in the JSON.

**Bills** — archived by session under `sessions/<slug>/`

Each session in the biennium gets its own folder, never overwritten when another session begins, so this repo is a growing archive. Slugs: the **regular** session collapses to its biennium span (e.g. `2025-2026`); a **special** session keeps its identifier (e.g. `2026-ss`). `latest.json` at the root names the biennium, the session currently in progress, and every session's files.

| File | Format | Best for |
|------|--------|----------|
| `latest.json` | JSON | **Start here** — the biennium, `currentSession`, and a `sessions[]` list with each session's file paths |
| `sessions/<slug>/bills.json` | JSON | Developers — full records (sponsors, votes, links) scoped to that session |
| `sessions/<slug>/bills.csv` | CSV | **Spreadsheets** — one row per bill |
| `sessions/<slug>/bills.schema.json` | JSON Schema | Validating / typing `bills.json` |
| `sessions/<slug>/BILLS.md` | Markdown | **Reading** — counts by chamber, status, type, and top subjects |
| `sessions/<slug>/bills-subjects.json` | JSON | Manual subject-tag corrections applied during processing |

Resolve sessions from `latest.json`'s `sessions[]` rather than hard-coding a slug; `currentSession` names the one in progress.

> **Moved:** bills were previously flat files at the repo root (`ga-bills.json`, …). They now live under `sessions/<slug>/`; read `latest.json` to resolve each session's paths.

**Ballot measures**

Unlike bills, the measures file is a **single cross-cycle archive** — it spans every election and is never partitioned or deleted; consumers filter by `electionDate`. The root files below are canonical; the `ballot-measures/<year>/` folders are convenience views (filtered slices of the same data) for browsing one cycle at a time.

| File | Format | Best for |
|------|--------|----------|
| `ga-ballot-measures.json` | JSON | Developers — **all cycles**, full records |
| `ga-ballot-measures.csv` | CSV | Spreadsheets — all cycles, one row per measure |
| `ga-ballot-measures.schema.json` | JSON Schema | Validating / typing the measures file |
| [`BALLOT-MEASURES.md`](BALLOT-MEASURES.md) | Markdown | **Reading** — all cycles, grouped by election date |
| `ballot-measures/<year>/measures.json` · `.csv` · `.md` | JSON/CSV/MD | **One cycle at a time** — filtered views of the above |

Each cycle with measures has its own `ballot-measures/<year>/` folder.

In the bills JSON, `chamber` is `"lower"` (House) or `"upper"` (Senate); the CSV maps these to `House`/`Senate`.

---

## Bill records

Each `sessions/<slug>/bills.json` is `{ "metadata": {...}, "bills": [...] }`. The field list is defined by `bills.schema.json` in the same folder; `metadata` records where and when that file was generated (`source`, `generatedAt`, session, and bill count). This section only covers what the schema can't say.

- **`id`** is `gga-bill/<legislationId>`, where `legislationId` is the legis.ga.gov id that also appears in `billUrl`. The same number is also given in its own `legislationId` field.
- **`status` / `statusDate`** describe the last recorded action only. The full action history is not included, to keep the file a manageable size.
- **`sponsors[]`** carry `legisGaGovId`, which joins to the `legisGaGovId` field in [`ga-legislators`](https://github.com/Votega/ga-legislators).
- **`passageVotes[]`** hold aggregate tallies per chamber (and per-party tallies where they could be computed), not individual voters. How each legislator voted is in `ga-legislators`, under `sessions/<slug>/votes.json`.
- **`governorAction`** is an object once the Governor has acted on the measure (status, dates, act number where one exists) and `null` otherwise.
- **`abstract`** is the bill's own summary text, capped at 500 characters.

### Subject tags

Subject tags are VoteGA's own classification, not an official legis.ga.gov field. They are assembled from the O.C.G.A. title(s) a bill amends, a historical topical snapshot, manual corrections (the `bills-subjects.json` file in each session folder), and a `"Local / Municipal"` tag for bills that name a specific Georgia locality. Treat them as a convenience for filtering, not as an authoritative taxonomy.

---

## How data is updated

Files in this repo are pushed automatically from [Votega/votega.org](https://github.com/Votega/votega.org) and overwritten on each publish, so please don't edit them by hand. To report a problem, open an issue here.

---

## License
[CC-BY-SA 4.0](https://github.com/Votega/ga-legislation?tab=CC-BY-SA-4.0-1-ov-file)
