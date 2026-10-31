---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。作業を特定の方法で進める必要がある場合は、Copilot がそのガイダンスを参照できるようにします。[指示ファイル][instruction-files]には、求めるコードの内容だけでなく、その構成方法も記述できます。フィルター機能を構築したので、Copilot が参照した指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次の操作を行います。

- リポジトリの指示とパスを対象にした指示ファイルがエージェントにどのように伝わるかを確認します。
- コーディング標準に従うよう指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- コメントにはコードを繰り返すのではなく、意図や自明でない判断を説明します。
- `db/` および `src/lib/` のエクスポートされた関数では、インジェクション可能な `db` 引数がある場合はそれも含め、目的、パラメーター、戻り値を TSDoc/JSDoc で記述します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連するコードの変更時にコメントも最新の状態にします。
- 既存の書式設定と lint のガイダンスを維持します。

指示ファイルを使うと、Copilot がこれらの開発手法に沿って作業できるよう、適切な情報を提供できます。

## 指示ファイル

カスタム指示を使ってコンテキストや設定を伝えると、Copilot がコーディング スタイルや要件を理解しやすくなります。コーディング規約、ライブラリ、コードに含めたいコメントの種類などを指定でき、提案やコード スニペットをより適切なものにできます。リポジトリ全体、または特定のファイル種別に対して指示を作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。ほとんどのリクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` ファイルには、特定の言語、ファイル種別、タスク向けのガイダンスを記載します。

> [!NOTE]
> ほかの指示形式への対応状況は、利用する環境によって異なります。特定の形式に依存する前に、[カスタム指示のサポート リファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、すぐに使える指示ファイルが用意されています。変更を加える前に、既存の内容と、その影響を確認しましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングに関するガイダンスを確認します。このリポジトリで Copilot とやり取りするたびに、これらの指示が適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、指示の対象となるファイルを決める glob を指定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存のプロジェクト ファイルへのリンクを確認します。大きな指示を小さく再利用可能なファイルに分け、Copilot がコードを生成するときに参照する例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルは良い出発点ですが、まだ不足があります。コアの `copilot-instructions.md` ファイルを変更し、新しく生成される TypeScript に TSDoc コメントを追加するようにします。

1. `.github/copilot-instructions.md` で、**Code formatting guidance** というセクションを見つけます。35 行目付近にあります。
2. 最後の箇条書きとして、次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更は自動的に保存されます。

## 更新したガイダンスを適用する

Copilot CLI は会話を始めるときに、リポジトリの指示を読み込みます。更新した指示を適用するには、機能のコンテキストを保ったままフィルター機能の会話を終了して再開します。

1. フィルター機能の会話が開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチでリポジトリのルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合は、フィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新後のガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからどのように情報を取得し、新しい標準を機能に適用するかを確認しました。このレッスンでは、次の操作を行いました。

- リポジトリの指示とパスを対象にした指示ファイルがエージェントにどのように伝わるかを確認しました。
- コーディング標準に従うよう指示ファイルを更新しました。
- 指示ファイルの影響を確認しました。

次は、[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して実施します。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
