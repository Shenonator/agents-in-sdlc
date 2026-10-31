---
title: "练习 4 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 Copilot 云代理处理了三项任务，重点是改善用户体验和添加功能。云代理虽然设计为异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云代理的任务，包括 GitHub.com 上的[代理页面][agents-page]。在这个 mission control 中，可以查看所有具有未关闭拉取请求（PR）的代理任务、了解它们执行的操作，甚至引导进行中的会话。

本课将介绍如何：

- 浏览代理页面并监控云代理任务。
- 引导正在进行的会话，让代理添加其他功能。

## 场景

将任务分配给代理创建高对比度模式后，团队意识到也可以借此添加浅色模式。既然正在更新网站样式并添加切换功能，加入浅色模式也很合理。现在需要引导代理，确保它同时添加浅色和高对比度模式。

## 查看 Copilot 云代理任务

查看当前分配给 Copilot 云代理的任务状态。

1. 转到 [Copilot 代理页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。列表应包含已分配给 Copilot 的任务，例如：
    - 更新代码文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一项正在运行的任务，查看 Copilot 已执行的操作，例如：
    - 从存储库检出代码。
    - 创建 Copilot 的工作环境。
    - 设置 MCP 服务器。
    - 执行完成所分配任务的各个步骤。

> [!NOTE]
> 具体显示的步骤取决于 Copilot 当前的工作状态和所采用的方法。

4. 同时注意右侧显示的拉取请求窗格。可以在其中查看 PR 和已更改的文件，以便进一步监控。

## 引导云代理

现在已经了解活动任务，接下来让 Copilot 在实现高对比度模式的同时，也添加浅色模式切换开关。

1. 选择添加高对比度模式的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

    ![Mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到会话显示已完成设置并开始工作。此时会出现类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中，添加以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot 代理页面上的任务截图，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 完成工作

与之前一样，Copilot 会开始处理更新后的任务！它会先完成发送消息时正在执行的具体步骤，然后将新要求加入工作流程。

这同样需要几分钟，正好可以暂停一下，回顾至今学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 代理页面，这是监控和引导 GitHub Copilot 云代理任务的中心位置。借助此 mission control，可以跟踪所有活动和已完成的任务、审查代理正在执行的工作，还可以重定向进行中的任务，调整范围或提供额外指导。

你已了解以下概念：

- 浏览 mission control 和代理页面，监控云代理任务。
- 重定向正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能，接下来可以[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 代理页面][agents-page]
- [关于自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
