# Anki SQLite single-card edit: legal article text

Session learning from a one-off edit in Higor's local Anki profile.

## Context

Local profile path found under WSL:

`/mnt/c/Users/higor/AppData/Roaming/Anki2/hbzeira/collection.anki2`

Legal decks used names like:

- `Área Fiscal\x1fDireito Administrativo`
- `Área Fiscal\x1fDireito Constitucional`
- `Área Fiscal\x1fDireito Penal`
- `Área Fiscal\x1fDireito Tributário`

A sample legal card used this tag:

`Direito_Penal::Culpabilidade::Coacao_e_Obediencia_Hierarquica`

## Pitfall discovered

An initial SQLite update appeared successful but the user still saw the old formatting in Anki. The durable fix was:

1. Backup collection.
2. Update the note/card and `col` timestamps/usn.
3. Commit.
4. Run `pragma wal_checkpoint(TRUNCATE)` if WAL files are present.
5. Close and reopen a new read-only SQLite connection and verify the persisted content from disk.

Example verification flags:

- `HAS OLD HEADER: False`
- `HAS LITERAL: True`
- `Integrity: ok`

## Formatting preference for legal article additions

Bad default: decorative insert like emoji header, `<hr>`, bold/italic, quotes.

Preferred default: literal law wording with only smaller font size, e.g.:

```html
<br><br><div style="font-size: 85%;">Coação irresistível e obediência hierárquica<br>Art. 22 - Se o fato é cometido sob coação irresistível ou em estrita obediência a ordem, não manifestamente ilegal, de superior hierárquico, só é punível o autor da coação ou da ordem.</div>
```

## Minimal write pattern

```python
import sqlite3, time, shutil
from pathlib import Path

DB = Path('/mnt/c/Users/higor/AppData/Roaming/Anki2/hbzeira/collection.anki2')
backup = DB.with_name(f'collection.anki2.backup-{int(time.time())}.sqlite')
shutil.copy2(DB, backup)

con = sqlite3.connect(DB, timeout=30)
con.create_collation('unicase', lambda a,b: (a.casefold()>b.casefold())-(a.casefold()<b.casefold()))
cur = con.cursor()

nid = 1790158375776
flds, tags = cur.execute('select flds,tags from notes where id=?', (nid,)).fetchone()
parts = flds.split('\x1f')
parts[1] = parts[1].rstrip() + '<br><br><div style="font-size: 85%;">...</div>'
now = int(time.time())

cur.execute('BEGIN IMMEDIATE')
cur.execute('update notes set flds=?, mod=?, usn=-1 where id=?', ('\x1f'.join(parts), now, nid))
cur.execute('update cards set mod=?, usn=-1 where nid=?', (now, nid))
cur.execute('update col set mod=?, scm=?', (now, now * 1000))
con.commit()
cur.execute('pragma wal_checkpoint(TRUNCATE)')
con.close()
```

Always verify in a fresh connection afterward.