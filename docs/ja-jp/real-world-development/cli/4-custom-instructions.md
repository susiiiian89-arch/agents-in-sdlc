---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う際はコンテキストが重要です。タスクを特定の方法で実行する必要がある場合、そのガイダンスを Copilot が参照できるようにします。[指示ファイル][instruction-files]には、求めるコードだけでなく、コードの構成方法も記述できます。フィルタリングを実装したので、Copilot が使用した指示を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次の操作を行います。

- リポジトリの指示とパスに応じた指示ファイルがエージェントにどう適用されるかを確認します。
- コーディング標準に従うよう指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

優れた開発チームである Tailspin Toys には、開発手法に関するガイドラインと要件があります。具体的には、次のとおりです。

- コメントでは、コードの繰り返しではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数では、`db` 引数を注入できる場合はその引数も含め、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連コードが変更された際にはコメントを最新の状態にします。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使って、Copilot がこれらの開発プラクティスに沿って作業するための情報を提供します。

## 指示ファイル

カスタム指示を使うと、コンテキストや好みを Copilot に伝え、コーディングスタイルや要件を理解しやすくできます。コーディング規約、ライブラリ、追加したいコメントの種類などを指定できるため、より適切な提案やコードスニペットを得るのに役立ちます。リポジトリ全体に適用する指示も、特定の種類のファイルに限定した指示も作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に送信される単一の指示ファイルです。多くのリクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスクに関するガイドラインを提供します。

> [!NOTE]
> ほかの形式の指示への対応状況は、環境によって異なります。形式を利用する前に、[カスタム指示のサポート状況][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

変更を加える前に、スタータープロジェクトに含まれる指示ファイルを確認し、既存の内容を把握しましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディングガイダンスを確認します。この指示は、このリポジトリに関するすべての Copilot とのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関する指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドは、指示が適用されるファイルを指定する glob です。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存プロジェクトファイルへのリンクを確認します。大きな指示セットを小さな再利用可能なファイルに分割し、参考例を指定できます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルは出発点として適切ですが、まだ不足している点があります。新たに生成する TypeScript に TSDoc コメントを追加するよう、`copilot-instructions.md` を変更します。

1. `.github/copilot-instructions.md` の **Code formatting guidance** セクションを探します。35行目付近にあります。
2. 最後の箇条書きとして、次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話開始時にリポジトリの指示を読み込みます。更新した指示を適用するため、フィルタリングの会話を終了して再開します。これにより、機能のコンテキストを保ったまま新しいガイダンスを読み込めます。

1. フィルタリングの会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたら、フィルタリングの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新後のガイダンスを適用するよう依頼します。

   ```plaintext
   指示とコードのガイダンスを更新しました。生成したコードをこのガイダンスに合わせて更新してください。
   ```

4. `/diff` を入力して、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot が指示ファイルをどのように読み込むかを確認し、新しい標準を機能に適用しました。具体的には、次の操作を行いました。

- リポジトリの指示とパスに応じた指示ファイルがエージェントにどう適用されるかを確認しました。
- コーディング標準に従うよう指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して実行できるようにします。

## リソース

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート状況][custom-instructions-support]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
