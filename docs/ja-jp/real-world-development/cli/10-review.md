---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2つの pull request、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連のワークフローを進めました。次の作業を行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、シード済みのフィルタリング Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初の pull request (PR) を手動でマージしました。
- フィルタリング Issue から作業を始め、Plan モードで実装方法を決め、Autopilot モードで構築して、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright のブラウザー操作を提供する Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリング機能を確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、テストカバレッジ、スキルの結果、ブラウザーでの検証結果を評価しました。
- フィルタリングの変更全体をレビューし、Agent Merge を許可して PR をマージしました。
- コンテキスト、モデル、共有、省略可能なクラウドへの委任に使うスラッシュコマンドを確認しました。

## 作成したもの

このワークショップでは、更新済みの `main` からそれぞれ別のブランチを作成し、2つの PR を作成しました。

1. **星評価:** 既存の `starRating` と未評価の状態をゲームカードに表示します。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズして、QA プロファイルと関連テストを追加します。

フィルタリングを計画して PR を作成するまで、同じ会話とブランチを使いました。このワークショップを簡略化するため、その作業を1つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を介した Copilot のブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA はこれらの結果と、要件およびテストカバレッジのレビューをまとめてから PR を作成できる状態か確認しました。

追加するテストは実際の不足を補うものである必要があります。QA 実行で新しいテストが不要と判断されても、問題ではありません。マージを許可する前に、コードと検証結果を確認してください。変更を加えた場合は、影響を受ける検証を再実行します。

## ベストプラクティス

Copilot に与えるコンテキストとツールは、作業結果に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズは会話を越えて再利用し、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは繰り返し実行するタスクを説明し、カスタムエージェントは専門家の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選択します。作業前に方法を検討する場合は **Plan**、範囲の限られた変更でやり取りを続ける場合は **Interactive**、明確なタスクを任せる場合は **Autopilot** を使います。定型的な編集には高速なモデルを選び、複雑な作業にはより高性能なモデルを選びます。

ツールやインフラストラクチャと同じように、コンテキストも重要です。何を、なぜ、どのように作成するかを明確に伝えると、結果が大きく変わります。

## さらに確認する

主要なワークフローを完了しました。Copilot CLI の次の機能も確認できます。

- `/review` を使って、コードレビューエージェントに変更を分析させます。
- `/rubber-duck` を使って、問題を説明し別の視点を得ます。
- `/fleet` を使って、独立したサブタスクを並行して調整します。
- `/worktree` を使って、別のタスクを分離します。
- `/delegate` を使って、Copilot cloud agent にタスクを委任します。

## 次のステップ

ツールの使い方を上達させるには、継続して使うのが最善です。実際のコード、個人的なプロジェクト、以前から作りたいと思っていた小さなアプリなどで活用してください。チームと学んだことを共有し、他の人からも学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、省略可能な[Foundry Backer Concierge シリーズ][foundry]に進んでください。他の環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI で pull request を管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
