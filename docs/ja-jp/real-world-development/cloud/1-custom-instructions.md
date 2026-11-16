---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使う際はコンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に背景情報を伝える必要がある場合、その情報を利用できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に何を実行するかだけでなく、どのように実行するかも伝えられます。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを使い、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot にどう伝わるかを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code および CLI のハーネスとは異なり、この演習では変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub issue を基に非同期で作業するため、影響をリアルタイムで並べて示すのが難しいからです。後の演習で cloud agent が作成した pull request をレビューする際に、指示ファイルの影響を確認します。

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

## リポジトリの新しい標準を追加する

ここで行う唯一の編集として、ドキュメントを TSDoc コメントとファイルレベルのコメントヘッダーでコード内に記述するプロジェクト全体のルールを追加します。後の演習で issue を cloud agent に割り当てると、この指示が適用されます。

編集の前にブランチを作成します（cloud agent は issue の対象ブランチから指示を読み込みますが、変更を整理してコミットします）。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 指示の更新をコミット、push、マージする

Cloud agent は issue が対象とするブランチから指示ファイルを読み込みます。次の演習で issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。そのため、cloud agent が指示を使えるように、変更を `main` にマージする必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチを push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` への pull request を開き、マージします。次の演習で割り当てた issue に取り組む際、cloud agent はこれらの指示を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチを省略して `main` に直接コミットしてもかまいません。実際のプロジェクトでの手順に近づけるため、ここではブランチを使っています。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` に新しいリポジトリ全体の標準を追加しました。このハーネスの後半で cloud agent が作成する pull request に、その標準が反映されます。

次は、[初めての issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
