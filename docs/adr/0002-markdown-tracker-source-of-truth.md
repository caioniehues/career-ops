# Markdown tracker is the source of truth, not a database

The application Tracker is a markdown table (`data/applications.md`). New rows are never
edited into it directly by agents — each Evaluation writes a one-line TSV to
`batch/tracker-additions/`, and `merge-tracker.mjs` merges them (handling dedup, the
status/score column swap, and report-link normalization). The SQLite `data/applications.db`
is a **derived** query index, rebuilt by `tracker.mjs sync` and safe to delete.

We picked markdown-first over a database-first design because the Tracker must stay
**human-readable, git-diffable, and hand-editable** — the user owns it and reviews every
change. A DB as source of truth would make the User layer opaque and merge-hostile. The
trade-off: concurrent batch writers can't append safely to one file, which is exactly why
additions go through per-evaluation TSV files plus a deterministic merge step rather than
in-place edits. Querying/aggregation is recovered by projecting into the derived `.db`.
