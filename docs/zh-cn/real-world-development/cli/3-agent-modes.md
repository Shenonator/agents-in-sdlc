---
title: "课程 3 - 代理模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案，使用 Autopilot 根据 issue 实现筛选功能，并在 Interactive 模式下审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们刚刚为项目添加了一项小功能，但更大的更改需要更完善的流程。GitHub Copilot CLI 可以配合组织现有的工作流程，帮助我们以正确的方式构建正确的内容。本系列课程将带你体验典型的代理驱动开发流程：从 issue 创建新功能，确保代码有效、功能符合预期，最终成功合并到项目中。

> [!NOTE]
> 在后续功能工作流中，你会继续使用同一对话和分支。通常，不同文件类型会使用不同分支或 PR；此处为了聚焦核心概念，会采用简化做法。

本课将介绍如何：

- 根据 GitHub issue 开始新的 Copilot 对话。
- 在 Plan 模式下确定需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续实现此功能时，你还将更新存储库说明、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 代理并创建功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访问者需要按类别和发行商筛选游戏。待办 issue 描述了该功能，但在编写代码前，需要先确定如何组合多个类别等细节。你将使用 Plan 模式确定这些决策，然后批准一个范围明确的 Autopilot 实现。

## 背景

将 AI 编码代理引入开发流程，并不会改变基本原则。事实上，这些原则变得更加重要！大多数开发人员遵循的流程大致如下：

1. 从描述需求的已提交 issue 开始。
2. 制定构建方案。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但大多数流程都与上述方式类似。

遵循这一标准流程，可以确保 AI 生成的代码符合既定要求，并经历与手写代码相同的审核流程。

## 对话模式

**对话模式**控制代理的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与代理协作。代理提出更改建议，并等待你的输入后再继续。
- **Plan**：代理先制定计划，但不能编辑项目文件。
- **Autopilot**：代理自主工作，包括编写代码、运行测试并不断迭代，直至完成任务。

先从 Plan 模式开始并审查计划，然后使用 Autopilot 实现计划。

## 从 issue 开始

开始筛选功能之前，先返回 Codespace，确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有 issue。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的 issue。
9. 按 <kbd>C</kbd> 将 issue 添加到提示中，然后返回 **Session** 选项卡。

注意提示现在以 `#7`（或类似编号）开头。`#` 可将 GitHub 上的 issue 或拉取请求（PR）加入上下文。

## 规划筛选功能

规划可以先确定功能实现方式或任务处理方案，再交给 Copilot。对于复杂任务，花些时间规划总是有帮助的。现在切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的 issue 引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会先探索项目，再确定最佳方案并制定计划。

3. Copilot 可能会询问筛选功能应如何工作。根据自己的偏好回答，没有错误答案！
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动审查计划。
6. 如果计划中的任何部分不符合你的决定，请让 Copilot 修改。

## 批准 Autopilot

计划编写并审查完毕后，就可以开始实现了！让 Copilot 使用 Autopilot 模式完成工作。

Autopilot 会让 Copilot 迭代处理问题，直至它认为任务已经完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前安装版本中含义相同的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 审查并验证实现

代码生成后，和其他代码一样，合并前需要审查。现在检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 将结果与 issue 以及规划时做出的决定进行比较。
4. 审查完代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并要求 Copilot 解决任何失败。

## 试用新功能

代码看起来没问题，但实际运行如何？像之前一样启动应用，然后通过 Codespaces 转发端口打开网站。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及计划中约定的筛选组合。
4. 确认重置和无结果时的行为符合 issue 和你的决定。
5. 返回 Codespace，让 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你已使用不同对话模式构建并审查功能。本课中，你：

- 根据 GitHub issue 开始新的 Copilot 对话。
- 在 Plan 模式下确定需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

接下来，使用[自定义说明][next-lesson]，深入了解如何生成符合文档化实践的代码。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
