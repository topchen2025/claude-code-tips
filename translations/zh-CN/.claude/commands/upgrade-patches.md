---
description: 将系统提示补丁升级到最新的 Claude Code 版本
---

将系统提示补丁升级到最新的 Claude Code 版本。

1. 运行 `npm view @anthropic-ai/claude-code version` 获取最新版本
2. 列出 `system-prompt/` 下的版本目录以找到最近的已打补丁版本
3. 如果最新版本的补丁已存在，告知并停止
4. 如果不存在，遵循 `system-prompt/UPGRADING.md` 进行升级
5. 始终完成 UPGRADING.md 底部的**完整**最终验证检查列表——所有三种构建类型 (npm, native-linux, native-macos) 以及矩阵中的所有测试，而不仅仅是其中之一。对于像 `/context` 这类交互式测试，请使用 tmux。