---
title: "练习 1 - 自定义说明（云代理）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云代理 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果某项任务必须以特定方式完成，或者 Copilot 需要了解背景信息，就要确保它能访问这些上下文。[说明文件][instruction-files]可提供此类指导，让 Copilot 不仅了解要做什么，也了解应如何完成。

本练习将介绍如何：

- 了解项目特定上下文、编码准则和文档标准如何通过存储库自定义说明及路径范围说明文件传递给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作环境不同，本练习不会使用前后对比提示。Copilot 云代理会在 GitHub issue 上异步工作，因此难以实时并排展示指令的影响。稍后审查云代理创建的拉取请求时，你会看到说明文件发挥的作用。

## 说明文件

### 场景

作为一家优秀的软件开发公司，Tailspin Toys 制定了一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加一段注释，说明该文件的用途。

通过使用说明文件，可以确保 Copilot 了解这些实践要求，并据此完成任务。

### 自定义说明

自定义说明为 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。它可以让你指定首选编码规范、库，甚至是希望添加的注释类型。你可以为整个存储库创建说明，也可以针对特定文件类型创建说明，为任务提供上下文。

说明文件分为两类：

- `.github/copilot-instructions.md` 是 Copilot 在此存储库的**每个**请求中都会收到的单个说明文件。它应包含项目级信息，即与 Copilot 聊天或 CLI 请求大多相关的上下文，例如技术栈、所构建内容的概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md` 文件可针对特定任务或文件类型创建。你可以用它们为 TypeScript 或 Astro 等特定语言提供指导，也可以用于创建 UI 组件或一组新测试等任务。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云代理都会使用存储库级说明文件，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

创建说明文件的完整讨论超出了本练习的范围。示例项目展示了一种有代表性的方法。总体而言：

- 将 `copilot-instructions.md` 中的说明聚焦于项目级指导，例如项目说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件针对特定文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言，保持指导清晰，并提供代码示例，说明代码应该和不应该采用什么写法。

说明文件没有唯一的正确写法，AI 的使用方式也不止一种。通过试验，你会逐渐了解哪些方式适合自己的项目。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一套完善的说明文件。浏览此项目中的文件时，你可能会发现有些文件专门用于多种任务，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮你生成说明文件。不同产品界面的操作方式有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相应的练习会在适用时介绍具体操作。
>
> 想找模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中收集了说明文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索此项目中的自定义说明文件

花些时间查看此存储库附带的说明文件：其中有一个核心 `copilot-instructions.md`，还有一组用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web 界面中打开它们。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，注意其中对项目的简要说明，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等章节。在与 Copilot 交互时，这些内容都适用。
3. 打开 `.github/instructions` 文件夹并浏览其中的内容。你会看到 Astro 文件、Drizzle 数据层、测试等方面的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob，用于确定说明适用的文件。此处会匹配任何 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 查看此项目中创建单元测试所需遵循的说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。注意其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明集拆分成更小、可重复使用的文件，并为 Copilot 提供可参考的示例。文件中的路径相对于说明文件所在位置，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 章节记录了项目的编码标准，但目前尚未要求在代码中添加文档。接下来，你会添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来要做的唯一编辑，是添加一条项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。云代理在本工作环境后续处理分配给它的 issue 时会采用这条规则。

编辑之前，请先创建一个分支（云代理会读取 issue 目标分支中的说明，但你仍应整洁地提交更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，以便改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 章节，该章节通常在第 27 行附近。注意其中如何记录项目的编码标准；目前尚无代码文档规则，因此生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别（`.github/copilot-instructions.md`）创建说明文件来提供全局指导，也可以创建 `*.instructions.md` 文件来指导特定语言、文件类型或任务。对于文档注释规则等项目级标准，存储库级文件是合适的位置。

## 提交、推送并合并说明更新

云代理会读取 issue 目标分支中的说明。下一练习将 issue 分配给 Copilot 时，它会从 `main` 分支创建分支，因此必须先将说明更改合并到 `main`，云代理才能使用这些说明。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并将其合并。这样，云代理在本工作环境下一练习处理 issue 时就能读取这些说明。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，可以跳过分支步骤，直接提交到 `main`。此处创建分支是为了模拟实际项目中的工作方式。

## 总结和后续步骤

你已了解 Copilot 如何从项目说明文件中获取上下文，并在 `.github/copilot-instructions.md` 中添加了新的存储库级标准。接下来的练习中，可以在云代理创建的拉取请求中看到该标准的应用。

接下来，[将第一个 issue 分配给 Copilot 云代理][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更优 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 说明文件和其他资源集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
