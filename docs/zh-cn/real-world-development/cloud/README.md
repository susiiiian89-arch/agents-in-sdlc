---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot 云端代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot 云端代理](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** 可以在云端异步处理工作。你在 GitHub 上分配任务后，云端代理会在后台接手工作——探索存储库、进行修改并打开拉取请求——同时你可以继续处理其他事情。

在这些练习中，你将添加云端代理会遵循的自定义指令，然后分配 GitHub 问题，让它实现相应工作。你将检查并使用自定义代理来引导它的工作方式，在代理仪表板中监视并引导会话，最后检查它创建的拉取请求并迭代改进结果。

## 练习

| 练习 | 主题 | 描述 |
|----------|-------|-------------|
| [0. 准备工作][ex0] | 设置 | 创建存储库和 Codespace |
| [1. 自定义指令][ex1] | 上下文 | 添加云端代理将遵循的自定义指令 |
| [2. 云端代理][ex2] | 异步代理 | 将问题分配给 Copilot 云端代理 |
| [3. 自定义代理][ex3] | 专用代理 | 检查并使用自定义代理 |
| [4. 管理代理][ex4] | 监视 | 监视并引导代理会话 |
| [5. 迭代改进][ex5] | 审查 | 检查 PR、迭代改进 Copilot 的工作并选择后续步骤 |

## 先决条件

参加本工作坊前，请确保具备以下条件：

- [ ] 拥有有效的 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 方案的 GitHub 帐户
- [ ] 已为你的帐户或组织启用 Copilot 云端代理

> [!TIP]
> 没有付费方案？通过[教育版 GitHub][callout-student-plan-education]，经验证的学生可以免费使用 GitHub Copilot。**Copilot Student** 方案包含本工作坊所用的代理、MCP、代码审查和 Copilot CLI 功能，因此可完成所有练习。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Copilot Student、Pro、Pro+、Business 和 Enterprise 方案均可使用云端代理；Copilot Free 除外。对于 Copilot Business 或 Enterprise，管理员必须为组织启用此功能。

> [!NOTE]
> 本练习不涉及 MCP。要了解如何在 Copilot 中使用 MCP 服务器，请参阅 [CLI 练习][cli-harness]或 [VS Code 练习][vscode-harness]。

## 开始

**[从练习 0：准备工作开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
[cli-harness]: ../cli/
[vscode-harness]: ../vscode/
