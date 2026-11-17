---
title: "第 4 课 - 使用自定义指令指导 Copilot"
description: "探索存储库指令，添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务有特定做法，应将指导信息提供给 Copilot。[指令文件][instruction-files]不仅说明想要什么代码，也说明代码应如何组织。完成筛选功能后，接下来探索 Copilot 使用的指令、添加文档标准，并将其应用于代码。

在本课中，你将：

- 了解存储库指令和路径范围指令文件如何传递给代理。
- 更新指令文件，确保遵循编码标准。
- 观察指令文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如果有可注入的 `db` 参数，也要记录。
- 可复用 Astro 组件应说明其 `Props` 契约；相关代码更改时，注释也应保持更新。
- 保留现有格式和 lint 指南。

使用指令文件可确保 Copilot 按照这些实践完成任务。

## 指令文件

自定义指令为 Copilot 提供上下文和偏好，帮助它理解编码风格和要求。指令可规定编码约定、库以及注释风格，范围既可以涵盖整个存储库，也可以针对特定文件类型。

- `.github/copilot-instructions.md`：存储库的单个指令文件，会随该存储库的**每个**请求发送给 Copilot。此文件应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导。

> [!NOTE]
> 其他指令格式及其支持情况因环境而异。依赖特定格式前，请查看[自定义指令支持参考][custom-instructions-support]。

## 探索项目中的自定义指令文件

起始项目已包含一组指令文件。先探索现有文件，再修改并观察效果。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 查看项目简介和编码指导；这些指令适用于此存储库中的每次 Copilot 交互。
4. 打开 `.github/instructions` 文件夹，查看其中针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；其中的 glob 决定指令适用于哪些文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它对其他指令文件和项目现有文件的链接。这样可以将较大的指令集拆分为较小、可复用的文件，并为 Copilot 提供参考示例。

## 更新指令以符合团队指南

现有文件是不错的起点，但还缺少一项要求。修改核心 `copilot-instructions.md`，要求新生成的 TypeScript 包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分（大约第 35 行）。
2. 在该部分最后添加以下条目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库指令。保存更新后的指令后，退出并恢复筛选对话，以便 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录中恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，选择筛选对话，并确认检出的是 `game-filters-cli` 分支。
3. 请 Copilot 应用更新后的指导：

   ```plaintext
   我们刚刚更新了指令和代码指导。请更新你生成的代码，使其符合这些指导。
   ```

4. 输入 `/diff` 并查看更改的 TypeScript 文件。检查新生成的 TSDoc 注释是否准确说明代码。

## 总结和后续步骤

已了解 Copilot CLI 如何从指令文件获取上下文，并将新标准应用于功能。本课中，你：

- 探索了存储库指令和路径范围指令文件如何传递给代理。
- 更新了指令文件以确保遵循编码标准。
- 观察了指令文件对代码的影响。

接下来[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保以一致方式运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建有效自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot：指令文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
