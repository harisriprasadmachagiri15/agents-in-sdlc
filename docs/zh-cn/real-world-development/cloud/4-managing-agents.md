---
title: "练习 4 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几节练习中，你让 GitHub Copilot cloud agent 处理了三项改进用户体验和添加功能的任务。Cloud agent 虽然异步自主工作，但监控这些任务仍然很重要。

GitHub.com 提供多种管理分配给 cloud agent 的任务的工具，包括 [agents 页面][agents-page]。在这个 mission control 中，可以查看所有具有未关闭拉取请求（PR）的代理任务、查看已执行的操作，也可以在会话进行中提供指导。

本练习将：

- 使用 agents 页面监控 cloud agent 任务；
- 在进行中的会话中提出额外功能要求。

## 场景

分配高对比度模式任务后，团队意识到也应该添加浅色模式。由于代理已经在更新网站样式并添加切换功能，因此适合一并实现。现在引导代理，同时添加浅色模式和高对比度模式。

## 查看 Copilot cloud agent 任务

查看当前分配给 Copilot cloud agent 的所有任务状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。列表应包含分配给 Copilot 的任务，例如：
   - 为代码库补充文档；
   - 生成修改游戏的应用程序编程接口（API）；
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码；
   - 创建 Copilot 工作环境；
   - 设置模型上下文协议（MCP）服务器；
   - 执行完成任务所需的其他步骤。

> [!NOTE]
> 具体步骤会根据 Copilot 的工作状态和采用的方案而有所不同。

4. 查看右侧显示的 PR 窗格，其中可以查看 PR 和变更文件。

## 引导 cloud agent

现在在代理实现高对比度模式的同时，请求它添加浅色模式切换开关。

1. 选择与添加高对比度模式相关的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

   ![Mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到设置完成并开始执行任务。此时会出现类似下文的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Agents 页面中突出显示引导活动会话对话框的 cloud agent 任务截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 会确认收到提示，并将其纳入工作流程。

## 等待 Copilot 完成工作

Copilot 会继续处理更新后的任务。在收到新消息时，它会先完成当前步骤，然后将新要求加入工作流程。

这可能需要几分钟。等待期间，可以回顾目前学到和体验的内容。

## 总结和后续步骤

本练习介绍了 Copilot agents 页面，这是监控和指导 GitHub Copilot cloud agent 任务的中心位置。通过 mission control，可以跟踪所有进行中和已完成的任务、审查代理执行的工作，也可以重定向进行中的任务，调整范围或补充指导。

本练习涉及以下内容：

- 使用 mission control 和 agents 页面监控 cloud agent 任务；
- 重定向进行中的会话，请求添加其他功能。

Copilot 正在完成无障碍功能。接下来，[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
