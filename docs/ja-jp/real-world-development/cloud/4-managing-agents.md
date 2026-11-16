---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の2つの演習では、ユーザー体験の改善と機能追加を目的とする3つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、そのタスクを監視することも重要です。

cloud agent に割り当てたタスクを管理するツールはいくつかあります。GitHub.com の[エージェントページ][agents-page]もそのひとつです。この Mission control では、pull request（PR）が開かれているすべてのエージェントタスクを確認できます。実行された操作を調べたり、進行中のセッションに指示を送って作業を調整したりできます。

このレッスンでは、次の操作を行います。

- cloud agent のタスクを監視するために、エージェントページを確認します。
- 作業中のセッションに追加機能を依頼し、進め方を調整します。

## シナリオ

ハイコントラストモードの作成をエージェントに割り当てた後、チームはライトモードも追加するとよいと考えました。サイトのスタイル更新と切り替え機能を実装中のため、同時に追加するのが自然です。エージェントの作業を調整し、ハイコントラストに加えてライトモードも実装してもらいます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの状態を確認します。

1. [Copilot エージェントページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインの両方に表示されるタスク一覧を確認します。次のように、Copilot に割り当てたタスクが表示されます。
   - コードのドキュメントを更新する。
   - ゲーム変更用の API を生成する。
   - Web サイトにハイコントラストモードを追加する。
3. 実行中のタスクを選択し、Copilot が行った作業を確認します。次のような操作が含まれる場合があります。
   - リポジトリからコードをチェックアウトする。
   - Copilot の作業環境を作成する。
   - MCP サーバーをセットアップする。
   - 割り当てられたタスクを完了するためにさまざまな手順を実行する。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や作業方法によって異なります。

4. 右側に表示される pull request（PR）ペインも確認します。PR と変更されたファイルを表示し、進捗を監視できます。

## cloud agent の作業を調整する

実行中のタスクを確認できたので、ハイコントラストモードの作業中にライトモードの切り替えも追加するよう Copilot に依頼します。

1. ハイコントラストモードの追加に関するセッションを選択します。Copilot が付けた名前や作業状況によって、タイトルは異なります。

    ![Mission control のアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セッションを数分監視し、セットアップが完了して作業が始まるまで待ちます。次のようなメッセージが表示されたら作業開始です。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    ハイコントラストモードの作業に加えて、ライトモードも追加してください。ユーザーが希望する表示モードを選べるように、ライトモード用の切り替えも設けてください。
    ```

    ![Copilot の作業中にセッションへ指示を追加するダイアログ](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを確認し、作業に取り入れることを確認します。

## Copilot の作業を待つ

前と同じように、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中だった手順を終えた後に、新しい要望を作業の流れに加えます。

今回も完了まで数分かかります。ここまでに学んだことを振り返りながら待ちましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視し、案内する中心的な場所である Copilot エージェントページを確認しました。Mission control では、進行中や完了したタスクを追跡し、実行された作業をレビューし、作業中のタスクの範囲やガイダンスを変更できます。

次の内容を学びました。

- Mission control とエージェントページを使って cloud agent のタスクを監視する方法。
- 作業中のセッションに指示を送り、追加機能を依頼する方法。

Copilot がアクセシビリティ機能を実装している間に、次は Copilot が作成した[pull request を反復して改善します][next-lesson]。

## リソース

- [Copilot エージェントページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
