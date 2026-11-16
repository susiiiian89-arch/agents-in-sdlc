---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使う際はコンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に背景情報を伝える必要がある場合、その情報を利用できるようにします。Copilot に特定のコンテキストを伝える方法はいくつかあります。なかでも重要なのが[指示ファイル][instruction-files]です。コード生成の指針を伝えるために使います。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを使い、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot にどう伝わるかを確認します。
- 現在の指示を使って、フィルタリングに必要なデータの最初の部分（パブリッシャーヘルパー）を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードが新しい標準に従う様子を確認します。
- 指示の更新とフィルタリングの作業を `main` にコミットし、次の演習で Copilot が更新後のガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot の動作は確率的です。この演習では、文字単位で完全に同じ出力を得るのではなく、指示の更新後に傾向がどう変わるかを確認します。

## 指示ファイル

### シナリオ

優れた開発チームである Tailspin Toys には、開発手法に関するガイドラインと要件があります。具体的には、次のとおりです。

- データレイヤーには、必ずユニットテストを用意します。
- UI はダークモードにし、現代的な見た目にします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使って、Copilot がこれらの開発プラクティスに沿ってタスクを実行するための情報を提供します。

### カスタム指示

カスタム指示を使うと、コンテキストや好みを Copilot に伝え、コーディングスタイルや要件を理解しやすくできます。コーディング規約、ライブラリ、追加したいコメントの種類などを指定できるため、より適切な提案やコードスニペットを得るのに役立ちます。リポジトリ全体に適用する指示も、特定の種類のファイルに限定した指示も作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に送信される単一の指示ファイルです。技術スタック、開発対象の概要、ベストプラクティス、全体的なガイダンスなど、チャットや CLI の多くのリクエストに関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に関するガイドラインを提供します。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストの追加といったタスクに利用できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と、`applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方をコード生成時に使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準形式の指示ファイルにも対応しています。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成について詳しく扱うことは、この演習の範囲外です。サンプルプロジェクトの例は、一般的なアプローチを示しています。基本的な考え方は次のとおりです。

- `copilot-instructions.md` には、開発対象、プロジェクト構成、全体的なコーディング標準など、プロジェクト全体に関するガイダンスを記載します。
- `*.instructions.md` は、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル種別や、特定のタスク向けに使います。
- 自然な言葉で明確に指示します。コードの良い例と避けるべき例を示します。

指示ファイルの書き方に唯一の正解はありません。AI の使い方と同様に、プロジェクトで試しながら最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うすべてのプロジェクトに、充実した指示ファイルを用意することをお勧めします。このプロジェクトの指示を確認すると、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスクに対応するファイルがあります。
>
> Copilot に指示ファイルの作成を依頼することもできます。画面によって方法は異なります（たとえば、VS Code の **Configure Chat → Generate Agent Instructions** や、Copilot CLI の `/init` など）。必要な場合は、利用中の画面に対応するレッスンで手順を説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェント、その他のリソースを集めたリポジトリ [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認しましょう。すべてのリクエストに適用される `copilot-instructions.md` と、さまざまなタスク用の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には、**GitHub Actions Workflows** のガイダンスもあります。これらの指示は Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドは、リポジトリルートを基準とした glob を指定し、指示が適用されるファイルを決めます。たとえば、`**/*.test.ts` に一致する TypeScript のテストファイルが対象です。
5. このプロジェクトでユニットテストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾を確認します。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな指示セットを小さな再利用可能なファイルに分け、生成時の参考例を指定できます（ここにあるパスはリポジトリルートではなく、指示ファイルの場所を基準とします）。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で TSDoc コメントとファイルヘッダーコメントのルールを追加します。

## 変更用のブランチを作成する

ベストプラクティスに沿って、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

カスタム指示の影響を確認するため、まずは現在の指示ファイルを使った状態でプロンプトを送信します。その後、指示を更新して、同じプロンプトをもう一度送信します。

1. 前の演習で開いたエディターのタブを閉じ、Copilot に必要なコンテキストだけを渡します。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に示します。
3. Chat ビューのエージェント選択メニューから **Agent** を選び、ファイルの変更を適用できるようにします。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   src/lib/publishers.ts を作成または更新し、すべてのパブリッシャーについて name と id を返すデータアクセスヘルパーを追加してください。ファイルの変更を適用してください。
   ```

5. Copilot はプロジェクトを調査してコードを更新します。多くの場合、ヘルパーファイルとテストの両方が変更されます。
6. 提案されたヘルパーが、最初の引数として `db` クライアントを受け取る型付き関数であり、パブリッシャーの型付き配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約によるものです。
7. 提案されたコードに TSDoc コメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot の動作は確率的なため、指示がなくてもドキュメントコメントを追加する場合があります。その場合も問題ありません。指示を更新した後に一貫性が高まることが、この演習の目的です。

## リポジトリの新しい標準を追加する

前述のとおり、`.github/copilot-instructions.md` にはプロジェクト全体に適用する情報を記載します。コードの提案を改善するため、リポジトリのコーディング標準を追加しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. **Code formatting requirements** セクションを探します。27行目付近にあります。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関するルールがありません。そのため、生成されたヘルパーにドキュメントコメントが含まれていませんでした。
3. 既存の標準の直後に、次の Markdown を追加し、ファイルヘッダーと TSDoc コメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで学んだように、指示ファイルはリポジトリ全体に適用する `.github/copilot-instructions.md` にも、特定の言語、ファイル種別、タスクに使う `*.instructions.md` にも作成できます。ここで追加したドキュメントコメントのルールのような、プロジェクト全体の標準にはリポジトリレベルのファイルが適しています。

## 同じプロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot が適切なファイルに注目するようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   src/lib/publishers.ts を作成または更新し、すべてのパブリッシャーについて name と id を返すデータアクセスヘルパーを追加してください。ファイルの変更を適用してください。
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが追加されていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

これで新しいプロジェクト標準に従うよう Copilot を導き、次の演習で拡張する実際のコードに適用できました。

## 指示とブランチをコミットして push する

指示ファイルもほかのアセットと同じように、ソース管理で管理します。指示とヘルパーの変更をリポジトリにコミットして push しましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）を押して、新しいターミナルウィンドウを開きます。
2. ターミナルで `git status` を実行し、指示とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. ターミナルで指示の更新と、ヘルパーの基礎コードについて Copilot が変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルからブランチをリポジトリに push します。

   ```bash
   git push -u origin custom-instructions
   ```

## pull request を作成してマージする

ブランチを push したので、バックログにあるドキュメント標準の issue に関連付けて pull request を作成します。手動で行うこともできますが、GitHub ツールに接続済みの Copilot に依頼できます。issue を見つけ、issue をクローズする PR を作成してから、マージを依頼しましょう。

> [!NOTE]
> Copilot に GitHub 上の操作を依頼するのは今回が初めてです。プロジェクトテンプレートでは、GitHub のツールがあらかじめワークスペースに接続されています。Copilot がツールを初めて使うときに、VS Code で GitHub へのサインインを求められることがあります。画面の指示に従ってください。この接続のしくみ（Model Context Protocol）については、後の演習で説明します。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関連する issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。指示の更新と、今後のフィルタリング作業の基礎となる新しいパブリッシャーヘルパーの両方を説明に含めます。

   ```
   指示ファイルの更新に関する issue を探してください。現在のブランチから新しい PR を作成し、その PR が該当の issue をクローズすることを明記してください。また、今後のフィルタリング作業に向けてパブリッシャーヘルパーの基礎を追加したことも記載してください。
   ```

4. Copilot が issue を探して PR を作成します。
5. GitHub の操作を許可するよう Copilot から求められた場合は、コマンドを確認して適切に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトで PR をマージしてローカルのブランチを `main` に戻すよう Copilot に依頼します。

   ```
   PR を main にマージしてください。その後、ローカルで main に戻り、最新のコードを pull して最新状態にしてください。
   ```

7. GitHub の操作やシェルコマンドの実行を許可するよう求められたら、コマンドを確認して適切に **Allow** を選択します。

これで GitHub Copilot の支援を使って pull request を作成し、マージできました。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次の操作を行いました。

- *既存の*指示でコード生成プロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じプロンプトを再実行し、生成コードが新しい標準に従う様子を確認しました。
- 次の演習で続けて作業できるよう、指示の更新とヘルパーの基礎コードを `main` にコミットしました。

次は、[エージェントモード][next-lesson]で指示を活用し、Copilot にコードベース全体にわたる新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
