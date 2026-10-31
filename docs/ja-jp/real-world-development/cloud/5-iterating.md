---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善と機能追加を目的とする複数のタスクで GitHub Copilot と作業しました。Copilot にコードのドキュメントを追加させ、デザイン チームが改善できる関連ゲーム機能を構築させ、高コントラスト モードとライトモードの切り替えなどのアクセシビリティ機能を実装させました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って作業を改善しましょう。

### シナリオ

これまでに何度も確認したように、生成 AI を導入しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チーム全体にレビューを依頼する前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関して GitHub Copilot が提案した内容を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期かつ監視なしでタスクを実行するため、安全を確保するためのセキュリティ制約が設けられています。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みアクセス権はコードを作成するために使うブランチ**のみ**に限定されます。
- Cloud agent は GitHub Actions 内で動作し、作業用の独立した一時環境を作成します。
- GitHub Actions のワークフローを実行するには、人による承認が必要です。
- MCP サーバーなど、外部リソースへの[アクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、コードにドキュメントを追加するために GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR) を確認します。github.com の標準的な PR インターフェイスを使ってレビューします。

> [!NOTE]
> PR を確認すると、GitHub Copilot がファイアウォールでブロックされているという警告が表示される場合があります。これは想定された動作です。Copilot は、外部 MCP サーバーへの接続など、外部リソースへのアクセスが既定で制限されています。必要に応じて [Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com の自分のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** などのタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** が付いています。その場合は、Copilot の作業が完了するまで待ちます。数分かかる場合があるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. プルリクエストの準備ができたら、**Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しい TSDoc のドキュメントコメントなど、更新されたコードとドキュメントを確認します。具体的な変更内容は状況によって異なります。

    変更を確認しながら、TSDoc のドキュメントコメント、TypeScript の規約、コメント ヘッダーを探します。これらは演習 2 の最初に確認したカスタム指示ファイルに基づいています。

6. 更新内容を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. 承認待ちのワークフローを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローの承認と実行](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが追加されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot とのプルリクエスト作業は一方通行ではありません。チーム メンバーに依頼するのと同じように、プルリクエストまたはコードのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、対応する新しいセッションを開始します。結果は確定的ではないため、必ずしも適切な依頼文を指定できるわけではありません。Copilot に更新を依頼する例を紹介します。

- 各コード ファイルの先頭に、内容を簡潔に説明するコメント ヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の例などを参考に、コードベースに追加したいドキュメントの変更をコメントで依頼します。ほかのユーザーと同じように **@copilot** をメンションします。
2. **View Session** を選択し、Copilot の作業を確認します。Copilot が更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

    ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の変更が完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択して変更をレビューします。

納得できるまで、Copilot との反復作業を続けます。問題がなければ、ドラフト PR をレビュー可能な状態に変更して、main ブランチにマージします。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページへの関連ゲーム表示を実装した Issue に対し、Copilot が作成した PR を確認します。

1. GitHub.com の自分のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新内容を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
6. 承認待ちのワークフローを示す表示を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローの承認と実行](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック欄にワークフローが追加されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
9. **省略可能:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名、たとえば **copilot/fix-8** に置き換えます)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を実装しました。これまでと同様に、Copilot cloud agent に更新を依頼して反復できます。たとえば、表示する関連ゲームの数を変更したり、コメント ヘッダーと TSDoc のドキュメントコメントを追加したりできます (この Issue はカスタム指示を更新する**前**に割り当てたことを思い出してください)。**Conversation** タブでコメントすると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントで実装した機能をレビューします。この PR には、演習 3 で依頼した高コントラスト モードと、演習 4 のミッション コントロールから追加を依頼したライトモードの両方が含まれるはずです。

1. GitHub.com の自分のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストには **[WIP]** が付いています。その場合は、作業が完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注意します。
   - モード切り替え用の UI コンポーネント。
   - ユーザー設定を保存するためのローカル ストレージの使用。
   - 高コントラスト モードとライトモード用の CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)。
   - モードの切り替えを管理する JavaScript/TypeScript コード。

6. 更新内容を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. 承認待ちのワークフローを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローの承認と実行](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが追加されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
10. **省略可能:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えます)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次にアプリケーションを起動し、ブラウザーで高コントラスト モードとライトモードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ用カスタムエージェントが、アクセシビリティのベスト プラクティスに沿って機能を実装するよう Copilot をガイドしたことを確認します。アクセシビリティ上の問題や改善点がある場合は、前の PR と同様にコメントで **@copilot** をメンションし、修正を依頼できます。

## 省略可能な演習 — 委任を続ける

Cloud agent は、バックログの実際の項目を任せて、別の作業に集中するときに便利です。習慣にするために、リポジトリにいくつか Issue を作成して Copilot に割り当ててみましょう。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent のコースを完了しました。このレッスンを通じて、次のことを行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認し、cloud agent の出力への影響を調べました。**
- **Issue を Copilot cloud agent に割り当て、環境を設定して非同期で計画・実行する様子を確認しました。**
- **アクセシビリティ用のカスタムエージェントを作成して使用し、高コントラストとライトモードの切り替えを追加しました。**
- **Copilot agents ページをミッション コントロールとして使用し、アクセシビリティ セッションを監視して方向付けしました。**
- **Copilot が作成したプルリクエストをレビューして改善し、`@copilot` に変更を依頼し、ワークフローを承認しました。**

## 振り返りと次のステップ

Cloud agent のコースは完了です。さらに学習を続ける場合は、ほかのコースで異なる環境から関連するシナリオを体験できます。

- 🖥️ **[VS Code コース](../../vscode/)** — IDE から Copilot Chat のエージェント モードと MCP 統合を試します。
- 💻 **[CLI コース](../../cli/)** — Plan と Autopilot モード、エージェント スキル、QA カスタムエージェント、Playwright MCP、Agent Merge、任意の `/delegate` による cloud agent への連携を使い、ターミナルからレビュー済みの変更を2つリリースします。

自分のリポジトリでは、次のような作業も試せます。

- リファクタリングやテスト カバレッジの Issue を cloud agent に割り当てる。
- 別の分野向けのカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
