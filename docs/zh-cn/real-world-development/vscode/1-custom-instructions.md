---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Agent 模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，或者 Copilot 需要了解背景信息，就要确保它能访问这些上下文。[说明文件][instruction-files]可为代码生成提供指导。

本练习将介绍如何：

- 了解项目特定上下文、编码准则和文档标准如何通过存储库自定义说明及路径范围说明文件传递给 Copilot。
- 使用当前说明生成筛选功能的第一个数据切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 重新运行相同提示，观察生成的代码如何采用新标准。
- 将说明更新和筛选代码切片提交到 `main`，以便 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能与部分设定标准有所偏差，因为 Copilot 具有不确定性。本练习的目标是观察更新说明后行为的*整体趋势*，而不是让输出逐字相同。

## 说明文件

### 场景

Tailspin Toys 制定了一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加一段注释，说明该文件的用途。

通过使用说明文件，可以确保 Copilot 了解这些实践要求，并据此完成任务。

### 自定义说明

自定义说明为 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。它可以让你指定首选编码规范、库，甚至是希望添加的注释类型。你可以为整个存储库创建说明，也可以针对特定文件类型创建说明，为任务提供上下文。

说明文件分为两类：

- `.github/copilot-instructions.md` 是 Copilot 在此存储库的**每个**请求中都会收到的单个说明文件。它应包含项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md` 文件可针对特定任务或文件类型创建，例如为 TypeScript 或 Astro 提供指导，或指导创建 UI 组件或一组新测试。

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

先查看此项目附带的说明文件，再进行更改，以了解其影响。其中有一个核心 `copilot-instructions.md` 和一组用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web 界面中打开它们。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，注意对项目的简要说明，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等章节。在与 Copilot 交互时，这些内容都适用。在 **Code standards** 下方，注意嵌套的 **GitHub Actions Workflows** 指导。
3. 打开 `.github/instructions` 文件夹并浏览其中的内容。你会看到 Astro 文件、Drizzle 数据层、测试等方面的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob，用于确定说明适用的文件。例如，符合 `**/*.test.ts` 的 TypeScript 测试文件会匹配。
5. 查看此项目中创建单元测试所需遵循的说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。注意其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明集拆分成更小、可重复使用的文件，并为 Copilot 提供示例。文件中的路径相对于说明文件所在位置，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 章节记录了项目的编码标准，但目前尚未要求在代码中添加文档。接下来，你会添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

遵循最佳实践，为更改创建分支。

1. 返回上一练习中的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新的终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。不同练习所用的模式和模型可能不同；每个步骤会在适用时说明。

为了观察自定义说明的影响，先在当前说明文件保持不变时发送提示。稍后更新说明后，再次发送相同提示并比较结果。

1. 关闭之前练习留下的编辑器选项卡，避免 Copilot 获取不需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的代理下拉列表中选择 **Agent**，让 Copilot 应用文件更改。

   ![Chat 视图中的代理选择器截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 注意生成的辅助函数是一个有类型的函数，它将 `db` 客户端作为第一个参数，并返回有类型的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层规范。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> 由于 Copilot 的输出具有不确定性，即使没有明确要求，也可能添加文档注释。如果出现这种情况也没关系，本练习重点在于观察更新说明后行为的一致性变化。

## 添加新的存储库标准

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

## 重新发送提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat** 清除当前对话内容。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦到正确文件。
3. 发送与之前**相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意生成的文件现在以类似下面的注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意生成的函数现在包含类似下面的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚才引导 Copilot 遵循新的项目标准，并将其应用到真实代码中；下一练习会继续使用这段代码。

## 提交说明并推送分支

说明文件与存储库中的其他资源一样，使用相同的源代码管理方式。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新的终端窗口。
2. 在终端中运行以下命令，确认说明更新和辅助函数更改已存在：

   ```bash
   git status
   ```

3. 暂存并提交说明更新，以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在创建拉取请求，并关联待办列表中关于文档标准的 issue。Copilot 可以调用已连接到项目的 GitHub 工具，查找 issue、创建用于关闭该 issue 的 PR，然后合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上执行操作。项目模板已配置 GitHub 工具，因此无需设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按提示授权即可。稍后的练习会介绍这种连接的工作方式（Model Context Protocol）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保在代理选择器中选择 **Agent**。
3. 让 Copilot 查找与更新说明文件有关的 issue，并从当前分支创建 PR，同时描述说明更新和新的发行商辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找 issue 并创建 PR。
5. 系统提示 **Allow** Copilot 代表你执行 GitHub 操作时，请先审查命令，再酌情选择 **Allow**。
6. PR 创建后，让 Copilot 合并 PR 并将本地分支切回 main：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 系统提示 **Allow** Copilot 代表你执行 GitHub 操作和 shell 命令时，请先审查命令，再酌情选择 **Allow**。

现在，你已借助 GitHub Copilot 创建并合并了拉取请求！

## 总结和后续步骤

你已了解 Copilot 如何从说明文件中获取上下文，并在 VS Code 中使用 Copilot Chat：

- 发送代码生成提示，观察 Copilot 如何使用*现有*说明。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 重新发送相同提示，观察生成的代码如何采用新标准。
- 将说明更新和辅助函数基础提交到 `main`，供下一练习使用。

接下来，在[Agent 模式][next-lesson]中运用这些说明，让 Copilot 在整个代码库中添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更优 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot — 说明文件和其他资源集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
