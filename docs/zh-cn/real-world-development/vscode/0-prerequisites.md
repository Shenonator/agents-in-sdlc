---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 VS Code 练习前，需要先做好准备。你将创建 Tailspin Toys 存储库的个人副本、启动一个用于开发的 [codespace][codespaces]，并确认编辑器中的 GitHub Copilot Chat 已启动。

## 设置实验存储库

要为实验代码创建副本，请从[模板][template-repository]创建一个实例。新实例会包含实验所需的所有文件，你将在后续练习中使用它。

1. 在新的浏览器窗口中，转到 [Tailspin Toys 模板][tailspin-template]。
2. 在实验存储库页面上选择 **Use this template** 按钮，创建自己的存储库副本。然后选择 **Create a new repository**。

    ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果你正在参加由 GitHub 或 Microsoft 举办的活动，请遵循导师提供的说明。否则，可以在你有权使用 GitHub Copilot 的组织中创建新存储库。

    ![输入存储库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下所创建存储库的路径（**organization-or-user-name/repository-name**），稍后会在实验中用到。

> [!NOTE]
> **待办列表已准备就绪**
>
> 从模板创建存储库时，系统会自动为你创建一组 GitHub issue。整个研讨会都会使用这些 issue，无需自行创建。

## 创建 codespace

接下来，你将使用 codespace 完成实验练习。

[GitHub Codespaces][codespaces] 是一种基于云的开发环境，可让你直接在浏览器中编写、运行和调试代码。它提供功能齐全的 IDE，支持多种编程语言、扩展和工具。

1. 转到刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

    ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，然后选择 **+** 按钮来创建新的 Codespace。

    ![创建新的 codespace](../../../_images/ex0-create-codespace.png)

创建 codespace 需要几分钟，不过仍比手动安装所有服务快得多。等待期间，可以探索 GitHub Copilot 的其他功能，接下来我们会介绍这些功能。

> [!CAUTION]
> 后续练习还会用到此 codespace。现在请先让它保持打开状态，不要关闭浏览器选项卡。

> [!NOTE]
> 本研讨会可在 codespace 或本地 [dev container][dev-containers] 中运行。两者都会预先安装所有必需组件，让练习顺利进行。如果要在本地运行，请在 VS Code 中打开克隆的存储库，并在出现提示时选择 **Reopen in Container**。VS Code 将构建与 codespace 相同的 dev container。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 使用 GitHub Copilot Chat 和 Agent 模式

要在 VS Code 中使用 GitHub Copilot Chat Agent 模式，需要在 IDE 中安装 GitHub Copilot Chat 扩展。使用 GitHub Codespace 时，该扩展通常已安装。

> [!TIP]
> 如果尚未安装 GitHub Copilot Chat 扩展，可以从 [Visual Studio Code Marketplace][copilot-chat-extension] 安装。也可以在 Visual Studio Code 中打开 Extensions 视图，搜索 **GitHub Copilot Chat** 并选择 **Install**。

安装扩展后，可能还需要使用 GitHub 帐户进行身份验证。

1. 返回 codespace。
2. 如果编辑器右侧没有显示 Copilot Chat，请选择 codespace 顶部的 **Copilot Chat** 图标。
3. 在 Copilot Chat 窗口中输入类似“Hello world”的消息，然后按 Enter。此操作会启动 Copilot Chat。
4. 如果尚未完成身份验证，系统会提示你登录 GitHub 帐户。按提示进行身份验证。

    ![Copilot Chat 身份验证提示示例](../../../_images/ex1-copilot-authentication.png)

5. 身份验证完成后，Copilot Chat 窗口应会显示出来。

## 总结

恭喜你，已创建实验存储库的副本！你也启动了 codespace，后续编写代码时会用到它。

## 下一步

接下来开始使用 Copilot。继续学习[练习 1 - 自定义说明][next-lesson]，向 Copilot 介绍项目规范。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [根据模板创建存储库][template-repository]
- [Codespaces 入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
