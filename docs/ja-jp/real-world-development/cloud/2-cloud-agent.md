---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどがその例です。GitHub Copilot の cloud agent は、コードの更新や機能の追加といったタスクを自律的に実行できます。作業が終わると、人間の開発者がレビューできるドラフト PR を作成します。これにより、手間のかかるタスクを任せて開発を加速し、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って、次の操作を行います。

- コード生成環境をカスタマイズする。
- 操作を安全に行う方法を確認する。
- 範囲を明確に定めた Issue の重要性を理解する。
- Issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には、解決したい技術的負債があります。最初のサイトを開発した委託先が、望ましくない状態でドキュメントを残しました。実際には、ドキュメントがまったくありません。最初の対応として、アプリケーションのエクスポートされたすべての関数に TSDoc のドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する他のタイトルを「関連ゲーム」として表示し、閲覧を続けられるようにします。洗練された実装はまだ必要ありません。UX の受け入れテストに使える程度の機能を求めています。この作業は現在ブロッカーですが、優先度がより高い他の Issue があります。

どちらも、優先度を下げられてしまうことがあるタスクの例です。このようなタスクを Copilot cloud agent に割り当てれば、開発者は別の作業を非同期で進められます。後から Copilot の作業をレビューし、期待どおりか確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。[GitHub Issue を Copilot に割り当てる][assign-issue]など、いくつかの方法で作業を依頼できます。割り当て後、Copilot は進行状況を追跡するドラフトプルリクエストを作成し、環境をセットアップしてタスクを開始します。作業中でも完了後でも、Copilot のセッションを詳しく確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える必要がなくなるわけではありません。一見簡単そうな操作でも、詳しく見ると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階的に学び、試し、適応させます。生成 AI を利用しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示とその役割を学びました。すでにカスタム指示の演習を完了していれば、`.github/copilot-instructions.md` が機能することを確認しています。まだの場合は、少し時間を取って確認しましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを参照し、後でその影響を確認します。

リポジトリの GitHub Web UI で次のファイルを開くか、Codespace を使用している場合は Codespace で開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc のドキュメントコメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターを確認します。これは `**/*.test.ts` ファイルに指示を適用します。

次のセクションで **Code lacks documentation** Issue を cloud agent に割り当てると、プルリクエストに TSDoc のドキュメントコメント、TypeScript の規約、ファイルヘッダーが反映されます。後の演習で PR をレビューするときに、改めて確認します。

## Copilot cloud agent の開発環境をセットアップする

誰が作業する場合でも、コードを作成するときは通常、特定の環境や正常な状態を保つために実行するセットアップスクリプトが必要です。GitHub Copilot にタスクを割り当てる場合も同様です。Copilot も SWE と同じように作業を進めます。

Cloud agent は、作業環境として [GitHub Actions][github-actions] を使用します。作業を始める前に実行される[特別なセットアップワークフロー][setup-workflow]を `.github/workflows/copilot-setup-steps.yml` に作成して、環境をカスタマイズできます。これにより、必要な開発ツールや依存関係を利用できるようになります。このラボの流れを分かりやすくするため、あらかじめ設定済みです。Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、単一の Astro アプリ用にマイグレーションとシードを実行した SQLite データベースを利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *各ステップ*に必要な最小限の権限を設定します。Copilot の操作には専用のトークンが使用されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は、`contents: read` 権限が必要です。セットアップ手順でクローンしない場合、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体が Astro + Drizzle/libSQL になりました)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストに必要なデータをローカルの SQLite データベースにマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローファイルと同じですが、次の点に注意してください。

- `copilot-setup-steps` というジョブが1つあります。このジョブは、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot の作業を待たずにワークフローが正常に実行されることを確認できます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、多くのプロジェクトでは情報が古いか、まったく不足しています。こうした技術的負債は放置されがちで、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成を得意としているため、Copilot cloud agent に依頼するタスクに適しています。バックグラウンドで必要なドキュメントを作成し、次の演習でその作業をレビューします。

1. 新しいブラウザータブで、github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく生成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような名前になります。一覧に PR が表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が TODO リストを作成します。

> [!NOTE]
> Copilot が PR に TODO リストを表示するまで、数分かかることがあります。Copilot は環境を作成し（前述のワークフローを実行します）、プロジェクトを分析して、問題に取り組む方法を決めています。

13. リストと、実行予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッション表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新されたインジケーターを表示するために、ウィンドウの再読み込みが必要な場合があります。

16. ライブセッションをスクロールし、Copilot が問題を解決する様子を確認します。コードを調べて状態を把握し、計画を考えて決定した後、コードを作成する過程を確認できます。

この作業には数分かかる場合があります。Copilot cloud agent の主な目的の1つは、非同期にタスクを実行して別の作業に集中できるようにすることです。この機能を活用するため、もう1つのタスクも Copilot cloud agent に割り当ててから、アプリケーションに機能を追加する作業を行います。

## ゲーム詳細ページに関連ゲームのセクションを追加する

前述のとおり、GitHub Copilot cloud agent の大きな利点の1つは、作業を分担できることです。デザインチーム向けに関連ゲームのセクションを追加するのに時間がかからないとしても、その時間を別の作業に使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次のテキストを入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことが分かります。

![Issue に Copilot が取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼にも取り組んでいます。Copilot cloud agent は SWE と同様に動作するため、作業中に常に監視する必要はありません。完了後にレビューできます。次は、カスタムエージェントの作成と使用について確認しましょう。

## まとめと次のステップ

このレッスンでは、[GitHub Copilot cloud agent][copilot-agents]について学びました。cloud agent に Issue を割り当てると、非同期で実行できます。技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などを Copilot に任せられます。

次の操作を行いました。

- コード生成環境をカスタマイズしました。
- 操作を安全に実行する方法を確認しました。
- 範囲を明確に定めた Issue の重要性を学びました。
- Issue を Copilot に割り当てました。

cloud agent がバックグラウンドで作業している間に、次はカスタムエージェントを作成して使用します。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
