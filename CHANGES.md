# Changes

Changes to this repo's files that a consumer may need to act on. Routine data refreshes are not listed.

## Coming: 11 January 2027, new General Assembly

- `latest.json` names 2027-2028: `biennium`, `currentSession`, `sessions[]`, `billCount`. New bills go to new folders under `sessions/`.
- The 2025-2026 regular and 2026 special sessions move to `earlierBienniums[]`. Their folders, `sessions/2025-2026/` and `sessions/2026-ss/`, do not move.
- Bill numbers start again at 1, so a bill number on its own no longer identifies a bill. Key on the session and the number.

## November 2026: corrected vote results

A roll call now passes only with a majority of the chamber's seats, 91 of 180 in the House and 29 of 56 in the Senate (Ga. Const. Art. III, Sec. V, Para. V). Two kinds of measure need two-thirds of the members elected, 120 / 38: a resolution proposing a constitutional amendment (Art. X, Sec. I, Para. II), and a local act granting or changing a property-tax (homestead) exemption (Art. VII, Sec. II, Para. II(a); legis.ga.gov records every such act below 120 / 38 as lost). Before, `result` was `Pass` whenever yeas outnumbered nays, and the two-thirds rule was applied only to 2025-2026 amendments. Each vote below was published as `Pass`. legis.ga.gov's status history records it as lost ("Third Reading Lost", "Senate Lost"), or the chamber took it again the same day and the re-vote passed. Only `passageVotes[].result` changed.

Changed file: `sessions/2025-2026/bills.json` (and `bills.csv` where it carries the result). Bills from earlier General Assemblies are not published here.

| Bills | Chamber | Date | Tally | `result` now |
|---|---|---|---|---|
| HB 360 | Senate | 2025-03-28 | 26-24 | `fail` |
| SB 234, SB 235, SB 336 | House | 2025-04-04 | 112-59 | `fail` |
| SB 170 | Senate | 2026-01-28 | 27-22 | `fail` |
| HB 1324 | House | 2026-03-06 | 87-76 | `fail` |
| SB 568 | Senate | 2026-03-06 | 27-21 | `fail` |
| SB 175 | House | 2026-03-31 | 89-73 | `fail` |
| SB 614, SB 627 | House | 2026-04-02 | 94-75 | `fail` |
| HB 10EX, HB 11EX, HB 12EX, HB 13EX, HB 14EX, HB 15EX, HB 16EX, HB 17EX, HB 18EX, HB 19EX, HB 1EX, HB 20EX, HB 21EX, HB 22EX, HB 23EX, HB 24EX, HB 25EX, HB 26EX, HB 27EX, HB 28EX, HB 29EX, HB 2EX, HB 30EX, HB 31EX, HB 32EX, HB 33EX, HB 34EX, HB 35EX, HB 36EX, HB 37EX, HB 38EX, HB 39EX, HB 3EX, HB 40EX, HB 41EX, HB 42EX, HB 43EX, HB 44EX, HB 45EX, HB 46EX, HB 47EX, HB 48EX, HB 49EX, HB 4EX, HB 50EX, HB 51EX, HB 52EX, HB 53EX, HB 54EX, HB 55EX, HB 56EX, HB 57EX, HB 59EX, HB 5EX, HB 60EX, HB 61EX, HB 62EX, HB 63EX, HB 64EX, HB 65EX, HB 66EX, HB 67EX, HB 6EX, HB 7EX, HB 8EX, HB 9EX | House | 2026-06-20 | 95-67 | `fail` |
| SB 1EX, SB 2EX, SB 4EX | Senate | 2026-06-20 | 33-18 | `fail` |
| SB 1EX | Senate | 2026-06-20 | 33-18 | `fail` |
| SB 2EX | Senate | 2026-06-20 | 33-18 | `fail` |
| SB 4EX | Senate | 2026-06-20 | 33-15 | `fail` |
| HB 68EX, HB 70EX, HB 71EX, HB 72EX, HB 73EX, HB 74EX | House | 2026-06-22 | 97-74 | `fail` |
| HB 10EX, HB 11EX, HB 12EX, HB 13EX, HB 14EX, HB 15EX, HB 16EX, HB 17EX, HB 18EX, HB 19EX, HB 1EX, HB 20EX, HB 21EX, HB 22EX, HB 23EX, HB 24EX, HB 25EX, HB 26EX, HB 27EX, HB 28EX, HB 29EX, HB 2EX, HB 30EX, HB 31EX, HB 32EX, HB 33EX, HB 34EX, HB 35EX, HB 36EX, HB 37EX, HB 38EX, HB 39EX, HB 3EX, HB 40EX, HB 41EX, HB 42EX, HB 43EX, HB 44EX, HB 45EX, HB 46EX, HB 47EX, HB 48EX, HB 49EX, HB 4EX, HB 50EX, HB 51EX, HB 52EX, HB 53EX, HB 54EX, HB 55EX, HB 56EX, HB 57EX, HB 59EX, HB 5EX, HB 60EX, HB 61EX, HB 62EX, HB 63EX, HB 64EX, HB 65EX, HB 66EX, HB 67EX, HB 6EX, HB 7EX, HB 8EX, HB 9EX | House | 2026-06-22 | 97-75 | `fail` |
| HB 10EX, HB 11EX, HB 12EX, HB 13EX, HB 14EX, HB 15EX, HB 16EX, HB 17EX, HB 18EX, HB 19EX, HB 1EX, HB 20EX, HB 21EX, HB 22EX, HB 23EX, HB 24EX, HB 25EX, HB 26EX, HB 27EX, HB 28EX, HB 29EX, HB 2EX, HB 30EX, HB 31EX, HB 32EX, HB 33EX, HB 34EX, HB 35EX, HB 36EX, HB 37EX, HB 38EX, HB 39EX, HB 3EX, HB 40EX, HB 41EX, HB 42EX, HB 43EX, HB 44EX, HB 45EX, HB 46EX, HB 47EX, HB 48EX, HB 49EX, HB 4EX, HB 50EX, HB 51EX, HB 52EX, HB 53EX, HB 54EX, HB 55EX, HB 56EX, HB 57EX, HB 59EX, HB 5EX, HB 60EX, HB 62EX, HB 63EX, HB 64EX, HB 65EX, HB 66EX, HB 67EX, HB 6EX, HB 7EX, HB 8EX, HB 9EX | House | 2026-06-22 | 99-74 | `fail` |
| SB 5EX, SB 6EX, SB 7EX, SB 8EX, SB 9EX | Senate | 2026-06-22 | 31-20 | `fail` |
| HB 75EX, HB 76EX, HB 77EX, HB 78EX, HB 79EX, HB 80EX, HB 81EX, HB 82EX, HB 83EX | House | 2026-06-23 | 96-71 | `fail` |
| HB 75EX, HB 76EX, HB 77EX, HB 78EX, HB 79EX, HB 80EX, HB 81EX, HB 82EX, HB 83EX | House | 2026-06-23 | 97-72 | `fail` |
| HB 75EX, HB 76EX, HB 77EX, HB 78EX, HB 79EX, HB 80EX, HB 81EX, HB 82EX, HB 83EX | House | 2026-06-23 | 97-72 | `fail` |
| HB 68EX, HB 70EX, HB 71EX, HB 72EX, HB 73EX, HB 74EX | House | 2026-06-23 | 97-72 | `fail` |
| HB 68EX, HB 70EX, HB 71EX, HB 72EX, HB 73EX, HB 74EX | House | 2026-06-23 | 97-72 | `fail` |
| SB 5EX | Senate | 2026-06-23 | 32-19 | `fail` |
| SB 6EX | Senate | 2026-06-23 | 32-20 | `fail` |

## November 2026

- `latest.json` gained `earlierBienniums[]`, newest first. It is empty until the rollover.
