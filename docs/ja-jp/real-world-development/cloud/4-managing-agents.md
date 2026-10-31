---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習では、Copilot cloud agent にユーザー体験の改善と機能追加を目的とする3つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業するように設計されていますが、タスクの進捗を監視することも重要です。

GitHub.com の[エージェント ページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールがいくつかあります。このミッション コントロールでは、プルリクエスト (PR) が開いているすべてのエージェント タスクを確認できます。実行された操作を調べたり、進行中のセッションに追加の指示を送ったりできます。

このレッスンでは、次の操作を行います。

- エージェント ページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

エージェントに高コントラスト モードの作成を依頼した後、チームはライトモードも追加するとよいと気付きました。サイトのスタイル更新と切り替え機能に取り組んでいるため、同時に追加するのが合理的です。高コントラスト モードだけでなくライトモードも実装するよう、エージェントの作業を方向付けます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左ペインの両方にあるタスク一覧を確認します。割り当てた次のタスクが表示されます。
    - コードベースのドキュメントの更新。
    - ゲーム変更用 API の生成。
    - Web サイトへの高コントラスト モードの追加。
3. 実行中のタスクを1つ選択します。Copilot が実行した次のような作業を確認します。
    - リポジトリのチェックアウト。
    - Copilot が作業する環境の作成。
    - MCP サーバーの設定。
    - 割り当てたタスクを完了するための各種手順。

> [!NOTE]
> 表示される手順は、Copilot の作業状況やアプローチによって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを表示し、さらに詳しく監視できます。

## cloud agent の作業を方向付ける

現在実行中のタスクを確認したので、Copilot が高コントラスト モードを作業している間にライトモードも追加するよう依頼します。

1. 高コントラスト モードの追加を示すセッションを選択します。正確なタイトルは、Copilot が付けた名前や作業状況によって異なります。

    ![ミッション コントロールのアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まったことが表示されるまで、数分間セッションを確認します。次のようなメッセージが表示されたら作業開始です。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェント ページで作業中の cloud agent のタスクを表示し、セッションへの追加指示ダイアログを強調した画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が指示を受け取り、作業に取り込むことを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業していた手順が終わると、新しい依頼をワークフローに組み込みます。

前と同じように数分かかります。これまでに学んだことを振り返りながら待ちましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視し、ガイドする中心的な場所である Copilot agents ページを確認しました。このミッション コントロールでは、進行中および完了したタスクを追跡し、作業内容をレビューして、進行中のタスクに追加の指示を送って範囲を調整できます。

このレッスンでは、次の概念を確認しました。

- ミッション コントロールとエージェント ページで cloud agent のタスクを監視する。
- 進行中のセッションに追加機能を依頼する。

Copilot がアクセシビリティ機能を実装している間に、次は[Copilot が作成したプルリクエストを改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
