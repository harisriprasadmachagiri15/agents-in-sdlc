---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードでフィルタリング機能を作成しました。pull request (PR) を開く前に、ブラウザーで実際に動作することを確認します。ここでは **Playwright のブラウザー操作を提供する Model Context Protocol (MCP) サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次の作業を行います。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する方法を確認します。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot に Playwright MCP サーバーを使わせ、ブラウザーでフィルタリング機能を手動テストします。
- ブランチを公開し、フィルタリング作業の pull request を作成します。

## Model Context Protocol (MCP) とは

エージェントモードでエディターの外部にある機能を利用すると、さらに多くの作業を行えます。Model Context Protocol (MCP) は、Copilot が外部のツールやサービスと通信するための標準的な方法です。

![エージェントモードがコンテキスト、大規模言語モデル (LLM)、ツール (MCP サーバーと VS Code 拡張機能が提供するツールを含む) と連携する様子を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、人工知能 (AI) エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントはリソースを使って最新情報にアクセスしたり、ツールを使って開発者に代わって操作を実行したりできます。

ツールやリソースには、AI エージェントと外部サービスの間をつなぐ MCP サーバーを通じてアクセスします。MCP サーバーは、既存のアプリケーション プログラミング インターフェイス (API) や npm パッケージなどのローカルツールといった外部ツールとの通信を管理します。各 MCP サーバーは、エージェントが利用できるツールとリソースを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API を提供し、エージェントがリポジトリの作成や更新、Issue や pull request の管理などを行えるようにします。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化機能を提供します。Web ページへの移動、フォームへの入力、ボタンの選択などを行えます。

ほかにも多くの MCP サーバーがあります。GitHub は、MCP サーバーを見つけやすくし、エコシステムへの貢献を促すために[MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクト内の他の依存関係と同様に扱ってください。利用する前にソースコードを確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクトのテンプレートに含まれています。そのため、前の演習で Copilot がバックログの Issue を読み取れました。これは[リモート GitHub MCP サーバー][remote-github-mcp-server]を使います。ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot が最初にツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot がブラウザーを操作できるようになります。

1. `.vscode/mcp.json` で `github` の隣に `playwright` エントリを追加し、ファイルが次のようになるようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル**サーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると、ブラウザーが画面に表示されずに動作します。デスクトップ環境がない Codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンドテストで Playwright を使うため、通常は必要なブラウザーがすでにインストールされています。Copilot がブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行するよう依頼してから、もう一度試してください。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて**起動します。Copilot がサーバーのツールを初めて必要としたときに起動し、サーバーを信頼するか確認します。

1. `.vscode/mcp.json` で `playwright` エントリの上に **Start** 操作が表示されます。これを選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code からサーバーを信頼するよう求められたら、構成を確認して信頼し、起動を許可します。

この手動操作は省略できます。その場合、次の手順で Copilot がサーバーのツールを必要としたときに、VS Code がサーバーを起動し、信頼を確認します。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。エージェントはページを開き、移動し、フィルターを適用し、結果を読み取って報告できます。会話を離れずに機能が期待どおり動作するか確認できます。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティツリー][playwright-mcp-server]を使います。エージェントはボタン、リンク、リスト項目などのラベル付き要素を通じて動作を判断します。支援技術と同じように構造化された情報を使うため、簡単な機能確認がアクセシビリティの基本的な確認にもなります。

Playwright MCP サーバーに接続し、アプリを起動したら、Copilot にフィルタリング機能を試させます。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを使ってブラウザーを起動し、手順に沿って操作した結果を報告します。報告内容を Issue の受け入れ条件と照らし合わせます。問題がある場合は、追加の質問をしたり、PR を作成する前にコードを修正するよう依頼したりします。

> [!NOTE]
> このテストには、`http://localhost:4321` でアプリが実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送信する前にもう一度起動してください。Copilot が Playwright MCP サーバーを初めて使用するとき、ブラウザーをダウンロードする場合があります。ブラウザーが見つからないという報告があれば、`npx playwright install chromium` を実行するよう依頼してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開して pull request を作成する

機能が動作することを確認したので、チームがレビューできるよう pull request (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを選択してコミットメッセージを生成します。

   ![変更されたファイルを示す Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してリポジトリにブランチをプッシュします。

## pull request を作成する

GitHub.com や GitHub コマンドライン インターフェイス (CLI) など、pull request を作成する方法はいくつかあります。ここでは GitHub Copilot に依頼します。Copilot が関連する Issue を見つけ、PR に関連付けます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. エージェントドロップダウンで **Agent** が選択されていることを確認します。これにより、Copilot は GitHub ツールを使用できます。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、情報の収集や操作の実行を許可します。Copilot が GitHub ツールを初めて使用するときに GitHub 認証を求められた場合は、画面の指示に従います。
5. Copilot が Issue を検索し、該当するものを見つけて PR を作成する様子を確認します。
6. Copilot が表示したリンクを選択して PR を確認します。ただし、まだマージしないでください。

## まとめと次のステップ

この演習では、次の作業を行いました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する方法を確認しました。
- Playwright MCP サーバーをワークスペースに追加しました。
- ブラウザーでフィルタリング機能を手動テストするために、Copilot にサーバーを使わせました。
- ブランチを公開し、フィルタリング作業の pull request を作成しました。

機能のテストに MCP サーバーを使いました。MCP は Copilot を拡張する方法の1つにすぎません。次は[カスタムエージェントを作成し][next-lesson]、アクセシビリティレビューなど、対象を絞ったタスクを効率化します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
