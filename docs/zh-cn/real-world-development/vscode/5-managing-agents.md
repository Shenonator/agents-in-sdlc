---
title: "练习 5 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义代理][previous-lesson] |
|:--|

启用 GitHub Copilot Agent 模式后，它会自主探索代码库、提出更改建议、编辑文件并运行命令。由于这些工作实时发生在你的计算机上，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以内嵌审查每个差异、接受或拒绝单项更改，并通过后续提示引导对话、完善或扩展工作，而无需离开编辑器。

本练习将介绍如何：

- 实时监控无障碍代理的工作。
- 审查并接受建议的文件编辑。
- 在同一会话中引导代理添加浅色模式切换开关和高对比度模式。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义代理正在实现高对比度模式。工作进行期间，设计团队又提出了一个要求：为觉得默认主题太暗的用户添加浅色模式切换开关。无需从头开始新对话，你可以引导当前会话扩展工作范围。

## 监控 Copilot 进度

Copilot Chat 视图会显示代理执行的每个步骤，包括探索的文件、运行的命令和提出的编辑。每项文件编辑都会在聊天中内嵌显示差异及 **Keep** / **Undo** 控件。

1. 返回练习 4 中的 Copilot Chat 视图。
2. 代理工作期间滚动聊天内容，查看每次工具调用。你会看到文件读取、文件编辑，以及它请求运行的终端命令。
3. 代理提出文件编辑时，差异会内嵌显示：
    - 选择 **Keep** 接受更改。
    - 选择 **Undo** 撤销更改，让代理重新尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看代理在此会话中修改的完整文件列表。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），代理会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待代理完成高对比度模式。完成后应会显示总结所做更改的消息。

## 引导代理添加浅色模式

高对比度模式就绪后，在同一对话中继续添加浅色模式切换开关。继续使用同一会话时，代理会保留之前工作的上下文，包括修改的组件、样式方案和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 代理会从之前的工作继续，探索现有更改并提出浅色模式更新。
3. 与高对比度模式一样，逐项审查新编辑，并使用 **Keep** 或 **Undo**。
4. 如果代理的方向不符合预期，例如想以你不喜欢的方式重构切换 UI，可以选择聊天输入框旁的 **Stop** 按钮停止它，然后发送澄清提示。

> [!TIP]
> 引导进行中的会话比从头开始更快。可以使用后续提示请求调整（“简化 CSS——使用 CSS 变量，而不是重复选择器”）、纠正方向（“不要添加第三个切换开关，将浅色模式整合到现有设置面板中”），或扩展范围（“为两个切换开关添加键盘快捷键”）。

## 审查工作集

提交工作前，快速检查代理修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查更改文件的完整列表。应能看到 Astro 组件、样式和相关测试的更新。
3. 打开几个更改文件并查看差异。确认其中体现了自定义代理的无障碍模式，包括 ARIA 属性、键盘导航、语义 HTML 和本地存储持久化。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结和后续步骤

你已使用 Copilot Chat Agent 模式监控并引导进行中的会话：逐项审查生成的文件编辑、接受或拒绝更改，并通过后续提示扩展工作，同时保留上下文。

本课中，你：

- 实时监控代理的工作。
- 接受或拒绝单项文件编辑。
- 使用后续提示引导进行中的会话。

在[下一课][next-lesson]中，你将通过拉取请求发布无障碍功能，并审查本地自定义代理在整个实验中生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用 Agent 模式][agent-mode]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
