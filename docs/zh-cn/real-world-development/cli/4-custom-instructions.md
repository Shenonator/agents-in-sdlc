---
title: "课程 4 - 使用自定义说明引导 Copilot"
description: "探索存储库说明、添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，就要让 Copilot 能够访问相应指导。[说明文件][instruction-files]不仅描述想要什么代码，也说明代码应如何组织。现在你已构建筛选功能，接下来将探索 Copilot 使用的说明、添加文档标准，并将其应用于代码。

本课将介绍如何：

- 了解存储库说明和路径范围说明文件如何传递给代理。
- 更新说明文件，确保遵循编码标准。
- 查看说明文件对代码的影响。

## 场景

Tailspin Toys 制定了一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中的导出函数应使用 TSDoc/JSDoc 说明其用途、参数和返回值（包括其中可用的可注入 `db` 参数）。
- 可重复使用的 Astro 组件应说明其 `Props` 接口；相关代码更改时，注释也应保持更新。
- 应保留现有格式和 lint 指南。

通过说明文件，可以确保 Copilot 了解这些实践要求，并据此完成任务。

## 说明文件

自定义说明为 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。它可以让你指定首选编码规范、库，甚至是希望添加的注释类型。你可以为整个存储库创建说明，也可以针对特定文件类型创建说明，为任务提供上下文。

说明文件分为两类：

- `.github/copilot-instructions.md` 是 Copilot 在此存储库的**每个**请求中都会收到的单个说明文件，应包含与大多数请求相关的项目级信息。
- `.github/instructions/*.instructions.md` 文件可针对特定语言、文件类型或任务提供指导。

> [!NOTE]
> 不同环境对其他说明格式的支持有所不同。在依赖特定格式前，请查看[自定义说明支持参考][custom-instructions-support]。

## 探索此项目中的自定义说明文件

起始项目已包含一组说明文件。先探索现有文件，再进行更改，以了解其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，注意项目简要说明和编码指导。这些说明适用于在此存储库中与 Copilot 进行的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到 Astro 文件、Drizzle 数据层、测试等方面的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了决定说明适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看其中指向其他说明文件和项目现有文件的链接。这样可以将较大的说明集拆分成更小、可重复使用的文件，并提供可参考的示例。

## 更新说明文件以符合团队指导

虽然现有文件提供了很好的起点，但仍缺少一项规则。现在修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 章节，通常位于第 35 行附近。
2. 在该章节末尾添加以下项目符号：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库说明。保存更新后的说明后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 将其关闭。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录中恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选对话并确认当前检出的分支为 `game-filters-cli`。
3. 让 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并查看更改的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认其准确说明了代码。

## 总结和后续步骤

你已了解 Copilot CLI 如何从说明文件中获取上下文，并将新标准应用于功能代码。具体来说，你：

- 了解存储库说明和路径范围说明文件如何传递给代理。
- 更新说明文件以确保遵循编码标准。
- 查看说明文件对代码的影响。

接下来，[自定义并运行可重复使用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot — 说明文件和其他资源集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
