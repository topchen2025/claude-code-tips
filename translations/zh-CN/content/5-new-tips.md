# 5 个新的 Claude Code 技巧

不久前，我发布了 [32 Claude Code Tips: From Basics to Advanced](https://agenticcoding.substack.com/p/32-claude-code-tips-from-basics-to)。这里是一个快速更新，再补充 5 个技巧。

## 1. `/copy` 命令

把 Claude 的输出从终端拿出来的最简单方式。只需输入 `/copy`，它就会把 Claude 的上一条回复以 markdown 格式复制到你的剪贴板。

## 2. `/fork` 和 `--fork-session`

Claude Code 现在内置了会话分叉功能：

- `/fork` - 在会话内部进行分叉
- `--fork-session` - 与 `--resume` 或 `--continue` 一起使用（例如：`claude -c --fork-session`）

由于 `--fork-session` 没有短写形式，我创建了一个 shell function，用 `--fs` 作为快捷方式。[你可以在这里查看](https://github.com/ykdojo/claude-code-tips?tab=readme-ov-file#tip-23-clonefork-and-half-clone-conversations)。

## 3. 用于上下文交接的计划模式

使用 `/plan` 或 Shift+Tab 进入计划模式。让 Claude 收集下一个 agent 所需的全部上下文：

> 我刚启用了计划模式。把下一个 agent 需要的所有上下文都整理过来。下一个 agent 不会有任何其他上下文，所以你需要尽可能全面。

完成后，选择 Option 1（"Yes, clear context and auto-accept edits"），即可仅基于计划重新开始。新的 Claude 实例只会看到计划，不会带上旧会话的包袱。

## 4. 定期审查 CLAUDE\.md

你的 CLAUDE.md 文件会随着时间推移而过时。几周前还合理的说明，现在可能已经不再相关。我创建了一个 `review-claudemd` skill，它会分析你最近的会话并给出改进建议。[你可以在这里查看](https://github.com/ykdojo/claude-code-tips/tree/main/skills/review-claudemd)。

## 5. 用 Parakeet 进行语音转写

我一直在用语音转写和 Claude Code 对话，而不是打字。我刚刚在 [Super Voice Assistant](https://github.com/ykdojo/super-voice-assistant)（开源）里加入了对 Parakeet 的支持，速度非常快——Parakeet v2 运行速度约为实时的 110 倍，词错误率为 1.69%。对 Claude Code 来说已经足够准确。