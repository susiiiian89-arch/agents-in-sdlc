---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：准备工作][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解一些背景信息，就要确保它能够获取相关上下文。你可以通过多种方式向 Copilot 提供特定上下文，其中一种重要方式是使用[指令文件][instruction-files]，为代码生成提供指导。

本练习将完成以下内容：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义指令及按路径限定的指令文件提供给 Copilot。
- 使用当前说明生成筛选功能的第一个数据切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 再次运行相同提示，观察生成的代码如何遵循新标准。
- 将指令更新和筛选功能切片提交到 `main`，让 Copilot 在下一练习中使用更新后的指南。

> [!CAUTION]
> 生成的代码可能不符合你设定的某些标准。Copilot 具有非确定性。本练习的目的是观察更新说明后行为的*整体变化趋势*，而不是让输出与某种范例逐字相同。

## 指令文件

### 场景

像所有优秀的开发团队一样，Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加一段注释，说明该文件的作用。

通过使用指令文件，可以确保 Copilot 获取正确的信息，并按照上述实践完成任务。

### 自定义指令

自定义指令可以为 Copilot 提供上下文和偏好，帮助它更好地理解你的编码风格和要求。它是一种强大的功能，可以引导 Copilot 提供更符合需求的建议和代码片段。你可以指定偏好的编码规范、库，甚至希望代码包含哪类注释。你可以为整个存储库创建指令，也可以针对特定文件类型创建指令，为任务提供上下文。

指令文件有两种类型：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。文件应包含项目级信息，即适用于 Copilot 大多数聊天或 CLI 请求的上下文，例如技术栈、项目概述、最佳实践和全局指南。
- `.github/instructions/*.instructions.md`：可以为特定任务或文件类型创建这些文件。你可以为特定语言（如 TypeScript 或 Astro），或特定任务（如创建 UI 组件或一组单元测试）提供指南。

> [!NOTE]
> 在 IDE 中使用时，指令文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云端代理在生成代码时都会使用存储库级说明和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本工作坊的范围。不过，示例项目展示了一种有代表性的方法。总体而言：

- 将 `copilot-instructions.md` 中的指令聚焦于项目级指导，例如项目概述、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（单元测试、Astro 组件、数据层）或特定任务提供指令。
- 使用自然语言，保持指导清晰，并提供代码示例，说明代码应该和不应该采用什么形式。

创建指令文件没有唯一正确的方法，使用 AI 也是如此。通过实践，你会找到适合自己项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一套完善的指令文件。查看本项目中的文件时，你可能会发现有些文件针对多种任务，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮你生成指令文件。不同界面提供的操作方式各有不同（例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）。当前界面的课程会在相关处介绍具体方式。
>
> 想找模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义指令文件

花些时间阅读此存储库附带的指令文件：一个核心 `copilot-instructions.md` 文件，以及一组用于不同任务的 `*.instructions.md` 文件。你可以在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，了解项目简介及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 下，留意嵌套的 **GitHub Actions Workflows** 指南。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并查看其中的文件。你会看到适用于 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段：它设置一个相对于存储库根目录的 glob，用来确定哪些文件适用。此处匹配任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）。
5. 查看此项目中创建单元测试的专用指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。留意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成较小的可复用文件，并为 Copilot 提供可遵循的示例。（其中的路径相对于指令文件所在位置，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但目前尚未要求代码中包含文档注释。接下来的步骤会添加 TSDoc 文档注释和文件注释头的规则。

## 为修改创建分支

现在按照最佳实践，为修改创建一个分支。

1. 返回上一练习中使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板，并选择 **New Chat** 开始干净的新对话。不同练习的模式和模型选择可能不同，相关步骤会在适用时说明。

要观察自定义指令的影响，先在当前指令文件的基础上发送提示。稍后更新指令后，再次发送相同提示并对比结果。

1. 关闭之前练习中打开的编辑器标签页，让 Copilot 只获取你想要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应该放在哪里。
3. 在 Chat 视图的代理下拉菜单中选择 **Agent**，让 Copilot 应用文件修改。

   ![Chat 视图中代理选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 注意，生成的辅助函数是一个带类型的函数，它将 `db` 客户端作为第一个参数，并返回带类型的发行商数组。这些内容来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意，生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，因此它有可能即使在没有明确指令的情况下也添加文档注释。如果发生这种情况也没关系，本练习的重点仍是观察添加指令后行为一致性的提升。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。我们来记录存储库编码标准，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，该部分应在第 27 行附近。注意其中记录了项目编码标准，但尚未包含代码文档规则，这正是生成的辅助函数缺少文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所见，指令文件既可以放在存储库级别（`.github/copilot-instructions.md`），为整个项目提供指导，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指令。刚才添加的文档注释规则属于项目级标准，因此存储库级文件是最合适的位置。

## 再次运行提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除上下文。
2. 再次选择 `src/lib/publishers.ts`，让 Copilot 关注正确的文件。
3. 发送与之前**相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意，现在生成的文件开头包含类似以下内容的注释块：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发行商数据访问辅助函数。
    * 提供从数据库检索发行商信息的函数。
    */
   ```

5. 注意，生成的函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * 返回所有发行商的 ID 和名称列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 解析为发行商对象数组的 Promise。
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将它应用到实际代码中，下一练习将在此基础上继续开展。

## 提交指令并推送分支

指令文件与存储库中的其他资源一样，需要通过源代码管理进行维护。现在将修改提交并推送到存储库。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），打开新的终端窗口。
2. 在终端中运行以下命令，确认指令更新和辅助函数修改都已完成：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新，以及 Copilot 为辅助函数基础所做的所有修改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他相关修改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，应该创建拉取请求，并将其关联到待办列表中的文档标准问题。我们可以手动完成，也可以让 Copilot 使用已连接到项目的 GitHub 工具代为处理。现在让 Copilot 查找问题、创建用于关闭该问题的 PR，然后合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上操作。项目模板已经将 GitHub 工具连接到工作区，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示授权即可。后续练习中会介绍这种连接的工作方式（模型上下文协议）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保在代理选择器中选中 **Agent**。
3. 要求 Copilot 查找与更新指令文件相关的问题，并从当前分支创建 PR，说明其中既更新了指令，也添加了新的发行商辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找问题并创建 PR。
5. Copilot 提示你**允许**它代表你执行 GitHub 操作时，请先检查操作命令，再酌情选择 **Allow**。
6. PR 创建后，使用以下提示要求 Copilot 合并 PR，并让本地分支切回 `main`：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 提示你允许它执行 GitHub 操作和 shell 命令时，请先检查命令，再酌情授权。

现在，你已在 GitHub Copilot 的帮助下创建并合并了拉取请求！

## 总结和后续步骤

你了解了 Copilot 如何从项目指令文件中获取上下文，并在 VS Code 中使用 Copilot Chat 完成了以下工作：

- 发送代码生成提示，并观察 Copilot 如何根据*现有*指令生成代码。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 再次发送相同提示，观察生成的代码如何遵循新标准。
- 将指令更新和辅助函数基础提交到 `main`，让下一练习可以继续使用。

接下来，你将使用[代理模式][next-lesson]落实这些指令，让 Copilot 跨代码库添加新功能。

## 资源

- [自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建有效自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 的个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
