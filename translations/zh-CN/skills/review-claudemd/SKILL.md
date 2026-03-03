---
name: review-claudemd
description: 审查近期对话以改进 CLAUDE.md 文件。
---

# 从对话历史中审查 CLAUDE.md

分析近期对话，以改进全局（~/.claude/CLAUDE.md）和本地（项目）CLAUDE.md 文件。

## 步骤 1：查找对话历史

项目的对话历史位于 `~/.claude/projects/`。文件夹名称为项目路径，将斜杠替换为短横线。

```bash
# 查找项目文件夹（将 / 替换为 -）
PROJECT_PATH=$(pwd | sed 's|/|-|g' | sed 's|^-||')
CONVO_DIR=~/.claude/projects/-${PROJECT_PATH}
ls -lt "$CONVO_DIR"/*.jsonl | head -20
```

## 步骤 2：提取近期对话

将最近的 15-20 条对话（不包含当前对话）提取到临时目录：

```bash
SCRATCH=/tmp/claudemd-review-$(date +%s)
mkdir -p "$SCRATCH"

for f in $(ls -t "$CONVO_DIR"/*.jsonl | head -20); do
  basename=$(basename "$f" .jsonl)
  # 如已知则跳过当前对话
  cat "$f" | jq -r '
    if .type == "user" then
      "USER: " + (.message.content // "")
    elif .type == "assistant" then
      "ASSISTANT: " + ((.message.content // []) | map(select(.type == "text") | .text) | join("\n"))
    else
      empty
    end
  ' 2>/dev/null | grep -v "^ASSISTANT: $" > "$SCRATCH/${basename}.txt"
done

ls -lhS "$SCRATCH"
```

## 步骤 3：启动 Sonnet 子代理

启动并行的 Sonnet 子代理来分析对话。每个代理应读取：
- 全局 CLAUDE.md：`~/.claude/CLAUDE.md`
- 本地 CLAUDE.md：`./CLAUDE.md`（如果存在）
- 一批对话文件

给每个代理以下提示模板：

```
Read:
1. Global CLAUDE.md: ~/.claude/CLAUDE.md
2. Local CLAUDE.md: [project]/CLAUDE.md
3. Conversations: [list of files]

Analyze the conversations against BOTH CLAUDE.md files. Find:
1. Instructions that exist but were violated (need reinforcement or rewording)
2. Patterns that should be added to LOCAL CLAUDE.md (project-specific)
3. Patterns that should be added to GLOBAL CLAUDE.md (applies everywhere)
4. Anything in either file that seems outdated or unnecessary

Be specific. Output bullet points only.
```

按大小分批对话：
- 大型（>100KB）：每个代理 1-2 个
- 中型（10-100KB）：每个代理 3-5 个
- 小型（<10KB）：每个代理 5-10 个

## 步骤 4：汇总发现

将所有代理的结果合并为包含以下部分的摘要：

1. **违反的指令** - 已有但未遵守的规则（需要更强措辞）
2. **建议新增 - 本地** - 项目特定模式
3. **建议新增 - 全局** - 适用于所有项目的模式
4. **可能过时** - 可能不再相关的条目

以表格或项目符号呈现。询问用户是否需要起草修改。