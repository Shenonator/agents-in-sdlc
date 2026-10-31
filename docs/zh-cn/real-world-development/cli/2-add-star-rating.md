---
title: "课程 2 - 添加星级评分：快速改进"
description: "使用 Copilot CLI 修改游戏卡片，在转发的浏览器中审查更改，并将其作为第一个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试对话后，现在可以首次修改项目了。先从小处着手：游戏数据中已有星级评分，但主页上的游戏卡片尚未显示。你将让代理显示评分、审查更改，并将其作为第一个拉取请求合并。

本课将介绍如何：

- 在功能分支中开始有针对性的 Copilot 对话。
- 让代理对项目进行小幅修改。
- 使用 `/diff` 审查更改。
- 运行应用，并在转发的浏览器中确认更改。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，游戏详情页已经显示了评分，但主页上的游戏卡片只显示标题、类别、发行商和说明。作为热身，你将让代理在每张卡片上显示现有评分。这项更改小而独立，非常适合作为第一次会话的练习。

## 对话的组成

**对话**是你使用 Copilot CLI 处理任务的地方。与 Copilot 应用不同，常规 CLI 对话使用终端当前检出的存储库和 Git 分支，而不是创建专用工作树。保存的对话可以让你稍后返回同一讨论，而文件和分支仍是磁盘上的常规 Git 状态。

对话中会显示三类内容：你发送的提示和代理的响应、代理探索和编辑文件时执行的工具操作，以及可通过 `/diff` 检查的更改。

## 开始对话并请求更改

现在开始一个新对话，实现此功能。

1. 返回 Codespace。
2. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未运行，使用以下命令启动：

   ```bash
   copilot --yolo
   ```

4. 使用斜杠命令 `/new` 并按 <kbd>Enter</kbd>，确保开始新的会话。
5. 使用以下提示请求更改：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 会探索项目、查找显示游戏详情所用的文件，并创建所需代码。现在你已使用 Copilot CLI 添加了新功能！

## 审查差异

所有 AI 生成的更改在合并前都应审查，即使更改很小也是如此。现在就在 Copilot CLI 中查看更改。

1. 输入 `/diff` 并检查每个更改的文件。
2. 确认游戏卡片在有评分时显示数字评分，`starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖了这两种情况。
4. 审查 Copilot 运行的检查结果，并要求它修复任何失败。
5. 审查完毕后，按 <kbd>Esc</kbd> 退出差异界面。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 具有概率性而非确定性，因此你的代码可能与示例不同。请审查实际行为，而不是期待完全相同的实现。

## 检查更改

不能只阅读代码并假定它能正常运行。让 Copilot 启动网站，然后在 Codespaces 转发的浏览器中检查更新后的用户界面（UI）。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示了满分为 5 分的评分。
4. 返回 Copilot，让它停止刚才启动的服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 创建并合并第一个拉取请求

现在功能已创建，可以创建拉取请求（PR），将新代码合并到项目中。

1. 让默认代理提交更改：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。Copilot CLI 会在创建 PR 时推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），同时选择 Copilot CLI 显示的 URL，以打开 PR。
4. 审查更改的文件和检查结果。
5. 准备就绪后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，并使用 `/exit` 退出 Copilot CLI。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结和后续步骤

恭喜！你使用 GitHub Copilot CLI 发布了第一个更改！具体来说，你：

- 在功能分支中开始有针对性的 Copilot 对话。
- 引导代理修改游戏卡片。
- 使用 `/diff` 审查更改。
- 运行应用并在转发的浏览器中确认星级评分。
- 创建并合并第一个拉取请求。

接下来，你将从筛选 issue 入手，使用 [Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
