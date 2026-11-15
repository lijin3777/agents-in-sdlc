---
slug: ja-jp/real-world-development/app
title: "GitHub Copilot app"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** は Copilot CLI を基盤とするデスクトップアプリケーションで、エージェント主導の開発を1つの専用ワークスペースで実現します。並列エージェントセッション、切り替え可能なセッションモード、共有キャンバス、GitHub issue とプルリクエストのネイティブ管理機能を備えています。さらに **Agent Merge** を使うと、プルリクエストのリベース、レビューのフィードバック、継続的インテグレーション (CI) の修正、マージを管理できます。

このワークショップでは、Tailspin Toys で一連のワークフローを進めます。

1. プロジェクトを準備し、アプリをインストールして、リポジトリを接続します。ワークスペースと、あらかじめ用意されたバックログも確認します。
2. 星評価に対象を絞った変更を加え、ブラウザーでレビューして、最初のプルリクエスト (PR) を手動でマージします。
3. フィルター機能の issue から始め、**Plan** モードでアプローチを定義し、**Autopilot** モードで実装して、**Interactive** モードでレビューします。
4. リポジトリの指示を更新し、フィルター機能の作業に適用します。
5. 既存の `quality-checks` スキルをカスタマイズし、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルター機能を試します。
7. 品質保証 (QA) カスタムエージェントを作成して、要件、カバレッジ、検証の根拠をレビューします。
8. フィルター機能の変更全体をレビューし、2 つ目の PR に Agent Merge を使います。
9. キャンバスが利用できることを確認し、Database Explorer を使います。続いて、リポジトリに保存するトリアージキャンバスを作成してテストします。必要な機能を利用できない場合は、ブロッカーを記録して読み取り専用の代替方法を使うか、まとめの演習に進みます。

ワークショップの焦点を絞るため、PR は 2 つ作成します。1 つ目は星評価、2 つ目はフィルター機能と指示・スキル・QA プロファイル・テストの更新です。それぞれ、更新済みの `main` から開始します。フィルター機能と品質ワークフローでは、1 つのセッション、worktree、ブランチを共有し、ツールを学びながら作業を積み重ねます。最後のキャンバス演習はそのセッション内で行い、PR ワークフローを繰り返すのではなく、共有サーフェスの作成とテストに集中します。

## レッスン

| レッスン | トピック | 説明 |
|--------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | Node.js をインストールし、Tailspin Toys プロジェクトの自分用コピーを作成します |
| [1. Copilot app のインストール][ex1] | セットアップ | アプリをインストールしてプロジェクトを接続し、ワークスペースを確認します |
| [2. 星評価の追加で小さな成果を得る][ex2] | 最初の変更 | 既存の評価と null の場合の代替表示を追加し、PR 1 をマージします |
| [3. エージェントモード: Plan と Autopilot][ex3] | エージェントモード | issue から機能を計画し、Autopilot で構築して、Interactive モードでレビューします |
| [4. カスタム指示で Copilot を導く][ex4] | コンテキスト | 指示を確認して更新し、フィルター機能に適用します |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し実行できるチェック | 既存のスキルを確認し、レポート形式を変更して実行します |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの観察 | Customize から MCP を構成し、フィルターの動作を確認します |
| [7. QA エージェントを作成して使う][ex7] | 要件とカバレッジ | 専門的なプロファイルを作成して選択し、最終検証の根拠を集めます |
| [8. 機能 PR を作成してマージする][ex8] | レビューとマージ | フィルター機能、指示、スキル、QA プロファイル、テストをレビューし、2 つ目の PR に Agent Merge を使います |
| [9. キャンバスを確認して作成する][ex9] | コラボレーション | Database Explorer を使い、リポジトリに保存するトリアージキャンバスを作成してテストします |
| [10. 振り返りと次のステップ][ex10] | まとめ | ワークフロー、成果物、追加のリソースを確認します |

## 前提条件

このワークショップに参加する前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランを持つ GitHub アカウント
- [ ] **macOS、Linux、Windows** を実行するコンピューター
- [ ] コンピューターに[インストールされた Git][install-git]

> [!TIP]
> 有料プランを利用していない場合、認定を受けた学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれています。そのため、どの実習環境でもすべての手順を完了できます。

> [!NOTE]
> Copilot app は Codespace ではなく自分のコンピューターで動作します。[前提条件の演習][ex0]では、アプリをインストールする前に Node.js をインストールし、プロジェクトの自分用コピーを作成します。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合、管理者が **GitHub Copilot app** ポリシーを有効にしておく必要があります。この設定は Copilot CLI のポリシーとは別です。

## 始める

**[前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
