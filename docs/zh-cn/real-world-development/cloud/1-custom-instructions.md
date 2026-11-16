---
title: "练习 1 - 自定义指令（云端代理）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：准备工作][previous-lesson] · [下一课：Copilot 云端代理 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解一些背景信息，就要确保它能够获取相关上下文。[指令文件][instruction-files]用于提供此类指导，让 Copilot 不仅了解你希望它做*什么*，还了解你希望它*如何*完成工作。

本练习将完成以下内容：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义指令及按路径限定的指令文件提供给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 练习不同，这里不会运行修改指令前后的提示进行对比。Copilot 云端代理会异步处理 GitHub 问题，因此很难实时并排展示效果。稍后查看云端代理创建的拉取请求时，你会看到指令文件的影响。

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
3. 打开 `.github/instructions` 文件夹并查看其中的文件。你会看到适用于 Astro 文件、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段：它设置一个相对于存储库根目录的 glob，用来确定哪些文件适用。此处匹配任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）。
5. 查看此项目中创建单元测试的专用说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。留意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成较小的可复用文件，并为 Copilot 提供可遵循的示例。（其中的路径相对于指令文件所在位置，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但目前尚未要求代码中包含文档注释。接下来的步骤会添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来是本练习中唯一需要编辑文件的步骤：添加一条项目级规则，要求代码中包含 TSDoc 文档注释和文件级注释头。云端代理在本练习后续收到分配的问题时，会使用这条规则。

编辑前，先创建一个分支（云端代理会读取问题目标分支中的指令，但仍然要规范地提交你的修改）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 提交、推送并合并指令更新

云端代理会从问题的目标分支读取指令。下一练习中，你将把问题分配给 Copilot，Copilot 会从 `main` 创建分支。因此，指令修改必须先合并到 `main`，云端代理才能使用它。

1. 暂存并提交修改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并将其合并。下一练习中，云端代理处理分配给它的问题时就会读取这些指令。

> [!TIP]
> 如果你希望在本工作坊中直接使用 `main`，可以跳过分支，直接提交到 `main`。此处创建分支是为了让工作坊更贴近真实项目中的处理方式。

## 总结和后续步骤

你了解了 Copilot 如何从项目指令文件中获取上下文，并在 `.github/copilot-instructions.md` 中添加了新的存储库级标准。在本练习系列的后续部分，你会在云端代理生成的拉取请求中看到该标准的效果。

接下来，你将[把第一个问题分配给 Copilot 云端代理][next-lesson]。

## 资源

- [自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建有效自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
