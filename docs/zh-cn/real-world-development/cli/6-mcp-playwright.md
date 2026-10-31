---
title: "练习 6 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并使用它在浏览器中体验筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

编写代码不只是写代码。我们还要处理数据、外部服务，并允许 Copilot 使用其他自动化工具。模型上下文协议（MCP）服务器可以扩展 Copilot CLI 内置的能力，为它提供更多工具和服务。

本练习将：

- 了解模型上下文协议（MCP）以及 Copilot CLI 如何使用它；
- 如果尚未配置 Playwright MCP 服务器，则添加该服务器；
- 请代理操作浏览器，体验筛选功能。

## 场景

单元测试和端到端测试很重要，但验证界面更新也需要实际与界面交互。允许 Copilot 像用户一样操作网站，可以进一步自动化修改验证，并增强对功能行为的信心。

## 什么是模型上下文协议（MCP）？

[模型上下文协议（MCP）][mcp-blog-post]让人工智能（AI）代理能够与外部工具和服务通信。通过 MCP，代理可以实时访问最新信息，并代表你执行操作。

代理通过 MCP 服务器访问这些工具和资源。MCP 服务器充当代理与外部工具和服务之间的桥梁，每个服务器都代表代理可以访问的一组工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供管理 GitHub 存储库、议题和拉取请求的应用程序编程接口（API）。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化能力。

还有许多其他 MCP 服务器。GitHub 提供 [MCP 注册表][mcp-registry]，帮助发现生态系统中的项目并参与贡献。

> [!CAUTION]
> 将 MCP 服务器视为项目的其他依赖项。使用前检查源代码、验证发布者，并考虑安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，请添加它，以便 Copilot 像用户一样操作网站。

> [!NOTE]
> 如果 Copilot CLI 已配置 Playwright MCP，请跳过这些设置步骤，直接转到下一部分。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 输入名称 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 请 Copilot 使用 Playwright 体验功能

之前已手动确认功能行为。现在使用 Copilot 中的 Playwright MCP 服务器执行同样的操作。

1. 使用以下提示，请 Copilot 使用 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常无需明确指定 MCP 服务器，Copilot 会自行选择。不过，指出应使用的服务器有助于获得更一致的结果，也能减少一些令牌消耗。

2. 观察 Copilot 在浏览器中执行的步骤。
3. 阅读报告，确认行为符合预期。

Copilot 会启动服务器，使用 Playwright 操作网站，停止服务器并提供报告。

## 总结和后续步骤

已通过 Playwright MCP 服务器，在 Copilot CLI 中使用真实浏览器体验了功能。具体来说，已完成以下工作：

- 了解 MCP 以及 Copilot CLI 如何使用它；
- 如果尚未配置 Playwright MCP 服务器，则添加该服务器；
- 请代理操作浏览器，体验筛选功能。

接下来，[创建 QA 自定义代理][next-lesson]，将技能与浏览器工具结合起来。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [为 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
