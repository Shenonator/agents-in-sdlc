---
title: "Lição 2 - Adicione avaliações por estrelas: uma vitória rápida"
description: "Use o Copilot CLI para fazer uma pequena alteração nos cards de jogos, revise-a em um navegador encaminhado e mescle-a no seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e experimentou uma conversa, é hora de fazer sua primeira alteração no projeto. Vamos manter o escopo pequeno: os dados dos jogos já incluem uma avaliação por estrelas, mas ela ainda não aparece nos cards da página inicial. Você pedirá ao agente que a mostre, revisará a alteração e a mesclará no seu primeiro pull request.

Nesta lição, você vai:

- iniciar uma conversa focada com o Copilot em uma branch de funcionalidade;
- pedir ao agente para fazer uma pequena alteração no projeto;
- revisar a alteração com `/diff`;
- executar o aplicativo e confirmar a mudança em um navegador encaminhado;
- abrir e mesclar seu primeiro pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma avaliação por estrelas, que já aparece na página de detalhes. Os cards da página inicial, porém, mostram apenas o título, a categoria, a editora e a descrição. Como aquecimento, peça ao agente para exibir em cada card a avaliação existente — uma alteração pequena e independente, ideal para sua primeira sessão.

## Anatomia de uma conversa

Uma **conversa** é onde você trabalha com o Copilot CLI em uma tarefa. Ao contrário do aplicativo Copilot, uma conversa normal do CLI usa o repositório e a branch do Git atualmente selecionados no terminal, em vez de criar um worktree dedicado. As conversas salvas permitem retomar a mesma discussão mais tarde, enquanto os arquivos e a branch continuam sendo o estado normal do Git no disco.

Em uma conversa, você verá três coisas: seus prompts e as respostas do agente; a atividade do agente com ferramentas enquanto explora e edita arquivos; e as alterações que você pode inspecionar com `/diff`.

## Inicie uma conversa e solicite a alteração

Vamos iniciar uma nova conversa para implementar a funcionalidade.

1. Volte ao Codespace.
2. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Se o Copilot ainda não estiver em execução, use o comando:

   ```bash
   copilot --yolo
   ```

4. Inicie uma nova sessão com o comando slash `/new` e pressione <kbd>Enter</kbd>.
5. Use este prompt para solicitar a alteração:

   ```plaintext
   Mostre nos cards da lista de jogos a avaliação starRating de cada jogo, de 0 a 5. Se a avaliação for null, mostre "No rating yet". Mantenha o layout dos cards, adicione testes e execute as verificações pertinentes.
   ```

O Copilot explorará o projeto, localizará os arquivos usados para exibir os detalhes dos jogos e criará o código necessário. Você acabou de adicionar uma funcionalidade com o Copilot CLI.

## Revise as diferenças

Toda alteração gerada por IA deve ser revisada antes da mesclagem, mesmo as pequenas. Vamos explorar as mudanças diretamente no Copilot CLI.

1. Digite `/diff` e inspecione todos os arquivos alterados.
2. Confirme se o card mostra a avaliação numérica quando ela está disponível e `No rating yet` quando `starRating` é `null`.
3. Confirme se os testes cobrem os dois estados.
4. Revise os resultados das verificações executadas pelo Copilot e peça para corrigir eventuais falhas.
5. Ao terminar a revisão, pressione <kbd>Esc</kbd> para sair da tela de diferenças.

> [!NOTE]
> Como o Copilot, assim como todas as ferramentas de IA generativa, é probabilístico e não determinístico, o código exato pode variar. Revise o comportamento em vez de esperar uma implementação específica.

## Verifique as alterações

Não basta ler o código e presumir que funciona. Vamos pedir ao Copilot para iniciar o site e examinar a interface atualizada no navegador encaminhado pelo Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Inicie o aplicativo para que eu possa inspecionar a alteração das avaliações por estrelas no navegador. Informe a URL e mantenha o servidor em execução.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme se os cards dos jogos mostram as avaliações de zero a cinco.
4. Volte ao Copilot e peça para interromper o servidor iniciado:

   ```plaintext
   Interrompa o servidor de desenvolvimento que você iniciou.
   ```

## Abra e mescle seu primeiro pull request

Você criou a funcionalidade. Agora, crie um pull request (PR) para mesclar o novo código ao projeto.

1. Peça ao agente padrão para fazer commit da alteração:

   ```plaintext
   Faça commit das alterações revisadas nas avaliações por estrelas com uma mensagem de commit adequada.
   ```

2. Digite `/pr create`. Ao criar o PR, o Copilot CLI envia o commit existente e mostra a URL do PR.
3. Para abrir o PR, mantenha pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecione a URL exibida pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando estiver tudo pronto, selecione **Merge pull request** e confirme a mesclagem.
6. Volte ao Codespace e saia do Copilot CLI com `/exit`.
7. Atualize sua branch local `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Parabéns! Você entregou sua primeira alteração com o GitHub Copilot CLI. Você:

- iniciou uma conversa focada com o Copilot em uma branch de funcionalidade;
- orientou o agente para fazer uma pequena alteração nos cards de jogos;
- revisou a alteração com `/diff`;
- executou o aplicativo e confirmou a avaliação por estrelas em um navegador encaminhado;
- abriu e mesclou seu primeiro pull request.

Em seguida, você [começará pela issue de filtros e usará os modos Plan e Autopilot][next-lesson] para criar uma funcionalidade maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
