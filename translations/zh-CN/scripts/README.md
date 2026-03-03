# Claude Code 脚本

## context-bar.sh

一个用于 Claude Code 的双行状态栏脚本，显示模型、目录、git 分支、未提交文件数量、与 origin 的同步状态、上下文使用情况以及你的上一条消息。

**示例输出：**
```
Opus 4.5 | 📁claude-code-tips | 🔀main (scripts/context-bar.sh uncommitted, synced 12m ago) | ██░░░░░░░░ 18% of 200k tokens
💬 This is good. I don't think we need to change the documentation as long as we don't say that the default color is orange el...
```

### 安装

1. 将脚本复制到你的 Claude 脚本目录：
   ```bash
   mkdir -p ~/.claude/scripts
   cp context-bar.sh ~/.claude/scripts/
   chmod +x ~/.claude/scripts/context-bar.sh
   ```

2. 更新你的 `~/.claude/settings.json`：
   ```json
   {
     "statusLine": {
       "type": "command",
       "command": "~/.claude/scripts/context-bar.sh"
     }
   }
   ```

就是这样！

### 颜色主题

该脚本支持为模型名称和进度条设置可选颜色主题。编辑脚本顶部的 `COLOR` 变量：

```bash
# 颜色主题：gray, orange, blue, teal, green, lavender, rose, gold, slate, cyan
COLOR="orange"
```

运行 `bash scripts/color-preview.sh` 可预览所有选项：

![颜色预览选项](color-preview.png)

### 要求

- `jq`（用于 JSON 解析）
- `bash`
- `git`（可选，用于分支显示）
- Claude Code 2.0.65+（已验证可用；旧版本可能没有所需的 JSON 字段——可查看更早提交以适配旧版本）

### 工作原理

Claude Code 通过 stdin 以 JSON 格式将会话元数据传递给状态栏命令，其中包括：
- `model.display_name` - 模型名称
- `cwd` - 当前工作目录
- `context_window.total_input_tokens` - 已使用的输入 token 总数
- `context_window.total_output_tokens` - 已使用的输出 token 总数
- `context_window.context_window_size` - 最大上下文窗口大小
- `transcript_path` - 会话 transcript JSONL 文件路径

脚本使用这些 JSON 字段来计算上下文使用量（输入 + 输出 token），并显示占上下文窗口的百分比。使用 `/context` 可查看精确的 token 明细。