---
title: "练习 10 - 回顾和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你在 Tailspin Toys 的连续开发流程中使用了 GitHub Copilot CLI。你已经：

- 准备 Codespace、安装 Copilot CLI、探索项目并找到预置的筛选议题；
- 添加星级评分，在转发的浏览器中检查结果，并手动合并第一个拉取请求（PR）；
- 从筛选议题开始，在 Plan 模式中确定方案，在 Autopilot 模式中实现，并在 Interactive 模式中审查；
- 使用自定义说明指导代理，自定义现有 `quality-checks` 技能，并使用它运行单元测试、lint 和类型检查；
- 添加 Playwright 模型上下文协议（MCP）服务器，并在真实浏览器中体验筛选功能；
- 创建并选择质量保证（QA）自定义代理，评估需求、覆盖率、技能结果和浏览器证据；
- 审查完整的筛选修改，并授权 Agent Merge 处理筛选 PR；
- 探索用于管理上下文、模型、共享和选择性云端委派的斜杠命令。

## 已交付的内容

本工作坊包含两个 PR 里程碑，每个里程碑都从更新后的 `main` 分支创建独立分支：

1. **星级评分**：在游戏卡片上显示已有 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流**：实现筛选、更新说明并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置，并加入相关测试。

从规划筛选功能到创建其 PR，你一直使用同一对话和分支。为简化工作坊流程，这些修改合并在一个 PR 中。

## 不同类型的验证

你通过自动化测试、亲自检查浏览器，以及使用 MCP 让 Copilot 探索浏览器来检查代码。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 综合这些结果，在 PR 创建前检查需求和测试覆盖率。

新增测试应解决实际缺口；如果 QA 检查后认为无需新增测试，也可能是正确结果。授权合并前，应检查代码和证据；修改后应重新获取受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本工作坊中，你更新了说明、自定义技能、创建 QA 配置并配置 MCP 服务器。可以在不同对话中复用这些自定义项，并根据团队需求调整。说明设定标准，技能描述可重复执行的任务，自定义代理定义专用角色，MCP 服务器连接外部工具。请检查实际修改和工具结果，而不只是代理的摘要。

根据任务选择**模式和模型**。使用 **Plan** 先考虑方案，使用 **Interactive** 跟进聚焦的小改动，使用 **Autopilot** 处理范围明确的任务。常规编辑可选择更快的模型，复杂任务则可选择能力更强的模型。

上下文依然与基础设施同样重要。清晰说明要构建*什么*、*为什么*以及*如何*实现，会显著影响输出。

## 深入探索

核心工作流已经完成。还可以探索以下 Copilot CLI 功能：

- `/review`：请代码审查代理分析修改。
- `/rubber-duck`：讨论问题并获得另一种观点。
- `/fleet`：并行协调彼此独立的子任务。
- `/worktree`：隔离单独的任务。
- `/delegate`：将任务发送给 Copilot cloud agent。

## 后续步骤

提升工具使用能力的最佳方法就是继续使用它。可以将它用于生产代码、个人项目，或一直想做却尚未动手的小应用。与团队分享经验，也向团队成员学习。还可以继续探索相关文档。

如果想从终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若想比较其他工作环境，可查看 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot cloud agent 工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
