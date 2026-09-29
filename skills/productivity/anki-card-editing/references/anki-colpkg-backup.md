# Creating an importable Anki `.colpkg` backup from WSL

Use when Higor asks for an Anki backup that can be imported elsewhere, especially “coloca na área de trabalho” or “formato que eu só importe”.

## Goal

Produce a `.colpkg` file, not just a raw `collection.anki2` copy. The `.colpkg` should include:

- the collection database;
- media files from `collection.media/`;
- Anki-compatible `meta`, `collection.anki21b`, `collection.anki2`, and `media` entries;
- numbered media payload files.

## Preflight

1. Locate the profile, commonly:
   `/mnt/c/Users/higor/AppData/Roaming/Anki2/hbzeira/`
2. Check Anki is not running:
   `ps -ef | grep -i '[a]nki' || true`
3. Check WAL state:
   - `collection.anki2-wal` missing or zero bytes is OK if no Anki process is running.
   - non-empty WAL is a blocker: ask Higor to close Anki cleanly or handle checkpointing safely.
4. Find Desktop path. For Higor it has been:
   `/mnt/c/Users/higor/OneDrive/Área de Trabalho`

## Python dependencies

The WSL environment may not have Anki Python modules. You can still create a portable `.colpkg` with stdlib + `zstandard`:

```bash
python3 -m venv /tmp/anki-backup-venv
/tmp/anki-backup-venv/bin/pip install -q zstandard
```

## Known-good script

```python
import sqlite3, zipfile, time, json
from pathlib import Path
import zstandard as zstd

profile = Path('/mnt/c/Users/higor/AppData/Roaming/Anki2/hbzeira')
collection = profile / 'collection.anki2'
media_dir = profile / 'collection.media'
desktop = Path('/mnt/c/Users/higor/OneDrive/Área de Trabalho')
ts = time.strftime('%Y-%m-%d_%H-%M-%S')
out = desktop / f'backup-anki-higor-{ts}.colpkg'
snapshot = Path('/tmp') / f'anki_collection_snapshot_{ts}.anki2'

# Consistent SQLite snapshot; safer than copying collection.anki2 directly.
src = sqlite3.connect(f'file:{collection}?mode=ro', uri=True)
src.create_collation('unicase', lambda a,b: (a.casefold()>b.casefold())-(a.casefold()<b.casefold()))
dst = sqlite3.connect(snapshot)
src.backup(dst)
dst.close(); src.close()

con = sqlite3.connect(snapshot)
con.create_collation('unicase', lambda a,b: (a.casefold()>b.casefold())-(a.casefold()<b.casefold()))
integrity = con.execute('pragma integrity_check').fetchone()[0]
notes = con.execute('select count(*) from notes').fetchone()[0]
cards = con.execute('select count(*) from cards').fetchone()[0]
con.close()
if integrity != 'ok':
    raise SystemExit(f'integrity failed: {integrity}')

media_files = []
if media_dir.exists():
    media_files = [p for p in sorted(media_dir.iterdir(), key=lambda x: x.name) if p.is_file() and not p.name.startswith('.')]
media_map = {str(i): p.name for i, p in enumerate(media_files)}

cctx = zstd.ZstdCompressor(level=3)
compressed_collection = cctx.compress(snapshot.read_bytes())
compressed_media = cctx.compress(json.dumps(media_map, ensure_ascii=False, separators=(',', ':')).encode('utf-8'))

with zipfile.ZipFile(out, 'w', compression=zipfile.ZIP_DEFLATED, compresslevel=6, allowZip64=True) as z:
    z.writestr('meta', b'\x08\x03')
    z.writestr('collection.anki21b', compressed_collection)
    z.write(snapshot, 'collection.anki2')
    z.writestr('media', compressed_media)
    for i, p in enumerate(media_files):
        z.write(p, str(i))

with zipfile.ZipFile(out) as z:
    bad = z.testzip()
if bad:
    raise SystemExit(f'zip test failed at {bad}')

print({'output': str(out), 'notes': notes, 'cards': cards, 'media_files': len(media_files), 'integrity': integrity})
```

## Verification

After creating the file:

1. Open the zip and run `testzip()`.
2. Decompress `collection.anki21b` with zstandard stream reader; verify SQLite `pragma integrity_check = ok`.
3. Decompress `media`; JSON count should match the number of numbered media files.
4. Optionally verify a known recently-added note exists in the decompressed collection if the backup was requested after card insertion.
5. Report path, size, note/card count, media count, and verification results.

## Pitfalls

- Do not call the output a portable backup if it is only `collection.anki2`; that omits media and is not the normal import format.
- `collection.anki21b` in Anki `.colpkg` files is zstd-compressed and may not include content size in the frame header; use `ZstdDecompressor().stream_reader(...)` for verification.
- The `media` entry is also zstd-compressed JSON mapping numbered archive entries to original filenames.
- A zero-byte WAL with stale `-shm` is not necessarily a blocker when no Anki process is running.
