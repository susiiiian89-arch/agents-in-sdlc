---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2つの pull request、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連の開発ワークフローを進めました。次の操作を行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、用意されていたフィルタリングの issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初の pull request（PR）を手動でマージしました。
- フィルタリングの issue から始め、Plan モードで進め方を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol（MCP）サーバーを追加し、実際のブラウザーでフィルタリングを確認しました。
- 品質保証（QA）カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーで確認した根拠を評価しました。
- フィルタリングの変更全体を確認し、フィルタリング PR の Agent Merge を承認しました。
- コンテキスト、モデル、共有、任意のクラウドへの委任に使うスラッシュコマンドを確認しました。

## 実装した内容

このワークショップでは、更新後の `main` から作成した各ブランチで、2つの PR の節目を経験しました。

1. **星評価:** 既存の `starRating` と未評価であることを示す状態をゲームカードに表示します。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルタリングの計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡潔に進めるため、それらの作業を1つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を介した Copilot のブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA はこれらの結果に要件とテストカバレッジのレビューを加え、PR の前にまとめて評価しました。

追加するテストは、実際に不足している部分を補うものである必要があります。QA の実行で新たなテストが不要と判断される場合もあります。マージを承認する前にコードと根拠を確認し、変更後は影響する検証結果を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールによって、作業結果が変わります。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズは複数の会話で再利用し、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは繰り返し実行できるタスクを記述し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びましょう。**Plan** は実装前に方法を検討するとき、**Interactive** は範囲を絞った変更に参加しながら進めるとき、**Autopilot** は範囲が明確なタスクを任せるときに使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

ツールや設定と同じくらい、コンテキストも重要です。何を、なぜ、どのように作るかを明確に伝えると、結果は大きく変わります。

## さらに確認する

主要なワークフローを学びました。Copilot CLI には、ほかにも確認する価値のある機能があります。

- `/review` を使うと、コードレビューエージェントに変更を分析させることができます。
- `/rubber-duck` を使うと、問題について相談し、別の視点を得られます。
- `/fleet` を使うと、独立したサブタスクを並列に調整できます。
- `/worktree` を使うと、別のタスクを分離できます。
- `/delegate` を使うと、Copilot cloud agent にタスクを送信できます。

## 次のステップ

ツールを上達させる最善の方法は、使い続けることです。実際のコード、趣味のコード、長い間作りたいと思いながら手をつけていなかった小さなアプリなどで活用してください。チームと学びを共有し、お互いの経験から学びましょう。そして、ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys をさらに拡張したい場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進んでください。別の環境での開発を試すには、[VS Code ワークショップ][vscode]、[GitHub Copilot アプリのワークショップ][app]、または[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のカスタマイズ][customize-cli]
- [Copilot CLI で pull request を管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
