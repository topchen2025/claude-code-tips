# Claude Code 脚本

## context-bar.sh

一个用于 Claude Code 的双行状态栏脚本，显示模型、目录、git 分支、未提交文件数、与 origin 的同步状态、上下文使用情况，以及你的最后一条消息。

**示例输出：**
```
Opus 4.5 | 📁claude-code-tips | 🔀main (scripts/context-bar.sh uncommitted, synced 12m ago) | ██░░░░░░░░ 18% of 200k tokens
💬 This is good. I don't think we need to change the documentation as long as we don't say that the default color is orange el...
```

### 安装

1. 将脚本复制到你的 Claude scripts 目录：
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

脚本支持可选的模型名称和进度条颜色主题。编辑脚本顶部的 `COLOR` 变量：

```bash
# 颜色主题：gray, orange, blue, teal, green, lavender, rose, gold, slate, cyan
COLOR="orange"
```

运行 `bash scripts/color-preview.sh` 预览所有选项：

![Color preview options](color-preview.png)

### 要求

- `jq`（用于 JSON 解析）
- `bash`
- `git`（可选，用于分支显示）
- Claude Code 2.0.65+（已验证可用；较旧版本可能没有所需的 JSON 字段——较旧版本请查看早期提交）

### 工作原理

Claude Code 通过 stdin 以 JSON 的形式将会话元数据传递给状态栏命令，包括：
- `model.display_name` - 模型名称
- `cwd` - 当前工作目录
- `context_window.total_input_tokens` - 已使用的总输入 tokens
- `context_window.total_output_tokens` - 已使用的总输出 tokens
- `context_window.context_window_size` - 最大上下文窗口大小
- `transcript_path` - 会话 transcript JSONL 文件的路径

脚本使用这些 JSON 字段计算上下文使用情况（输入 + 输出 tokens），显示上下文窗口的百分比。使用 `/context` 获取精确的 token 分解。