---
title: "第 2 课 - 添加星级评分：快速改进"
description: "使用 Copilot CLI 修改游戏卡片，在转发的浏览器中检查更改，并创建和合并第一个拉取请求。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过对话后，现在为项目做第一个更改。游戏数据中已有星级评分，但主页上的游戏卡片尚未显示。请代理显示评分，检查更改，并将其作为第一个拉取请求合并。

在本课中，你将：

- 在功能分支中开始一次聚焦的 Copilot 对话。
- 请代理对项目做一项小更改。
- 使用 `/diff` 检查更改。
- 运行应用，并在转发的浏览器中确认更改。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，游戏详情页已经显示评分。但主页上的游戏卡片只显示标题、类别、发行商和说明。作为热身，请代理在每张卡片上显示已有评分。这项更改小而独立，适合第一次会话。

## 对话的组成

**对话 (conversation)** 是使用 Copilot CLI 完成任务的工作区。与 Copilot app 不同，普通 CLI 对话使用终端当前检出的存储库和 Git 分支，不会创建专用工作树。保存的对话可以稍后继续；文件和分支仍是磁盘上的常规 Git 状态。

对话中有三类内容：提示和代理回复、代理探索和编辑文件时的工具活动，以及可通过 `/diff` 检查的更改。

## 开始对话并请求更改

先开始新对话，再实现功能。

1. 返回 Codespace。
2. 如果终端尚未打开，在 Mac 上按 <kbd>Control</kbd>+<kbd>\`</kbd>，在 Windows/Linux 上按 <kbd>Ctrl</kbd>+<kbd>\`</kbd>。
3. 如果 Copilot CLI 尚未启动，运行以下命令：

   ```bash
   copilot --yolo
   ```

4. 输入 `/new` 并按 <kbd>Enter</kbd>，确保开始新会话。
5. 使用以下提示请求更改：

   ```plaintext
   在列表页的游戏卡片上显示每款游戏的 starRating（满分为 5 分）。如果评分为 null，则显示 "No rating yet"。保持卡片布局不变，添加测试并运行相关检查。
   ```

Copilot 会探索项目、找到显示游戏详情的文件并编写所需代码。现在已使用 Copilot CLI 添加新功能。

## 检查差异

即使更改很小，合并前也要检查所有 AI 生成的更改。直接在 Copilot CLI 中查看更改。

1. 输入 `/diff` 并检查每个更改的文件。
2. 确认游戏卡片在评分存在时显示数字评分，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种状态。
4. 检查 Copilot 运行的检查结果；如果有失败项，请它修复。
5. 完成检查后，按 <kbd>Esc</kbd> 退出差异界面。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 的输出具有概率性而非确定性，因此具体代码可能不同。检查实际行为，不要期待某一种固定实现。

## 检查更改

不要只阅读代码并假设它能正常运行。请 Copilot 启动网站，以便在 Codespaces 转发的浏览器中检查更新后的用户界面 (UI)。

1. 请 Copilot 启动应用：

   ```plaintext
   启动应用，让我在浏览器中检查星级评分更改。告诉我 URL，并让服务器继续运行。
   ```

2. Codespaces 显示端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示的评分为满分 5 分。
4. 返回 Copilot，请它停止启动的服务器：

   ```plaintext
   停止你启动的开发服务器。
   ```

## 创建并合并第一个拉取请求

功能已完成。接下来创建拉取请求 (PR)，将新代码合并到项目中。

1. 请默认代理提交更改：

   ```plaintext
   使用合适的提交消息提交已审查的星级评分更改。
   ```

2. 输入 `/pr create`。Copilot CLI 创建 PR 时会推送现有提交，并显示 PR URL。
3. 按住 Mac 上的 <kbd>Command</kbd> 或 Windows/Linux 上的 <kbd>Ctrl</kbd>，然后选择 Copilot CLI 显示的 URL 以打开 PR。
4. 检查更改的文件和检查结果。
5. 准备好后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，并输入 `/exit` 退出 Copilot CLI。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结和后续步骤

已使用 GitHub Copilot CLI 发布第一个更改。本课中，你：

- 在功能分支上开始了聚焦的 Copilot 对话。
- 引导代理小幅修改游戏卡片。
- 使用 `/diff` 检查了更改。
- 运行应用，并在转发的浏览器中确认星级评分。
- 创建并合并了第一个拉取请求。

接下来从筛选议题开始，使用 [Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
