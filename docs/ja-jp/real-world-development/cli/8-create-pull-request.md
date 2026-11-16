---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルタリング、指示、スキルの更新、QA プロファイル、テストをまとめて確認し、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、指示の更新、スキルの更新、品質保証（QA）プロファイル、テストは、すべて1つのブランチに保存されています。まとめて確認して pull request を作成しましょう。星評価の pull request（PR）は自分でマージしましたが、今回は**Agent Merge**にプロセスを任せます。

> [!NOTE]
> 通常は機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを簡潔に進めるため、フィルタリングと品質ワークフロー全体を1つの会話とブランチにまとめ、この PR に含めています。

このレッスンでは、次の操作を行います。

- Agent Merge とは何か、マージまでのライフサイクルをどう自動化するかを学びます。
- 機能全体の変更と検証結果を確認します。
- フィルタリングの PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルタリング機能の開発では、Copilot を使って計画、実装、検証を行いました。Tailspin Toys では、マージの承認を開発者の管理下に置いたまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は pull request をマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題（失敗した継続的インテグレーション（CI）チェックの修正、レビューコメントへの対応、必要に応じたリベースなど）に取り組みます。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその役割を任せることもできます。Agent Merge を有効にする前に、作業を確認し、マージできる状態か判断してください。

## Agent Merge で PR を管理する

すべてのコードができたので、まとめて確認し、PR を作成して、残りのプロセスを Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを使って新しい PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを監視します。すべて成功したらマージします。
7. 「PR #14 was squash-merged successfully.」などのメッセージが Copilot から表示されることを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限の不足を回避しません。ブロックされた場合は報告された理由を確認し、リポジトリで許可されていれば、レビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、pull request の処理など、開発プロセスの複数の部分を自動化しました。このレッスンでは、次の操作を行いました。

- Agent Merge とは何か、マージまでのライフサイクルをどう自動化するかを学びました。
- 機能全体の変更と検証結果を確認しました。
- フィルタリングの PR を作成しました。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、[Copilot CLI の便利なスラッシュコマンド][next-lesson]を確認し、コンテキスト、モデル、共有、任意のクラウドへの委任について学びます。

## リソース

- [Copilot CLI で pull request を管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
