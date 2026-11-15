---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用しましょう。変更の計画、コードの作成、動作のテスト、プルリクエストのレビュー、エージェントを使った作業管理を繰り返しながら進めます。

## シナリオ

あなたは、開発者をテーマにしたボードゲームのクラウドファンディングを運営する架空の会社、Tailspin Toys に新しく加わった開発者です。チームのバックログは GitHub issue として登録済みで、すぐに着手できます。フィルターやページネーションなどの機能開発に加えて、アクセシビリティやコーディング規約などの品質改善も含まれています。

## 環境を選ぶ

GitHub Copilot は、どの環境でも利用できます。使いたい開発環境を選んでください。各ワークショップでは、それぞれ独自のセットアップを行い、共通の Tailspin Toys シナリオを使用します。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、指示、スキル、カスタムエージェント、Playwright MCP、Agent Merge、実用的なスラッシュコマンドを扱います。

### [GitHub Copilot アプリ][app]

デスクトップアプリで複数のエージェントセッションを並行して実行し、セッションモードを切り替え、キャンバスで共同作業を行い、GitHub issue とプルリクエストを管理します。

### [GitHub Copilot クラウドエージェント][cloud]

GitHub issue を非同期エージェントに割り当て、作業を案内し、進行状況を監視して、エージェントが作成したプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに、Copilot Chat の agent mode、MCP サーバー、カスタムエージェントを利用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
