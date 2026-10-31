---
title: "练习 1 - 自定义说明（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：GitHub Copilot cloud agent →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 应该了解背景信息，就需要确保它能访问这些上下文。[说明文件][instruction-files]用于提供此类指导，让 Copilot 不仅了解要做*什么*，也了解*如何*完成。

本练习将：

- 了解项目上下文、编码准则和文档标准如何通过存储库自定义说明及按路径限定范围的说明文件传递给 Copilot；
- 向 `.github/copilot-instructions.md` 添加新的全库标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作环境不同，这里不会运行前后对比提示。Copilot cloud agent 会异步处理 GitHub 议题，因此较难实时并排展示指令的影响。稍后审查 cloud agent 创建的拉取请求时，可以看到说明文件的作用。

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
> IDE 中的说明文件只用于 Copilot Chat 代码生成，不用于代码补全或下一次编辑建议。
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

花些时间查看本存储库随附的说明文件，其中有核心 `copilot-instructions.md` 和一组面向不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub Web UI 中打开。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下嵌套的 **GitHub Actions Workflows** 指导，这些内容适用于与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览，查看针对 Astro、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了相对于存储库根目录的 glob。此处匹配任何 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 查看为本项目创建单元测试的说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意它链接到其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件。这样可以将较大的说明集拆分成更小、可复用的文件，并为 Copilot 指定可参考的示例。文件中的路径相对于说明文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求添加代码内文档。接下来会添加 TSDoc 注释和文件注释头规则。

## 添加新的存储库标准

接下来只需编辑一次：在项目级规则中增加 TSDoc 文档注释和文件注释头要求。cloud agent 之后处理议题时会读取这些说明。

先创建分支。cloud agent 会读取议题目标分支中的说明，但仍应单独提交这项修改：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` 用于提供项目级信息。现在更新编码标准，以改善 Copilot 的代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（大约在第 27 行）。注意其中已记录项目编码标准，但尚无代码内文档规则。
3. 在现有标准下方添加以下 Markdown 内容：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 说明文件既可放在存储库级别（`.github/copilot-instructions.md`）以提供全局指导，也可使用 `*.instructions.md` 针对语言、文件类型或任务提供指导。刚添加的文档注释规则属于项目级标准，应放在存储库级文件中。

## 提交、推送并合并说明更新

cloud agent 会读取议题目标分支中的说明。下一练习中将议题分配给 Copilot 时，它会从 `main` 分支创建分支，因此必须先将说明更新合并到 `main`。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。这样，cloud agent 在下一练习中处理议题时就会读取这些说明。

> [!TIP]
> 如果希望直接在 `main` 分支完成本工作坊，可跳过创建分支的步骤，直接提交到 `main`。此处创建分支是为了模拟实际项目中的工作方式。

## 总结和后续步骤

你已了解 Copilot 如何读取本项目的说明文件，并向 `.github/copilot-instructions.md` 添加了新的存储库级标准。后续练习中，cloud agent 生成的拉取请求会体现该标准。

接下来，将第一个议题[分配给 Copilot cloud agent][next-lesson]。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：说明文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
