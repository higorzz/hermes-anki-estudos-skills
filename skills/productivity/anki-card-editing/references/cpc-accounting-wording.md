# CPC wording enrichment for Anki accounting cards

Use this when Higor asks to add CPC support to Contabilidade/Auditoria Anki cards.

## Lesson from session

A prior terms-only block (`HERMES_TERMS_SOURCE_V1`) was not enough: Higor wanted the actual CPC wording that supports the card. Replace the terms block with an official wording block when the answer already cites a CPC item.

## Source pattern

- Use official Comitê de Pronunciamentos Contábeis (CPC) pages/PDFs.
- Main listing: `https://www.cpc.org.br/CPC/Documentos-Emitidos/Pronunciamentos`
- Pronouncement detail pages use IDs, e.g. observed IDs:
  - CPC 07: `/CPC/Documentos-Emitidos/Pronunciamentos/Pronunciamento?Id=38`
  - CPC 23: `...?Id=54`
  - CPC 26: `...?Id=57`
  - CPC 27: `...?Id=58`
  - CPC 46: `...?Id=78`
- Pick the main pronouncement PDF, not termo de aprovação, sumário, or relatório de audiência.
- Extract text from PDFs with Python/PyMuPDF (`fitz`) if `pdftotext` is unavailable.

## HTML marker and style

Use a separate marker for CPC text:

```html
<!-- HERMES_CPC_SOURCE_V1 --><div style="font-size: 65%; text-align: left;"><div><b>CPC 46, item 26</b><br>26. Os custos de transação não incluem custos de transporte...</div><br><div>Fonte oficial: Comitê de Pronunciamentos Contábeis (CPC). Consulta em DD/MM/AAAA.</div></div><!-- /HERMES_CPC_SOURCE_V1 -->
```

If `HERMES_TERMS_SOURCE_V1` exists and the user asked for CPC wording, replace the terms block instead of appending a second support block.

## Scope rules

- Add only the cited item(s) or the minimal portion directly supporting the card.
- Do not paste whole pronouncements.
- For cards citing multiple CPC items, include only the portions tied to the answer, e.g. CPC 46 items 9 and 24 for “valor justo = preço de saída em transação não forçada”.
- Keep source text literal, but compact PDF line wraps.

## Single-card proof pattern

When the user asks if you can edit one Anki card or prior bulk attempts were unstable:
1. Create a backup next to `collection.anki2`.
2. Edit one representative note only.
3. Reopen the DB read-only and verify marker, stale block removal, exact inserted wording, tag/deck, and `pragma integrity_check`.
4. Report the exact tag so Higor can locate it in Anki.

## Example verified card

For tag `Contabilidade_Geral::CPC_46::Mensuracao_Valor_Justo`, card about transport costs in fair value measurement, insert CPC 46 item 26:

> 26. Os custos de transação não incluem custos de transporte. Se a localização for uma característica do ativo (como pode ser o caso para, por exemplo, uma commodity), o preço no mercado principal (ou mais vantajoso) deve ser ajustado para refletir os custos, se houver, que seriam incorridos para transportar o ativo de seu local atual para esse mercado.
