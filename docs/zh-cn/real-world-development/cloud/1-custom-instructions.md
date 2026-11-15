---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：先决条件][previous-lesson] | [下一课：Copilot 云智能体 →][next-lesson] |
|:--|--:|

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解一些背景信息，就应确保它能够获取这些上下文。[指令文件][instruction-files]用于提供这些指导，帮助 Copilot 理解你想让它做*什么*，以及希望它*如何*完成。

本练习将学习如何：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义指令及按路径应用的指令文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作环境不同，本练习不会运行修改前后对比提示。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示指令的影响。稍后审查云智能体创建的拉取请求时，可以看到指令文件的作用。

## 指令文件

### 场景

Tailspin Toys 和其他优秀的开发团队一样，制定了一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式，并具有现代感。
- 应使用 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加一段注释，说明该文件的用途。

使用指令文件可确保 Copilot 获取正确信息，并按照上述实践完成任务。

### 自定义指令

自定义指令为 Copilot 提供上下文和偏好，帮助它了解你的编码风格和要求。你可以通过这种强大功能引导 Copilot 提供更相关的建议和代码片段，也可以指定首选编码约定、库，甚至希望在代码中使用的注释类型。可以为整个存储库或特定文件类型创建指令，以提供任务级上下文。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。它应包含适用于大多数聊天或 CLI 请求的项目级信息，例如所用技术栈、构建内容概述、最佳实践和其他全局指南。
- `.github/instructions/*.instructions.md`：可以为特定任务或文件类型创建此类文件，提供 TypeScript 或 Astro 等特定语言的指南，也可以用于创建 UI 组件或一组新的单元测试等任务。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级指令文件和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本研讨会的范围。不过，示例项目展示了一种有代表性的方法。总体而言：

- 让 `copilot-instructions.md` 专注于项目级指导，例如构建内容说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（单元测试、Astro 组件、数据层）或特定任务提供详细指令。
- 使用自然语言，保持指导清晰，并提供代码应如何编写（以及不应如何编写）的示例。

创建指令文件没有唯一正确的方法，使用 AI 也是如此。通过实践，你会找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应包含一组完善的指令文件。查看本项目中的指令文件时，可能会发现有些文件针对多种任务，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同产品界面的操作方式有所差异，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应产品的课程会在相关步骤中介绍这些功能。
>
> 想寻找模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义指令文件

花些时间阅读本存储库附带的指令文件：一个核心 `copilot-instructions.md` 文件，以及一组针对不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 中的 **GitHub Actions Workflows** 子部分。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到适用于 Astro 文件、Drizzle 数据层、测试等的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob，用于确定指令适用的文件。此处，任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都会匹配。
5. 查看此项目专用于创建单元测试的指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成更小的可复用文件，并为 Copilot 提供生成代码时可参考的示例。（这些路径相对于指令文件所在目录，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目的编码标准，但尚未要求在代码中添加文档注释。接下来会添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来需要进行本练习唯一的编辑：添加一条项目级规则，要求在代码中使用 TSDoc 文档注释，并添加文件级注释头。云智能体之后处理分配给它的议题时会读取这条规则。

编辑前，先创建并切换到一个分支（云智能体会从议题的目标分支读取指令，但仍建议单独提交这些更改）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在为存储库添加编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，约在第 27 行。查看其中记录的项目编码标准；注意尚无关于代码文档注释的规则，这也是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级文件（`.github/copilot-instructions.md`）中设置全局指导，也可以在 `*.instructions.md` 文件中设置适用于特定语言、文件类型或任务的指导。你刚刚添加的文档注释规则属于项目级标准，应放在存储库级文件中。

## 提交、推送并合并指令更新

云智能体会从议题目标分支读取指令。下一练习中将议题分配给 Copilot 时，它会从 `main` 分支创建分支，因此必须先将指令更改合并到 `main`，云智能体才能读取。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并完成合并。下一练习中云智能体处理议题时，就会读取这些指令。

> [!TIP]
> 如果本研讨会中更想直接在 `main` 上操作，可以跳过创建分支，直接提交到 `main`。此处创建分支是为了让研讨会流程更贴近真实项目。

## 总结和后续步骤

你已了解 Copilot 如何从项目的指令文件获取上下文，并在 `.github/copilot-instructions.md` 中添加了新的存储库级标准。在本工作环境的后续课程中，你会看到云智能体创建的拉取请求如何遵循此标准。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个提示][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件及其他资源集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
