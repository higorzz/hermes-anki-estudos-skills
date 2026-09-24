# Hermes Anki/Estudos Skills

Skills do Hermes Agent para o fluxo de estudo/Anki do Higor, especialmente geração de flashcards estilo C/E para concursos fiscais e escrita segura no banco local do Anki.

## Skills incluídas

| Skill | Descrição |
|---|---|
| `tec-concursos-anki-cards` | Gera flashcards objetivos a partir de prints/textos do Tec Concursos, leis, CPC/NBC, editais e materiais de estudo, com aprovação antes de inserir no Anki. |
| `anki-card-editing` | Workflow seguro para localizar, editar, inserir e verificar cards/notas diretamente no banco local do Anki. |

## Estrutura

```text
skills/
  productivity/
    tec-concursos-anki-cards/
      SKILL.md
    anki-card-editing/
      SKILL.md
      references/
        *.md
```

## Como instalar em outro Hermes

Copie as pastas para o diretório de skills do perfil Hermes:

```bash
mkdir -p ~/.hermes/skills/productivity
cp -a skills/productivity/tec-concursos-anki-cards ~/.hermes/skills/productivity/
cp -a skills/productivity/anki-card-editing ~/.hermes/skills/productivity/
```

Depois rode:

```bash
hermes skills list
```

## Observações

- Estas skills refletem preferências específicas do Higor para Anki/concursos fiscais.
- Não incluem coleção Anki, cards pessoais, banco `.anki2`, credenciais ou dados sensíveis.
- O fluxo padrão é: gerar cards → aprovar → inserir no Anki com backup e verificação.

## Licença

MIT.
