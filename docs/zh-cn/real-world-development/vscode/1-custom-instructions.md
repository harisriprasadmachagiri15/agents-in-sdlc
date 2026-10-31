---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 应该了解背景信息，就需要确保它能访问这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中一种主要方式是使用[说明文件][instruction-files]指导代码生成。

本练习将：

- 了解项目上下文、编码准则和文档标准如何通过存储库自定义说明及按路径限定范围的说明文件传递给 Copilot；
- 在当前说明生效时生成筛选功能的第一个数据切片（发行商辅助函数）；
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准；
- 重新运行相同提示，观察生成的代码如何采用新标准；
- 将说明更新和筛选功能数据切片提交到 `main`，供下一练习使用。

> [!CAUTION]
> 生成的代码可能不符合所有已设定的标准。Copilot 具有概率性。此练习旨在观察更新说明后行为变化的*趋势*，而不是要求输出逐字符完全一致。

## 说明文件

### 场景

Tailspin Toys 有一套开发实践准则和要求，包括：

- 数据层始终需要单元测试。
- 用户界面（UI）应使用深色模式，并具有现代感。
- 应使用 TSDoc 文档注释为代码添加文档。
- 每个文件顶部都应有一段注释，说明该文件的作用。

使用说明文件可以确保 Copilot 获取正确的信息，并按照这些实践要求完成任务。

### 自定义说明

自定义说明为 Copilot 提供上下文和偏好，帮助它理解编码风格和需求。可以用它指定编码约定、库以及注释风格，也可以为整个存储库或特定文件类型创建说明，提供任务级上下文。

说明文件分为两类：

- `.github/copilot-instructions.md`：每次请求都会发送给 Copilot 的单个说明文件。它应包含项目级信息，例如大多数聊天或 CLI 请求都适用的技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：针对特定任务或文件类型创建的说明，可用于指导特定语言（如 TypeScript 或 Astro），或创建 UI 组件和单元测试等任务。

> [!NOTE]
> 集成开发环境（IDE）中的说明文件只用于 Copilot Chat 代码生成，不用于代码补全或下一次编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时都会使用存储库级说明文件以及带有 `applyTo` front matter 的 `*.instructions.md` 文件。
>
> Copilot [还支持其他标准的说明文件][custom-instructions-support]，包括 `AGENTS.md` 和 `CLAUDE.md`。

### 管理说明文件的最佳实践

本工作坊不会完整讲解如何创建说明文件，但示例项目展示了有代表性的方法：

- 将 `copilot-instructions.md` 聚焦于项目级指导，例如项目说明、结构和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型（单元测试、Astro 组件、数据层）或任务提供具体说明。
- 使用自然语言，保持指导清晰，并提供代码应如何编写或不应如何编写的示例。

创建说明文件没有唯一正确的方法，使用 AI 也是如此。通过试验找到最适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应具备一套完善的说明文件。查看本项目中的文件时，会看到 UI 更新和 Astro 等任务也有相应说明。
>
> Copilot 也可以帮助生成说明文件。不同界面的入口不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应的课程会说明具体入口。
>
> 想找模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中收录了说明文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看本项目中的自定义说明文件

阅读存储库随附的说明文件，包括核心 `copilot-instructions.md` 和针对不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub Web UI 中打开。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下嵌套的 **GitHub Actions Workflows** 指导，这些内容适用于与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览，查看针对 Astro、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意 `applyTo` 字段，它设置了相对于存储库根目录的 glob；例如 `**/*.test.ts` 会匹配 TypeScript 测试文件。
5. 查看为本项目创建单元测试的说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意它链接到其他说明文件（如 `unit-tests.instructions.md`）和项目现有文件。这样可以将较大的说明集拆分成较小、可复用的文件，并提供可参考的示例。文件中的路径相对于说明文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求添加代码内文档。接下来会添加 TSDoc 注释和文件注释头规则。

## 为修改创建分支

先为修改创建分支。

1. 返回之前使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。不同练习所需的模式和模型可能不同，相关步骤会注明。

先在当前说明生效的情况下发送提示。稍后更新说明并重发相同提示，以便观察差异。

1. 关闭之前练习遗留的编辑器标签页，让 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放置的位置。
3. 在 Chat 视图的代理下拉菜单中选择 **Agent**，允许 Copilot 修改文件。

   ![Chat 视图中代理选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码修改，通常会同时修改 helper 文件和测试。
6. 注意生成的辅助函数是一个带类型的函数，第一参数接收 `db` 客户端，并返回带类型的发行商列表。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，即使没有明确要求，也可能添加文档注释。如果出现这种情况也没关系；此练习仍然要观察更新说明后的一致性变化。

## 添加新的存储库标准

`.github/copilot-instructions.md` 用于提供项目级信息。现在为存储库编码标准添加要求，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（大约在第 27 行）。此部分记录了编码标准，但尚无代码内文档要求。
3. 在现有标准下方添加以下 Markdown 内容：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存文件。

> [!TIP]
> 说明文件既可放在存储库级别（`.github/copilot-instructions.md`）以提供全局指导，也可使用 `*.instructions.md` 针对语言、文件类型或任务提供指导。刚添加的文档注释规则属于项目级标准，应放在存储库级文件中。

## 重新运行提示并查看变化

1. 返回 Copilot Chat 并选择 **New Chat**，清空对话内容。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦正确文件。
3. 发送与之前相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意生成文件现在会以类似以下内容的注释块开头：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发行商数据访问辅助函数。
    * 提供从数据库检索发行商信息的函数。
    */
   ```

5. 注意生成的函数现在会包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * 返回所有发行商的列表，包含其 id 和名称。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 一个 Promise，解析为发行商对象数组。
    */
   ```

Copilot 已遵循新的项目标准。下一练习会继续使用这项数据层基础。

## 提交并推送说明和 helper

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
2. 运行以下命令，确认说明更新和 helper 修改已存在：

   ```bash
   git status
   ```

3. 暂存并提交说明更新及 Copilot 为 helper 基础所做的所有修改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示其他辅助修改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 推送分支：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求（PR），并关联待办事项中的文档标准议题。可以手动操作，也可以请 Copilot 使用已连接的 GitHub 工具查找议题、创建关联 PR 并合并。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已配置 GitHub 工具，因此无需额外设置。Copilot 首次使用时，VS Code 可能会提示登录 GitHub。之后的练习会介绍这种连接所用的模型上下文协议（MCP）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认代理选择器中选中 **Agent**。
3. 请 Copilot 查找与更新说明文件有关的议题，并从当前分支创建 PR，同时说明其中包括说明更新和为后续筛选工作创建的发行商辅助函数基础：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. 如果 Copilot 请求 **Allow** 以代表你执行 GitHub 操作，请审查操作并酌情允许。
6. 创建 PR 后，请 Copilot 合并 PR，并将本地分支切回 main：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果 Copilot 请求 **Allow** 以执行 GitHub 操作或 shell 命令，请审查命令并酌情允许。

你已借助 GitHub Copilot 创建并合并拉取请求。

## 总结和后续步骤

你已了解 Copilot 如何读取本项目的说明文件，并使用 Copilot Chat 完成了以下工作：

- 在*现有*说明的基础上发送代码生成提示，并观察生成结果；
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准；
- 重发相同提示，观察生成的代码如何采用新标准；
- 将说明更新和辅助函数基础提交到 `main`，供下一练习继续使用。

接下来，在[代理模式][next-lesson]中应用这些说明，让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
