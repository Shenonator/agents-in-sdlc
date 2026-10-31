---
title: "练习 4 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是针对开发工作流中特定任务或领域定制的 AI 助手。通过在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义代理，可以提供专门的说明、最佳实践、编码模式和领域知识，帮助 Copilot 更有效地完成特定类型的工作。团队可以将专业知识编写成可重复使用的代理，例如确保符合 [WCAG][wcag] 要求的无障碍代理、遵循安全编码实践的安全代理，或保持测试模式一致的测试代理。

自定义代理通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在 `~/.copilot/agents` 中全局定义。每个文件都包含 YAML frontmatter，其中至少要有 `name` 和 `description`，后面是定义代理行为、专业知识和说明的 Markdown 提示。

### 自定义代理与代理技能

自定义代理和[代理技能][agent-skills-concept]在概念上有些重叠。两者主要通过 Markdown 文件定义，都会指导 AI 如何执行操作。最简单的区分方式是：**自定义代理是工作者，技能是工具**。

自定义代理有独立的上下文窗口，能够编排技能（甚至其他代理）来完成工作。在本实验中，无障碍自定义代理会依据无障碍指南审查并更新网站；在此过程中，它可以调用拉取请求工作流技能，或运行和管理测试的技能。

> [!NOTE]
> 编写自定义代理没有唯一的正确方式。与 AI 的其他用法一样，需要在自己的环境和场景中不断测试和迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，由于文本与背景颜色对比度不足，有些用户难以阅读当前的暗色主题。为解决这一无障碍问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

无障碍功能至关重要，因此希望尽快完成这项功能。接下来会使用自定义代理生成相应功能。

本练习将介绍如何：

- 审查现有的无障碍自定义代理。
- 在 Copilot Chat 中使用无障碍代理来实现高对比度模式。

## 审查无障碍自定义代理

我们已为你创建了一个无障碍自定义代理。现在查看其内容，了解它将如何引导 Copilot。

返回 codespace，打开终端，并从 `main` 创建新分支来实现无障碍功能（筛选 PR 将保留在独立分支中）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 注意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` frontmatter。

3. 接下来浏览其他章节，查看其中列出的内容：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见问题和错误列表。

## 在 Copilot Chat 中使用自定义代理

VS Code 会在 Copilot Chat 视图底部的代理下拉列表中显示 `.github/agents` 中定义的所有自定义代理。选择代理后，会将聊天会话限制在该代理的说明和工具范围内。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。不同练习所用的模式和模型可能不同；每个步骤会在适用时说明。

1. 如果代理下拉列表尚未选择 **Agent**，请先选择它。

   ![Chat 视图中的代理选择器截图。](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的代理下拉列表（其中显示当前活动的代理，默认是 **default**）。
3. 从可用代理列表中选择 **Accessibility agent**。
4. 向无障碍代理发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 会开始工作：探索代码库、提出更改建议并编辑项目文件。每项编辑都会在聊天中内嵌显示文件路径和差异，供你审查。
6. 随着编辑发生，**Files changed** 指示器会更新，显示代理修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际修改存储库中的文件，例如 Astro 组件、CSS 和相关测试。

下一练习会审查并引导此项进行中的工作。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，也就是针对特定任务和领域定制的 AI 助手。借助自定义代理，可以将团队的专业知识和标准编写成可重复使用的代理，引导 Copilot 更有效地完成特定类型的工作。

你已了解以下概念：

- 如何定义自定义代理。
- 如何在 Copilot Chat Agent 模式中使用自定义代理。

接下来，[监控并引导代理的工作][next-lesson]，在更改生成时进行审查，并在同一会话中添加浅色模式切换开关。

## 资源

- [关于自定义代理][custom-agents]
- [在 IDE 中创建自定义代理][creating-custom-agents-ide]
- [VS Code 中的自定义代理][custom-agents-vscode]
- [自定义代理配置][custom-agents-config]
- [awesome-copilot 中的自定义代理][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
