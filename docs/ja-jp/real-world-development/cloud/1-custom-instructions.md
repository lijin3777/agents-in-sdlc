---
title: "演習 1 - カスタム指示 (クラウドエージェント)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot クラウドエージェント →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストにアクセスできるようにします。[指示ファイル][instruction-files]を使うと、Copilot にタスクの内容だけでなく、実行方法も伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント規約が Copilot に伝わる方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい規約を追加します。

> [!NOTE]
> VS Code や CLI の実習環境とは異なり、この演習では変更前後を比較するプロンプトを実行しません。Copilot クラウドエージェントは GitHub issue に対して非同期に作業するため、リアルタイムで影響を並べて示すのが難しいためです。後の演習でクラウドエージェントが作成するプルリクエストをレビューするときに、指示ファイルの効果を確認できます。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。具体的には、次のとおりです。

- データレイヤーには、必ず単体テストを用意します。
- UI はダークモードにし、現代的な見た目にします。
- コードのドキュメントとして TSDoc のドキュメントコメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使って、Copilot が示されたプラクティスに沿ってタスクを実行するために必要な情報を提供します。

### カスタム指示

カスタム指示を使うと、コンテキストや希望を Copilot に伝え、コーディングスタイルや要件を理解しやすくできます。関連性の高い提案やコードスニペットを得るために、コーディング規約、ライブラリ、好みのコメントの書き方などを指定できます。リポジトリ全体に適用する指示も、特定の種類のファイルに適用するタスクレベルの指示も作成できます。

指示ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md`: リポジトリに関する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。Copilot に送るチャットや CLI のほとんどのリクエストに関係する、プロジェクトレベルの情報を記述します。技術スタック、開発中のものの概要、ベストプラクティス、全体に適用するガイダンスなどが該当します。
- `.github/instructions/*.instructions.md`: 特定のタスクやファイルの種類に合わせて作成できます。TypeScript や Astro などの言語、UI コンポーネントの作成や新しい単体テストの追加といったタスクのガイドラインを記述できます。

> [!NOTE]
> IDE で作業する場合、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot クラウドエージェントは、コード生成時にリポジトリレベルの指示と `applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は、AGENTS.md や CLAUDE.md など、ほかの規格を使った指示ファイルにも対応しています。[詳細][custom-instructions-support]をご覧ください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成方法を詳しく説明することは、このワークショップの範囲外です。ただし、サンプルプロジェクトには参考になる例があります。基本的には、次のようにします。

- `copilot-instructions.md` には、開発中のものの説明、プロジェクトの構造、全体に適用するコーディング規約など、プロジェクトレベルのガイダンスを記述します。
- `*.instructions.md` ファイルには、ファイルの種類 (単体テスト、Astro コンポーネント、データレイヤー) や特定のタスクに関する指示を記述します。
- 自然な言葉を使い、明確なガイダンスと、コードのよい例・悪い例を示します。

AI の使い方に唯一の正解がないように、指示ファイルの作成方法にも唯一の正解はありません。プロジェクトに合う方法は、試行錯誤を通じて見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分に整備された指示ファイルのセットを用意してください。このプロジェクトのファイルを確認すると、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク用の指示があることが分かります。
>
> Copilot に指示ファイルを作成してもらうこともできます。操作方法は利用する環境によって異なります (たとえば、VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。このワークショップでは、該当するレッスンで操作方法を紹介します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェントなどを集めたリポジトリ [awesome-copilot][awesome-copilot] をご覧ください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリには、コアとなる `copilot-instructions.md` と、さまざまなタスク用の `*.instructions.md` ファイルが含まれています。エディターまたは GitHub の Web UI で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** では、入れ子になった **GitHub Actions Workflows** のガイダンスにも注目します。これらの指示は、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて、ファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、リポジトリルートを基準とした glob を指定します。たとえば、`**/*.test.ts` に一致する TypeScript のテストファイルが対象になります。
5. このプロジェクトの単体テスト作成に関する指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾を確認します。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな指示を再利用できる小さなファイルに分け、Copilot がコードを作成する際に参考にできる例を示せます。ここにあるパスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング規約が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で、TSDoc のドキュメントコメントとファイルヘッダーに関する規則を追加します。

## リポジトリに新しい規約を追加する

次の手順では、ここで行う唯一の編集として、コード内に TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーを記述するプロジェクト全体の規則を追加します。後の演習で issue を割り当てると、クラウドエージェントはこの規則に従います。

編集する前に、作業用のブランチを作成します。クラウドエージェントは issue の対象ブランチにある指示を読み込みますが、変更をきれいにコミットするため、ここではブランチを使います。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前に説明したように、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。コードの提案を改善するために、リポジトリのコーディング規約を文書化しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング規約が記載されていますが、コード内のドキュメントに関する規則はまだありません。そのため、生成されたヘルパーにはドキュメントコメントがありませんでした。
3. 既存の規約のすぐ下に次の Markdown の行を追加し、Copilot がファイルヘッダーと TSDoc のドキュメントコメントを追加するよう指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで見たように、リポジトリレベルの指示 (`.github/copilot-instructions.md`) は全体に適用するガイダンスに使い、`*.instructions.md` ファイルは特定の言語、ファイルの種類、タスクに使います。今回追加したドキュメントコメントの規則など、プロジェクト全体の規約には、リポジトリレベルのファイルが適しています。

## 指示の更新をコミット、プッシュ、マージする

クラウドエージェントは、issue の対象ブランチにある指示ファイルを読み込みます。次の演習で issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。したがって、クラウドエージェントに指示を読み込ませるには、変更を `main` にマージする必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開き、マージします。次の演習でクラウドエージェントが issue に対応する際、この指示が読み込まれます。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチを作成せず、変更を `main` にコミットしてもかまいません。実際のプロジェクトと同じ手順を体験できるように、ここではブランチを使用しています。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを得る方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の新しい規約を追加しました。この実習環境の残りの演習で、クラウドエージェントが作成するプルリクエストを通じて規約の効果を確認できます。

次は[初めての issue を Copilot クラウドエージェントに割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のよりよいカスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
