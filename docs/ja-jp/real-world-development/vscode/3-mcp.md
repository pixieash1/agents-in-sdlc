---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードでフィルター機能を構築しました。プルリクエストを作成する前に、ブラウザーで実際に動作することを確認しましょう。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させてテストします。その後、ブランチを公開して PR を作成します。

この演習では、次の操作を行います。

- Model Context Protocol (MCP) とは何か、MCP サーバーが Copilot にツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にサーバーを使用させ、ブラウザーでフィルター機能を手動テストします。
- ブランチを公開し、フィルター機能のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外部に接続できると、さらに多くの作業を行えるようになります。Copilot が外部のツールやサービスと通信するための標準的な方法が Model Context Protocol (MCP) です。

![エージェントモードがコンテキスト、LLM、ツール（MCP サーバーや VS Code 拡張機能が提供するツールを含む）と連携する仕組みを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントが外部のツールやサービスと通信できます。MCP 経由で、リソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わって操作を実行したりできます。

これらのツールやリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを通じてアクセスします。MCP サーバーは、既存の API や NPM パッケージなどのローカルツールと AI エージェントとの通信を管理します。各 MCP サーバーが、それぞれ異なるツールとリソースを提供します。

よく使われる MCP サーバーの例を紹介します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。AI エージェントを使って、リポジトリの新規作成や更新、Issue やプルリクエストの管理などを行えます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。AI エージェントを使って、Web ページへの移動、フォームへの入力、ボタンの選択などを行えます。

ほかにも、さまざまなツールやリソースにアクセスできる MCP サーバーがあります。GitHub がホストする [MCP レジストリ](https://github.com/mcp)で、コミュニティ内の MCP サーバーを探したり、投稿したりできます。

> [!CAUTION]
> セキュリティに関して、MCP サーバーはプロジェクト内の他の依存関係と同様に扱ってください。使用する前に、ソースコードを確認し、発行元を検証し、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使用し、機密リソースや操作へのアクセスを許可するときは注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで利用する MCP サーバーを構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されています。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` の設定はプロジェクトテンプレートに含まれているため、前の演習では Copilot がバックログの Issue を読み取れました。これは[リモート GitHub MCP サーバー][remote-github-mcp-server]を使用します。ローカルへのインストールは不要です。VS Code が HTTP 経由で接続し、Copilot が初めてツールを使用するときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot がブラウザーを操作できます。機能をテストするのに最適です。

1. `.vscode/mcp.json` で `github` と同じ階層に `playwright` の設定を追加し、ファイルが次のようになるようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートサーバーの `github` とは異なり、`playwright` はローカルサーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると、Playwright は表示ウィンドウなしでブラウザーを実行します。Codespace には表示用デスクトップがないため、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトでは、エンドツーエンドテストにも Playwright を使用しているため、通常は Playwright に必要なブラウザーがすでにインストールされています。Copilot からブラウザーが見つからないという報告があった場合は、`npx playwright install chromium` を実行して、もう一度試してください。

## サーバーを起動して信頼する

VS Code は、MCP サーバーのツールが初めて必要になったときに、**必要に応じて**サーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** 操作が表示されます。選択してサーバーを起動し、設定が正しいことを確認します。
2. VS Code からサーバーを信頼するかどうか尋ねられた場合は、設定を確認し、信頼して起動します。

手動でサーバーを起動する必要はありません。省略した場合は、次の手順で Copilot が初めてツールを使用するときに、VS Code がサーバーを起動して信頼するか確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを操作して確認する代わりに、エージェントにページを開かせ、移動やフィルターの適用を行わせ、結果を読み取らせて、その内容を報告させます。会話を続けながら機能が期待どおりに動作することを確認できます。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティツリー][playwright-mcp-server]を参照します。つまり、エージェントはボタン、リンク、リスト項目などのラベル付き要素を使って推論します。これは支援技術と同じような方法です。そのため、機能の簡易テストと同時に、アクセシビリティの基本も確認できます。

サーバーを接続してアプリを起動したら、次のプロンプトを Copilot に送り、フィルター機能をテストします。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバー経由でブラウザーを起動し、各手順を実行して結果を報告します。レポートを Issue の受け入れ条件と照らし合わせてください。期待どおりでない場合は、追加の質問をするか、コードを修正するよう依頼してからプルリクエストを作成します。

> [!NOTE]
> このテストには、`http://localhost:4321` でアプリが実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前にもう一度起動してください。Copilot が Playwright MCP サーバーを初めて使うとき、ブラウザーをダウンロードする場合があります。ブラウザーが見つからないという報告があった場合は、`npx playwright install chromium` を実行して、もう一度試してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認したので、チームでレビューするためにプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace で **Source Control** パネルに移動し、Copilot が作成した変更を確認します。
2. **+** アイコンを選択して、変更をステージングします。
3. **Sparkle** ボタンを使ってコミットメッセージを生成します。

   ![変更内容を示す Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンドラインインターフェイス (CLI) など、プルリクエストを作成する方法はいくつかあります。ここでは GitHub Copilot に依頼します。関連する Issue を見つけ、関連付けられた PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. Copilot が GitHub ツールを使用できるよう、エージェントのドロップダウンから **Agent** が選択されていることを確認します。
3. 次のプロンプトを送信して、PR の作成を Copilot に依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を収集して操作を行う際に、必要に応じて **Continue** を選択します。GitHub ツールを初めて使用するときは、GitHub での認証を求められる場合があります。画面の指示に従って認証してください。
5. Copilot が Issue を検索し、該当するものを見つけて PR を作成する様子を確認します。
6. Copilot が作成したリンクを選択してプルリクエストを確認します。ただし、まだマージ**しないでください**。

## まとめと次のステップ

おめでとうございます。この演習では、次の操作を行いました。

- Model Context Protocol (MCP) とは何か、MCP サーバーが Copilot にツールを追加する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- Playwright MCP サーバーでブラウザーを操作し、リリース前にフィルター機能を手動でテストしました。
- ブランチを公開し、フィルター機能のプルリクエストを作成しました。

MCP サーバーを使って機能をテストしました。Copilot の機能を拡張する方法は MCP だけではありません。次は[カスタムエージェントを作成][next-lesson]し、アクセシビリティの確認など、特定のタスクを効率化します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP レジストリ][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
