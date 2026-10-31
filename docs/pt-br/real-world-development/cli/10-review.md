---
title: "Lição 10 - Conclusão e próximos passos"
description: "Recapitule o fluxo de trabalho do Copilot CLI, os dois pull requests, as personalizações reutilizáveis e outros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Você usou o GitHub Copilot CLI em um fluxo de trabalho contínuo na Tailspin Toys. Você:

- preparou um Codespace, instalou o Copilot CLI, explorou o projeto e encontrou a issue de filtros incluída no backlog;
- adicionou avaliações por estrelas, revisou o resultado em um navegador encaminhado e mesclou manualmente seu primeiro pull request (PR);
- começou pela issue de filtros, definiu a abordagem no modo Plan, desenvolveu no modo Autopilot e revisou no modo Interactive;
- orientou o agente com instruções personalizadas, personalizou a skill `quality-checks` existente e usou-a para executar testes unitários, lint e verificações de tipos;
- adicionou o servidor Playwright Model Context Protocol (MCP) e usou-o para explorar os filtros em um navegador real;
- criou e selecionou um agente QA (garantia de qualidade) personalizado para avaliar requisitos, cobertura, resultados da skill e evidências do navegador;
- revisou as alterações completas dos filtros e autorizou o Agent Merge para o PR;
- explorou comandos slash para contexto, modelos, compartilhamento e delegação opcional ao agente de nuvem.

## O que você entregou

O workshop tem dois marcos de PR, cada um em sua própria branch criada a partir de `main` atualizada:

1. **Avaliações por estrelas:** exibir `starRating` e um estado explícito para jogos sem avaliação nos cards.
2. **Filtros e fluxo de qualidade:** implementar os filtros, atualizar as instruções e aplicá-las à funcionalidade, personalizar o relatório `quality-checks`, criar um perfil de QA e incluir os testes relacionados.

Do planejamento dos filtros até a abertura do PR, você usou a mesma conversa e a mesma branch. Reunimos esse trabalho em um único PR para simplificar o workshop.

## Diferentes tipos de verificação

Você verificou o código de várias maneiras: testes automatizados, inspeção do site no navegador e exploração do navegador pelo Copilot usando MCP. A skill `quality-checks` executou testes unitários, lint e verificações de tipos e apresentou os resultados no novo formato. Antes do PR, o QA reuniu esses resultados com a revisão dos requisitos e da cobertura de testes.

Os testes adicionados devem preencher lacunas reais; uma execução de QA que conclua que não são necessários testes novos também pode estar correta. Revise o código e as evidências antes de autorizar a mesclagem e atualize as evidências afetadas depois de qualquer alteração.

## Práticas recomendadas

O contexto e as ferramentas fornecidos ao Copilot influenciam o trabalho. Neste workshop, você atualizou instruções, personalizou uma skill, criou um perfil de QA e configurou um servidor MCP. Reutilize essas personalizações em diferentes conversas e ajuste-as conforme as necessidades da equipe. As instruções definem padrões, as skills descrevem tarefas repetíveis, os agentes personalizados definem funções especializadas e os servidores MCP conectam ferramentas externas. Revise as alterações e os resultados das ferramentas, não apenas o resumo do agente.

Escolha o **modo e o modelo** de acordo com a tarefa. Use o **Plan** para pensar na abordagem antes de desenvolver, o **Interactive** para acompanhar de perto mudanças focadas e o **Autopilot** para tarefas com escopo bem definido. Escolha um modelo mais rápido para edições rotineiras e outro mais capaz para tarefas complexas.

O contexto continua sendo tão importante quanto a infraestrutura. Descrever com clareza *o que* você quer desenvolver, *por quê* e *como* influencia significativamente o resultado.

## Explore mais

Você concluiu o fluxo principal do workshop. Vale a pena conhecer mais alguns recursos do CLI:

- `/review` para pedir ao agente de revisão de código que analise as alterações;
- `/rubber-duck` para conversar sobre um problema e obter outra perspectiva;
- `/fleet` para coordenar subtarefas independentes em paralelo;
- `/worktree` para isolar uma tarefa separada;
- `/delegate` para enviar uma tarefa ao agente de nuvem do Copilot.

## Próximos passos

A melhor maneira de melhorar com qualquer ferramenta é continuar usando-a. Use-a em código de produção, em projetos pessoais ou naquele pequeno aplicativo que você imagina há anos, mas ainda não começou. Compartilhe seus aprendizados com a equipe e aprenda com os outros. E, como sempre, explore a documentação.

Se quiser ampliar o Tailspin Toys pelo terminal, continue com a série opcional [Foundry Backer Concierge][foundry]. Para comparar outros ambientes, explore o [workshop do VS Code][vscode], o [workshop do aplicativo GitHub Copilot][app] ou o [workshop do agente de nuvem do Copilot][cloud].

## Recursos

- [Sobre o GitHub Copilot CLI][about-cli]
- [Referência de comandos do Copilot CLI][cli-reference]
- [Personalizar o Copilot CLI][customize-cli]
- [Gerenciar pull requests com o Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
