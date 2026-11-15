---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または構成し、ブラウザーからフィルター機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに説明したように、コードを書くことだけが開発ではありません。データや外部サービスを扱い、Copilot が利用できる自動化機能を追加することも必要です。そこで役立つのが MCP サーバーです。MCP サーバーを使うと、Copilot は CLI に組み込まれた機能だけでなく、さらに多くのツールやサービスを利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように利用するかを理解します。
- Playwright MCP サーバーがまだ利用できない場合は追加します。
- エージェントにブラウザーを操作させ、フィルター機能を試します。

## シナリオ

単体テストやエンドツーエンドテストは重要ですが、UI の更新を検証するには、実際に UI を操作する必要があります。Copilot にユーザーと同じように開発中の Web サイトを使わせることで、変更が期待どおりに動作する確信を高め、変更作業をさらに自動化できます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] を使うと、AI エージェントは外部のツールやサービスと通信できます。MCP によって、AI エージェントは外部のツールやサービスとリアルタイムにやり取りし、最新情報へのアクセスや代理操作が可能になります。

エージェントは、外部ツールやサービスとの橋渡しをする MCP サーバーを通じて、ツールやリソースにアクセスします。MCP サーバーごとに、AI エージェントが利用できるツールとリソースのセットが異なります。

よく利用されている MCP サーバーには、次のようなものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステム内の MCP サーバーを見つけやすくし、貢献を促すための [MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクトのほかの依存関係と同様に扱ってください。使用前にソースコードを確認し、公開元を確かめ、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、追加して Copilot がユーザーと同じように Web サイトを操作できるようにします。

> [!NOTE]
> Copilot CLI ですでに Playwright MCP を構成している場合は、セットアップの手順を省略して次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類が **STDIO** であることを確認し、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次のコマンドを貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright を使って Copilot に機能を試させる

これまでは、機能が期待どおりに動作することを手動で確認しました。次は、Copilot で利用できる Playwright MCP サーバーを使って同じ確認を行いましょう。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に指示します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常は、MCP サーバーの利用を明示しなくても、Copilot が自動的に判断します。ここでは使うツールが分かっているので、Copilot に伝えておくと、より一貫した結果を得られ、トークンも節約できます。

2. Copilot がブラウザーで機能の動作を確認するため、さまざまな手順を実行する様子を確認します。
3. 結果の報告を読み、すべてが期待どおりに動作したか確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作し、作業後にサーバーを停止して、結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を試しました。このレッスンでは、次のことを行いました。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように利用するかを学びました。
- Playwright MCP サーバーがまだ利用できない場合に追加しました。
- エージェントにブラウザーを操作させ、フィルター機能を試しました。

次は[QA カスタムエージェントを作成し][next-lesson]、スキルとブラウザーツールを専門的な役割として組み合わせます。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
