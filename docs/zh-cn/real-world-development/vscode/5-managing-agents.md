---
title: "练习 5 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义代理][previous-lesson] |
|:--|

Copilot 处于代理模式时，会自主探索代码库、提出修改、编辑文件并运行命令。由于这些工作实时发生在本机，VS Code 中的 Copilot Chat 会显示每次工具调用和文件修改。你可以查看内嵌差异、接受或拒绝单项修改，并通过后续提示引导对话，无需离开编辑器即可完善或扩展工作。

本练习将：

- 实时监控无障碍代理的工作；
- 审查并接受建议的文件修改；
- 在会话进行中引导代理，添加与高对比度模式并列的浅色模式开关。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义代理正在实现高对比度模式。开发过程中，设计团队又提出了浅色模式开关需求，以便偏好较浅主题的用户使用。无需从头开始新对话，可以引导现有会话扩展功能。

## 监控 Copilot 进度

Copilot Chat 视图会显示代理执行的每个步骤，包括探索的文件、运行的命令和提出的修改。每项文件修改都会显示内嵌差异和 **Keep** / **Undo** 控件。

1. 返回练习 4 中使用的 Copilot Chat 视图。
2. 代理工作时滚动聊天记录，查看工具调用，包括文件读取、文件修改和请求运行的终端命令。
3. 代理提出文件修改时，差异会内嵌显示：
   - 选择 **Keep** 接受修改；
   - 选择 **Undo** 撤销修改，让代理重新处理。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看此会话中代理修改的全部文件。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），代理会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待代理完成高对比度模式。完成后应会显示总结修改内容的消息。

## 引导代理添加浅色模式

高对比度模式完成后，在同一对话中添加浅色模式开关。继续使用当前聊天会话时，代理会保留此前工作的上下文，包括修改的组件、样式方案和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. 代理会检查之前的修改，并提出添加浅色模式的修改。
3. 像审查高对比度功能一样，逐项检查新的修改，并使用 **Keep** 或 **Undo** 进行处理。
4. 如果代理的方案不符合预期，例如试图以不希望的方式重构开关用户界面（UI），可以选择聊天输入框旁的 **Stop** 按钮停止任务，然后发送澄清提示。

> [!TIP]
> 使用后续提示引导进行中的会话，通常比从头开始更快。可以要求调整（“简化 CSS，改用 CSS 变量，避免重复选择器”）、纠正方向（“不要添加第三个开关，将浅色模式整合到现有设置面板”），或扩展范围（“为两个开关添加键盘快捷方式”）。

## 审查工作集

提交修改前，快速检查代理修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 检查所有变更文件。应包含 Astro 组件、样式和相关测试。
3. 打开部分变更文件并检查差异。确认实现体现了自定义代理提出的无障碍模式，包括无障碍富互联网应用（ARIA）属性、键盘导航、语义化 HTML 和本地存储持久化。
4. 确认满意后，在 Source Control 面板中暂存并提交修改。下一课会发布这些修改。

## 总结和后续步骤

你使用 Copilot Chat 代理模式监控并引导了进行中的会话，检查每项文件修改、接受或拒绝修改，并通过后续提示扩展工作，同时保留上下文。

本练习中，你：

- 实时监控代理的工作；
- 接受或拒绝单项文件修改；
- 通过后续提示引导进行中的会话。

接下来，在[下一课][next-lesson]中发布无障碍功能并创建拉取请求，同时审查本地自定义代理在整个实验中的工作。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用代理模式][agent-mode]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
