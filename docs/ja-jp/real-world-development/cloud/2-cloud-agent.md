---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加などを自律的に実行できます。エージェントの作業が完了すると、人がレビューできるドラフト PR が作成されます。面倒なタスクを任せて開発を加速し、より大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次のことを確認します。

- コード生成環境をカスタマイズする方法。
- 操作を安全に行う方法。
- Issue の範囲を明確に定義する重要性。
- Issue を Copilot に割り当てる方法。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。サイトの初期バージョンを作った請負業者が、ドキュメントを適切に整備していませんでした。実際、ドキュメントがまったくありません。まず、アプリケーションのエクスポートされたすべての関数に TSDoc のドキュメントコメントを追加したいと考えています。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリーのほかのゲームを表示し、続けて閲覧できるようにします。現時点では完成度を高める必要はなく、UX の受け入れテストに使えるものを求めています。この作業はブロッカーですが、優先度がより高い Issue もあります。

どちらも、優先度が下がりやすいタスクです。Copilot cloud agent に割り当てれば、開発者は別の作業に集中し、後から Copilot の結果をレビューできます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドで作業できます。たとえば、[GitHub Issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)方法があります。割り当てると、Copilot は進捗を追跡するドラフト プルリクエストを作成し、環境を設定してタスクに取り組みます。作業中または完了後に Copilot のセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 適切な範囲の指示が重要

GitHub Copilot に魔法はありません。考える作業を省略することはできません。一見簡単そうな作業でも、詳しく調べると複雑な部分があります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペア プログラマーとして、段階的に作業し、学び、試し、必要に応じて調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot の動作への影響を確認しました。カスタム指示を使う演習をすでに行った場合は、`.github/copilot-instructions.md` が機能することを確認したはずです。まだの場合は、この機会に確認してください。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認しておくと、後でその影響を判断できます。

GitHub Web UI で次のファイルを開くか、起動済みの codespace から確認します。

- `.github/copilot-instructions.md` - **Code standards** セクション、特に TypeScript の規約と TSDoc のドキュメントコメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` フロントマターで、`**/*.test.ts` ファイルに指示の対象が限定されていることを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てるとき、作成されるプルリクエストで TSDoc のドキュメントコメント、TypeScript の規約、コメント ヘッダーを確認してください。これらの指示ファイルの内容が反映されています。後の演習で PR をレビューするときにも、改めて確認します。

## Copilot cloud agent の開発環境を設定する

誰が作業する場合でも、コードの作成には通常、適切な環境と、状態を整えるためのセットアップ スクリプトが必要です。SWE と同様に作業する Copilot にタスクを割り当てる場合も同じです。

Cloud agent は [GitHub Actions][github-actions] を使って作業環境を準備します。`.github/workflows/copilot-setup-steps.yml` ファイルに設定する[専用のセットアップ ワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールと依存関係を利用できるようにできます。ラボの流れを円滑にして、学習の機会を提供するため、この設定はあらかじめ用意されています。単一の Astro アプリで、Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、マイグレーションとシードが済んだ SQLite データベースを Copilot が利用できるようにします。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub Actions ワークフローと同様ですが、注目する点がいくつかあります。

- `copilot-setup-steps` という単一のジョブが含まれています。このジョブは、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot に実行されるのを待たずに、正常に実行されるかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、多くのプロジェクトでは情報が古かったり、まったくなかったりします。このような技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメントの作成を得意としているため、cloud agent に割り当てる Issue に適しています。Copilot はバックグラウンドで必要なドキュメントを作成します。後の演習で、作業結果をレビューします。

1. 新しいブラウザー タブで github.com の自分のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てのダイアログを開きます。

    ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

    ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` などになります。新しい PR が表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が ToDo リストを作成したことを確認します。

> [!NOTE]
> Copilot の ToDo リストが PR に表示されるまで数分かかることがあります。Copilot は環境を準備し (前述のワークフローを実行)、プロジェクトを分析して、問題に取り組む方法を決めています。

13. リストと、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

    ![Copilot のセッション画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するには、ブラウザーを更新する必要がある場合があります。

16. ライブ セッションをスクロールして Copilot の作業を確認します。コードを調べて状態を理解すること、考えて適切な計画を決めること、コードを作成することなどが含まれます。

作業には数分かかる場合があります。Copilot cloud agent の主な目的の1つは、タスクを非同期で実行して別の作業に集中できるようにすることです。もう1つのタスクも Copilot cloud agent に割り当て、その間にアプリケーションへ機能を追加する作業を始めましょう。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の大きな利点の1つは、作業を分けられることです。自分が1つの作業に取り組みながら、エージェントに別の作業を任せられます。デザイン チーム向けの関連ゲームの追加に時間がかからないとしても、その時間をほかの作業に使える可能性があります。Copilot cloud agent に割り当てましょう。

1. github.com の自分のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てのダイアログを開きます。

    ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業を始めたことを示す 👀 が Issue の最初のコメントに表示されます。

![Copilot が作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼に取り組んでいます。Copilot cloud agent は SWE と同じように動作するため、常に監視する必要はなく、完了してからレビューできます。次は、カスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

この演習では、[GitHub Copilot cloud agent][copilot-agents]を使いました。cloud agent に Issue を割り当て、非同期で作業させることができます。技術的負債の解消、新機能の作成、別のフレームワークへのコード移行などに利用できます。

この演習では、次の概念を確認しました。

- コード生成環境のカスタマイズ。
- 操作を安全に実行すること。
- Issue の範囲を明確に定義する重要性。
- Issue を Copilot に割り当てる方法。

cloud agent がバックグラウンドで作業している間に、カスタムエージェントを作成して使う方法を確認します。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示も適用されます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
