---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** 可在云端异步处理任务。你可以在 GitHub 上分配工作，cloud agent 会在后台探索存储库、进行修改并创建拉取请求，让你可以继续处理其他工作。

本系列练习将添加 cloud agent 会遵循的自定义说明，然后分配 GitHub 议题并让它实施。你还将审查和使用自定义代理来指导工作方式，从 agents 仪表板监控并引导会话，最后审查 cloud agent 创建的拉取请求并迭代结果。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 Codespace |
| [1. 自定义说明][ex1] | 上下文 | 添加 cloud agent 将遵循的自定义说明 |
| [2. Cloud agent][ex2] | 异步代理 | 将议题分配给 Copilot cloud agent |
| [3. 自定义代理][ex3] | 专用代理 | 查看并使用自定义代理 |
| [4. 管理代理][ex4] | 监控 | 监控并引导代理会话 |
| [5. 迭代][ex5] | 审查 | 审查 PR、迭代 Copilot 的工作并选择后续步骤 |

## 先决条件

参加本工作坊前，请确保具备以下条件：

- [ ] 拥有有效的 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 账户
- [ ] 已为你的账户或组织启用 Copilot cloud agent

> [!TIP]
> 没有付费计划？通过验证的学生可通过 [GitHub Education][callout-student-plan-education] 免费使用 GitHub Copilot。**Copilot Student** 计划包含本工作坊使用的代理、模型上下文协议（MCP）、代码审查和 Copilot CLI 功能，可用于完成所有工作环境的练习。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> 除 Copilot Free 之外，Copilot Student、Pro、Pro+、Business 和 Enterprise 均提供 cloud agent。在 Copilot Business 或 Enterprise 计划中，管理员必须为组织启用该功能。

> [!NOTE]
> 本工作环境不涉及 MCP。若想了解如何将 MCP 服务器与 Copilot 配合使用，请查看 [CLI 工作环境](../cli/)或 [VS Code 工作环境](../vscode/)。

## 开始

**[从练习 0：先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
