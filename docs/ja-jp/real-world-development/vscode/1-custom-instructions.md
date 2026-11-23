---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を利用するうえで、コンテキストは重要です。特定の方法でタスクを実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストにアクセスできるようにします。Copilot に具体的なコンテキストを伝える方法はいくつかあります。その中心となるのが[指示ファイル][instruction-files]です。指示ファイルを使うと、コード生成の指針を伝えられます。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- 既存の指示を適用した状態で、フィルター機能の最初のデータ処理（publisher ヘルパー）を生成します。
- `.github/copilot-instructions.md` に新しいリポジトリ全体の標準を追加します。
- 同じプロンプトをもう一度実行し、生成されたコードが新しい標準に従うことを確認します。
- 指示の更新とフィルター機能の一部を `main` にコミットし、Copilot が次の演習で新しい指針を使えるようにします。

> [!CAUTION]
> 生成されるコードは、設定した標準から外れる場合があります。Copilot の出力は一定ではありません。この演習の目的は、更新後の指示によって動作にどのような傾向の変化が生じるか確認することであり、出力を一字一句一致させることではありません。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プロセスに関するガイドラインと要件があります。具体的には次のとおりです。

- データレイヤーには、必ずユニットテストが必要です。
- UI はダークモードで、モダンな印象にする必要があります。
- TSDoc のドキュメントコメントをコードに追加する必要があります。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加する必要があります。

指示ファイルを使って、Copilot がこれらの開発慣行に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示では、Copilot がコーディングスタイルや要件を理解できるよう、コンテキストや好みを指定します。コーディング規約、ライブラリ、コードに含めたいコメントの種類などを指定して、Copilot の提案やコードスニペットをより適切なものにできます。リポジトリ全体または特定の種類のファイルに対する指示を作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。技術スタック、開発対象の概要、ベストプラクティスなど、Copilot Chat や CLI に送るほとんどのリクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に作成できる指示ファイルです。TypeScript や Astro などの言語や、UI コンポーネントやユニットテストの作成などのタスクに対するガイドラインを記載できます。

> [!NOTE]
> IDE で作業する場合、指示ファイルがコード生成に使われるのは Copilot Chat のみです。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示ファイルと `applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方を使ってコードを生成します。
>
> また、Copilot は AGENTS.md や CLAUDE.md など、他の規格を使った指示ファイルにも対応しています。[指示ファイルの対応状況][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成について詳しく説明することは、この演習の範囲外です。ただし、サンプルプロジェクトには代表的な方法が含まれています。基本的には、次の点を意識してください。

- `copilot-instructions.md` には、開発対象の説明、プロジェクトの構造、全体的なコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル種類や、特定のタスクに関する指示を記載します。
- 自然な言葉を使い、明確なガイダンスを記載します。コードの良い例と悪い例を示します。

指示ファイルの作成方法は1つに限らず、AI の使い方にも決まった形はありません。プロジェクトで試行錯誤しながら、自分に合った方法を見つけましょう。

> [!TIP]
> GitHub Copilot を使用するすべてのプロジェクトで、充実した指示ファイルを用意することをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルを生成してもらうこともできます。利用するインターフェイスによって方法は異なります。たとえば、VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使います。各インターフェイスのレッスンで、該当する操作を説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェントなどが集められたリポジトリ [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認します。基本となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明や **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** の指針があります。これらの指示は、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドには、指示の適用対象を決めるリポジトリルートからの glob が設定されています。ここでは、`**/*.test.ts` に一致する TypeScript テストファイルが対象です。
5. このプロジェクトでユニットテストを作成する際に適用される指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの他の指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな指示を再利用可能な小さなファイルに分け、Copilot がコードを生成するときに参照できる例を示せます。ここに記載されたパスはリポジトリルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションではプロジェクトのコーディング標準を説明していますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc のドキュメントコメントとファイルヘッダーに関するルールを追加します。

## 変更用のブランチを作成する

ここではベストプラクティスに従って、変更用のブランチを作成します。

1. 前の演習で使用した Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

カスタム指示の効果を確認するため、まずは現在の指示ファイルを使ってプロンプトを送信します。その後、指示を更新して同じプロンプトをもう一度送り、結果を比較します。

1. Copilot が必要なコンテキストだけを使えるよう、前の演習で開いたエディタータブをすべて閉じます。
2. `src/lib/publishers.ts` を開き、ヘルパーを置く場所を Copilot に示します。
3. Chat ビューのエージェントのドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューでエージェントの選択欄を示す画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを確認してコードを更新します。多くの場合、ヘルパーファイルとテストの両方が変更されます。
6. 生成されたヘルパーが、最初の引数として `db` クライアントを受け取り、publisher の型付き配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約によるものです。
7. 生成されたコードに TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot の出力は確率的に変化するため、指示がなくてもドキュメントコメントを追加する場合があります。その場合も問題ありません。指示を更新した後の一貫性の向上を確認することが目的です。

## 新しいリポジトリ標準を追加する

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を伝えるためのファイルです。コード提案を改善するために、リポジトリのコーディング標準へルールを追加しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. **Code formatting requirements** セクションを探します。27行目付近にあります。プロジェクトのコーディング標準を説明していますが、インラインのドキュメントに関するルールはありません。そのため、生成されたヘルパーにドキュメントコメントが含まれませんでした。
3. 次の Markdown を既存の標準の直下に追加し、ファイルヘッダーと TSDoc のドキュメントコメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリレベルの `.github/copilot-instructions.md` には全体に適用する指示を、`*.instructions.md` には特定の言語、ファイル種類、タスク向けの指示を記載できます。ここで追加するドキュメントコメントのルールのように、プロジェクト全体の標準にはリポジトリレベルのファイルを使います。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話をクリアします。
2. `src/lib/publishers.ts` を選択し、Copilot に適切なファイルを認識させます。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 生成されたファイルの先頭に、次のようなコメントブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 生成された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準を指示して、次の演習で使用する実際のコードに適用できました。

## 指示とブランチをコミット、プッシュする

指示ファイルも他のアセットと同じように、ソース管理で管理します。更新した指示ファイルとブランチをリポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルウィンドウを開きます。
2. ターミナルで次のコマンドを実行し、指示の更新とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. 指示の更新と、ヘルパーの基礎として Copilot が変更したすべてのファイルをステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、プルリクエストを作成し、指示標準に関する Issue と関連付けます。手動で行う代わりに、GitHub ツールが接続されている Copilot に依頼できます。Issue を検索し、Issue をクローズする PR を作成するようプロンプトを送ります。

> [!NOTE]
> この演習では初めて、Copilot に GitHub 上の操作を依頼します。プロジェクトテンプレートでは GitHub のツールがあらかじめワークスペースに接続されているため、セットアップは不要です。Copilot がツールを初めて使用するとき、VS Code で GitHub へのサインインを求められる場合があります。画面の指示に従って認証してください。後の演習では、この接続の仕組みである Model Context Protocol (MCP) を学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェントの選択欄で **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する Issue を探し、現在のブランチから PR を作成して、指示の変更と今後のフィルター作業に向けた publisher ヘルパーの基礎の両方を説明するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. Copilot に代わって GitHub の操作を実行するための **Allow** が求められたら、コマンドを確認して、適切な場合に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージを依頼し、ブランチを `main` に戻します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot に代わって GitHub 操作やシェルコマンドを実行するための **Allow** が求められたら、コマンドを確認して、適切な場合に **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトの指示ファイルが Copilot にコンテキストを伝える方法を確認し、VS Code の Copilot Chat で次の操作を行いました。

- 既存の指示を適用した状態でコード生成のプロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` に新しいリポジトリ全体の標準を追加しました。
- 同じプロンプトをもう一度送り、生成されたコードが新しい標準に従うことを確認しました。
- 指示の更新と publisher ヘルパーの基礎を `main` にコミットし、次の演習で使えるようにしました。

次は、Copilot がコードベース全体に機能を追加する[エージェントモード][next-lesson]で、指示を活用します。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
