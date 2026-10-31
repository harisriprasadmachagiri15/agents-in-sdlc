---
title: "练习 7 - 创建并使用质量保证代理"
description: "创建质量保证（QA）自定义代理，结合议题需求、quality-checks 技能和 Playwright 模型上下文协议（MCP）。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，也使用 Playwright 模型上下文协议（MCP）在浏览器中检查筛选功能。现在将这些能力整合到一个自定义代理中，并定义清晰的质量保证（QA）流程。

本练习将：

- 了解自定义代理如何结合说明、技能和 MCP 工具；
- 创建并检查可复用的质量保证配置；
- 选择 QA 代理，并根据筛选议题审查其发现。

## 场景

在创建拉取请求（PR）之前，Tailspin Toys 希望统一检查需求、代码质量、自动检查、测试覆盖率和浏览器行为。自定义代理可以协调 QA 流程，并生成可复用的报告。

## 什么是自定义代理？

自定义代理是在 Markdown 配置文件中定义的 Copilot 专用版本。配置文件说明代理的用途、指令和可用工具。本工作坊将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

这些自定义功能各有不同用途：存储库说明描述团队标准；`quality-checks` 技能封装可重复运行的检查；Playwright MCP 提供浏览器工具；QA 配置则告诉 Copilot 如何运用这些能力评估需求并报告结果。它们彼此互补，不需要另开对话。

## 创建 QA 配置

创建功能 PR 前，请 Copilot 创建可复用的 QA 配置。配置将明确 QA 执行的检查和必须遵守的边界。

1. 返回 Codespace，确认筛选对话处于 Interactive 模式。
2. 请默认代理创建新的自定义代理：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置

使用新代理之前，先检查配置，确认 Copilot 捕获了预期的 QA 流程和边界。这样可避免代理遗漏要求，或在只需要验证功能时擅自修改功能。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 front matter。`description` 为必需字段；`name` 是可选字段，但可提供清晰的显示名称。
3. 阅读配置说明，确认 QA 会从需求出发、遵循存储库说明、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会先询问，而且不会提交修改或创建拉取请求。
5. 如果生成的配置遗漏上述职责或边界，请在继续前要求默认代理修改。

## 根据议题运行 QA

Copilot CLI 会在对话启动时加载项目代理。创建配置后，恢复同一筛选对话并选择 QA，以便它使用已有的议题和规划决策。

1. 使用以下提示启用代理：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的代理可能暂时不会显示在可用代理列表中，但仍可使用上述命令启用。

2. 使用以下提示请 QA 代理审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. 等待 QA 代理完成工作。
4. 阅读代理提供的报告。

## 总结和后续步骤

已添加可复用的专用代理，并审查了它的工作。具体来说，已完成以下工作：

- 了解自定义代理如何结合说明、技能和 MCP 工具；
- 创建并检查可复用的质量保证配置；
- 选择 QA 代理，并根据筛选议题审查其发现。

现在，功能实现、技能更新、QA 配置、测试和验证报告都已准备好接受审查。接下来，[创建功能 PR 并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义代理][create-agents]
- [自定义代理配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
