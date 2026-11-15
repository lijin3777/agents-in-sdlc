---
title: "演習 2 - Copilot agent mode で新機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの単純な更新でも、通常は複数のファイルを変更し、テストを実行するなどの作業が必要です。開発者は、必要なファイルを探し、変更を加え、テストを実行し、問題をデバッグし、変更が漏れたファイルを特定して、さらに更新するという作業を行います。ほかにも必要な作業は次々と出てきます。

このようなときに役立つのが Copilot agent mode です。

Copilot agent mode は、IDE 内でより自律的に作業できるよう設計されています。開発者のように既存のプロジェクト構造を調査し、必要な更新を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。agent mode を使って、Web サイトに新機能を追加する方法を確認しましょう。

> [!NOTE]
> agent mode とクラウドエージェントは名前が似ていますが、異なる用途を想定しています。agent mode は IDE 内で動作し、短いフィードバックサイクルで対話しながら作業できます。クラウドエージェントはチームの一員のように非同期で作業し、issue とプルリクエストを通じてやり取りします。

この演習では、次の方法を学びます。

- Copilot agent mode でプロジェクトを調査し、関連ファイルを特定して、複数の変更を連携して行う。
- GitHub Copilot agent mode で UI とデータレイヤーにわたる新機能を実装する。
- Copilot agent mode が生成した変更とテストを、コードベースにマージする前にレビューする。

## シナリオ

ゲームの一覧が増えてきたため、ユーザーがカテゴリーや発行元で絞り込めるようにします。前の演習では発行元ヘルパーを追加しました。次は Copilot agent mode を使って、データレイヤー、UI、テストの残りの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを見て、現在の機能を確認しましょう。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示できます。単一の Astro アプリとして、ビルド時に静的 HTML のページを生成します。ページは Drizzle ORM を通じてフロントマターからローカル SQLite データベースを直接クエリします。別のバックエンド API やクライアントサイドの UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、必要なインタラクティブ機能は標準の DOM API を使う小さな Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるように、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。次の手順で GitHub Codespace から実行できます。

1. Codespace に戻ります。前の演習の後、`main` に戻っているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. フィルター機能用の新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を示すバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで Astro サーバーのアドレス `http://localhost:4321` を選択するとき、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押したままにして Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。これは Web サーバーをホストしている Codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトを起動したら、その機能を確認できます。主な機能は次のとおりです。

- **Home Page**: タイトル、画像、説明を含むボードゲームの一覧が表示されます。
- **Game Details Page**: ゲームを選択すると、タイトル、説明、発行元、カテゴリーなどの情報が表示される詳細ページが開きます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、必要な箇所で説明します。

Web サイトの初期実装は機能しますが、新しい機能を追加して改善したいと考えています。まずバックログを確認しましょう。テンプレートからプロジェクトをセットアップしたときに、issue のバックログも自動的に作成されています。GitHub Copilot に項目を表示してもらいます。

1. Chat ビューのエージェントドロップダウンから **Agent** を選択します。**Agent** は、ファイル間の変更を自律的に計画・実装し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント選択を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者は、特定のモデルを推奨しているわけではありません。このワークショップの作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案されるコードにある程度一貫性を持たせ、使いやすい体験にすることが目的です。ただし、LLM は確率的に動作するため、ワークショップの記載とは異なる提案が出る場合があります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信して、バックログの issue について質問します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot が、ユーザーに最も役立つと考えた項目を優先順位付けしたことを確認してください。

## 指示ファイルを確認する

エージェントにコードを生成してもらう前に、Copilot に作業のコンテキストを提供する指示ファイルを確認しましょう。Web サイトへの機能追加方法に関するコンテキストが記載された [user interface (UI)][ui-instructions] ファイルを使います。

1. Codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能を追加するときの全体的なガイダンスを確認します。次の内容が含まれています。
   - アーキテクチャの概要
   - コンポーネントの設計、テスト可能性、アクセシビリティに関する原則
   - Astro、Tailwind CSS、Drizzle など、さまざまなファイル形式に関する指示ファイルへのリンク

> [!TIP]
> 指示ファイルからは、別の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの場所からの相対パスです。これにより、指示の再利用や、複雑な内容の分割、例やテンプレートの提示がしやすくなります。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の 3 つの変更が必要です。

- データレイヤー (`src/lib/`) に残りのフィルターロジックを追加または調整する。
- フィルターの動作に関するテストを追加または更新する。
- ゲーム一覧ページにフィルター UI を追加する。

さらに、コードベースにマージする前に、テストを実行して成功させる必要があります。Copilot agent mode にこれらのタスクを実行してもらい、機能を追加しましょう。

1. 現在の Copilot の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログの関連 issue に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調査し、必要なファイルを探す様子を確認します。データレイヤーのヘルパー、UI、テストが見つかり、ファイルの変更とテストの実行が始まります。

   ![Copilot がプロジェクトのファイルを調査している様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot は、プロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクとコードベースの複雑さによっては、数分かかることがあります。作業中、コードエディターに **Keep** と **Undo** のボタンが表示される場合があります。Copilot が完了すると、すべての変更に対して **Keep** または **Undo** を選べるようになるため、作業中に選択する必要はありません。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat ペインでテスト実行の確認を求めるダイアログのスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はタスクが完了し、エラーがないと判断するまで、コード生成とテストを繰り返します。

   ![Copilot agent mode を使用した Chat セッションの完了状態を示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新しい機能を試します。
11. すべてが動作し、コードのレビューも完了したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot agent mode を使って Tailspin Toys の Web サイトに新機能を追加する方法を学びました。この演習では、次のことを確認しました。

- GitHub Copilot agent mode で UI とデータレイヤーにわたる新機能を実装できること。
- Copilot agent mode でプロジェクトを調査し、関連ファイルを特定して、複数の変更を連携して行えること。
- Copilot agent mode が生成した変更とテストを、コードベースにマージする前にレビューすること。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成しましょう。

### ボーナスの演習 — ページ分割を実装する

ゲーム一覧が増えると、ページ分割が必要になります。この演習で学んだことを使い、Copilot にページ分割を実装するよう依頼します。コードを作成する際は、次の点を検討してください。

- 既存の指示ファイルを使うなど、既存のベストプラクティスに従う。
- ページサイズをユーザーが選べるようにするか、固定にするかなど、ページ分割の方法を検討する。
- 希望どおりの実装に必要なガイダンスを Copilot に伝えるプロンプトを作成する。
- GitHub Copilot に変更を依頼してコンテキストを提供し、何度か反復する必要がある場合があります。Copilot との作業では通常の進め方です。

## リソース

- [Copilot の ask、edit、agent mode: それぞれの役割と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
