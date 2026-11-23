---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を利用するうえで、コンテキストは重要です。特定の方法でタスクを実行する必要がある場合は、そのガイダンスを Copilot が利用できるようにします。[指示ファイル][instruction-files]には、必要なコードの内容だけでなく、その構成方法も記述します。フィルター機能を構築したので、Copilot が使用した指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次の操作を行います。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに伝わる方法を確認します。
- コーディング標準に従うよう指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プロセスに関するガイドラインと要件があります。具体的には次のとおりです。

- コメントにはコードの内容を繰り返さず、意図や自明ではない判断を説明します。
- `db/` と `src/lib/` のエクスポートされた関数には、目的、パラメーター、戻り値を TSDoc/JSDoc で記載します。注入可能な `db` 引数がある場合は、それも説明します。
- 再利用可能な Astro コンポーネントには `Props` の契約を文書化し、関連コードが変わったらコメントも最新に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使って、Copilot がこれらの開発慣行に沿ってタスクを実行できるようにします。

## 指示ファイル

カスタム指示では、Copilot がコーディングスタイルや要件を理解できるよう、コンテキストや好みを指定します。コーディング規約、ライブラリ、コードに含めたいコメントの種類などを指定して、Copilot の提案やコードスニペットをより適切なものにできます。リポジトリ全体または特定の種類のファイルに対する指示を作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。ほとんどのリクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種類、タスク向けのガイドラインを提供します。

> [!NOTE]
> ほかの指示形式やサポート状況は環境によって異なります。特定の形式を利用する前に、[カスタム指示のサポートリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スタータープロジェクトには、作業を始めやすくするため、いくつかの指示ファイルがあらかじめ含まれています。変更を加える前に既存のファイルを確認し、その影響を見てみましょう。

1. Codespace に戻ります。
2. Codespaces エディターで（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディングガイダンスを確認します。この指示は、このリポジトリで Copilot と行うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドには、指示の適用対象を決める glob が設定されています。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存のプロジェクトファイルへのリンクを確認します。大きな指示を再利用可能な小さなファイルに分け、Copilot が参照できる例を提示できます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルはよい出発点ですが、まだ不足している部分があります。新しく生成される TypeScript に TSDoc コメントを追加するため、中心となる `copilot-instructions.md` ファイルを変更します。

1. `.github/copilot-instructions.md` で、**Code formatting guidance** というセクションを探します。35行目付近にあります。
2. 最後の箇条書きに次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを適用する

Copilot CLI は、会話の開始時にリポジトリの指示を読み込みます。更新した指示を保存した後、フィルター機能の会話を終了して再開すると、機能のコンテキストを保ったまま、新しい指示を読み込めます。

1. フィルター機能の会話がまだ開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリルートで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合は、フィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. 次のプロンプトを使い、更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しく生成された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。具体的には、次の操作を行いました。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに伝わる方法を確認しました。
- コーディング標準に従うよう指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、lint とテストを一貫して実行するため、[再利用可能な quality-checks スキルをカスタマイズして実行します][next-lesson]。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
