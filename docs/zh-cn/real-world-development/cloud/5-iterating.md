---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，你与 GitHub Copilot 合作处理了多项改善用户体验和添加功能的任务：为代码添加文档、实现供设计团队迭代的相关游戏功能，以及实现高对比度和浅色模式等无障碍功能。现在检查代码更改；必要时向 Copilot 提供反馈，改进其工作。

### 场景

加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。始终需要审查生成的代码并遵循正常的 DevOps 流程。接下来审查 GitHub Copilot 为代码生成文档、实现相关游戏功能和无障碍功能所做的更改，然后再允许团队其他成员进行审查。

## 安全性和 GitHub Copilot cloud agent

Copilot cloud agent 会异步且无人监督地执行任务，因此设有安全限制：

- Copilot 对存储库只有读取权限，对它用于编写代码的分支才有写入权限。
- cloud agent 在 GitHub Actions 中运行，并在独立、临时的环境中工作。
- 运行任何 GitHub Actions 工作流前，都必须获得人工批准。
- [默认限制外部资源访问](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先通过 github.com 上的标准 PR 界面，检查 Copilot cloud agent 创建的第一个 PR（为代码添加文档）。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**：默认情况下 Copilot 对外部资源的访问受限，包括调用外部 MCP 服务器。可按需[自定义或禁用 Copilot cloud agent 防火墙][agent-firewall]。

1. 在浏览器中打开自己的存储库。
2. 选择 **Pull Requests**，打开 PR 列表。
3. 打开标题类似 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。请等待它完成；这可能需要几分钟。

4. PR 准备好后，选择 **Files changed** 选项卡并审查更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新添加的 TSDoc 文档注释和其他文档。具体更改可能不同；检查 TSDoc 注释、TypeScript 约定和文件头注释，这些要求来自练习 2 中查看的自定义指令文件。
6. 完成更改审查后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准。
8. 如果有，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流会出现在 PR 的 checks 区域中。等待单体 Astro 应用的项目检查通过；这可能需要几分钟。

## 请求 GitHub Copilot 更改

可以像在团队成员的 PR 评论中一样提及 Copilot，也可以在代码行上添加内联评论。Copilot 会读取评论并启动新会话处理要求。由于结果具有非确定性，无法规定唯一的提示文本；例如可以要求它：

- 为每个代码文件顶部添加简要说明文件用途的注释。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建介绍 Astro 应用结构的 README。

1. 添加评论并提及 **@copilot**，请求更新生成的文档。可使用上述示例，也可提出其他文档要求。
2. 选择 **View Session**，观察 Copilot 启动新会话并执行更新。
3. 选择 **Back to pull request** 返回 PR。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，确认 PR 中出现了新的提交。
5. 选择 **Files changed** 选项卡并审查更改。

可以继续迭代，直到满意为止。完成后，可将 PR 从草稿转换为准备审查状态，再合并到 main 分支。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来检查 Copilot 为游戏详情页显示相关游戏的议题创建的 PR。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡并审查生成的代码。
5. 审查完成后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准；如果有，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

7. 在 PR checks 区域等待单体 Astro 应用的项目检查通过。这可能需要几分钟。
8. **可选：**在 Codespace 中检出分支，手动测试相关游戏功能。将 `<branch-name>` 替换为 Copilot 创建的分支名，例如 **copilot/fix-8**：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已构建相关游戏功能。和之前一样，可以通过新评论继续迭代，例如调整显示的相关游戏数量，或要求添加文件头注释和 TSDoc 文档注释。注意：分配该议题时，尚未更新自定义指令。Copilot 会在 **Conversation** 选项卡中看到评论并启动新会话。

## 审查无障碍功能

最后审查使用无障碍自定义智能体实现的功能。此 PR 应同时包含练习 3 中要求的高对比度模式，以及练习 4 在 agents 页面中追加要求的浅色模式。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 会带有 **[WIP]** 标记。请等待它完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡并审查生成的代码。
5. 检查实现，特别关注：
   - 用于切换模式的 UI 组件。
   - 使用 local storage 保存用户偏好。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性，如 ARIA 标签和键盘导航。
   - 管理模式切换的 JavaScript/TypeScript 代码。
6. 完成审查后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准；如果有，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 等待 PR checks 区域中的单体 Astro 应用项目检查通过，这可能需要几分钟。
9. **可选：**在 Codespace 中检出 Copilot 创建的分支并手动测试无障碍功能：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   然后启动应用，在浏览器中测试高对比度和浅色模式开关，并确认页面重新加载后仍保留设置。

可以看到，无障碍自定义智能体帮助 Copilot 按照无障碍最佳实践实现功能。如有顾虑或改进建议，可像之前一样在评论中提及 **@copilot** 请求更新。

## 可选练习 — 继续委派工作

cloud agent 适合处理待办列表中的真实任务，让开发人员可以转向其他工作。可在存储库中创建更多议题并分配给 Copilot，例如：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

已完成 cloud agent 工作环境的练习。在这些课程中，你：

- **检查了存储库自带的自定义指令文件**，以便之后观察它们对 cloud agent 输出的影响。
- **将议题分配给 Copilot cloud agent**，并观察它设置环境、制定计划和异步执行任务。
- **创建并使用无障碍自定义智能体**，添加高对比度和浅色模式开关。
- **使用 Copilot agents 页面作为任务控制中心**，监控并引导无障碍会话。
- **审查并迭代 cloud agent 的 PR**，通过提及 `@copilot` 请求更改并批准工作流。

## 审查和后续步骤

cloud agent 工作环境的练习已完成。其他工作环境可帮助继续探索：

- 🖥️ **[VS Code 工作环境](../../vscode/)** — 在 IDE 中探索 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 工作环境](../../cli/)** — 在终端中完成两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` cloud agent 桥接。

也可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率议题分配给 cloud agent。
- 为其他领域创建自定义智能体。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
