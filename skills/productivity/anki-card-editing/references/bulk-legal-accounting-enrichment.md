# Bulk Anki legal/accounting enrichment notes

Use this as a pattern when Higor asks to enrich many Anki study cards with legal article text or accounting/auditing support terms.

## Source policy

- For Brazilian federal legal text, use official authority sites, primarily compiled Planalto/Presidência pages:
  - Constituição: `https://www.planalto.gov.br/ccivil_03/constituicao/constituicao.htm`
  - Código Penal: `https://www.planalto.gov.br/ccivil_03/decreto-lei/del2848compilado.htm`
  - Código Civil: `https://www.planalto.gov.br/ccivil_03/leis/2002/l10406compilada.htm`
  - CDC: `https://www.planalto.gov.br/ccivil_03/leis/l8078compilado.htm`
  - CPC: `https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2015/lei/l13105.htm`
  - Lei 14.133/2021: `https://www.planalto.gov.br/ccivil_03/_ato2019-2022/2021/lei/l14133.htm`
  - Lei 8.987/1995: `https://www.planalto.gov.br/ccivil_03/leis/l8987compilada.htm`
  - Lei 8.429/1992: `https://www.planalto.gov.br/ccivil_03/leis/l8429.htm`
  - Lei 11.417/2006: `https://www.planalto.gov.br/ccivil_03/_ato2004-2006/2006/lei/l11417.htm`
  - Lei 6.404/1976: `https://www.planalto.gov.br/ccivil_03/leis/l6404consol.htm`
- Fetch with a normal browser user-agent; Planalto may time out with some curl defaults but Python `urllib.request.Request(..., headers={'User-Agent':'Mozilla/5.0'})` worked.
- If the cited basis is jurisprudential (e.g. STF Tema), do not insert unverified text; report it as skipped unless an official court source is successfully read.

## HTML conventions

- Default legal/source block: `font-size: 65%; text-align: left;`.
- Avoid emojis, decorative headers, horizontal rules, italics/bold except small source labels if needed in bulk reports.
- Add explicit source/date line: `Fonte oficial: Planalto/Presidência da República. Consulta em DD/MM/AAAA.`
- Use comments as idempotency markers:
  - `<!-- HERMES_LEGAL_SOURCE_V1 --> ... <!-- /HERMES_LEGAL_SOURCE_V1 -->`
  - `<!-- HERMES_TERMS_SOURCE_V1 --> ... <!-- /HERMES_TERMS_SOURCE_V1 -->`

## Implementation pitfalls

- Remove previous generated blocks before re-running; otherwise cards accumulate duplicate law text.
- Remove any one-off test block that was added before the bulk marker if it duplicates the official section.
- For long statutes/Constitution articles, keep caput + the cited paragraph/inciso/alínea, but continue wrapped lines until the next marker so dispositive text is not cut off.
- Reopen the SQLite DB read-only after writes and count markers; do not rely only on the first connection.
