---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、通常は複数のファイルを変更し、テストなどを実行する必要があります。開発者は必要なファイルを探して変更し、テストを実行し、問題をデバッグし、更新し忘れたファイルを見つけて、さらに修正することになります。作業は次々と続きます。

ここで役立つのが Copilot エージェントモードです。

Copilot エージェントモードは、IDE 内でより自律的に動作します。開発者と同じように、まず既存のプロジェクト構造を調べ、必要な更新を行い、テストなどのタスクを実行して、見つかった問題を自動的に修正します。エージェントモードを使って、サイトに新しい機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる用途向けに構築されています。エージェントモードは IDE 内で動作するため、すばやいフィードバックや対話が可能です。Cloud agent はチームメンバーのように非同期で作業し、Issue やプルリクエストを通じてやり取りします。

この演習では、次の操作を学びます。

- Copilot エージェントモードにプロジェクトを調べさせ、関連ファイルを特定させて、連携した変更を行わせる。
- GitHub Copilot エージェントモードで、UI とデータレイヤーにまたがる新機能を実装する。
- Copilot エージェントモードが生成した変更とテストをレビューしてから、コードベースにマージする。

## シナリオ

ゲームの数が増えてきたため、ユーザーがカテゴリや publisher で絞り込めるようにします。前の演習で publisher ヘルパーを追加しました。次は、Copilot エージェントモードを使って、残りのデータレイヤー、UI、テストの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを見て、現在の機能を確認しましょう。

この Web サイトは、開発者向けテーマのボードゲームクラウドファンディングプラットフォームです。ゲームの一覧や詳細を表示できます。Web サイトは単一の Astro アプリで、ビルド時に静的 HTML としてページを生成します。ページは frontmatter 内で Drizzle ORM を使ってローカルの SQLite データベースを直接参照します。独立したバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、インタラクティブ機能には標準の DOM API を使用する小さな Astro `<script>` を追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習を終えていれば、`main` ブランチにいるはずです。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、Codespace 内に新しいターミナルウィンドウを開きます。
3. フィルター機能の作業用に、新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を含むバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くかどうか尋ねるダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルの Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的にリダイレクトされます。これは、Web サーバーをホストしている Codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトを起動したら、機能を確認しましょう。主な機能は次のとおりです。

- **ホームページ**: タイトル、画像、説明を含むボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、そのタイトル、説明、publisher、カテゴリなどの情報を表示する詳細ページが開きます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

Web サイトは現在のところ動作しますが、さらに機能を追加したいと考えています。まず、バックログを確認しましょう。テンプレートからプロジェクトをセットアップすると、GitHub Issue のバックログも自動的に作成されています。GitHub Copilot に項目を表示してもらいます。

1. Chat ビューのエージェントのドロップダウンから **Agent** を選択します。**Agent** はファイルをまたぐ変更を自律的に計画して実装し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューでエージェントの選択欄を示す画面](../../../_images/shared-chat-mode-selector.png)

2. 使用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者は、特定のモデルを推奨するものではありません。ワークショップの作成時に Claude Sonnet 4.5 を使用したため、手順に含めています。これにより、比較的一貫したコード提案が得られ、スムーズに進められることを期待しています。ただし、大規模言語モデルの出力は確率的に変化するため、手順とは異なる提案が表示される場合があります。これは正常な動作です。

> [!NOTE]
> 大規模言語モデルの性質上、Copilot が別の MCP コマンドを使う場合がありますが、タスクは同様に完了できます。

3. Copilot に次のプロンプトを送信し、Issue のバックログについて尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot がユーザーに最も役立つと思われる項目を優先順位付けしたことを確認してください。

## 指示ファイルを確認する

Copilot にコードを生成させる前に、作業のコンテキストを伝えるために使う指示ファイルを確認しましょう。Web サイトの機能追加の進め方を説明する [user interface (UI)][ui-instructions] ファイルを使用します。

1. Codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能を追加する際の全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティに関する原則。
   - Astro、Tailwind CSS、Drizzle など、ファイル種類ごとの指示ファイルへのリンク。

> [!TIP]
> 指示ファイルでは、他の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用し、大きな指示セットを管理しやすい単位に分け、コード生成の例やテンプレートを示すことができます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくともアプリケーションに次の3種類の変更が必要です。

- データレイヤー (`src/lib/`) で残りのフィルターロジックを追加または調整する。
- フィルターの動作を確認するテストを追加または更新する。
- ゲーム一覧ページにフィルター UI を追加する。

また、すべてのテストを実行して成功を確認してから、コードベースにマージする必要があります。Copilot エージェントモードにこれらの作業を任せましょう。

1. 現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択し、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示す画面](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントのドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューでエージェントの選択欄を示す画面](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログの関連 Issue を基に機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がまずプロジェクトを調べ、必要なファイルを特定する様子を確認します。データレイヤーのヘルパー、UI、テストを探し、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトファイルを調べている画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、変更、テストの実行など、複数のタスクを実行します。タスクの複雑さやコードベースによって、数分かかる場合があります。その間、エディターに **Keep** と **Undo** ボタンが表示されることがあります。Copilot が完了すると、変更ごとに **Keep** または **Undo** が表示されるため、作業中に選択する必要はありません。

7. Copilot から求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat ペインでテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。問題ありません。Copilot はコード生成とテストを繰り返し、タスクを完了してエラーが見つからなくなるまで修正します。

   ![Copilot エージェントモードの完了したチャットセッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新しい機能を試します。
11. 動作を確認してコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

おめでとうございます。この演習では、GitHub Copilot エージェントモードを使って Tailspin Toys の Web サイトに機能を追加する方法を確認しました。次の操作を学びました。

- GitHub Copilot エージェントモードで、UI とデータレイヤーにまたがる新機能を実装する。
- Copilot エージェントモードにプロジェクトを調べさせ、関連ファイルを特定させて、連携した変更を行わせる。
- Copilot エージェントモードが生成した変更とテストをレビューしてから、コードベースにマージする。

次は [Playwright MCP サーバーで機能をテスト][next-lesson]し、プルリクエストを作成します。

### おまけの演習 — ページングを実装する

ゲーム一覧が増えると、ページングが必要になります。この演習で学んだことを使い、Copilot にページングを実装するよう依頼します。次の点を検討してください。

- 既存の指示ファイルを使うなど、現在のベストプラクティスに従います。
- ページサイズをユーザーが選べるようにするか、固定にするかなど、ページングの方法を検討します。
- 希望どおりの実装に必要なガイダンスを含めて、プロンプトを作成します。
- GitHub Copilot にコンテキストを伝えて変更を依頼し、何度か反復する必要があるかもしれません。Copilot を使う通常のワークフローです。

## リソース

- [Copilot の ask、edit、agent モード — それぞれの役割と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
