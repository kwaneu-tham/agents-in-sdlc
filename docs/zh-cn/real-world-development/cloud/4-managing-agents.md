---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

前几项练习中，已请 Copilot cloud agent 执行三项改善体验和添加功能的任务。cloud agent 虽然异步自主工作，仍需监控任务。GitHub.com 上的 [agents 页面][agents-page]提供任务控制中心，可查看有开放 PR 的任务、检查操作并引导正在进行的会话。

在本练习中，你将：

- 浏览 agents 页面，监控 cloud agent 任务。
- 引导正在进行的会话，请求添加功能。

## 场景

团队在请求高对比度模式后，决定也添加浅色模式。由于智能体已经在更新网站样式并实现切换功能，可以在同一会话中引导它一并添加浅色模式。

## 查看 cloud agent 任务

1. 打开 [Copilot agents 页面][agents-page]。
2. 查看主区域和左侧的任务列表，确认已分配的任务，例如更新文档、生成游戏 API 和添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 执行的操作，例如检出代码、创建环境、设置 MCP 服务器和完成任务的步骤。
4. 查看右侧 PR 区域中的 PR 和更改文件。

> [!NOTE]
> 显示的具体步骤会因 Copilot 的工作进度和实现方式而异。

## 引导 cloud agent

找到正在实现高对比度模式的会话，并要求添加浅色模式。

1. 选择与添加高对比度模式有关的会话。会话标题会因 Copilot 采用的名称和当前工作状态而异。

   ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示设置已完成并开始工作。此时会出现类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入：

   ```plaintext
   在实现高对比度模式的同时，也添加浅色模式。用户也应能通过开关选择所需的显示模式。
   ```

   ![agents 页面中突出显示引导活动会话对话框的 cloud agent 任务截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 确认 Copilot 已收到提示，并将其纳入工作流程。

## 让 Copilot 继续工作

Copilot 会继续处理更新后的任务，并在完成发送提示时正在执行的步骤后，将新要求加入工作流程。与之前一样，这需要几分钟；等待期间可以停下来回顾已学习和探索的内容。

## 总结和后续步骤

本练习探索了 Copilot agents 页面，即监控和引导 GitHub Copilot cloud agent 任务的中心位置。使用此任务控制中心可以跟踪所有进行中和已完成的任务、审查工作过程，并在会话进行中调整范围或补充指导。本练习中，你：

- 探索任务控制中心和 agents 页面，以监控 cloud agent 任务。
- 重定向正在进行的会话，请求添加功能。

Copilot 正在完成无障碍功能，接下来[迭代审查它创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|
