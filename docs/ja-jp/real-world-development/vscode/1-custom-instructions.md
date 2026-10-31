---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。作業を特定の方法で進める必要がある場合や、Copilot に背景情報を伝えたい場合は、そのコンテキストを参照できるようにします。Copilot と特定のコンテキストを共有する方法はいくつかあります。中でも重要なのは[指示ファイル][instruction-files]です。コード生成のガイダンスを提供します。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパスを対象にした指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどのように伝わるかを確認します。
- 現在の指示を使って、フィルター機能の最初のデータ部分 (パブリッシャー ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードが新しい標準に従う様子を確認します。
- 次の演習で Copilot が更新後のガイダンスを使えるよう、指示の更新とフィルター機能の一部を `main` にコミットします。

> [!CAUTION]
> 生成コードが、設定した標準の一部に従わない場合があります。Copilot は確率的に動作します。この演習では、出力の文字単位の一致ではなく、指示を更新した後の動作の**傾向**を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- データ層には必ずユニットテストを用意します。
- UI はダークモードにして、現代的な印象にします。
- コードのドキュメントには TSDoc のドキュメントコメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使うと、Copilot がこれらの開発手法に沿って作業できるよう、適切な情報を提供できます。

### カスタム指示

カスタム指示を使ってコンテキストや設定を伝えると、Copilot がコーディング スタイルや要件を理解しやすくなります。コーディング規約、ライブラリ、コードに含めたいコメントの種類などを指定でき、提案やコード スニペットをより適切なものにできます。リポジトリ全体、または特定のファイル種別に対して指示を作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。Copilot Chat や CLI でのほとんどのリクエストに関係するプロジェクトレベルの情報を記載します。たとえば、技術スタック、開発中の内容の概要、ベスト プラクティス、全体に適用するガイダンスなどです。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイル種別を対象に作成できます。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストの追加といった作業向けのガイダンスを用意できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示と、`applyTo` フロントマターで対象を指定した `*.instructions.md` ファイルの両方を使います。
>
> また、Copilot は AGENTS.md や CLAUDE.md など、ほかの標準形式の指示ファイルにも対応しています。[指示ファイルのサポート][custom-instructions-support]をご覧ください。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成について詳しく説明することはこのワークショップの範囲外ですが、サンプル プロジェクトには代表的な構成を示す例が含まれています。基本的には、次の点を意識してください。

- `copilot-instructions.md` には、プロジェクトの説明、構成、全体に適用するコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` ファイルは、ユニットテスト、Astro コンポーネント、データ層などのファイル種別や、特定のタスク向けの詳細な指示に使います。
- 自然な言葉で、分かりやすいガイダンスを書きます。コードの良い例と避けるべき例を示します。

指示ファイルを作成する唯一の正解はありません。AI の使い方と同様に、プロジェクトやシナリオに合わせて試しながら調整してください。

> [!TIP]
> GitHub Copilot を使うすべてのプロジェクトで、充実した指示ファイルを用意することをお勧めします。このプロジェクトの指示ファイルを確認すると、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあることに気付くでしょう。
>
> Copilot を使って指示ファイルを生成することもできます。方法は利用する環境によって異なります (たとえば、VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など)。関連する場合は、各コースで説明します。
>
> テンプレートや出発点を探していますか。[awesome-copilot][awesome-copilot] には、指示ファイル、カスタムエージェント、その他のリソースが多数あります。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリには、中心となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルが含まれています。エディターまたは GitHub Web UI で開いて内容を確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には、**GitHub Actions Workflows** に関するガイダンスがあります。これらの指示は、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、リポジトリのルートからの相対パスとして、指示の対象となるファイルを決める glob を指定していることを確認します。ここでは、`**/*.test.ts` に一致する TypeScript のテスト ファイルが対象です。
5. このプロジェクトでユニットテストを作成する際の指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの別の指示ファイルや、プロジェクト内の既存ファイルへのリンクに注目してください。これにより、大きな指示を小さく再利用可能なファイルに分け、Copilot がコードを生成するときに参照する例を示せます (パスはリポジトリのルートではなく、指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc のドキュメントコメントとファイル ヘッダーのルールを追加します。

## 変更用のブランチを作成する

ベスト プラクティスに従い、作業用のブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、該当する手順で説明します。

カスタム指示の影響を確認するには、まず現在の指示ファイルがある状態でプロンプトを送信します。その後、指示を更新し、同じプロンプトを再送信して違いを確認します。

1. 前の演習で開いたエディター タブを閉じ、Copilot が必要なコンテキストだけを使うようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に伝えます。
3. Chat ビューのエージェントのドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べてコードを更新します。多くの場合、ヘルパー ファイルとテストの両方が変更されます。
6. 提案されたヘルパーが、`db` クライアントを最初の引数として受け取り、型付きのパブリッシャー配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層規約に基づいています。
7. 提案されたコードに TSDoc のドキュメントコメントとファイルレベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する可能性があります。その場合も問題ありません。指示を更新した後に動作の一貫性が改善されることが、この演習の目的です。

## リポジトリ全体に適用する標準を追加する

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。コードの提案を改善するために、リポジトリのコーディング標準を記述しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを見つけます。プロジェクトのコーディング標準が説明されていますが、コード内のドキュメントに関するルールがありません。そのため、生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の標準のすぐ下に次の Markdown 行を追加し、ファイル ヘッダーと TSDoc のドキュメントコメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ レベル (`.github/copilot-instructions.md`) に全体向けの指示を作成することも、言語、ファイル種別、タスク向けの `*.instructions.md` ファイルを作成することもできます。今回追加したドキュメントコメントのルールのように、プロジェクト全体に適用する標準にはリポジトリ レベルの指示ファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話をクリアします。
2. `src/lib/publishers.ts` をクリックし、Copilot が適切なファイルに注目するようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されたことを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが追加されたことを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

これで新しいプロジェクト標準に従うよう Copilot をガイドし、次の演習で使うコードに適用できました。

## 指示をコミットしてブランチをプッシュする

指示ファイルもリポジトリ内のほかの資産と同じように、ソース管理で管理します。変更をコミットしてブランチをプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナル ウィンドウを開きます。
2. ターミナルで次のコマンドを実行し、指示とヘルパーの変更が存在することを確認します。

   ```bash
   git status
   ```

3. ターミナルで、指示の更新と Copilot がヘルパーの基盤として変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルでブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の Issue と関連付けてプルリクエストを作成します。手動で行う代わりに、すでにプロジェクトに接続されている GitHub ツールを使って Copilot に依頼できます。Issue を見つけ、Issue をクローズする PR を作成してから、マージを依頼します。

> [!NOTE]
> Copilot に GitHub 上の操作を依頼するのはこれが初めてです。プロジェクト テンプレートでは、GitHub ツールがワークスペースにあらかじめ接続されています。Copilot が初めてツールを使うとき、VS Code から GitHub へのサインインを求められる場合があります。表示される手順に従ってください。この接続の仕組み (Model Context Protocol) は、後の演習で説明します。

1. codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関係する Issue を探し、現在のブランチから PR を作成して、指示の更新と新しいパブリッシャー ヘルパーの基盤の両方を説明するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. Copilot が代わりに GitHub 操作を実行する許可を求めたら、コマンドを確認して、適切な場合は **Allow** を選択します。
6. PR が作成されたら、次のプロンプトで PR をマージしてローカル ブランチを `main` に戻し、最新のコードを取得するよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が代わりに GitHub 操作やシェル コマンドを実行する許可を求めたら、コマンドを確認して、適切な場合は **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

Copilot がこのプロジェクトのコンテキストを指示ファイルからどのように取得するかを確認し、VS Code の Copilot Chat を使って次の操作を行いました。

- 既存の指示を使ってコード生成プロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードが新しい標準に従う様子を確認しました。
- 次の演習で作業を続けられるよう、指示の更新とヘルパーの基盤を `main` にコミットしました。

次は[エージェントモード][next-lesson]で指示を活用し、Copilot にコードベース全体にまたがる新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのより良いカスタム指示を作成する5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
