---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys で一連の開発ワークフローを実施しました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、あらかじめ用意されたフィルター機能の issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター機能の issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を試しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、テストカバレッジ、スキルの結果、ブラウザーでの検証情報を確認しました。
- フィルター機能の変更全体をレビューし、フィルター機能の PR で Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意のクラウドへの委任に使えるスラッシュコマンドを確認しました。

## 作成したもの

このワークショップでは、更新済みの `main` からそれぞれ独自のブランチを作成し、2 つの PR を完了します。

1. **星評価**: 既存の `starRating` と未評価であることを示す状態をゲームカードに表示します。
2. **フィルター機能と品質ワークフロー**: フィルター機能を実装し、指示を更新してその機能に適用し、`quality-checks` レポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルター機能の計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを簡略化するため、これらの作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を介した Copilot のブラウザー操作という、複数の方法でコードを確認しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA は PR 作成前に、これらの結果と要件、テストカバレッジのレビューをまとめました。

追加のテストが必要な場合は、実際に不足している部分を補うテストを作成します。QA の実行で追加テストが不要と判断される場合もあります。マージを許可する前に、コードと検証の根拠をレビューしてください。変更があった場合は、影響を受ける検証の根拠を取り直します。

## ベストプラクティス

Copilot に提供するコンテキストとツールが、作業の成果を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整してください。指示は規約を定め、スキルは繰り返し実行するタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。実装前に方針を検討するには **Plan**、範囲を絞った変更を一緒に進めるには **Interactive**、範囲を明確にしたタスクには **Autopilot** を使います。定型的な編集には応答が速いモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラと同じように、コンテキストも重要です。何を、なぜ、どのように作りたいのかを明確に説明すると、結果が大きく変わります。

## さらに試す

コアのワークフローを確認できました。Copilot CLI には、次のような追加機能もあります。

- `/review` で、コードレビューエージェントに変更を分析してもらいます。
- `/rubber-duck` で、問題について対話し、別の視点を得ます。
- `/fleet` で、独立したサブタスクを並行して調整します。
- `/worktree` で、別のタスクを分離します。
- `/delegate` で、Copilot クラウドエージェントにタスクを送信します。

## 次のステップ

どんなツールも、使い続けることが上達への近道です。実際のコード、趣味のコード、何年も作ろうと思いながら手を付けていなかった小さなアプリなどで活用してください。チームと学びを共有し、ほかのメンバーからも学びましょう。そして、ドキュメントも確認してください。

ターミナルから Tailspin Toys を発展させる場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進んでください。別の環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot アプリのワークショップ][app]、または[Copilot クラウドエージェントのワークショップ][cloud]をご覧ください。

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
