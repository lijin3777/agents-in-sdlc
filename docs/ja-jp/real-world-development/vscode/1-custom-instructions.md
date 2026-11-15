---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストにアクセスできるようにします。Copilot と特定のコンテキストを共有する方法はいくつかあります。特に重要なのが、コード生成に関するガイダンスを提供する[指示ファイル][instruction-files]です。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント規約が Copilot に伝わる方法を確認します。
- 現在の指示を使って、フィルター機能の最初のデータスライス (発行元ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい規約を追加します。
- 同じプロンプトをもう一度実行し、生成コードに新しい規約が反映されることを確認します。
- 指示の更新とフィルター機能のスライスを `main` にコミットし、次の演習で Copilot が更新されたガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定した規約の一部に従わないことがあります。Copilot の動作は確率的です。この演習では、更新後の指示によって動作にどのような*傾向*が生じるかを確認します。生成結果が一字一句一致することを目指すものではありません。

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
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾を確認します。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな指示を再利用できる小さなファイルに分け、Copilot がコードを作成する際に参考にできる例を示せます (ここにあるパスはリポジトリのルートではなく、指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング規約が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で、TSDoc のドキュメントコメントとファイルヘッダーに関する規則を追加します。

## 変更用のブランチを作成する

ここではベストプラクティスに従い、作業用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する*前*に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、必要な箇所で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。後で指示を更新し、同じプロンプトを再送信して結果の違いを確認します。

1. 前の演習で開いたエディタータブをすべて閉じ、Copilot が必要なコンテキストだけを参照するようにします。
2. `src/lib/publishers.ts` を開きます。Copilot にヘルパーの作成場所を示します。
3. Chat ビューのエージェントドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調査してコードを更新します。ヘルパーファイルとテストの両方が変更されることがあります。
6. 提案されたヘルパーは、最初の引数に `db` クライアントを受け取り、型付きの発行元の配列を返す関数です。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約に従っています。
7. 提案されたコードに TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する可能性があります。その場合も問題ありません。指示を更新すると一貫性が高まることが、この演習の目的です。

## リポジトリに新しい規約を追加する

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

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` をもう一度選択し、Copilot が正しいファイルを対象にするようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが追加されていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot を導いて新しいプロジェクト規約に従わせ、次の演習で使用する実際のコードに適用できました。

## 指示とブランチをコミットする

指示ファイルはリポジトリ内のほかの資産と同様に、バージョン管理の対象です。指示をリポジトリにコミットし、ブランチをプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次を実行し、指示の更新とヘルパーの変更が含まれていることを確認します。

   ```bash
   git status
   ```

3. ターミナルで、指示の更新と Copilot がヘルパーの基盤のために変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加ファイルが表示された場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルからブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント規約の issue に関連付けてプルリクエストを作成します。手動で行うこともできますが、プロジェクトに接続済みの GitHub ツールを使って Copilot に依頼できます。issue を見つけ、issue をクローズする PR を作成してから、マージしてもらいましょう。

> [!NOTE]
> この演習では、Copilot に GitHub 上の操作を初めて依頼します。プロジェクトテンプレートでは、GitHub ツールがワークスペースにあらかじめ接続されています。Copilot が初めてツールを使うとき、VS Code で GitHub へのサインインを求められることがあります。プロンプトに従って認証してください。後の演習で、この接続方法である Model Context Protocol について学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択で **Agent** が選択されていることを確認します。
3. 指示ファイルの更新と、新しい発行元ヘルパーの基盤の両方を説明する PR を現在のブランチから作成し、関連する issue を見つけるよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を見つけて PR を作成します。
5. Copilot が GitHub 操作の **Allow** を求めたら、コマンドを確認して、適切な場合に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトを使って PR をマージし、ブランチを `main` に戻すよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub 操作とシェルコマンドの実行について Copilot が **Allow** を求めたら、コマンドを確認して、適切な場合に **Allow** を選択します。

GitHub Copilot のサポートを受けて、プルリクエストを作成してマージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを得る方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の指示を使ってコード生成のプロンプトを送り、Copilot が作成した内容を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい規約を追加しました。
- 同じプロンプトを再度実行し、提案されたコードに新しい規約が反映されることを確認しました。
- 指示の更新とヘルパーの基盤を `main` にコミットし、次の演習で使えるようにしました。

次は[agent mode][next-lesson]を使い、Copilot にコードベース全体にわたる新機能を追加してもらいましょう。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のよりよいカスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
