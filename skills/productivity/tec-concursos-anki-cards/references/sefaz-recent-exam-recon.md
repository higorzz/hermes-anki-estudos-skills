# SEFAZ recent-exam reconnaissance

Use this reference when Higor asks for a scan of recent SEFAZ/SEFA auditor/fiscal concursos, especially to compare bancas, gabaritos, provas applied, and assunto patterns before generating study material.

## Workflow

1. **Clarify the universe by action, not by edital**
   - If Higor asks for “últimas SEFAZ” in an exam-analysis context, default to **provas já aplicadas**, not merely edital publicado.
   - Explicitly label exceptions: edital publicado but not applied; provas applied but certame suspended; homologated/finalized.
   - If the user asks “todos já aconteceram?”, correct the list by status rather than defending the original ordering.

2. **Build a compact candidate list first**
   - Candidate fields: órgão/concurso, cargo, year, banca, prova date/status, source URL.
   - For fiscal state roles include SEFA/SEFAZ names and equivalent fiscal titles when appropriate: Auditor Fiscal, Auditor Fiscal de Receitas, Fiscal de Tributos, Auditor Fiscal do Tesouro, etc.
   - When a role is not exactly named “Auditor Fiscal”, flag it briefly instead of silently mixing it.

3. **Prefer pages that expose proof artifacts**
   - Banca pages when reachable: FCC, Cebraspe, Fadesp, FGV, etc.
   - Reliable aggregator pages can be used for quick reconnaissance when they link or quote gabarito/prova details, especially Estratégia pages such as:
     - gabarito extraoficial pages;
     - “saíram os gabaritos” pages;
     - main concurso pages with status/banca/prova date/discipline matrices.
   - Keep source caveats concise: “gabarito/extraoficial localizado”, “página oficial com consulta individual”, “usei matriz do edital porque não achei gabarito público detalhado”.

4. **Extract subject patterns from the actual question/gabarito page when possible**
   - For HTML tables, parse tables rather than reading visually; they often contain question number, answer, and truncated enunciado.
   - Use question stems to infer topic clusters: e.g. “COBIT 2019”, “CAP”, “IPVA RJ”, “suspensão do crédito tributário”.
   - If only the discipline matrix is available, say so and summarize by matrix rather than pretending a question-level review.

5. **Output format Higor prefers for this class**
   - Start with a compact list/table of the concursos and bancas.
   - Then provide summaries by concurso with bullet groups per matéria.
   - Be direct and mark uncertainty/limitations inline.
   - Avoid long methodological explanation unless asked.

## Pitfalls

- **Do not rank “latest” only by edital year** if the question is about exams that have happened. Use prova applied / result / homologation status.
- **Do not drop a concurso just because it is suspended** if Higor explicitly says to keep it; instead annotate the status.
- **Do not overstate a gabarito source**. Distinguish official gabarito, preliminary gabarito, extraoficial correction, and edital matrix.
- **Do not make Anki cards automatically from this reconnaissance.** If Higor later asks for cards, return to the normal approval-before-insert workflow.
