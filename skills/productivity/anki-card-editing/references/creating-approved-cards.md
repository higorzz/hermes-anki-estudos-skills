# Creating approved Anki cards directly in Higor's collection

Use this when Higor approves a drafted batch and asks to add it to Anki.

## Known-good workflow from Derecho Civil batch

- Collection path used in WSL: `/mnt/c/Users/higor/AppData/Roaming/Anki2/hbzeira/collection.anki2`.
- Known Área Fiscal deck IDs used successfully:
  - Direito Civil: `Área Fiscal\x1fDireito Civil` (`did=1771364993467`).
  - Direito Tributário: `Área Fiscal\x1fDireito Tributário` (`did=1747650582304`).
  - Reforma Tributária: `Área Fiscal\x1fReforma Tributária` (`did=1789219039421`).
  - Legislação Tributária Estadual: `Área Fiscal\x1fLegislação Tributária Estadual` (`did=1790362785275`).
- When a batch contains Reforma Tributária cards, place those cards in the dedicated Reforma Tributária deck when requested or when the card/tag is clearly reform-specific; do not bury them in Direito Tributário by default.
- Note type for ordinary C/E cards: `Basic` (`mid=1628628663976`) with fields `Front` and `Back` separated by `\x1f`.
- Front should contain only the assertive prompt; do not prefix `TEMA`.
- Back should contain concise `GABARITO: ...` explanation. For legal cards with direct statutory support, append an idempotent `HERMES_LEGAL_SOURCE_V1` block at `font-size: 65%`.
- Tags must use exactly three hierarchy levels, e.g. `direito_civil::negocio_juridico::condicao`, stored in `notes.tags` with leading/trailing spaces.

## Safety notes

1. Check for Anki processes before writing.
2. Refuse to write if `collection.anki2-wal` exists and is non-empty.
3. A stale `collection.anki2-shm` file may remain with zero-byte WAL after a clean close; do not treat the `-shm` file alone as a hard blocker if no Anki process is running and the WAL is empty.
4. Create a timestamped backup before inserting.
5. Insert into `notes` and `cards`, update/create `tags`, and bump `col.mod`/`col.scm`.
6. Use `usn=-1` for local changes.
7. Verify after writing with read-only reopen:
   - `pragma integrity_check = ok`;
   - inserted note count equals approved card count;
   - inserted cards are in the intended deck;
   - all tags have exactly three levels;
   - fronts do not start with `TEMA`;
   - legal-source block count matches cards that received article support.

## Article-source rule

For legal batches, include official/legal support blocks only when the card has a direct and safe statutory basis. Do not force article blocks on purely doctrinal cards such as classifications that have no specific dispositive text.