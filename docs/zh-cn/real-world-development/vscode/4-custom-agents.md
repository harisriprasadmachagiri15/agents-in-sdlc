---
title: "练习 4 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是针对开发工作流中特定任务或领域定制的人工智能（AI）助手。通过在存储库 `.github/agents` 文件夹中的 Markdown 文件定义代理，可以提供专门指令、最佳实践、编码模式和领域知识，帮助 Copilot 更有效地处理相应工作。团队可以将专业知识编写成可复用的代理，例如确保符合《Web 内容无障碍指南》（WCAG）的无障碍代理、遵循安全编码实践的安全代理，或保持测试模式一致的测试代理。

自定义代理通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局放在 `~/.copilot/agents` 中。每个文件都包含至少具有 `name` 和 `description` 字段的 YAML front matter，后面是定义代理行为、专业知识和指令的 Markdown 提示。

### 自定义代理与代理技能

自定义代理和[代理技能][agent-skills-concept]有些重叠，两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。可以将自定义代理理解为工作者，将技能理解为工具。

自定义代理拥有独立的上下文窗口，并负责协调技能（甚至其他代理）完成工作。本实验中的无障碍自定义代理会根据无障碍指南审查和更新网站；它可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 创建自定义代理没有唯一正确的方法。与 AI 的其他用法一样，需要根据环境和场景进行测试和迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能访问其众筹平台，不受视觉能力或偏好限制。近期用户反馈指出，当前深色主题中的文字与背景对比度不足，难以阅读。设计团队因此要求添加可切换的高对比度模式。

无障碍功能至关重要，因此希望尽快实现。接下来使用自定义代理生成该功能。

本练习将：

- 查看现有的无障碍自定义代理；
- 在 Copilot Chat 中使用无障碍代理实现高对比度模式。

## 查看无障碍自定义代理

项目已提供无障碍自定义代理。先查看其内容，了解它将如何指导 Copilot。

返回 Codespace，在终端中从 `main` 创建新分支，用于无障碍功能开发（筛选功能拉取请求（PR）将保留在练习 3 的分支中）。

1. 运行以下命令：

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML front matter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` front matter。

3. 浏览后续章节，了解：
   - 为无障碍网站生成代码时的核心职责；
   - 无障碍最佳实践；
   - HTML、CSS 和 JavaScript 代码示例；
   - 常见错误和问题。

## 在 Copilot Chat 中使用自定义代理

VS Code 会在 Copilot Chat 视图底部的代理下拉菜单中显示 `.github/agents` 中定义的每个自定义代理。选择自定义代理后，聊天会遵循该代理的指令和工具配置。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。不同练习所需的模式和模型可能不同，相关步骤会注明。

1. 如果代理下拉菜单中尚未选择 **Agent**，请选择它。

   ![Chat 视图中代理选择器的截图](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的代理下拉菜单。默认情况下，当前代理为 **default**。
3. 从列表中选择 **Accessibility agent**。
4. 向无障碍代理发送以下提示：

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat 会探索代码库、提出修改并编辑项目文件。每项修改都会在聊天中显示文件路径和差异，供你审查。
6. 随着修改进行，**Files changed** 指示器会更新，显示代理已修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际修改存储库中的文件，例如 Astro 组件、CSS 和相关测试。

下一练习会审查并引导这项进行中的工作。

## 总结和后续步骤

本练习介绍了 GitHub Copilot 的[自定义代理][custom-agents]，也就是针对特定任务和领域定制的 AI 助手。使用自定义代理可以将团队专业知识和标准编写成可复用配置，帮助 Copilot 更有效地处理特定工作。

本练习涉及以下内容：

- 如何定义自定义代理；
- 如何在 Copilot Chat 代理模式中使用自定义代理。

接下来，[监控并引导代理工作][next-lesson]，审查实时修改，并在同一会话中添加浅色模式开关。

## 资源

- [关于自定义代理][custom-agents]
- [在 IDE 中创建自定义代理][creating-custom-agents-ide]
- [VS Code 中的自定义代理][custom-agents-vscode]
- [自定义代理配置][custom-agents-config]
- [awesome-copilot 中的自定义代理][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
