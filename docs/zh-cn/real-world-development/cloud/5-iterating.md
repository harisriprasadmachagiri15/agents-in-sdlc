---
title: "练习 5：迭代 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理代理][previous-lesson] |
|:--|

## 审查工作

在本实验中，你使用 GitHub Copilot 执行了多项改进用户体验和添加功能的任务，包括为代码补充文档、构建供设计团队迭代的相关游戏功能，以及实现高对比度和浅色模式等无障碍功能。现在检查代码修改；如有需要，向 Copilot 提供反馈，改进结果。之后再启用团队其他成员的审查。

### 场景

生成式人工智能（AI）不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循正常的 DevOps 流程。接下来审查 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能生成的修改。

## 安全与 GitHub Copilot cloud agent

Copilot cloud agent 会异步且自主地执行任务，因此必须遵守一些安全限制：

- Copilot 对存储库只有读取权限，只能写入它用于修改代码的分支。
- Cloud agent 在 GitHub Actions 中运行，并在独立的临时环境中工作。
- GitHub Actions 工作流必须经过人工批准后才能运行。
- [默认情况下，外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括模型上下文协议（MCP）服务器。

## 审查生成的文档

先查看 GitHub Copilot cloud agent 创建的第一个 PR，它为代码补充了文档。通过 GitHub.com 上的标准 PR 界面审查。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。如有需要，可以[自定义或禁用 Copilot cloud agent 防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开 PR 列表。
3. 打开标题类似 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 尚未完成任务，PR 标题会带有 **[WIP]** 标记。请等待 Copilot 完成工作，这可能需要几分钟。

4. PR 准备好后，选择 **Files changed** 选项卡，审查修改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新增的 TSDoc 文档注释和其他文档。具体修改会有所不同。检查 TSDoc 注释、TypeScript 约定和文件注释头；它们来自练习 2 中查看的自定义说明文件。
6. 审查完成且结果符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流会在 PR 的检查区域中排队。如果一切正常，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 修改

可以在 PR 评论中提及 Copilot，也可以在代码行内评论中提及，就像与团队成员协作一样。Copilot 会读取评论并启动新会话处理修改。由于结果具有非确定性，这里不提供固定提示文本。可以请 Copilot：

- 在每个代码文件顶部添加简要说明其用途的注释头；
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释；
- 创建介绍 Astro 应用结构的 README。

1. 添加评论请求 Copilot 修改生成的文档，并像提及用户一样标记 **@copilot**。可以使用上述建议，也可以提出其他文档要求。
2. 选择 **View Session**，查看 Copilot 如何开始新会话并执行修改。
3. 选择 **Back to pull request** 返回 PR。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成修改后，PR 中会出现新的提交。
5. 选择 **Files changed** 选项卡并审查修改。

可以继续迭代，直到满意为止。完成后，可将 PR 从草稿转换为准备审查状态，然后合并到 main 分支。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来查看 Copilot 为“在游戏详情页显示相关游戏”议题创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查完成且结果符合预期后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有等待批准的工作流。
7. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 查看 PR 检查区域中的工作流。若一切正常，单体 Astro 应用的项目检查将通过；此过程可能需要几分钟。
9. **可选：** 可以在 Codespace 中检出此分支，手动测试相关游戏功能。打开终端并运行以下命令，将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已构建相关游戏功能。与之前一样，可以通过新评论要求 cloud agent 更新结果，例如调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释（注意：分配该议题时尚未更新自定义说明）。在 **Conversation** 选项卡中添加评论后，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义代理实现的功能。此 PR 应包括练习 3 中分配的高对比度模式，以及练习 4 中通过 mission control 请求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。请等待 Copilot 完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 检查实现，尤其关注：
   - 用于切换模式的用户界面（UI）组件；
   - 使用本地存储保存用户偏好；
   - 高对比度和浅色模式的 CSS 或样式修改；
   - 无障碍属性（无障碍富互联网应用（ARIA）标签、键盘导航等）；
   - 管理模式切换的 JavaScript/TypeScript 代码。
6. 审查完成且结果符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 查看 PR 检查区域中的工作流。若一切正常，单体 Astro 应用的项目检查将通过；此过程可能需要几分钟。
10. **可选：** 在 Codespace 中检出此分支，手动测试无障碍功能。打开终端并运行以下命令，将 `<branch-name>` 替换为 Copilot 创建的分支名称：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   然后启动应用，在浏览器中测试高对比度和浅色模式开关，并确认刷新页面后设置仍然有效。

无障碍自定义代理帮助 Copilot 按照无障碍最佳实践实现这些功能。如果发现无障碍方面的问题或改进点，可像之前一样在评论中标记 **@copilot**，请求更新。

## 可选练习 — 继续委派任务

Cloud agent 适合处理待办事项，让你能专注于其他工作。可以在自己的存储库中创建更多议题并分配给 Copilot，例如：

- 在游戏详情页添加支持者兴趣表单；
- 为游戏列表页添加分页；
- 为数据访问 helper 添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 工作环境的练习。通过这些课程，你：

- **检查了存储库提供的自定义说明文件**，了解它们之后如何影响 cloud agent 的输出；
- **将议题分配给 Copilot cloud agent**，观察它设置环境、制定计划并异步执行任务；
- **创建并使用自定义代理**，实现高对比度和浅色模式；
- **使用 Copilot agents 页面作为 mission control**，监控并引导进行中的无障碍会话；
- **审查并迭代 cloud agent 的拉取请求**，通过标记 `@copilot` 请求修改并批准工作流。

## 回顾和后续步骤

你已完成 Cloud agent 工作环境的练习。还可以通过其他工作环境继续实践：

- 🖥️ **[VS Code 工作环境](../../vscode/)** — 在集成开发环境（IDE）中使用 Copilot Chat 代理模式并集成模型上下文协议（MCP）。
- 💻 **[CLI 工作环境](../../cli/)** — 在终端中交付两项经过审查的修改，使用 Plan 和 Autopilot 模式、代理技能、自定义 QA 代理、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云端代理桥接。

在自己的存储库中，还可以尝试：

- 将重构或测试覆盖率议题分配给 cloud agent；
- 为其他领域创建自定义代理；
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 代理][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理代理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
