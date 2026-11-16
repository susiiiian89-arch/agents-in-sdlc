---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理代理][previous-lesson] |
|:--|

## 检查工作

在整个实验中，你与 GitHub Copilot 一起完成了多项任务，以改善用户体验并添加功能。你让 Copilot 为代码添加文档、为设计团队构建可供迭代的相关游戏功能，以及实现包括高对比度和浅色模式切换开关在内的无障碍功能。现在检查代码修改；如果需要，还可以向 Copilot 提供反馈，进一步完善工作。

### 场景

正如前面多次强调的那样，引入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要检查生成的代码，并遵循正常的 DevOps 流程。基于这一点，先检查 GitHub Copilot 关于添加文档、相关游戏功能和无障碍功能的建议，然后再允许团队其他成员进行审查。

## 安全和 GitHub Copilot 云端代理

Copilot 云端代理异步且无人监管地执行任务，因此设置了一些安全限制，确保整个流程安全。这些限制包括：

- Copilot 对存储库只有读取权限，对它用于编写代码的分支才有写入权限。
- 云端代理在 GitHub Actions 中运行，并创建独立的临时环境进行工作。
- 任何 GitHub Actions 工作流在运行前都需要人工批准。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括对 MCP 服务器的访问。

## 检查生成的文档

先查看 GitHub Copilot 云端代理生成的第一个拉取请求（PR），也就是为代码添加文档。你将使用 GitHub.com 上的标准 PR 界面完成此任务。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括调用外部 MCP 服务器。如果需要，可以[自定义或禁用 Copilot 云端代理防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 的拉取请求，也可能是更完整的标题。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]** 标记。如果是这样，请等待 Copilot 完成工作。这可能需要几分钟，你可以稍作休息，或回顾目前学到的内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡，检查修改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新更新的代码，其中包括新生成的 TSDoc 文档注释和其他文档。具体修改内容可能有所不同。

    浏览修改时，检查 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自你在练习 2 开始时查看的自定义指令文件。

6. 检查更新并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该会看到一些工作流正在等待批准的指示器。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 你应该会看到工作流在拉取请求检查区域中排队运行。如果一切正常，单 Astro 应用的项目检查将通过。这可能需要几分钟。

## 请求 GitHub Copilot 修改

与 Copilot 协作并非单向过程。你可以像标记其他团队成员一样，在拉取请求评论或代码内联评论中标记 Copilot。Copilot 会读取这些评论，并启动新的会话来处理它们。由于结果具有非确定性，我们无法给出固定的请求文本。可以要求 Copilot 更新以下内容：

- 在每个代码文件顶部添加简要说明其用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用的结构。

1. 添加评论，请求修改生成的文档，并像标记普通用户一样标记 **@copilot**。可以使用上面的任一建议，也可以提出其他你希望代码库中包含的文档要求。
2. 选择 **View Session**，观察 Copilot 完成工作。注意 Copilot 会启动新会话来进行修改。
3. 选择 **Back to pull request** 返回拉取请求。

    ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成修改后，拉取请求中应该会出现新的提交。
5. 选择 **Files changed** 选项卡，检查修改。

你可以继续迭代，直到满意为止。满意后，可以将 PR 从草稿状态改为准备审查，然后合并到 main 分支。

![将 PR 改为准备审查](../../../_images/ex7-ready-for-review.png)

## 检查相关游戏功能

现在返回 Copilot 为解决“在游戏详情页显示相关游戏”问题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR，也可能是更完整的标题。
4. 选择 **Files changed** 选项卡，检查生成的代码。
5. 检查修改并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
6. 你应该会看到一些工作流正在等待批准的指示器。
7. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

8. 你应该会看到工作流在拉取请求检查区域中排队运行。如果一切正常，单 Astro 应用的项目检查将通过。这可能需要几分钟。
9. **可选：**你也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 和终端，运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已经构建了相关游戏功能！和之前一样，你可以通过迭代方式让 Copilot 云端代理继续修改。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释（请记住，分配此任务时，你还没有更新自定义指令！）。和之前一样，可以在 **Conversation** 选项卡中添加评论来提出请求，Copilot 会看到评论并启动新会话。

## 检查无障碍功能

最后，检查使用无障碍自定义代理实现的功能。此 PR 应该同时包含你在练习 3 中分配的高对比度模式，以及你在练习 4 的任务控制中心中提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR，也可能是更完整的标题。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]** 标记。如果是这样，请等待 Copilot 完成工作。这可能需要几分钟。

4. 选择 **Files changed** 选项卡，检查生成的代码。
5. 检查实现，特别留意：
   - 用于切换模式的 UI 组件。
   - 使用本地存储保存用户偏好。
   - 高对比度和浅色模式的 CSS 或样式修改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 用于管理模式切换的 JavaScript/TypeScript 代码。

6. 检查修改并确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 你应该会看到一些工作流正在等待批准的指示器。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 你应该会看到工作流在拉取请求检查区域中排队运行。如果一切正常，单 Astro 应用的项目检查将通过。这可能需要几分钟。
10. **可选：**你可以在 Codespace 中切换到此分支，手动测试无障碍功能。打开 Codespace 和终端，运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，并在浏览器中测试高对比度和浅色模式切换开关，确认功能符合预期，并且设置会在页面重新加载后保留。

请注意，无障碍自定义代理如何引导 Copilot 按照无障碍最佳实践实现这些功能。如果发现任何无障碍问题或改进点，可以像处理之前的 PR 一样，在评论中标记 **@copilot**，请求更新。

## 可选练习——继续委派任务

当你可以把真实待办任务交给云端代理，自己继续处理其他工作时，它的效果最佳。为了养成这种习惯，可以在存储库中再创建几个问题并分配给 Copilot。以下是一些想法：

- 在游戏详情页创建支持者兴趣表单。
- 在游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云端代理练习。在这些课程中，你：

- **检查了此存储库附带的自定义指令文件**，以便之后观察它们对云端代理输出的影响。
- **将问题分配给 Copilot 云端代理**，并观察它设置环境、制定计划并异步执行任务。
- **创建并使用自定义代理**处理无障碍功能，添加高对比度和浅色模式切换开关。
- **将 Copilot 代理页面用作任务控制中心**，在无障碍会话进行中监视并引导代理。
- **检查并迭代改进云端代理的拉取请求**，标记 `@copilot` 请求修改并批准工作流。

## 回顾和后续步骤

你已完成云端代理练习。如果想继续探索，其他练习可以补充你在这里实践的内容：

- 🖥️ **[VS Code 练习](../../vscode/)**——在 IDE 中探索 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 练习](../../cli/)**——在终端中交付两项经过审查的修改，使用 Plan 和 Autopilot 模式、代理技能、自定义 QA 代理、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云端代理连接。

在自己的存储库中，还可以尝试以下后续任务：

- 将重构或测试覆盖率问题分配给云端代理。
- 为其他领域创建自定义代理。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 代理][copilot-agents]
- [将 GitHub 问题分配给 Copilot][assign-issue]
- [Copilot 云端代理设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云端代理防火墙][agent-firewall]

---

| [← 上一课：管理代理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
