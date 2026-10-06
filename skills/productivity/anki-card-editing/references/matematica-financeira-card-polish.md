# Polishing existing Matemática Financeira cards

Use when Higor asks to review or improve existing Matemática Financeira Anki cards, especially after comparing them with the stronger Português card style.

## Quality target

Matemática Financeira cards should not be only formula memorization. Keep cloze for formulas, but make the collection more exam-useful by mixing:

- formula/mechanics cloze cards;
- Certo/Errado cards with plausible calculation traps;
- short scenario cards that force identifying the correct procedure.

A good target for future batches is roughly:

- 40% cloze for formulas, factors, relationships, and exact components;
- 40% Certo/Errado with plausible banca-style traps;
- 20% short mini-scenario/procedure cards.

Do not force this ratio when the source does not support it, but use it as a review lens.

## Existing-card edit workflow

1. Work read-only first: count candidate notes in the Matemática Financeira deck/tags, list note types, tags, and representative fronts/backs.
2. Identify cards that are technically correct but weak because they are:
   - formula-only when a short usage context would help;
   - dependent on “no exemplo...” or a remembered prior question;
   - missing the distinction between when to multiply vs divide, capitalize vs discount, or choose a data focal;
   - too generic to stand alone after days of review.
3. Before writes, follow the normal SQLite safety workflow: ensure no Anki process, refuse non-empty WAL, back up `collection.anki2` plus side files, then write and checkpoint.
4. Preserve note type, deck, and tags unless there is a clear taxonomy error.
5. Improve fronts by adding minimal context, not a mini-aula. Examples:
   - `Em uma compra com entrada, prestações mensais e pagamento final desconhecido...`
   - `Se um bem custa R$ X à vista e a comparação será feita em data futura sob juros compostos...`
   - `Em uma questão com dois títulos de mesmo valor nominal...`
6. Improve backs by making the decisive operation explicit and bolding the exact trap in HTML:
   - multiply vs divide factor;
   - value present vs discount;
   - SAC fixes amortization, Price fixes installment;
   - nominal vs effective rate;
   - real rate uses compound relation, not direct subtraction.
7. For cloze cards, keep the blank precise and add one short `Back Extra` explaining when/why the formula applies.
8. For formula-heavy Matemática Financeira cards, prefer Anki MathJax/LaTeX instead of plain-text formulas in both front and back. Use display math for main formulas, e.g. `<br>{{c1::<div style="text-align:center; margin: 4px 0;">\\[A=\\frac{N}{(1+i)^n}\\]</div>}}`. In back-side term/practice blocks, always define every symbol used in the formula before the worked example (for example: `A = valor atual; N = valor nominal/futuro; i = taxa por período; n = número de períodos`; for series: `FV/VP = valor futuro/presente; PMT = prestação; i = taxa; n = número de pagamentos`; for discounts: `N = valor nominal; d/i = taxa de desconto; n = prazo`). Put the worked example formula on its own line as display math after a short `Ex.:` line, e.g. `Ex.: N=121, i=10%, n=2.<br><div style="text-align:center; margin: 4px 0;">\\[A=\\frac{121}{(1{,}10)^2}=100\\]</div>`. Never leave examples as cramped text like `100·s_n|i`, `0,02·3=0,06`, or unexplained `N/i/n`; avoid nested MathJax delimiters such as `\\(A=\\frac{N}{\\((1+i)^n\\)}\\)`.
9. After writing, reopen read-only and verify:
   - `integrity_check = ok`;
   - WAL is empty after checkpoint;
   - target note count still matches expectation;
   - tags are preserved;
   - spot-check representative edited strings.

## New-card gaps worth considering

If not already covered, suggest for approval before inserting:

- taxa nominal vs efetiva;
- taxa real vs aparente/inflação;
- SAC vs Price;
- série antecipada vs postecipada traps;
- data focal choice does not alter final result when all flows are transported correctly;
- desconto comercial vs racional: `Ac < Ar` and `Dc > Dr` for same nominal/taxa/prazo.

Do not insert new cards before Higor approves the draft. Once approved, insert directly into `2018::Matemática Financeira` using Basic/Cloze note types and verify as usual.