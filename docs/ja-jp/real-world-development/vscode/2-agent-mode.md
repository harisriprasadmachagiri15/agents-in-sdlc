---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの単純な変更でも、通常は複数のファイルを更新し、テストを実行する必要があります。開発者は、必要なファイルを探し、変更を加え、テストを実行し、デバッグし、更新し忘れたファイルを見つけて、さらに変更を加えます。こうした作業が続きます。

Copilot エージェントモードを使うと、統合開発環境 (IDE) でより自律的に作業できます。開発者と同じようにプロジェクトの構造を調査し、必要な変更を行い、テストなどのタスクを実行し、問題が見つかった場合は自動的に修正します。エージェントモードを使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる用途を持ちます。エージェントモードは IDE 内で動作するため、すばやくフィードバックを得て対話できます。Cloud agent はチームメンバーのように非同期で作業し、Issue と pull request を介してやり取りします。

この演習では、次の作業を行います。

- Copilot エージェントモードがプロジェクトを調査し、関連ファイルを特定して、複数のファイルにわたる変更を行う方法を確認します。
- GitHub Copilot エージェントモードを使って、ユーザーインターフェイス (UI) とデータ層にまたがる新機能を実装します。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューします。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリと発行元で絞り込めるようにします。前の演習で発行元ヘルパーを追加しました。次は、Copilot エージェントモードを使って、データ層、UI、テストの残りの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を把握します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示します。単一の Astro アプリで、ビルド時に静的ハイパーテキスト マークアップ言語 (HTML) としてページをレンダリングします。ページは、フロントマターから Drizzle オブジェクト リレーショナル マッピング (ORM) を使ってローカルの SQLite データベースを直接クエリします。独立したバックエンド アプリケーション プログラミング インターフェイス (API) やクライアント側ユーザーインターフェイス (UI) フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、インタラクティブな機能は標準のドキュメント オブジェクト モデル (DOM) API を使った小規模な Astro `<script>` で追加します。

### Web サイトを起動する

`npm run dev` タスクを使うと、Astro の開発サーバーを起動できます。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習を終えていれば、`main` に戻っているはずです。
2. Codespace で新しいターミナルウィンドウを開きます。<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. フィルタリング作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro の開発サーバーが起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くように求めるダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら Astro サーバーのアドレス `http://localhost:4321` を選択し、Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。これは、Web サーバーをホストする Codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトを起動したら、現在の機能を確認します。主な機能は次のとおりです。

- **ホームページ**: タイトル、画像、説明とともにボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習によって異なるため、必要な場合は各手順で説明します。

Web サイトの初期実装は動作しますが、さらに機能を追加します。まずバックログを確認します。テンプレートからプロジェクトを作成すると、Issue のバックログも自動的に作成されています。GitHub Copilot に一覧を表示するよう依頼します。

1. Chat ビューのエージェントドロップダウンで **Agent** を選択します。**Agent** エージェントは、自律的に計画を立て、複数ファイルを変更し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェントピッカーを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 使用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者は、特定のモデルを推奨するものではありません。ワークショップの作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案の一貫性を高めることが目的ですが、大規模言語モデル (LLM) の動作は確率的なため、ここに示す内容と異なる提案が生成される場合があります。これは想定内です。

> [!NOTE]
> 確率的な動作のため、Copilot は別の MCP コマンドを使う場合がありますが、タスクを完了できるはずです。

3. 次のプロンプトを送信し、バックログにある Issue を Copilot に確認させます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot がユーザーに最も役立つと考える項目を優先順位付けしたことを確認します。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、作業のコンテキストを Copilot に伝える指示ファイルを確認します。ここでは、Web サイトの機能追加に関するガイダンスを含む [ユーザーインターフェイス (UI)][ui-instructions] ファイルを使います。

1. Codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加時の全体的なガイダンスを確認します。アーキテクチャの概要、コンポーネントの設計、テスト容易性とアクセシビリティの原則、Astro、Tailwind CSS (カスケーディング スタイルシート)、Drizzle などの各ファイルに関する指示へのリンクが含まれています。

> [!TIP]
> 指示ファイルから、他の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用し、大きな指示を小さく管理しやすい単位に分け、例やテンプレートを示すことができます。

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくとも次の3つを更新します。

- データ層 (`src/lib/`) に残りのフィルタリングロジックを追加または調整します。
- フィルタリングの動作を確認するテストを追加または更新します。
- ゲーム一覧ページにフィルタリング UI を追加します。

さらに、コードベースにマージする前にテストを実行して合格する必要があります。Copilot エージェントモードに作業を依頼します。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択し、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェントピッカーを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルとして **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログにある関連 Issue の要件に基づいて機能を実装するよう、Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調査し、関連するファイルを探して、データ層、UI、テストを変更する様子を確認します。

   ![Copilot がプロジェクトのファイルを調査している様子](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトを調査し、ファイルを変更して、テストを実行します。タスクの複雑さやコードベースによって数分かかる場合があります。作業中、コードエディターに **Keep** と **Undo** のボタンが表示されることがあります。Copilot が作業を終えると、すべての変更に対して **Keep** または **Undo** を選択できるようになるため、作業中に選択する必要はありません。

7. Copilot に求められたら **Continue** を選択してテストを実行します。

   ![Copilot Chat ペインでテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストに失敗したりする場合があります。Copilot はコード生成とテストを反復し、エラーがないと判断するまで作業を続けるため、これは問題ありません。

   ![Copilot エージェントモードの完了したチャットセッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを起動したブラウザーに戻り、新しい機能を試します。
11. 動作を確認してコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot エージェントモードで Tailspin Toys Web サイトに機能を追加する方法を確認しました。

- Copilot エージェントモードを使って、UI とデータ層にまたがる新機能を実装しました。
- エージェントモードがプロジェクトを調査し、関連ファイルを特定して、複数のファイルにわたる変更を行う方法を確認しました。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューしました。

次は[Playwright MCP サーバーで機能をテストし][next-lesson]、pull request を作成します。

### ボーナスの演習 - ページネーションを実装する

ゲームの一覧が増えた場合は、ページネーションが必要になるでしょう。この演習で学んだことを使い、Copilot にサイトのページネーションを実装させます。次の点を検討してください。

- 既存の指示ファイルを使うなど、現在のベストプラクティスに従います。
- ページサイズをユーザーが選べるようにするか、値を固定するかなど、ページネーションの動作を決めます。
- 希望する実装に必要なガイダンスをプロンプトに含めます。
- Copilot に変更を依頼してコンテキストを追加するなど、何度かやり取りが必要になる場合があります。これは Copilot を使う通常のワークフローです。

## リソース

- [Copilot の Ask、Edit、Agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
