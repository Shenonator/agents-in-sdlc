---
title: "Exercício 2 - Adicione funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até mesmo uma atualização simples em um aplicativo normalmente exige alterar vários arquivos e executar operações, como testes. Como pessoa desenvolvedora, seu fluxo costuma envolver localizar todos os arquivos necessários, fazer as alterações, executar os testes, depurar, descobrir qual arquivo ficou de fora, atualizá-lo e assim por diante.

É aí que entra o modo de agente do Copilot.

O modo de agente do Copilot foi criado para trabalhar com mais autonomia no IDE. Ele funciona de maneira semelhante a uma pessoa desenvolvedora: começa explorando a estrutura do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos explorar como usar o modo de agente para adicionar funcionalidades ao site.

> [!NOTE]
> Apesar dos nomes parecidos, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente trabalha no IDE, com ciclos rápidos de feedback e interação. O agente de nuvem trabalha de forma assíncrona, como uma pessoa da equipe, usando issues e pull requests para interagir com você.

Neste exercício, você vai aprender como:

- o modo de agente do Copilot explora o projeto, identifica os arquivos relevantes e coordena as alterações;
- o modo de agente do GitHub Copilot implementa funcionalidades na interface e na camada de dados;
- revisar as alterações e os testes gerados pelo modo de agente do Copilot antes de mesclá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem os jogos por categoria e editora. No exercício anterior, você adicionou um helper de editoras. Agora, vai concluir o trabalho restante na camada de dados, na interface e nos testes usando o modo de agente do Copilot.

## Execute o site da Tailspin Toys

Antes de fazer alterações, explore o site da Tailspin Toys para entender seus recursos atuais.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com tema de desenvolvimento. Ele permite listar jogos e exibir detalhes sobre eles. O site é um único aplicativo Astro que renderiza as páginas como HTML estático durante a compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, usando o ORM Drizzle — não há API de back-end separada nem framework de interface no cliente. Os helpers reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é adicionada com um pequeno `<script>` do Astro, com escopo limitado e usando APIs padrão do DOM.

### Inicie o site

Para facilitar a execução do site, há uma tarefa `npm run dev` que inicia o único servidor de desenvolvimento Astro. Para executá-la no GitHub Codespace:

1. Volte ao Codespace. Você deverá estar em `main` depois do exercício anterior.
2. Abra um novo terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Atualize `main` e crie uma branch para o trabalho dos filtros:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute o comando para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento Astro estiver pronto, você verá uma mensagem com a URL, semelhante a esta:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se aparecer uma caixa de diálogo perguntando se você quer abrir um navegador para `http://localhost:4321`, feche-a selecionando o **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Em um Codespace, selecionar o link `localhost` no terminal redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o Codespace, que agora hospeda o servidor Web.

### Explore o site

Com o site em execução, explore suas funcionalidades principais:

- **Página inicial**: exibe a lista de jogos de tabuleiro, com títulos, imagens e descrições.
- **Página de detalhes do jogo**: quando você seleciona um jogo, abre uma página com mais informações, como título, descrição, editora e categoria.

## Explore o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e a seleção de modelo variam por exercício — as etapas indicarão o que usar quando necessário.

A implementação inicial do site funciona, mas queremos adicionar novos recursos. Vamos começar revisando o backlog. Ao configurar o projeto a partir do modelo, um conjunto de issues foi criado automaticamente. Peça ao GitHub Copilot para exibi-las.

1. Na lista de agentes da exibição Chat, selecione **Agent**. O agente planeja e implementa alterações em vários arquivos, executa comandos no terminal e usa ferramentas.

   ![Captura de tela do seletor de agentes na exibição Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores deste workshop não estão recomendando um modelo específico. Durante a criação do workshop, usamos Claude Sonnet 4.5 e o incluímos nas instruções para tornar as sugestões de código mais consistentes e oferecer uma boa experiência. No entanto, como os LLMs são probabilísticos, talvez as sugestões sejam diferentes das apresentadas no workshop. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar um comando MCP diferente, mas ainda deverá conseguir concluir a tarefa.

3. Peça ao Copilot para mostrar o backlog usando o prompt:

   ```plaintext
   Mostre o backlog de itens do meu repositório do GitHub. Ajude-me a priorizá-los com base na utilidade para quem usa o produto.
   ```

4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista de issues gerada.

Observe como o Copilot também priorizou os itens de acordo com o que considera mais útil para as pessoas usuárias.

## Revise os arquivos de instruções

Antes de pedir ao agente para gerar o código, revise o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como adicionar funcionalidades ao site.

1. No Codespace, acesse `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades:
   - uma visão geral da arquitetura;
   - princípios de design de componentes, testabilidade e acessibilidade;
   - links para arquivos de instruções de tipos de arquivo, incluindo:
     - Astro;
     - Tailwind CSS;
     - Drizzle.

> [!TIP]
> Os arquivos de instruções podem referenciar outros arquivos de instruções e arquivos do projeto. Os caminhos são relativos ao arquivo de instruções. Assim, é possível reutilizar instruções, dividir conjuntos complexos em partes menores e usar exemplos e modelos.

## Implemente os filtros

Para concluir a funcionalidade de filtros, são necessárias pelo menos três alterações separadas no aplicativo:

- adicionar ou aprimorar a lógica restante dos filtros na camada de dados (`src/lib/`);
- adicionar ou atualizar testes do comportamento dos filtros;
- atualizar a página de listagem de jogos para incluir a interface de filtros.

Além disso, os testes precisam ser aprovados antes da mesclagem. O modo de agente do Copilot pode executar essas tarefas. Vamos adicionar a funcionalidade.

1. Continue a conversa atual com o Copilot ou selecione **New Chat** para iniciar outra conversa.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Captura de tela da seleção do arquivo de instruções da interface](../../../_images/ex3-select-instructions-file.png)

3. Confirme se **Agent** continua selecionado na lista de agentes da exibição Chat.

   ![Captura de tela do seletor de agentes na exibição Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirme se **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot para implementar a funcionalidade com base na issue relacionada do backlog:

   ```plaintext
   Atualize o site para incluir filtros por editora e categoria conforme os requisitos da issue relacionada no backlog do GitHub. O helper de editoras criado no exercício anterior já existe; preserve e refine esse trabalho conforme necessário ao concluir as alterações restantes na camada de dados, na interface e nos testes. Confirme se todos os testes foram aprovados antes de concluir. O servidor já está em execução, portanto não é necessário iniciá-lo.
   ```

6. Observe o Copilot explorar o projeto e localizar os arquivos relacionados à funcionalidade, incluindo os helpers da camada de dados, a interface e os testes. Em seguida, ele começará a modificar arquivos e executar testes.

   ![Captura de tela do Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, modificar arquivos e executar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, talvez apareçam os botões **Keep** e **Undo** no editor. Quando o Copilot terminar, você verá esses botões para todas as alterações; não é necessário selecioná-los enquanto o trabalho estiver em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Captura de tela de uma caixa de diálogo no painel Copilot Chat pedindo confirmação para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Pode haver pausas e até falhas em alguns testes durante o processo. Isso é normal. O Copilot alterna entre a geração de código e os testes até concluir a tarefa sem detectar erros.

   ![Captura de tela de uma sessão completa no modo de agente do Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado e procure possíveis problemas.

> [!CAUTION]
> Lembre-se: é sempre importante revisar o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Volte ao navegador com o site em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela Copilot Chat.

## Resumo e próximos passos

Parabéns! Neste exercício, exploramos como usar o modo de agente do GitHub Copilot para adicionar recursos ao site da Tailspin Toys. Aprendemos como:

- o modo de agente do GitHub Copilot implementa funcionalidades na interface e na camada de dados;
- o modo de agente explora o projeto, identifica arquivos relevantes e coordena as alterações;
- revisar as alterações e os testes gerados antes de mesclá-los à base de código.

Agora vamos [testar a funcionalidade com o servidor Playwright MCP][next-lesson] e abrir um pull request.

### Exercício extra de exploração — implemente a paginação

À medida que a lista de jogos cresce, será necessário oferecer paginação. Use o que aprendeu neste exercício para pedir ao Copilot que adicione a paginação ao site. Algumas considerações:

- siga as práticas recomendadas existentes, incluindo o uso dos arquivos de instruções;
- decida como implementar a paginação: permitir que a pessoa usuária escolha o tamanho da página ou defini-lo no código;
- no prompt, forneça as orientações necessárias para que o Copilot implemente o comportamento desejado;
- talvez seja necessário iterar com o GitHub Copilot, pedir alterações e fornecer contexto. Esse é o fluxo normal de trabalho com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
