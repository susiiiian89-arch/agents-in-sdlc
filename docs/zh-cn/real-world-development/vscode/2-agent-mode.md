---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。作为开发者，你的工作流程通常包括：找到所有相关文件、进行修改、运行测试、调试、发现遗漏的文件、再次修改……类似步骤不胜枚举。

Copilot 代理模式正是为此而生。

Copilot 代理模式可以在 IDE 中更自主地工作。它的行为类似开发者：先探索现有项目结构，再执行必要的修改、运行测试等任务，并自动修复发现的问题。现在来了解如何使用代理模式为网站添加新功能。

> [!NOTE]
> 虽然名称相似，但代理模式和云端代理适用于两种不同的体验。代理模式在 IDE 中执行任务，便于快速反馈和交互。云端代理则像团队中的编程伙伴一样异步工作，通过问题和拉取请求与你协作。

本练习将介绍如何：

- 使用 Copilot 代理模式探索项目、识别相关文件并协调修改。
- 使用 GitHub Copilot 代理模式跨 UI 和数据层实现新功能。
- 在将代码合并到代码库之前，检查 Copilot 代理模式生成的修改和测试。

## 场景

游戏列表不断增长，你希望允许用户按类别和发行商筛选。你已在上一练习中添加了发行商辅助函数，现在将使用 Copilot 代理模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行任何修改前，先体验 Tailspin Toys 网站，了解它目前的功能。

该网站是一个采用开发者主题的桌游众筹平台。用户可以浏览游戏列表和详情。网站是一个单独的 Astro 应用，会在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能则通过小型、作用域受限且使用标准 DOM API 的 Astro `<script>` 添加。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按照以下步骤，可以在 GitHub Codespace 中运行该任务：

1. 返回 Codespace。上一练习结束后，你应该已经切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开新的终端窗口。
3. 创建并切换到筛选功能的新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，你会看到类似以下内容的横幅，其中显示服务器 URL：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否要在浏览器窗口中打开 `http://localhost:4321`，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 Codespace 时，在终端中选择 localhost URL 会自动将你重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 Codespace 的私有隧道，网站服务器现在就在其中运行！

### 浏览网站

网站运行后，你可以体验其功能。网站主要功能包括：

- **主页**：显示桌游列表，包括标题、图片和描述。
- **游戏详情页**：选择游戏后，会打开详情页，显示标题、描述、发行商和类别等信息。

## 使用 Copilot 浏览待办列表

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板，并选择 **New Chat** 开始干净的新对话。不同练习的模式和模型选择可能不同，相关步骤会在适用时说明。

网站的初始实现已经可以运行，但我们还想添加新功能。先查看待办列表。你从模板创建项目时，系统已经自动创建了一组问题；现在让 GitHub Copilot 显示这些问题。

1. 在 Chat 视图的代理下拉菜单中选择 **Agent**。**Agent** 代理可以自主规划并跨文件实现修改、运行终端命令和调用工具。

   ![Chat 视图中代理选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并非在推荐某个模型。编写本工作坊时使用了 Claude Sonnet 4.5，因此将它写入了说明，希望代码建议相对一致，从而带来良好的体验。不过，由于 LLM 具有概率性，你可能会收到与工作坊描述不同的建议。这完全正常，符合预期。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应该能够完成任务。

3. 向 Copilot 发送以下提示，询问待办问题：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有问题的命令。
5. 查看生成的问题列表。

请注意，Copilot 甚至会根据它认为对用户最有用的程度，为这些项目排定优先级。

## 检查指令文件

让代理生成代码之前，可以先检查用于向 Copilot 提供工作上下文的指令文件。我们将使用[用户界面（UI）指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中包含向网站添加功能时应遵循的指导。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 浏览添加功能时应遵循的总体指南，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍访问原则。
   - 指向各类文件专用说明的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指令文件可以引用其他指令文件和项目中的文件。路径相对于指令文件所在位置。这种方式便于复用内容，将较大的指令集拆分成较小且易管理的部分，并提供示例和模板。

## 实现筛选功能

完成筛选功能至少需要对应用进行三项独立修改：

- 在数据层（`src/lib/`）添加或完善剩余的筛选逻辑。
- 添加或更新筛选行为的测试。
- 更新游戏列表页面，加入筛选 UI。

此外，在将所有修改合并到代码库之前，还需要运行测试并确保通过。Copilot 代理模式可以代为处理这些任务！现在来添加筛选功能。

1. 你可以继续使用当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 指令文件。

   ![选择 UI 指令文件的示例屏幕截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的代理下拉菜单中仍然选中 **Agent**。

   ![Chat 视图中代理选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍然选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，让 Copilot 根据待办列表中的相关问题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目、查找与功能相关的文件。你会看到它找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 正在探索项目文件的屏幕截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> 你会看到 Copilot 执行多项任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项修改都会显示 **Keep** 或 **Undo** 选项，因此不必在工作进行中选择。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 面板中要求用户确认运行测试的对话框屏幕截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会出现暂停，也可能有测试暂时失败。这没有关系！Copilot 会在生成代码和运行测试之间反复迭代，直到完成任务且未检测到错误。

   ![包含完整 Copilot 代理模式会话的 Chat 屏幕截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查看是否有潜在问题。

> [!CAUTION]
> 请记住，务必检查 Copilot 或其他 AI 工具生成的代码。

10. 返回运行着网站的浏览器，体验新功能！
11. 确认一切正常并检查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加新功能。我们了解了如何：

- 使用 GitHub Copilot 代理模式跨 UI 和数据层实现新功能。
- 使用 Copilot 代理模式探索项目、识别相关文件并协调修改。
- 在将代码合并到代码库之前，检查 Copilot 代理模式生成的修改和测试。

现在，让我们[使用 Playwright MCP 服务器测试功能][next-lesson]，并为其打开拉取请求。

### 加分探索练习——实现分页

游戏列表不断增加后，需要支持分页。运用本练习中学到的技能，提示 Copilot 更新网站以实现分页。编写提示时可以考虑：

- 遵循现有最佳实践，包括使用已有的指令文件。
- 思考如何实现分页，以及是否允许用户选择每页大小，还是使用固定值。
- 编写提示时，提供 Copilot 所需的指导，确保实现符合你的要求。
- 你可能需要与 GitHub Copilot 多次交互，要求它修改并提供上下文。这是使用 Copilot 时的正常工作流程！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能和使用时机][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
