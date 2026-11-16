---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードを使って、フィルタリング機能を実装しました。pull request を開く前に、ブラウザーで実際に動作することを確認しましょう。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーで機能をテストしてもらいます。その後、ブランチを公開して PR を作成します。

この演習では、次の操作を行います。

- Model Context Protocol（MCP）とは何か、MCP サーバーが Copilot にツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot に Playwright MCP を使って、ブラウザーでフィルタリング機能を手動テストするよう依頼します。
- ブランチを公開し、フィルタリング作業の pull request を作成します。

## Model Context Protocol（MCP）とは

エージェントモードは、エディターの外部にある機能を利用すると、さらに強力になります。Model Context Protocol（MCP）は Copilot が外部のツールやサービスを利用するための標準的な方法です。エージェントが外部のツールやサービスと通信できるようにします。

![エージェントモードがコンテキスト、LLM、ツール（MCP サーバーや VS Code 拡張機能が提供するツールを含む）とやり取りする仕組みを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol（MCP）](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)を使うと、AI エージェントは外部のツールやサービスと通信できます。MCP によって、AI エージェントはリソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わって操作を実行したりできます。

これらのツールやリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを介してアクセスします。MCP サーバーは、既存の API や npm パッケージなどのローカルツールといった外部ツールとの通信を管理します。各 MCP サーバーは、AI エージェントが利用できるツールやリソースのセットを提供します。

よく使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。新しいリポジトリの作成、既存リポジトリの更新、issue や pull request の管理などをエージェントが実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。Web ページへの移動、フォームへの入力、ボタンの選択などをエージェントが実行できます。

さまざまなツールやリソースにアクセスできる MCP サーバーがほかにもあります。GitHub は、エコシステム内の MCP サーバーを見つけやすくし、貢献を促すために[MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはプロジェクト内のほかの依存関係と同様に扱ってください。利用前にソースコードを確認し、発行元を検証して、セキュリティへの影響を慎重に検討してください。信頼できる MCP サーバーのみを利用し、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで利用できる MCP サーバーを構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. すでに `github` サーバーが構成されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクトテンプレートに含まれています。そのため前の演習で Copilot がバックログの issue を読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot がそのツールを初めて使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと Copilot がブラウザーを操作できるため、機能のテストに適しています。

1. `.vscode/mcp.json` に `github` と並べて `playwright` エントリを追加し、次の内容にします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル**サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグは Playwright に画面を表示せずブラウザーを実行するよう指示します。デスクトップ環境のない codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトではエンドツーエンドテストに Playwright を使っているため、必要なブラウザーは通常すでにインストールされています。Copilot がブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行するよう依頼して、もう一度試してください。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて**起動します。Copilot がそのサーバーのツールを初めて使うときにサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` で `playwright` エントリの上に **Start** 操作が表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code からサーバーを信頼するか確認されたら、構成を確認し、信頼することを選択してサーバーを起動します。

手動で起動する必要はありません。省略した場合は、次の手順で Copilot が初めてツールを使うときに VS Code がサーバーを起動し、信頼するか確認します。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]が提供する実際のブラウザーを Copilot が操作できます。自分でアプリを操作する代わりに、エージェントがページを開き、移動し、フィルターを適用して、結果を読み上げ、確認内容をまとめます。会話を離れずに機能をすばやく検証できます。

内部的には、Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティツリー][playwright-mcp-server]を利用します。ボタン、リンク、リスト項目などの構造化されたラベル付き要素に基づいてエージェントが判断するため、基本的な機能確認と同時に簡単なアクセシビリティ確認もできます。

サーバーを接続し、アプリを起動した状態で、Copilot に次の手順でフィルタリング機能を試すよう依頼します。

```text
Playwright MCP サーバーを使って、実行中のアプリ http://localhost:4321 をブラウザーで開き、新しいゲームのフィルタリング機能を検証してください。

1. ゲームページに移動し、表示されているゲームの数を記録してください。
2. カテゴリフィルターを適用し、そのカテゴリのゲームだけが一覧に表示されることを確認してください。
3. フィルターを解除してからパブリッシャーフィルターを適用し、そのパブリッシャーのゲームだけが一覧に表示されることを確認してください。
4. カテゴリフィルターとパブリッシャーフィルターを組み合わせ、両方の条件が結果に反映されることを確認してください。

各手順で確認したことを報告し、期待どおりに動作しない点があれば指摘してください。
```

Copilot は Playwright MCP サーバーを介してブラウザーを起動し、各手順を実行して結果を報告します。issue の受け入れ条件と照らし合わせて報告を確認してください。問題があれば、追加の質問をするか、PR を開く前にコードの修正を依頼します。

> [!NOTE]
> テスト時にはアプリを `http://localhost:4321` で実行しておく必要があります。開発サーバーを停止した場合は、プロンプトを送信する前に再起動します。Copilot が Playwright MCP サーバーを初めて使うときにブラウザーがダウンロードされる場合があります。不足していると報告されたら、`npx playwright install chromium` を実行するよう依頼して再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開して pull request を作成する

機能が動作することを確認できたので、チームのレビュー用に pull request（PR）を開きます。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が加えた変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミットメッセージを生成します。

   ![変更内容を表示する Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリに push します。

## pull request を作成する

GitHub.com や GitHub コマンドラインインターフェイス（CLI）など、pull request を作成する方法はいくつかあります。GitHub Copilot を使っているので、PR の作成も依頼しましょう。関連する issue を見つけ、PR に関連付けられます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. エージェント選択メニューで **Agent** が選択されていることを確認し、Copilot が GitHub ツールを使えるようにします。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   リポジトリでカテゴリとパブリッシャーによるフィルタリングに関する issue を探してください。現在のブランチから新しい pull request を作成し、該当する issue に関連付けてください。
   ```

4. 必要に応じて **Continue** を選択し、情報の取得や操作を許可します。GitHub ツールを初めて使うときは、GitHub への認証を求められることがあります。画面の指示に従ってください。
5. Copilot が issue を検索し、該当するものを見つけて PR を作成する様子を確認します。
6. Copilot が作成したリンクを選択し、pull request を確認します。ただし、まだ**マージしないでください**。

## まとめと次のステップ

おめでとうございます。この演習では次の操作を行いました。

- Model Context Protocol（MCP）とは何か、MCP サーバーが Copilot にツールを追加する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- ブラウザーでフィルタリング機能を手動でテストしてからリリースしました。
- ブランチを公開し、フィルタリング作業の pull request を開きました。

MCP サーバーを機能のテストに使いましたが、MCP は Copilot を拡張する方法のひとつです。次は、アクセシビリティレビューなど、対象を絞ったタスクを効率化する[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
