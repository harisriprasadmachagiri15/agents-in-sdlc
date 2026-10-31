---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期积压的功能请求。GitHub Copilot cloud agent 可以自主更新代码和添加功能。完成任务后，它会创建草稿拉取请求（PR），供开发者审查。这样可以将繁琐任务交给代理处理，加快开发进程，让开发者专注于更重要的工作。

本练习将使用 Copilot cloud agent：

- 自定义代码生成环境；
- 了解如何安全地执行操作；
- 认识清晰定义议题的重要性；
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初开发网站的承包商没有为代码编写文档。团队希望先为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现体验。他们想在每款游戏的详情页显示同类别的其他游戏，方便访问者继续浏览。团队目前只需要可用于验收测试用户体验的初步实现。这项工作目前受阻，而还有优先级更高的议题。

这类任务很容易被搁置，适合分配给 Copilot cloud agent 异步处理。开发者可以继续处理其他工作，之后再审查 Copilot 的结果。

## GitHub Copilot cloud agent 简介

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像开发者一样在后台执行任务。可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿 PR 跟踪进度、设置环境并开始处理任务。你可以在会话进行中或完成后查看详情。解决方案准备好审查时，Copilot 会在 PR 中通知你。

## 明确界定任务的重要性

GitHub Copilot 并非魔法，不能替代思考。即使看似简单的操作也可能包含复杂环节，因此要谨慎界定 cloud agent 的任务范围。将它视为人工智能（AI）结对程序员：分阶段工作、学习、试验并不断调整。引入生成式 AI 不会改变软件开发的基本原则。

## 本存储库中的自定义说明

之前的练习介绍了自定义说明及其用途。如果已完成自定义说明练习，就会看到 `.github/copilot-instructions.md` 的作用；否则可先快速查看。分配任务给 Copilot cloud agent 前，先以只读方式检查存储库中的说明文件，之后便能观察它们的影响。

在 GitHub Web UI 中或已运行的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：查看 `applyTo` front matter，它将说明限定为 `**/*.test.ts` 文件。

下一部分将 `Code lacks documentation` 议题分配给 cloud agent。审查生成的 PR 时，留意 TSDoc 注释、TypeScript 约定和文件注释头；这些要求来自说明文件。之后的练习还会再次检查。

## 设置 Copilot cloud agent 的开发环境

创建代码通常需要指定环境并运行设置脚本，确保环境就绪。将任务分配给 Copilot 时也是如此，它会像软件工程师一样工作。

Cloud agent 使用 [GitHub Actions][github-actions] 执行任务。可在 `.github/workflows/copilot-setup-steps.yml` 中创建[专用设置工作流][setup-workflow]，让它在开始工作前运行，以安装必要工具和依赖项。本实验预先配置了该工作流，确保 Copilot 可以使用 Node.js、项目依赖、端到端测试浏览器，以及已迁移和填充数据的单体 Astro 应用 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你在存储库的 "Actions" 选项卡中测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为这些步骤设置所需的最低权限。Copilot 会获得自己的令牌来执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果设置步骤不克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在基于 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，为构建和测试提供数据。
      - name: Set up the database
        run: npm run db:setup
```

该文件与其他 GitHub 工作流类似，但有几点需要注意：

- 它只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理 PR 前，GitHub Actions 会先运行此作业。
- `workflow_dispatch` 触发器允许你在存储库的 **Actions** 选项卡中手动运行工作流。这有助于测试工作流，而不必等待 Copilot 启动。

## 添加代码文档

很多项目都存在文档过时或缺失的技术债务，这会降低开发效率，也会增加维护代码库或引入新开发者的难度。Copilot 擅长生成文档，因此适合将这项工作分配给 cloud agent。它会在后台生成文档，后续练习会审查结果。

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 在 **Title** 中输入 `Code lacks documentation`。
6. 在 **Description** 中输入：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的 PR，标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中尚未显示新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会在 PR 中创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才会在 PR 中创建待办事项列表，因为它需要创建环境（运行前面介绍的工作流）、分析项目并确定处理任务的方案。

13. 查看待办事项列表和即将执行的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到状态指示器更新。

16. 浏览实时会话，了解 Copilot 如何探索代码、分析项目状态、暂停思考并确定方案，然后生成代码。

这可能需要几分钟。Copilot cloud agent 的主要目标之一是异步执行任务，让你可以处理其他工作。接下来会利用这一点：再分配一项任务给 cloud agent，然后编写代码为应用添加功能。

## 在游戏详情页添加相关游戏

GitHub Copilot cloud agent 的一大优势是可以拆分工作：你专注于一项任务，代理处理另一项任务。即使设计团队要求的相关游戏功能不需要很长时间，交给代理处理也能让你腾出时间做其他工作。现在将这项任务分配给 Copilot cloud agent。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 在 **Title** 中输入 `Show related games on the game details page`。
6. 在 **Description** 中输入：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题的第一条评论中会出现 👀，表示 Copilot 已开始处理任务。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。它会像软件工程师一样工作，因此无需一直监控，完成后再审查即可。现在可以开始创建和使用自定义代理。

## 总结和后续步骤

本练习介绍了 [GitHub Copilot cloud agent][copilot-agents]。可以将议题分配给 cloud agent，让它异步处理技术债务、开发功能或迁移代码。

本练习涉及以下内容：

- 自定义代码生成环境；
- 了解如何安全地执行操作；
- 认识清晰定义议题的重要性；
- 将议题分配给 Copilot。

Cloud agent 正在后台工作，现在可以开始创建并使用自定义代理。[Copilot cloud agent 也支持 MCP 服务器][cloud-agent-mcp]，并可使用之前练习中介绍的自定义说明。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义代理 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
