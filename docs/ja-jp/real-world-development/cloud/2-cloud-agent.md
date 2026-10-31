---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加などを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト pull request (PR) を作成します。これにより、時間のかかる作業を任せて開発プロセスを加速し、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って、次の内容を確認します。

- コード生成環境をカスタマイズする方法。
- 操作を安全に行う方法。
- Issue の範囲を明確にする重要性。
- Issue を Copilot に割り当てる方法。

## シナリオ

Tailspin Toys には解消したい技術的負債があります。最初のサイトを開発した委託先が、十分なドキュメントを残していません。まず、アプリケーションのエクスポートされたすべての関数に TypeScript 用のドキュメントコメント形式である TSDoc のコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する他のゲームを表示する機能を求めています。完成度の高い実装はまだ必要ありませんが、UX の受け入れテストに使える状態にしたいと考えています。現在は作業の妨げになっていますが、他に優先度の高い Issue があります。

どちらも優先順位が下がりやすい作業の例です。Copilot cloud agent に割り当てると、非同期で作業を進める間に開発者は他のタスクに取り組み、後から成果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドで作業できます。[GitHub Issue を Copilot に割り当てる][assign-issue]など、複数の方法で作業を依頼できます。割り当てると、Copilot は進行状況を追跡するドラフト pull request を作成し、環境をセットアップして、タスクに取り組み始めます。作業中も完了後もセッションを確認できます。提案された解決策のレビュー準備ができると、pull request で通知されます。

## 適切な範囲の指示が重要

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単そうな作業でも、詳しく見ると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペアプログラマーとして扱い、段階を踏んで学び、試し、適応させます。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習ではカスタム指示と、その使い方を確認しました。カスタム指示の演習をすでに終えている場合は、`.github/copilot-instructions.md` が機能していることを確認済みです。まだの場合は、ここで内容を簡単に確認します。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認し、後でその影響を確認できるようにします。

リポジトリの GitHub Web ユーザーインターフェイス (UI)、または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` - **Code standards** セクション、特に TypeScript の規約と TSDoc のドキュメントコメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` フロントマターが `**/*.test.ts` ファイルに指示を適用することを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、これらの指示ファイルによる TSDoc のコメント、TypeScript の規約、コメントヘッダーを確認してください。後の演習で pull request をレビューするときにも、改めて確認します。

## Copilot cloud agent の開発環境を設定する

誰がコードを書く場合でも、通常は開発環境とセットアップスクリプトが必要です。タスクを Copilot に割り当てる場合も同様です。Copilot はソフトウェアエンジニアのように作業します。

Cloud agent は作業環境として [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定した[専用セットアップワークフロー][setup-workflow]を作成して、作業開始前に実行するツールや依存関係を指定できます。このラボの流れを簡単にし、学習に集中できるよう、あらかじめ設定されています。Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、単一の Astro アプリ用にマイグレーションとシードを済ませた SQLite データベースを利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # セットアップ手順に必要な最小限の権限を設定します。Copilot は操作用のトークンを別途受け取ります。
    permissions:
      # セットアップ手順内でリポジトリをクローンする場合 (依存関係のインストールなど) は、contents: read 権限が必要です。クローンしない場合は、手順の完了後に Copilot が自動でクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ローカルの SQLite データベースをマイグレーションしてシードし、ビルドとテスト用のデータを準備します。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同様ですが、重要な点がいくつかあります。

- `copilot-setup-steps` という単一のジョブが含まれています。このジョブは、Copilot が pull request の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot の実行を待たずに、ワークフローが成功するか確認できます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、情報が古かったり、まったくなかったりするプロジェクトは少なくありません。こうした技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメント作成にも役立つため、cloud agent に割り当てるのに適した Issue です。バックグラウンドで必要なドキュメントを作成し、後の演習でその内容をレビューします。

1. 新しいブラウザータブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成された pull request を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものになります。新しい pull request が一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が To Do リストを作成します。

> [!NOTE]
> Copilot の To Do リストが pull request に表示されるまで、数分かかることがあります。Copilot は環境を作成し (前に説明したワークフローを実行します)、プロジェクトを分析して、問題に取り組む方法を決めています。

13. リストを確認し、Copilot が実行するタスクを把握します。
14. pull request のタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示が更新されない場合は、ウィンドウを更新してください。

16. ライブセッションをスクロールして、Copilot の作業を確認します。コードを調査して状態を把握し、計画を考えて判断した後、コードを作成する様子を確認できます。

この作業には数分かかる場合があります。Copilot cloud agent の主な目的は、非同期でタスクを実行し、開発者が他の作業に集中できるようにすることです。この機能を活用するため、もう1つのタスクを cloud agent に割り当てた後、アプリケーションに機能を追加する作業を進めます。

## ゲーム詳細ページに関連ゲームのセクションを追加する

GitHub Copilot cloud agent の利点の1つは、作業を分担できることです。デザインチーム向けの関連ゲームのセクションは時間のかかる作業ではないかもしれませんが、その時間を別の作業に使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことがわかります。

![Copilot が Issue に対応していることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼にも取り組み始めました。Copilot cloud agent はソフトウェアエンジニアと同じように作業するため、常に監視する必要はありません。完了後にレビューできます。次は、カスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使って、Issue を Copilot に割り当て、非同期で作業させる方法を確認しました。cloud agent を使うと、技術的負債の解消、新機能の作成、別のフレームワークへのコード移行などに取り組めます。

次の内容を確認しました。

- コード生成環境をカスタマイズする方法。
- 操作を安全に行う方法。
- Issue の範囲を明確にする重要性。
- Issue を Copilot に割り当てる方法。

cloud agent がバックグラウンドで作業している間に、次はカスタムエージェントを作成して使用します。[Copilot cloud agent は Model Context Protocol (MCP) サーバー][cloud-agent-mcp]とカスタム指示も利用できます。カスタム指示については前の演習で確認しました。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
