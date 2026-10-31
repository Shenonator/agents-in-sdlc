---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの最も簡単な更新でも、通常は複数のファイルを変更し、テストを実行するなどの作業が必要です。開発者は通常、必要なファイルを見つけて変更し、テストを実行し、デバッグし、変更し忘れたファイルを探し、さらに更新します。作業は次々と続きます。

こうした場面で役立つのが Copilot エージェントモードです。

Copilot エージェントモードは、IDE 内でより自律的に作業するよう設計されています。開発者のように既存のプロジェクト構成を調べ、必要な更新を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。エージェントモードを使って Web サイトに新機能を追加する方法を確認しましょう。

> [!NOTE]
> エージェントモードと cloud agent は名前が似ていますが、異なる作業体験を提供します。エージェントモードは IDE 内で動作し、すばやいフィードバックと対話を可能にします。Cloud agent はチーム メンバーのように非同期で作業し、Issue やプルリクエストを通じてやり取りします。

この演習では、次の操作を学びます。

- Copilot エージェントモードでプロジェクトを調べ、関連ファイルを特定し、複数のファイルにまたがる変更を行う方法。
- GitHub Copilot エージェントモードで UI とデータ層にまたがる新機能を実装する方法。
- Copilot エージェントモードが生成した変更とテストをレビューしてからコードベースにマージする方法。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリーやパブリッシャーで絞り込めるようにします。前の演習でパブリッシャー ヘルパーを追加しました。残りのデータ層、UI、テストの作業を Copilot エージェントモードで完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを調べ、現在の機能を確認しましょう。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディング プラットフォームです。ゲームを一覧表示し、詳細を表示できます。このサイトは単一の Astro アプリで、ビルド時にページを静的 HTML として生成します。ページはフロントマターで Drizzle ORM を通じてローカルの SQLite データベースを直接クエリします。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセス ヘルパーは `src/lib/` にあり、インタラクティブな機能は標準 DOM API を使う小さな Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。次の手順で GitHub Codespace から実行できます。

1. codespace に戻ります。前の演習の後なので、`main` にいるはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナル ウィンドウを開きます。
3. フィルター機能用の新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL を示すメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を選択して Web サイトを開きます。

> [!NOTE]
> codespace では、Codespace ターミナルの localhost URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは codespace の Web サーバーをホストするためのプライベート トンネルです。

### Web サイトを確認する

Web サイトが起動したら、その機能を確認します。主な機能は次のとおりです。

- **ホームページ**: ボードゲームの一覧とタイトル、画像、説明を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリーなど、詳細情報が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、該当する手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加して改善したいと考えています。まずバックログを確認しましょう。テンプレートからプロジェクトを設定すると、Issue のバックログが自動で作成されます。GitHub Copilot に項目を表示してもらいます。

1. Chat ビューのエージェントのドロップダウンから **Agent** を選択します。**Agent** は自律的に計画を立て、複数ファイルに変更を加え、ターミナル コマンドやツールを実行します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップの作成時に Claude Sonnet 4.5 を使ったため、比較的一貫した体験になるよう手順に含めています。ただし、LLM は確率的に動作するため、記載されたものと異なる提案が表示される場合があります。これは正常な動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP コマンドを使う場合がありますが、タスク自体は完了できるはずです。

3. 次のプロンプトを Copilot に送信して、Issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot がユーザーに最も役立つと判断した項目に基づいて、優先順位も付けています。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、作業のコンテキストを Copilot に伝える指示ファイルを確認しましょう。Web サイトに機能を追加する方法に関する情報が含まれる[ユーザー インターフェイス (UI) の指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティに関する原則。
   - 次のようなファイル種別に特化した指示へのリンク。
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの場所からの相対パスです。指示を再利用し、複雑な内容を小さく管理しやすい単位に分け、例やテンプレートを示すことができます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の3種類の変更が必要です。

- データ層 (`src/lib/`) に残りのフィルター ロジックを追加または調整します。
- フィルターの動作を確認するテストを追加または更新します。
- ゲーム一覧ページにフィルター UI を追加します。

さらに、コードベースにマージする前にテストを実行し、成功させる必要があります。Copilot エージェントモードを使えば、これらの作業を依頼できます。機能を追加しましょう。

1. 現在の Copilot の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、UI 指示ファイルを指定します。

   ![UI 指示ファイルを選択する画面の例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントのドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. モデルが引き続き **Claude Sonnet 4.5** であることを確認します。
5. バックログ内の関連 Issue に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、必要なファイルを探して作業を始めます。データ層のヘルパー、UI、テストを見つけて変更し、テストを実行します。

   ![Copilot がプロジェクト ファイルを調べる画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行うため、タスクやコードベースの複雑さによっては数分かかります。作業中、コード エディターに **Keep** と **Undo** ボタンが表示されることがあります。Copilot の作業が終わると、すべての変更に対して **Keep** または **Undo** が表示されます。作業中に選択する必要はありません。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![テストを実行してよいか確認する Copilot Chat のダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に停止したり、テストが失敗したりする場合があります。問題ありません。Copilot はタスクが完了し、エラーが検出されなくなるまで、コード生成とテストを繰り返します。

   ![Copilot エージェントモードの完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やほかの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを起動したブラウザーに戻り、新機能を確認します。
11. すべてが動作し、コードのレビューも終わったら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

おめでとうございます。この演習では、GitHub Copilot エージェントモードで Tailspin Toys の Web サイトに機能を追加する方法を確認しました。次のことを学びました。

- GitHub Copilot エージェントモードで UI とデータ層にまたがる新機能を実装する方法。
- Copilot エージェントモードでプロジェクトを調べ、関連ファイルを特定し、複数のファイルにまたがる変更を行う方法。
- Copilot エージェントモードが生成した変更とテストをレビューしてからコードベースにマージする方法。

次は [Playwright MCP サーバーで機能をテスト][next-lesson]し、プルリクエストを作成します。

### 追加の探索演習 - ページネーションを実装する

ゲームの一覧が増えると、ページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションを実装するよう依頼します。次の点を検討してください。

- 既存の指示ファイルを使うなど、現在のベスト プラクティスに従います。
- ページサイズをユーザーが選べるようにするか、固定値にするかなど、ページネーションの方法を検討します。
- 希望どおりの実装に必要なガイダンスを、プロンプトに含めます。
- Copilot に変更を依頼したりコンテキストを伝えたりしながら、何度か調整する必要があるかもしれません。Copilot と作業する場合、この反復は通常の流れです。

## リソース

- [Copilot の Ask、Edit、Agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
