---
name: reddit-fetch
description: 当 WebFetch 被阻止时，使用 Gemini CLI 获取 Reddit 内容。用于访问 Reddit URL、在 Reddit 上研究主题，或当 Reddit 返回 403/阻止错误时。
---

# 通过 Gemini CLI 获取 Reddit 内容

当 WebFetch 无法访问 Reddit（被阻止、403 等）时，请通过 tmux 使用 Gemini CLI。

选择一个唯一的会话名称（例如 `gemini_abc123`），并在整个过程中始终一致地使用它。

## 设置

```bash
tmux new-session -d -s <session_name> -x 200 -y 50
tmux send-keys -t <session_name> 'gemini -m gemini-3-pro-preview' Enter
sleep 3  # 等待 Gemini CLI 加载
```

## 发送查询并捕获输出

```bash
tmux send-keys -t <session_name> 'Your Reddit query here' Enter
sleep 30  # 等待响应（按需调整，复杂搜索最多可达 90 秒）
tmux capture-pane -t <session_name> -p -S -500  # 捕获输出
```

## 如何判断 Enter 是否已发送

请专门查找 YOUR QUERY TEXT。它是在边框框内还是框外？

**Enter 未发送** - 你的查询在框内：
```
╭─────────────────────────────────────╮
│ > Your actual query text here       │
╰─────────────────────────────────────╯
```

**Enter 已发送** - 你的查询在框外，后面会有活动输出：
```
> Your actual query text here

⠋ Our hamsters are working... (processing)

╭────────────────────────────────────────────╮
│ >   Type your message or @path/to/file     │
╰────────────────────────────────────────────╯
```

注意：空提示 `Type your message or @path/to/file` 总是会显示在框内——这是正常现象。关键在于 YOUR 查询文本是在框内还是框外。

如果你的查询在框内，请运行 `tmux send-keys -t <session_name> Enter` 进行提交。

## 完成后清理

```bash
tmux kill-session -t <session_name>
```