---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに `quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザーからフィルター機能を確認しました。次は、明確な QA プロセスを持つカスタムエージェントで、これらの機能を組み合わせます。

このレッスンでは、次の操作を行います。

- カスタムエージェントが指示、スキル、MCP ツールと連携する方法を確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA エージェントを選択し、フィルター機能の Issue と照らし合わせて指摘事項をレビューします。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザー上の動作を一貫してレビューしたいと考えています。カスタムエージェントを使って QA のプロセスを調整し、再利用可能なレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown のプロファイルで定義する Copilot の特化バージョンです。プロファイルには、エージェントの目的、指示、使用可能なツールを記載します。このワークショップでは、`.github/agents/qa.agent.md` で QA の役割を定義し、Copilot CLI で選択します。

これまでに使用したカスタマイズは、それぞれ異なる役割を持ちます。リポジトリの指示はチームの標準を説明し、`quality-checks` スキルは繰り返し実行できるチェックをまとめます。Playwright MCP はブラウザーツールを提供します。QA プロファイルは、それらを使って要件を評価し、結果を報告する方法を Copilot に伝えます。カスタムエージェントはこれらを置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用可能な QA プロファイルを Copilot に作成してもらいます。プロファイルには、QA が実行するチェックと、その作業の境界の両方を定義します。

1. Codespace に戻り、フィルター機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のプロンプトで新しいカスタムエージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、想定した QA ワークフローと作業の境界がプロファイルに反映されていることを確認します。これにより、検証だけを行うつもりなのに、不完全なエージェントや範囲の広すぎるエージェントが機能を変更するのを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを読みます。`description` は必須です。`name` は任意ですが、含めるとエージェントの表示名が明確になります。
3. プロファイルの指示を読み、QA が要件から開始し、リポジトリの指示に従い、`quality-checks` スキルを実行して Playwright MCP を使うことを確認します。
4. QA が根拠を示して結果を報告し、実装コードを変更する前に確認を求め、変更をコミットしたりプルリクエストを開いたりしないことを確認します。
5. これらの役割や制限がプロファイルに含まれていない場合は、続行する前に既定のエージェントに修正を依頼します。

## Issue を基に QA を実行する

Copilot CLI は、会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成した後に同じフィルター機能の会話を再開し、QA を選択すると、すでにコンテキストに含まれている Issue と計画の決定を利用できます。

1. 次のプロンプトで QA エージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成した直後は、利用可能なエージェントの一覧に表示されないことがあります。上記のコマンドを実行すると QA エージェントが有効になります。

2. 次のプロンプトで QA エージェントに機能をレビューしてもらいます。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 作業が完了したら、出力されたレポートを読みます。

## まとめと次のステップ

ワークフローに再利用可能な専門的役割を追加し、その作業をレビューしました。このレッスンでは、次の操作を行いました。

- カスタムエージェントが指示、スキル、MCP ツールと連携する方法を確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルター機能の Issue と照らし合わせて指摘事項をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートをレビューできる状態になりました。次は[機能 PR にまとめ、Agent Merge を使用します][next-lesson]。

## リソース

- [Copilot CLI 用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
