---
title: "练习 2 - GitHub Copilot 云端代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都受到技术债务困扰。这可能是尚未解决的安全问题、需要更新的旧代码，或一直搁置在待办列表中的功能请求，因为始终没有时间实现。GitHub Copilot 云端代理可以自主执行更新代码和添加功能等任务。代理完成工作后，会创建一个草稿 PR，供开发者检查。这样可以将繁琐任务交给代理，加快开发流程，让开发者专注于更重要的工作。

本课将使用 Copilot 云端代理探索以下内容：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 明确界定问题范围的重要性。
- 将问题分配给 Copilot。

## 场景

Tailspin Toys 有一些希望解决的技术债务。最初受雇开发网站的承包商没有留下完善的文档——事实上，你会发现项目完全没有文档。首先，他们希望为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队已经准备好改善游戏发现体验。他们希望在每款游戏的详情页上显示相关游戏——也就是同一类别中的其他游戏——让用户可以继续浏览。他们暂时不需要完善的实现，只需要一个可以用于用户体验验收测试的版本。目前这是一个阻碍，但还有其他优先级更高的问题。

这些都是容易被降低优先级的任务，也很适合分配给 Copilot 云端代理。它可以异步处理这些任务，让开发者先专注于其他工作，稍后再回来检查 Copilot 的成果并确认一切符合预期。

## 了解 GitHub Copilot 云端代理

[GitHub Copilot 云端代理](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发者一样在后台执行任务。就像与开发者协作一样，你可以通过多种方式启动工作，包括[将 GitHub 问题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求以跟踪进度、设置工作环境，并开始处理任务。你可以在 Copilot 会话进行中或完成后查看详情。建议方案准备好供你检查时，Copilot 会在拉取请求中标记你。

## 明确界定任务范围的重要性

GitHub Copilot 并非魔法——你仍然需要思考。即使看似简单的操作，深入分析后也可能十分复杂，因此[请谨慎界定 Copilot 云端代理的任务范围][cloud-agent-best-practices]。把它当作 AI 结对编程伙伴：分阶段工作、学习、尝试并不断调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令，以及它们如何引导 Copilot。如果你已完成自定义指令的实践练习，就已经见过 `.github/copilot-instructions.md` 的效果；如果还没看过，现在正好快速浏览一下。将工作分配给 Copilot 云端代理前，先只读查看此存储库中的指令文件，之后就能发现它们的影响。

在 GitHub Web 界面或正在运行的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`——查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`——注意其中的 `applyTo` frontmatter，它将这些说明限定用于 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 问题分配给云端代理时，注意查看生成的拉取请求中是否包含 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容都来自指令文件。后续练习检查 PR 时还会再次提到。

## 为 Copilot 云端代理设置开发环境

无论由谁编写代码，通常都需要特定环境，并运行一些设置脚本，确保所有内容处于良好状态。将任务分配给 Copilot 也是如此，因为它会以类似软件工程师（SWE）的方式完成工作。

云端代理使用 [GitHub Actions][github-actions] 作为工作环境。你可以创建专用的[设置工作流][setup-workflow]，在 `.github/workflows/copilot-setup-steps.yml` 中进行配置，并在云端代理开始工作前运行，以便它能访问所需的开发工具和依赖项。为了保证实验流程顺利并提供学习机会，项目已预先配置好此工作流。它会确保 Copilot 能访问 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及已迁移并填充数据的单 Astro 应用 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可从存储库的 "Actions" 选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为*你的步骤*设置尽可能低的所需权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如为了安装依赖项），需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，Copilot 会在步骤完成后自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在使用 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

它看起来与其他 GitHub 工作流文件类似，但有几个关键点：

- 它只包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求之前，GitHub Actions 会运行这个作业。
- 注意 `workflow_dispatch` 触发器。它允许你从存储库的 **Actions** 选项卡手动运行工作流。这有助于测试工作流是否成功，而不必等待 Copilot 触发运行。

## 添加文档

大家都知道文档的重要性，但大多数项目中的文档要么已经过时，要么完全缺失。这类技术债务经常得不到处理，降低了工作效率，也增加了维护代码库或帮助新开发者加入团队的难度。Copilot 很擅长生成文档，因此这是分配给 Copilot 云端代理的理想问题。它会在后台生成所需文档。后续练习中，你会回来检查它的工作。

1. 在新浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新建问题对话框。
4. 选择 **Blank issue**，创建新问题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建问题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给问题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚生成的拉取请求（PR），标题类似于 `[WIP]: Code lacks documentation`。如果列表中没有出现新 PR，请稍等一两分钟并刷新浏览器窗口。
12. 几分钟后，Copilot 应该会创建一个待办任务列表。

> [!NOTE]
> Copilot 在 PR 中生成待办列表可能需要几分钟。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 检查任务列表和即将执行的任务。
14. 向下滚动拉取请求时间线，你应该会看到 Copilot 已开始处理问题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 你可能需要刷新窗口才能看到更新后的指示器。

16. 注意，你可以滚动查看实时会话，了解 Copilot 如何解决问题，包括探索代码并了解当前状态、暂停思考并确定合适的计划，以及编写代码。

这可能需要几分钟。Copilot 云端代理的主要目标之一是异步执行任务，让我们可以专注于其他工作。现在利用这个功能：先为 Copilot 云端代理分配另一项任务，然后我们转而为应用编写代码和添加功能。

## 在游戏详情页添加相关游戏部分

正如前面提到的，GitHub Copilot 云端代理的一大优点是可以拆分工作，让你和代理分别专注于不同任务。即使为设计团队添加相关游戏部分花费的时间不长，这段时间也可以用于其他工作。现在把任务分配给 Copilot 云端代理！

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新建问题对话框。
4. 选择 **Blank issue**，使用空白模板。
5. 将 **Title** 设置为：`Show related games on the game details page`
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建问题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给问题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，你会看到问题的第一条评论旁边出现一组 👀，表示 Copilot 已经开始处理！

![Copilot 使用眼睛表情符号表示正在处理问题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在认真处理你的第二个请求！Copilot 云端代理的工作方式类似软件工程师，因此你无需主动监视它，可以等它完成后再检查。现在来了解如何创建和使用自定义代理。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云端代理][copilot-agents]。使用云端代理，可以将问题分配给 Copilot 异步处理。你可以让 Copilot 解决技术债务、创建新功能，或协助将代码迁移到其他框架。

本课介绍了以下概念：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 明确界定问题范围的重要性。
- 将问题分配给 Copilot。

云端代理正在后台认真工作，现在可以开始创建和使用自定义代理。[Copilot 云端代理也可以使用 MCP 服务器][cloud-agent-mcp]，并且可以使用之前模块中介绍的自定义指令。

## 资源

- [关于 Copilot 云端代理][copilot-agents]
- [将 GitHub 问题分配给 Copilot][assign-issue]
- [Copilot 云端代理设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义代理 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
