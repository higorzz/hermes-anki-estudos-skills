---
name: tec-concursos-anki-cards
description: Use when Higor sends prints, OCR text, copied page content, alternatives, gabaritos, explanations, laws, edital excerpts, study materials, CPC items, or other exam-study sources and wants objective Anki flashcards for fiscal-area study. Drafts short Cebraspe-style Certo/Errado or cloze cards for approval, then inserts approved cards into Anki with standardized tags and concise official source blocks when needed.
version: 1.0.0
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [anki, concursos, tec-concursos, flashcards, direito, contabilidade, fiscal]
    related_skills: [anki-card-editing]
---

# Study Sources → Anki Cards

## Overview

Use this skill to transform Tec Concursos screenshots, OCR, copied question pages, alternatives, answer keys, explanations, specific laws, edital excerpts, PDF/material de estudo passages, CPC items, accounting/auditing standards, and other exam-study sources into concise Anki flashcards for Higor's fiscal-area study.

The goal is **not** to summarize the whole subject. The goal is to identify the exact gap revealed by the question, error, guess, answer key, or explanation and turn that point into active recall.

Treat every legible detail from the user's source as authoritative. Do not challenge, correct, or contradict the gabarito, professor's comment, or explanation shown in the provided material. If something seems counterintuitive, still build the card from it.

## When to Use

Use when Higor sends or asks about:

- Tec Concursos prints or copied page content;
- alternatives, gabarito, comments, professor explanation, or student explanation from a question;
- specific laws, articles, Constituição, CTN, leis tributárias, lei seca, jurisprudência, súmulas, edital excerpts, PDF/material de estudo, aula, resumo, CPC/CFC/CVM/Receita/Tesouro/STN material, or other exam-study sources;
- a wrong answer, guessed correct answer, doubt, new exception, prazo, competência, requisito, conceito, classificação, consequence, formula, or interpretation point;
- a request like "gera cards", "faz flashcards", "transforma em Anki", "estilo Cebraspe", "card desse comentário", "card dessa lei", "card desse edital", or "card desse material".

Do **not** use for:

- bulk editing or correcting existing Anki cards unrelated to a new Tec Concursos source; use `anki-card-editing` for that;
- long thematic summaries unless the user explicitly asks;
- inventing cards from general knowledge not present in the source.

## Approval-Then-Insert Workflow

This skill is not only for drafting cards. Higor wants Hermes to **show the generated cards first for approval** and, after approval, **insert them directly into Anki**.

Default workflow:

1. Generate the cards from the Tec Concursos source and show them in the normal output format, including `TEMA`, `TAG`, front, back, and final report.
2. Wait for Higor's approval or requested edits. Do not write to Anki before approval.
3. After approval, insert the approved cards into Anki using the local Anki workflow. Load/follow `anki-card-editing` for safety and database handling.
4. Verify persistence after insertion and report concisely what was added.

Approval phrases such as "aprovado", "pode adicionar", "coloca no Anki", "manda pro Anki", "ok", or equivalent mean: proceed to Anki insertion using the last approved card set, unless Higor changed the content.

If Higor asks for edits before approval, revise the cards and ask for approval again. The reviewed card text is the source of truth for insertion.

## Source Handling Beyond Tec Concursos

The source can be a question platform, a statute, a specific article, an edital, a PDF, copied study material, a teacher note, a CPC item, Higor's own pasted notes, or a request to research recent fiscal concursos/provas before deciding what to study.

For SEFAZ/SEFA recent-exam reconnaissance — e.g. “últimas SEFAZ”, “quais já tiveram prova aplicada”, “pega o gabarito e resume os assuntos cobrados” — use `references/sefaz-recent-exam-recon.md`. Key rule: distinguish edital publicado from prova aplicada/homologada, annotate banca/status, and summarize assunto patterns by matéria without automatically creating Anki cards.

Apply these source rules:

1. If the source is a solved question, prioritize the explanation/gabarito and the exact tested gap.
2. If the source is lei seca or CPC/standard text, create cards from the normative content itself: conditions, requirements, exceptions, deadlines, competences, consequences, definitions, formulas, or enumerations.
3. If the source is an edital, create cards only for examinable requirements, structure, deadlines, objective rules, program content distinctions, or constraints that Higor may need to recall; do not turn the whole edital into generic cards.
4. If the source is material de estudo, professor notes, or a PDF, preserve the material's claim as authoritative unless Higor asks for fact-checking.
5. If Higor asks from a named law/CPC without pasting the text, retrieve the official text when tools are available before drafting. Do not invent wording from memory.
6. For large sources, select only high-yield, autonomous points and avoid card spam.

## User Context and Lens

Higor studies for fiscal-area concursos: Auditor Fiscal, Analista Tributário, Fiscal de Tributos, Auditor de Controle, and related roles.

Common disciplines include Direito Tributário, Legislação Tributária, Direito Administrativo, Constitucional, Contabilidade, Auditoria, AFO, Finanças Públicas, Economia, Estatística, RLM, Matemática Financeira, TI, Português, Civil, Empresarial, and Penal when included in the edital.

Even when the source is legal, frame the point by fiscal-exam logic, not by judicial-career depth. Prefer what avoids a future mistake in an objective exam.

## Core Quantity Rule

Default: **1 relevant point = 1 flashcard**, but keep batches lean. Higor prefers fewer cards focused on what is most likely to be cobrando/recorrente, rather than exhaustive coverage of every exception.

When analyzing aulas/PDFs, first map what appears most in the theory and questions, then draft a compact batch from those high-yield patterns. Avoid low-frequency edge cases unless the material itself emphasizes them or Higor asks for them.

Create cards only for autonomous, useful points actually present in the source.

Quantity rules:

1. If the user marked an incorrect option and both the wrong reasoning and the correct answer are identifiable, create **2 cards**: one targeting the mistaken/wrong alternative and one targeting the correct point.
2. If there is only one central information point, create **1 card**.
3. If the explanation contains several independent and important points, create more than one card.
4. If the user asks for more cards about alternatives, then expand into alternatives, but still avoid redundancy.
5. Do not automatically make one card per alternative in multiple-choice questions; do so only when each alternative contains an autonomous relevant point.
6. Do not create cards about facts absent from the print/text.
7. Do not create redundant cards or thematic filler.

Completion criterion: every card should map to one explicit point in the source and the final report should explain the quantity in one sentence.

## Source Priority

When there is more than one source in the material, prioritize:

1. Professor/exam explanation;
2. official gabarito/comment;
3. question text and alternatives;
4. student's comment;
5. user's own note or marking.

If an alternative and a professor comment conflict, follow the professor/comment/gabarito as truth for card generation.

## Card Types

### Certo/Errado cards — default

Prioritize Certo/Errado cards even when the source, banca, or inspiration is multiple choice. For Higor, the default difficulty mix should weight **CEBRASPE, FGV, and FCC equally**: CEBRASPE-style objective assertions and subtle inversions; FGV-style fine conceptual distinctions/exceptions/consequences; FCC-style lei seca precision, deadlines, requirements, and “salvo se” wording. Even when a card is based on FGV or FCC patterns, the output format remains **Certo/Errado**, unless Higor explicitly asks for another format.

Rules:
Rules:

1. Each card tests one main idea.
2. The front is a short, objective exam-style assertion.
3. When it improves recall, especially in Portuguese/grammar cards or rules that are easier to recognize in context, put a **short concrete example directly in the front** and make the assertion about that example. Higor explicitly prefers examples on the front to emphasize the tested rule when it makes sense. Example pattern: `Em “houve problemas”, ...`; avoid long examples that turn the front into a mini-aula.
   - For **Português** cards, the front must contain enough context to stand alone in Anki. Do not use a bare fragment if the rule depends on interpretation, regência, pontuação, coesão, reescrita, or semantic value. Prefer a short full sentence/period plus the assertion, e.g. `No período “...”, ...`. Higor explicitly corrected that Portuguese cards need more context so the card makes sense during review.
4. The back has the gabarito and a brief justification.
5. Avoid open prompts like "conceitue", "explique" or "quais são".
6. Do not spoil the exact tested trick in `TEMA`; use broad subject labels.
7. Difficulty should come from precision, not convoluted wording.
8. Keep front and back short.
9. Do not invent exceptions, deadlines, case law, article numbers, formulas, or requirements.

### Errado cards

When writing an Errado item, make the error subtle, plausible, and technical — the kind of statement a prepared-but-imprecise candidate could believe.

Good distortions:

- swapping prazo, number, percentage, fraction, formula component, or threshold for a plausible one;
- inverting rule and exception;
- swapping competence between similar bodies/entities;
- adding or removing a discreet requirement;
- inverting paired concepts;
- changing subject, object, condition, consequence, or scope;
- generalizing a specific hypothesis or restricting a general rule.

Avoid absurd errors and giveaway words such as "sempre", "nunca", "jamais", "em qualquer hipótese", "absolutamente", "exclusivamente", "obrigatoriamente" unless the source itself requires them.

Before finalizing an Errado card, mentally validate: **a candidate who does not dominate this point could think it is true?** If not, rewrite it.

### Cloze / omissão de palavras

Use cloze/omissão de palavras **com mais frequência quando for pertinente**, especially when the learning target is better recalled by completing a precise term, relation, or formula than by judging a C/E assertion. Higor has explicitly corrected that I had been using cloze too little; do not default mechanically to Certo/Errado when cloze would be more efficient.

Prefer cloze for:

- formulas, equations, symbols, and calculation patterns;
- paired distinctions and contrastive concepts (for example, “por fora” vs. “por dentro”, “linear” vs. “exponencial”);
- lists, roles, ordem, requisitos cumulativos;
- deadlines, percentages, thresholds, and exact numbers;
- exact enumerations;
- short normative wording where the missing term matters;
- technical vocabulary where the exam trick is the missing word itself.

Do not use cloze just to vary format: use it because the missing element is what Higor needs to retrieve. For batches in Matemática Financeira, Estatística, RLM, Economia, Contabilidade, TI, and other formula/list-heavy subjects, actively consider a cloze-heavy batch before choosing C/E.

## Certo/Errado Proportion

For batches with several cards, balance Certo and Errado around 50/50 without sacrificing quality. Higor has explicitly corrected that batches with noticeably more Certo than Errado are not varied enough; for even-sized thematic batches, default to an exact 50/50 split unless the source itself makes that impossible.

- With 1 card: choose Certo or Errado based on what best memorizes the point.
- With 2 cards: prefer 1 Certo and 1 Errado if it makes sense.
- With large even batches not tied to a fixed gabarito (for example “3 cards per recurring theme”), plan the polarity count before drafting and end with an equal split, e.g. 15 Certo / 15 Errado for 30 cards. This is not optional for Higor: he corrected a 24/16 TI batch and asked for 50/50, so verify the count before presenting.
- With odd batches, keep the difference at most 1 unless the source requires otherwise.
- Never force an Errado card if it would require an invented or silly distortion; instead revise another card where a plausible technical inversion exists.
- In the final report, include the C/E count and quickly verify it matches the planned balance before presenting.

## Legal, Accounting, and Official Support

For legal, accounting, auditing, tax, constitutional, administrative, and related cards, add a concise supporting citation when feasible and useful.

Rules:

1. Prefer official Brazilian sources for current wording: Planalto/Presidência for federal statutes and Constitution; official CPC/CFC/CVM/Receita/Tesouro/STN sources as applicable.
2. If the supporting article/device is visible in the source, cite it directly.
3. If the source names the law/article but not the text, you may look up the official article before finalizing if a web/search tool is available.
4. Keep citation subordinate and concise. Do not paste long law excerpts unless the card depends on exact wording.
5. In jurisprudence, test the thesis, condition, exception, or consequence; do not focus on judgment number unless indispensable.
6. In accounting/math/statistics/finance, prefer concept, formula, condition of application, common error, or interpretation of result.

Back-side pattern for support:

`GABARITO: ✅ Certo — ... **trecho decisivo** ... (Base: art. X da Lei Y, quando aplicável).`

If no reliable support can be retrieved without overreaching, omit the citation rather than guessing.

## Official Source Blocks and Short Term Glosses on the Back Side

For law/CPC/accounting/legal cards, the Anki version should include the relevant lei seca/CPC/source wording at the **end of the verso** in a small block, following Higor's established Anki convention.

For **Português/Gramática** cards, when the card uses a technical term that may be the learning bottleneck — e.g. complemento nominal, oração subordinada completiva nominal, adjunto adnominal, sujeito paciente, se apassivador, índice de indeterminação do sujeito, regência, crase, próclise, conectivo concessivo/causal/conclusivo etc. — add a tiny explanatory gloss at the end of the `VERSO`, in the same spirit as lei seca blocks: subordinate, compact, and not a mini-aula. The gloss should explain only the term needed for that card, preferably in 1 sentence or 1 short line per term.

Use this only in the card that will be inserted into Anki, and show it in the approval draft when feasible so Higor can approve the exact final content.

### Portuguese term gloss block

Use `HERMES_TERMS_SOURCE_V1` for compact grammar/Português definitions. This is not an official-source quote; it is a microgloss to prevent Higor from having to remember the meaning of the technical label before answering the actual card.

HTML pattern:

```html
<br><br><!-- HERMES_TERMS_SOURCE_V1 --><div style="font-size: 65%; text-align: left;"><b>Termo(s):</b><br><b>[termo]</b>: [definição mínima].<br><b>[termo 2]</b>: [definição mínima, se indispensável].</div><!-- /HERMES_TERMS_SOURCE_V1 -->
```

Rules:

1. Add only terms that appear in the card or are essential to understand the answer.
2. Keep each definition very short; default to one line per term.
3. Do not turn the gloss into a grammar lesson, list of exceptions, or source block.
4. If editing/reinserting a card, remove/replace any old `HERMES_TERMS_SOURCE_V1` block instead of duplicating it.
5. For Português cards, favor these term glosses when the verso names a technical classification without defining it.

### Legal source block

Use official Planalto/Presidência or another official source. Keep only the minimum portion that supports the card: caput, parágrafo, inciso, alínea, or short article. Do not paste a whole long statute or unrelated nearby provisions.

HTML pattern:

```html
<br><br><!-- HERMES_LEGAL_SOURCE_V1 --><div style="font-size: 65%; text-align: left;"><div><b>[Norma, dispositivo]</b><br>[texto literal oficial mínimo que sustenta o card]</div><br><div>Fonte oficial: [órgão/site]. Consulta em DD/MM/AAAA.</div></div><!-- /HERMES_LEGAL_SOURCE_V1 -->
```

### CPC/accounting source block

Use official CPC/CFC/CVM/Receita/Tesouro/STN sources as applicable. For CPC, prefer the official CPC pronouncement PDF/page and include only the cited item(s) or minimal supporting passage.

HTML pattern:

```html
<br><br><!-- HERMES_CPC_SOURCE_V1 --><div style="font-size: 65%; text-align: left;"><div><b>[CPC/Norma, item]</b><br>[texto literal oficial mínimo que sustenta o card]</div><br><div>Fonte oficial: Comitê de Pronunciamentos Contábeis (CPC). Consulta em DD/MM/AAAA.</div></div><!-- /HERMES_CPC_SOURCE_V1 -->
```

### NBC/CFC auditing source block

For Auditoria cards based on NBC TA/NBC PA/NBC TI/NBC TO or other CFC auditing standards, include the concise CFC/NBC wording at the end of the verso by default when the card tests literal wording, definitions, enumerations, exceptions, prohibitions, documentation requirements, or report wording. Auditoria questions often hinge on exact NBC wording, so treat these like Direito/Contabilidade source blocks: retrieve/quote the applicable item when feasible and avoid relying only on paraphrase.

Use only the exact item/subitem needed for the card; do not paste a whole NBC or long unrelated application guidance. If one NBC item supports several cards, each card may receive the same small block or a narrower excerpt from that item.

HTML pattern:

```html
<br><br><!-- HERMES_NBC_SOURCE_V1 --><div style="font-size: 65%; text-align: left;"><div><b>[NBC TA/NBC PA/NBC TI, item]</b><br>[texto literal oficial mínimo que sustenta o card]</div><br><div>Fonte oficial: Conselho Federal de Contabilidade (CFC). Consulta em DD/MM/AAAA.</div></div><!-- /HERMES_NBC_SOURCE_V1 -->
```

Rules:

1. The source block goes at the final end of `VERSO`, after the concise answer/justification.
2. Use `font-size: 65%` exactly.
3. Keep it literal but compact line wraps.
4. Do not add emojis, decorative separators, or long explanations inside the source block.
5. If the source text is long, trim to the exact supporting legal/accounting unit.
6. If no official source can be read, say that in the approval draft and do not fabricate the block.
7. When editing/reinserting an existing generated support block, remove/replace old `HERMES_LEGAL_SOURCE_V1`, `HERMES_CPC_SOURCE_V1`, `HERMES_NBC_SOURCE_V1`, or `HERMES_TERMS_SOURCE_V1` blocks rather than duplicating them.
8. For Auditoria cards, prefer adding `HERMES_NBC_SOURCE_V1` blocks with the literal NBC item whenever the issue is supported by NBC TA/NBC PA/NBC TI wording; omit only when the point is purely doctrinal or no reliable item can be identified.
9. This is mandatory at insertion time for Direito, Legislação Tributária, Contabilidade/CPC, Auditoria/NBC, and correlated official-standard cards: the write/insert script must verify that every inserted note has the expected `HERMES_*_SOURCE_V1` block, unless the approval draft explicitly marked that no reliable official source was available. Do not report insertion success if any target note is missing its source marker.

## Tags and Themes

Every card must include a standardized `TAG:` line in the approval draft and must be inserted into Anki with the same tag(s). Tags are part of the deliverable, not optional metadata.

### Tag format

Use lowercase, ASCII, no spaces, separated by `::`.

Required format:

`materia::assunto_amplo::subassunto`

Always use exactly 3 hierarchy levels. Do **not** create 2-level tags such as `economia::politica_monetaria`, because Higor wants the Anki tag dropdown to expose a selectable subassunto level. If the subtopic is genuinely broad, use a stable third level such as `geral` or `instrumentos`, but prefer a meaningful subassunto.

Rules:

1. Keep the same tag for the same correlated theme across sessions.
2. Do not create hyper-specific one-off tags for tiny variations.
3. Prefer stable fiscal-exam taxonomy over the wording of the material.
4. Use singular/plural consistently by common subject name; do not alternate synonyms.
5. If a card fits multiple subjects, use the primary subject tag and optionally one secondary tag only when it materially helps retrieval.
6. `TEMA` is human-facing and can be uppercase with accents; `TAG` is Anki-facing and must stay normalized.
7. Do **not** insert `TEMA` at the beginning of the Anki front by default; it can spoil the retrieval cue. Use the front for the assertive prompt only, and rely on deck + tags for organization. Keep `TEMA` in the approval draft unless Higor explicitly asks to embed it in the card.
8. During Anki insertion, verify the tags were actually written to the note.

### Theme format

Use broad, non-spoiling theme labels:

`TEMA: DISCIPLINA — ASSUNTO AMPLO`

If needed:

`TEMA: DISCIPLINA — ASSUNTO AMPLO — SUBASSUNTO`

Do not make `TEMA` reveal the trick of the card.

### Canonical tag examples

The current approved Anki standard for Área Fiscal is lowercase ASCII with **exactly 3 `::` levels**. Existing Área Fiscal tags were normalized to this style; keep future cards consistent with these shapes.

- `direito_administrativo::poder_de_policia`
- `direito_administrativo::atos_administrativos::atributos`
- `direito_administrativo::licitacoes::dispensa`
- `direito_administrativo::licitacoes::registro_de_precos`
- `direito_administrativo::servicos_publicos::permissao`
- `direito_constitucional::direitos_fundamentais`
- `direito_constitucional::organizacao_do_estado::competencias`
- `direito_constitucional::stf::sumula_vinculante`
- `direito_constitucional::ministerio_publico`
- `direito_tributario::competencia_tributaria`
- `direito_tributario::credito_tributario::suspensao`
- `direito_tributario::obrigacao_tributaria`
- `legislacao_tributaria::icms`
- `legislacao_tributaria::iss`
- `contabilidade::cpc_23::politicas_estimativas_erros`
- `contabilidade::cpc_46::mensuracao_valor_justo`
- `contabilidade::ativos::depreciacao`
- `contabilidade::demonstracoes_contabeis`
- `auditoria::evidencia_de_auditoria`
- `afo::orcamento_publico::principios`
- `afo::receita_publica`
- `economia::politica_fiscal::instrumentos`
- `economia::politica_fiscal::estabilizadores_automaticos`
- `economia::politica_monetaria::instrumentos_classicos`
- `economia::politica_monetaria::instrumentos_nao_convencionais`
- `economia::moeda::agregados_monetarios`
- `economia::moeda::papel_moeda`
- `economia::moeda::base_monetaria`
- `economia::moeda::multiplicador_monetario`
- `economia::moeda::demanda_por_moeda`
- `matematica_financeira::juros_compostos`
- `estatistica::probabilidade`
- `rlm::logica_proposicional`
- `ti::itil4::principios_orientadores`
- `ti::seguranca_da_informacao`
- `portugues::sintaxe::concordancia`

When in doubt, choose the closest broad canonical tag and keep it stable. If a new recurring theme appears, create a normalized tag once and reuse it. Avoid using a statute number itself as the subassunto when it is the default statute for the whole assunto (for example, in licitações under Direito Administrativo, prefer `dispensa`, `inexigibilidade`, `registro_de_precos`, etc., rather than `lei_14133`).

## Output Format

During the approval stage, return only cards plus the final report and a short approval line. Do not add a long introduction. Do not say "claro" or "aqui estão".

After the cards, include this concise line:

```text
APROVAÇÃO: Se estiver ok, responda “aprovado” que eu adiciono esses cards diretamente no Anki.
```

Do not include this approval line when Higor only asked to preview without insertion, or after the cards have already been inserted.

For Certo/Errado cards:

```text
CARD 1

TEMA: [DISCIPLINA — ASSUNTO AMPLO]
TAG: [materia::assunto_amplo]

FRENTE:
[assertiva objetiva de Certo ou Errado]

VERSO:
GABARITO: [✅ Certo / ❌ Errado] — [justificativa curta, com o trecho decisivo em **negrito**]

────────────────────────
```

For cloze cards:

```text
CARD X

TIPO: Omissão de palavras
TEMA: [DISCIPLINA — ASSUNTO AMPLO]
TAG: [materia::assunto_amplo]

TEXTO:
[frase com {{c1::lacuna}}]

VERSO/OBS:
[explicação breve, se necessário]

────────────────────────
```

Final report:

```text
TOTAL DE FLASHCARDS: [número]
C/E: [x] Certo / [y] Errado
OMISSÃO DE PALAVRAS: [número]
CRITÉRIO USADO: [1 frase dizendo por que criou essa quantidade]
```

## Response Discipline

- Be direct.
- Always include the subject in the first card.
- Do not explain the method.
- Do not ask generic final questions.
- Do not create extra cards "por cautela".
- If the source is too blurry/illegible, create cards only from legible content and state the limitation briefly in the final criterion.
- If the user sends multiple images/pages, process all legible unique points and deduplicate repeated content.
- If metadata/context is missing in legal evidence or a factual-procedural item, mention only if it blocks card accuracy.

## Anki Insertion After Approval

When Higor approves generated cards, insert them into Anki rather than merely telling him to copy them.

Use `anki-card-editing` for the concrete database safety workflow, including:

1. Locate the Anki profile/collection, commonly under `/mnt/c/Users/<user>/AppData/Roaming/Anki2/<profile>/collection.anki2` in WSL.
2. Check whether Anki is running before writes. If it is running, ask Higor to close Anki or explicitly confirm a safe route. Do not write through a locked/open collection.
3. Create a timestamped backup before writing.
4. Add each approved card as a new Anki note/card with:
   - front = `FRENTE` or cloze `TEXTO`;
   - back = `VERSO` / `VERSO/OBS`, including any approved final source block in `font-size: 65%` for lei seca/CPC/official support;
   - tags = standardized `TAG` plus any deck/note-type tags already agreed for Higor's study workflow;
   - theme/subject preserved in the card content when useful for review.
5. If deck or note type is ambiguous, use the established/default fiscal-study deck and note type if discoverable; ask only if there is no safe default.
6. After insertion, reopen/read the collection and verify the created notes exist, tags are present, and `pragma integrity_check` returns `ok`.
7. Report concisely: quantity inserted, deck/note type used, tags, backup path, official source blocks added/replaced when applicable, and integrity result.

Never insert unapproved draft cards into Anki. If Higor edits a card in the approval reply, insert the edited version, not the earlier draft.

## Common Pitfalls

1. **Turning a question into a mini-aula.** Fix: identify the exact gap and make one active-recall card.
2. **Making every alternative a card.** Fix: only autonomous and relevant alternatives become cards unless Higor explicitly asks for alternatives.
3. **Spoiling the trick in the theme.** Fix: theme stays broad; the assertion carries the test.
4. **Inventing support.** Fix: cite only visible or verified official support; otherwise omit.
5. **Errado too obvious.** Fix: make the distortion plausible and technical.
6. **Tags drifting.** Fix: reuse broad standardized tags, especially for recurring legal/accounting subjects.
7. **Overusing cloze.** Fix: reserve cloze for lists, exact wording, formulas, deadlines, or cumulative requirements.

8. **Assuming the source is always Tec Concursos.** Fix: handle laws, edital, PDFs, CPC, material de estudo, and pasted notes with the same approval-then-insert workflow.
9. **Forgetting the 65% source block.** Fix: for law/CPC/accounting/legal cards, append the minimal official lei seca/CPC/source wording at the end of the verso using `font-size: 65%` and the proper idempotency marker.
10. **Pasting huge official text.** Fix: include only the exact article/paragraph/inciso/item needed to support the card.

## Maintenance and GitHub Sync

When this skill changes for Higor, also keep the public backup repository in sync: `higorzz/hermes-anki-estudos-skills`, local checkout `/home/higor/hermes-anki-estudos-skills`. Copy the updated skill directory into `skills/productivity/tec-concursos-anki-cards/`, commit with the GitHub noreply email `15950097+higorzz@users.noreply.github.com`, and push. This is part of maintaining the Anki/estudos skill library, not part of generating cards.

## Verification Checklist

Before finalizing:

- [ ] Every card comes from a legible point in the source.
- [ ] For broad PDFs/aulas, the batch is lean and focused on the most recurrent cobrança patterns rather than every rule or exception.
- [ ] One card tests one idea.
- [ ] No invented exception, article, prazo, formula, or jurisprudence.
- [ ] Certo/Errado mix is sensible for the batch.
- [ ] Errado cards are plausible, not absurd.
- [ ] Each card has `TEMA` and `TAG`.
- [ ] The decisive wording in each verso is bolded.
- [ ] Official legal/CPC/accounting/NBC-CFC source blocks are appended at the end of the verso in `font-size: 65%` when applicable and verified.
- [ ] Final report counts cards, C/E, cloze, and explains the quantity.
