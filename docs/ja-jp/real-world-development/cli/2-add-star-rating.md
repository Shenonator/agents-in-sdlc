---
title: "レッスン 2 - 星評価の追加: 小さな改善"
description: "Copilot CLI でゲームカードに小さな変更を加え、転送したブラウザーで確認して、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールして会話を試したので、次はプロジェクトに最初の変更を加えます。変更は小さくします。ゲームのデータには星評価がすでにありますが、ホームページのゲームカードには表示されていません。エージェントに評価を表示するよう依頼し、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次の操作を行います。

- 機能ブランチで、対象を絞った Copilot の会話を始めます。
- エージェントにプロジェクトの小さな変更を依頼します。
- `/diff` で変更をレビューします。
- アプリを実行し、転送したブラウザーで変更を確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価を設定できます。評価はゲーム詳細ページにはすでに表示されていますが、ホームページのゲームカードにはタイトル、カテゴリー、パブリッシャー、説明しか表示されていません。練習として、各カードに既存の評価を表示するようエージェントに依頼します。これは小規模で独立した変更なので、最初のセッションにぴったりです。

## 会話の構成

**会話**では Copilot CLI を使ってタスクに取り組みます。Copilot app と異なり、通常の CLI の会話では、専用の worktree ではなく、ターミナルで現在チェックアウトしているリポジトリと Git ブランチを使います。保存した会話は後で再開できますが、ファイルとブランチは通常の Git 状態としてディスク上に残ります。

会話では、プロンプトとエージェントの応答、ファイルを調査・編集するエージェントのツール操作、`/diff` で確認できる変更の3つを扱います。

## 会話を始めて変更を依頼する

機能の実装を始めるため、新しい会話を開始します。

1. Codespace に戻ります。
2. まだターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. まだ起動していない場合は、次のコマンドを実行して Copilot を起動します。

   ```bash
   copilot --yolo
   ```

4. スラッシュ コマンド `/new` を実行して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次のプロンプトを使って変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲーム詳細の表示に使われているファイルを見つけて、必要なコードを作成します。これで Copilot CLI を使った新機能の追加は完了です。

## 差分をレビューする

小さな変更でも、AI が生成した変更はすべてマージ前にレビューします。Copilot CLI で変更を確認しましょう。

1. `/diff` を入力し、変更されたファイルをすべて確認します。
2. 評価がある場合はゲームカードに数値が表示され、`starRating` が `null` の場合は `No rating yet` と表示されることを確認します。
3. テストで両方の状態が網羅されていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら、<kbd>Esc</kbd> を押して差分画面を閉じます。

> [!NOTE]
> ほかの生成 AI ツールと同様に、Copilot は確率的に動作するため、生成されるコードは環境によって異なる場合があります。特定の実装と比較するのではなく、動作をレビューしてください。

## 変更を確認する

コードを読んだだけで動作すると判断するのではなく、Copilot に Web サイトを起動してもらい、Codespaces で転送したブラウザーから更新された UI を確認します。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能と表示されたら、**Open in Browser** を選択します。
3. ゲームカードに5段階の評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを作成してマージする

機能を実装できました。次は、プロジェクトに新しいコードを取り込むプルリクエスト (PR) を作成します。

1. 既定のエージェントに変更をコミットするよう依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR の作成時に既存のコミットをプッシュし、PR の URL を表示します。
3. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、Copilot CLI に表示された URL を選択して PR を開きます。
4. 変更されたファイルとチェックをレビューします。
5. 問題がなければ **Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

おめでとうございます。GitHub Copilot CLI を使って、初めての変更をリリースしました。このレッスンでは、次の操作を行いました。

- 機能ブランチで対象を絞った Copilot の会話を始めました。
- ゲームカードに小さな変更を加えるようエージェントに指示しました。
- `/diff` で変更をレビューしました。
- アプリを起動し、転送したブラウザーで星評価を確認しました。
- 最初のプルリクエストを作成してマージしました。

次は、[フィルター機能の Issue から始めて Plan モードと Autopilot モードを使い][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
