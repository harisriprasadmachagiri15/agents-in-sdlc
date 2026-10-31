---
title: "练习 9 - GitHub Copilot CLI 斜杠命令"
description: "探索管理上下文、选择模型、共享会话和选择性委派给云端代理的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

和其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。它们可以访问高级功能、幕后信息和其他配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。下面将介绍其他实用命令。

本练习将：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文；
- 使用 `/model` 查看可用模型；
- 了解 `/share` 如何导出或共享会话；
- 探索用于并行工作、工作树和委派任务给云端代理的可选命令。

## 场景

核心 CLI 工作流已完成。现在了解其他功能，包括管理上下文、切换模型、共享会话，以及选择性地将任务委派给 [Copilot cloud agent][about-cloud-agent]。

## 查看 Copilot CLI 上下文

处理较大或较复杂的任务时，可能会接近模型上下文窗口的容量上限。Copilot CLI 会在需要时自动压缩对话；你也可以使用斜杠命令查看或手动压缩。

1. 返回 Codespace；如果 Copilot CLI 尚未打开，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 注意模型、当前令牌用量，以及上下文在系统说明、工具、消息和可用空间之间的分布。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> 上下文窗口接近容量上限时，Copilot CLI 会自动压缩上下文。使用 `/compact` 可自行选择压缩时机。切换到无关任务并需要全新对话时，使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，开发者的偏好也不同。Copilot CLI 可列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型和用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队合作和分享经验有助于所有人改进人工智能（AI）工具的使用方式。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看当前版本中的 `/share` 选项。
2. 如果要共享此会话，请选择合适的目标，例如 `/share file`，将其导出为本地 Markdown 文件。
3. 发送或发布导出内容前先进行检查。会话导出可能包含提示、回复和项目信息。

发布链接或 gist 是可选操作。不要发布团队无意共享的存储库或对话内容。

## 可选：扩展工作或委派任务

核心工作坊已完成。Copilot CLI 还为较大的任务提供了以下命令：

- `/fleet` 可将彼此独立的子任务分配给多个子代理并行运行。
- `/worktree` 可为单独任务创建隔离的 Git 工作树。
- `/delegate` 可将任务发送给 Copilot cloud agent，代理会异步工作，并可能创建拉取请求。

这些命令是可选的，因为它们可能创建额外工作树或触发远程操作。尝试前先启动一项范围清晰的新任务，并按常规流程审查结果。若要深入了解异步代理工作，可继续学习[云端代理工作坊][cloud-workshop]。

## 总结和后续步骤

使用 Copilot CLI 斜杠命令可以配置工具、共享会话并查看幕后运行情况。具体来说，已完成以下工作：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文；
- 使用 `/model` 查看可用模型；
- 了解 `/share` 如何导出或共享会话；
- 探索用于并行工作、工作树和委派任务给云端代理的可选命令。

Copilot CLI 还有更多斜杠命令值得探索。接下来，[回顾所学内容][next-lesson]并了解后续学习资源。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 上下文管理][context-management]
- [关于 Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
