---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルタリング、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、指示ファイルの更新、スキルの更新、品質保証 (QA) プロファイル、テストは、すべて1つのブランチに保存されています。まとめてレビューし、pull request (PR) を作成します。星評価の PR は自分でマージしましたが、今回は **Agent Merge** にマージの作業を任せます。

> [!NOTE]
> 通常は機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡略化するため、フィルタリングと品質ワークフローに関する作業を1つの会話とブランチにまとめ、この PR に含めました。

このレッスンでは、次の作業を行います。

- Agent Merge と、マージまでのライフサイクルを自動化する方法を確認します。
- 機能の変更全体と検証の根拠を確認します。
- フィルタリングの PR を作成します。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルタリングのワークフローでは、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys は、マージを許可する権限を開発者が保持したまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、pull request をマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題 (失敗した継続的インテグレーション (CI) チェック、レビューコメントへの対応、必要な場合のリベースなど) を解決し、リポジトリで許可されていれば GitHub の自動マージを有効にします。

ここまでは、開発者自身が **Merge pull request** を選択してきました。Agent Merge にその作業を任せることもできます。Agent Merge を有効にする前に、変更をレビューし、問題がないことを確認してください。

## Agent Merge で PR を管理する

コードの作成が終わったら、変更をレビューし、PR を作成して、残りの処理を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを実行して PR を作成します。
5. `/pr agentmerge` コマンドを実行して Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションの状況を監視します。すべて成功すると、マージを実行します。
7. Copilot から `"PR #14 was squash-merged successfully."` のようなメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリの設定、権限不足を回避しません。処理が止まった場合は、報告された理由を確認し、リポジトリで許可されていれば、レビュー済みの PR を手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、pull request の処理など、開発プロセスの一部を自動化しました。このレッスンでは、次の作業を行いました。

- Agent Merge と、マージまでのライフサイクルを自動化する方法を確認しました。
- 機能の変更全体と検証の根拠を確認しました。
- フィルタリングの PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、省略可能なクラウドへの委任に使う[便利な Copilot CLI スラッシュコマンドを確認します][next-lesson]。

## リソース

- [Copilot CLI で pull request を管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
