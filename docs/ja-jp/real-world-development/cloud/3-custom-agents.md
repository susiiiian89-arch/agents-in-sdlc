---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローにおける特定のタスクや分野に合わせて、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、特定の作業を効果的に進めるための指示、ベストプラクティス、コーディングパターン、専門知識を Copilot に伝えます。チームの知識を再利用可能なエージェントに反映できます。たとえば、[WCAG][wcag] への準拠を確認するアクセシビリティエージェント、セキュアコーディングに従うセキュリティエージェント、一貫したテストパターンを維持するテストエージェントを作成できます。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を記述する Markdown プロンプトが必要です。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、役割の重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。両者を区別するには、**カスタムエージェントは作業者、スキルはツール**と考えるとわかりやすいでしょう。

カスタムエージェントは独自のコンテキストウィンドウを持ち、作業を進める際にスキル（場合によってはほかのエージェントも）を調整します。このラボのアクセシビリティカスタムエージェントは、アクセシビリティガイドラインに沿ってサイトをレビューし、更新します。その作業の一部として、pull request のワークフローやテストの実行・管理を行うスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成方法に唯一の正解はありません。AI のほかの機能と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタムエージェントを使って、次の操作を行います。

- カスタムエージェントの定義方法を確認します。
- タスクをカスタムエージェントに割り当てます。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを使えるようにしたいと考えています。最近のユーザーフィードバックでは、テキストと背景色のコントラストが十分でないため、現在のダークテーマが読みにくいと感じるユーザーがいることがわかりました。このアクセシビリティの課題に対応するため、デザインチームはユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装したいと考えています。カスタムエージェントを使って機能を生成します。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントがすでに用意されています。内容を確認し、Copilot にどう指示するかを理解しましょう。

Codespace に戻り、アクセシビリティカスタムエージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを確認します。次の内容が含まれています。
   - アクセシビリティに配慮した Web サイトのコードを生成する際の主な責任。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違い。

## issue を作成して割り当てる

Mission control は、環境内のエージェントを管理する中心的な場所です。Copilot cloud agent にタスクを割り当て、進捗を監視し、作業中のセッションに追加のガイダンスを送ることができます。まず、ハイコントラストモードの作成を Copilot に割り当てましょう。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    サイトにハイコントラストモードが必要です。ユーザーがハイコントラストを切り替えられるようにしてください。ブラウザーの local storage に設定を保存してください。
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。
9. カスタムエージェント一覧から **Accessibility agent** を選択します。

    ![Accessibility カスタムエージェントが選択された cloud agent の割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

このレッスンでは、GitHub Copilot の[カスタムエージェント][custom-agents]について学びました。特定のタスクや分野に特化した AI アシスタントです。カスタムエージェントを使うと、チームの知識や標準を再利用可能な形にまとめ、Copilot が特定の作業を効果的に進めるための指針にできます。

次の内容を学びました。

- カスタムエージェントの定義方法。
- タスクをカスタムエージェントに割り当てる方法。

Copilot がハイコントラストモードを実装している間に、次は Mission control から[エージェントのセッションを監視して調整します][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使うための準備][org-custom-agents]
- [エンタープライズでカスタムエージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントを監視して管理する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
