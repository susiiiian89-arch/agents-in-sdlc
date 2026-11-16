---
title: "第 2 课 - 添加星级评分：快速改进"
description: "使用 Copilot CLI 修改游戏卡片，在转发的浏览器中检查结果，并将其作为你的第一个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过对话后，现在可以开始对项目进行第一次修改了。先从小处着手：游戏数据中已经有星级评分，但首页上的游戏卡片还没有显示它。你将让代理显示评分、检查修改，并将其作为你的第一个拉取请求合并。

本课将完成以下内容：

- 在功能分支上启动一次专注于任务的 Copilot 对话。
- 让代理对项目进行小幅修改。
- 使用 `/diff` 检查修改。
- 运行应用，并在转发的浏览器中确认修改。
- 创建并合并你的第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，而且游戏详情页已经显示了评分。不过，首页上的游戏卡片只显示标题、类别、发行商和描述。作为热身练习，你将让代理在每张卡片上显示现有评分。这是一个小而独立的修改，非常适合首次使用。

## 对话的组成

**对话**是你使用 Copilot CLI 处理任务的地方。与 Copilot 应用不同，普通 CLI 对话使用终端当前检出的存储库和 Git 分支，而不是创建专用工作树。保存的对话可以让你稍后回到同一讨论，而文件和分支仍是磁盘上的普通 Git 状态。

在对话中，你会看到三类内容：你的提示和代理的回复、代理探索和编辑文件时的工具活动，以及可通过 `/diff` 检查的修改。

## 开始对话并请求修改

让我们开始一个新对话，实现这个功能。

1. 返回 Codespace。
2. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未启动，请运行以下命令：

   ```bash
   copilot --yolo
   ```

4. 使用斜杠命令 `/new` 并按 <kbd>Enter</kbd>，确保已启动新会话。
5. 使用以下提示请求修改：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 会探索项目，找到用于显示游戏详情的文件，并创建所需代码。现在，你已通过 Copilot CLI 添加了一项新功能！

## 检查差异

所有 AI 生成的修改在合并前都应检查，即使是很小的修改也不例外。现在就在 Copilot CLI 中查看修改。

1. 输入 `/diff` 并检查每个已修改的文件。
2. 确认游戏卡片在有评分时显示数字评分，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种状态。
4. 检查 Copilot 运行的检查结果，并要求它修复任何失败。
5. 检查完成后，按 <kbd>Esc</kbd> 退出差异界面。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 具有概率性，而不是确定性的，因此你得到的代码可能有所不同。请检查实际行为，而不是期待某种固定实现。

## 检查修改

当然，我们不能只读代码就假定一切正常。让 Copilot 启动网站，以便我们在 Codespaces 转发的浏览器中检查更新后的用户界面（UI）。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. 当 Codespaces 报告端口 `4321` 可用时，选择 **在浏览器中打开**。
3. 确认游戏卡片显示了满分为五分的评分。
4. 返回 Copilot，并要求它停止启动的服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 创建并合并你的第一个拉取请求

现在你已经创建了这个功能！接下来创建拉取请求（PR），将新代码合并到项目中。

1. 让默认代理提交修改：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。Copilot CLI 创建 PR 时会推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Copilot CLI 显示的 URL，以打开 PR。
4. 检查已更改的文件和检查结果。
5. 准备就绪后，选择 **合并拉取请求**，然后确认合并。
6. 返回 Codespace，并使用 `/exit` 退出 Copilot CLI。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结和后续步骤

恭喜！你使用 GitHub Copilot CLI 发布了第一个修改！具体来说，你已经：

- 在功能分支上启动了一次专注于任务的 Copilot 对话。
- 引导代理修改了游戏卡片。
- 使用 `/diff` 检查了修改。
- 运行应用，并在转发的浏览器中确认星级评分。
- 创建并合并了第一个拉取请求。

接下来，你将[从筛选问题开始，使用 Plan 和 Autopilot 模式][next-lesson]来构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
