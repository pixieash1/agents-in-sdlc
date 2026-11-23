---
title: "演習 5: GitHub Copilot の作業を改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザーエクスペリエンスや機能の改善を目的とする複数のタスクを GitHub Copilot に依頼してきました。コードへのドキュメント追加、デザインチーム向けの関連ゲーム機能の作成、ハイコントラストモードとライトモードによるアクセシビリティ機能の実装を依頼しました。コードの変更内容を確認し、必要に応じて Copilot にフィードバックを送って作業を改善しましょう。

### シナリオ

生成 AI を利用しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードを必ずレビューし、通常の DevOps プロセスに沿って作業します。ここでは、チームのレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関して GitHub Copilot が提案した変更を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期に、監督者なしでタスクを実行するため、安全性を維持するためのセキュリティ制約が設定されています。

- Copilot はリポジトリに対する読み取りアクセス権を持ち、コードを変更するブランチに対してのみ書き込みアクセス権を持ちます。
- Cloud agent は GitHub Actions 内で実行され、作業用の独立した一時環境を作成します。
- GitHub Actions のワークフローを実行するには、人間による承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは、デフォルトで制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR)、つまりコードへのドキュメント追加を確認しましょう。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認するとき、GitHub Copilot がファイアウォールでブロックされているという警告が表示される場合があります。Copilot は、外部 MCP サーバーへの呼び出しなど、外部リソースへのアクセスがデフォルトで制限されているため、これは**想定された動作**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** など、ドキュメントに関するタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が表示されます。その場合は、Copilot が作業を終えるまで待ちます。数分かかることがあるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. プルリクエストの準備ができたら、**Files changed** タブを選択し、変更を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc のドキュメントコメントやその他のドキュメントが追加されたコードを確認します。具体的な変更内容は異なる場合があります。

    変更を確認するときは、TSDoc のドキュメントコメント、TypeScript の規約、ファイルヘッダーに注目してください。これらは、演習 2 の最初に確認したカスタム指示ファイルによるものです。

6. 更新内容に問題がないことを確認したら、**Conversation** タブに戻り、下にスクロールします。
7. 承認待ちのワークフローがあることを示すインジケーターを確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローがキューに追加されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業するのは、一方通行ではありません。チームメンバーと同じように、プルリクエストやコードのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は予測できないため、必ずこの文面を使うという指定はできません。たとえば、次のような変更を Copilot に依頼できます。

- 各コードファイルの先頭に、ファイルの概要を説明するコメントヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. コードベースに追加したいドキュメントについて、上記の例または別のアイデアを使って変更を依頼します。コメントでは他のユーザーと同じように **@copilot** をメンションします。
2. **View Session** を選択し、Copilot が変更を行う様子を確認します。更新のために新しいセッションが開始されます。
3. **Back to pull request** を選択して、プルリクエストに戻ります。

    ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択し、変更を確認します。

納得できるまで反復できます。問題がなければ、ドラフト PR をレビュー可能な状態にし、`main` ブランチにマージします。

![レビュー可能な状態に PR を変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページへの関連ゲーム表示を依頼した Issue に対し、Copilot が作成した PR を確認しましょう。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、関連ゲームに関するタイトルの PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新内容に問題がないことを確認したら、**Conversation** タブに戻り、下にスクロールします。
6. 承認待ちのワークフローがあることを示すインジケーターを確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック欄にワークフローがキューに追加されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストすることもできます。Codespace に移動し、ターミナルを開いて、次のコマンドを実行します（`<branch-name>` は Copilot が作成したブランチ名、たとえば **copilot/fix-8** に置き換えます）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同様に、Copilot cloud agent に追加の変更を依頼して、反復できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーと TSDoc のドキュメントコメントを追加したりできます（この作業は、カスタム指示を更新する前に依頼したため、追加を依頼できます）。前の PR と同様に、**Conversation** タブでコメントを追加すると、Copilot が新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、カスタムのアクセシビリティエージェントで実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 でミッションコントロールから追加を依頼したライトモードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、ハイコントラストモードに関するタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が表示されます。その場合は、Copilot が作業を完了するまで数分待ちます。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 次の実装に注目しながら、変更を確認します。
   - モードを切り替える UI コンポーネント。
   - ユーザー設定を維持する local storage の使用。
   - ハイコントラストモードとライトモードの CSS またはスタイルの変更。
   - アクセシビリティ属性（ARIA ラベル、キーボード操作など）。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 更新内容に問題がないことを確認したら、**Conversation** タブに戻り、下にスクロールします。
7. 承認待ちのワークフローがあることを示すインジケーターを確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローがキューに追加されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、次のコマンドを実行します（`<branch-name>` は Copilot が作成したブランチ名に置き換えます）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイコントラストモードとライトモードの切り替えが期待どおりに動作し、ページを再読み込みしても設定が維持されることを確認します。

カスタムのアクセシビリティエージェントが、アクセシビリティのベストプラクティスに沿って Copilot の実装を支援したことを確認してください。アクセシビリティ上の懸念や改善点がある場合は、これまでの PR と同じようにコメントで **@copilot** をメンションし、更新を依頼できます。

## 任意の演習 — 作業の委任を続ける

cloud agent に実際のバックログ項目を任せて、その間に別の作業を進めると効果的です。練習として、リポジトリにいくつか Issue を作成し、Copilot に割り当ててみましょう。次のようなアイデアがあります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。これまでのレッスンでは、次の操作を行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認し**、cloud agent の出力に影響することを確かめました。
- **Copilot cloud agent に Issue を割り当て**、環境のセットアップ、計画、非同期の実行を確認しました。
- **カスタムエージェントを作成して使用し**、アクセシビリティ向けのハイコントラストモードとライトモードを追加しました。
- **Copilot agents ページをミッションコントロールとして使い**、アクセシビリティ機能のセッションを監視して途中で指示を追加しました。
- **Copilot のプルリクエストをレビューして反復し**、`@copilot` に変更を依頼してワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。他のハーネスを使うと、ここで学んだ内容をさらに広げられます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat のエージェントモードと MCP 統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、エージェントスキル、カスタム QA エージェント、Playwright MCP、Agent Merge、cloud agent に接続する任意の `/delegate` ブリッジを使い、ターミナルからレビュー済みの変更を2つ作成します。

自分のリポジトリでは、次のようなフォローアップを試してください。

- リファクタリングやテストカバレッジの Issue を cloud agent に割り当てる。
- 別の分野向けにカスタムエージェントを作成する。
- 異なる技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
