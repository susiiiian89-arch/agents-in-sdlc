---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどありません。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残り続ける機能リクエストなどがその例です。GitHub Copilot の cloud agent は、コードの更新や機能の追加などのタスクを自律的に実行します。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。これにより、面倒な作業を任せて開発を加速し、開発者はより大局的な課題に集中できます。

Copilot cloud agent を使って、次の操作を学びます。

- コード生成環境をカスタマイズします。
- 操作が安全に行われるようにします。
- 範囲を明確に定義した issue の重要性を理解します。
- issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。サイトの初期バージョンを作成した委託先は、ドキュメントを十分に作成していません。実際、ドキュメントはほとんどありません。まず、アプリケーションのすべてのエクスポート関数に TSDoc のドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリにある別のタイトルを「関連ゲーム」として表示し、閲覧を続けられるようにします。現時点では完成された実装は必要なく、UX の受け入れテストに使えるものを求めています。この作業は現在のブロッカーですが、さらに優先度の高い issue がほかにあります。

どちらも、優先度が下がりやすいタスクの例です。Copilot cloud agent に割り当てると、非同期で作業させている間にほかのタスクに集中し、あとから戻って作業をレビューし、要件どおりか確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。たとえば、[GitHub issue を Copilot に割り当て][assign-issue]ます。割り当てると、Copilot は進捗を追跡するドラフト pull request を作成し、環境をセットアップしてタスクに取りかかります。セッションは作業中でも完了後でも確認できます。提案された解決策のレビュー準備ができると、pull request で通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単そうな作業にも複雑な側面があるため、[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階的に作業し、学び、試し、状況に応じて調整しましょう。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習ではカスタム指示とその役割を学びました。すでにカスタム指示の演習を終えている場合は、`.github/copilot-instructions.md` が機能することを確認しています。まだの場合は、ここで簡単に確認しましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認しておくと、あとでその効果を見つけやすくなります。

リポジトリの GitHub Web UI または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` - **Code standards** セクション、特に TypeScript の規約と TSDoc ドキュメントコメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` フロントマターを確認します。この指示は `**/*.test.ts` に一致するファイルに適用されます。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てると、生成された pull request に TSDoc ドキュメントコメント、TypeScript 規約、コメントヘッダーが反映されます。これらの指示ファイルによる影響は、後の演習で PR をレビューする際にも確認します。

## Copilot cloud agent の開発環境を設定する

誰が作業する場合でも、コードの作成には通常、特定の環境やセットアップスクリプトが必要です。タスクを割り当てられた Copilot もソフトウェアエンジニアのように作業するため、同様の準備が必要です。

Cloud agent は [GitHub Actions][github-actions] を作業環境として利用します。`.github/workflows/copilot-setup-steps.yml` に構成する[セットアップワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係をインストールできます。ラボを円滑に進めるため、このワークフローはあらかじめ設定されています。単一の Astro アプリ用に、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、移行とシードが済んだ SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
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

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub Actions ワークフローと同じように見えますが、いくつか重要な点があります。

- `copilot-setup-steps` という単一のジョブがあります。Copilot が pull request の作業を始める前に、GitHub Actions でこのジョブが実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot が実行するのを待たずに、正常に実行できるか確認するのに便利です。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、情報が古かったり、まったくなかったりするプロジェクトがほとんどです。こうした技術的負債を放置すると生産性が低下し、コードベースの保守や新しいメンバーの受け入れも難しくなります。Copilot はドキュメントの作成を得意としているため、cloud agent に任せるのに適した issue です。バックグラウンドで必要なドキュメントを生成してもらい、後の演習で作業をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して新しい issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    組織では、役立つ場合に関数やメソッドへ TSDoc ドキュメントコメントを付けることが求められています。しかし、最近の更新はこの標準に従っていません。動作の説明に役立つ箇所にドキュメントコメントを追加するため、既存のコードを更新する必要があります。
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

  ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成された pull request（PR）を開きます。タイトルは `[WIP]: Code lacks documentation` のような内容です。一覧に PR が表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後に、Copilot が todo リストを作成したことを確認します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで数分かかることがあります。Copilot は、前に説明したワークフローを実行して環境を作成し、プロジェクトを分析して、問題への最適な取り組み方を検討しています。

13. todo リストと、Copilot が実行するタスクを確認します。
14. pull request のタイムラインを下にスクロールし、Copilot が issue の作業を始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot セッションビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するため、ブラウザーを更新する必要がある場合があります。

16. ライブセッションをスクロールし、Copilot が問題を解決する様子を確認します。コードを調査して状態を把握し、計画を考えるために一時停止した後、コードを作成します。

完了まで数分かかることがあります。Copilot cloud agent の主な目的は、タスクを非同期で処理し、ほかの作業に集中できるようにすることです。この機能を活用するため、もう1つのタスクも Copilot cloud agent に割り当ててから、アプリケーションへの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

前述のとおり、GitHub Copilot cloud agent の利点のひとつは作業を分けられることです。デザインチーム向けの関連ゲームの追加に時間がかからないとしても、その時間をほかの作業に使えます。このタスクも Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    ゲームの詳細ページに関連ゲームを表示して、より多くのゲームを見つけられるようにしたいと考えています。デザインチームは UX を確認し、受け入れテストを行いたいと考えています。要件は次のとおりです。

   - `src/lib/` にデータアクセスヘルパーを追加し、指定したゲームを除く、同じカテゴリのほかのゲームを返すようにします。
   - ヘルパーを使って、ゲーム詳細ページに「Related games」セクションを表示します。
   - 関連ゲームがない場合にも対応します。
   - 新しいヘルパーのユニットテストを作成します。
   - PR を作成する前に、すべてのテストが成功することを確認します。
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことがわかります。

![Copilot が目の絵文字で issue の作業中であることを示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェアエンジニアと同じように作業するため、常に監視する必要はありません。完了後にレビューできます。次は、カスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents]について学びました。cloud agent に issue を割り当てると、非同期で作業を実行できます。技術的負債の解消、新機能の作成、別のフレームワークへのコード移行などに利用できます。

次の内容を学びました。

- コード生成環境をカスタマイズする方法。
- 操作が安全に行われるようにする方法。
- 範囲を明確に定義した issue の重要性。
- issue を Copilot に割り当てる方法。

cloud agent がバックグラウンドで作業している間に、次はカスタムエージェントを作成して使います。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

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
