---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローで、GitHub Copilot CLI を使用しました。次の作業を行いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、作成済みのフィルター機能の Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター機能の Issue から開始し、Plan モードでアプローチを決め、Autopilot モードで構築して、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 要件、テストカバレッジ、スキルの結果、ブラウザーでの根拠を評価する品質保証 (QA) カスタムエージェントを作成して選択しました。
- フィルター機能全体をレビューし、フィルター機能の PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意の cloud agent への委任に使うスラッシュコマンドを確認しました。

## 作成したもの

このワークショップでは、更新済みの `main` から作成した各ブランチで、2つの PR の節目を迎えました。

1. **星評価:** ゲームカードに、既存の `starRating` と未評価状態を明示して表示します。
2. **フィルター機能と品質ワークフロー:** フィルター機能を実装し、指示を更新してその内容を機能に適用します。`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルター機能を計画してから PR を作成するまで、同じ会話とブランチを使用しました。ワークショップの手順を簡潔にするため、これらの作業を1つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP 経由の Copilot によるブラウザー操作など、いくつかの方法でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、追加した書式で結果を報告しました。QA はこれらの結果と、要件およびテストカバレッジのレビューをまとめて PR の確認に役立てました。

追加するテストは、実際の不足を補うものである必要があります。新たなテストが不要という QA の結果が正しい場合もあります。マージを許可する前に、コードと根拠を確認してください。変更があった場合は、影響を受ける根拠も更新します。

## ベストプラクティス

Copilot に与えるコンテキストとツールが作業内容を形作ります。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。会話をまたいでカスタマイズを再利用し、チームのニーズの変化に合わせて調整してください。指示は標準を設定し、スキルは反復可能なタスクを定義し、カスタムエージェントは専門的な役割を定め、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更やツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。構築前にアプローチを検討するには **Plan**、対象を絞った変更をやり取りしながら進めるには **Interactive**、範囲を明確にしたタスクを任せるには **Autopilot** を使います。日常的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選べます。

インフラストラクチャと同じように、コンテキストも重要です。構築する**内容**、その**理由**、望ましい**方法**を明確に説明すると、出力が大きく変わります。

## さらに確認する

主要なワークフローを完了しました。Copilot CLI には、ほかにも便利な機能があります。

- `/review` でコードレビューエージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を相談し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送ります。

## 次のステップ

どのツールでも、上達する最善の方法は使い続けることです。実際のコード、趣味のコード、長い間作ろうと思っていた小さなアプリなどで活用してください。チームと学んだことを共有し、互いの知見から学びましょう。いつものように、ドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張したい場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進んでください。他の環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
