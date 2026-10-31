---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更を計画し、コードを作成し、動作をテストし、pull request をレビューし、エージェントを使って作業を管理する一連の流れを反復します。

## シナリオ

架空の会社 Tailspin Toys に新しく加わった開発者として作業します。Tailspin Toys は、開発者をテーマにしたボードゲームのクラウドファンディングを提供しています。チームのバックログは GitHub Issue として用意されているため、取り組む項目を選べます。バックログには、フィルタリングやページネーションなどの機能開発に加えて、アクセシビリティやコーディング標準の改善も含まれています。

## 作業環境を選択する

GitHub Copilot は、開発者が作業する環境で利用できます。開発方法に合った環境を選択してください。各ワークショップは、それぞれ独自のセットアップ手順から始まり、共通の Tailspin Toys シナリオを使います。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan と Autopilot の各モード、指示、スキル、カスタムエージェント、Playwright のブラウザー操作を支援する Model Context Protocol (MCP) サーバー、Agent Merge、実用的なスラッシュコマンドを試します。

### [GitHub Copilot app][app]

デスクトップアプリでエージェントの並列セッションを実行し、セッションモードを切り替え、Canvas で共同作業し、GitHub Issue と pull request を管理します。

### [GitHub Copilot cloud agent][cloud]

非同期エージェントに GitHub Issue を割り当て、作業の方向を調整し、進行状況を監視して、作成された pull request をレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに、Copilot Chat のエージェントモード、Model Context Protocol (MCP) サーバー、カスタムエージェントを利用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
