---
title: "レッスン 1 - GitHub Copilot app をインストールする"
description: "GitHub Copilot app をインストールし、テンプレートから作成したリポジトリを接続して、ワークスペースを確認し、クイックチャットを試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** は、エージェント主導の開発に使うデスクトップアプリケーションです。GitHub Copilot CLI を基盤とし、GitHub とネイティブに統合されているため、リポジトリ、ブランチ、CI パイプラインをすぐに利用できます。複数のエージェントを個別に作業させ、それぞれ分離されたワークスペースで並行して作業を指示し、繰り返し作業を自動化するワークフロー向けに設計されています。Node.js のインストールとプロジェクトのコピーができたので、次はアプリをインストールしてリポジトリを接続します。

このレッスンでは、次のことを行います。

- GitHub Copilot app をインストールしてサインインします。
- GitHub リポジトリからプロジェクトをアプリに追加します。
- テンプレートによって用意されたバックログなど、ワークスペースの構成を確認します。
- クイックチャットを試し、アプリ自体について学びます。

## シナリオ

チームは、増え続けるバックログに対応するために AI エージェントを導入しています。Copilot app では、issue の選択、エージェントの実行、変更のレビュー、プルリクエストのマージを一か所から指示できます。このレッスンでは、アプリをインストールして接続し、プロジェクトについての会話を始められるようにします。

> [!NOTE]
> 対象となる Copilot プランが必要です。Copilot Free、Copilot Student、または有料プラン (Pro、Pro+、Business、Enterprise) を利用してください。Copilot Business または Copilot Enterprise を使用している場合、管理者が **GitHub Copilot app** ポリシーを有効にする必要があります。

## GitHub Copilot app をインストールして構成する

GitHub Copilot app を使用するには、まずアプリをインストールします。Windows、macOS、Linux 向けのバージョンが用意されています。アプリをインストールして認証し、Tailspin Toys リポジトリを追加しましょう。

1. ブラウザーで [GitHub Copilot app のランディングページ][download-app]を開きます。
2. 使用しているプラットフォーム向けのアプリをダウンロードし、ランディングページの手順に従ってインストールします。
3. インストール後、アプリを開きます。
4. **Sign in to GitHub** を選択し、画面の指示に従って GitHub.com または GHE.com のアカウントでサインインします。データ所在地に対応した GitHub Enterprise Cloud を使用している場合は、**GitHub Enterprise Cloud** を選択し、求められたら `*.ghe.com` のホスト名を入力します。

   GitHub Enterprise Server を使用している場合は、まずクラウドアカウントでオンボーディングを完了します。その後、**Settings** を開き、**Accounts**、**Add account**、**GitHub Enterprise Server** の順に選択します。[Enterprise Server のサインインガイド][ghes-sign-in]に従い、ブラウザーでアプリを承認します。

5. オンボーディングでリポジトリの選択画面が表示された場合は、テンプレートから作成した Tailspin Toys リポジトリを選択します。リポジトリの選択を省略して、オンボーディングの後に追加することもできます。
6. テーマの選択を求められたらテーマを選び、オンボーディングを完了します。
7. サイドバーの **Sessions** でリポジトリを確認します。すでに一覧に表示されている場合は、再度追加する必要はありません。
8. リポジトリがまだ接続されていない場合は、**Sessions** の横にある **+** を選択して新しいセッションを作成し、**Add GitHub repository** を選択します。前提条件のレッスンで記録した所有者とリポジトリ名を入力します。
9. リポジトリがサイドバーに表示されることを確認します。ここで使うレイアウトでは、`tailspin-toys` などのリポジトリフォルダーが **Sessions** の下にグループ化され、各リポジトリの下に個々のセッションが表示されます。

## ワークスペースの構成を確認する

プロジェクトを接続したら、アプリの使い方を確認しましょう。サイドバーには、主に次の領域があります。

- **New** - ここから Copilot との新しいチャットセッションを開始できます。
- **Pull requests** - アプリの GitHub ネイティブ統合を通じて表示されるプルリクエストです。アプリを離れずに、プルリクエストの一覧表示、絞り込み、CI 状態の確認、レビューを行えます。
- **Issues** - 同じように表示される issue です。issue を表示して絞り込み、issue からセッションを開始できます。
- **Automations** — スケジュールまたはオンデマンドで実行する、保存済みのエージェントタスクです。やることリストの管理、定期的なプロジェクトの保守、その他の面倒な作業を任せるのに役立ちます。振り返りでは次のステップとして紹介します。別のワークショップ演習ではありません。
- **Customize** - MCP サーバー、プラグイン、スキルなどのコンポーネントを追加して、Copilot app の機能を拡張します。Playwright MCP の設定に使用します。
- **Chats** — 独自のブランチやワークスペースを必要としない、質問やブレインストーミング向けの軽い会話です。最初のチャットを始めると、このセクションが表示されます。表示されない場合は、**Sessions** の横にある **+** を選択して、最初のチャットを開始します。
- **Sessions** — リポジトリのフォルダーと、その中のエージェントセッションをまとめます。`tailspin-toys` などのリポジトリ名を選択するとリポジトリのビューが開き、その下にあるセッションを選択すると会話に戻ります。各セッションは独立したワークスペースで実行されるため、変更が競合することなく複数のセッションを同時に実行できます。星評価を追加する際に、最初のセッションを開始します。

ワークショップを進めながら、ワークスペースを確認していきます。

> [!TIP]
> 迷ったときは Copilot に質問しましょう。操作方法が分からない場合や、何かが可能かどうか知りたい場合は、Copilot に尋ねると案内してくれます。

### 用意されたバックログを確認する

バックログのないプロジェクトはほとんどなく、Tailspin Toys も例外ではありません。テンプレートの作成時に用意されたバックログを確認しましょう。

1. サイドバーの **Sessions** の下で、個々のセッションではなく Tailspin Toys のリポジトリフォルダーを選択します。次に、リポジトリの上部ナビゲーションで **Issues** を選択します。
2. issue 番号を決めつけず、次のタイトルを探します。

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. issue を選択して詳細を読みます。各 issue はエージェントセッションの開始点にもなります。簡単な最初の変更を完了した後、フィルター機能の issue から作業を始めます。

> [!NOTE]
> **Issues** と **Pull requests** の一覧は自動的に絞り込まれ、Copilot app に追加したリポジトリの項目だけが表示されます。ほかのリポジトリの作業項目を表示するには、そのリポジトリもアプリに追加してください。

## クイックチャットを試す

アプリに慣れるには、アプリ自体について質問するのが効果的です。その用途には **quick chat** が適しています。Quick chats では、ブランチや worktree を作成せずに質問したりブレインストーミングしたりできます。セッションを必要としない、その場限りの簡単な質問に最適です。

1. サイドバーで **Chats** の横にある **+** を選択し、新しいチャットを開きます。**Chats** がまだ表示されていない場合は、**Sessions** の横にある **+** を選択して最初のチャットを開始します。その後、**Chats** が表示されます。
2. アプリのセッションについて質問します。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 会話ビューで回答を読みます。各セッションが分離された独自の git worktree で実行されるため、変更が競合することなく複数のエージェントを並列に実行できます。会話は続けることも、新しいチャットをいつでも開始することもできます。

## まとめと次のステップ

GitHub Copilot app をインストールし、プロジェクトを接続して、ワークスペースを確認しました。次の方法を学びました。

- アプリをインストールして GitHub にサインインする。
- GitHub リポジトリからプロジェクトを追加する。
- ワークスペースの構成を確認し、**Issues** で用意されたバックログを見つける。
- クイックチャットで、その場限りの簡単な質問をする。

次は[最初のエージェントセッションを開始し][next-lesson]、ゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app の使用を開始する][getting-started]
- [GitHub Copilot app でエージェントセッションを操作する][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
[ghes-sign-in]: https://docs.github.com/enterprise-server@3.22/copilot/copilot-on-ghes/sign-in-on-copilot-app
