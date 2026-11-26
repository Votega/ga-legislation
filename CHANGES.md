# Changes

Changes to this repo's files that a consumer may need to act on. Routine data refreshes are not listed.

## Coming: 11 January 2027, new General Assembly

- `latest.json` names 2027-2028: `biennium`, `currentSession`, `sessions[]`, `billCount`. New bills go to new folders under `sessions/`.
- The 2025-2026 regular and 2026 special sessions move to `earlierBienniums[]`. Their folders, `sessions/2025-2026/` and `sessions/2026-ss/`, do not move.
- Bill numbers start again at 1, so a bill number on its own no longer identifies a bill. Key on the session and the number.

## November 2026

- `latest.json` gained `earlierBienniums[]`, newest first. It is empty until the rollover.
