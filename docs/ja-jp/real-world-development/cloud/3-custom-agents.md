---
title: "演習 3 - カスタムエージェント"
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

- カスタムエージェントの定義方法を確認します。
- タスクをカスタムエージェントに割り当てます。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントがあらかじめ作成されています。内容を確認し、Copilot の作業にどう反映されるかを把握します。

Codespace に戻り、アクセシビリティエージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを確認します。アクセシブルな Web サイトのコード生成に関する主な責任、アクセシビリティのベストプラクティス、HTML、CSS、JavaScript のコード例、よくある問題と間違いが記載されています。

## Issue を作成して割り当てる

Mission control は、環境内のエージェントを操作するための一元的な場所です。Copilot cloud agent にタスクを割り当て、進行状況を監視し、作業の方向を変更したり、追加のガイダンスを送ったりできます。まず、ハイコントラストモードの作成を Copilot に割り当てます。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

    ![カスタムエージェントの選択と Accessibility agent が強調表示された cloud agent 割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を始めます。

## まとめと次のステップ

[カスタムエージェント][custom-agents]は、特定のタスクや分野に合わせて作成する専門的なアシスタントです。チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot の作業を導く方法を確認しました。

このレッスンでは、次の作業を行いました。

- カスタムエージェントの定義方法を確認しました。
- タスクをカスタムエージェントに割り当てました。

Copilot がハイコントラストモードを実装している間に、次は[エージェントセッションを監視して方向を調整します][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [Organization でカスタムエージェントを利用する準備][org-custom-agents]
- [Enterprise でカスタムエージェントを利用する準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
