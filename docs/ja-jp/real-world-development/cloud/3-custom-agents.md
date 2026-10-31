---
title: "演習 3 - カスタムエージェント"
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

この演習では、カスタムエージェントを使って次のことを確認します。

- カスタムエージェントの定義方法。
- タスクをカスタムエージェントに割り当てる方法。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のユーザー フィードバックでは、テキストと背景色のコントラストが十分でないため、現在のダークテーマが読みにくいという意見がありました。このアクセシビリティの課題に対応するため、ユーザーが切り替えられる高コントラスト モードをデザイン チームが求めています。

アクセシビリティは重要なため、できるだけ早く実装したいと考えています。カスタムエージェントを使って機能を生成します。

## アクセシビリティ カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントは、あらかじめ作成されています。内容を確認して、Copilot をどのようにガイドするか把握しましょう。

codespace に戻り、アクセシビリティ カスタムエージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドが含まれる YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成を担う主要な役割。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違い。

## Issue を作成して割り当てる

ミッション コントロールは、環境内のエージェントを操作する中心的な場所です。Copilot cloud agent にタスクを割り当て、進捗を監視し、追加の指示で方向を調整できます。まず、高コントラスト モードを作成するタスクを Copilot に割り当てましょう。

1. リポジトリに移動します。
2. Issues タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい Issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てのダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

    ![アクセシビリティ エージェントが選択された cloud agent の割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を始めます。

## まとめと次のステップ

この演習では、特定のタスクや分野に合わせた専門 AI アシスタントである [GitHub Copilot のカスタムエージェント][custom-agents]を確認しました。カスタムエージェントを使うと、チームの専門知識や標準を再利用可能なエージェントとしてまとめ、Copilot が作業をより効果的に進めるためのガイダンスを提供できます。

この演習では、次の概念を確認しました。

- カスタムエージェントの定義方法。
- タスクをカスタムエージェントに割り当てる方法。

Copilot が高コントラスト モードを実装している間に、次はミッション コントロールから[エージェント セッションを監視して方向付けます][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使うための準備][org-custom-agents]
- [エンタープライズでカスタムエージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
