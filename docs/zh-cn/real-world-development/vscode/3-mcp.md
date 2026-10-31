---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：Agent 模式][previous-lesson] · [下一课：自定义代理 →][next-lesson]

你刚使用 Agent 模式构建了筛选功能。创建拉取请求之前，应确认它在浏览器中确实可以正常工作。你无需亲自在应用中逐项操作，而是可以连接 **Playwright MCP 服务器**，让 Copilot 在真实浏览器中测试该功能，然后发布分支并创建 PR。

本练习将介绍如何：

- 了解 Model Context Protocol (MCP) 及 MCP 服务器如何通过新工具扩展 Copilot。
- 将 Playwright MCP 服务器添加到工作区。
- 让 Copilot 使用它在浏览器中手动测试筛选功能。
- 发布分支并为筛选功能创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

Agent 模式可以使用编辑器之外的工具，功能会强大得多。Copilot 使用 Model Context Protocol (MCP) 连接外部工具和服务；它是一种让代理与外部工具和服务通信的标准方式。

![示意图显示 Agent 模式的内部工作方式，以及它如何与上下文、LLM 和工具交互，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 代理提供了与外部工具和服务通信的方式。通过 MCP，AI 代理可以实时连接外部工具和服务，使用资源访问最新信息，并使用工具代表你执行操作。

AI 代理通过 MCP 服务器访问这些工具和资源。MCP 服务器充当代理与外部工具和服务之间的桥梁，并负责管理代理与外部工具之间的通信，例如现有 API 或 NPM 等本地工具。每个 MCP 服务器都代表 AI 代理可以访问的一组工具和资源。

目前有一些广受欢迎的 MCP 服务器：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供用于管理 GitHub 存储库的 API，让 AI 代理可以创建新存储库、更新现有存储库，以及管理 issue 和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化功能，让 AI 代理可以访问网页、填写表单和选择按钮。

此外还有许多提供不同工具和资源的 MCP 服务器。GitHub 托管了 [MCP 注册表](https://github.com/mcp)，帮助社区发现和贡献生态系统中的内容。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用之前，请仔细审查其源代码、验证发布者，并考虑安全影响。只使用信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 审查 MCP 配置

`.vscode/mcp.json` 文件用于配置此 VS Code 工作区可使用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 你应该会看到已配置的 `github` 服务器：

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

项目模板附带了此 `github` 配置，因此 Copilot 能在上一练习中读取 issue 待办列表。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装。VS Code 会通过 HTTP 连接到服务器；Copilot 首次使用其工具时，你需要通过 GitHub 身份验证。

## 添加 Playwright MCP 服务器

接下来添加第二台服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，正好适合用来测试功能。

1. 在 `.vscode/mcp.json` 中，在 `github` 旁边添加 `playwright` 配置，使文件如下所示：

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

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在本机运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标记让 Playwright 在没有可见窗口的情况下运行浏览器，这对于没有桌面环境的 codespace 是必需的。

> [!NOTE]
> Tailspin Toys 项目已在端到端测试中使用 Playwright，因此通常已安装 Playwright 所需的浏览器。如果 Copilot 后续报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会按需启动 MCP 服务器——Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. `.vscode/mcp.json` 中的 `playwright` 配置上方会出现 **Start** 操作。选择它以立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请审查配置并选择信任，以便启动服务器。

手动启动是可选的。如果跳过此步骤，VS Code 会在下一步 Copilot 首次需要其工具时启动服务器，并询问你是否信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可操作的真实浏览器。你无需亲自在应用中逐项操作，代理可以打开页面、导航、应用筛选条件并读取结果，然后向你总结观察结果。无需离开对话即可快速确认功能行为是否符合预期。

Playwright MCP 服务器在底层使用页面的[无障碍树][playwright-mcp-server]而不是截图。因此，代理会处理带有结构和标签的元素（如按钮、链接和列表项），与辅助技术的处理方式相同；快速的功能检查也可以顺便验证基本无障碍情况。

连接服务器并启动应用后，让 Copilot 测试刚完成的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、逐步完成操作并报告结果。根据 issue 中的验收标准审查其总结。如果发现异常，请提出后续问题，或让它在创建拉取请求前修复代码。

> [!NOTE]
> 测试时，应用必须在 `http://localhost:4321` 运行。如果已停止开发服务器，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium` 并重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求（PR），让团队审查。首先发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![Source Control 面板显示更改内容的截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行界面（CLI）等多种方式创建拉取请求。既然正在使用 GitHub Copilot，不妨让它代为创建 PR！它可以找到相关 issue 并创建关联的 PR。

1. 打开 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 在代理下拉列表中确认选择 **Agent**，这样 Copilot 才能使用 GitHub 工具。
3. 让 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 收集信息并执行操作。首次使用 GitHub 工具时，系统可能会要求你进行 GitHub 身份验证；按提示授权即可。
5. 观察 Copilot 如何搜索 issue、找到正确的 issue 并创建 PR。
6. 选择 Copilot 生成的链接，审查拉取请求，但暂时**不要合并**。

## 总结和后续步骤

恭喜！本练习中，你：

- 了解了 Model Context Protocol (MCP) 及 MCP 服务器如何通过新工具扩展 Copilot。
- 将 Playwright MCP 服务器添加到了工作区。
- 使用它在交付前通过浏览器手动测试筛选功能。
- 发布分支并为筛选工作创建拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义代理][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 到底是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
