---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案、使用 Autopilot 根据议题构建筛选功能，并在 Interactive 模式下审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

此前已为项目添加一项小功能。更大的更改需要更完善的流程。GitHub Copilot CLI 可以融入组织现有的工作流，帮助按正确方式构建正确的功能。本课开始一系列智能体开发流程练习：从议题出发生成新功能，确认代码有效、行为符合预期，最后成功合并。

> [!NOTE]
> 后续功能工作将沿用同一对话和分支。实际工作中，不同文件类型的更改通常会使用不同分支或 PR；本研讨会为聚焦核心概念而简化了流程。

在本课中，你将：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在 Plan 模式下确定需求。
- 使用 Autopilot 模式实现功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续实现此功能时，还会更新存储库指令、自定义现有 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并创建功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访问者需要按类别和发行商筛选游戏。待办议题描述了功能，但组合类别等细节需要在编码前确定。使用 Plan 模式明确这些决定，然后授权 Autopilot 在限定范围内实现。

## 背景

将 AI 编码智能体引入开发流程，并不会改变基本原则；这些原则反而更重要。大多数开发人员的工作流大致如下：

1. 从描述需求的已登记议题开始。
2. 制定实现计划。
3. 编写并审查代码。
4. 运行测试验证代码。
5. 手动验证新功能。
6. 创建拉取请求 (PR)。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但大多与上述流程相似。

遵循标准流程可以确保 AI 生成的代码满足需求，并像手写代码一样经过审查。

## 对话模式

**对话模式**决定代理拥有多少自主权。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可循环切换模式：

- **Interactive**：你与代理协作；代理提出更改建议，并等待输入后再继续。
- **Plan**：代理先制定计划，在此模式下不能编辑项目文件。
- **Autopilot**：代理自主编写代码、运行测试并迭代，直到认为任务完成。

先使用 Plan 模式并审查计划，再使用 Autopilot 实现。

## 从议题开始

开始筛选功能前，返回 Codespace，确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，在 Mac 上按 <kbd>Control</kbd>+<kbd>\`</kbd>，在 Windows/Linux 上按 <kbd>Ctrl</kbd>+<kbd>\`</kbd>。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题加入提示，并返回 **Session** 选项卡。

提示现在应以 `#7`（或类似编号）开头。`#` 可将 GitHub 议题或拉取请求 (PR) 加入上下文。

## 规划筛选功能

规划可以在交给 Copilot 实现前确定功能或任务的方案。复杂任务尤其值得先花时间规划。切换到 Plan 模式，请 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   为实现此功能制定计划。
   ```

Copilot 会先探索项目，再确定合适的实现方案；过程中可能会询问筛选行为的细节。根据偏好回答，没有唯一正确答案。

3. 计划完成后，在 Mac 上按 <kbd>Control</kbd>+<kbd>E</kbd>，在 Windows/Linux 上按 <kbd>Ctrl</kbd>+<kbd>E</kbd> 展开计划。
4. 浏览计划，并要求 Copilot 修改不符合决定的部分。

## 批准 Autopilot

计划编写并审查后即可开始实现。Autopilot 会持续处理问题，直到认为任务完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或已安装版本中含义相同的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 如何按计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准计划后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决环境问题，再判断检查是否完成。

## 审查并验证实现

与其他代码一样，生成的代码也必须在合并前审查。审查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选实现和测试。
3. 根据议题和规划时确定的事项检查结果。
4. 审查代码后按 <kbd>Esc</kbd> 退出差异视图。
5. 检查项目运行的检查结果；如有失败，请 Copilot 修复。

## 体验新功能

代码看起来没问题后，还要实际运行。请 Copilot 启动应用，并通过 Codespaces 转发端口打开网站。

1. 请 Copilot 启动应用：

   ```plaintext
   启动应用，让我在浏览器中尝试筛选功能。告诉我 URL，并让服务器继续运行。
   ```

2. Codespaces 显示端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及计划中确定的组合。
4. 确认重置和无结果时的行为符合议题及规划决定。
5. 返回 Codespace，请 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

已使用不同的对话模式构建并审查功能。本课中，你：

- 从 GitHub 议题开始了新的 Copilot 对话。
- 在 Plan 模式下确定了需求。
- 使用 Autopilot 模式实现了功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来通过[使用自定义指令][next-lesson]了解如何让代码遵循已记录的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
