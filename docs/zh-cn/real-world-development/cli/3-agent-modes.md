---
title: "第 3 课 - 代理模式：Plan 和 Autopilot"
description: "使用 Plan 确认方案，使用 Autopilot 根据问题实现筛选功能，并在 Interactive 模式下检查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们先为项目添加了一项小功能。但更大的修改需要更完善的流程。幸运的是，GitHub Copilot CLI 能够配合组织现有的工作流程，帮助我们以正确的方式构建正确的功能。这是多节课程中的第一节，你将按照典型的代理驱动开发流程，从问题出发生成新功能，确保代码有效、功能符合预期，最终成功合并到项目中。

> [!NOTE]
> 在继续完成此功能的工作时，你会沿用同一对话和分支。通常，不同文件类型的工作会使用不同分支或 PR；本课程为了让你专注于核心概念，采用了简化方式。

本课将完成以下内容：

- 从 GitHub 问题启动新的 Copilot 对话。
- 在 Plan 模式下确定需求。
- 使用 Autopilot 模式实现新功能。
- 检查代码。
- 在转发的浏览器中手动验证功能。

继续实现此功能时，你还将更新存储库指令、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 代理，并打开功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访问者需要按类别和发行商筛选游戏。待办问题描述了这项功能，但在编写代码前，还需要确认如何组合类别等细节。你将使用 Plan 模式解决这些问题，然后授权 Autopilot 在明确范围内实现功能。

## 背景

在开发流程中引入 AI 编码代理，并不会改变基本原则。事实上，这些原则变得更加重要！大多数开发者遵循的流程大致如下：

1. 从描述待完成工作的已提交问题开始。
2. 制定要构建内容的计划。
3. 编写并检查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码经过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但大多数流程都与上述步骤类似。

遵循这种标准流程，可以确保 AI 生成的代码符合既定要求，并像手写代码一样经过审核。

## 对话模式

**对话模式**控制代理的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在不同模式间切换：

- **Interactive**：你与代理协作。代理提出修改建议，并等待你输入后再继续。
- **Plan**：代理先制定计划，但不能编辑项目文件。
- **Autopilot**：代理自主工作——编写代码、运行测试并反复迭代，直到任务完成。

先从 Plan 模式开始，检查计划，然后使用 Autopilot 实现计划。

## 从问题开始

开始筛选功能的工作之前，请返回 Codespace，并确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有问题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的问题。
9. 按 <kbd>C</kbd> 将问题添加到提示中，然后返回 **Session** 选项卡。

现在你会看到提示以 `#7`（或类似编号）开头。`#` 可将 GitHub 上的问题或拉取请求（PR）加入上下文。

## 规划筛选功能

规划让你有机会在交给 Copilot 前，先确定实现功能或执行任务的方法。对于复杂工作，花些时间规划通常很有帮助。现在切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的问题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会开始制定计划！它会先探索项目，再确定合适的实现方式。

3. 过程中，Copilot 可能会询问筛选功能的具体行为。根据你的偏好作答，没有错误答案！
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动检查计划。
6. 如果计划中的任何部分不符合你的决定，请让 Copilot 修改。

## 批准 Autopilot

计划编写并检查完毕后，就可以开始实现了！让 Copilot 使用 Autopilot 完成工作。

Autopilot 会让 Copilot 持续处理问题，直到它认为任务已经完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或你安装的版本中名称相近的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 如何按照已确定的计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先检查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 检查并验证实现

代码生成后，和其他代码一样，合并前需要先检查。现在一起检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 对照问题和规划时做出的决定检查结果。
4. 检查完代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 检查项目的验证输出，并要求 Copilot 解决任何失败。

## 体验新功能

好的，代码看起来没问题——但它能运行吗？像之前一样启动应用，通过 Codespaces 转发端口打开网站。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. 当 Codespaces 报告端口 `4321` 可用时，选择 **在浏览器中打开**。
3. 尝试按类别筛选、按发行商筛选，以及计划中确定的组合方式。
4. 确认重置和无结果时的行为符合问题要求和你的决定。
5. 返回 Codespace，并要求 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你已使用不同的对话模式构建并检查了一项功能。本课中，你：

- 从 GitHub 问题启动了新的 Copilot 对话。
- 在 Plan 模式下确定了需求。
- 使用 Autopilot 模式实现了新功能。
- 检查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，我们将[使用自定义指令][next-lesson]深入了解如何生成符合文档规范的代码。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
