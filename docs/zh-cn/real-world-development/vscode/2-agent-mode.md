---
title: "练习 2 - 使用 Copilot Agent 模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行测试等操作。开发人员通常需要查找所有相关文件、进行更改、运行测试、调试、找出遗漏的文件并再次更新……工作往往会不断延伸。

Copilot Agent 模式正是为此而设计的。

Copilot Agent 模式可以在 IDE 中更自主地工作。它的行为类似开发人员：先探索现有项目结构，再执行必要更新、运行测试等任务，并自动修复发现的问题。现在来了解如何使用 Agent 模式为网站添加新功能。

> [!NOTE]
> Agent 模式和云代理名称相似，但适用于不同的体验。Agent 模式在 IDE 中执行任务，可快速反馈并进行交互；云代理则像团队成员一样异步工作，通过 issue 和拉取请求与你协作。

本练习将介绍如何：

- 让 Copilot Agent 模式探索项目、查找相关文件并协调更改。
- 使用 GitHub Copilot Agent 模式跨 UI 和数据层实现新功能。
- 在将代码合并到代码库之前，审查 Agent 模式生成的更改和测试。

## 场景

游戏列表不断扩大，你希望允许用户按类别和发行商筛选。上一练习已添加发行商辅助函数，现在将使用 Copilot Agent 模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

修改之前，先探索 Tailspin Toys 网站，了解其当前功能。

该网站是一个采用开发者主题的棋盘游戏众筹平台，允许用户浏览游戏列表并查看游戏详情。它是一个单体 Astro 应用，构建时会将页面渲染成静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库；没有独立后端 API 或客户端 UI 框架。可重复使用的数据访问辅助函数位于 `src/lib/`，交互功能通过少量范围明确的 Astro `<script>` 和标准 DOM API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单体 Astro 开发服务器。可以按以下步骤在 GitHub Codespace 中运行：

1. 返回 codespace。上一练习结束后，当前应位于 `main` 分支。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），在 Codespace 中打开新的终端窗口。
3. 创建并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下横幅的 URL：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否要在浏览器窗口中打开 `http://localhost:4321`，选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），并选择终端中显示的 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 codespace 时，在 Codespace 终端中选择 localhost URL 会自动将你重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 codespace 的私有隧道，网站服务器现在通过它提供服务。

### 探索网站

网站运行后，可以探索其功能。主要功能包括：

- **主页**：显示棋盘游戏列表，包括标题、图片和说明。
- **游戏详情页**：选择游戏后，会打开详情页，其中包含标题、说明、发行商和类别等信息。

## 使用 Copilot 探索待办列表

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。不同练习所用的模式和模型可能不同；每个步骤会在适用时说明。

网站的初始实现已可正常工作，但还需要添加新功能。先查看待办列表。根据模板设置项目时，系统已自动创建一组 issue；现在让 GitHub Copilot 显示这些 issue。

1. 在 Chat 视图的代理下拉列表中选择 **Agent**。Agent 代理可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的代理选择器截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写研讨会时使用的是 Claude Sonnet 4.5，因此步骤中列出了该模型，希望由此获得相对一致的代码建议。但由于大型语言模型具有不确定性，你收到的建议可能与研讨会示例不同，这是正常且符合预期的情况。

> [!NOTE]
> 由于大型语言模型的输出具有不确定性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问 issue 待办列表：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 运行列出所有 issue 的命令。
5. 审查生成的 issue 列表。

注意 Copilot 还会根据它认为对用户最有用的程度，为这些项目排定优先级。

## 审查说明文件

让代理生成代码之前，先审查用于为 Copilot 提供上下文的说明文件。接下来会使用[用户界面（UI）](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)说明文件，其中介绍了如何为网站添加功能。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看添加功能的整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向各类文件说明的链接，包括 Astro、Tailwind CSS 和 Drizzle。

> [!TIP]
> 说明文件可以引用其他说明文件和项目中的文件。路径相对于说明文件所在位置，便于重复使用内容、将复杂说明拆分成更小的部分，并提供示例和模板。

## 实现筛选功能

要完成筛选功能，至少需要对应用做三项单独更新：

- 在数据层（`src/lib/`）添加或完善剩余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选 UI。

此外，合并前还需要运行并通过测试。Copilot Agent 模式可以为你完成这些任务。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 说明文件。

   ![选择 UI 说明文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的代理下拉列表中仍选择了 **Agent**。

   ![Chat 视图中的代理选择器截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，让 Copilot 根据待办列表中相关 issue 实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并查找相关文件。它应该会找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 正在探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。在此过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会有 **Keep** 或 **Undo** 按钮，因此无需在工作进行期间选择它们。

7. 按 Copilot 的提示选择 **Continue** 运行测试。

   ![Copilot Chat 窗格中的对话框截图，提示用户确认是否运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会出现暂停，也可能有测试失败。这没有关系！Copilot 会在代码生成和测试之间反复迭代，直至完成任务且不再检测到错误。

   ![Copilot Agent 模式完整 Chat 会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 请始终审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行的网站浏览器，试用新功能！
11. 确认一切正常并完成代码审查后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot Agent 模式为 Tailspin Toys 网站添加新功能。你已了解如何：

- 使用 GitHub Copilot Agent 模式跨 UI 和数据层实现新功能。
- 让 Copilot Agent 模式探索项目、查找相关文件并协调更改。
- 在将代码合并到代码库之前，审查 Agent 模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]并创建拉取请求。

### 额外探索练习——实现分页

游戏列表不断扩大后，需要启用分页。使用本练习学到的技能，让 Copilot 为网站添加分页。可以考虑以下事项：

- 遵循现有最佳实践，包括使用已有的说明文件。
- 考虑分页的实现方式，例如是否允许用户选择每页大小，还是固定大小。
- 编写提示时，提供必要指导，让 Copilot 按照你的预期实现功能。
- 可能需要与 GitHub Copilot 迭代沟通、请求更改并提供上下文。这是与 Copilot 协作的常见方式。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的 Agent 模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
