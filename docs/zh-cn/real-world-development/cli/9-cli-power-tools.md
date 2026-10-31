---
title: "课程 9 - GitHub Copilot CLI 中的斜杠命令"
description: "探索管理上下文、选择模型、共享会话和可选委派给云代理的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。它们可以访问高级功能、幕后信息和其他配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令，现在再来了解几个实用命令。

本课将介绍如何：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 了解可用模型。
- 了解如何通过 `/share` 导出或共享会话。
- 探索并行工作、工作树和委派给云代理的可选命令。

## 场景

核心 CLI 工作流已完成。现在了解其他能力：管理上下文、切换模型、共享会话，以及选择是否将工作委派给 [Copilot 云代理][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理较大或较复杂的任务时，可能会达到模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话；你也可以使用斜杠命令检查或压缩上下文。

1. 返回 Codespace。如果 Copilot CLI 尚未打开，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 用量，以及上下文在系统说明、工具、消息和可用空间之间的分布。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> 上下文窗口接近上限时，Copilot CLI 会自动压缩上下文。想自行决定时间时，可以使用 `/compact`。如果要切换到无关任务并开始全新对话，可以使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，不同开发人员也有不同偏好。Copilot CLI 可列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型及其用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队合作并分享经验有助于每个人更好地使用 AI 工具。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看当前安装版本中的 `/share` 选项。
2. 如果要共享此会话，请选择符合需求的目标位置，例如使用 `/share file` 导出本地 Markdown 文件。
3. 发送或发布导出内容前先进行审查。会话导出可能包含提示、响应和项目详情。

是否发布链接或 gist 由你决定。不要发布团队无意共享的存储库或对话内容。

## 可选：扩展工作或委派任务

核心研讨会已完成。Copilot CLI 还为较大的任务提供了以下命令：

- `/fleet` 可将彼此独立的子任务分配给子代理并行处理。
- `/worktree` 可为单独的任务创建隔离的 Git 工作树。
- `/delegate` 可将任务交给 Copilot 云代理异步处理，它可能会创建拉取请求。

这些命令是可选的，因为它们可能创建其他工作树或远程工作。尝试之前，先开始一个范围明确的新任务，并通过常规工作流审查结果。若要深入了解异步代理工作，请继续学习[云代理研讨会][cloud-workshop]。

## 总结和后续步骤

使用 Copilot CLI 斜杠命令可以配置工具、共享会话并查看幕后操作。本课中，你：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 了解可用模型。
- 了解如何通过 `/share` 导出或共享会话。
- 探索了并行工作、工作树和委派给云代理等可选命令。

Copilot CLI 还有更多斜杠命令和功能值得探索！接下来，[回顾所学内容][next-lesson]并了解后续学习方式。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云代理][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
