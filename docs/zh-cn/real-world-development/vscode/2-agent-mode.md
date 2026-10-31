---
title: "练习 2 - 使用 Copilot 代理模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用修改，通常也涉及多个文件以及运行测试等操作。开发者需要找到相关文件、进行修改、运行测试、调试、找出遗漏文件并再次修改，流程可能很繁琐。

Copilot 代理模式可以帮助处理这些工作。

代理模式能在集成开发环境（IDE）中更自主地工作。它会先探索项目结构，查找需要修改的文件，执行修改、运行测试，并自动修复发现的问题。接下来了解如何使用代理模式为网站添加新功能。

> [!NOTE]
> 代理模式和 cloud agent 名称相似，但适用于不同的工作方式。代理模式在 IDE 中执行任务，可快速反馈并随时交互；cloud agent 则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将了解如何：

- 使用 Copilot 代理模式探索项目、识别相关文件并协调多文件修改；
- 使用 GitHub Copilot 代理模式跨用户界面（UI）和数据层实现新功能；
- 在合并前审查 Copilot 代理模式生成的修改和测试。

## 场景

随着游戏列表不断增加，用户需要按类别和发行商筛选。上一练习已添加发行商辅助函数，现在将使用 Copilot 代理模式完成其余数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

先查看 Tailspin Toys 网站，了解现有功能。

该网站是一个带开发者主题的棋盘游戏众筹平台，可列出游戏并显示详情。它是一个单体 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 front matter 使用 Drizzle ORM 直接查询本地 SQLite 数据库，没有独立的后端应用程序编程接口（API）或客户端用户界面（UI）框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能由使用标准文档对象模型（DOM）API 的小型 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单体 Astro 开发服务器。通过以下步骤在 GitHub Codespace 中运行网站：

1. 返回 Codespace。上一练习结束后应已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 更新主分支并为筛选功能创建分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否在浏览器窗口中打开 `http://localhost:4321`，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 Codespace 时，选择终端中的 localhost 链接会自动跳转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是托管 Web 服务器的 Codespace 私有隧道。

### 浏览网站

网站启动后，可以浏览现有功能：

- **主页**：显示游戏列表，包括标题、图片和描述。
- **游戏详情页**：选择游戏后可查看标题、描述、发行商和类别等详细信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。不同练习所需的模式和模型可能不同，相关步骤会注明。

网站的初始实现已可正常使用，但还需要增加功能。先查看待办事项。使用模板创建项目时，系统已自动创建议题；请 GitHub Copilot 列出这些议题。

1. 在 Chat 视图的代理下拉菜单中选择 **Agent**。**Agent** 代理可自主规划并跨文件实现修改、运行终端命令和调用工具。

   ![Chat 视图中代理选择器的截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并未推荐任何特定模型。编写本工作坊时使用了 Claude Sonnet 4.5，因此示例使用了该模型，以便尽量保持体验一致。由于大型语言模型（LLM）具有概率性，实际建议可能不同，这是正常现象。

> [!NOTE]
> 由于大型语言模型（LLM）具有概率性，Copilot 可能使用不同的模型上下文协议（MCP）命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问存储库待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

Copilot 还会根据它认为对用户最有用的程度为议题排序。

## 查看说明文件

让代理生成代码前，先查看为 Copilot 提供上下文的说明文件。本练习将使用 [UI 说明文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中介绍了如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看整体指导，包括：
   - 项目架构概览；
   - 组件设计、可测试性和无障碍原则；
   - 指向 Astro、Tailwind CSS 和 Drizzle 等特定文件类型说明的链接。

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于说明文件所在位置。这样可复用说明、拆分复杂内容，并提供示例和模板。

## 实现筛选功能

完成筛选至少需要对应用做三项修改：

- 在数据层（`src/lib/`）添加或完善筛选逻辑；
- 添加或更新筛选测试；
- 更新游戏列表页，添加筛选 UI。

此外，合并前必须运行并通过测试。Copilot 代理模式可以完成这些任务。

1. 继续当前对话，或选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 `ui` 说明文件。

   ![选择 UI 说明文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的代理下拉菜单中仍选中 **Agent**。

   ![Chat 视图中代理选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选中 **Claude Sonnet 4.5**。
5. 使用以下提示，请 Copilot 根据待办议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目并查找相关数据层、UI 和测试文件，然后修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会探索项目、修改文件并运行测试。根据任务和代码库的复杂度，这可能需要几分钟。过程中代码编辑器可能会显示 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有修改都会显示 **Keep** 或 **Undo**，无需在工作过程中选择。

7. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 面板询问是否允许运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 处理中可能会暂停，测试也可能暂时失败。Copilot 会在生成代码和运行测试之间迭代，直至任务完成且未发现错误。

   ![Copilot 代理模式完整聊天会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在问题。

> [!CAUTION]
> 始终要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回运行网站的浏览器，体验新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

本练习介绍了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加功能。你已了解如何：

- 使用 GitHub Copilot 代理模式跨 UI 和数据层实现功能；
- 使用 Copilot 代理模式探索项目、识别相关文件并协调多文件修改；
- 在合并前审查 Copilot 代理模式生成的修改和测试。

接下来，[使用 Playwright MCP 服务器测试功能][next-lesson]，然后创建拉取请求。

### 额外探索练习 — 实现分页

随着游戏列表增大，需要添加分页。运用本练习所学知识，请 Copilot 更新网站以实现分页。编写提示时可考虑：

- 遵循现有最佳实践，包括使用已有说明文件；
- 确定分页方式，例如允许用户选择页面大小，或将其设为固定值；
- 为 Copilot 提供实现所需的指导；
- 通过后续提示和补充上下文迭代修改。这是正常的协作流程。

## 资源

- [Copilot ask、edit 和 agent 模式：功能和适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
