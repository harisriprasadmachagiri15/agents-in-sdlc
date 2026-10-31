---
title: "练习 4 - 使用自定义说明指导 Copilot"
description: "查看存储库说明，添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要以特定方式完成，就应让 Copilot 能够获取相关指导。[说明文件][instruction-files]不仅描述想要什么代码，也描述代码应如何组织。现在已经构建了筛选功能，接下来将查看 Copilot 使用的说明、添加文档标准，并将其应用到代码中。

本练习将：

- 了解存储库说明和按路径限定范围的说明文件如何传递给代理；
- 更新说明文件，确保遵循编码标准；
- 查看说明文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践准则和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明其用途、参数和返回值；如果有可注入的 `db` 参数，也应予以说明。
- 可复用 Astro 组件应说明其 `Props` 契约；相关代码发生变化时，注释也应保持更新。
- 应保留现有格式和 lint 指导。

使用说明文件可以确保 Copilot 获取正确的信息，并按照这些实践要求完成任务。

## 说明文件

自定义说明可为 Copilot 提供上下文和偏好，帮助它理解编码风格和需求。可以用它引导 Copilot 提供更相关的建议和代码片段，也可以指定编码约定、库以及注释风格。说明可应用于整个存储库，也可针对特定文件类型提供任务级上下文。

说明文件分为两类：

- `.github/copilot-instructions.md`：每次请求都会发送给 Copilot 的单个说明文件。它应包含对大多数请求都适用的项目级信息。
- `.github/instructions/*.instructions.md`：针对特定语言、文件类型或任务提供指导的说明文件。

> [!NOTE]
> 不同环境支持的其他说明格式可能有所不同。依赖特定格式前，请查看[自定义说明支持参考][custom-instructions-support]。

## 查看本项目中的自定义说明文件

入门项目已经包含了一些说明文件。修改之前，先查看现有文件，了解其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，了解项目简介和编码指导。此文件适用于存储库中的每次 Copilot 交互。
4. 打开 `.github/instructions` 文件夹并浏览。这里包含 Astro 文件、Drizzle 数据层、测试等说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它指定了说明适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向其他说明文件和项目现有文件的链接。这让你可以将较大的说明集拆分成较小、可复用的文件，并提供可参考的示例。

## 更新说明文件以符合团队指导

现有文件虽然已经很有帮助，但还缺少一项要求。现在更新核心 `copilot-instructions.md` 文件，要求新生成的 TypeScript 添加 TSDoc 注释。Copilot CLI 会在对话启动时加载存储库说明。保存修改后，退出并恢复筛选对话，以便 Copilot 在保留功能上下文的同时加载新指导。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分（大约在第 35 行）。
2. 在该部分末尾添加以下项目符号：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

3. 保存文件。
4. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
5. 在存储库根目录的 `game-filters-cli` 分支上恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认检出的是 `game-filters-cli` 分支。
6. 请 Copilot 按新指导更新代码：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

7. 输入 `/diff`，检查变更的 TypeScript 文件。确认新生成的 TSDoc 注释准确说明了代码。

## 总结和后续步骤

已了解 Copilot CLI 如何读取说明文件，并应用新的代码标准。具体来说，已完成以下工作：

- 查看存储库说明和按路径限定范围的说明文件如何传递给代理；
- 更新说明文件，确保遵循编码标准；
- 查看说明文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [创建有效自定义说明的最佳实践][instructions-best-practices]
- [自定义说明支持参考][custom-instructions-support]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
