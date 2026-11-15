---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、使い方を確認して、あらかじめ用意されたフィルター機能の issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作する強力なエージェント型コーディングアシスタントです。コードベースを調査し、コードを生成し、コマンドを実行し、外部ツールとやり取りできます。すべてコマンドラインから行えるため、タスクを任せたり、変更を依頼したりしながら、集中して作業できます。最初のステップはツールのインストールです。すでに使い慣れたツールでインストールできます。

このレッスンでは、次のことを行います。

- npm を使って GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、簡単な会話を試します。
- 組み込みの GitHub MCP サーバーを使って、フィルター機能の issue を見つけます。

## シナリオ

チームは、増え続けるバックログに対応するために AI エージェントを使い始めています。Copilot CLI はその機能をターミナルに持ち込みます。ターミナルは、多くの開発者が日常的に作業する場所です。このレッスンでは、インストールと認証を済ませ、ワークショップの残りで利用できる状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew でインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされていて、バージョン要件を満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

Copilot CLI を初めて起動すると、GitHub アカウントでの認証を求められます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 求められた場合は、デバイスコードの手順に従って認証し、Copilot CLI を承認します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択して信頼します。

> [!NOTE]
> Codespace では、GitHub セッションを通じてすでに認証されていることがあります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めてください。

## 使い方を確認する

通常のシェルプロンプトで入力したコマンドは、Codespace で直接実行されます。Copilot CLI を起動すると、自然言語はエージェントへの入力に、スラッシュコマンドは会話の操作に使われます。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押します。続けてもう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力して、インストール済みのバージョンで利用できるコマンドを確認し、<kbd>Esc</kbd> でヘルプ画面を閉じます。
3. Copilot に簡単な質問をして、すべてが動作することを確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. 回答を読み、Copilot が回答前にリポジトリを調査していることを確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用できることを確認します。
6. Copilot にフィルター機能の issue を探してもらいます。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開いて issue を読みます。最初の簡単な変更を完了した後に、この issue を使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトされているブランチを使い、自動的には worktree を作成しません。変更を始める前に、機能ブランチを作成します。

## ワークショップ用のショートカットを使う

Copilot CLI は通常、設定済みの権限の範囲外でツールを使う前に確認を求めます。このワークショップでは、ユーザーが承認したショートカットである `--yolo` を使って再起動します。Codespace 内では承認のプロンプトがなくなるため、演習に集中できます。

> [!CAUTION]
> `--yolo` は、すべての自動権限 (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) を有効にします。Codespace や VM などの隔離された環境でのみ使い、日常の開発で既定の設定にしないでください。詳細については[ツールの使用を許可または禁止する][allow-all-warning]を参照してください。

後のレッスンで issue やプルリクエストを扱えるよう、このワークショップでは `--enable-all-github-mcp-tools` を使って、GitHub MCP の読み取り・書き込みツールを有効にします。Codespace によってローカルコンピューターへのアクセスは制限されますが、認証済みの GitHub リソースには引き続きアクセスできます。変更を公開またはマージする前にレビューしてください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについてもう 1 つ簡単な質問をして、会話が動作することを確認し、`/exit` で終了します。

Copilot は会話を自動的に保存します。後で指示を変更したりエージェントを追加したりした後に、`copilot --resume` を使って同じ機能の会話とブランチに戻ります。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- npm を使って GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼して、簡単な会話を試しました。
- 組み込みの GitHub MCP サーバーを使って、フィルター機能の issue を見つけました。

次は[最初の対象を絞った変更を始め][next-lesson]、Copilot CLI を使ってゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
