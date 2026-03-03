---
name: half-clone
description: 克隆当前对话的后半部分，丢弃较早的上下文，以减少 token 使用量同时保留最近的工作。
---

克隆当前对话的后半部分，丢弃较早的上下文，以减少 token 使用量同时保留最近的工作。

步骤：
1. 获取当前会话 ID 和项目路径：`tail -1 ~/.claude/history.jsonl | jq -r '[.sessionId, .project] | @tsv'`
2. 使用 bash 查找 half-clone-conversation.sh：`find ~/.claude -name "half-clone-conversation.sh" 2>/dev/null | sort -V | tail -1`
   - 无论通过插件安装还是手动符号链接，都能找到该脚本
   - 使用版本排序，如果存在多个版本，将优先使用最新版本
3. 预览对话以验证会话 ID：`<script-path> --preview <session-id> <project-path>`
   - 检查首条和末尾消息与当前对话是否匹配
4. 运行克隆：`<script-path> <session-id> <project-path>`
   - 始终使用历史记录中的项目路径，而非当前工作目录
5. 告知用户可以使用 `claude -r` 访问半克隆对话，并查找标记为 `[HALF-CLONE <timestamp>]`（例如 `[HALF-CLONE Jan 7 14:30]`）的条目。脚本会自动在克隆文件末尾追加对原始对话的引用。