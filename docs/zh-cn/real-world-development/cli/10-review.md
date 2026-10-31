---
title: "课程 10 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可重复使用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你使用 GitHub Copilot CLI 完成了 Tailspin Toys 的连续工作流。你：

- 准备了 Codespace、安装了 Copilot CLI、探索了项目并找到了已创建的筛选 issue。
- 添加了星级评分，在转发的浏览器中审查结果，并手动合并了第一个拉取请求（PR）。
- 从筛选 issue 开始，在 Plan 模式下确定方案，在 Autopilot 模式下实现功能，并在 Interactive 模式下审查。
- 使用自定义说明引导代理、自定义现有的 `quality-checks` 技能，并使用它运行单元测试、lint 和类型检查。
- 添加 Playwright Model Context Protocol (MCP) 服务器，并通过真实浏览器探索筛选功能。
- 创建并选择质量保证（QA）自定义代理，以评估需求、覆盖率、技能结果和浏览器证据。
- 审查了完整的筛选更改，并授权 Agent Merge 处理筛选 PR。
- 探索了用于管理上下文、模型、共享和可选云代理委派的斜杠命令。

## 完成的工作

本研讨会包含两个 PR 里程碑，各自基于更新后的 `main` 分支创建独立分支：

1. **星级评分**：在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流**：实现筛选功能、更新说明并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置文件并包含相关测试。

从规划筛选功能到创建其 PR，你一直使用同一对话和分支。为简化研讨会流程，我们将所有相关工作放在一个 PR 中。

## 不同类型的验证

你以多种方式检查了代码：自动化测试、亲自在浏览器中检查，以及使用 MCP 让 Copilot 在浏览器中探索。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按新格式报告结果。QA 综合这些结果，并在创建 PR 前审查需求和测试覆盖率。

添加的测试应弥补真实的覆盖缺口；QA 运行后无需添加测试也可能是正确结果。授权合并前，请审查代码和证据；更改代码后，应更新受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本研讨会中，你更新了说明、自定义技能、创建 QA 配置文件并配置 MCP 服务器。可以在多个对话中重复使用这些自定义项，并根据团队需求调整。说明用于设定标准，技能用于描述可重复执行的任务，自定义代理用于定义专用角色，MCP 服务器用于连接外部工具。请审查实际更改和工具结果，而不只是代理总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 先考虑方案，使用 **Interactive** 参与范围明确的更改，使用 **Autopilot** 处理范围清晰的任务。常规编辑可选择较快的模型，复杂工作则可选择能力更强的模型。

上下文与基础设施同样重要。清楚说明要构建**什么**、**为什么**以及**如何**构建，会显著影响输出。

## 继续探索

核心工作流已完成。还可以探索以下 CLI 功能：

- 使用 `/review` 让代码审查代理分析更改。
- 使用 `/rubber-duck` 梳理问题并获取另一种观点。
- 使用 `/fleet` 并行协调独立子任务。
- 使用 `/worktree` 隔离单独的任务。
- 使用 `/delegate` 将任务交给 Copilot 云代理。

## 后续步骤

提升工具使用能力的最佳方式就是持续练习！可以将它用于生产代码、兴趣项目，或多年来一直想构建却还未着手的小应用。与团队分享经验，也向团队成员学习。一如既往，还可以继续探索文档。

如果想从终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若要比较其他工作环境，可以探索 [VS Code 研讨会][vscode]、[GitHub Copilot 应用研讨会][app]或 [Copilot 云代理研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
