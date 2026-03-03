# 5 条新的 Claude Code 提示

不久前，我发布了 [32 Claude Code Tips: From Basics to Advanced](https://agenticcoding.substack.com/p/32-claude-code-tips-from-basics-to)。这里快速更新 5 个新提示。

## 1. `/copy` 命令

将 Claude 输出从终端中取出最简单的方式。输入 `/copy`，它会将 Claude 的最后一次回复作为 markdown 复制到剪贴板。

## 2. `/fork` 和 `--fork-session`

Claude Code 现在内置了对话分叉功能：

- `/fork` - 在对话中分叉
- `--fork-session` - 与 `--resume` 或 `--continue` 一起使用（例如 `claude -c --fork-session`）

由于 `--fork-session` 没有短格式，我创建了一个 shell 函数用 `--fs` 作为快捷方式。[你可以在这里看到](https://github.com/ykdojo/claude-code-tips?tab=readme-ov-file#tip-23-clonefork-and-half-clone-conversations)。

## 3. Plan 模式用于上下文交接

通过 `/plan` 或 Shift+Tab 进入 plan 模式。让 Claude 收集下一个代理需要的全部上下文：

> I just enabled plan mode. Bring over all of the context that you need for the next agent. The next agent will not have any other context, so you'll need to be pretty comprehensive.

完成后，选择选项 1（“Yes, clear context and auto-accept edits”）以仅使用计划重新开始。新的 Claude 实例只看到计划，没有旧对话的负担。

## 4. 定期审查 CLAUDE\.md

你的 CLAUDE.md 文件会随着时间而过时。几周前合理的指令可能现在不再相关。我创建了一个 `review-claudemd` 技能，分析你最近的对话并提出改进建议。[你可以在这里查看](https://github.com/ykdojo/claude-code-tips/tree/main/skills/review-claudemd)。

## 5. 使用 Parakeet 进行语音转录

我一直使用语音转录与 Claude Code 交流，而不是打字。我刚刚为 [Super Voice Assistant](https://github.com/ykdojo/super-voice-assistant)（开源）添加了 Parakeet 支持，速度非常快 - Parakeet v2 以约 110 倍实时速度运行，词错误率为 1.69%。对 Claude Code 来说足够准确。