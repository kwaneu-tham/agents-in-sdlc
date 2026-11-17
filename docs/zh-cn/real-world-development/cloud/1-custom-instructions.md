---
title: "练习 1 - 自定义指令 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，或者 Copilot 需要了解背景信息，就要确保它能访问这些上下文。[指令文件][instruction-files]用于提供此类指导，让 Copilot 不仅了解要做什么，也了解如何完成。

在本练习中，你将：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义指令及路径范围指令文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作环境不同，本练习不会通过前后提示实时对比效果。Copilot cloud agent 会异步处理 GitHub 议题，因此较难实时并排展示指令的影响。稍后审查 cloud agent 创建的拉取请求时，可以观察指令文件的影响。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代感。
- 代码应包含 TSDoc 文档注释。
- 每个文件开头应有说明文件用途的注释块。

使用指令文件为 Copilot 提供这些要求，使它按项目实践完成任务。

### 自定义指令

自定义指令向 Copilot 提供上下文和偏好，帮助它理解编码风格和要求。可借此指导它提供更相关的建议和代码片段，并指定编码约定、库以及注释风格。指令既可以适用于整个存储库，也可以针对特定文件类型或任务。

指令文件有两种：

- `.github/copilot-instructions.md`：存储库的单个指令文件，会随该存储库的**每个**请求发送给 Copilot。此文件应包含适用于大多数聊天或 CLI 请求的项目级信息，例如技术栈、项目概况、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建，用于指导特定语言（例如 TypeScript 或 Astro），也可指导 UI 组件或一组单元测试等任务。

> [!NOTE]
> 在 IDE 中，指令文件只用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时，都会使用存储库级指令和带 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

本研讨会不涵盖完整的指令文件编写过程，但示例项目展示了一种有代表性的做法：

- 将 `copilot-instructions.md` 聚焦于项目级指导，例如项目内容、结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务编写专用指令。
- 使用自然语言，保持指导清晰，并提供代码应该和不应该采用的形式示例。

指令文件和 AI 的使用方式都没有唯一正确答案。通过试验找出适合项目的方法。

> [!TIP]
> 使用 GitHub Copilot 的每个项目都应有一套完善的指令文件。浏览本项目文件时，可看到针对多种任务的指令，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同工作界面的操作方式不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`；当前工作环境的相关课程会在适用时介绍。
>
> 需要模板或起点？可浏览包含指令文件、自定义智能体和其他资源的 [awesome-copilot][awesome-copilot] 存储库。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义指令文件

花些时间查看存储库自带的指令文件：一个核心 `copilot-instructions.md`，以及一组针对不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开它们。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 中嵌套的 **GitHub Actions Workflows** 指导；这些内容适用于与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹，浏览其中针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；其中的 glob 相对于存储库根目录，决定指令适用于哪些文件。例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件会应用这些指令。
5. 查看此项目的单元测试编写指引。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。查看它指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成较小、可复用的文件，并为 Copilot 提供代码示例。此处路径相对于指令文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求代码内文档。接下来会添加 TSDoc 文档注释和文件头注释规则。

## 添加存储库级标准

本练习唯一需要编辑的内容是添加项目级规则，要求代码中包含 TSDoc 文档注释和文件级头注释。后续练习中，cloud agent 会在处理分配给它的议题时读取此标准。

先创建工作分支。cloud agent 会从议题目标分支读取指令；本练习仍会单独提交更改。

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。添加存储库编码标准有助于改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到大约第 27 行的 **Code formatting requirements** 部分。注意其中记录了项目编码标准，但尚未规定代码内文档；因此生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown，要求 Copilot 添加文件头注释和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如前一课所述，指令文件可以放在存储库级文件 `.github/copilot-instructions.md` 中提供全局指导，也可以放在 `*.instructions.md` 文件中，限定到特定语言、文件类型或任务。刚添加的文档注释规则属于项目级标准，应放在存储库级文件中。

## 提交、推送并合并指令更新

指令文件与存储库中的其他资源一样，使用相同的源代码管理流程。下一练习把议题分配给 Copilot cloud agent 时，它会从 `main` 分支开始工作，因此必须先将指令更改合并到 `main`。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。之后 cloud agent 处理下一练习中分配的议题时，就会读取这些指令。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，可以跳过分支操作并直接提交到 `main`。此处使用分支是为了模拟真实项目的工作方式。

## 总结和后续步骤

已了解 Copilot 如何从项目指令文件获取上下文，并向 `.github/copilot-instructions.md` 添加存储库级标准。后续可以在 cloud agent 创建的 PR 中观察这些标准的效果。

接下来[将第一个议题分配给 Copilot cloud agent][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义指令的 5 个提示][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
