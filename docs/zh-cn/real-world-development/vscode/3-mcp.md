---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：代理模式][previous-lesson] · [下一课：自定义代理 →][next-lesson]

你刚刚使用代理模式构建了筛选功能。创建拉取请求（PR）之前，应先在浏览器中确认功能正常。接下来连接 **Playwright 模型上下文协议（MCP）服务器**，让 Copilot 操作真实浏览器测试功能，然后发布分支并创建 PR。

本练习将：

- 了解模型上下文协议（MCP）以及 MCP 服务器如何为 Copilot 扩展工具；
- 在工作区中添加 Playwright MCP 服务器；
- 请 Copilot 使用服务器在浏览器中手动测试筛选功能；
- 发布分支并为筛选功能创建拉取请求。

## 什么是模型上下文协议（MCP）？

当代理模式可以访问编辑器以外的工具时，能力会大幅增强。模型上下文协议（MCP）为 Copilot 提供了这种能力，是代理与外部工具和服务通信的标准方式。

![代理模式及其与上下文、大型语言模型（LLM）和工具交互的示意图，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议（MCP）](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)让人工智能（AI）代理能够与外部工具和服务通信。代理可以通过资源访问最新信息，并通过工具代表你执行操作。

代理通过 MCP 服务器访问这些工具和资源。MCP 服务器充当代理与外部工具和服务（例如现有 API 或 npm 包等本地工具）之间的桥梁。每个服务器都代表代理可以访问的一组工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供管理 GitHub 存储库的应用程序编程接口（API），可创建存储库、更新内容、管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力，可浏览网页、填写表单和选择按钮。

还有许多其他 MCP 服务器提供不同工具和资源。GitHub 提供 [MCP 注册表](https://github.com/mcp)，帮助发现生态系统中的项目并参与贡献。

> [!CAUTION]
> 将 MCP 服务器视为项目的其他依赖项。使用前仔细检查源代码、验证发布者并考虑安全影响。只使用信任的 MCP 服务器，并谨慎授予敏感资源或操作的访问权限。

## 检查 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 确认文件中已配置 `github` 服务器：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

项目模板已包含 `github` 条目，因此 Copilot 能在上一练习中读取待办议题。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装。VS Code 通过 HTTP 连接，Copilot 首次使用其工具时需要使用 GitHub 身份验证。

## 添加 Playwright MCP 服务器

接下来添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]可为 Copilot 提供可控浏览器，用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，与 `github` 并列。文件内容如下：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在本机运行 `npx @playwright/mcp@latest` 启动它。`--headless` 参数会让 Playwright 在后台运行浏览器，无需显示窗口；Codespace 没有可显示浏览器的桌面，因此必须使用此参数。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 运行端到端测试，因此通常已安装 Playwright 所需的浏览器。如果 Copilot 报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会按需启动 MCP 服务器。Copilot 首次需要使用服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它以启动服务器并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请检查配置并选择信任，以便启动服务器。

手动启动是可选操作。如果跳过，VS Code 会在下一步 Copilot 首次需要这些工具时启动服务器，并询问是否信任。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]可让 Copilot 操作真实浏览器。代理可以打开页面、应用筛选并读取结果，然后向你总结观察到的情况。无需离开对话即可快速验证功能行为。

Playwright MCP 服务器使用页面的[无障碍树][playwright-mcp-server]，而不是截图。因此，代理会依据结构化且带标签的元素（如按钮、链接和列表项）进行判断，就像辅助技术一样。这样，简单的功能检查也能兼顾基本的无障碍验证。

确认服务器已连接且应用正在运行后，向 Copilot 发送以下提示，让它测试筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、执行测试并汇报结果。对照议题中的验收标准审查报告；如果发现异常，请追问或让 Copilot 修复代码后再创建拉取请求。

> [!NOTE]
> 测试时应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请重新启动后再发送提示。Copilot 首次使用 Playwright MCP 服务器时可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium` 并重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求（PR），供团队审查。先发布 `filtering-vscode` 分支。

1. 在 Codespace 中打开 **Source Control** 面板，审查 Copilot 所做的修改。
2. 选择 **+** 图标暂存修改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![Source Control 面板显示所做修改的截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行界面（CLI）创建拉取请求。也可以请已连接 GitHub 工具的 Copilot 查找相关议题并关联到 PR。

1. 打开 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 在代理下拉菜单中确认选择 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 请 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 获取信息并执行操作。首次使用 GitHub 工具时，可能会提示进行 GitHub 身份验证，请按提示操作。
5. 查看 Copilot 如何搜索议题、找到相关议题并创建 PR。
6. 选择 Copilot 提供的链接查看拉取请求，但**暂时不要合并**。

## 总结和后续步骤

本练习中，你：

- 了解 MCP 以及 MCP 服务器如何为 Copilot 扩展工具；
- 在工作区中添加 Playwright MCP 服务器；
- 在发布前使用它在浏览器中手动测试筛选功能；
- 发布分支并为筛选功能创建拉取请求。

MCP 服务器可用于测试功能，但它只是扩展 Copilot 的一种方式。接下来，[创建自定义代理][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
