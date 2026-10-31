---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理代理][previous-lesson] |
|:--|

在整个实验中，你使用 GitHub Copilot 处理了多个任务，以改善用户体验并添加功能。你让 Copilot 为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现包括高对比度和浅色模式切换开关在内的无障碍功能。现在一起查看代码更改，必要时向 Copilot 提供反馈，改进其工作，然后为团队启用审查。

### 场景

正如前面多次强调的那样，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循正常的 DevOps 流程。接下来，在为团队启用审查之前，先检查 GitHub Copilot 对文档、相关游戏功能和无障碍功能的建议。

## 安全与 GitHub Copilot 云代理

Copilot 云代理会在没有监督的情况下异步执行任务，因此设置了一些安全限制，以确保工作安全：

- Copilot 只能读取存储库，并且只能写入它用于工作的分支。
- 云代理在 GitHub Actions 中运行，并在单独的临时环境中工作。
- 运行任何 GitHub Actions 工作流都必须由人工批准。
- [默认情况下，外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先查看 GitHub Copilot 云代理创建的第一个拉取请求（PR），其中包含为代码添加的文档。此处会使用 GitHub.com 上的标准 PR 界面。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为默认情况下 Copilot 只能有限访问外部资源，包括外部 MCP 服务器。如有需要，可以[自定义或禁用 Copilot 云代理防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests** 打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作。这可能需要几分钟，可以休息一下或回顾所学内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新更新的代码，其中包括 TSDoc 文档注释和其他文档。具体更改可能有所不同。查看时，检查 TSDoc 文档注释、TypeScript 规范和注释头。这些内容来自练习 2 开始时审查的自定义说明文件。
6. 审查完成且确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流提示。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会在拉取请求的检查区域中排队运行。如果一切顺利，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 更改

在拉取请求中与 Copilot 协作并非单向过程。你可以像对待团队成员一样，在 PR 评论或代码行内评论中标记 Copilot。Copilot 会读取这些评论并启动新会话来处理反馈。由于生成结果具有不确定性，无法给出固定提示，但可以让 Copilot：

- 在每个代码文件顶部添加简要说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，说明 Astro 应用结构。

1. 添加评论并标记 **@copilot**，请求其更改生成的文档。可以使用上面的建议，也可以提出其他文档要求。
2. 选择 **View Session** 查看 Copilot 的工作过程。注意 Copilot 会启动新会话来进行更新。
3. 选择 **Back to pull request** 返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中会出现新的提交。
5. 选择 **Files changed** 选项卡，审查更改。

你可以继续迭代，直到满意为止。完成后，可以将 PR 从草稿状态改为准备审查，然后合并到 main 分支。

![Convert PR to ready](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在查看 Copilot 为解决游戏详情页显示相关游戏问题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查完成且确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有等待批准的工作流提示。
7. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流应会在 PR 的检查区域中排队运行。如果一切顺利，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。
9. **可选：**可以在 Codespace 中切换到此分支，手动测试相关游戏功能。转到 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能！与之前一样，可以通过请求更新来迭代改进云代理的工作，例如调整显示的相关游戏数量，或要求添加注释头和 TSDoc 文档注释（请记住，分配此任务时，你尚未更新自定义说明）。和之前一样，可以在 **Conversation** 选项卡中发表评论提出要求，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义代理实现的功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 中在 mission control 请求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，重点检查：
   - 用于切换模式的 UI 组件。
   - 使用本地存储保留用户偏好的方式。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。
6. 审查完成且确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流提示。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会在 PR 的检查区域中排队运行。如果一切顺利，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到此分支，手动测试无障碍功能。转到 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换开关，并确认它们可以正常运行且在重新加载页面后仍然生效。

注意无障碍自定义代理如何引导 Copilot 遵循无障碍最佳实践来实现这些功能。如果发现无障碍方面的问题或改进点，可以像处理之前的 PR 一样，在评论中标记 **@copilot** 并请求更新。

## 可选练习 — 继续委派任务

云代理最适合处理待办列表中的真实任务，让你可以继续处理其他工作。试着养成这种习惯：为存储库创建几个 issue 并分配给 Copilot。可以尝试以下任务：

- 在游戏详情页创建支持者兴趣表单。
- 在游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云代理工作环境中的练习。在这些课程中，你：

- **检查了此存储库附带的自定义说明文件**，以便之后观察云代理如何使用它们。
- **将 issue 分配给 Copilot 云代理**，并观察它设置环境、制定计划和异步执行任务。
- **创建并使用自定义代理**来处理无障碍需求，并添加高对比度和浅色模式切换开关。
- **使用 Copilot 代理页面作为 mission control**，监控并引导无障碍会话。
- **审查并迭代改进云代理的拉取请求**，标记 `@copilot` 请求更改并批准工作流。

## 审查和后续步骤

你已完成云代理工作环境的练习。如果想继续探索，其他工作环境可以补充所学内容：

- 🖥️ **[VS Code 工作环境](../../vscode/)** — 在 IDE 中体验 Copilot Chat Agent 模式和 MCP 集成。
- 💻 **[CLI 工作环境](../../cli/)** — 在终端中使用 Plan 和 Autopilot 模式、代理技能、自定义 QA 代理、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云代理桥接，交付两个经过审查的更改。

在自己的存储库中，还可以尝试以下后续任务：

- 将重构或测试覆盖率 issue 分配给云代理。
- 为其他领域创建自定义代理。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 代理][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot 云代理设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云代理防火墙][agent-firewall]

---

| [← 上一课：管理代理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
