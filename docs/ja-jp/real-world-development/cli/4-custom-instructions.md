---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法で作業する必要がある場合は、そのガイダンスを Copilot が利用できるようにします。[指示ファイル][instruction-files]では、必要なコードだけでなく、コードの構成方法も説明できます。フィルタリング機能を作成したので、Copilot が使った指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次の作業を行います。

- リポジトリの指示ファイルとパスに応じた指示ファイルを使って、エージェントに情報を提供する方法を確認します。
- コーディング標準に従うよう指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には開発作業に関するガイドラインと要件があります。たとえば、次のようなものです。

- コメントには、コードの内容を繰り返すのではなく、目的や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポートされた関数には、目的、パラメーター、戻り値を説明する TypeScript 用のドキュメント仕様 (TSDoc) または JavaScript 用のドキュメント仕様 (JSDoc) を記載します。`db` 引数を注入できる場合は、その引数も説明します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連コードが変わったらコメントも更新します。
- 既存の書式設定と lint の指示を維持します。

指示ファイルを使って Copilot に適切な情報を提供し、こうした開発慣行に沿って作業できるようにします。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝えられます。優先するコーディング規約やライブラリ、コードに含めるコメントの種類などを指定でき、より適切な提案やコード生成につながります。リポジトリ全体に適用する指示も、特定のファイルやタスクに適用する指示も作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。ほとんどのリクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスクに関するガイドラインを記載するファイルです。

> [!NOTE]
> その他の指示形式への対応状況は環境によって異なります。形式に依存する前に、[カスタム指示のサポート][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

このスタータープロジェクトには、すぐに確認できる指示ファイルが含まれています。変更前に内容を確認して、指示の影響を比べられるようにします。

1. Codespace に戻ります。
2. Codespaces エディターで、ターミナルではなく `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングガイダンスを確認します。この指示は、リポジトリ内の Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象を決める glob です。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存のプロジェクトファイルへのリンクを確認します。指示を再利用可能な小さなファイルに分け、例を参照できます。

## チームのガイダンスに合わせて指示ファイルを更新する

すでにある指示ファイルは有用ですが、不足している点があります。`copilot-instructions.md` を変更して、新たに生成される TypeScript に TSDoc コメントを追加するよう指示します。

1. `.github/copilot-instructions.md` で、27行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きとして、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを適用する

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。フィルタリング会話のコンテキストを保ったまま、新しい指示を読み込めるよう、会話を一度終了して再開します。

1. フィルタリングの会話がまだ開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリルートの `game-filters-cli` ブランチから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示された場合は、フィルタリングの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. Copilot に、更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI がこのプロジェクトの指示をどう使うかを確認し、新しい標準を機能に適用しました。このレッスンでは、次の作業を行いました。

- リポジトリの指示ファイルとパスに応じた指示ファイルを使って、エージェントに情報を提供する方法を確認しました。
- コーディング標準に従うよう指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して実行できるようにします。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 用のカスタム指示を改善する5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot - 指示ファイルなどのコレクション][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
