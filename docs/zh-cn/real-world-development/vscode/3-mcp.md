---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：代理模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

已使用代理模式构建筛选功能。创建拉取请求前，应在浏览器中确认功能正常。本练习将连接 **Playwright MCP 服务器**，让 Copilot 操作真实浏览器测试功能，然后发布分支并创建 PR。

在本练习中，你将：

- 了解 Model Context Protocol (MCP) 以及 MCP 服务器如何通过新工具扩展 Copilot。
- 向工作区添加 Playwright MCP 服务器。
- 请 Copilot 使用它在浏览器中手动测试筛选功能。
- 发布分支，并为筛选更改创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

当代理模式可以访问编辑器以外的资源时，它会更加强大。Model Context Protocol (MCP) 是 Copilot 与这些资源交互的方式，也是代理与外部工具和服务通信的标准。MCP 可让 AI 代理实时获取最新信息（通过资源）并代表你执行操作（通过工具）。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 代理与外部工具和服务之间的桥梁，负责管理代理与现有 API 或 NPM 包等本地工具之间的通信；每个服务器提供一组不同的工具和资源。

![代理模式的工作原理及其与上下文、LLM 和工具（包括 MCP 服务器与 VS Code 扩展提供的工具）交互的示意图](../../../_images/ex1-mcp-diagram.png)

- **[GitHub MCP Server][github-mcp-server]**：提供管理 GitHub 存储库的 API，允许代理创建和更新存储库，并管理议题和拉取请求。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化能力，允许代理访问网页、填写表单和选择按钮。

GitHub 还提供 [MCP 注册表][mcp-registry]，帮助发现服务器并参与生态系统。

> [!CAUTION]
> 应像对待其他依赖项一样对待 MCP 服务器。使用前检查源代码、验证发布者并评估安全影响；只使用可信服务器，谨慎授予敏感资源或操作的访问权限。

## 检查 MCP 配置

`.vscode/mcp.json` 配置此 VS Code 工作区可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 确认项目已配置以下 `github` 服务器：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

模板提供此远程 GitHub MCP 服务器，因此 Copilot 能在前一练习读取议题。VS Code 通过 HTTP 连接，首次使用工具时使用 GitHub 身份验证。

## 添加 Playwright MCP 服务器

添加第二个服务器，让 Copilot 操作浏览器。

1. 在 `.vscode/mcp.json` 的 `github` 旁添加 `playwright`：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. 保存文件。VS Code 会在本机运行 `npx @playwright/mcp@latest` 启动本地 Playwright 服务器；`--headless` 让浏览器无可见窗口运行，适用于没有桌面环境的 Codespace。

> [!NOTE]
> 项目已使用 Playwright 运行端到端测试，通常已安装所需浏览器。如果 Copilot 报告缺少浏览器，请让它运行 `npx playwright install chromium` 后重试。

## 启动并信任服务器

VS Code 按需启动 MCP 服务器：Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问是否信任。

1. 在 `.vscode/mcp.json` 中选择 `playwright` 条目上方的 **Start**，确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请检查配置并确认信任。

也可以跳过手动启动；Copilot 首次使用该服务器时，VS Code 会启动它并提示信任。

## 测试筛选功能

Playwright MCP 可操作真实浏览器。确认服务器已连接且应用正在运行后，在 Copilot Chat 中发送以下请求：

```text
使用 Playwright MCP 服务器打开 http://localhost:4321 上运行的应用，并验证新的游戏筛选功能：

1. 打开游戏页面并记录列出的游戏数量。
2. 应用类别筛选，确认列表只显示该类别的游戏。
3. 清除筛选，再应用发行商筛选，确认列表只显示该发行商的游戏。
4. 同时应用类别和发行商筛选，确认结果同时符合两个条件。

报告每一步的观察结果，并指出任何不符合预期的行为。
```

Playwright MCP 服务器通过页面的[无障碍树][playwright-mcp-server]工作，而不是依赖截图。代理会处理带标签的结构化元素（按钮、链接和列表项），与辅助技术类似；因此，功能检查也可以作为初步无障碍检查。服务器会启动浏览器、逐步操作并报告结果。将报告与议题验收条件对照；发现问题时，先追问或让 Copilot 修复，再创建 PR。

> [!NOTE]
> 测试时应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请先重启。首次使用 Playwright MCP 时可能需要下载浏览器；缺少浏览器时运行 `npx playwright install chromium` 后重试。

## 发布分支并创建拉取请求

确认功能正常后，发布 `filtering-vscode` 分支供团队审查。

1. 在 Codespace 中打开 **Source Control** 面板并审查 Copilot 所做更改。
2. 选择 **+** 暂存更改，使用 **Sparkle** 按钮生成提交消息，再选择 **Publish** 推送分支。
3. 在 Copilot Chat 中选择 **New Chat**，确认代理下拉菜单中选择 **Agent**。
4. 发送以下提示，要求 Copilot 找到议题并关联 PR：

   ```plaintext
   找到存储库中关于按类别和发行商筛选的议题。为当前分支创建拉取请求，并将其关联到正确的议题。
   ```

5. 按需选择 **Continue** 允许 Copilot 执行操作。首次使用 GitHub 工具时，按提示完成身份验证。
6. 打开 Copilot 创建的链接并审查 PR，**暂时不要合并**。

## 总结和后续步骤

本练习中，你：

- 了解了 Model Context Protocol (MCP) 以及 MCP 服务器如何通过新工具扩展 Copilot。
- 向工作区添加了 Playwright MCP 服务器。
- 使用该服务器在发布前通过浏览器手动测试了筛选功能。
- 发布了分支，并为筛选工作创建了拉取请求。

接下来[创建自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 是什么，为什么大家都在讨论？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
