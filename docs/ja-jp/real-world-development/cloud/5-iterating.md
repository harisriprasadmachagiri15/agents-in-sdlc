---
title: "演習 5: GitHub Copilot の作業を改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業内容をレビューする

このラボでは、GitHub Copilot にドキュメントの追加、デザインチームが改善を検討する関連ゲーム機能の作成、ハイコントラストモードとライトモードを含むアクセシビリティ機能の実装を依頼しました。コードの変更内容を確認し、必要に応じて Copilot にフィードバックを送って改善しましょう。

### シナリオ

生成 AI を利用しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。ここでは、チーム全体にレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した変更を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期に、監督者なしでタスクを実行するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みアクセスはコード用ブランチに限られます。
- Cloud agent は GitHub Actions 内で動作し、作業用の一時的な環境を別に作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- 既定では、[外部リソースへのアクセス][agent-firewall]が制限されています。Model Context Protocol (MCP) サーバーも対象です。

## 生成されたドキュメントをレビューする

最初に、GitHub Copilot cloud agent が作成したコードへのドキュメント追加に関する pull request (PR) を、GitHub.com の標準的な PR インターフェイスで確認します。

> [!NOTE]
> PR を確認するときに、GitHub Copilot がファイアウォールによってブロックされているという警告が表示されることがあります。Copilot は、外部 MCP サーバーへの呼び出しなど、既定では外部リソースへのアクセスが制限されているため、これは**想定内**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、pull request の一覧を開きます。
3. **Add missing documentation** など、同様のタイトルが付いた pull request を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、pull request のタイトルに **[WIP]** が含まれます。その場合は、Copilot が作業を完了するまで待ちます。数分かかることがあります。その間に休憩したり、これまでに学んだことを振り返ったりしてください。

4. pull request の準備ができたら **Files changed** タブを選択し、変更内容を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc (TypeScript 用のドキュメントコメント形式) のコメントやその他のドキュメントなど、更新されたコードを確認します。具体的な変更内容は異なります。コードを確認しながら、演習 2 の最初に確認したカスタム指示ファイルによる TSDoc コメント、TypeScript の規約、コメントヘッダーが反映されているかも確認します。
6. 更新内容をレビューし、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. ワークフローの承認待ちを示す通知があるか確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. pull request の checks セクションにワークフローが追加されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

Copilot との pull request での作業は、一方通行ではありません。チームメンバーと同じように、pull request やコードへのインラインコメントで **@copilot** をメンションできます。Copilot はコメントを確認し、変更に対応する新しいセッションを開始します。結果は確率的に変わるため、決まった文面を指定することはできません。たとえば、次のような変更を依頼できます。

- コードファイルの先頭に、ファイルの役割を簡潔に説明するコメントヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 生成されたドキュメントへの変更を依頼するコメントを追加し、他のユーザーと同じように **@copilot** をメンションします。上記の例、またはコードベースに追加したいドキュメントについて、別の案を使ってもかまいません。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot は更新用の新しいセッションを開始します。
3. **Back to pull request** を選択して、pull request に戻ります。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、pull request に新しいコミットが表示されます。
5. **Files changed** タブを選択し、変更内容をレビューします。

問題がなければ、続けて Copilot に改善を依頼できます。ドラフト PR をレビュー可能な状態にし、`main` ブランチにマージします。

![PR をレビュー可能な状態にする操作](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

次に、ゲーム詳細ページへの関連ゲーム表示に関する Issue について、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルが付いた PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新内容に問題がなければ **Conversation** タブに戻って下にスクロールします。
6. ワークフローの承認待ちを示す通知があるか確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. pull request の checks セクションにワークフローが追加されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
9. **省略可能:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同様に、Copilot cloud agent に新しいコメントを追加して変更を依頼できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーや TSDoc のドキュメントコメントを追加したりできます (この Issue はカスタム指示の更新前に割り当てたため、指示が反映されていない場合があります)。**Conversation** タブにコメントを追加すると、Copilot がその内容を確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティカスタムエージェントを使って実装した機能をレビューします。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 でミッションコントロールから追加を依頼したライトモードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、pull request のタイトルに **[WIP]** が含まれます。作業が完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装をレビューします。特に、次の項目を確認してください。
   - モードを切り替えるユーザーインターフェイス (UI) コンポーネント。
   - ユーザー設定を保持する local storage の使い方。
   - ハイコントラストモードとライトモードのカスケーディング スタイルシート (CSS) またはスタイルの変更。
   - アクセシビリティ属性 (Accessible Rich Internet Applications (ARIA) ラベル、キーボード操作など)。
   - モード切り替えを管理する JavaScript または TypeScript コード。

6. 更新内容に問題がなければ **Conversation** タブに戻って下にスクロールします。
7. ワークフローの承認待ちを示す通知があるか確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. pull request の checks セクションにワークフローが追加されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
10. **省略可能:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次に、アプリケーションを起動し、ブラウザーでハイコントラストモードとライトモードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティカスタムエージェントが、アクセシビリティのベストプラクティスに沿った実装を支援したことを確認してください。アクセシビリティに関する問題や改善点があれば、前の PR と同様にコメントで **@copilot** をメンションし、変更を依頼できます。

## 省略可能な演習 — さらにタスクを委任する

cloud agent を活用すると、バックログの実際のタスクを任せ、その間に別の作業を進められます。いくつか Issue を作成して Copilot に割り当て、この方法を試してみてください。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。このレッスンでは、次の作業を行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認し、後で cloud agent の出力に反映される様子を把握しました。**
- **Issue を Copilot cloud agent に割り当て、環境の準備、計画、非同期の実行を確認しました。**
- **アクセシビリティ用のカスタムエージェントを作成して使用し、ハイコントラストモードとライトモードの切り替えを追加しました。**
- **Copilot agents ページをミッションコントロールとして使い、アクセシビリティセッションを監視して作業中に指示を追加しました。**
- **cloud agent の pull request をレビューし、@copilot をメンションして変更を依頼し、ワークフローを承認しました。**

## レビューと次のステップ

Cloud agent ハーネスを完了しました。他のハーネスでは、ここで練習した内容を別の環境で試せます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — 統合開発環境 (IDE) で Copilot Chat のエージェントモードと MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan、Autopilot、エージェントスキル、カスタム品質保証 (QA) エージェント、Playwright MCP、Agent Merge、および省略可能な `/delegate` による cloud agent への引き継ぎを使って、レビュー済みの2つの変更をターミナルから提供します。

自分のリポジトリで、次のアイデアを試してください。

- リファクタリングやテストカバレッジの Issue を cloud agent に割り当てます。
- 別の分野向けのカスタムエージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
