---
name: anki-card-editing
description: Safely inspect and edit a user's local Anki collection, especially one-off card/note text changes and legal-study cards that need cited article text added to the answer side.
---

# Anki Card Editing

Use this skill when the user asks to access, inspect, search, edit, insert approved drafts into, or bulk-fix local Anki cards/notes/decks.

Scope boundary: this skill is the **database/Anki operations skill**. It does not decide study pedagogy, quantity, wording, tags, or card style from source material. For generating new Área Fiscal cards from questions, laws, CPC/NBC standards, screenshots, PDFs, or study material, first use `tec-concursos-anki-cards`; after Higor approves the draft, use this skill only to safely write/update/verify the approved cards in Anki.

## Safety workflow

1. Locate the Anki profile and collection, commonly on Windows via WSL:
   - `/mnt/c/Users/<user>/AppData/Roaming/Anki2/<profile>/collection.anki2`
2. Check whether Anki is running before writes. If it is, ask the user to close Anki or proceed only with read-only inspection/copy-based analysis.
3. If `collection.anki2-wal` exists and is non-empty even though no Anki process is visible, do not write immediately. Ask Higor to confirm Anki is closed; after confirmation, preserve timestamped pre-checkpoint copies of `collection.anki2`, `collection.anki2-wal`, and `collection.anki2-shm`, run `PRAGMA wal_checkpoint(FULL)`, then continue only if `integrity_check` is ok.
4. Before any write, create a timestamped backup next to `collection.anki2`.
5. Prefer editing a single note first as a sample before bulk changes.
5. After writing, verify by reopening the database read-only and checking:
   - target note/card fields contain the intended text;
   - stale text was removed;
   - generated source-block markers are balanced (for example exactly one `<!-- HERMES_LEGAL_SOURCE_V1 -->` and one `<!-- /HERMES_LEGAL_SOURCE_V1 -->` when editing a single legal block); orphan closing markers can remain if the previous block was malformed, so explicitly count start/end markers;
   - for newly inserted/edited Direito, Legislação Tributária, Contabilidade/CPC, Auditoria/NBC, and correlated official-standard cards, every target note has the expected `HERMES_LEGAL_SOURCE_V1`, `HERMES_CPC_SOURCE_V1`, or `HERMES_NBC_SOURCE_V1` marker unless explicitly documented as source-unavailable;
   - `pragma integrity_check` returns `ok`.
6. If WAL files exist or a prior edit does not appear in Anki, force a WAL checkpoint after commit, then reopen the DB from disk to verify persistence.
7. When Higor reports repeated large Anki sync counts after Hermes/direct-SQLite edits, diagnose sync state read-only before proposing repairs: check `integrity_check`, WAL/SHM side files, `usn=-1` counts for `notes`/`cards`/`graves`, recent `mod` buckets, and large non-negative `usn` clusters. If `usn=-1` is zero and integrity is ok, the local DB is not sitting with thousands of dirty Hermes edits; large download counts likely come from AnkiWeb/mobile/previous Anki corrections. See `references/sync-diagnostics-direct-sqlite-edits.md`.
8. For direct insertion of approved new cards, see `references/creating-approved-cards.md` for the known-good workflow: Basic note insertion, Área Fiscal deck mapping, 3-level tags, legal-source blocks, and the stale `-shm`/empty-WAL pitfall.
8. **Pre-insertion source-block audit for study cards:** before inserting any generated Direito, Direito Financeiro/AFO, Legislação Tributária, Contabilidade/Custos, or Auditoria cards, scan the approved batch and verify each applicable card already has the expected official/normative marker (`HERMES_LEGAL_SOURCE_V1`, `HERMES_CPC_SOURCE_V1`, or `HERMES_NBC_SOURCE_V1`) unless the draft explicitly marks source unavailable. A generic “Fonte: banca/prova” line is not a substitute for these blocks. Do not report success until this audit passes; if missing, enrich the draft/DB first and re-verify marker counts.

## Modern Anki SQLite notes

Some newer Anki collections have normalized tables (`decks`, `notetypes`, `fields`, `templates`, `tags`) and blank JSON fields in `col`. Do not assume `col.decks`/`col.models` contains JSON.

When using Python `sqlite3`, define Anki's `unicase` collation before queries that may touch indexed text tables:

```python
con.create_collation('unicase', lambda a,b: (a.casefold()>b.casefold())-(a.casefold()<b.casefold()))
```

Relevant tables:
- `notes`: `id`, `mid`, `tags`, `flds`, `mod`, `usn`; fields are separated by `\x1f`.
- `cards`: `id`, `nid`, `did`, `ord`, `mod`, `usn`.
- `decks`: `id`, `name`.
- `fields`: `ntid`, `ord`, `name`.
- `tags`: `tag`.

For changes, update the note field, set `notes.mod`, `notes.usn=-1`, matching `cards.mod`, `cards.usn=-1`, and bump `col.mod`/`col.scm`.

## Área Fiscal tag standard

When creating, inserting, or bulk-normalizing Higor's Área Fiscal Anki cards, use the approved tag standard:

- lowercase ASCII only;
- no accents, no spaces, no hyphens;
- words separated with `_`;
- hierarchy separated with `::`;
- default shape: `materia::assunto_amplo::subassunto`;
- use at least 3 hierarchy levels so Anki exposes a dropdown for the subassunto; do not leave broad two-level tags like `economia::politica_monetaria`;
- for norm/CPC/NBC/Lei-based trees, use 4 levels when the middle topic should be browsable, e.g. `contabilidade::cpc_03::dfc::metodo_direto`, `contabilidade::cpc_09::dva::estrutura`, and `contabilidade::lei_6404::patrimonio_liquido::reserva_legal`;
- if the card is broad, use a stable third level such as `geral` or `instrumentos`, but prefer a meaningful subassunto;
- use deck/subdeck for broad subject placement and tags for precise retrieval;
- do not place the human-facing `TEMA` at the start of the card front by default, because it can cue the answer.

Examples:

- `direito_administrativo::licitacoes::garantia_de_proposta`
- `direito_constitucional::stf::sumula_vinculante::efeitos`
- `direito_penal::teoria_do_crime::erro_de_tipo_e_proibicao`
- `contabilidade::cpc_23::politicas_estimativas_erros`
- `economia::politica_fiscal::instrumentos`
- `economia::politica_fiscal::estabilizadores_automaticos`
- `economia::politica_monetaria::instrumentos_classicos`
- `economia::politica_monetaria::instrumentos_nao_convencionais`
- `economia::moeda::agregados_monetarios`
- `economia::moeda::papel_moeda`
- `ti::itil4::principios_orientadores`

For tag rewrites, treat them as database writes: create a backup, update `notes.tags`, set `notes.mod/usn=-1`, update matching `cards.mod/usn=-1`, bump `col.mod/scm`, maintain the `tags` table when possible, reopen read-only, verify all target notes have canonical tags, and run `pragma integrity_check`.

## Legal-card article text style

When adding cited legal article wording to the back/answer side of Higor's legal cards:

- Use official Brazilian authority sources for the current wording; prefer Planalto/Presidência compiled pages for federal statutes and Constitution. Do **not** bulk-insert article text from memory.
- Keep the statutory text as literal as possible, matching the law's heading and article wording.
- For screenshots/specific cards where Higor says the lei seca is too large, work **one card at a time**: identify the note by its front text, add only the minimum legal excerpt that supports that card, verify marker count + `integrity_check`, then proceed to the next card only if requested. Higor explicitly prefers this slower per-card workflow to avoid breaking multiple cards at once.
- If Higor changes scope from “editar” to “refazer/criar novo e deixar o antigo”, do **not** update the existing note. Create a new equivalent note in the same deck/tag with the revised concise legal-source block, and leave the old card untouched.
- Compact unnecessary source line-wraps from Planalto: keep line breaks only when moving to a new legal unit (article, paragraph, inciso, alínea, or a true heading). Do not preserve arbitrary HTML/text wraps that make the card tall.
- Watch for Planalto headings between articles (e.g. next-article headings such as “Exclusão de ilicitude”) being captured by naive “until next Art.” extraction; remove those if they do not belong to the cited article.
- Do **not** add emojis, decorative labels, horizontal rules, bold/italic styling, quotation marks, or explanatory titles unless the user asks.
- The only default styling should be smaller font size so it feels subordinate to the answer explanation; Higor settled on `font-size: 65%` after testing 85% and 50%.
- Use simple HTML such as:

```html
<br><br><div style="font-size: 65%;">Coação irresistível e obediência hierárquica<br>Art. 22 - Se o fato é cometido sob coação irresistível ou em estrita obediência a ordem, não manifestamente ilegal, de superior hierárquico, só é punível o autor da coação ou da ordem.</div>
```

## Bulk enrichment workflow

For large batches of legal/accounting study cards:

1. Create a full timestamped backup before the bulk run, plus extra backups before corrective re-runs.
2. Map candidate notes by deck/tag and count them before writing.
3. Fetch/update a local cache of official source pages and include the consultation date in the inserted block.
4. Add idempotent HTML markers (for example `HERMES_LEGAL_SOURCE_V1`, `HERMES_CPC_SOURCE_V1`, `HERMES_TERMS_SOURCE_V1`) so re-runs remove/replace previous generated blocks instead of duplicating them.
5. For long statutes/articles, **do not paste the whole law or whole long article**. Higor strongly prefers lei seca blocks to contain only the legal wording that is coherent with the flashcard, even if that means using bracketed ellipses inside a caput.
   - if the card cites a paragraph/inciso/alínea, include only the minimum caput/context needed plus that specific device;
   - if the caput is long and only one clause matters, quote just that clause with `[...]` rather than the whole caput;
   - if the card depends only on the caput, include only the relevant caput portion;
   - only include a full article when it is genuinely short;
   - remove unrelated nearby articles accidentally captured by proximity (e.g. CF art. 2 when the intended citation is Lei 8.987 art. 2º).
6. Before reporting done, inspect oversized generated legal blocks (rough rule: >1,800–2,200 visible chars) and trim them manually/curated if needed.
7. For Português/Gramática cards, when enriching existing notes that use technical labels without explanation (for example complemento nominal, oração subordinada completiva nominal, sujeito paciente, se apassivador, índice de indeterminação do sujeito, regência, crase, próclise, conectivo concessivo/causal/conclusivo), append a compact `HERMES_TERMS_SOURCE_V1` microgloss at the end of the answer side. Use the same subordinate style as legal-source blocks: `font-size: 65%`, one very short definition per term, and no mini-aula. Remove/replace an existing terms block instead of duplicating it.
8. For Contabilidade/Auditoria cards, when the answer cites a CPC item, prefer adding the **actual pertinent CPC wording** from official CPC PDFs/pages, not only a keyword/terms block. Keep it scoped to the cited item(s) and the assertion in the card; do not paste whole pronouncements. Use `HERMES_CPC_SOURCE_V1` for generated CPC wording. If a previous `HERMES_TERMS_SOURCE_V1` terms-only block is present and the user asked for CPC wording, replace that terms block with the CPC wording rather than appending both.
9. Before bulk edits, if prior runs have been unstable or the user is frustrated, do a one-card proof edit first: backup, edit one representative note, reopen read-only, report tag/deck/exact inserted text/integrity. After proof succeeds, run the rest as a single idempotent script with minimal chatter.
10. Produce a report: target count, changed count, per-deck counts, official URLs, skipped items/reasons, backup paths, and `integrity_check`.

## Maintenance and GitHub Sync

When this skill changes for Higor, also keep the public backup repository in sync: `higorzz/hermes-anki-estudos-skills`, local checkout `/home/higor/hermes-anki-estudos-skills`. Copy the updated skill directory into `skills/productivity/anki-card-editing/`, commit with the GitHub noreply email `15950097+higorzz@users.noreply.github.com`, and push. This is a maintenance step for the skill library; do not mix it with Anki database writes unless the user asked for both.

## Verification response

After editing, report concisely:
- what card/note was edited;
- the exact tag(s), so the user can locate it in Anki;
- the exact HTML/text inserted if relevant;
- backup path;
- persistence checks (`HAS OLD HEADER`, `HAS LITERAL`, `integrity_check`, etc.).

## Interaction style for Higor during Anki edits

Higor strongly dislikes repeated mid-task narration when an Anki bulk edit is already underway. Do not pause after every diagnostic step with status prose. Instead:
- if the user asks to proceed, keep working through backup → edit → verify → final report;
- only interrupt for a real blocker (Anki/process lock, non-empty WAL, failed integrity check, missing official source, or a decision that changes the write scope);
- if prior attempts got stuck, switch to a simpler workflow: one-card test edit first, then one deterministic bulk script.

## References

- `references/anki-sqlite-single-card-edit.md` — concrete notes from a successful one-card legal-article edit, including WAL/checkpoint pitfall.
- `references/bulk-legal-accounting-enrichment.md` — bulk enrichment pattern for Higor's legal/accounting cards, official source URLs, idempotent markers, and formatting preferences.
- `references/legal-block-trimming-pitfalls.md` — Higor's correction that long laws/articles must be trimmed to the exact supporting part, plus pitfalls from Planalto extraction and oversized-block verification.
- `references/cpc-accounting-wording.md` — CPC/Contabilidade enrichment pattern: replace terms-only blocks with concise official CPC item wording from CPC PDFs, using `HERMES_CPC_SOURCE_V1`.
- `references/anki-colpkg-backup.md` — create an importable `.colpkg` backup from WSL using a SQLite snapshot, zstd-compressed collection/media map, and full media payloads; includes verification steps.
- `references/sync-diagnostics-direct-sqlite-edits.md` — read-only diagnostic workflow for large/repeated sync counts after Hermes direct SQLite edits, including `usn=-1`, WAL/SHM, integrity, and large `usn` cluster interpretation.