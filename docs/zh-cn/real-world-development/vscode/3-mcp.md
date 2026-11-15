---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用智能体模式构建了筛选功能。创建拉取请求前，应先在浏览器中确认功能确实正常。与其自行逐页检查，不如连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器为你测试功能，然后发布分支并创建 PR。

本练习将学习如何：

- 了解 Model Context Protocol (MCP) 以及 MCP 服务器如何通过新工具扩展 Copilot。
- 将 Playwright MCP 服务器添加到工作区。
- 让 Copilot 使用 Playwright MCP 在浏览器中手动测试筛选功能。
- 发布分支，并为筛选工作创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

智能体模式接入编辑器以外的工具后，功能会强大得多。Copilot 通过 Model Context Protocol (MCP) 实现这一点——MCP 是智能体与外部工具和服务通信的标准方式。

![智能体模式内部工作原理及其与上下文、LLM 和工具的交互示意图，其中包括由 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 智能体提供了与外部工具和服务通信的方式。通过 MCP，AI 智能体可以实时与外部工具和服务通信，从而使用资源获取最新信息，并代表你使用工具执行操作。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，负责管理智能体与外部工具（例如现有 API 或 npm 包等本地工具）之间的通信。每个 MCP 服务器都提供一组 AI 智能体可访问的工具和资源。

以下是几个常见的 MCP 服务器：

- **[GitHub MCP 服务器](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API。AI 智能体可以用它创建新存储库、更新现有存储库，以及管理议题和拉取请求。
- **[Playwright MCP 服务器](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化功能。AI 智能体可以用它浏览网页、填写表单和选择按钮。

还有许多其他 MCP 服务器提供不同工具和资源。GitHub 提供了 [MCP 注册表](https://github.com/mcp)，帮助用户发现并参与生态系统。

> [!CAUTION]
> 从安全角度来看，应像对待项目的其他依赖项一样对待 MCP 服务器。使用前，请仔细审查其源代码、验证发布者，并考虑安全影响。只使用信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区中可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 你应该会看到已经配置好的 `github` 服务器：

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

`github` 条目随项目模板一起提供，因此上一练习中 Copilot 能够读取议题待办列表。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装任何内容——VS Code 通过 HTTP 连接到它，并在 Copilot 首次使用其工具时使用 GitHub 进行身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，正好可以用于测试功能。

1. 在 `.vscode/mcp.json` 中，在 `github` 旁边添加 `playwright` 条目，文件内容如下：

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

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标志会让 Playwright 在不显示窗口的情况下运行浏览器；Codespace 中没有桌面环境，因此必须使用此标志。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 执行端到端测试，因此通常已安装 Playwright 所需的浏览器。如果 Copilot 之后报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器：Copilot 首次需要使用服务器工具时，VS Code 会启动服务器，并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它以立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请审查配置并选择信任，以便启动服务器。

手动启动是可选的。如果跳过此步骤，下一步中 Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可驱动的真实浏览器。无需自行在应用中逐项操作，智能体可以打开页面、导航、应用筛选条件、读取结果并总结观察到的情况。无需离开对话即可快速确认功能是否符合预期。

Playwright MCP 服务器在底层使用页面的[无障碍树][playwright-mcp-server]，而不是屏幕截图。这样，智能体可以像辅助技术一样，根据带标签的结构化元素（按钮、链接、列表项）进行推理，因此快速的功能检查也可以作为简单的无障碍检查。

连接服务器并启动应用后，让 Copilot 测试刚刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、逐项测试并报告结果。根据议题中的验收标准审查报告；如果发现异常，提出后续问题，或让它修复代码后再创建拉取请求。

> [!NOTE]
> 测试期间，应用必须在 `http://localhost:4321` 运行。如果已停止开发服务器，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium` 并重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求 (PR)，供团队审查了。首先发布 `filtering-vscode` 分支。

1. 在 Codespace 中打开 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示更改内容的 Source Control 面板屏幕截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过多种方式创建拉取请求，例如使用 github.com 或 GitHub 命令行界面 (CLI)。既然已在使用 GitHub Copilot，不妨让它代为创建 PR！它可以找到相关议题并将其关联到 PR。

1. 转到 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 确认在智能体下拉列表中选中 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 让 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 执行必要的操作来收集信息并完成任务。首次使用 GitHub 工具时，系统可能会要求你使用 GitHub 进行身份验证；按照提示授权即可。
5. 查看 Copilot 如何搜索议题、找到正确议题并创建 PR。
6. 选择 Copilot 生成的链接以审查拉取请求，但**暂时不要合并**。

## 总结和后续步骤

恭喜！本练习中，你：

- 了解了 Model Context Protocol (MCP) 以及 MCP 服务器如何通过新工具扩展 Copilot。
- 将 Playwright MCP 服务器添加到工作区。
- 使用该服务器在发布功能前通过浏览器手动测试了筛选功能。
- 发布了分支，并为筛选工作创建了拉取请求。

你已使用 MCP 服务器*测试*功能，不过 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义智能体][next-lesson]，简化无障碍审查等专注型任务。

## 资源

- [MCP 究竟是什么，为什么大家都在讨论？][mcp-blog-post]
- [Microsoft Playwright MCP 服务器][playwright-mcp-server]
- [GitHub MCP 服务器][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
