---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの数回の演習で、GitHub Copilot cloud agent にユーザー体験の向上や機能追加に関する3つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクは、GitHub.com の[agents ページ][agents-page]など、さまざまなツールで管理できます。この一元管理画面では、開いている pull request のあるすべてのエージェントタスクを確認できます。作業内容を調べたり、進行中のセッションに追加の指示を送ったりできます。

このレッスンでは、次の作業を行います。

- agents ページで cloud agent のタスクを監視します。
- 進行中のセッションに追加の機能を依頼します。

## シナリオ

ハイコントラストモードの作成をエージェントに依頼した後、チームはライトモードも追加したいと考えました。サイトのスタイルと切り替え機能を更新しているため、同じ作業に含めるのが合理的です。エージェントにライトモードも追加してもらい、ハイコントラストモードと合わせて実装します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインの両方にあるタスク一覧を確認します。割り当てたタスクとして、次のものが表示されます。
    - コードベースのドキュメントの更新。
    - ゲーム変更用 API の生成。
    - Web サイトへのハイコントラストモードの追加。
3. 実行中のタスクを1つ選択し、Copilot が実行した作業を確認します。コードのチェックアウト、作業環境の作成、Model Context Protocol (MCP) サーバーのセットアップ、タスクを完了するための各種操作などが含まれます。

> [!NOTE]
> 表示される手順は、Copilot の作業状態や選択した方法によって異なります。

4. 右側に表示される pull request (PR) ペインも確認します。ここから PR と変更ファイルを確認し、進行状況を監視できます。

## Cloud agent に追加の指示を送る

実行中のタスクを確認したので、ハイコントラストモードに取り組んでいる Copilot にライトモードも追加するよう依頼します。

1. ハイコントラストモードの追加に関するセッションを選択します。タイトルは、Copilot が付けた名前や作業状態によって異なります。

    ![ミッションコントロールのアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot の作業中にセッションへ指示を送るダイアログを示す画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを確認し、作業に追加したことを確認します。

## Copilot に作業を続けてもらう

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行中だった手順を終えた後、追加の依頼をワークフローに組み込みます。

ここまでと同様に、完了まで数分かかる場合があります。その間に、これまでに学んだことを振り返ってください。

## まとめと次のステップ

Copilot agents ページを使って、GitHub Copilot cloud agent のタスクを監視し、進行中のセッションの内容を変更する方法を確認しました。

次の内容を確認しました。

- agents ページを使って cloud agent のタスクを監視しました。
- 進行中のセッションに追加の機能を依頼しました。

Copilot がアクセシビリティ機能の作業を進めている間に、次は[Copilot が作成した pull request を改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
