---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法で作業する必要がある場合や、Copilot に共有したい背景情報がある場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に「何を」するかだけでなく、「どのように」作業するかも伝えられます。

この演習では、次の作業を行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここではプロンプトの変更前後を比較しません。Copilot cloud agent は GitHub Issue に対して非同期に動作するため、リアルタイムで並べて効果を示すのが難しいためです。このハーネスの後半で cloud agent が作成した pull request をレビューする際に、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には開発作業に関するガイドラインと要件があります。たとえば、次のようなものです。

- データ層には必ずユニットテストを追加します。
- ユーザーインターフェイス (UI) はダークモードで、モダンな印象にします。
- TypeScript 用のドキュメントコメント形式である TSDoc のコメントをコードに追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使って Copilot に適切な情報を提供し、こうした開発慣行に沿って作業できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件を Copilot に伝えられます。優先するコーディング規約やライブラリ、コードに含めるコメントの種類などを指定でき、より適切な提案やコード生成につながります。リポジトリ全体に適用する指示も、特定のファイルやタスクに適用する指示も作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。技術スタック、開発中の内容、ベストプラクティスなど、通常のチャットや CLI リクエストの多くに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に使う指示ファイルです。TypeScript などの言語、ユーザーインターフェイス (UI) コンポーネントの作成、ユニットテストなどに関する指示を記載できます。

> [!NOTE]
> 統合開発環境 (IDE) では、指示ファイルは Copilot Chat でのコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示ファイルと、`applyTo` フロントマターで対象を指定した `*.instructions.md` ファイルを使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準形式の指示ファイルにも対応しています。[指示ファイルのサポート][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

この演習のサンプルプロジェクトでは、指示ファイルの代表的な構成を確認できます。基本的な考え方は次のとおりです。

- `copilot-instructions.md` には、プロジェクトの説明、構成、全体的なコーディング標準など、プロジェクト全体に関係する情報を記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データ層など、特定のファイルの種類やタスクに関する指示を記載します。
- 自然な言葉で、明確なガイダンスとコードの良い例・悪い例を示します。

指示ファイルの作成方法は1つではありません。AI のほかの使い方と同様に、実際に試しながら、プロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意してください。このプロジェクトには、[UI 更新用][ui-instructions]や [Astro 用][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルを生成させることもできます。操作方法は環境によって異なります (たとえば、VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など)。必要な場合は、利用中の環境向けのレッスンで説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタムエージェントなどを集めたリポジトリ [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリには、コアとなる `copilot-instructions.md` と、タスクごとの `*.instructions.md` ファイルが含まれています。エディターまたは GitHub の Web UI で開いて内容を確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要に加え、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** にある **GitHub Actions Workflows** の指示も、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象を決めるリポジトリルート相対の glob です。ここでは、たとえば `**/*.test.ts` に一致する TypeScript テストファイルが対象です。
5. ユニットテストを作成するときに適用される指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾まで確認します。`unit-tests.instructions.md` などの指示ファイルや既存ファイルへのリンクがあります。大きな指示を再利用可能な小さなファイルに分け、例を参照できます。ここにあるパスはリポジトリルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で TSDoc コメントとファイルヘッダーの規則を追加します。

## リポジトリ全体に適用する標準を追加する

ここでは `.github/copilot-instructions.md` に、コード内の TSDoc コメントとファイルヘッダーを追加する規則を記載します。後の演習で cloud agent が Issue に対応するとき、この指示が使われます。

編集する前に、作業用のブランチを作成します。cloud agent は Issue の対象ブランチから指示を読み取りますが、変更をコミットするためにもブランチを用意します。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` を編集し、プロジェクトのコーディング標準を Copilot が適用できるようにします。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27行目付近にある **Code formatting requirements** セクションを探します。現在はコーディング標準が記載されていますが、コード内のドキュメントに関する規則はありません。
3. 既存の標準の直下に、次の Markdown 行を追加して、ファイルヘッダーと TSDoc コメントを指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで説明したように、リポジトリレベルの `.github/copilot-instructions.md` は全体に適用する指示を置く場所であり、`*.instructions.md` は言語、ファイルの種類、タスクごとの指示を置く場所です。ドキュメントコメントの規則のような、プロジェクト全体の標準にはリポジトリレベルのファイルが適しています。

## 指示の変更をコミット、プッシュ、マージする

cloud agent は、Issue の対象となるブランチの指示ファイルを読み取ります。次の演習で Issue を Copilot に割り当てると、`main` からブランチが作成されるため、この変更を `main` に反映しておく必要があります。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` への pull request を作成し、マージします。次の演習で Issue に割り当てると、cloud agent がこの指示を読み取れるようになります。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合はブランチの作成を省略し、`main` に直接コミットできます。ブランチを使う手順は、実際のプロジェクトでの作業方法に近づけるためのものです。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルのコンテキストをどう使うかを確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。この演習では、次の作業を行いました。

- リポジトリのカスタム指示とパスに応じた指示ファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。

以降のハーネスで cloud agent が作成する pull request をレビューするときに、この標準が適用されているかを確認します。

次は、[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 用のカスタム指示を改善する5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot - 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
