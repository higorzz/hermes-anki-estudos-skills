# Legal block trimming pitfalls from Higor's Anki edits

When enriching Anki legal cards with official-law text, Higor prefers concise statutory support, not full laws or long articles.

## Formatting preference

- Keep `font-size: 65%` for appended legal text.
- Compact Planalto line wraps: only break lines at real legal units (article, paragraph, inciso, alínea, or a true heading).
- Do not add emojis, horizontal rules, decorative titles, or bold/italic styling beyond minimal source/title labels if already used by the generated block.

## Scope preference

Do **not** paste the whole law or a whole long article just because it was fetched from Planalto.

Use this rule:

- Small article: OK to include in full.
- Long article: include only the part that explains the card.
- If the card cites `§`, inciso, or alínea: include minimal caput/context plus that specific device.
- If the explanation is about only one concept in the caput: include only the caput.
- If multiple official sources are cited, include each only if it truly supports the explanation.

## Pitfalls seen

- Naive citation extraction can confuse nearby numbers and add unrelated law text, e.g. CF art. 2 when the relevant citation was Lei 8.987/1995 art. 2º, IV.
- Naive `until next Art.` extraction from Planalto can capture headings belonging to the next article/topic, e.g. Código Penal art. 22 followed by “Exclusão de ilicitude”. Remove those trailing headings unless they belong to the cited device.
- Article 128 CF, Article 37 CF, Article 103-A CF, Article 180 CP, Article 50 CC, and Lei 14.133 long articles can become too tall if pasted whole. Prefer curated caput + relevant paragraph/inciso.

## Verification checklist

After a bulk enrichment/correction run:

1. Count legal markers (`HERMES_LEGAL_SOURCE_V1`) and term markers (`HERMES_TERMS_SOURCE_V1`).
2. Inspect generated legal blocks longer than ~1,800–2,200 visible characters.
3. Check samples from each deck, especially long-source cards.
4. Confirm `pragma integrity_check = ok`.
5. Report backup path and any cards skipped or manually trimmed.
