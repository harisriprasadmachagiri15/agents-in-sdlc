---
title: "练习 3 - 代理模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案，再从议题出发使用 Autopilot 构建筛选功能，并通过 Interactive 模式审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们刚刚为项目添加了一项小功能。较大的修改需要更完善的流程。GitHub Copilot CLI 可以融入组织现有的工作流，帮助团队以正确方式构建正确的功能。接下来的几节将演示典型的代理辅助开发流程：从议题开始创建功能，确保代码有效、行为符合预期，最后成功合并到项目中。

> [!NOTE]
> 后续功能开发会继续使用同一对话和分支。通常会为不同文件类型使用不同分支或拉取请求，但这里会简化流程，以便聚焦核心概念。

本练习将：

- 从 GitHub 议题启动新的 Copilot 对话；
- 在 Plan 模式中明确需求；
- 使用 Autopilot 模式实现新功能；
- 审查代码；
- 在转发的浏览器中手动验证功能。

继续开发此功能时，还会更新存储库说明、自定义现有 `quality-checks` 技能、添加 MCP 验证、创建 QA 代理并打开功能拉取请求。

## 场景

Tailspin Toys 的游戏目录不断扩大，访问者需要按类别和发行商筛选游戏。待办事项中的议题描述了该功能，但组合类别等细节需要在编码前确定。先使用 Plan 模式确定这些决策，再授权 Autopilot 在限定范围内实施。

## 背景

在开发流程中引入人工智能（AI）编码代理并不会改变基本原则，反而使这些原则更加重要。大多数开发者采用的流程大致如下：

1. 从描述需求的议题开始。
2. 制定实现计划。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但大多数流程都与上述步骤类似。

遵循标准流程可以确保 AI 生成的代码符合需求，并经过与手动编写的代码相同的审核过程。

## 对话模式

**对话模式**控制代理的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与代理协作。代理提出修改建议，并等待你输入后再继续。
- **Plan**：代理先制定计划，但不能编辑项目文件。
- **Autopilot**：代理自主编写代码、运行测试并迭代，直至任务完成。

先使用 Plan 模式，审查计划后再用 Autopilot 实施。

## 从议题开始

开始筛选功能之前，先返回 Codespace，确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示中并返回 **Session** 选项卡。

现在提示中应出现 `#7`（或类似编号）。`#` 可将 GitHub 议题或拉取请求添加到上下文中。

## 规划筛选功能

规划可以在交给 Copilot 实施之前先确定功能方案。对于复杂任务，花些时间规划通常很有帮助。现在切换到 Plan 模式，让 Copilot 创建计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会先探索项目，再确定合适的实现方案。

3. Copilot 可能会询问筛选行为的具体要求。请根据自己的偏好回答，没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 浏览计划。
6. 如果计划与已确定的决策不符，请让 Copilot 修改。

## 批准 Autopilot

计划撰写并审查完毕后，就可以开始实施。使用 Autopilot 让 Copilot 自行完成工作。

Autopilot 会让 Copilot 持续处理问题，直至它认为任务已完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前版本中含义相近的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实施，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 审查并验证实现

与其他代码一样，生成的代码也必须在合并前审查。现在检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 将结果与议题及规划时的决策进行比较。
4. 审查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 检查项目检查结果；如果失败，请让 Copilot 修复。

## 体验新功能

代码看起来没问题，但实际运行情况如何？像之前一样启动应用，通过 Codespaces 转发端口打开网站。

1. 请 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别、发行商以及计划中确定的组合进行筛选。
4. 确认重置和无结果时的行为符合议题和已确定的方案。
5. 返回 Codespace，请 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

本练习使用不同对话模式构建并审查了功能。具体来说，已完成以下工作：

- 从 GitHub 议题启动新的 Copilot 对话；
- 在 Plan 模式中明确需求；
- 使用 Autopilot 模式实现新功能；
- 审查代码；
- 在转发的浏览器中手动验证功能。

接下来，通过[自定义说明][next-lesson]深入了解代码生成，确保生成的代码遵循已记录的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
