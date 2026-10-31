---
title: "レッスン 2 - 星評価の追加で小さな成果を得る"
description: "GitHub Copilot app で最初のエージェントセッションを開始し、ゲームカードに小さな変更を加えて、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

ワークスペースを確認し、クイックチャットも試しました。次は、**エージェントセッション**を開始し、プロジェクトに最初の変更を加えます。変更は小規模なものにします。ゲームのデータにはすでに星評価が含まれていますが、ホームページのゲームカードにはまだ表示されていません。エージェントに表示を依頼し、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次の操作を行います。

- エージェントセッションを開始し、セッションの構成を学びます。
- エージェントにプロジェクトの小さく対象を絞った変更を依頼します。
- ワークスペースの差分ビューで変更をレビューします。
- アプリをローカルで実行し、ブラウザーで変更を確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価を設定でき、ゲーム詳細ページにはすでに表示されています。一方、ホームページのゲームカードには、タイトル、カテゴリー、パブリッシャー、説明しか表示されていません。最初のセッションの準備運動として、各カードに既存の評価を表示するようエージェントに依頼します。小規模で自己完結した、最初のセッションにぴったりの変更です。

## セッションの構成

**セッション**とは、エージェントと会話しながら、分離された独自のワークスペースで作業することです。各セッションに**専用の git worktree とブランチ**が割り当てられます。そのため、一方で機能を追加し、もう一方でバグを修正するなど、変更を競合させずに複数のセッションを同時に実行できます。セッションはサイドバーにリポジトリごとにまとめて表示され、選択すると切り替えられます。

セッション内には、エージェントとの**会話**、ファイルを調査・編集するエージェントの**ツール アクティビティ**、差分付きの**変更済みファイル一覧**が表示されます。

## セッションを開始して変更を依頼する

新しいセッションを始めて、プロジェクトを確認しながら機能を実装します。[アプリのセットアップ][prior-lesson]では、GitHub リポジトリからプロジェクトを追加しました。そのリポジトリ用のセッションを作成し、変更を依頼しましょう。

1. GitHub Copilot app に戻るか、アプリを開きます。
2. サイドバー上部の **+** を選択してセッションを開始します。
3. リポジトリとして `tailspin-toys` を選択します。
4. プロンプト ボックスの下で **new working tree** と **Interactive** モードを選択し、次のプロンプトを使って変更を依頼します。

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. <kbd>Enter</kbd> を押して、プロンプトを Copilot に送信します。

Copilot app は、まずプロジェクトの分離されたコピーである新しい worktree を作成します。次にプロジェクトを調べ、新機能の追加に必要な更新対象ファイルを見つけてコードを作成します。これで Copilot app を使って新機能を追加できました。

## 差分をレビューする

AI が生成した変更は、どれほど小さくてもマージ前にレビューします。Copilot app 内で変更を確認しましょう。

1. アプリの右上隅にある **Toggle review panel** を選択します。開いたパネルで **Changes** を選ぶと、変更されたファイルと差分が表示されます。

    ![Create PR の右側にある Toggle review panel ボタンを矢印で示した GitHub Copilot app の上部ツールバー](../../../_images/app-2-review-panel.png)

2. ゲーム詳細の表示に使われる中心的なファイル `GameCard.astro` にコードが追加されていることを確認します。評価があれば表示し、`starRating` が `null` なら "No rating yet" にフォールバックする、次のような小さなブロックになるはずです。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Copilot は、すべての生成 AI ツールと同様に確率的に動作するため、実際のコードは上記と異なる場合があります。ただし、比較的よく似たものになります。

## 変更を確認する

ブラウザーを開く前に、エージェントが実行した自動チェックの結果をレビューします。数値の `starRating` と、`null` の場合の代替表示をテストしていることを確認します。前提条件が不足していたり、チェックがスキップされていたりする場合は成功ではありません。インストールの要求は内容を確認してから承認してください。

コードを読むだけで動くと判断するのではなく、Copilot に Web サイトを起動させ、更新された UI を確認しましょう。組み込みのブラウザーキャンバスでサイトを開けます。

> [!TIP]
> キャンバスは、Copilot app 内で使えるインタラクティブなウィジェットです。後のレッスンではカスタムキャンバスを調べ、自分でも作成しますが、ここでは組み込みのブラウザーキャンバスを使います。

1. 次のプロンプトを使い、アプリを起動してブラウザーキャンバスで開くよう Copilot に依頼します。

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. しばらくするとアプリが起動し、Copilot app 内にブラウザー ウィンドウが開きます。
3. 評価済みのゲームカードに5点満点の値が表示されることを確認します。
4. 確認が終わったら、次のプロンプトを使い、このセッションで起動した開発サーバーを停止するよう Copilot に依頼します。

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 最初のプルリクエストを作成してマージする

機能を作成できました。次は、新しいコードを既存のコードベースにマージするためのプルリクエスト (PR) を作成します。

1. 右上隅にある **Create PR** を選択します。
2. 求められた場合は **Sign in with your browser** を選択し、画面の指示に従って認証します。
3. Copilot から PR の作成前に関連する Issue が必要だと説明された場合は、その手順に従うことを確認します。
4. Copilot が PR を作成します。
5. チャットのすぐ上にある **PR** バブルを選択し、レビューペインでプルリクエストを開きます。ここで PR をレビューできます。
6. 問題がなければ **Ready to merge** を選択します。
7. 新しいダイアログ ウィンドウで **Merge pull request** を選択して PR をマージします。

## まとめと次のステップ

GitHub Copilot app で最初の変更をリリースしました。このレッスンでは、次の操作を行いました。

- エージェントセッションを開始し、セッションの構成を学びました。
- ゲームカードに小さく対象を絞った変更を加えるようエージェントに指示しました。
- ワークスペースの差分ビューで変更をレビューしました。
- アプリをローカルで実行し、ブラウザーで星評価を確認しました。
- PR 1 を作成してチェックをレビューし、明示的にマージしました。

次は、[フィルター機能の Issue から始めて Plan モードと Autopilot モードを使い][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot app でエージェント セッションを操作する][agent-sessions]
- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app で Issue とプルリクエストを管理する][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#ワークスペースを確認する
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
