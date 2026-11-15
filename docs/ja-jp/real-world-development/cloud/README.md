---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot クラウドエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot クラウドエージェント](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot がクラウドで非同期に作業できます。GitHub で作業を割り当てると、クラウドエージェントはバックグラウンドでリポジトリを調査し、変更を行い、プルリクエストを開きます。その間、別の作業を続けられます。

各演習では、まずクラウドエージェントが従うカスタム指示を追加し、その後 GitHub issue を割り当てて実装を任せます。カスタムエージェントを確認して利用しながら進め方を調整し、エージェントのダッシュボードからセッションの進行を監視して誘導します。最後に、エージェントが作成したプルリクエストをレビューし、結果を改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成します |
| [1. カスタム指示][ex1] | コンテキスト | クラウドエージェントが従うカスタム指示を追加します |
| [2. クラウドエージェント][ex2] | 非同期エージェント | GitHub issue を Copilot クラウドエージェントに割り当てます |
| [3. カスタムエージェント][ex3] | 専門エージェント | カスタムエージェントを確認して利用します |
| [4. エージェントの管理][ex4] | 監視 | エージェントのセッションを監視して誘導します |
| [5. 反復的な改善][ex5] | レビュー | PR をレビューし、Copilot の作業を改善して、次の手順を選びます |

## 前提条件

このワークショップに参加する前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランを持つ GitHub アカウント
- [ ] アカウントまたは組織で有効になっている Copilot クラウドエージェント

> [!TIP]
> 有料プランをお持ちでない場合、認定を受けた学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、どの実習環境でもすべての手順を完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> クラウドエージェントは、Copilot Free を除く **Copilot Student、Pro、Pro+、Business、Enterprise** のすべてのプランで利用できます。Copilot Business または Enterprise では、管理者が組織に対して有効化する必要があります。

> [!NOTE]
> この実習環境では MCP を扱いません。Copilot で MCP サーバーを使う方法については、[CLI の実習環境](../cli/)または [VS Code の実習環境](../vscode/)をご覧ください。

## 始める

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
