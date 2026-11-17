---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

许多组织都面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或长期搁置的功能请求。GitHub Copilot cloud agent 可自主更新代码和添加功能，完成后创建草稿 PR，供开发人员审查。它可以处理繁琐任务，让开发人员专注于更重要的工作。

在本练习中，你将探索 Copilot cloud agent 的以下用法：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 了解明确限定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务。最初受雇开发网站的承包商没有留下完善的文档，事实上，代码完全缺少文档。首先需要为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改善游戏发现体验：在每款游戏的详情页显示同类别的其他游戏，让用户可以继续浏览。团队目前不需要精致的实现，只需要可用于用户体验验收测试的版本。此工作目前受阻，但还有优先级更高的议题。

这两项都是容易被降低优先级的工作，适合分配给 Copilot cloud agent。它可以异步处理任务，让开发人员专注于其他工作，再回来审查结果。

## 了解 Copilot cloud agent

[GitHub Copilot cloud agent][cloud-agent] 可在后台执行任务。将 GitHub 议题分配给 Copilot 后，它会建立环境、创建用于跟踪进度的草稿 PR 并开始工作。可在任务进行中或完成后查看会话；准备好审查解决方案时，Copilot 会在 PR 中通知你。

## 明确限定指令和任务范围的重要性

GitHub Copilot 没有魔法，使用它仍然需要思考。看似简单的操作也可能包含复杂细节，因此要[谨慎限定交给 Copilot cloud agent 的任务范围][cloud-agent-best-practices]。像与 AI 结对编程一样分阶段工作、学习、试验并逐步调整；引入生成式 AI 并不会改变软件开发的基本原则。

## 此存储库中的自定义指令

前面练习已介绍自定义指令及其对 Copilot 的指导作用。若已完成相关练习，就已看过 `.github/copilot-instructions.md` 的效果；若还没有，现在可以快速查看。分配任务前，以只读方式查看此存储库已有的指令文件，稍后可据此检查输出。在 GitHub 网页界面或已运行的 Codespace 中打开以下文件：

1. 打开 `.github/copilot-instructions.md`，查看 **Code standards** 中的 TypeScript 和 TSDoc 要求。
2. 打开 `.github/instructions/unit-tests.instructions.md`，注意 `applyTo` 如何限定 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给 cloud agent 后，检查 PR 中的 TSDoc 注释、TypeScript 约定和文件头注释；这些内容由上述指令文件提供，稍后的 PR 审查练习还会再次检查。

## 为 Copilot cloud agent 设置开发环境

无论由谁编写代码，通常都需要特定环境并运行设置脚本；将任务分配给 Copilot 时也一样。cloud agent 使用 [GitHub Actions][github-actions] 配置工作环境。可在 `.github/workflows/copilot-setup-steps.yml` 中创建[专用设置工作流][setup-workflow]，在 agent 开始工作前安装所需开发工具和依赖。实验已预先配置此工作流，以确保 Copilot 可使用 Node.js、项目依赖、端到端测试所需的浏览器，以及迁移并填充数据的 SQLite 数据库。

此文件与其他 GitHub 工作流文件类似，但有几点需要注意：

- 它只包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理 PR 前，GitHub Actions 会运行此作业。
- `workflow_dispatch` 触发器允许从存储库的 **Actions** 选项卡手动运行工作流，以便在 Copilot 使用前确认设置成功。

实验中预先配置的工作流如下：

```yaml
name: "Copilot Setup Steps"

# 允许从存储库的“Actions”选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为这些步骤设置所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果未在设置步骤中克隆，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在基于 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

## 将文档议题分配给 Copilot

在 GitHub 上创建议题并分配给 Copilot，跟踪它生成文档的进度。

1. 在浏览器中打开自己的存储库，选择 **Issues**，再选择 **New issue** 和 **Blank issue**。
2. 将 **Title** 设置为 `Code lacks documentation`，将 **Description** 设置为：

   ```plaintext
   组织要求在有助于理解代码时，为函数和方法添加 TSDoc 文档注释。但最近的更新没有遵循此标准。需要更新现有代码，在文档注释能够阐明行为时补充这些注释。
   ```

3. 选择 **Create**，然后在右侧选择 **Assign to Copilot** 打开分配对话框。

   ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

4. 选择 **Assign**。

   ![Copilot 议题分配详情](../../../_images/ex4-assign-copilot-details.png)

5. 打开 **Pull Requests** 选项卡，找到标题类似 `[WIP]: Code lacks documentation` 的 PR。若暂未出现，稍候并刷新页面。
6. 等待 Copilot 生成待办清单，然后查看任务和 PR 时间线中的进度。
7. 如果待办清单尚未出现，请等待几分钟。Copilot 此时正在创建环境（运行前面介绍的工作流）、分析项目并确定合适的实现方案。
8. 查看待办清单和 Copilot 准备执行的任务。
9. 向下滚动 PR 时间线，确认 Copilot 已开始处理议题，然后选择 **View session**。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

9. 查看实时会话，了解 Copilot 如何探索代码、理解当前状态、暂停思考并选择方案，然后编写代码。

这可能需要几分钟。cloud agent 的主要用途之一是异步执行任务，让开发人员可以处理其他工作。接下来利用这一点，将另一个任务分配给 cloud agent，同时继续开发应用功能。

## 分配相关游戏功能

cloud agent 可并行处理不同工作。创建另一个议题，让它实现相关游戏功能。

1. 在存储库的 **Issues** 选项卡中选择 **New issue** 和 **Blank issue**。
2. 将 **Title** 设置为 `Show related games on the game details page`，并将以下内容设为 **Description**：

   ```markdown
   我们希望在每款游戏的详情页显示相关游戏，帮助用户发现更多游戏。设计团队希望探索用户体验并进行验收测试。需求如下：

   - 在 `src/lib/` 中添加数据访问辅助函数，返回与指定游戏属于同一类别的其他游戏，但不包含该游戏本身。
   - 在游戏详情页显示使用此辅助函数的“Related games”区域。
   - 处理游戏没有相关游戏的情况。
   - 为新辅助函数编写单元测试。
   - 创建 PR 前，确保所有测试通过。
   ```

3. 选择 **Create**，在右侧选择 **Assign to Copilot** 打开分配对话框。

   ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

4. 选择 **Assign**。
5. 观察议题首条评论中的 👀 标记，确认 Copilot 已开始处理。

![Copilot 使用 👀 表情表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。它会像软件工程师一样工作，因此无需持续监控；完成后再审查即可。现在可以创建并使用自定义智能体。

## 总结和后续步骤

本练习使用 [GitHub Copilot cloud agent][cloud-agent] 异步处理了议题。你了解了如何自定义生成环境、安全管理操作、明确议题范围并分配任务。cloud agent 还支持 MCP 服务器和此前了解的自定义指令。

接下来创建并使用自定义智能体。

## 资源

- [关于 Copilot cloud agent][cloud-agent]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions

| [下一课：自定义智能体 →][next-lesson] |
|--:|
