---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルター機能を検証するために使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くことだけが開発ではありません。手動でコードの動作を確認し、指示ファイルで規約に従うようにしました。では、テストはどうでしょうか。lint や継続的インテグレーション (CI) のほかの作業はどうでしょうか。

このようなタスクには、**エージェントスキル**が適しています。スキルを使うと、こうした作業を適切に実行する方法を Copilot が理解できます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果の形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、`quality-checks` スキルを使って単体テスト、lint、型チェックを実行します。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェントスキルには、再利用可能なタスクの指示、実行可能なスクリプト、補助的なリソースをまとめます。基本的には、スキルと同じ名前のフォルダーに `SKILL.md` という Markdown ファイルを配置します。Markdown ファイルには、スキルの内容を定義する `name` と `description` を含むフロントマター、機能の概要、呼び出すタイミングのガイダンスを記述します。フォルダーには、スキルから呼び出すスクリプトやほかのリソース用のサブフォルダーを含めることもできます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、既存のプロジェクトコマンドを使うため、`SKILL.md` のみで構成されています。

スキルは、プロジェクトの `.github/skills` フォルダーに置いて、チームで共有・再利用できるリポジトリ資産にできます。または、ユーザーのスキルフォルダー `~/.copilot/skills` に置くこともできます。

## スキルを確認する

Tailspin Toys チームが単体テスト、lint、型チェックの実行に使う `quality-checks` スキルを確認しましょう。

1. Codespace に戻ります。Codespaces のエディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭の `name` と `description` を読みます。description によって、Copilot がスキルを呼び出すタイミングを判断しやすくなります。
3. 指示を読み、テストと lint の手順を Copilot にどのように案内しているか確認します。
4. **Results output formatting** セクションがまだないことを確認します。

## 変更前にスキルを実行する

スキルは、Copilot CLI から直接、または自然言語を使って呼び出せます。3 種類のチェックを実行するよう Copilot に依頼します。

1. Interactive モードでフィルター機能の会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に出力されるレポートを確認します。

## レポートをカスタマイズする

何を実行したか、成功したか、ツールが実際に何を報告したかが分かるレポートに改善しましょう。スキルを更新します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの最後に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更を加えたので、スキルがどのように動作するか確認しましょう。Copilot CLI は会話を再起動せずに、編集されたスキルを再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどとまったく同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に出力されるレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェントスキルをカスタマイズして使いました。このレッスンでは、次のことを行いました。

- 単体テスト、lint、型チェックを実行する `quality-checks` スキルを確認しました。
- 結果の形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

フィルター機能の PR には、この変更も含まれます。次は[Playwright MCP サーバー][next-lesson]を使って、Copilot がサイトを直接操作できるようにしましょう。

## スキルの例

次のコミュニティの例は参考資料です。追加のタスクではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とバンドルされたエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
