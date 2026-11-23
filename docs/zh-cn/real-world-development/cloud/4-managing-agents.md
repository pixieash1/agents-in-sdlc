---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几节练习中，你让 Copilot 云智能体处理了三项不同任务，用于改善用户体验并添加功能。云智能体可以异步、自主地工作，但仍然需要监控这些任务。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个 mission control 中，可以查看所有创建了开放拉取请求 (PR) 的智能体任务。你可以探索智能体执行的操作，也可以引导正在进行的会话。

本课将：

- 探索智能体页面并监控云智能体任务。
- 引导正在进行的会话，要求添加其他功能。

## 场景

将任务分配给智能体以创建高对比度模式后，团队意识到也可以同时添加浅色模式。由于智能体已经在更新网站样式并添加切换功能，将此功能一并加入很合理。现在需要引导智能体，确保它同时添加浅色和高对比度模式。

## 查看 Copilot 云智能体任务

先查看分配给 Copilot 云智能体的所有任务的当前状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主区域和左侧区域中的任务列表。应能看到分配给 Copilot 的任务，包括：
   - 更新代码文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 检出存储库中的代码。
   - 为 Copilot 创建工作环境。
   - 设置 MCP 服务器。
   - 执行完成任务所需的各项步骤。

> [!NOTE]
> 具体显示的步骤因 Copilot 当前的工作状态和执行方式而异。

4. 同时查看右侧显示的 PR 面板，以便监控 PR 和更改的文件。

## 引导云智能体

你已了解当前活动任务，现在在 Copilot 实现高对比度模式期间，要求它同时添加浅色模式。

1. 选择描述添加高对比度模式的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

    ![mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它完成设置并开始工作。出现类似下方的消息时，就表示已开始。
3. 在 **Steer active session while Copilot is working** 对话框中，添加以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的云智能体任务截图，突出显示 steer active session while copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 会确认收到提示，并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务！在你发送消息时，它可能正在执行某一步；完成该步骤后，它会将新请求纳入工作流程。

与之前一样，这需要几分钟，正好可以暂停一下，回顾目前学到和探索的内容。

## 总结与后续步骤

本课介绍了 Copilot 智能体页面，即监控和引导 GitHub Copilot 云智能体任务的中心位置。通过这个 mission control，可以跟踪所有活动和已完成的任务、审查正在进行的工作，甚至重定向正在运行的任务以调整范围或提供额外指导。

你已探索以下概念：

- 使用 mission control 和智能体页面监控云智能体任务。
- 重定向正在运行的会话，要求添加其他功能。

Copilot 正在完成无障碍功能，接下来可以[迭代改进它创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
