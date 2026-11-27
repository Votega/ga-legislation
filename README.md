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
| `latest.json` | JSON | **Start here** — the biennium, `currentSession`, a `sessions[]` list with each session's file paths, and `earlierBienniums[]`: every earlier General Assembly held here, newest first |
| `sessions/<slug>/bills.json` | JSON | Developers — full records (sponsors, votes, links) scoped to that session |
| `sessions/<slug>/bills.csv` | CSV | **Spreadsheets** — one row per bill, including where it ended up (`stage`) and its committees |
| `sessions/<slug>/bills.schema.json` | JSON Schema | Validating / typing `bills.json` |
| `sessions/<slug>/BILLS.md` | Markdown | **Reading** — counts by chamber, status, type, and top subjects |
| `sessions/<slug>/bills-subjects.json` | JSON | Manual subject-tag corrections applied during processing |

Resolve sessions from `latest.json`'s `sessions[]` rather than hard-coding a slug; `currentSession` names the one in progress. Earlier General Assemblies are under `earlierBienniums[]`, each with its sessions' files (empty until the first rollover, in January 2027). Bill numbers restart every General Assembly, so `HB 1` in 2025-2026 and `HB 1` in 2027-2028 are different bills: key on the session and the bill number together.

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

In the bills JSON, `chamber` is `"lower"` (House) or `"upper"` (Senate); the CSV maps these to `House`/`Senate`. New CSV columns are only ever added at the end, so code that reads columns by position keeps working.

---

## Bill records

Each `sessions/<slug>/bills.json` is `{ "metadata": {...}, "bills": [...] }`. The field list is defined by `bills.schema.json` in the same folder (its `schemaVersion` says which fields it describes; its `$id` is its own path); `metadata` records where and when that file was generated (`source`, `generatedAt`, session, and bill count) and the session's legislative calendar. This section only covers what the schema can't say.

- **`id`** is `gga-bill/<legislationId>`, where `legislationId` is the legis.ga.gov id that also appears in `billUrl`. The same number is also given in its own `legislationId` field.
- **`status` / `statusDate`** are legis.ga.gov's latest recorded action, **not an outcome**: a vetoed bill's last action reads "Date Signed by Governor". For the outcome read `governorAction` or `stage`. Every action is in `statusHistory` (`[datetime, code]`, or `[datetime, code, AmSub]`, oldest first; the codes are legis.ga.gov's).
- **`stage`** says where the bill is: a `label` ("Passed House · Died in Senate Finance"), a `kind` for filtering, and `steps` (origin committee, origin floor, other committee, other floor, final version, Governor; or filed and adopted for a simple resolution), each `done`, `current`, `stopped` or `ahead`. A bill counts as dead once its session is past the last day of its legislative calendar without reaching the Governor. In the CSV, `stage` is the label.
- **`committees`** is the committee holding the bill in each chamber **now**. legis.ga.gov overwrites it when a bill is moved or recommitted, and keeps no record of the original referral. **`committeeHistory`** lists the moves VoteGA has seen between its runs, as `{chamber, from, to, seenOn}`; `seenOn` is the day a run noticed the move, not the day it happened. It is recorded from the 2027 session on and is empty before then.
- **`committees[].chair`** is the committee's chair in the current biennium, as `{name, legisGaGovId, party}`. It comes from legis.ga.gov's member records, joined on the committee id. It is absent on bills of other bienniums and when no sitting member chairs the committee. The `stage` step for the committee a bill is in, or died in, ends "· Chair: Rep. X (R)".
- **Stall reasons in `stage`:** "Postponed in the House"; "Recommitted <date>" on the committee step; a floor loss later undone by reconsideration reads as awaiting a vote ("Lost 52–114 Mar 6 · reconsidered Mar 6"), not as lost; a second loss on reconsideration is counted on the later vote.
- **`legislationType`** is legis.ga.gov's own kind of measure: `GEN` (general bill), `LOC` (local act), `CA` (constitutional amendment), `PRIV` and `NP` (resolutions). **`footnotes`** are legis.ga.gov's procedural notes ("Modified Structured Rule", "Tabled in Senate").
- **`sponsors[]`** carry `legisGaGovId`, which joins to the `legisGaGovId` field in [`ga-legislators`](https://github.com/Votega/ga-legislators).
- **`passageVotes[]`** hold aggregate tallies per chamber (and per-party tallies where they could be computed), not individual voters. How each legislator voted is in `ga-legislators`, under `sessions/<slug>/votes.json`. `result` follows Georgia's constitutional thresholds (a majority of the chamber's seats; two-thirds for constitutional amendments, local tax-exemption acts and veto overrides), never yeas over nays. `time` orders votes taken the same day. A vote with `linkedBy: "listing"` is one legis.ga.gov links to no bill; the bill was read from the roll call's own listing.
- **`metadata.legislativeCalendar`** lists the session's legislative days per chamber, `{"lower": [[day, "YYYY-MM-DD"], ...], "upper": [...]}`. A regular session numbers its days 1–40 in each year (day 28 is Crossover Day; day 40 of the second year is sine die); a special session numbers them 1 to N.
- **`governorAction`** is an object once the Governor has acted on the measure (status, dates, act number where one exists) and `null` otherwise. A vetoed bill carries `vetoNumber`, the Governor's numbering for the year (Veto 1, 2, ...).
- **`governorStatements`** holds the Governor's veto message, or a signing statement where the Governor issued one, word for word from the Office of the Governor's yearly "Veto Messages and Signing Statements" document ([gov.georgia.gov](https://gov.georgia.gov/executive-action/legislation/vetoed-legislation)): `kind` (`veto` or `signing`), `number`, `year`, `text` (paragraphs separated by a blank line) and `sourceUrl`. legis.ga.gov does not carry these. They are added once the Governor publishes the document, after the signing deadline (mid-May), so a fresh veto has none for a few days. The budget's line-item vetoes are not included.
- **While a bill waits on the Governor**, `stage.governorDeadline` is the day the Governor must act by: six days from receiving it, or 40 days after adjournment when the General Assembly adjourns first (Ga. Const. Art. III, Sec. V, Para. XIII). A bill neither signed nor vetoed by then becomes law, and its `stage` says "Became law without the Governor's signature".
- **`abstract`** is the bill's own summary text, capped at 500 characters.

### Subject tags

Subject tags are VoteGA's own classification, not an official legis.ga.gov field. They are assembled from the O.C.G.A. title(s) a bill amends, a historical topical snapshot, manual corrections (the `bills-subjects.json` file in each session folder), and a `"Local / Municipal"` tag for bills that name a specific Georgia locality. Treat them as a convenience for filtering, not as an authoritative taxonomy.

---

## How data is updated

Files in this repo are pushed automatically from [Votega/votega.org](https://github.com/Votega/votega.org) and overwritten on each publish, so please don't edit them by hand. To report a problem, open an issue here.

---

## Linking to this data

- **A fixed session:** link `sessions/<slug>/` (for example `sessions/2025-2026/bills.json`). Those folders are never moved or rewritten once the session is over.
- **Whatever is current:** read `latest.json`. Its `biennium`, `currentSession` and `sessions[]` change when a new General Assembly is seated; the next time is **Monday, 11 January 2027**, when they name 2027-2028 and the 2025-2026 sessions move to `earlierBienniums[]`.
- **An exact copy:** link a commit (`https://raw.githubusercontent.com/Votega/ga-legislation/<commit>/...`).

Changes that affect consumers are listed in [`CHANGES.md`](CHANGES.md).

## License
[CC-BY-SA 4.0](https://github.com/Votega/ga-legislation?tab=CC-BY-SA-4.0-1-ov-file)
