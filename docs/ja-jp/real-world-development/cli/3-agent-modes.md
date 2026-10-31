---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で進め方を合意し、Autopilot で Issue に基づくフィルタリングを構築して、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

まず、プロジェクトに小さな機能を追加しました。より大きな変更には、さらに慎重なプロセスが必要です。GitHub Copilot CLI は既存の組織のワークフローに沿って作業し、適切な方法で必要な機能を構築できるようにします。このレッスンを含む一連のレッスンでは、Issue を起点に機能を作成し、コードの妥当性と機能の動作を確認してから、プロジェクトにマージする一般的なエージェント駆動の開発プロセスを進めます。

> [!NOTE]
> 機能開発を続ける間は、同じ会話とブランチを使います。通常はファイルの種類ごとに別のブランチや pull request を作成しますが、ここでは主な概念に集中するため手順を簡略化します。

このレッスンでは、次の作業を行います。

- GitHub Issue から Copilot の新しい会話を開始します。
- Plan モードで要件を決めます。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動で検証します。

この機能の作業を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増えてきたため、訪問者はカテゴリや発行元でゲームを絞り込む必要があります。バックログの Issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に合意しておきたい詳細があります。Plan モードでこれらの決定を行い、範囲を決めた実装を Autopilot に任せます。

## 背景

AI コーディングエージェントを開発に取り入れても、基本は変わりません。むしろ、より重要になります。多くの開発者は、次のような流れで作業します。

1. 作業内容を記載した Issue を作成します。
2. 実装する内容を計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. pull request (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> チームや組織によって詳細は異なりますが、多くの場合はここに示した流れに近い手順になります。

この標準的な方法を守ることで、AI が生成したコードが要件を満たし、手書きのコードと同じレビューと検証を受けるようにできます。

## 会話モード

**会話モード**は、エージェントに与える自律性の度合いを決めます。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押してモードを切り替えます。

- **Interactive**: 開発者とエージェントが協力して作業します。エージェントは変更を提案し、次の作業に進む前に入力を待ちます。
- **Plan**: エージェントが先に計画を作成します。プロジェクトファイルの編集はできません。
- **Autopilot**: エージェントが自律的に作業し、コードを作成してテストを実行し、完了と判断するまで反復します。

まず Plan モードで計画を作成してレビューし、その後 Autopilot モードで実装します。

## Issue から作業を始める

フィルタリングを始める前に、Codespace に戻ってリポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルタリング用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を2回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーを使って **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub 上の Issue や pull request (PR) をコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てると、機能の実装やタスクの実行を Copilot に任せる前に、作業方法を決められます。複雑なタスクでは、事前に計画を立てることをお勧めします。Plan モードに切り替えて、Copilot に計画を作成させます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** になっていることを確認します。
2. 前の手順で追加した Issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調査し、最適な方法を検討して計画を作成します。

3. フィルタリングの動作について質問された場合は、希望に合わせて回答します。どの選択肢を選んでもかまいません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を上から下まで確認します。
6. 自分の決定と合わない箇所があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画が完成して内容を確認したので、実装に進みます。Autopilot を使って、Copilot に作業を任せます。

Autopilot では、Copilot がタスクを完了したと判断するまで作業を反復できます。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みのバージョンに表示されている同様のラベルのオプションを選択します。
2. プロンプトの下にあるモード表示が **Autopilot** になっていることを確認します。
3. Copilot が計画に沿って反復し、コードを作成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。先に計画をレビューしてください。Copilot が依存関係の不足やポートの競合を報告した場合は、チェック完了と判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

他のコードと同じように、生成されたコードもマージする前にレビューする必要があります。コードを確認し、サイトを実行して動作を検証します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルタリングの実装とテストを確認します。
3. 結果を Issue と計画時に決めた内容と照らし合わせます。
4. コードのレビューが終わったら <kbd>Esc</kbd> を押して差分表示を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

コードを確認しました。次はアプリを起動し、Codespaces で転送されたポートからサイトを開いて、実際に動作するか確認します。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら **Open in Browser** を選択します。
3. カテゴリによるフィルタリング、発行元によるフィルタリング、計画時に合意した組み合わせを試します。
4. リセットと結果が空になる場合の動作が、Issue と決定した内容に合っていることを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーの停止を依頼します。

## まとめと次のステップ

会話モードを使い分けて、機能を作成しレビューしました。このレッスンでは、次の作業を行いました。

- GitHub Issue から Copilot の新しい会話を開始しました。
- Plan モードで要件を決めました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動で検証しました。

次は[カスタム指示を使って][next-lesson]、コードが文書化された規約に従って生成されるようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
