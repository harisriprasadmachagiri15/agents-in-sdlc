---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法で作業する必要がある場合や、Copilot に共有したい背景情報がある場合は、そのコンテキストを参照できるようにします。コード生成の指針を伝える[指示ファイル][instruction-files]は、そのための重要な方法の1つです。

この演習では、次の作業を行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- 既存の指示を適用した状態で、フィルタリング用の最初のデータスライス (発行元ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトをもう一度実行し、生成されたコードが新しい標準に従う様子を確認します。
- 次の演習で Copilot が更新済みの指示を使えるよう、指示の変更とフィルタリング用の変更を `main` にコミットします。

> [!CAUTION]
> 生成されたコードが、設定した標準に従わない場合があります。Copilot の動作は確率的です。ここでは、指示を更新した後に動作がどのように変わるかを確認します。生成結果が完全に一致することを目指すものではありません。

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

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。技術スタック、開発中の内容、ベストプラクティスなど、プロジェクト全体に関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に使う指示ファイルです。TypeScript などの言語、UI コンポーネントの作成、ユニットテストなどに関する指示を記載できます。

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

指示ファイルの作成方法は1つではありません。人工知能 (AI) のほかの使い方と同様に、実際に試しながら、プロジェクトに合う方法を見つけてください。

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
6. `.github/instructions/drizzle.instructions.md` を開き、末尾まで確認します。`unit-tests.instructions.md` などの指示ファイルや既存ファイルへのリンクがあります。大きな指示を再利用可能な小さなファイルに分け、例を参照できます。ここにあるパスは指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で TSDoc コメントとファイルヘッダーの規則を追加します。

## 変更を加えるブランチを作成する

ここでは、作業用のブランチを作成します。

1. 前の演習で使用した Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習によって異なるため、必要な場合は各手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルの状態でプロンプトを送信します。後で指示を更新した後、同じプロンプトをもう一度実行して違いを確認します。

1. 前の演習で使ったエディタータブを閉じ、Copilot が必要なコンテキストだけを使うようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを配置する場所を Copilot に示します。
3. Chat ビューのエージェントドロップダウンで **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェントピッカーを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調査してコードを更新します。多くの場合、ヘルパーファイルとテストが変更されます。
6. 提案されたヘルパーが、型付けされた関数として `db` クライアントを最初の引数に受け取り、型付きの発行元配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層の規約によるものです。
7. 生成されたコードに TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot の動作は確率的なため、指示がなくてもドキュメントコメントを追加する場合があります。その場合も問題ありません。ここでは、指示を追加した後に一貫性がどう変わるかを確認します。

## リポジトリ全体に適用する標準を追加する

`.github/copilot-instructions.md` を使って、プロジェクトのコーディング標準を Copilot に伝えます。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27行目付近にある **Code formatting requirements** セクションを探します。現在はコーディング標準が記載されていますが、ドキュメントコメントに関する規則はありません。そのため、前に生成したヘルパーにはドキュメントコメントが含まれていません。
3. 既存の標準の直下に、次の Markdown 行を追加して、ファイルヘッダーと TSDoc コメントを指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで説明したように、リポジトリレベルの `.github/copilot-instructions.md` は全体に適用する指示を置く場所であり、`*.instructions.md` は言語、ファイルの種類、タスクごとの指示を置く場所です。ドキュメントコメントの規則のような、プロジェクト全体の標準にはリポジトリレベルのファイルが適しています。

## プロンプトをもう一度実行して違いを確認する

1. Copilot Chat に戻り、**New Chat** を選択して入力内容をクリアします。
2. `src/lib/publishers.ts` をもう一度選択し、Copilot に対象ファイルを示します。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Tailspin Toys クラウドファンディングプラットフォームの発行元データアクセスヘルパー。
    * データベースから発行元の情報を取得する関数を提供します。
    */
   ```

5. 関数に、次のような TSDoc コメントが追加されていることを確認します。

   ```typescript
   /**
    * ID と名前を含む発行元の一覧を返します。
    *
    * @param db - Drizzle データベースクライアント。
    * @returns 発行元オブジェクトの配列に解決される Promise。
    */
   ```

Copilot を新しいプロジェクト標準に従わせ、次の演習で使うコードに適用しました。

## 指示とブランチをコミットしてプッシュする

指示ファイルも、他のファイルと同じようにソース管理します。指示の更新とヘルパーの変更をコミットし、ブランチをリポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルウィンドウを開きます。
2. ターミナルで次のコマンドを実行し、指示とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. ターミナルで指示の更新と、ヘルパー用に Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルからブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## pull request を作成してマージする

ブランチをプッシュしたら、指示標準に関する Issue を pull request に関連付けます。GitHub ツールを使用中のプロジェクトに接続済みの Copilot に、Issue を見つけて PR を作成し、マージするよう依頼します。

> [!NOTE]
> Copilot に GitHub 上の操作を依頼するのは、今回が初めてです。プロジェクトのテンプレートでは GitHub ツールがワークスペースにあらかじめ接続されています。Copilot が初めてツールを使うとき、VS Code で GitHub へのサインインを求められる場合があります。表示される手順に従ってください。この接続の仕組み (Model Context Protocol) については、後の演習で説明します。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェントピッカーで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する Issue を見つけ、現在のブランチから PR を作成するよう Copilot に依頼します。指示の変更と、今後のフィルタリング作業の基礎となる発行元ヘルパーの両方を PR に記載するよう伝えます。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が情報を収集して操作を実行するための確認を求めた場合は、**Allow** を選択します。
5. PR が作成されたら、Copilot に PR のマージと、ローカルブランチの `main` への切り替えを依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Copilot が GitHub 操作とシェルコマンドの実行許可を求めた場合は、内容を確認して適切に **Allow** を選択します。

GitHub Copilot の支援で pull request を作成し、マージしました。

## まとめと次のステップ

このプロジェクトで Copilot が指示を使う方法を確認し、VS Code の Copilot Chat を使って次の作業を行いました。

- リポジトリの指示ファイルとパスに応じた指示ファイルが Copilot にコンテキストを伝える方法を確認しました。
- 既存の指示を適用した状態でコード生成プロンプトを送り、フィルタリング用の最初のデータスライスとして発行元ヘルパーを生成しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトをもう一度実行し、生成されたコードに新しい標準が反映される様子を確認しました。
- 次の演習で使えるよう、指示の更新と発行元ヘルパーをコミットして `main` に反映しました。

次は[エージェントモードで指示を適用し][next-lesson]、Copilot にコードベース全体にわたる新機能を追加させます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 用のカスタム指示を改善する5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot - 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
