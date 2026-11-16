---
title: "练习 4 - 监视和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几节练习中，你让 Copilot 云端代理处理了三项独立任务，用于改进用户体验和添加功能。云端代理旨在异步自主工作，但监视这些任务也很重要。

你可以使用多种工具管理分配给云端代理的任务，包括 GitHub.com 上的[代理页面][agents-page]。在这个任务控制中心，你可以查看所有创建了未关闭拉取请求（PR）的代理任务。你可以查看代理执行的操作，甚至可以引导正在进行的会话。

本课将完成以下内容：

- 浏览代理页面，监视云端代理任务。
- 引导正在进行的会话，请求添加其他功能。

## 场景

将任务分配给代理以创建高对比度模式后，团队意识到同时添加浅色模式也是个好时机。网站样式和切换功能已经在修改，因此一并加入这项功能很合理。现在引导代理工作，确保它同时添加浅色模式和高对比度模式。

## 查看 Copilot 云端代理任务

查看当前分配给 Copilot 云端代理的所有任务的状态。

1. 打开 [Copilot 代理页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。你应该会看到分配给 Copilot 的任务，包括：
    - 更新代码库文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，检查 Copilot 已执行的工作。这些操作可能包括：
    - 从存储库检出代码。
    - 创建 Copilot 的工作环境。
    - 设置 MCP 服务器。
    - 执行完成分配任务所需的各项步骤。

> [!NOTE]
> 列出的具体步骤会根据 Copilot 的工作状态和采用的方法而有所不同。

4. 另外，注意右侧显示的拉取请求（PR）面板。你可以在此查看 PR 和已更改的文件，以便进一步监视。

## 引导云端代理

现在你已经查看了正在进行的任务，可以在 Copilot 实现高对比度模式时，要求它添加浅色模式切换开关。

1. 选择与添加高对比度模式相关的会话。确切标题会根据 Copilot 使用的名称和当前工作状态而有所不同。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到会话显示已完成设置并开始工作。出现类似以下消息时，就说明已经开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，添加以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![代理页面中突出显示引导活动会话对话框的云端代理任务截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 如何确认收到提示并将其加入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务！它会先完成发送消息时正在执行的具体步骤，然后把新请求加入工作流程。

和之前一样，这需要几分钟。此时正好可以暂停一下，回顾目前学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 代理页面，这是监视和引导 GitHub Copilot 云端代理任务的中心位置。在这个任务控制中心，你可以跟踪所有活动和已完成的任务、检查正在执行的工作，甚至可以重定向正在运行的任务来调整范围或提供额外指导。

本课介绍了以下概念：

- 使用任务控制中心和代理页面监视云端代理任务。
- 重定向正在进行的会话，请求添加新功能。

Copilot 正在完成无障碍功能的工作。接下来，你将[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 代理页面][agents-page]
- [关于自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
