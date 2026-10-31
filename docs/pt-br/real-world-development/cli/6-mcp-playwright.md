---
title: "Lição 6 - Valide a funcionalidade com Playwright MCP"
description: "Inspecione ou configure o Playwright MCP no Copilot CLI e use-o para explorar a funcionalidade de filtros no navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como já destacamos, escrever código envolve mais do que apenas escrever código. Precisamos trabalhar com dados, serviços externos e outras automações que podem ser disponibilizadas ao Copilot. É aqui que entram os servidores MCP: eles permitem que o Copilot vá além dos recursos incluídos no CLI, oferecendo mais ferramentas e serviços.

Nesta lição, você vai:

- entender o que é o Model Context Protocol (MCP) e como o Copilot CLI o usa;
- adicionar o servidor Playwright MCP, se ele ainda não estiver disponível;
- pedir ao agente para controlar um navegador e explorar a funcionalidade de filtros.

## Cenário

Testes unitários e de ponta a ponta são importantes, mas validar mudanças na interface exige interagir com ela. Você quer permitir que o Copilot use o site como uma pessoa usuária faria e automatize melhor a validação, aumentando a confiança de que as mudanças funcionam como esperado.

## O que é o Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de IA se comuniquem com ferramentas e serviços externos. Com o MCP, os agentes acessam informações atualizadas e executam ações em seu nome.

As ferramentas e os recursos são acessados por meio de um servidor MCP, que atua como ponte entre o agente de IA e as ferramentas e serviços externos. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente de IA pode acessar.

Alguns servidores MCP conhecidos:

- **[Servidor MCP do GitHub][github-mcp]**: dá acesso a APIs para gerenciar repositórios, issues e pull requests do GitHub.
- **[Servidor Playwright MCP][playwright-mcp-server]**: oferece recursos de automação de navegador com Playwright.

Há muitos outros servidores MCP disponíveis. O GitHub mantém um [registro de MCP][mcp-registry] para facilitar a descoberta e a contribuição ao ecossistema.

> [!CAUTION]
> Trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor, revise o código-fonte, verifique o publicador e considere as implicações de segurança.

## Adicione o servidor Playwright MCP

Se o servidor Playwright MCP ainda não estiver disponível, adicione-o para que o Copilot possa interagir com o site como uma pessoa usuária faria.

> [!NOTE]
> Se o Playwright MCP já estiver configurado no Copilot CLI, ignore estas etapas e continue na próxima seção.

1. Volte ao Codespace.
2. No Copilot CLI, abra a caixa de diálogo para adicionar um servidor MCP com o comando:

   ```plaintext
   /mcp add
   ```

3. Em nome, digite `playwright` e pressione <kbd>Tab</kbd>.
4. Confirme o tipo de servidor **STDIO** pressionando <kbd>Enter</kbd> e, em seguida, pressione <kbd>Tab</kbd>.
5. Cole o seguinte na caixa de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pressione <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para salvar o novo servidor MCP.
7. Pressione <kbd>Esc</kbd> para sair da caixa de diálogo do MCP.

## Peça ao Copilot para explorar a funcionalidade com o Playwright

Antes, você confirmou manualmente que a funcionalidade funciona como esperado. Agora, vamos usar o servidor Playwright MCP disponível no Copilot para fazer o mesmo.

1. Use este prompt para pedir ao Copilot que valide a funcionalidade com o servidor Playwright MCP:

   ```plaintext
   Inicie o aplicativo e use o Playwright MCP para verificar os filtros conforme a issue e nosso plano. Diga o que funciona e o que não funciona, sem fazer alterações. Quando terminar, interrompa o servidor iniciado.
   ```

> [!NOTE]
> Não é necessário dizer ao Copilot para usar o servidor MCP; normalmente ele descobrirá isso sozinho. Porém, como você sabe qual ferramenta deve usar, indicar a direção pode ajudar a obter resultados mais consistentes e economizar tokens.

2. Acompanhe as etapas executadas pelo Copilot no navegador para confirmar se a funcionalidade funciona.
3. Leia o relatório e confira se tudo se comporta como esperado.

O Copilot iniciará o servidor, usará o Playwright para interagir com o site, interromperá o servidor e apresentará um relatório.

## Resumo e próximos passos

Parabéns, você usou o servidor Playwright MCP para explorar sua funcionalidade em um navegador real a partir do Copilot CLI. Você:

- aprendeu o que é o Model Context Protocol (MCP) e como o Copilot CLI o usa;
- adicionou o servidor Playwright MCP, caso ele ainda não estivesse disponível;
- pediu ao agente para controlar um navegador e explorar a funcionalidade de filtros.

Em seguida, você [criará um agente QA personalizado][next-lesson] que combinará a skill e as ferramentas de navegador em uma função especializada.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor Playwright MCP da Microsoft][playwright-mcp-server]
- [Adicionar servidores MCP ao Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
