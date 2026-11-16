---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、任意のクラウドへの委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI には操作に使えるスラッシュコマンドが多数用意されています。これらのコマンドを使うと、高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、ほかの便利なコマンドをいくつか確認します。

このレッスンでは、次の操作を行います。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストをどう管理するかを確認します。
- `/model` を使い、利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並列作業、worktree、クラウドエージェントへの委任に使う任意のコマンドを確認します。

## シナリオ

CLI の主要なワークフローが完了しました。次は、コンテキストの管理、モデルの切り替え、セッションの共有、そして任意で[Copilot cloud agent][about-cloud-agent]に作業を委任するなど、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルの最大コンテキストウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュコマンドを使って自分で確認したり圧縮したりできます。

1. Codespace に戻り、まだ開いていない場合はリポジトリルートから Copilot CLI を起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように分けられているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力し、結果を比較します。会話が短い場合、大きな違いはないことがあります。

> [!NOTE]
> コンテキストウィンドウが埋まると、Copilot CLI は自動的にコンテキストを圧縮します。自分でタイミングを選びたい場合は `/compact` を使います。関連のないタスクに切り替える場合は、`/clear` または `/new` で新しい会話を開始します。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者ごとに好みも異なります。Copilot CLI では、利用可能なモデルの一覧を表示して選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを使うか、別のモデルを選択するか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力して学びを共有すると、AI ツールの活用方法を改善できます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有可能なリンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みバージョンで利用できる `/share` のオプションを確認します。
2. セッションを共有する場合は、目的に合った出力先を選択します。たとえば、`/share file` はローカルの Markdown ファイルにエクスポートします。
3. エクスポートした内容を、誰かに送信したり公開したりする前に確認します。プロンプト、応答、プロジェクトの詳細が含まれることがあります。

リンクや gist の公開は任意です。チームが共有するつもりのないリポジトリや会話の内容を公開しないでください。

## 任意: 作業の並列化や委任

ワークショップの主要な内容は完了しました。Copilot CLI には、大規模なタスク向けのコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分割し、並列に実行できます。
- `/worktree` は、別のタスク用に分離された Git worktree を作成できます。
- `/delegate` は、Copilot cloud agent にタスクを送り、非同期で作業させて pull request を作成できます。

これらのコマンドを使うと、追加の worktree が作成されたり、リモートで作業が行われたりするため、任意の機能です。試すときは、範囲を明確にした新しいタスクから始め、通常のワークフローで結果を確認してください。非同期エージェントの作業について詳しく知りたい場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、構成の変更、セッションの共有、内部動作の確認ができます。このレッスンでは、次の操作を行いました。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストをどう管理するかを確認しました。
- `/model` を使い、利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並列作業、worktree、クラウドエージェントへの委任に使う任意のコマンドを確認しました。

ほかにも多くのスラッシュコマンドがあります。次は、[学んだことを振り返り][next-lesson]、さらに学ぶための次のステップを確認して、このワークショップを締めくくりましょう。

## リソース

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
