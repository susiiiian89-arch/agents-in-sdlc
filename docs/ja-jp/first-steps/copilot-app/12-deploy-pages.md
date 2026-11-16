---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Copilot アプリを使って Pages ワークフローを構成し、pull request を確認して、公開された Space Quiz を検証します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages で Space Quiz をデプロイし、誰でもアクセスできる場所に公開しましょう。Copilot はワークフローの準備とデプロイ手順の案内を行いますが、変更をレビューしてサイトが公開されたことを確認するのはユーザーの役割です。

このレッスンでは、次の操作を行います。

- Copilot に GitHub Pages デプロイの構成を依頼します。
- マージ前にワークフローと pull request をレビューします。
- デプロイを進め、公開されたサイトを確認します。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、手軽に試す方法がありません。単一ファイルのサイトを GitHub Pages で公開すると、サーバーやランタイムの依存関係を追加せず、公開 URL からアクセスできるようになります。

## デプロイの準備を Copilot に依頼する

> [!WARNING]
> pull request の作成時に、コードがコミットおよび push されることがあります。このプロンプトを送る前に、承認設定と送信先のリポジトリを確認してください。**Approve all** を選択している場合、個別の確認プロンプトは表示されないことがあります。

`space-quiz` の Copilot アプリセッションで、次のプロンプトを送信します。

```plaintext
このリポジトリで GitHub Pages にデプロイする準備をしてください。必要に応じて Web サイトをビルドし、既定のブランチに変更が反映された後に GitHub Pages へデプロイする GitHub Actions ワークフローを作成してください。README がまだない場合は、アプリとローカルでの実行方法を説明する README を追加してください。これらの変更を含む pull request を作成し、そのリンクと、必要な Pages の設定や権限を報告してください。マージ、自動マージの有効化、Pages 設定の変更、デプロイは行わないでください。作業を停止して、レビューを待ってください。
```

## ワークフローと pull request をレビューする

1. README は、リポジトリにまだない場合に限って追加されていることを確認します。
2. ワークフローのビルドとデプロイの手順、トリガー、権限など、pull request の変更をレビューします。
3. ワークフローが、マージされていない pull request ではなく既定のブランチからデプロイすることを確認します。
4. 修正が必要な場合は依頼し、更新後の差分を確認します。マージを承認する前に、必須の pull request チェックが成功するまで待ちます。

## マージとデプロイを承認する

> [!WARNING]
> 次のプロンプトを送信すると、レビュー済みの pull request のマージ、リポジトリの Pages 設定の変更、サイトの公開をエージェントに許可します。レビューを完了し、リポジトリと pull request を確認してから送信してください。承認設定によっては、追加の確認なしにこれらの操作が行われる場合があります。

`<PR_NUMBER>` をレビューした pull request の番号に置き換えてから、別のプロンプトとして送信します。

```plaintext
このリポジトリの pull request <PR_NUMBER> をレビューし、マージとデプロイを承認しました。GitHub Actions を使って GitHub Pages を有効にし、必須チェックが成功した後に限り、その pull request をマージしてください。デプロイが成功するまで監視し、公開された Web サイトの最終 URL を報告してください。Pages の有効化やマージに必要な権限がない場合は、私が何を承認または実行する必要があるかを正確に説明してください。サイトが公開されるまでは成功と報告しないでください。
```

![3段階の図: マージ済みの space-quiz pull request、Build website、Upload artifact、Deploy to Pages の各ステップが成功した GitHub Actions Deploy website の実行、yourname.github.io/space-quiz/ に公開された Space Quiz](../../../_images/first-steps-app-pages-deploy.svg)

## デプロイを確認する

1. GitHub Actions のデプロイ実行が成功するまで確認します。
2. 報告された URL を開き、Space Quiz が表示されることを確認します。

エージェントが Pages の有効化や pull request のマージを実行できない場合は、案内された手順に従います。デプロイが成功し、公開サイトが読み込まれるまで、タスクが完了したと判断しないでください。

## まとめと次のステップ

Space Quiz がライブサイトとして公開され、エージェントの報告だけに頼らず、デプロイを確認できました。[レッスン 13: 振り返りと次のステップ][next-lesson]に進みます。

[next-lesson]: ../13-review/
