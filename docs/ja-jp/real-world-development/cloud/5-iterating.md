---
title: "演習 5: GitHub Copilot の作業を反復的に改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善と機能追加を目的とする複数のタスクで、GitHub Copilot と一緒に作業してきました。コードにドキュメントを追加し、デザインチームが試せる関連ゲーム機能を作成し、ハイコントラストモードとライトモードの切り替えなど、アクセシビリティ機能を実装するよう Copilot に依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って、作業を改善しましょう。

### シナリオ

これまでに何度も説明したように、生成 AI を導入しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業することが重要です。そこで、チームのレビューを有効にする前に、ドキュメントの作成、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した変更を確認します。

## セキュリティと GitHub Copilot クラウドエージェント

Copilot クラウドエージェントは、監視を受けずに非同期でタスクを実行するため、安全性を保つためのセキュリティ上の制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、コードの変更に使うブランチ**のみ**への書き込みアクセス権を持ちます。
- クラウドエージェントは GitHub Actions 内で動作し、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは、既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

GitHub Copilot クラウドエージェントが最初に作成した PR、つまりコードにドキュメントを追加するプルリクエスト (PR) から確認しましょう。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認するときに、ファイアウォールによって GitHub Copilot がブロックされているという警告が表示されることがあります。Copilot は MCP サーバーなどの外部リソースへのアクセスが既定で制限されているため、これは**想定どおり**です。必要であれば、[Copilot クラウドエージェントのファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com でリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** など、同様のタイトルが付いたプルリクエストを開きます。

> [!NOTE]
> Copilot がまだタスクを実行中の場合、プルリクエストには **[WIP]** が付いています。その場合は、Copilot が作業を完了するまで待ちます。数分かかることがあります。その間に休憩したり、これまでに学んだことを振り返ったりしてください。

4. プルリクエストの準備ができたら、**Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc のドキュメントコメントやその他のドキュメントが追加されたコードを確認します。具体的な変更内容は毎回異なります。

    変更を確認するときは、TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーに注目してください。これらは、演習 2 の最初に確認したカスタム指示ファイルから適用されます。

6. 更新を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. 承認待ちのワークフローがあることを示すインジケーターを確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローがキューに登録されたことが表示されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業する際は、一方的に変更を受け取るだけではありません。チームメンバーと同様に、プルリクエストやコードへのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認して、対応のために別のセッションを開始します。結果は確定的ではないため、必ずこう依頼すればよいという文面はありません。Copilot に更新を依頼する例を次に示します。

- 各コードファイルの先頭に、ファイルの役割を説明するコメントヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記の例またはコードベースに追加したいドキュメントについての別の提案を使って、**@copilot** をメンションしたコメントを投稿し、生成されたドキュメントの変更を依頼します。
2. **View Session** を選択して Copilot の作業を確認します。Copilot が更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

    ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択して変更をレビューします。

納得できるまで、Copilot との反復的な改善を続けてください。準備ができたら、ドラフトの PR をレビュー可能な状態に変更して、main ブランチにマージします。

![レビュー可能な状態に PR を変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue に対して、Copilot が作成した PR に戻りましょう。

1. GitHub.com でリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルが付いた PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
6. 承認待ちのワークフローがあることを示すインジケーターを確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック欄にワークフローがキューに登録されたことが表示されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストすることもできます。Codespace に移動し、ターミナルを開いて、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名、たとえば **copilot/fix-8** に置き換えてください)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。これまでと同様に、Copilot クラウドエージェントとの作業を反復して更新を依頼できます。たとえば、表示する関連ゲームの数を調整するよう依頼したり、コメントヘッダーと TSDoc のドキュメントコメントを追加するよう依頼したりできます (この作業は、カスタム指示を更新する**前**に割り当てたことを思い出してください)。**Conversation** タブに新しいコメントを追加すれば、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティのカスタムエージェントで実装したアクセシビリティ機能をレビューします。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 のミッションコントロールで依頼したライトモードの両方が含まれているはずです。

1. GitHub.com でリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだタスクを実行中の場合、プルリクエストには **[WIP]** が付いています。その場合は、Copilot が作業を完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 次の点に特に注意しながら実装を確認します。
   - モード切り替え用の UI コンポーネント
   - ユーザー設定を維持するためのローカルストレージの使用方法
   - ハイコントラストモードとライトモードの CSS またはスタイルの変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - モード切り替えを管理する JavaScript/TypeScript コード

6. 更新を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. 承認待ちのワークフローがあることを示すインジケーターを確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローがキューに登録されたことが表示されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストすることもできます。Codespace に移動し、ターミナルを開いて、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えてください)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次にアプリケーションを起動し、ブラウザーでハイコントラストモードとライトモードの切り替えを試します。期待どおりに動作し、ページを再読み込みしても設定が維持されることを確認します。

アクセシビリティのカスタムエージェントが、アクセシビリティのベストプラクティスに従うよう Copilot を導いたことを確認してください。アクセシビリティ上の懸念や改善点があれば、これまでの PR と同様に、コメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 委任を続ける

クラウドエージェントには、実際のバックログ項目を割り当てて、別の作業に集中する方法が効果的です。習慣にするため、リポジトリにさらにいくつか issue を登録し、Copilot に割り当ててみてください。たとえば、次のようなタスクがあります。

- ゲーム詳細ページに支援者向けの関心登録フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

クラウドエージェントの実習環境を完了しました。各レッスンで、次のことを行いました。

- **リポジトリに含まれるカスタム指示ファイルを確認し**、後でクラウドエージェントの出力に及ぼす効果を把握できるようにしました。
- **issue を Copilot クラウドエージェントに割り当て**、環境のセットアップ、計画、非同期の実行を確認しました。
- **アクセシビリティ用のカスタムエージェントを作成して使い**、ハイコントラストモードとライトモードの切り替えを追加しました。
- **Copilot agents ページをミッションコントロールとして使い**、アクセシビリティのセッションを監視して作業中に方向を調整しました。
- **クラウドエージェントのプルリクエストをレビューして反復的に改善し**、`@copilot` に変更を依頼して、ワークフローを承認しました。

## 振り返りと次のステップ

クラウドエージェントの実習環境を完了しました。引き続き学ぶ場合は、ほかの実習環境で、ここで取り組んだ内容をさらに掘り下げることができます。

- 🖥️ **[VS Code の実習環境](../../vscode/)** — IDE から Copilot Chat の agent mode と MCP の統合を確認します。
- 💻 **[CLI の実習環境](../../cli/)** — Plan モードと Autopilot モード、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、クラウドエージェントに戻る任意の `/delegate` を使い、ターミナルからレビュー済みの変更を 2 つリリースします。

自分のリポジトリでは、次のことを試してください。

- リファクタリングやテストカバレッジの issue をクラウドエージェントに割り当てる。
- 別の分野向けのカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot クラウドエージェントのセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot クラウドエージェントのファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
