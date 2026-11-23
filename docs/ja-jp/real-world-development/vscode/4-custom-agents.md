---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローにおける特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成してエージェントを定義し、特定の作業をより効果的に実行するための指示、ベストプラクティス、コーディングパターン、分野固有の知識を Copilot に伝えます。チームの専門知識を再利用可能なエージェントとして形式化できます。たとえば、[WCAG][wcag] 準拠を強化するアクセシビリティエージェント、安全なコーディングに従うセキュリティエージェント、一貫したテストパターンを守るテストエージェントなどを作成できます。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を記載する Markdown プロンプトが必要です。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、役割が重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業の方法を指示します。最も分かりやすい違いは、**カスタムエージェントが作業者で、スキルがツール**という点です。

カスタムエージェントには専用のコンテキストウィンドウがあり、作業の一環としてスキルや他のエージェントを調整できます。このラボでは、アクセシビリティ向けのカスタムエージェントがアクセシビリティガイドラインに沿ってサイトをレビューし、更新します。その作業の一環として、プルリクエストワークフローのスキルやテストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI の他の用途と同様に、環境やシナリオに合わせてテストと反復を行ってください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるように取り組んでいます。現在のダークテーマは、テキストと背景色のコントラストが不十分で、読みにくいと感じるユーザーがいるという意見が寄せられました。このアクセシビリティ上の課題に対応するため、デザインチームはユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

この演習では、次の操作を行います。

- 既存のアクセシビリティ向けカスタムエージェントを確認します。
- Copilot Chat でアクセシビリティエージェントを使い、ハイコントラストモードを実装します。

## アクセシビリティ向けカスタムエージェントを確認する

アクセシビリティ向けのカスタムエージェントは、あらかじめ用意されています。内容を確認して、Copilot をどのように支援するかを理解しましょう。

Codespace に戻り、ターミナルを開いて、`main` からアクセシビリティ作業用の新しいブランチを作成します（演習 3 のフィルター PR とは分けて作業します）。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` フィールドと `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` フィールドと `description` フィールドを含むフロントマターが必要です。

3. 続くセクションを確認します。次の内容が記載されています。
   - アクセシブルな Web サイトのコードを生成するときの主な役割。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違い。

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェントのドロップダウンには、`.github/agents` で定義されたすべてのカスタムエージェントが表示されます。カスタムエージェントを選択すると、そのエージェントの指示やツールを使うチャットセッションを開始できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

1. エージェントのドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューでエージェントの選択欄を示す画面](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部にあるエージェントのドロップダウンを選択します。現在のエージェントが表示されています。既定では **default** です。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. アクセシビリティエージェントに次のプロンプトを送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調べ、変更を提案して、プロジェクトファイルに適用します。変更ごとに、ファイルパスと差分がチャットに表示されます。
6. 変更が適用されると、エージェントが変更した作業ファイルを示す **Files changed** の表示が更新されます。

> [!NOTE]
> Copilot が Astro コンポーネント、CSS、関連するテストなどのファイルを変更するため、数分かかる場合があります。

次の演習では、進行中の作業を確認し、追加の指示を送ります。

## まとめと次のステップ

このレッスンでは、GitHub Copilot の[カスタムエージェント][custom-agents]について学びました。特定のタスクや分野に合わせた AI アシスタントです。カスタムエージェントを使うと、チームの専門知識や標準を再利用可能な形にまとめ、特定の種類の作業をより効果的に Copilot に指示できます。

次の操作を行いました。

- カスタムエージェントの定義方法を確認しました。
- Copilot Chat のエージェントモードでカスタムエージェントを使用しました。

次は[エージェントの作業を監視して誘導][next-lesson]します。変更を確認し、同じセッションにライトモードの切り替えを追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
