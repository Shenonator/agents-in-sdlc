---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan でアプローチを決め、Autopilot で Issue に基づくフィルター機能を実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

まず小さな機能をプロジェクトに追加しました。より大きな変更には、さらにしっかりとしたプロセスが必要です。GitHub Copilot CLI は、組織で既に使われている作業の流れに合わせ、正しい方法で適切なものを構築できるように設計されています。このレッスンから、エージェントを使った開発プロセスを段階的に進めます。Issue を起点に新機能を作り、コードの妥当性と機能の動作を確かめ、最終的にプロジェクトへマージします。

> [!NOTE]
> 機能の作業を続ける間は、同じ会話とブランチを使います。通常、ファイル種別ごとに異なるブランチや PR を使いますが、ここでは主な概念に集中するため簡略化しています。

このレッスンでは、次の操作を行います。

- GitHub Issue から新しい Copilot の会話を始めます。
- Plan モードで要件を定義します。
- Autopilot モードで新機能を実装します。
- コードをレビューします。
- 転送したブラウザーで機能を手動で検証します。

機能の作業を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者がゲームをカテゴリーやパブリッシャーで絞り込めるようにする必要があります。バックログの Issue には機能が記載されていますが、カテゴリーの組み合わせ方など、コーディング前に合意が必要な詳細があります。Plan モードで方針を決めてから、範囲を限定した実装を Autopilot に許可します。

## 背景

AI コーディング エージェントを開発に取り入れても、基本的な考え方は変わりません。むしろ、より重要になります。多くの開発者は、次のような流れで作業します。

1. 必要な作業を記述した Issue から始めます。
2. 構築する内容を計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> チームや組織によって詳細は異なりますが、ほとんどの場合は上記の流れに沿っています。

この標準的な手順に従うことで、AI が生成したコードが要件を満たし、手作業で書いたコードと同じレビュー プロセスを経るようにできます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、モードを切り替えられます。

- **Interactive**: エージェントと一緒に作業します。エージェントは変更を提案し、次に進む前に入力を待ちます。
- **Plan**: エージェントが最初に計画を作成します。プロジェクト ファイルの編集はできません。
- **Autopilot**: エージェントがコードを書き、テストを実行し、タスクが完了するまで反復して自律的に作業します。

まず Plan モードで計画をレビューしてから、Autopilot で実装します。

## Issue から始める

フィルター機能の作業を始める前に、Codespace に戻ってリポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は、再起動してから続けます。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新してから、フィルター機能のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を2回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7` (または同様の番号) で始まることを確認してください。`#` を使うと、GitHub 上の Issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルター機能を計画する

計画を立てると、Copilot に機能の実装やタスクを任せる前に、アプローチを決められます。複雑な作業では、事前に時間をかけて計画するのが効果的です。Plan モードに切り替えて、Copilot に計画を作成してもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 前の手順で追加した Issue 参照の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot がプロジェクトを調べ、適切なアプローチを決めて計画を作成します。

3. フィルター機能の動作について質問された場合は、希望に応じて回答します。正解は1つではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を上下にスクロールしてレビューします。
6. 決めた内容と異なる部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画ができてレビューも終わったので、実装に進みます。Autopilot を使って Copilot に作業を任せましょう。

Autopilot では、Copilot が完了したと判断するまで問題に取り組み、反復して改善します。

1. **Accept plan and build on autopilot (recommended)** を選択します。インストール済みのバージョンにより、同様のラベルが表示されることがあります。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が合意済みの計画に沿ってコードを生成し、テストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。先に計画をレビューしてください。Copilot が依存関係の不足やポートの競合を報告した場合は、チェック完了と判断する前に環境の問題を解決してください。

## 実装をレビューして検証する

ほかのコードと同様に、生成されたコードもマージ前にレビューします。コードを確認し、サイトを起動して問題がないか調べましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力して、フィルター機能の実装とテストを確認します。
3. 結果を Issue と計画中に決めた内容と比較します。
4. コードをレビューしたら、<kbd>Esc</kbd> を押して差分表示を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

コードは問題なさそうですが、実際に動くでしょうか。前回と同じようにアプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能と表示されたら、**Open in Browser** を選択します。
3. カテゴリーによる絞り込み、パブリッシャーによる絞り込み、計画で合意した組み合わせを試します。
4. リセット時と結果が空の場合の動作が、Issue と合意内容に沿っていることを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

会話モードを使い分けて、機能を構築しレビューしました。このレッスンでは、次の操作を行いました。

- GitHub Issue から Copilot の会話を始めました。
- Plan モードで要件を定義しました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送したブラウザーで機能を手動で検証しました。

次は、ドキュメント化された開発手法に沿ってコードを生成する方法を確認するため、[カスタム指示を使います][next-lesson]。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
