---
title: "演習 2 - GitHub Copilot クラウドエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残ったままの機能リクエストなどがあります。GitHub Copilot のクラウドエージェントは、コードの更新や機能の追加などのタスクを自律的に実行できます。エージェントが作業を終えると、人間の開発者がレビューできるドラフト PR を作成します。これにより、面倒な作業を任せて開発を加速し、より重要な課題に集中できます。

Copilot クラウドエージェントを使って、次のことを行います。

- コード生成の環境をカスタマイズする。
- 操作が安全に行われるようにする。
- issue の範囲を明確に定めることの重要性を理解する。
- issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys では、解決したい技術的負債があります。最初のサイトを作成した委託先がドキュメントを適切に整備しなかったため、コードにはドキュメントがほとんどありません。最初の対処として、アプリケーションのエクスポートされたすべての関数に TSDoc のドキュメントコメントを追加したいと考えています。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリーのほかのゲームを関連ゲームとして表示し、閲覧を続けられるようにします。まだ完成度の高い実装は必要ありません。ユーザー体験 (UX) の受け入れテストに利用できるものを求めています。この作業は現在ブロッカーですが、もっと優先度の高い issue がほかにあります。

このようなタスクは後回しにされやすく、Copilot クラウドエージェントに割り当てるのに適しています。Copilot クラウドエージェントは非同期に作業するため、開発者は別のタスクに集中できます。後から戻って作業をレビューし、期待どおりの内容になっているか確認できます。

## GitHub Copilot クラウドエージェントについて

[GitHub Copilot クラウドエージェント](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。人間の開発者に仕事を依頼する場合と同様に、[GitHub issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)など、複数の方法で依頼できます。割り当てると、Copilot は進行状況を追跡するドラフトのプルリクエストを作成し、環境をセットアップして、タスクに取りかかります。作業中でも完了後でも、Copilot のセッションを詳しく確認できます。提案された解決策のレビュー準備ができると、Copilot はプルリクエストであなたに通知します。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。単純に見える操作でも、詳しく見ると複雑なことがあります。[Copilot クラウドエージェントに依頼するタスクの範囲][cloud-agent-best-practices]を明確にしてください。AI ペアプログラマーとして、段階的に作業し、学び、試し、必要に応じて調整しましょう。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

これまでの演習では、カスタム指示と Copilot の動作を導く方法を学びました。カスタム指示の演習をすでに行った場合は、`.github/copilot-instructions.md` が機能することを確認しています。まだの場合は、ここで簡単に確認しましょう。Copilot クラウドエージェントに作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認すると、後でその効果を把握できます。

GitHub の Web UI または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript の規約と TSDoc のドキュメントコメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` フロントマターが `**/*.test.ts` ファイルに指示を適用することを確認します。

次のセクションで **Code lacks documentation** issue をクラウドエージェントに割り当てるとき、作成されるプルリクエストに TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーが反映されているか確認します。後の演習で PR をレビューするときに、改めて取り上げます。

## Copilot クラウドエージェントの開発環境をセットアップする

コードを作成する際は、誰が作業する場合でも、通常は環境の準備とセットアップスクリプトの実行が必要です。ソフトウェアエンジニアのように作業する Copilot にタスクを割り当てる場合も同様です。

クラウドエージェントは GitHub Actions を使って作業環境を構成します。`.github/workflows/copilot-setup-steps.yml` ファイルに[専用のセットアップワークフロー][setup-workflow]を作成すると、作業開始前の環境をカスタマイズし、必要な開発ツールや依存関係を利用できるようにできます。このラボでは、学習をスムーズに進めるため、すでに構成されています。単一の Astro アプリであるこのプロジェクトに必要な Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションと初期データ投入済みの SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 手順に必要な最小限の権限を設定します。Copilot は操作用の独自のトークンを受け取ります。
    permissions:
      # セットアップ手順の一部としてリポジトリをクローンする場合 (依存関係のインストールなど) は、`contents: read` 権限が必要です。セットアップ手順でリポジトリをクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (現在、アプリ全体は Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ローカルの SQLite データベースをマイグレーションして初期データを投入し、ビルドとテストで使えるようにします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同様ですが、次の点に注目してください。

- `copilot-setup-steps` という単一のジョブが含まれています。このジョブは、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーにより、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot が実行するまで待たずに、ワークフローが正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、古い情報しかない、またはドキュメントがまったくないプロジェクトも少なくありません。こうした技術的負債が放置されると、生産性が低下し、コードベースの保守や新しい開発者の受け入れが難しくなります。Copilot はドキュメントの作成を得意としているため、これはクラウドエージェントに依頼するのに適した issue です。バックグラウンドで必要なドキュメントを作成してもらい、後の演習で成果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てのダイアログを開きます。

  ![issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような名前になっています。一覧に表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が todo リストを作成したことを確認します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで、数分かかることがあります。Copilot は環境を作成し (前述のワークフローを実行し)、プロジェクトを分析して、問題に対処する最適な方法を検討しています。

13. リストと、実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッションビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新されたインジケーターを表示するには、ブラウザーを更新する必要がある場合があります。

16. ライブセッションをスクロールして Copilot の作業を確認します。コードを調査して状態を把握する様子、計画を考えて判断するために一時停止する様子、コードを作成する様子などを確認できます。

作業には数分かかることがあります。Copilot クラウドエージェントの主な目的の 1 つは、非同期にタスクを実行し、開発者が別の作業に集中できるようにすることです。この機能を活用するため、別のタスクも Copilot クラウドエージェントに割り当て、その間にアプリケーションに機能を追加するコードを書きましょう。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot クラウドエージェントの大きな利点の 1 つは、作業を分けられることです。開発者が一方のタスクに取り組みながら、エージェントが別のタスクを進められます。デザインチームが求める関連ゲーム機能は時間のかからない作業かもしれませんが、その時間をほかのタスクに使えます。Copilot クラウドエージェントに割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てのダイアログを開きます。

  ![issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことが分かります。

![issue の作業中であることを示す目の絵文字を使った Copilot の表示](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取りかかりました。Copilot クラウドエージェントはソフトウェアエンジニアと同じように作業するため、積極的に監視する必要はありません。完了後にレビューすればよいので、次はカスタムエージェントの作成と利用に進みましょう。

## まとめと次のステップ

このレッスンでは、[GitHub Copilot クラウドエージェント][copilot-agents]について学びました。クラウドエージェントに issue を割り当て、非同期に作業してもらえます。技術的負債への対処、新機能の作成、別のフレームワークへのコード移行などに利用できます。

次の概念を確認しました。

- コード生成の環境をカスタマイズする。
- 操作が安全に行われるようにする。
- issue の範囲を明確に定めることの重要性を理解する。
- issue を Copilot に割り当てる。

クラウドエージェントがバックグラウンドで作業している間に、カスタムエージェントの作成と利用に進みましょう。[Copilot クラウドエージェントは MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで学んだカスタム指示にも対応しています。

## リソース

- [Copilot クラウドエージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot クラウドエージェントのセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
