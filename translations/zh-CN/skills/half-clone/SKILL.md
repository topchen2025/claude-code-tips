---
name: half-clone
description: 克隆当前对话的后半部分，丢弃较早的上下文，以减少 token 使用量，同时保留最近的工作内容。
---

克隆当前对话的后半部分，丢弃较早的上下文，以减少 token 使用量，同时保留最近的工作内容。

步骤：
1. 获取当前会话 ID 和项目路径：`tail -1 ~/.claude/history.jsonl | jq -r '[.sessionId, .project] | @tsv'`
2. 使用 bash 查找 half-clone-conversation.sh：`find ~/.claude -name "half-clone-conversation.sh" 2>/dev/null | sort -V | tail -1`
   - 这会找到该脚本，无论它是通过插件安装还是手动符号链接安装的
   - 如果存在多个版本，则使用版本排序以优先选择最新版本
3. 预览对话以验证会话 ID：`<script-path> --preview <session-id> <project-path>`
   - 检查第一条和最后一条消息是否与当前对话匹配
4. 运行克隆：`<script-path> <session-id> <project-path>`
   - 始终传入 history 条目中的项目路径，而不是当前工作目录
5. 告诉用户他们可以使用 `claude -r` 访问半克隆后的对话，并查找标记为 `[HALF-CLONE <timestamp>]` 的那一项（例如，`[HALF-CLONE Jan 7 14:30]`）。该脚本会在克隆文件末尾自动附加对原始对话的引用。