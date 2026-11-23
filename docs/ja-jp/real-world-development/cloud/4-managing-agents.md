---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習で、ユーザーエクスペリエンスの改善や機能追加を目的とする3つのタスクを GitHub Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。[GitHub.com のエージェントページ][agents-page]もその1つです。このミッションコントロールでは、オープンプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。実行された操作を調べたり、進行中のセッションに追加の指示を送ったりできます。

このレッスンでは、次の操作を行います。

- エージェントページで cloud agent のタスクを監視します。
- 進行中のセッションに追加の機能を依頼します。

## シナリオ

エージェントにハイコントラストモードの作成を依頼した後、チームはライトモードも追加するよい機会だと考えました。サイトのスタイルを更新し、切り替え機能を追加しているため、ライトモードも一緒に実装するのが合理的です。エージェントに指示を追加し、ハイコントラストモードとライトモードの両方を実装してもらいます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインの両方にタスクの一覧が表示されます。Copilot に割り当てたタスクとして、次の項目が表示されます。
    - コードベースのドキュメントの更新。
    - ゲームを変更するための API の生成。
    - Web サイトへのハイコントラストモードの追加。
3. 実行中のタスクを選択し、Copilot が実行したタスクを確認します。次のような操作が含まれる場合があります。
    - リポジトリからコードをチェックアウトする。
    - Copilot が作業する環境を作成する。
    - MCP サーバーをセットアップする。
    - 割り当てられたタスクを完了するために、さまざまな手順を実行する。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認してください。PR と変更されたファイルを確認して、さらに監視できます。

## cloud agent の作業を誘導する

現在実行中のタスクを確認できたので、ハイコントラストモードの作業中にライトモードの切り替えも追加するよう Copilot に依頼しましょう。

1. ハイコントラストモードの追加に関連するセッションを選択します。正確なタイトルは、Copilot が付けた名前や作業状況によって異なります。

    ![ミッションコントロールのアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まるまで、セッションを数分間確認します。次のようなメッセージが表示されたら、作業が開始されています。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェントページで cloud agent タスクの誘導ダイアログが強調表示された画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを認識し、作業に取り入れる様子を確認します。

## Copilot の作業を待つ

Copilot は、更新されたタスクに取り組みます。メッセージ送信時に進行中だったステップを完了した後、新しい依頼をワークフローに取り込みます。

この作業にも数分かかります。待つ間に、ここまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、Copilot cloud agent のタスクを監視、誘導する中心的な場所である Copilot agents ページを確認しました。このミッションコントロールでは、実行中および完了したすべてのタスクを追跡し、実行内容をレビューして、進行中のタスクに追加の指示を送れます。

次の操作を行いました。

- ミッションコントロールとエージェントページを使って、cloud agent のタスクを監視しました。
- 進行中のセッションに追加の機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、[Copilot が作成したプルリクエストを改善][next-lesson]しましょう。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
