---
title: "Lição 5 - Personalize e use a skill quality-checks"
description: "Explore a skill quality-checks existente, personalize o formato do relatório e use-a para validar os filtros."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Escrever código envolve mais do que apenas escrever código. Já validamos manualmente que ele funciona e usamos arquivos de instruções para garantir que siga nossos padrões. Mas e os testes? E o lint? E as outras partes da integração contínua (CI)?

Para esses tipos de tarefa, as **skills de agente** são ideais. As skills ajudam o Copilot a entender como executar operações como essas.

Nesta lição, você vai:

- explorar a skill `quality-checks` existente;
- personalizar o formato dos resultados;
- recarregar e executar a skill.

## Cenário

A Tailspin Toys usa a skill `quality-checks` para executar testes unitários, lint e verificações de tipos. A equipe quer aprimorar o relatório para facilitar a leitura dos resultados.

## Instruções, scripts e recursos

As skills de agente reúnem instruções reutilizáveis para tarefas, scripts executáveis e recursos de apoio que o agente carrega sob demanda. Em sua forma básica, uma skill é uma pasta com o nome da skill que contém um arquivo Markdown chamado `SKILL.md`. O Markdown inclui frontmatter com um nome e uma descrição que definem a skill, uma visão geral do que ela faz e orientações sobre quando deve ser usada. A pasta também pode conter subpastas com scripts e outros recursos que a skill utiliza durante a execução.

> [!NOTE]
> Uma skill não precisa de arquivos e pastas adicionais. A skill `quality-checks` da Tailspin Toys contém apenas `SKILL.md`, pois usa os comandos já existentes no projeto.

As skills podem ficar na pasta `.github/skills` do projeto para serem compartilhadas e reutilizadas pela equipe ou na pasta de skills do usuário, em `~/.copilot/skills`.

## Explore a skill

Vamos explorar a skill criada pela equipe da Tailspin Toys para executar testes unitários, lint e verificações de tipos, chamada `quality-checks`.

1. Volte ao Codespace. No editor do Codespaces, abra `.github/skills/quality-checks/SKILL.md`.
2. Leia os campos `name` e `description` no início do arquivo. A descrição ajuda o Copilot a entender quando deve usar a skill.
3. Leia as instruções e observe como elas orientam o Copilot no processo de testes e lint.
4. Observe que a skill ainda não tem uma seção **Results output formatting**.

## Execute a skill antes de fazer alterações

Vamos pedir ao Copilot para executar as três verificações da skill.

1. Volte à conversa sobre os filtros no modo Interactive.
2. Use o prompt:

   ```plaintext
   Execute a skill quality-checks para testes unitários, lint e verificações de tipos.
   ```

3. Observe o relatório apresentado ao final.

## Personalize o relatório

Queremos um relatório melhor, que informe o que foi executado, se houve aprovação e o que as ferramentas relataram. Vamos atualizar a skill para gerar esse relatório.

1. Volte a `.github/skills/quality-checks/SKILL.md`.
2. Adicione a seção a seguir ao final do arquivo:

   ```markdown
   ## Results output formatting

   Após a conclusão, informe cada comando executado e se ele foi aprovado, reprovado ou bloqueado. Inclua contagens de testes, durações, erros, avisos e outras métricas somente quando forem informadas pela ferramenta. Indique a próxima ação para cada falha ou bloqueio e nunca descreva uma verificação ignorada ou incompleta como aprovada.
   ```

3. O arquivo será salvo automaticamente.

## Execute a skill atualizada

Com a alteração pronta, vamos ver a skill em ação. O Copilot CLI pode recarregar skills editadas sem reiniciar a conversa.

1. Digite:

   ```plaintext
   /skills reload
   ```

2. Use exatamente o mesmo prompt de antes:

   ```plaintext
   Execute a skill quality-checks para testes unitários, lint e verificações de tipos.
   ```

3. Observe o relatório final e compare-o com o primeiro.

## Resumo e próximos passos

Você personalizou e usou uma skill de agente existente. Nesta lição, você:

- explorou a skill `quality-checks` para testes unitários, lint e verificações de tipos;
- personalizou o formato dos resultados;
- recarregou e executou a skill.

Essa alteração acompanhará os filtros no PR da funcionalidade. Em seguida, você permitirá que o Copilot interaja diretamente com o site [por meio do servidor Playwright MCP][next-lesson].

## Mais exemplos de skills

Estes exemplos da comunidade servem como referência, não são tarefas adicionais:

- [Especificação de Agent Skills][skill-spec]
- [Fluxo de contribuição: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas e script de exportação incluído: `drawio`][drawio-example]
- [Testes de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
