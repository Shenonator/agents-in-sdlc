---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。作業を特定の方法で進める必要がある場合や、Copilot に背景情報を伝えたい場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot が作業の内容だけでなく方法も理解できるようにガイダンスを提供できます。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパスを対象にした指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどのように伝わるかを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code および CLI のコースとは異なり、ここでは変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub Issue に対して非同期で動作するため、リアルタイムで影響を並べて示すのは難しいためです。このコースの後半で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認します。

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

このリポジトリに含まれる指示ファイルを確認します。中心となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には、**GitHub Actions Workflows** に関するガイダンスがあります。これらの指示は、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、リポジトリのルートからの相対パスとして、指示の対象となるファイルを決める glob を指定していることを確認します。ここでは、`**/*.test.ts` に一致する TypeScript のテスト ファイルが対象です。
5. このプロジェクトでユニットテストを作成する際の指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの別の指示ファイルや、プロジェクト内の既存ファイルへのリンクに注目してください。これにより、大きな指示を小さく再利用可能なファイルに分け、Copilot がコードを生成するときに参照する例を示せます (パスはリポジトリのルートではなく、指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc のドキュメントコメントとファイル ヘッダーのルールを追加します。

## リポジトリ全体に適用する標準を追加する

ここで編集するのは、コード内のドキュメントを TSDoc のドキュメントコメントとファイルレベルのコメント ヘッダーで記述するという、プロジェクト全体のルールです。後の演習で cloud agent が Issue に取り組むとき、このルールを参照します。

編集する前に、作業用のブランチを作成します (cloud agent は Issue の対象ブランチにある指示を読み取りますが、変更を整理してコミットしておきます)。

1. codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前の説明のとおり、`.github/copilot-instructions.md` はプロジェクトレベルの情報を Copilot に伝えます。コードの提案を改善するために、プロジェクトのコーディング標準を記述しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを見つけます。プロジェクトのコーディング標準が説明されていますが、コード内のドキュメントに関するルールがありません。そのため、生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の標準のすぐ下に、次の Markdown 行を追加し、ファイル ヘッダーと TSDoc のドキュメントコメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ レベル (`.github/copilot-instructions.md`) に全体向けの指示を作成することも、言語、ファイル種別、タスク向けの `*.instructions.md` ファイルを作成することもできます。今回追加したドキュメントコメントのルールのように、プロジェクト全体に適用する標準にはリポジトリ レベルの指示ファイルが適しています。

## 指示の変更をコミット、プッシュ、マージする

cloud agent は、Issue の対象ブランチにある指示ファイルを読み取ります。次の演習で Issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。そのため、cloud agent が指示を使えるように、変更を `main` に取り込む必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。次の演習で Issue に取り組むとき、cloud agent はこの指示を参照します。

> [!TIP]
> このワークショップでは `main` で直接作業することもできます。その場合はブランチを作成せず、`main` に直接コミットしてください。ここでは、実際のプロジェクトでの作業方法を体験できるよう、ブランチを作成しています。

## まとめと次のステップ

Copilot がこのプロジェクトのコンテキストを指示ファイルからどのように取得するかを確認し、リポジトリ全体に適用する標準を追加しました。このコースの後半で cloud agent が作成するプルリクエストを確認するときに、この標準が使われていることを確認できます。

次は、[Copilot cloud agent に最初の Issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのより良いカスタム指示を作成する5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
