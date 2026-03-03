---
name: clone
description: 克隆当前对话，以便用户可以分支并尝试不同的方法。
---

克隆当前对话，以便用户可以分支并尝试不同的方法。

步骤：
1. 获取当前会话 ID 和项目路径：`tail -1 ~/.claude/history.jsonl | jq -r '[.sessionId, .project] | @tsv'`
2. 使用 bash 查找 clone-conversation.sh：`find ~/.claude -name "clone-conversation.sh" 2>/dev/null | sort -V | tail -1`
   - 无论通过插件安装还是手动符号链接，都能找到脚本
   - 使用版本排序在存在多个版本时优先选择最新版本
3. 运行：`<script-path> <session-id> <project-path>`
   - 始终使用历史记录中的项目路径，而不是当前工作目录
4. 告诉用户可以使用 `claude -r` 访问克隆的对话，并找到标记为 `[CLONED <timestamp>]` 的对话（例如 `[CLONED Jan 7 14:30]`）