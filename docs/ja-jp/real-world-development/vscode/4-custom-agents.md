---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた専門 AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、対象を絞った指示、ベスト プラクティス、コーディング パターン、分野固有の知識を提供できます。チームの専門知識を再利用可能なエージェントとしてまとめることもできます。たとえば、[WCAG][wcag] への準拠を確認するアクセシビリティ エージェント、安全なコーディング手法に従うセキュリティ エージェント、一貫したテスト パターンを維持するテスト エージェントなどです。

カスタムエージェントは、プロジェクト内の `.github/agents` フォルダー、またはユーザー全体に適用される `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を定義する Markdown のプロンプトを記述します。

### カスタムエージェントとエージェント スキルの違い

カスタムエージェントと[エージェント スキル][agent-skills-concept]には一部共通点があります。どちらも主に Markdown ファイルで定義され、AI に作業の方法を指示します。違いを簡潔に表すなら、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキスト ウィンドウを持ち、スキル (場合によってはほかのエージェントも) を調整して作業します。このラボのアクセシビリティ カスタムエージェントは、アクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業の中で、プルリクエストのワークフロー スキルや、テストを実行・管理するスキルなどを呼び出すことができます。

> [!NOTE]
> カスタムエージェントを作成する唯一の正解はありません。AI のほかの用途と同様に、利用環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のユーザー フィードバックでは、テキストと背景色のコントラストが十分でないため、現在のダークテーマが読みにくいという意見がありました。このアクセシビリティの課題に対応するため、デザイン チームは、ユーザーが切り替えられる高コントラスト モードを求めています。

アクセシビリティは重要なため、できるだけ早く実装したいと考えています。カスタムエージェントを使って機能を生成します。

この演習では、次の操作を行います。

- 既存のアクセシビリティ カスタムエージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、高コントラスト モードを実装します。

## アクセシビリティ カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントは、あらかじめ作成されています。内容を確認して、Copilot をどのようにガイドするか把握しましょう。

codespace に戻り、ターミナルを開いて、アクセシビリティ機能用に `main` から新しいブランチを作成します (演習 3 のフィルター機能 PR は別にしておきます)。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドが含まれる YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成を担う主要な役割。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違い。

## Copilot Chat でカスタムエージェントを使う

VS Code では、`.github/agents` で定義されたすべてのカスタムエージェントが、Copilot Chat ビュー下部のエージェント選択メニューに表示されます。カスタムエージェントを選択すると、会話でそのエージェントの指示とツールを使えます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、該当する手順で説明します。

1. Chat ビューのエージェントのドロップダウンから、まだ選択されていない場合は **Agent** を選択します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部にあるエージェント選択メニューを選択します。現在のエージェント名が表示されます。既定では **default** です。
3. 利用可能なエージェント一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティ エージェントに送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調べて変更を提案し、プロジェクト ファイルを編集します。編集内容は、ファイル パスと差分とともにチャットに表示されます。
6. エージェントが編集したファイルに合わせて、**Files changed** の表示が更新されます。

> [!NOTE]
> 作業には数分かかる場合があります。Copilot はリポジトリに実際の変更を加えます。既存の Astro コンポーネント、CSS、関連するテストなどが編集されます。

次の演習で、この作業の進捗を確認し、方向を調整します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた専門 AI アシスタントである [GitHub Copilot のカスタムエージェント][custom-agents]を確認しました。カスタムエージェントを使うと、チームの専門知識や標準を再利用可能なエージェントとしてまとめ、Copilot が作業をより効果的に進めるためのガイダンスを提供できます。

このレッスンでは、次の概念を確認しました。

- カスタムエージェントの定義方法。
- Copilot Chat のエージェントモードでカスタムエージェントを使う方法。

次は、エージェントの作業を[監視して方向付けます][next-lesson]。変更を確認しながら、同じセッションにライトモードの切り替えを追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
