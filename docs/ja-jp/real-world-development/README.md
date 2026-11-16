---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共通のバックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用しましょう。反復しながら変更を計画し、コードを作成し、動作をテストし、pull request をレビューし、エージェントで作業を管理します。

## シナリオ

架空の企業 Tailspin Toys に新しく加わった開発者として作業します。同社は開発者向けテーマのボードゲーム用クラウドファンディングを提供しています。チームのバックログは GitHub issue として作成済みで、すぐに着手できます。フィルタリングやページネーションなどの機能開発に加えて、アクセシビリティやコーディング標準などの品質改善も含まれています。

## 作業環境を選ぶ

GitHub Copilot は、利用する環境に合わせて使えます。開発方法に合った環境を選択してください。各ワークショップは独自のセットアップ手順から始まり、共通の Tailspin Toys シナリオを使用します。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan と Autopilot モード、指示、スキル、カスタムエージェント、Playwright MCP、Agent Merge、便利なスラッシュコマンドを活用します。

### [GitHub Copilot アプリ][app]

デスクトップアプリでエージェントセッションを並行して実行し、セッションモードを切り替え、キャンバスで共同作業し、GitHub issue と pull request を管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub issue を非同期エージェントに割り当て、作業を案内して進捗を監視し、作成された pull request をレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに、Copilot Chat のエージェントモード、MCP サーバー、カスタムエージェントを活用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
