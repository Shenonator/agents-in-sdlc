---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2つのプルリクエスト、再利用可能なカスタマイズ、追加のリソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローで GitHub Copilot CLI を使いました。次の操作を行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、用意されたフィルター機能の Issue を見つけました。
- 星評価を追加し、転送したブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター機能の Issue から始め、Plan モードでアプローチを決め、Autopilot モードで構築して、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの検証を評価しました。
- フィルター機能全体をレビューし、フィルター機能の PR で Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意のクラウド委任に使うスラッシュコマンドを確認しました。

## 作成したもの

このワークショップには、更新済みの `main` からそれぞれ別のブランチで作成する、2つの PR の節目があります。

1. **星評価**: 既存の `starRating` と、評価がない場合の明示的な表示をゲームカードに追加します。
2. **フィルター機能と品質ワークフロー**: フィルター機能を実装し、指示を更新して機能に適用します。`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを含めます。

フィルター機能の計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを簡潔に進めるため、これらの作業を1つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を介した Copilot のブラウザー操作という複数の方法でコードを確認しました。`quality-checks` スキルでユニットテスト、lint、型チェックを実行し、追加した形式で結果を報告しました。QA はこれらの結果と、要件およびテスト カバレッジの確認をまとめ、PR 作成前の判断材料を提供しました。

追加するテストは、実際に不足している部分を補うものであるべきです。QA の実行で新しいテストが不要と判断される場合もあります。マージを許可する前に、コードと検証の証拠を確認し、変更後は影響する検証をやり直してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業の結果に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。会話をまたいでカスタマイズを再利用し、チームのニーズに合わせて調整できます。指示は標準を定め、スキルは繰り返し実行する作業を説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。**Plan** は実装前にアプローチを検討するとき、**Interactive** は対象を絞った変更に関与し続けたいとき、**Autopilot** は範囲が明確なタスクに適しています。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。何を、なぜ、どのように構築したいかを明確に伝えると、結果が大きく変わります。

## さらに確認する

主要なワークフローは完了しました。ほかにも確認できる CLI 機能があります。

- `/review` でコードレビュー エージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を相談し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを依頼します。

## 次のステップ

どのツールも、使い続けることが上達への近道です。プロダクション コード、趣味のコード、何年も作りたいと思っていた小さなアプリなどで活用してください。チームで学びを共有し、お互いの経験から学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張したい場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]をご覧ください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のカスタマイズ][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
