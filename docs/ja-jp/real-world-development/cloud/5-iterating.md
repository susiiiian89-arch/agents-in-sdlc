---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善と機能追加を目的とする複数のタスクで GitHub Copilot を使いました。コードへのドキュメント追加、デザインチーム向けの関連ゲーム機能の実装、ハイコントラストとライトモードの切り替えを含むアクセシビリティ機能を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックして、作業を改善しましょう。

### シナリオ

これまでに何度も説明したように、生成 AI を導入しても、ソフトウェア設計や DevOps の基本は変わりません。常に生成コードをレビューし、通常の DevOps プロセスに沿って作業します。そこで、チームにレビューを依頼する前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関して GitHub Copilot が提案した内容を確認します。

## GitHub Copilot cloud agent とセキュリティ

Copilot cloud agent は非同期かつ監視なしで作業するため、安全性を保つ目的でセキュリティ上の制約が設けられています。次のような制約があります。

- Copilot はリポジトリに対する読み取りアクセスを持ち、書き込みはコード用ブランチに対してのみ許可されます。
- Cloud agent は GitHub Actions 内で実行され、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローを実行するには、人間の承認が必要です。
- [外部リソースへのアクセスは既定で制限されます](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーも対象です。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した1つ目の pull request（PR）、コードへのドキュメント追加を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認する際に、GitHub Copilot がファイアウォールでブロックされているという警告が表示されることがあります。Copilot は外部 MCP サーバーへの呼び出しを含む外部リソースへのアクセスが既定で制限されているため、これは**想定された動作**です。必要であれば、Copilot cloud agent の[ファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して pull request の一覧を開きます。
3. **Add missing documentation** など、同様のタイトルが付いた pull request を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、pull request のタイトルに **[WIP]** が含まれます。その場合は作業の完了を待ちます。数分かかることがあります。その間に休憩を取ったり、学んだことを振り返ったりしてください。

4. pull request の準備ができたら **Files changed** タブを選択し、変更を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく更新されたコードを確認します。新しい TSDoc ドキュメントコメントや、その他のドキュメントが追加されています。変更の内容は状況によって異なります。

    変更を確認しながら、TSDoc ドキュメントコメント、TypeScript の規約、コメントヘッダーを探します。これらは演習 2 の最初に確認したカスタム指示ファイルに基づいています。

6. 更新をレビューして問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. pull request のチェック欄にワークフローが登録されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

pull request で Copilot と作業するのは、一方通行ではありません。pull request のコメントやコードのインラインコメントで、チームメンバーと同じように Copilot にメンションできます。Copilot はコメントを読み、対応するための新しいセッションを開始します。結果は決定的ではないため、必ず依頼するべき文面を指定することはできません。たとえば、次のような更新を依頼できます。

- 各コードファイルの先頭に、役割を簡潔に説明するコメントヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc ドキュメントコメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. **@copilot** をほかのユーザーと同じようにメンションして、生成されたドキュメントの変更を依頼するコメントを追加します。上記の例、またはコードベースに追加したいドキュメントに関する別の提案を使ってください。
2. **View Session** を選択して Copilot の作業を確認します。Copilot が更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると pull request に戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の変更が完了すると、pull request に新しいコミットが表示されます。
5. **Files changed** タブを選択して変更を確認します。

納得できるまで反復してかまいません。内容が整ったら、PR をドラフトからレビュー可能な状態に変更し、`main` ブランチにマージします。

![PR をレビュー可能な状態にする](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページへの関連ゲーム表示に関する issue を解決するため、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルが付いた PR を選択します。
4. **Files changed** タブを選択して生成コードをレビューします。
5. 更新を確認して問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. pull request のチェック欄にワークフローが登録されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
9. **任意:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストすることもできます。Codespace に移動してターミナルを開き、次のコマンドを実行します（`<branch-name>` は Copilot が作成したブランチ名、たとえば **copilot/fix-8** に置き換えます）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を実装しました。前と同じように、Copilot cloud agent に更新を依頼して反復できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーや TSDoc ドキュメントコメントを追加したりできます（この issue はカスタム指示の更新**前**に割り当てたことを思い出してください）。前と同じように **Conversation** タブにコメントを追加すると、Copilot がそれを読み取り、新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティカスタムエージェントを使って実装した機能をレビューします。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 の Mission control で追加を依頼したライトモードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、pull request のタイトルに **[WIP]** が含まれます。その場合は作業の完了を待ちます。数分かかることがあります。

4. **Files changed** タブを選択して生成コードをレビューします。
5. 実装を確認し、特に次の点を確認します。
   - モードを切り替える UI コンポーネント。
   - ユーザー設定を保存する local storage の利用。
   - ハイコントラストモードとライトモード用の CSS またはスタイルの変更。
   - アクセシビリティ属性（ARIA ラベル、キーボード操作など）。
   - モードの切り替えを管理する JavaScript/TypeScript コード。

6. 更新を確認して問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. pull request のチェック欄にワークフローが登録されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストすることもできます。Codespace に移動してターミナルを開き、次のコマンドを実行します（`<branch-name>` は Copilot が作成したブランチ名に置き換えます）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイコントラストとライトモードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティカスタムエージェントが、アクセシビリティのベストプラクティスに従って Copilot の実装を導いたことがわかります。アクセシビリティについて気になる点や改善案があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 委任を続ける

cloud agent は、実際のバックログ項目を任せて別の作業に集中できるときに効果を発揮します。習慣にするため、リポジトリにいくつか issue を作成して Copilot に割り当ててみましょう。次のようなアイデアがあります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。これらのレッスンを通じて、次の操作を行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認し**、cloud agent の出力への影響を後で確認できるようにしました。
- **issue を Copilot cloud agent に割り当て**、環境のセットアップ、計画、非同期の実行を確認しました。
- アクセシビリティ向けの**カスタムエージェントを作成して使い**、ハイコントラストとライトモードの切り替えを追加しました。
- Copilot エージェントページを Mission control として使い、作業中のアクセシビリティセッションを監視して調整しました。
- Copilot の pull request をレビューして反復し、**@copilot** に変更を依頼し、ワークフローを承認しました。

## 振り返りと次のステップ

Cloud agent ハーネスを完了しました。さらに学びたい場合は、ほかのハーネスで今回の経験を補えます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から直接 Copilot Chat のエージェントモードと MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、任意の `/delegate` による cloud agent への連携を使い、ターミナルからレビュー済みの変更を2つリリースします。

自分のリポジトリでは、次のアイデアを試してください。

- リファクタリングやテストカバレッジの issue を cloud agent に割り当てます。
- 別の分野向けのカスタムエージェントを作成します。
- 異なる技術スタック用に `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

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
