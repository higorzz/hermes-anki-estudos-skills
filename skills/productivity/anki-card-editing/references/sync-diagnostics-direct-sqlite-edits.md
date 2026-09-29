# Anki sync diagnostics after direct SQLite edits

Use this when Higor reports repeated large Anki sync counts after Hermes/SQL edits, especially counts like thousands of `↓` after changing only one card.

## Read-only diagnostic pattern

1. Treat this as a sync-state investigation first, not an immediate DB repair.
2. Confirm Anki is closed before interpreting SQLite side files. If possible check both WSL/Linux and Windows processes.
3. Inspect the live profile DB read-only, usually:
   `/mnt/c/Users/higor/AppData/Roaming/Anki2/<profile>/collection.anki2`
4. Use SQLite read-only URI and define the `unicase` collation before indexed text queries:

```python
con = sqlite3.connect('file:' + path + '?mode=ro', uri=True)
con.create_collation('unicase', lambda a,b: (a.casefold()>b.casefold())-(a.casefold()<b.casefold()))
```

5. Check:
   - `pragma integrity_check`;
   - `collection.anki2-wal` and `collection.anki2-shm` presence/mtime/size;
   - totals for `notes`, `cards`, `graves`, `revlog`;
   - `count(*) where usn=-1` for `notes`, `cards`, `graves`, and optionally `revlog`;
   - recent `mod` buckets for notes/cards;
   - large `usn` clusters: `select usn,count(*) from cards group by usn order by count(*) desc limit 15`.

## Interpretation

- Many `usn=-1` rows after Hermes edits means the local desktop has unsynced local changes; large upload counts are expected.
- `usn=-1 = 0` for notes/cards/graves plus `integrity_check=ok` means the local DB is not sitting with thousands of dirty rows from Hermes SQL edits. Large `↓` counts are likely remote/AnkiWeb/cellphone sync state or prior Anki corrections being downloaded.
- A stale `collection.anki2-shm` without `collection.anki2-wal` does not by itself prove pending writes. Be cautious, but do not overstate it.
- Large clusters of cards sharing the same non-negative `usn` and identical `mod` timestamp usually point to a prior batch sync/agendamento/check-database event, not necessarily current Hermes content edits.

## User-facing guidance

For Higor, keep the answer direct:

- Say whether the local DB is dirty or clean.
- Give the exact counts (`notes usn=-1`, `cards usn=-1`, `integrity_check`).
- If clean locally but sync still downloads thousands, explain that the issue is probably remote sync state/AnkiWeb/celular rather than Hermes leaving local SQL edits pending.
- Recommend backup/export before any forced one-way sync.
- If choosing a source of truth, prefer the desktop collection only after Higor confirms it looks correct.

## Safe next steps when local DB is clean but downloads continue

1. Desktop Anki with add-ons disabled (`Shift`) → sync until complete.
2. Sync desktop again; it should be zero/near-zero.
3. Sync mobile until complete; sync again; should be zero/near-zero.
4. If large downloads still recur after a controlled one-card edit, consider Anki's one-way sync/reset using the verified desktop as source, after a fresh export/backup.
