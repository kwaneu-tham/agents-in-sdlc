---
title: "练习 1 - 自定义指令 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。可通过多种方式向 Copilot 提供特定上下文，其中关键方式是使用[指令文件][instruction-files]指导代码生成。

在本练习中，你将：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义指令及路径范围指令文件传递给 Copilot。
- 使用当前指令生成筛选功能的第一个数据切片（发行商辅助函数）。
- 向 `.github/copilot-instructions.md` 添加存储库级标准。
- 再次发送相同提示，观察生成代码如何采用新标准。
- 将指令更新和筛选代码提交到 `main`，供下一练习使用。

> [!CAUTION]
> 生成的代码可能不完全遵循设置的标准，因为 Copilot 具有概率性。本练习旨在观察更新指令前后的行为趋势，而不是逐字匹配输出。

## 指令文件

### 场景

Tailspin Toys 的开发实践指南包括：数据层始终需要单元测试；UI 应采用深色模式并具有现代感；代码应包含 TSDoc 文档注释；每个文件开头应有说明文件用途的注释块。使用指令文件为 Copilot 提供这些要求。

### 自定义指令

自定义指令向 Copilot 提供上下文和偏好，帮助它理解编码风格和要求。指令可以规定编码约定、库以及注释风格，范围既可以涵盖整个存储库，也可以针对特定文件类型。

- `.github/copilot-instructions.md` 是存储库级指令文件，会随该存储库的**每个**请求发送给 Copilot，应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md` 可针对特定任务或文件类型提供指导，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时会使用存储库级指令，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出本研讨会范围，但示例项目展示了一种有代表性的方法：

- 将 `copilot-instructions.md` 聚焦于项目级指导，例如项目内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供专用指令。
- 使用自然语言，保持指导清晰，并提供代码应该和不应该采用的形式示例。

指令文件没有唯一的编写方式，AI 的使用方式也没有唯一答案。通过试验找出适合项目的方法。

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

## 查看项目中的自定义指令文件

存储库提供了一个核心 `copilot-instructions.md` 文件，以及一组针对不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开并查看。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 中嵌套的 **GitHub Actions Workflows** 指导；这些内容适用于与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹，浏览其中针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；其中的 glob 相对于存储库根目录，决定指令适用于哪些文件。例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件会应用这些指令。
5. 查看此项目的单元测试编写指引。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。查看它指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成较小、可复用的文件，并为 Copilot 提供代码示例。此处路径相对于指令文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求代码内文档。接下来会添加 TSDoc 文档注释和文件头注释规则。

## 为更改创建分支

遵循分支实践，为本次更改创建分支。

1. 返回上一练习中的 Codespace。
2. 在终端中按 Mac 上的 <kbd>Control</kbd>+<kbd>`</kbd> 或 Windows/Linux 上的 <kbd>Ctrl</kbd>+<kbd>`</kbd> 打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

先使用当前指令发送提示，更新指令后再发送同一提示进行比较。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始干净的对话。不同练习所用的模式和模型可能不同；相关步骤会在需要时说明。

1. 关闭之前练习留下的编辑器选项卡，避免 Copilot 获取不需要的上下文。
2. 关闭之前练习留下的编辑器选项卡，打开 `src/lib/publishers.ts`，为 Copilot 提供辅助函数的位置上下文。
3. 在 Chat 视图的代理下拉菜单中选择 **Agent**。
   
   ![Chat 视图中显示代理选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加数据访问辅助函数，返回所有发行商的名称和 ID。应用文件更改。
   ```

5. 观察 Copilot 探索项目并应用更改，通常会同时修改辅助函数和测试。
6. 注意建议的辅助函数是一个类型化函数：它将 `db` 客户端作为第一个参数，并返回类型化发行商数组。这来自适用于 `src/lib/*.ts` 的 `.github/instructions/drizzle.instructions.md` 数据层约定。
7. 检查此时生成的代码是否缺少 TSDoc 文档注释和文件级注释。

> [!CAUTION]
> Copilot 具有概率性，即使没有指令也可能生成文档注释。这并不影响本练习观察更新指令后行为是否更一致的目标。

## 添加存储库级标准

在 `.github/copilot-instructions.md` 的 **Code formatting requirements** 部分下方添加文件头注释和 TSDoc 规则。

1. 打开 `.github/copilot-instructions.md`，找到大约第 27 行的 **Code formatting requirements**。
2. 在现有标准下方添加：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

3. 保存文件。

## 再次发送提示并观察变化

使用同一提示比较更新指令前后的建议。

1. 返回 Copilot Chat，选择 **New Chat**，然后重新打开 `src/lib/publishers.ts`。
2. 再次发送与之前完全相同的提示：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加数据访问辅助函数，返回所有发行商的名称和 ID。应用文件更改。
   ```

3. 检查建议文件是否以类似以下注释块开头：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发行商数据访问辅助函数。
    * 提供从数据库检索发行商信息的函数。
    */
   ```

4. 检查函数是否包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * 返回包含所有发行商 ID 和名称的列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 解析为发行商对象数组的 Promise。
    */
   ```

Copilot 已根据新标准更新实际代码，下一练习将在此基础上继续。

## 提交指令并推送分支

指令文件与存储库中的其他资源一样，使用相同的源代码管理流程。将指令更新和辅助函数基础推送到存储库，使下一练习能在 `main` 上使用这些更改。

1. 在 Codespace 中打开新终端，在 Mac 上按 <kbd>Control</kbd>+<kbd>`</kbd>，在 Windows/Linux 上按 <kbd>Ctrl</kbd>+<kbd>`</kbd>。
2. 运行 `git status`，确认指令更新和辅助函数更改都存在。
3. 暂存并提交指令更新及 Copilot 为辅助函数基础所做的所有更改；如果 `git status` 显示还有测试等支持性更改，也将那些文件加入暂存区：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有支持性更改（例如测试），也暂存那些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 推送分支：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求并将其关联到待办列表中的文档标准议题。Copilot 已连接 GitHub 工具，可协助查找议题、创建并合并 PR。

> [!NOTE]
> 这是 Copilot 首次代表你操作 GitHub。项目模板已在工作区配置 GitHub 工具，无需额外设置。Copilot 首次使用 GitHub 工具时，VS Code 可能会提示登录；按照提示完成登录。后续练习会介绍这种基于 Model Context Protocol 的连接。

1. 在 Codespace 中打开 Copilot Chat。
2. 在 Mac 上按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>，或在 Windows/Linux 上按 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>，打开 Copilot Chat 视图，并确认代理选择器中选中 **Agent**。
3. 要求 Copilot 查找与更新指令文件有关的议题，并从当前分支创建 PR，说明指令更新和为后续筛选工作添加的发行商辅助函数基础：

   ```plaintext
   找到与更新指令文件相关的议题。从当前分支创建 PR，说明该 PR 会关闭此议题，并注明我们还为后续筛选工作添加了发行商辅助函数基础。
   ```

4. Copilot 将查找议题并创建 PR。
5. Copilot 请求代表你执行 GitHub 操作时，检查命令并酌情选择 **Allow**。
6. PR 创建后，要求 Copilot 合并 PR 并在本地切回 main：

   ```plaintext
   将 PR 合并到 main。然后在本地切回 main 并拉取最新代码，确保代码保持最新。
   ```

7. Copilot 请求代表你执行 GitHub 操作和 shell 命令时，检查命令并酌情选择 **Allow**。

现在已借助 GitHub Copilot 创建并合并拉取请求。

## 总结和后续步骤

已了解 Copilot 如何从指令文件获取上下文，并在 VS Code Copilot Chat 中先后使用旧、新指令生成代码。本练习中，你：

- 使用现有指令发送了代码生成提示。
- 向 `.github/copilot-instructions.md` 添加了存储库级标准。
- 再次发送相同提示，观察代码如何采用新标准。
- 将指令和辅助函数基础提交到 `main`，供下一练习使用。

接下来在[代理模式][next-lesson]中应用这些指令，让 Copilot 跨代码库实现新功能。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义指令的 5 个提示][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot：指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
