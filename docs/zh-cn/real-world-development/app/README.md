---
slug: zh-cn/real-world-development/app
title: "GitHub Copilot app"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** 是一款基于 Copilot CLI 构建的桌面应用，可将智能体驱动的开发集中到统一且专注的工作区。它支持并行智能体会话、可切换的会话模式、共享画布，以及原生 GitHub issue 和拉取请求管理功能。其中包括 **Agent Merge**，可引导拉取请求完成变基、处理审查反馈、修复持续集成 (CI) 问题并执行合并。

本研讨会使用 Tailspin Toys 展示完整的持续工作流：

1. 准备项目、安装应用、连接存储库，并熟悉工作区和模板创建的待办列表。
2. 完成范围明确的星级评分更改，在浏览器中审查，然后手动合并第一个拉取请求 (PR)。
3. 从筛选 issue 开始，在 **Plan** 模式中确定方案，在 **Autopilot** 模式中构建，再在 **Interactive** 模式中审查。
4. 更新存储库说明，并将其应用于筛选功能。
5. 自定义现有的 `quality-checks` 技能，并用它运行项目检查。
6. 添加 Playwright Model Context Protocol (MCP) 服务器，并用它在浏览器中探索筛选功能。
7. 创建质量保证 (QA) 自定义代理，并用它审查需求、覆盖率和验证证据。
8. 审查完整的筛选功能更改，并对第二个 PR 使用 Agent Merge。
9. 检查画布可用性，然后使用 Database Explorer 创建并测试由存储库支持的分流画布。如果所需功能不可用，请记录阻塞原因并使用只读替代方案，或继续查看总结。

为使研讨会重点明确，你将创建两个 PR：先提交星级评分，再提交筛选功能及说明更新、技能更新、QA 配置文件和测试。每个 PR 都从更新后的 `main` 开始。筛选和质量工作流共用一个会话、工作树和分支，以便在探索各项工具时继续基于已有成果构建。最后的画布练习保留在其会话中，让你专注于创建和测试共享界面，无需重复 PR 工作流。

## 课程

| 课程 | 主题 | 说明 |
|--------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 安装 Node.js，并创建自己的 Tailspin Toys 项目副本 |
| [1. 安装 Copilot app][ex1] | 设置 | 安装应用、连接项目并熟悉工作区 |
| [2. 添加星级评分：快速改进][ex2] | 首次更改 | 显示现有评分和未评分时的备用文本，然后合并 PR 1 |
| [3. 代理模式：Plan 和 Autopilot][ex3] | 代理模式 | 从 issue 规划功能，使用 Autopilot 构建，再在 Interactive 模式下审查 |
| [4. 使用自定义说明引导 Copilot][ex4] | 上下文 | 探索并更新说明，再将其应用于筛选功能 |
| [5. 自定义并使用 quality-checks 技能][ex5] | 可重复运行的检查 | 探索现有技能、更改报告格式并运行技能 |
| [6. 使用 Playwright MCP 验证功能][ex6] | 浏览器观察 | 通过 Customize 配置 MCP，并检查筛选行为 |
| [7. 创建并使用 QA 代理][ex7] | 需求和覆盖率 | 创建并选择专用配置文件，收集最终验证证据 |
| [8. 创建并合并功能 PR][ex8] | 审查和合并 | 审查筛选功能、说明、技能、QA 配置文件和测试，再对第二个 PR 使用 Agent Merge |
| [9. 探索并创建画布][ex9] | 协作 | 使用 Database Explorer，再创建并测试由存储库支持的分流画布 |
| [10. 总结和后续步骤][ex10] | 总结 | 回顾工作流、产出和更多资源 |

## 先决条件

参加本研讨会前，请确保具备以下条件：

- [ ] 拥有有效 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 方案的 GitHub 帐户
- [ ] 一台运行 **macOS、Linux 或 Windows** 的计算机
- [ ] 计算机上已[安装 Git][install-git]

> [!TIP]
> 没有付费方案？通过 [GitHub Education][callout-student-plan-education] 验证身份的学生可以免费使用 GitHub Copilot。**Copilot Student** 方案包含本研讨会所用的代理、MCP、代码审查和 Copilot CLI 功能，因此可用于完成所有工作环境中的练习。

> [!NOTE]
> Copilot app 在本地计算机而非 codespace 中运行，因此[先决条件练习][ex0]会先指导你安装 Node.js 并创建项目副本，然后再安装应用。

> [!NOTE]
> 如果使用 Copilot Business 或 Copilot Enterprise，管理员必须保持 **GitHub Copilot app** 策略处于启用状态。这与 Copilot CLI 策略不同。

## 开始

**[从先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
