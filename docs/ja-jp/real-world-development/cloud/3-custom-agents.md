---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に特化した AI アシスタントを作成できます。プロジェクトの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義すると、特定の作業を効果的に行うための指示、ベストプラクティス、コーディングパターン、分野固有の知識を Copilot に提供できます。チームの専門知識を再利用可能なエージェントとして明文化できます。たとえば、[WCAG][wcag] への準拠を徹底するアクセシビリティエージェント、安全なコーディングプラクティスに従うセキュリティエージェント、一貫したテストパターンを維持するテストエージェントなどです。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダーにある Markdown ファイル、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を定義する Markdown プロンプトを記述します。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、役割が一部重複するところがあります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。違いを分かりやすく表すと、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキストウィンドウを持ち、作業の一部としてスキルやほかのエージェントを調整するよう設計されています。このラボでは、アクセシビリティのカスタムエージェントがアクセシビリティガイドラインに照らしてサイトをレビューし、更新します。その作業の一環として、プルリクエストのワークフロー用スキルやテストの実行・管理用スキルなどを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成方法に唯一の正解はありません。AI に関するほかのことと同様に、利用する環境やシナリオに合わせてテストと調整を続けてください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタムエージェントを使って、次のことを行います。

- カスタムエージェントを定義する方法を確認する。
- カスタムエージェントにタスクを割り当てる。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるよう取り組んでいます。最近、現在のダークテーマではテキストと背景色のコントラストが不十分で、読みにくいというフィードバックがありました。このアクセシビリティの問題に対処するため、デザインチームは、ユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なので、この機能をできるだけ早く実装したいと考えています。カスタムエージェントを使って機能を生成しましょう。

## アクセシビリティのカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントは、すでに用意されています。内容を確認して、Copilot をどのように導くか理解しましょう。

Codespace に戻り、アクセシビリティ用のカスタムエージェントファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` のフィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを確認します。アクセシブルな Web サイトのコード生成を担当する際の要件、アクセシビリティのベストプラクティス、HTML・CSS・JavaScript のコード例、よくある問題と間違いなどが記載されています。

## issue を作成して割り当てる

ミッションコントロールは、環境内のエージェントと作業するための中心的な場所です。Copilot クラウドエージェントにタスクを割り当て、監視し、追加のガイダンスを与えて作業の方向を変えることもできます。まず、ハイコントラストモードを作成するタスクを Copilot に割り当てましょう。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てのダイアログを開きます。
9. カスタムエージェントの一覧から **Accessibility agent** を選択します。

    ![クラウドエージェントの割り当て画面で、カスタムエージェントとアクセシビリティが強調表示されたスクリーンショット](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドでタスクを開始します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に特化した AI アシスタントである [GitHub Copilot のカスタムエージェント][custom-agents]について学びました。カスタムエージェントを使うと、チームの専門知識や規約を再利用可能なエージェントとして明文化し、Copilot がより効果的に作業できるように導けます。

次の概念を確認しました。

- カスタムエージェントを定義する方法を確認する。
- カスタムエージェントにタスクを割り当てる。

Copilot がハイコントラストモードの実装を進めている間に、ミッションコントロールから[エージェントセッションを監視して誘導する][next-lesson]方法を確認しましょう。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使う準備をする][org-custom-agents]
- [エンタープライズでカスタムエージェントを使う準備をする][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot クラウドエージェント][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
