---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente de nuvem do Copilot que executasse três tarefas diferentes para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente de nuvem trabalhe de forma assíncrona e autônoma, ainda é importante acompanhar essas tarefas.

Há várias ferramentas para gerenciar as tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse centro de controle, você vê todas as tarefas dos agentes que têm pull requests (PRs) abertos. Você pode explorar as operações realizadas e até orientar uma sessão em andamento.

Neste exercício, você vai:

- explorar a página de agentes para acompanhar as tarefas do agente de nuvem;
- orientar uma sessão em andamento e solicitar uma funcionalidade adicional.

## Cenário

Depois que a equipe atribuiu ao agente a criação do modo de alto contraste, percebeu que seria bom adicionar também um modo claro. Como o trabalho já atualiza o estilo do site e adiciona uma opção de alternância, pareceu fazer sentido incluir essa funcionalidade. Você orientará o agente para garantir que ele implemente o modo claro além do alto contraste.

## Revise as tarefas do agente de nuvem do Copilot

Vamos conferir o status atual de todas as tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Você verá as tarefas atribuídas ao Copilot, incluindo:
    - atualizar a documentação da base de código;
    - gerar APIs para modificar jogos;
    - adicionar um modo de alto contraste ao site.
3. Selecione uma das tarefas em andamento. Revise as operações realizadas pelo Copilot, que podem incluir:
    - baixar o código do repositório;
    - criar o ambiente de trabalho do Copilot;
    - configurar servidores MCP;
    - executar várias etapas para concluir a tarefa atribuída.

> [!NOTE]
> As etapas listadas variam conforme o estado do trabalho do Copilot e a abordagem adotada.

4. Observe também o painel do pull request (PR), que aparece à direita. Ele permite acompanhar o PR e os arquivos alterados.

## Oriente o agente de nuvem

Agora que você viu as tarefas em andamento, vamos pedir ao Copilot para incluir o modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão referente à criação do modo de alto contraste. O título exato varia conforme o nome usado pelo Copilot e o estado atual do trabalho.

    ![Sessão de acessibilidade no centro de controle](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que ela indique que a configuração foi concluída e o trabalho começou. Você verá mensagens semelhantes às mostradas abaixo.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione o seguinte prompt:

    ```plaintext
    Enquanto trabalhamos no modo de alto contraste, vamos adicionar também um modo claro. Esse modo também deve ter uma opção de alternância para que as pessoas escolham a exibição desejada.
    ```

    ![Captura de tela da tarefa do agente de nuvem na página de agentes, com a caixa de diálogo para orientar a sessão ativa do Copilot destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot confirma o recebimento do prompt e o inclui no fluxo de trabalho.

## Deixe o Copilot trabalhar

Como antes, o Copilot começará a trabalhar na tarefa atualizada. Ele incorporará a nova solicitação ao fluxo depois de concluir a etapa em andamento no momento em que você enviou a mensagem.

Isso também levará alguns minutos. Aproveite para fazer uma pausa e refletir sobre o que você aprendeu e explorou até agora.

## Resumo e próximos passos

Neste exercício, você explorou a página de agentes do Copilot, seu centro de controle para acompanhar e orientar tarefas do agente de nuvem. Você pode acompanhar tarefas em andamento e concluídas, revisar o trabalho e redirecionar tarefas em execução para ajustar o escopo ou fornecer orientações adicionais.

Você explorou como:

- usar o centro de controle e a página de agentes para acompanhar as tarefas do agente de nuvem;
- redirecionar uma sessão em andamento para pedir uma funcionalidade adicional.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, vamos [iterar sobre os pull requests que ele criou][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
