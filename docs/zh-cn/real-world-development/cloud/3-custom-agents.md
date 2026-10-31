---
title: "练习 3 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是针对开发工作流中特定任务或领域定制的人工智能（AI）助手。通过在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义代理，可以提供专门的指令、最佳实践、编码模式和领域知识，帮助 Copilot 更有效地处理相应工作。团队可以将专业知识编写成可复用的代理，例如确保符合《Web 内容无障碍指南》（WCAG）的无障碍代理、遵循安全编码实践的安全代理，或保持测试模式一致的测试代理。

自定义代理通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局放在 `~/.copilot/agents` 中。每个文件都包含至少具有 `name` 和 `description` 字段的 YAML front matter，后面是定义代理行为、专业知识和指令的 Markdown 提示。

### 自定义代理与代理技能

自定义代理和[代理技能][agent-skills-concept]有些重叠，两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。可以将自定义代理理解为工作者，将技能理解为工具。

自定义代理拥有独立的上下文窗口，并负责协调技能（甚至其他代理）完成工作。本实验中的无障碍自定义代理会根据无障碍指南审查和更新网站；它可以调用拉取请求工作流技能，或运行和管理测试的技能。

> [!NOTE]
> 创建自定义代理没有唯一正确的方法。与 AI 的其他用法一样，需要根据环境和场景进行测试和迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

本练习将了解：

- 如何定义自定义代理；
- 如何将任务分配给自定义代理。

## 场景

Tailspin Toys 致力于让所有用户都能访问其众筹平台，不受视觉能力或偏好限制。近期用户反馈指出，当前深色主题中的文字与背景对比度不足，难以阅读。设计团队因此要求添加可切换的高对比度模式。

无障碍功能至关重要，因此希望尽快实现。接下来使用自定义代理生成该功能。

## 查看无障碍自定义代理

项目已提供无障碍自定义代理。先查看其内容，了解它将如何指导 Copilot。

返回 Codespace，然后查看无障碍自定义代理文件：

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML front matter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` front matter。

3. 浏览后续章节，了解：
   - 为无障碍网站生成代码时的核心职责；
   - 无障碍最佳实践；
   - HTML、CSS 和 JavaScript 代码示例；
   - 常见错误和问题。

## 创建并分配议题

Mission control 是管理环境中所有代理的中心位置。可以在其中将任务分配给 Copilot cloud agent、监控任务，以及重定向并指导进行中的会话。现在请将创建高对比度模式的任务分配给 Copilot。

1. 打开存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 在 **Title** 中输入 `Add high contrast mode to website`。
6. 在 **Description** 中输入：

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义代理列表中选择 **Accessibility agent**。

   ![Cloud agent 分配界面中突出显示自定义代理和无障碍代理](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 将在后台开始处理任务。

## 总结和后续步骤

本练习介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，也就是针对特定任务和领域定制的 AI 助手。借助自定义代理，可以将团队专业知识和标准编写成可复用的代理，帮助 Copilot 更有效地处理特定工作。

本练习涉及以下内容：

- 如何定义自定义代理；
- 如何将任务分配给自定义代理。

Copilot 正在实现高对比度模式。接下来转到 mission control，[监控并引导代理会话][next-lesson]。

## 资源

- [关于自定义代理][custom-agents]
- [在组织中使用自定义代理的准备工作][org-custom-agents]
- [在企业中使用自定义代理的准备工作][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot cloud agent][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
