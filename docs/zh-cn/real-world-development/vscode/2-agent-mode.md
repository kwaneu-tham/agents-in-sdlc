---
title: "练习 2 - 使用 Copilot 代理模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行运行测试等操作。开发人员的工作流程往往包括找到所有相关文件、进行更改、运行测试、调试、查找遗漏的文件并再次修改，整个过程可能会不断重复。

Copilot 代理模式可以帮助处理这些工作。

Copilot 代理模式在 IDE 中自主工作：先探索现有项目结构，再执行所需更新、运行测试等任务，并自动修复发现的问题。接下来了解如何使用代理模式为网站添加新功能。

> [!NOTE]
> 代理模式和 cloud agent 名称相似，但适用于不同的体验。代理模式在 IDE 中执行任务，便于快速反馈和交互。cloud agent 则像团队成员一样通过议题和拉取请求异步协作。

在本练习中，你将了解如何：

- 让 Copilot 代理模式探索项目、找到相关文件并协调更改。
- 使用 GitHub Copilot 代理模式跨 UI 和数据层实现新功能。
- 在将代码合并到代码库前审查 Copilot 代理模式生成的更改和测试。

## 场景

随着游戏列表增加，需要允许用户按类别和发行商筛选。上一练习已添加发行商辅助函数；现在使用 Copilot 代理模式完成其余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

先了解 Tailspin Toys 网站当前的功能，再进行任何更改。

该网站是带有开发者主题的棋盘游戏众筹平台，可列出游戏并显示详情。它是一个 Astro 应用，在构建时将页面呈现为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`；交互功能使用标准 DOM API 编写的小型、限定范围的 Astro `<script>`。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按照以下步骤在 GitHub Codespace 中运行网站。

1. 返回 Codespace。上一练习结束后应位于 `main`。
2. 在 Codespace 中按 Mac 上的 <kbd>Control</kbd>+<kbd>`</kbd> 或 Windows/Linux 上的 <kbd>Ctrl</kbd>+<kbd>`</kbd>，打开新终端。
3. 创建并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下的 URL 横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否要为 `http://localhost:4321` 打开浏览器窗口，请选择 **x** 将其关闭。

5. 按住 Mac 上的 <kbd>Command</kbd> 或 Windows/Linux 上的 <kbd>Ctrl</kbd>，并选择终端中的 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中选择终端里的 localhost 链接时，会自动转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 Codespace 的私有隧道，网站服务器正在其中运行。

### 浏览网站

网站运行后，查看其主要功能：

- **主页**：显示棋盘游戏列表，包括标题、图片和说明。
- **游戏详情页**：选择游戏后会打开详情页，显示标题、说明、发行商和类别等信息。

## 使用 Copilot 探索待办列表

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始干净的对话。不同练习所用的模式和模型可能不同；相关步骤会在需要时说明。

网站的初始实现已可正常使用，但还可以添加新功能。先查看项目待办列表。模板创建项目时会自动创建一组议题，请 GitHub Copilot 显示这些项目。

1. 在 Chat 视图的代理下拉菜单中选择 **Agent**。**Agent** 会自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中显示代理选择器的截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写研讨会时使用的是 Claude Sonnet 4.5，因此将它列入步骤中，以便建议相对一致、确保体验良好。不过，大型语言模型具有概率性，收到的建议可能与示例不同，这是正常且预期的情况。

> [!NOTE]
> 由于大型语言模型具有概率性，Copilot 可能使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问待办议题：

   ```plaintext
   请显示我的 GitHub 存储库中的待办事项，并根据对用户的帮助程度协助确定优先级。
   ```

4. 选择 **Continue** 运行列出所有议题的命令。
5. 查看生成的议题列表。

Copilot 还会根据它判断对用户最有帮助的程度，为这些项目排序。

## 查看指令文件

让代理生成代码前，先查看用于提供工作上下文的指令文件。项目中的 [用户界面 (UI)][ui-instructions]指令文件说明如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看添加功能的整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 针对不同文件类型的具体指令链接，包括 Astro、Tailwind CSS 和 Drizzle。

> [!TIP]
> 指令文件可引用其他指令文件和项目文件。路径相对于指令文件的位置，因此可复用内容、拆分复杂指导，并提供示例和模板。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## 实现筛选功能

完成筛选至少需要对应用进行三项更新：

- 在数据层 (`src/lib/`) 添加或完善其余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选 UI。

此外，合并前必须运行并通过测试。Copilot 代理模式可以执行这些任务。

1. 继续当前 Copilot 对话，或选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 `ui` 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的代理下拉菜单中仍选中 **Agent**。

   ![Chat 视图中显示代理选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 确认模型仍为 **Claude Sonnet 4.5**。
5. 使用待办列表中相关议题的要求，请 Copilot 实现功能：

   ```plaintext
   请根据待办列表中相关 GitHub 议题的要求，更新网站以支持按发行商和类别筛选。上一练习已添加 publishers 辅助函数；完成其余数据层、UI 和测试时，请保留并按需完善该工作。完成前确保所有测试通过。服务器已经运行，无需重新启动。
   ```

6. 观察 Copilot 探索项目并找到相关文件，包括数据层辅助函数、UI 和测试，然后修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会探索项目、修改文件并运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。过程中编辑器可能会显示 **Keep** 和 **Undo**；Copilot 完成后，每项更改都会有相应的 **Keep** 或 **Undo**，因此无需在工作进行中选择。

7. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 面板中询问用户是否允许运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 工作过程中可能会暂停，甚至出现测试失败。这没有关系；Copilot 会在生成代码和运行测试之间迭代，直到完成任务且未发现错误。

   ![Copilot 代理模式完整 Chat 会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 始终需要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

本练习探索了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加新能力。你了解了如何：

- 使用 GitHub Copilot 代理模式跨 UI 和数据层实现功能。
- 让 Copilot 代理模式探索项目、找到相关文件并协调更改。
- 在将代码合并到代码库前审查 Copilot 代理模式生成的更改和测试。

接下来使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 额外探索练习 — 实现分页

随着游戏列表增加，可能需要启用分页。使用本练习所学知识，请 Copilot 更新网站以实现分页。编写提示时可考虑：

- 遵循现有最佳实践，包括使用已有指令文件。
- 考虑分页方式：允许用户选择每页大小，还是使用固定值。
- 提供足够指导，让 Copilot 按期望实现功能。
- 可能需要通过请求更改并提供上下文来迭代。这是使用 Copilot 的正常流程。

## 资源

- [Copilot ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
