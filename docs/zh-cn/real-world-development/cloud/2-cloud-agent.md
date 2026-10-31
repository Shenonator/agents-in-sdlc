---
title: "练习 2 - GitHub Copilot 云代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都面临技术债务问题。这可能是尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云代理专为更新代码、添加功能等任务而设计，并可自主完成工作。代理完成任务后，会创建一个草稿 PR，供开发人员审查。这样可以将繁琐任务交给代理，加快开发流程，让开发人员专注于更重要的事项。

本练习将介绍如何使用 Copilot 云代理：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 了解明确界定 issue 范围的重要性。
- 将 issue 分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇开发网站的承包商没有留下文档——事实上，代码完全缺少文档。首先，他们希望为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队准备改进游戏发现功能。他们希望在每款游戏的详情页中显示相关游戏，也就是同一类别中的其他游戏。暂时不需要完善的实现，只需要一个可用于 UX 验收测试的版本。目前这项工作受阻，但其他 issue 的优先级更高。

这两类任务都很容易被不断推迟，适合交给 Copilot 云代理。它可以异步处理任务，让开发人员先处理其他工作，之后再回来审查 Copilot 的成果并确认一切符合预期。

## 认识 GitHub Copilot 云代理

[GitHub Copilot 云代理](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像开发人员一样在后台处理任务。你可以通过多种方式启动任务，例如[将 GitHub issue 分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求来跟踪进度、设置环境并开始处理任务。你可以在会话进行期间或完成后查看详情。解决方案准备好审查时，Copilot 会在拉取请求中通知你。

## 明确界定任务范围的重要性

GitHub Copilot 并非魔法——使用它并不意味着可以跳过思考。即使是看似简单的操作，深入分析后也会发现其复杂性，因此请留意[如何界定云代理任务的范围][cloud-agent-best-practices]。把它当作 AI 结对编程伙伴：分阶段工作、学习、试验并根据结果调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 此存储库中的自定义说明

前面的练习介绍了自定义说明及其如何引导 Copilot。如果你已完成自定义说明练习，就已经见过 `.github/copilot-instructions.md` 的作用；如果还没有，现在可以快速查看。将工作分配给 Copilot 云代理前，先只读查看此存储库中的说明文件，之后就能观察它们的影响。

在存储库的 GitHub Web 界面中打开以下文件；如果已启动 codespace，也可以在那里查看：

- `.github/copilot-instructions.md`：查看 **Code standards** 章节，尤其是 TypeScript 规范和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将这些说明限定为 `**/*.test.ts` 文件。

在下一节将 `Code lacks documentation` issue 分配给云代理时，观察生成的拉取请求是否采用了 TSDoc 文档注释、TypeScript 规范和文件注释头。稍后的练习还会再次介绍这些说明文件。

## 为 Copilot 云代理设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，确保环境就绪。将任务分配给 Copilot 时也是如此，因为它会像软件工程师一样处理任务。

云代理使用 [GitHub Actions][github-actions] 环境。你可以创建[特殊设置工作流][setup-workflow]，在 `.github/workflows/copilot-setup-steps.yml` 中进行配置，让它在开始工作前运行。这样可以为代理提供所需的开发工具和依赖项。实验已预先配置好工作流，让流程更顺畅，并帮助你了解这一功能。它确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及单体 Astro 应用已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

此工作流与其他 GitHub 工作流类似，但有几个重要之处：

- 它包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求之前，GitHub Actions 会运行此作业。
- 注意 `workflow_dispatch` 触发器，它允许你在存储库的 **Actions** 选项卡中手动运行工作流。这样可以测试工作流是否成功，而不必等待 Copilot 启动它。

## 添加文档

每个人都知道文档的重要性，但大多数项目的信息要么过时，要么完全缺失。这类技术债务常常无人处理，降低效率，也让维护代码库或引入新开发人员变得更加困难。Copilot 擅长创建文档，因此可以把这项工作交给 Copilot 云代理。它会在后台生成所需文档，稍后的练习中你将审查结果。

1. 在新的浏览器选项卡中转到 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
4. 选择 **Blank issue** 创建新 issue。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

  ![将 issue 分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新创建的拉取请求（PR），其标题类似 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 的待办事项列表可能需要几分钟才会显示在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 查看列表及其计划完成的任务。
14. 向下滚动拉取请求时间线，查看 Copilot 已开始处理 issue 的通知。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码、了解现状、暂停思考并确定合适的方案，以及编写代码。

这通常需要几分钟。Copilot 云代理的主要目标之一是异步处理任务，让我们可以继续处理其他工作。接下来，我们会利用这一点：再分配一项任务，然后编写代码，为应用添加功能。

## 在游戏详情页添加相关游戏区域

如前所述，GitHub Copilot 云代理的一大优势是可以分工，你可以处理一组任务，而代理负责另一组任务。即使为设计团队添加相关游戏区域不需要很长时间，这段时间也可以用于其他工作。现在就把任务分配给 Copilot 云代理。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

  ![将 issue 分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

很快，issue 的第一条评论中就会出现一组 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理 issue](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求！云代理的工作方式类似软件工程师，因此不需要持续监控；等它完成后再审查即可。接下来，我们来创建和使用自定义代理。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云代理][copilot-agents]。可以将 issue 分配给云代理，让它异步处理。你可以用 Copilot 处理技术债务、创建新功能，或协助将代码从一个框架迁移到另一个框架。

你已了解以下概念：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 明确界定 issue 范围的重要性。
- 将 issue 分配给 Copilot。

云代理正在后台工作，现在可以开始创建和使用自定义代理。[Copilot 云代理还可以使用 MCP 服务器][cloud-agent-mcp]，并能使用前面模块介绍的自定义说明。

## 资源

- [关于 Copilot 云代理][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot 云代理设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义代理 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
