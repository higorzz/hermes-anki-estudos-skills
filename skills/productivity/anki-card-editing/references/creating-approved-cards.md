# Creating approved Anki cards directly in Higor's collection

Use this when Higor approves a drafted batch and asks to add it to Anki.

## Known-good workflow from Derecho Civil batch

- Collection path used in WSL: `/mnt/c/Users/higor/AppData/Roaming/Anki2/hbzeira/collection.anki2`.
- Known Área Fiscal deck IDs used successfully:
  - Direito Civil: `Área Fiscal\x1fDireito Civil` (`did=1771364993467`).
  - Direito Tributário: `Área Fiscal\x1fDireito Tributário` (`did=1747650582304`).
  - Reforma Tributária: `Área Fiscal\x1fReforma Tributária` (`did=1789219039421`).
  - Legislação Tributária Estadual: `Área Fiscal\x1fLegislação Tributária Estadual` (`did=1790362785275`).
  - AFO e Direito Financeiro: `Área Fiscal\x1fAFO e Direito Financeiro`.
  - Contabilidade de Custos: `Área Fiscal\x1fContabilidade de Custos`.
- Deck routing defaults: Direito Financeiro/AFO/Finanças Públicas → AFO e Direito Financeiro; Contabilidade de Custos → Contabilidade de Custos; Reforma Tributária/IBS/CBS/Imposto Seletivo/EC 132/2023/LC 214/2025 → Reforma Tributária; Contabilidade Geral/Avançada that is not cost accounting → Contabilidade Geral e Avançada.
- When a batch contains Reforma Tributária cards, place those cards in the dedicated Reforma Tributária deck when requested or when the card/tag is clearly reform-specific; do not bury them in Direito Tributário by default.
- Note type for ordinary C/E cards: `Basic` (`mid=1628628663976`) with fields `Front` and `Back` separated by `\x1f`.
- Front should contain only the assertive prompt; do not prefix `TEMA`.
- Back should contain concise `GABARITO: ...` explanation. Because Anki fields render HTML rather than Markdown, do not insert Markdown emphasis such as `**texto**`; convert emphasis to `<b>texto</b>` before writing notes.
- When inserting a prior-concurso provenance note, keep it subordinate but contextual: use one `<br>` before the block and one `<br>` before any CPC/lei/NBC block after it. Include concurso/year plus assunto/subassunto, omit the matéria label because deck/tag already show it, and do not add a “Como caiu” label, e.g. `<br><div style="font-size: 65%; text-align: left;">SEFAZ RN 2025 — assunto sintaxe / regência. A banca cobrou reescrita preservando regência e sentido.</div>`.
- For generated study cards in Direito/Direito Financeiro/AFO/Legislação Tributária, append an idempotent `HERMES_LEGAL_SOURCE_V1` block at `font-size: 65%` when there is a direct statutory, constitutional, jurisprudential, or normative support. For Contabilidade/Custos, append `HERMES_CPC_SOURCE_V1` (or equivalent CFC/CPC normative support); for Auditoria, append `HERMES_NBC_SOURCE_V1` when supported by NBC/CFC wording. A generic “Fonte: banca/prova” line is useful provenance but does **not** satisfy this requirement.
- Before writing, count expected source blocks from the cards' primary tags and fail the insertion audit if any applicable card lacks its marker, unless the approval draft explicitly says reliable official support is unavailable for that card.
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