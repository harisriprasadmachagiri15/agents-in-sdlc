---
title: "练习 8 - 创建并合并功能拉取请求（PR）"
description: "一起审查筛选功能、说明、技能更新、质量保证（QA）配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、说明更新、技能更新、质量保证（QA）配置和测试都保存在同一个分支中。现在可以一起审查这些修改并创建拉取请求（PR）。之前你手动合并了星级评分 PR；这次将使用 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会将功能、说明更新、技能更新和 QA 代理拆分为几个独立 PR。为简化本工作坊流程，这些筛选和质量工作都保留在同一对话和分支中，并通过这个 PR 一起提交。

本练习将：

- 了解 Agent Merge 以及它如何自动处理合并流程；
- 检查完整的功能变更和验证证据；
- 创建筛选功能 PR；
- 审查后才启用 Agent Merge，并确认 PR 已合并。

## 场景

筛选功能开发过程中，你使用 Copilot 规划、实现并验证了功能。Tailspin Toys 现在希望自动处理剩余 PR 工作，同时让开发者掌控合并授权。

## Agent Merge 简介

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会解决阻止 PR 合并的问题，例如修复失败的持续集成（CI）检查、回复审查意见以及必要时变基；随后，在存储库允许的情况下启用 GitHub 自动合并。

此前你是通过选择 **Merge pull request** 来合并。Agent Merge 可以代为处理这项工作。启用前仍应审查修改，并确认可以合并。

## 使用 Agent Merge 管理 PR

所有代码都已完成后，一起审查代码并创建 PR，再让 Agent Merge 处理其余流程。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入 `/agent`，打开代理对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新的 PR。
5. 使用 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。所有检查成功后，它会执行合并。
7. 确认收到类似 `PR #14 was squash-merged successfully.` 的 Copilot 消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或权限限制。如果流程受阻，请阅读报告的原因，并在存储库允许时手动完成已审查的合并。

## 总结和后续步骤

你已经自动化了开发流程中的多个环节，包括生成代码、测试和验证代码，以及处理拉取请求。具体来说，已完成以下工作：

- 了解 Agent Merge 以及它如何自动处理合并流程；
- 检查完整的功能变更和验证证据；
- 创建筛选功能 PR；
- 仅在审查后启用 Agent Merge，并确认 PR 已合并。

接下来，[探索更多实用的 Copilot CLI 斜杠命令][next-lesson]，管理上下文和模型、共享会话，并选择性地委派给云端代理。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
