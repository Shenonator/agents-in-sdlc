---
title: "课程 5 - 自定义并使用 quality-checks 技能"
description: "探索现有的 quality-checks 技能、自定义报告格式并用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码不仅仅是编写代码。我们已手动验证代码能够运行，也使用说明文件确保遵循标准。测试、lint 和其他持续集成（CI）环节又该如何处理？

对于这类任务，**代理技能**最为合适！技能可以帮助 Copilot 正确运行这类操作。

本课将介绍如何：

- 探索现有的 `quality-checks` 技能。
- 自定义结果的报告格式。
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，让结果更易阅读。

## 说明、脚本和资源

代理技能将可重复使用的任务说明、可执行脚本和支持资源打包在一起，供代理按需加载。技能的核心是一个以技能名称命名的文件夹，其中包含 Markdown 文件 `SKILL.md`。该 Markdown 文件包含描述技能名称和用途的 frontmatter、功能概述，以及调用时机说明。文件夹中还可以包含供技能调用的脚本和其他资源。

> [!NOTE]
> 技能不一定需要额外文件或文件夹。Tailspin Toys 的 `quality-checks` 技能仅包含 `SKILL.md`，因为它使用项目现有命令。

技能可以放在项目的 `.github/skills` 文件夹中，成为团队共享和重复使用的存储库资源，也可以放在用户的技能文件夹 `~/.copilot/skills` 中。

## 探索技能

先了解 Tailspin Toys 团队用于运行单元测试、lint 和类型检查的 `quality-checks` 技能。

1. 返回 Codespace。在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读顶部的 `name` 和 `description`。说明有助于 Copilot 判断何时调用技能。
3. 阅读说明，了解它如何引导 Copilot 执行测试和 lint。
4. 注意技能目前尚无 **Results output formatting** 章节。

## 修改前先运行技能

技能可以通过 Copilot CLI 命令或自然语言直接调用。现在让 Copilot 运行三项检查。

1. 返回处于 Interactive 模式的筛选对话。
2. 使用以下提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最终报告。

## 自定义报告

现在改进报告，清楚说明运行了什么、是否成功，以及工具报告的实际结果。通过更新技能来实现这一目标。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下章节：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

现在查看更改效果！Copilot CLI 可以在不重启对话的情况下重新加载已编辑的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 输入与之前完全相同的提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最终报告并与第一次的报告比较。

## 总结和后续步骤

你已自定义并使用现有代理技能。本课中，你：

- 探索了用于单元测试、lint 和类型检查的 `quality-checks` 技能。
- 自定义了结果报告格式。
- 重新加载并运行了技能。

此更改会与筛选功能一起纳入功能 PR。接下来，通过 [Playwright MCP 服务器][next-lesson]让 Copilot 直接与网站交互。

## 更多技能示例

以下社区示例仅供参考，不是额外的练习任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表和捆绑的导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
