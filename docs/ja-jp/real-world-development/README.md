---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完全なアプリケーション、共有バックログを通じて、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用しましょう。変更の計画、コードの作成、動作のテスト、プルリクエストのレビュー、エージェントを使った作業の管理を反復して行います。

## シナリオ

架空の企業 Tailspin Toys の新しい開発者として参加します。Tailspin Toys は、開発者向けテーマのボードゲームをクラウドファンディングで支援する会社です。チームのバックログは GitHub Issue として用意されているので、すぐに作業を開始できます。フィルターやページネーションなどの機能開発に加えて、アクセシビリティやコーディング標準の改善も含まれます。

## 環境を選ぶ

GitHub Copilot は、どの環境でも利用できます。作業方法に合った環境を選んでください。各ワークショップでは専用のセットアップ手順から始め、共通の Tailspin Toys シナリオに取り組みます。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、指示、スキル、カスタムエージェント、Playwright MCP、Agent Merge、実用的なスラッシュコマンドを活用します。

### [GitHub Copilot app][app]

デスクトップアプリでエージェントセッションを並行して実行し、セッションモードを切り替え、キャンバスで共同作業を行い、GitHub Issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期エージェントに割り当て、作業を誘導し、進行状況を監視して、作成されたプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに、Copilot Chat のエージェントモード、MCP サーバー、カスタムエージェントを活用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
