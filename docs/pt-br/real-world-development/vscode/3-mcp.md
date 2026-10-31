---
title: "Exercício 3 - Teste sua funcionalidade com o servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar a funcionalidade de filtros com o modo de agente. Antes de abrir um pull request, confirme se ela realmente funciona no navegador. Em vez de testar o aplicativo manualmente, você conectará o **servidor Playwright MCP** e deixará que o Copilot controle um navegador real para testar a funcionalidade. Depois, publicará sua branch e abrirá o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas do Copilot;
- adicionar o servidor Playwright MCP ao workspace;
- pedir ao Copilot para usá-lo e testar manualmente os filtros no navegador;
- publicar sua branch e abrir um pull request com as alterações dos filtros.

## O que é o Model Context Protocol (MCP)?

O modo de agente fica muito mais poderoso quando pode acessar recursos além do editor. O Model Context Protocol (MCP) é a maneira como o Copilot faz isso: um padrão que permite ao agente se comunicar com ferramentas e serviços externos.

![Diagrama dos componentes internos do modo de agente e de sua interação com o contexto, o LLM e as ferramentas — incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA se comuniquem com ferramentas e serviços externos. Com o MCP, os agentes acessam informações atualizadas (usando recursos) e executam ações em seu nome (usando ferramentas).

Essas ferramentas e esses recursos são acessados por meio de um servidor MCP, que atua como ponte entre o agente de IA e as ferramentas e os serviços externos. O servidor MCP gerencia a comunicação entre o agente e as ferramentas externas, como APIs existentes ou ferramentas locais em pacotes npm. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente de IA pode acessar.

Alguns servidores MCP conhecidos:

- **[Servidor MCP do GitHub](https://github.com/github/github-mcp-server)**: dá acesso a APIs para gerenciar repositórios do GitHub. Ele permite que o agente crie repositórios, atualize repositórios existentes e gerencie issues e pull requests.
- **[Servidor Playwright MCP](https://github.com/microsoft/playwright-mcp)**: oferece recursos de automação de navegador com Playwright. Ele permite que o agente acesse páginas, preencha formulários e selecione botões.

Há muitos outros servidores MCP disponíveis que oferecem acesso a ferramentas e recursos diferentes. O GitHub mantém um [registro de MCP](https://github.com/mcp) para facilitar a descoberta e a contribuição ao ecossistema.

> [!CAUTION]
> Em relação à segurança, trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor MCP, revise cuidadosamente o código-fonte, verifique o publicador e considere as implicações de segurança. Use somente servidores MCP confiáveis e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revise a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste workspace do VS Code.

1. Abra `.vscode/mcp.json` no Codespace.
2. Você verá um servidor `github` já configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

Essa entrada `github` vem com o modelo do projeto. Por isso, o Copilot conseguiu ler as issues do backlog no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], então não é preciso instalá-lo localmente. O VS Code se conecta a ele por HTTP, e você autentica sua conta do GitHub na primeira vez que o Copilot usa uma de suas ferramentas.

## Adicione o servidor Playwright MCP

Agora você adicionará um segundo servidor. O [servidor Playwright MCP][playwright-mcp-server] oferece ao Copilot um navegador que ele pode controlar, exatamente o que precisamos para testar a funcionalidade.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` junto com `github`, para que o arquivo fique assim:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Salve o arquivo.

Ao contrário do servidor remoto `github`, o `playwright` é um servidor **local**: o VS Code o inicia na máquina executando `npx @playwright/mcp@latest`. A opção `--headless` executa o navegador sem uma janela visível, necessária em um Codespace sem ambiente de área de trabalho.

> [!NOTE]
> O projeto Tailspin Toys já usa Playwright nos testes de ponta a ponta, então o navegador necessário normalmente já está instalado. Se o Copilot informar que não há um navegador disponível, peça para executar `npx playwright install chromium` e tente novamente.

## Inicie e confie no servidor

O VS Code inicia servidores MCP **sob demanda**: na primeira vez que o Copilot precisa das ferramentas de um servidor, o VS Code o inicia e pede que você confirme se confia nele.

1. Em `.vscode/mcp.json`, aparecerá uma ação **Start** acima da entrada `playwright`. Selecione-a para iniciar o servidor e confirmar se a configuração está correta.
2. Se o VS Code perguntar se você confia no servidor, revise a configuração e autorize-o para que possa iniciar.

Iniciá-lo manualmente é opcional. Se você ignorar esta etapa, o VS Code iniciará o servidor e pedirá que você confie nele na primeira vez que o Copilot precisar de suas ferramentas.

## Teste a funcionalidade de filtros

O [servidor Playwright MCP][playwright-mcp-server] oferece ao Copilot um navegador real para controlar. Em vez de você navegar pelo aplicativo e conferir as mudanças, o agente pode abrir uma página, navegar, aplicar filtros, ler os resultados e resumir o que observou. É uma maneira rápida de confirmar se a funcionalidade funciona como esperado sem sair da conversa.

Nos bastidores, o servidor Playwright MCP usa a [árvore de acessibilidade][playwright-mcp-server] da página, não capturas de tela. Assim, o agente analisa elementos estruturados e rotulados (botões, links, itens de lista) como a tecnologia assistiva faz. Uma verificação funcional rápida também funciona como uma verificação básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot para testar os filtros que você acabou de criar:

```text
Usando o servidor Playwright MCP, abra um navegador no aplicativo em execução em http://localhost:4321 e verifique a nova funcionalidade de filtros de jogos:

1. Acesse a página de jogos e informe quantos jogos estão listados.
2. Aplique um filtro de categoria e confirme se a lista passa a exibir somente jogos dessa categoria.
3. Remova o filtro e aplique um filtro de editora. Confirme se a lista exibe somente os jogos dessa editora.
4. Combine um filtro de categoria com um de editora e confirme se os resultados respeitam ambos.

Descreva o que observou em cada etapa e destaque qualquer comportamento inesperado.
```

O Copilot abrirá um navegador pelo servidor Playwright MCP, seguirá as etapas e apresentará os resultados. Compare o resumo com os critérios de aceitação da issue. Se algo parecer incorreto, faça perguntas complementares ou peça para corrigir o código antes de abrir o pull request.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321` para esse teste. Se você interrompeu o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt. Na primeira vez que o Copilot usar o servidor Playwright MCP, talvez seja necessário baixar um navegador. Se o Copilot informar que falta um navegador, peça para executar `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publique a branch e crie um pull request

Agora que confirmou que a funcionalidade funciona, você pode abrir um pull request (PR) para que a equipe a revise. Primeiro, publique a branch `filtering-vscode`.

1. Acesse o painel **Source Control** no Codespace e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Captura de tela do painel Source Control com as alterações realizadas](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Crie o pull request

Há várias maneiras de criar um pull request, incluindo o github.com e a interface de linha de comando (CLI) do GitHub. Como você já está trabalhando com o GitHub Copilot, peça que ele crie o PR. Ele pode localizar a issue relevante e associá-la ao PR.

1. Acesse o painel Copilot Chat e selecione **New Chat** para iniciar uma nova sessão.
2. Confirme se **Agent** está selecionado na lista de agentes para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot para criar um PR:

   ```plaintext
   Encontre no repositório a issue sobre filtros por categoria e editora. Crie um pull request para a branch atual e associe-o à issue correta.
   ```

4. Se necessário, selecione **Continue** para permitir que o Copilot colete informações e execute as operações. Na primeira vez que usar uma ferramenta do GitHub, talvez seja necessário autenticar sua conta. Siga as instruções para autorizar.
5. Observe o Copilot pesquisar as issues, localizar a correta e criar o PR.
6. Selecione o link criado pelo Copilot para revisar o pull request, mas **não o mescle ainda**.

## Resumo e próximos passos

Parabéns! Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas do Copilot;
- adicionou o servidor Playwright MCP ao workspace;
- usou o servidor para testar manualmente a funcionalidade de filtros no navegador antes de publicá-la;
- publicou sua branch e abriu um pull request com o trabalho dos filtros.

Você usou um servidor MCP para *testar* uma funcionalidade, mas o MCP é apenas uma das maneiras de ampliar o Copilot. Em seguida, vamos [criar um agente personalizado][next-lesson] para agilizar tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor Playwright MCP da Microsoft][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro de MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
