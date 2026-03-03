---
name: review-claudemd
description: 审查最近的对话，找出可改进 CLAUDE.md 文件的地方。
---

# 根据对话历史审查 CLAUDE.md

分析最近的对话，以改进全局（~/.claude/CLAUDE.md）和本地（项目）CLAUDE.md 文件。

## 第 1 步：查找对话历史

项目的对话历史位于 `~/.claude/projects/`。文件夹名称是将项目路径中的斜杠替换为短横线后的结果。

```bash
# 查找项目文件夹（将 / 替换为 -）
PROJECT_PATH=$(pwd | sed 's|/|-|g' | sed 's|^-||')
CONVO_DIR=~/.claude/projects/-${PROJECT_PATH}
ls -lt "$CONVO_DIR"/*.jsonl | head -20
```

## 第 2 步：提取最近的对话

将最近的 15-20 个对话（不包括当前对话）提取到一个临时目录：

```bash
SCRATCH=/tmp/claudemd-review-$(date +%s)
mkdir -p "$SCRATCH"

for f in $(ls -t "$CONVO_DIR"/*.jsonl | head -20); do
  basename=$(basename "$f" .jsonl)
  # 如果已知则跳过当前对话
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

## 第 3 步：启动 Sonnet 子代理

启动并行的 Sonnet 子代理来分析对话。每个代理应读取：
- 全局 CLAUDE.md：`~/.claude/CLAUDE.md`
- 本地 CLAUDE.md：`./CLAUDE.md`（如果存在）
- 一批对话文件

给每个代理以下提示词模板：

```
读取：
1. 全局 CLAUDE.md：~/.claude/CLAUDE.md
2. 本地 CLAUDE.md：[project]/CLAUDE.md
3. 对话：[文件列表]

根据这两个 CLAUDE.md 文件分析对话。找出：
1. 已存在但被违反的指令（需要强化或改写）
2. 应添加到 LOCAL CLAUDE.md 的模式（项目特定）
3. 应添加到 GLOBAL CLAUDE.md 的模式（适用于所有地方）
4. 两个文件中看起来已过时或不必要的内容

请具体说明。仅输出项目符号列表。
```

按大小对对话进行分批：
- 大（>100KB）：每个代理 1-2 个
- 中（10-100KB）：每个代理 3-5 个
- 小（<10KB）：每个代理 5-10 个

## 第 4 步：汇总发现

将所有代理的结果合并为摘要，包含以下部分：

1. **已违反的指令** - 未被遵循的现有规则（需要更强措辞）
2. **建议新增 - LOCAL** - 项目特定模式
3. **建议新增 - GLOBAL** - 适用于所有地方的模式
4. **可能已过时** - 可能不再相关的条目

以表格或项目符号列表呈现。询问用户是否希望起草编辑内容。