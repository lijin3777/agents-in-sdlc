---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。开发者通常需要找到所有相关文件、进行更改、运行测试、调试、发现遗漏的文件并继续更新……类似的工作还会不断重复。

Copilot 智能体模式正是为此而设计的。

Copilot 智能体模式可以更自主地在 IDE 中工作。它的行为类似开发者：先探索现有项目结构，执行所需更新，运行测试等任务，并自动修复发现的问题。现在了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 尽管名称相似，智能体模式和云智能体适用于两种不同的体验。智能体模式在 IDE 中执行任务，便于快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将学习如何：

- 使用 Copilot 智能体模式探索项目、识别相关文件并协调更改。
- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 在将代码合并到代码库之前，审查 Copilot 智能体模式生成的更改和测试。

## 场景

随着游戏列表不断增长，希望允许用户按类别和发行商筛选。上一练习已添加发行商辅助函数，现在将使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行更改前，先浏览 Tailspin Toys 网站，了解其当前功能。

该网站是一个以开发者为主题的桌游众筹平台，允许用户列出游戏并查看详情。网站是一个单体 Astro 应用，会在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能使用标准 DOM API，通过小型且作用域明确的 Astro `<script>` 添加。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单体 Astro 开发服务器。按照以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。上一练习结束后应已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开新终端窗口。
3. 创建并切换到用于筛选功能的新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器准备就绪后，会显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中，从终端选择 localhost URL 会自动将你重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是一个通往 Codespace 的私有隧道，Web 服务器目前就在其中运行！

### 浏览网站

网站运行后，可以探索其功能。主要功能包括：

- **主页**：显示桌游列表，包括游戏标题、图片和描述。
- **游戏详情页**：选择游戏后会打开详情页，显示更多信息，包括标题、描述、发行商和类别。

## 使用 Copilot 探索待办列表

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板，并选择 **New Chat** 开始干净的新对话。每个练习所用的模式和模型可能不同，相关步骤会注明需要选择的模式和模型。

网站的初始版本可以正常使用，但还需要添加更多功能。先查看待办列表。从模板创建项目时，系统已自动创建议题待办列表；让 GitHub Copilot 显示这些项目。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。**Agent** 智能体会自主规划并跨文件实现更改、运行终端命令并调用工具。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写本研讨会时使用的是 Claude Sonnet 4.5，因此将其列入说明，希望代码建议相对一致，以便提供良好体验。不过，由于 LLM 具有概率性，收到的建议可能与研讨会中的描述不同。这完全正常，也在预期之中。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能使用不同的 MCP 命令，但仍应能完成任务。

3. 发送以下提示，让 Copilot 显示议题待办列表：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

请注意，Copilot 还根据它认为对用户最有用的程度，为这些项目排定了优先级。

## 审查指令文件

让智能体生成代码前，先查看将用于提供工作上下文的指令文件。接下来会使用[用户界面 (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) 文件，其中包含如何为网站添加功能的上下文。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意添加功能时的整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍访问原则。
   - 指向各文件类型特定指令的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指令文件可以引用其他指令文件和项目中的文件。路径相对于指令文件所在位置。这样便于重复使用内容、将复杂指令拆分成更易管理的部分，以及提供示例和模板。

## 实现筛选功能

要完成筛选功能，至少需要对应用程序进行三项独立更新：

- 在数据层 (`src/lib/`) 添加或完善其余筛选逻辑。
- 为筛选行为添加或更新测试。
- 更新游戏列表页面，加入筛选 UI。

此外，合并前需要运行并通过测试。Copilot 智能体模式可以替你完成这些任务！现在来添加功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 **ui** 指令文件。

   ![选择 UI 指令文件的屏幕截图示例](../../../_images/ex3-select-instructions-file.png)

3. 在 Chat 视图的智能体下拉列表中确认仍选中 **Agent**。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认模型仍为 **Claude Sonnet 4.5**。
5. 使用以下提示，让 Copilot 根据待办列表中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并查找与功能有关的文件。你会看到它找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 探索项目文件的屏幕截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，可能需要几分钟。在此过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会有 **Keep** 或 **Undo** 按钮，因此无需在工作过程中立即选择。

7. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 面板中要求用户确认是否运行测试的对话框](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至出现测试失败。这没有关系！Copilot 会在代码生成和测试之间不断迭代，直到任务完成且未检测到错误。

   ![Copilot 智能体模式完整 Chat 会话的屏幕截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 请记住，始终需要审查 Copilot 或任何 AI 工具生成的代码。

10. 返回运行着网站的浏览器，探索新功能！
11. 确认一切正常并完成代码审查后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加新功能。你了解了如何：

- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 使用 Copilot 智能体模式探索项目、识别相关文件并协调更改。
- 在将代码合并到代码库之前，审查 Copilot 智能体模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 附加探索练习 — 实现分页

随着游戏列表不断增长，需要添加分页。运用本练习学到的技能，提示 Copilot 更新网站以实现分页。编写代码时可考虑以下事项：

- 遵循现有最佳实践，包括使用现有指令文件。
- 考虑分页方式：允许用户选择每页大小，还是使用固定大小。
- 编写提示时，提供必要的指导，以便 Copilot 按照预期实现功能。
- 可能需要与 GitHub Copilot 多次迭代，请求更改并补充上下文。这是与 Copilot 协作时的正常流程！

## 资源

- [Copilot ask、edit 和 agent 模式：功能及使用时机][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
