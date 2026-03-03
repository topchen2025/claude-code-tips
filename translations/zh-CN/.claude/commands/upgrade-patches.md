---
description: 将 system prompt 补丁升级到最新的 Claude Code 版本
---

将 system prompt 补丁升级到最新的 Claude Code 版本。

1. 运行 `npm view @anthropic-ai/claude-code version` 以获取最新版本
2. 列出 `system-prompt/` 下的版本目录，以找到最近的已打补丁版本
3. 如果最新版本的补丁已存在，报告该情况并停止
4. 如果不存在，按照 `system-prompt/UPGRADING.md` 进行升级
5. 务必完成 UPGRADING.md 底部的**完整**最终验证清单——三种构建类型（npm、native-linux、native-macos）以及矩阵中的所有测试，而不只是其中一个。对 `/context` 这类交互式测试请使用 tmux。