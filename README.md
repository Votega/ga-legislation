# ga-legislation

Georgia legislative data for the 2025–2026 General Assembly session, published by [VoteGA.org](https://votega.org).

Files are updated automatically whenever the source data changes in the [votega.org](https://github.com/Votega/votega.org) repository.

---

## Files

| File | Description |
|------|-------------|
| `ga-bills.json` | All bills and resolutions introduced in the 2025–2026 session |
| `ga-bills-subjects.json` | Manual subject-tag overrides applied during processing |

---

## `ga-bills.json`

### Top-level structure

```json
{
  "metadata": {
    "generatedAt": "2026-05-01T12:00:00+00:00",
    "session": "2025_26",
    "sessionName": "2025-2026 Regular Session",
    "source": "Open States (bulk export — May 2026)",
    "totalBills": 4200
  },
  "bills": [ ... ]
}
```

### Bill object

| Field | Type | Description |
|-------|------|-------------|
| `id` | string | Open States bill ID (e.g. `ocd-bill/...`) |
| `identifier` | string | Bill number (e.g. `HB 123`, `SB 45`) |
| `billType` | string | `"bill"`, `"resolution"`, `"joint resolution"`, etc. |
| `chamber` | string | `"lower"` (House) or `"upper"` (Senate) |
| `title` | string | Full bill title |
| `abstract` | string | Short description, capped at 500 characters (empty string if none) |
| `status` | string | Description of the last recorded action |
| `statusDate` | string | ISO date of the last recorded action |
| `subjects` | string[] | Subject tags (from Open States or inferred from title) |
| `sponsors` | object[] | See below |
| `billUrl` | string | Link to the bill on legis.ga.gov (falls back to first source URL) |
| `textUrl` | string | Direct link to bill text (may be empty) |
| `passageVotes` | object[] | Passage vote tallies per chamber — see below |

#### `sponsors[]`

```json
{ "name": "Jane Smith", "primary": true }
```

#### `passageVotes[]`

```json
{
  "chamber": "lower",
  "date": "2025-03-15",
  "result": "pass",
  "motionText": "Passage",
  "yea": 105,
  "nay": 62,
  "other": 3
}
```

`chamber` is `"lower"` or `"upper"`. `result` is `"pass"` or `"fail"`.

### Subject tags

Subject tags come from Open States. Bills with no Open States tags may receive an inferred `"Local / Municipal"` tag if the title identifies a specific Georgia locality (e.g. "City of Marietta; ..."). Additional tags are applied via `ga-bills-subjects.json`.

---

## `ga-bills-subjects.json`

A maintainer-controlled file that overrides or adds subject tags for specific bills. Keys are bill identifiers (`"HB 123"`); values are subject-tag arrays. Keys beginning with `_` are metadata comments and are ignored during processing.

```json
{
  "_note": "Manual subject overrides ...",
  "SB 30": ["Health"],
  "HB 741": ["Local / Municipal"]
}
```

---

## How data is updated

Files in this repo are pushed automatically from [Votega/votega.org](https://github.com/Votega/votega.org) via a GitHub Actions workflow (`.github/workflows/publish-ga-bills-to-ga-legislation.yml`). The workflow runs whenever `ga-bills.json` or `ga-bills-subjects.json` changes on the `main` branch, and can also be triggered manually.

Source data comes from [Open States](https://openstates.org/) and is processed by `scripts/process_ga_bills.py` in the votega.org repo.

---

## Data notes

- **`actions` are omitted.** The full action history is not included to keep the file size manageable. `status` and `statusDate` reflect the last action only.
- **Individual voter arrays are omitted.** `passageVotes` contains only aggregate yea/nay/other counts. Per-member vote records are in `ga-member-votes.json` in the votega.org repo, keyed by OCD person ID.
- **`abstract` may be empty.** Not all bills have an abstract in the Open States data.
- This dataset covers the **2025–2026 Regular Session** only.
