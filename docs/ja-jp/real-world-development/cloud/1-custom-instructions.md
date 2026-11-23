---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を利用するうえで、コンテキストは重要です。特定の方法でタスクを実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストにアクセスできるようにします。[指示ファイル][instruction-files]を使うと、Copilot に希望する作業だけでなく、その進め方も伝えられます。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- `.github/copilot-instructions.md` に新しいリポジトリ全体の標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここではプロンプトの実行前後を比較しません。Copilot cloud agent は GitHub Issue に基づいて非同期で動作するため、リアルタイムで並べて効果を示すのは困難です。cloud agent が生成するプルリクエストをこのハーネスの後半でレビューするときに、指示ファイルの影響を確認します。

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

## 新しいリポジトリ標準を追加する

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を伝えるためのファイルです。コード提案を改善するために、リポジトリのコーディング標準へルールを追加しましょう。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を伝えるファイルです。コーディング標準を記載して、コード提案を改善しましょう。

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

## 指示の更新をコミット、プッシュ、マージする

cloud agent は、Issue の対象ブランチにある指示ファイルを読み込みます。次の演習で Copilot に Issue を割り当てるとき、Copilot は `main` からブランチを作成します。そのため、cloud agent が新しい指示を参照できるよう、変更を `main` にマージする必要があります。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。次の演習で Issue を割り当てるときに、cloud agent がこの指示を参照できるようになります。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチの手順を省略して、変更を `main` に直接コミットしてもかまいません。実際のプロジェクトでの進め方に近づけるため、このレッスンではブランチを使用しています。

## まとめと次のステップ

このプロジェクトの指示ファイルが Copilot にコンテキストを伝える方法を確認し、リポジトリ全体に適用する新しい標準を `.github/copilot-instructions.md` に追加しました。このハーネスの以降のレッスンで、cloud agent が作成するプルリクエストに標準が反映されていることを確認します。

次は、[Copilot cloud agent に最初の Issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
