---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用智能体模式构建了筛选功能。在创建拉取请求前，应先确认它能在浏览器中正常工作。与其手动浏览应用，不如连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器为你测试功能，然后发布分支并创建 PR。

本练习将：

- 了解 Model Context Protocol (MCP) 是什么，以及 MCP 服务器如何为 Copilot 扩展新工具。
- 将 Playwright MCP 服务器添加到工作区。
- 请 Copilot 使用该服务器在浏览器中手动测试筛选功能。
- 发布分支，并为筛选功能创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

当智能体模式能够访问编辑器之外的内容时，其能力会大幅增强。Copilot 使用 Model Context Protocol (MCP) 与外部工具和服务通信。它是智能体与外部工具及服务交互的一种标准方式。

![智能体模式内部工作原理及其与上下文、LLM 和工具的交互示意图，包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 智能体提供了与外部工具和服务通信的方式。借助 MCP，AI 智能体可以实时与外部工具和服务通信，通过资源获取最新信息，并通过工具代表你执行操作。

这些工具和资源通过 MCP 服务器访问。MCP 服务器是 AI 智能体与外部工具和服务之间的桥梁，负责管理双方之间的通信，例如连接已有 API 或 NPM package 等本地工具。每个 MCP 服务器都提供一组不同的工具和资源，供 AI 智能体使用。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供用于管理 GitHub 存储库的 API，让 AI 智能体可以创建和更新存储库、管理议题和拉取请求等。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：提供基于 Playwright 的浏览器自动化能力，让 AI 智能体可以浏览网页、填写表单和选择按钮等。

还有许多其他 MCP 服务器，可用于访问不同的工具和资源。GitHub 托管了 [MCP 注册表](https://github.com/mcp)，帮助提升生态系统的可发现性并促进贡献。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前，请仔细审查其源代码、验证发布者并评估安全影响。只使用可信的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区中可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 你应该会看到已经为你配置的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

该 `github` 配置由项目模板提供，因此 Copilot 能在上一练习中读取待办事项。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装任何内容。VS Code 通过 HTTP 连接服务器，并在 Copilot 首次使用其工具时使用 GitHub 进行身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，正好可以用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 配置，与 `github` 并列，使文件如下所示：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标志指示 Playwright 在无可见窗口的情况下运行。由于 codespace 没有可显示浏览器的桌面环境，因此必须使用此标志。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 进行端到端测试，因此 Playwright 所需的浏览器通常已经安装。如果 Copilot 之后报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器：Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 配置上方会出现 **Start** 操作。选择它以立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问是否信任此服务器，请检查配置并选择信任，以便启动服务器。

手动启动是可选的。如果跳过此步骤，VS Code 会在下一步中 Copilot 首次需要服务器工具时启动它，并询问你是否信任。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可驱动的真实浏览器。无需手动浏览应用，智能体可以打开页面、导航、应用筛选条件并读取结果，然后向你总结观察结果。这是在不离开对话的情况下，快速确认功能行为是否符合预期的方法。

Playwright MCP 服务器依靠页面的[无障碍树][playwright-mcp-server]而不是截图工作。这意味着智能体会像辅助技术一样，根据带标签的结构化元素（按钮、链接、列表项）进行推理，因此快速的功能检查也可以充当轻量级的无障碍合理性检查。

服务器已连接且应用正在运行时，请 Copilot 测试刚完成的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、逐步完成检查，并报告结果。根据议题中的验收标准审查总结。如果发现异常，可以继续提问，或在创建拉取请求前让 Copilot 修复代码。

> [!NOTE]
> 本测试要求应用在 `http://localhost:4321` 运行。如果已停止开发服务器，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器；如果它报告缺少浏览器，请让它运行 `npx playwright install chromium` 后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求 (PR)，让团队审查。首先发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示更改内容的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过多种方式创建拉取请求，例如在 github.com 上创建，或使用 GitHub 命令行界面 (CLI)。既然你正在使用 GitHub Copilot，不如让它代为创建 PR！它可以找到相关议题并将 PR 与议题关联起来。

1. 打开 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 确认在智能体下拉菜单中选择了 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 请 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 收集信息并执行操作。它首次使用 GitHub 工具时，可能会提示你进行 GitHub 身份验证；按照提示授权即可。
5. 观察 Copilot 搜索议题、找到正确议题并创建 PR。
6. 选择 Copilot 生成的链接审查拉取请求，但暂时**不要合并**。

## 总结与后续步骤

恭喜！本练习中，你：

- 了解了 Model Context Protocol (MCP) 是什么，以及 MCP 服务器如何为 Copilot 扩展新工具。
- 将 Playwright MCP 服务器添加到了工作区。
- 使用该服务器在发布前通过浏览器手动测试筛选功能。
- 发布分支并创建了筛选功能拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义智能体][next-lesson]，以便简化无障碍审查等专门任务。

## 资源

- [MCP 到底是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
