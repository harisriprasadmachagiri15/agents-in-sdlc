---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローにおける特定のタスクや分野に合わせて、専門的な人工知能 (AI) アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、対象の作業に合わせた指示、ベストプラクティス、コーディングパターン、専門知識を指定します。チームの知識を再利用可能なエージェントにできます。たとえば、[Web Content Accessibility Guidelines (WCAG)][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディングに従うセキュリティエージェント、一貫したテストパターンを守るテストエージェントを作成できます。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を記載する Markdown プロンプトが必要です。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。違いを簡潔に表すと、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントには専用のコンテキストウィンドウがあり、作業の一環としてスキルや他のエージェントを調整できます。このラボのアクセシビリティカスタムエージェントは、アクセシビリティガイドラインに沿ってサイトをレビューし、更新します。その作業の一環として、pull request ワークフローやテスト実行・管理のスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成方法に、唯一の正解はありません。AI のほかの使い方と同様に、環境やシナリオに合わせてテストし、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、誰もがクラウドファンディングプラットフォームを利用できるようにしたいと考えています。最近のユーザーフィードバックでは、現在のダークテーマについて、テキストと背景色のコントラストが不十分で読みづらいという意見がありました。このアクセシビリティの課題に対応するため、デザインチームはユーザーが切り替えられるハイコントラストモードを求めています。

アクセシビリティは重要なため、できるだけ早く実装する必要があります。カスタムエージェントを使って機能を生成します。

この演習では、次の作業を行います。

- アクセシビリティカスタムエージェントを確認します。
- Copilot Chat でアクセシビリティエージェントを使い、ハイコントラストモードを実装します。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントがあらかじめ作成されています。内容を確認し、Copilot の作業にどう反映されるかを把握します。

Codespace に戻り、ターミナルを開いて `main` からアクセシビリティ作業用の新しいブランチを作成します。演習 3 のフィルタリング用 pull request (PR) とは分けて作業します。

1. ターミナルで次のコマンドを実行し、アクセシビリティ用のブランチを作成します。

      ```bash
      git checkout main
      git pull
      git checkout -b accessibility-vscode
      ```

2. `.github/agents/accessibility.md` を開きます。
3. `name` と `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必要です。

4. 続くセクションを確認します。アクセシブルな Web サイトのコード生成に関する主な責任、アクセシビリティのベストプラクティス、ハイパーテキスト マークアップ言語 (HTML)、カスケーディング スタイルシート (CSS)、JavaScript のコード例、よくある問題と間違いが記載されています。

## Copilot Chat でカスタムエージェントを使う

VS Code では、`.github/agents` に定義したカスタムエージェントが Copilot Chat ビュー下部のエージェントドロップダウンに表示されます。カスタムエージェントを選択すると、その指示とツールに合わせてチャットセッションを設定できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習によって異なるため、必要な場合は各手順で説明します。

1. チャットビューのエージェントドロップダウンで、**Agent** を選択します。すでに選択されている場合はそのまま進みます。

   ![Chat ビューのエージェントピッカーを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. チャットビュー下部のエージェントドロップダウンを選択します。既定では、アクティブなエージェントとして **default** が表示されます。
3. 一覧から **Accessibility agent** を選択します。
4. アクセシビリティエージェントに、次のプロンプトを送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat はコードベースを調査し、変更を提案してプロジェクトファイルに適用します。各編集は、ファイルパスと差分を含めてチャット内に表示されます。
6. 編集が適用されると、**Files changed** の表示が更新され、エージェントが変更した作業ファイルを確認できます。

> [!NOTE]
> この処理には数分かかる場合があります。Copilot は実際にリポジトリを変更しながら、Astro コンポーネント、CSS、関連テストなどの既存ファイルを編集します。

次の演習では、この作業を確認し、必要に応じて方向を調整します。

## まとめと次のステップ

[カスタムエージェント][custom-agents]は、特定のタスクや分野に合わせて作成する専門的なアシスタントです。チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot の作業を導く方法を確認しました。

このレッスンでは、次の作業を行いました。

- アクセシビリティカスタムエージェントを確認しました。
- Copilot Chat でアクセシビリティエージェントを使い、ハイコントラストモードを実装しました。

次は[エージェントの作業を監視して方向を調整し][next-lesson]、変更を確認しながら同じセッションにライトモードの切り替えを追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
