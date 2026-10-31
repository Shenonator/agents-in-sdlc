---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot 云代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot 云代理](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** 可在云端异步处理任务。你在 GitHub 上分配工作后，云代理会在后台接手任务，探索存储库、进行更改并创建拉取请求；此时你可以继续处理其他事情。

在这一系列练习中，你将添加云代理要遵循的自定义说明，然后分配一个 GitHub issue，让它实现相应工作。你还将审查和使用自定义代理来引导它的处理方式，从代理仪表板监控并引导会话，最后审查其拉取请求并迭代改进结果。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 codespace |
| [1. 自定义说明][ex1] | 上下文 | 添加云代理要遵循的自定义说明 |
| [2. 云代理][ex2] | 异步代理 | 将 issue 分配给 Copilot 云代理 |
| [3. 自定义代理][ex3] | 专用代理 | 审查并使用自定义代理 |
| [4. 管理代理][ex4] | 监控 | 监控并引导代理会话 |
| [5. 迭代][ex5] | 审查 | 审查 PR、迭代改进 Copilot 的工作并选择后续步骤 |

## 先决条件

参加本研讨会之前，请确保具备以下条件：

- [ ] 拥有有效的 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 方案的 GitHub 帐户
- [ ] 已为帐户或组织启用 Copilot 云代理

> [!TIP]
> 没有付费方案？通过 [GitHub Education][callout-student-plan-education] 验证身份的学生可以免费使用 GitHub Copilot。**Copilot Student** 方案包含本研讨会所用的代理、MCP、代码审查和 Copilot CLI 功能，因此可用于完成所有工作环境中的练习。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Copilot Student、Pro、Pro+、Business 和 Enterprise 均可使用云代理；只有 Copilot Free 方案不支持。在 Copilot Business 或 Enterprise 中，管理员必须为组织启用此功能。

> [!NOTE]
> 本工作环境不涉及 MCP。若要了解如何将 MCP 服务器与 Copilot 配合使用，请参阅 [CLI 工作环境](../cli/)或 [VS Code 工作环境](../vscode/)。

## 开始

**[从练习 0：先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
