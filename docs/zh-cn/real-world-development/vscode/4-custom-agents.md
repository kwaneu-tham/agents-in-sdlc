---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

[GitHub Copilot 自定义智能体][custom-agents]是针对开发工作流中特定任务或领域定制的 AI 助手。可在 `.github/agents` Markdown 文件中提供专用指令、最佳实践、编码模式和领域知识，将团队经验编码为可复用智能体，例如执行 [WCAG][wcag] 要求的无障碍智能体、安全智能体或测试智能体。智能体也可全局定义在 `~/.copilot/agents` 中。每个文件包含至少具有 `name` 和 `description` 的 YAML frontmatter，以及描述智能体行为的提示。

### 自定义智能体与智能体技能

两者都可通过 Markdown 文件定义，并指导 AI 执行任务。简单区分：**自定义智能体是执行工作的角色，技能是它使用的工具**。智能体拥有自己的上下文窗口，可协调技能和其他智能体。本练习中的无障碍智能体会根据无障碍指南审查和更新网站，也可调用 PR 或测试技能。

> [!NOTE]
> 编写自定义智能体没有唯一正确方式。请针对环境和场景测试并迭代。

[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能访问其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，当前深色主题中的文字与背景颜色对比度不足，导致部分用户难以阅读。为解决这一无障碍问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

无障碍对平台至关重要，因此需要尽快实现此功能。本练习将使用自定义智能体生成相关功能。

在本练习中，你将：

- 查看现有的无障碍自定义智能体。
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 查看无障碍自定义智能体

先从 `main` 创建独立分支，避免与练习 3 的筛选 PR 混在一起。

1. 返回 Codespace，在终端运行：

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. 打开 `.github/agents/accessibility.md`，查看包含 `name` 和 `description` 的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 查看智能体关于无障碍网站、最佳实践、HTML/CSS/JavaScript 示例和常见问题的内容。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的代理下拉菜单中显示 `.github/agents` 里定义的每个自定义智能体。选择自定义智能体后，该聊天会话将使用它的指令和工具。

1. 在 Copilot Chat 面板选择 **New Chat**，开始干净的对话。
2. 确认代理下拉菜单中选择 **Agent**，然后选择 **Accessibility agent**。

   ![Chat 视图中显示代理选择器的截图](../../../_images/shared-chat-mode-selector.png)

3. 发送以下提示：

   ```plaintext
   为网站添加高对比度模式。用户应能通过开关启用高对比度，并使用浏览器 local storage 在页面重新加载后保留设置。
   ```

4. 查看 Copilot 探索代码库并提出更改。每项编辑都会以内联差异的形式显示；**Files changed** 指示器会更新当前工作集。

> [!NOTE]
> 这项工作可能需要几分钟。Copilot 会修改 Astro 组件、CSS 和相关测试等现有文件。

## 总结和后续步骤

本练习探索了 GitHub Copilot [自定义智能体][custom-agents]，这类 AI 助手专门针对特定任务和领域。自定义智能体可将团队专业知识和标准编码为可复用配置，引导 Copilot 更有效地完成特定工作。

在本练习中，你了解了：

- 如何定义自定义智能体。
- 如何在 Copilot Chat 代理模式中使用自定义智能体。

接下来[监控并引导智能体工作][next-lesson]，在同一会话中添加浅色模式。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [Awesome Copilot 中的自定义智能体][awesome-copilot-agents]

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|
