---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に特化した AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義すると、特定の作業を効果的に行うための指示、ベストプラクティス、コーディングパターン、分野固有の知識を Copilot に提供できます。チームの専門知識を再利用可能なエージェントとして明文化できます。たとえば、[WCAG][wcag] への準拠を徹底するアクセシビリティエージェント、安全なコーディングプラクティスに従うセキュリティエージェント、一貫したテストパターンを維持するテストエージェントなどです。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダーにある Markdown ファイル、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を定義する Markdown プロンプトを記述します。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、役割が一部重複するところがあります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。違いを分かりやすく表すと、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキストウィンドウを持ち、作業の一部としてスキルやほかのエージェントを調整するよう設計されています。このラボでは、アクセシビリティのカスタムエージェントがアクセシビリティガイドラインに照らしてサイトをレビューし、更新します。その作業の一環として、プルリクエストのワークフロー用スキルやテストの実行・管理用スキルなどを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成方法に唯一の正解はありません。AI に関するほかのことと同様に、利用する環境やシナリオに合わせてテストと調整を続けてください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるよう取り組んでいます。最近、現在のダークテーマではテキストと背景色のコントラストが不十分で、読みにくいというフィードバックがありました。このアクセシビリティの問題に対処するため、デザインチームは、ユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なので、この機能をできるだけ早く実装したいと考えています。カスタムエージェントを使って機能を生成しましょう。

この演習では、次のことを行います。

- 既存のアクセシビリティ用カスタムエージェントを確認する。
- Copilot Chat でアクセシビリティエージェントを使い、ハイコントラストモードを実装する。

## アクセシビリティのカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントは、すでに用意されています。内容を確認して、Copilot をどのように導くか理解しましょう。

Codespace に戻り、ターミナルを開いて `main` からアクセシビリティ作業用の新しいブランチを作成します (演習 3 のフィルター PR とは分けて作業します)。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` のフィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを確認します。アクセシブルな Web サイトのコード生成を担当する際の要件、アクセシビリティのベストプラクティス、HTML・CSS・JavaScript のコード例、よくある問題と間違いなどが記載されています。

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat ビューの下部にあるエージェントドロップダウンには、`.github/agents` に定義されたすべてのカスタムエージェントが表示されます。カスタムエージェントを選択すると、そのエージェントの指示とツールに基づいてチャットセッションを進められます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、必要な箇所で説明します。

1. Chat ビューのエージェントドロップダウンで、**Agent** がまだ選択されていない場合は選択します。

   ![Chat ビューのエージェント選択を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. チャットビューの下部にあるエージェントドロップダウンを選択します。アクティブなエージェントが表示されます。既定では **default** です。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティエージェントに送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat が作業を始め、コードベースを調査して変更を提案し、プロジェクトファイルに編集を適用します。変更ごとに、ファイルパスとレビューできる差分がチャットに表示されます。
6. 編集が適用されると、**Files changed** のインジケーターが更新され、エージェントが変更した作業セットを確認できます。

> [!NOTE]
> この処理には数分かかることがあります。Copilot はリポジトリに実際の変更を加えています。作業中に Astro コンポーネント、CSS、関連テストなどの既存ファイルを編集します。

次の演習では、進行中の作業をレビューして誘導します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に特化した AI アシスタントである [GitHub Copilot のカスタムエージェント][custom-agents]について学びました。カスタムエージェントを使うと、チームの専門知識や規約を再利用可能なエージェントとして明文化し、Copilot がより効果的に作業できるように導けます。

次の概念を確認しました。

- カスタムエージェントを定義する方法を確認しました。
- Copilot Chat の agent mode でカスタムエージェントを使いました。

次は[エージェントの作業を監視して誘導し][next-lesson]、変更を進行中にレビューして、同じセッションにライトモードの切り替えを追加します。

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
