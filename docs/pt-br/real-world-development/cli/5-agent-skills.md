---
title: "Lição 5 - Personalizar e usar uma skill de verificações de qualidade"
description: "Explore a skill quality-checks existente, personalize o formato do relatório e use-a para validar a filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar envolve muito mais do que apenas escrever código. Já validamos manualmente que o código funciona e usamos arquivos de instruções para garantir que ele siga nossos padrões. Mas e os testes? E o lint? E as outras partes da integração contínua (CI)?

Para esses tipos de tarefa, as **skills de agente** são ideais! As skills ajudam o Copilot a entender como executar operações desse tipo corretamente.

Nesta lição, você:

- explorará a skill existente `quality-checks`;
- personalizará o formato dos resultados;
- recarregará e executará a skill.

## Cenário

A Tailspin Toys usa a skill `quality-checks` para testes de unidade, lint e verificações de tipos. A equipe quer melhorar o relatório para facilitar a leitura dos resultados.

## Instruções, scripts e recursos

As skills de agente agrupam instruções reutilizáveis para tarefas, scripts executáveis e recursos de apoio que um agente carrega quando necessário. Em geral, elas consistem em uma pasta com o nome da skill e um arquivo Markdown chamado `SKILL.md`. Esse arquivo contém frontmatter com um nome e uma descrição que definem a skill, uma visão geral do que ela faz e orientações sobre quando deve ser usada. A pasta também pode incluir subpastas com scripts e outros recursos que a skill utiliza.

> [!NOTE]
> Uma skill não precisa de pastas ou arquivos adicionais. A skill `quality-checks` da Tailspin Toys contém apenas `SKILL.md` porque usa os comandos já existentes no projeto.

As skills podem ficar na pasta `.github/skills` do projeto, como um recurso do repositório compartilhado e reutilizado pela equipe, ou na pasta de skills do usuário em `~/.copilot/skills`.

## Explore a skill

Vamos explorar a skill `quality-checks`, criada pela equipe da Tailspin Toys para executar testes de unidade, lint e verificações de tipos.

1. Volte ao Codespace. No editor do Codespaces, abra `.github/skills/quality-checks/SKILL.md`.
2. Leia os campos `name` e `description` na parte superior. A descrição ajuda o Copilot a entender quando deve chamar a skill.
3. Leia as instruções e observe como elas orientam o Copilot no processo de testes e lint.
4. Observe que a skill ainda não inclui uma seção **Results output formatting**.

## Execute a skill antes de fazer uma mudança

As skills podem ser chamadas diretamente no Copilot CLI ou por meio de linguagem natural. Vamos pedir ao Copilot que execute as três verificações da skill.

1. Volte à conversa de filtragem no modo Interactive.
2. Use o seguinte prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório final.

## Personalize o relatório

Vamos melhorar o relatório para indicar o que foi executado, se houve sucesso e quais resultados as ferramentas informaram. Atualize a skill para gerar esse relatório.

1. Volte a `.github/skills/quality-checks/SKILL.md`.
2. Adicione a seção a seguir ao final do arquivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. O arquivo será salvo automaticamente.

## Execute a skill atualizada

Agora que fizemos a mudança, vamos ver como ela funciona! O Copilot CLI pode recarregar skills editadas sem reiniciar a conversa.

1. Digite:

   ```plaintext
   /skills reload
   ```

2. Use exatamente o mesmo prompt de antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório final e compare-o com o primeiro.

## Resumo e próximos passos

Você personalizou e usou uma skill de agente existente. Nesta lição, você:

- explorou a skill `quality-checks` para testes de unidade, lint e verificações de tipos;
- personalizou o formato dos resultados;
- recarregou e executou a skill.

Essa mudança acompanhará a filtragem no PR do recurso. Em seguida, você permitirá que o Copilot interaja diretamente com o site [por meio do servidor MCP do Playwright][next-lesson].

## Mais exemplos de skills

Estes exemplos da comunidade servem como referência, não como tarefas adicionais:

- [Especificação de Agent Skills][skill-spec]
- [Fluxo de contribuição: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas e script de exportação incluído: `drawio`][drawio-example]
- [Testes em navegadores: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
