---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

[GitHub Copilot 自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的 AI 助手。可在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义智能体，为其提供专用指令、最佳实践、编码模式和领域知识。团队可将专业经验编码为可复用智能体，例如执行 [WCAG][wcag] 要求的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体由项目 `.github/agents` 文件夹或全局 `~/.copilot/agents` 中的 Markdown 文件定义。每个文件包含至少具有 `name` 和 `description` 的 YAML frontmatter，以及描述智能体行为、专长和指令的 Markdown 提示。

### 自定义智能体与智能体技能

两者都可通过 Markdown 文件定义，并指导 AI 执行操作。简单区分：**自定义智能体是执行工作的角色，技能是它使用的工具**。自定义智能体拥有自己的上下文窗口，可协调技能和其他智能体。本实验中的无障碍智能体会根据无障碍指南审查并更新网站，也可调用 PR 工作流或测试技能。

> [!NOTE]
> 编写自定义智能体没有唯一正确方式。请针对环境和场景测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 希望众筹平台能满足不同视觉能力和偏好的用户需求。有用户反馈，当前深色主题的前景与背景对比度不足，难以阅读。设计团队因此要求添加可切换的高对比度模式。使用自定义智能体尽快实现此功能。

在本练习中，你将了解：

- 如何定义自定义智能体。
- 如何将任务分配给自定义智能体。

## 检查无障碍自定义智能体

项目已提供无障碍自定义智能体。先查看它如何指导 Copilot。

1. 返回 Codespace，打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 查看后续部分中的职责、无障碍最佳实践、HTML/CSS/JavaScript 示例和常见问题。

## 创建并分配议题

使用 agents 页面将高对比度模式任务分配给 Copilot cloud agent。

1. 在浏览器中打开自己的存储库，选择 **Issues**、**New issue** 和 **Blank issue**。
2. 将 **Title** 设置为 `Add high contrast mode to website`，将 **Description** 设置为：

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

3. 选择 **Create**，然后选择 **Assign to Copilot**。
4. 从自定义智能体列表中选择 **Accessibility agent**，再选择 **Assign**。
5. 等待 Copilot 在后台处理任务。

![突出显示自定义智能体和无障碍选项的 cloud agent 分配界面截图](../../../_images/ex5-select-custom-agent.png)

## 总结和后续步骤

本练习介绍了 GitHub Copilot [自定义智能体][custom-agents]，即针对特定任务和领域定制的 AI 助手。使用自定义智能体可以将团队专业知识和标准编码为可复用智能体，引导 Copilot 更有效地完成特定类型的工作。

在本练习中，你了解了：

- 如何定义自定义智能体。
- 如何将任务分配给自定义智能体。

接下来在 agents 页面[监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [在组织中准备使用自定义智能体][org-custom-agents]
- [在企业中准备使用自定义智能体][enterprise-custom-agents]

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents

---

| [← 上一课：GitHub Copilot cloud agent][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|
