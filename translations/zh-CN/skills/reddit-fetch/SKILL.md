---
name: reddit-fetch
description: 当 WebFetch 被阻止时，使用 Gemini CLI 从 Reddit 获取内容。可用于访问 Reddit URL、在 Reddit 上研究主题，或 Reddit 返回 403/被阻止错误时使用。
---

# 通过 Gemini CLI 获取 Reddit

当 WebFetch 无法访问 Reddit（被阻止、403 等）时，通过 tmux 使用 Gemini CLI。

选择一个唯一的会话名称（例如，`gemini_abc123`），并始终如一地使用它。

## 设置

```bash
tmux new-session -d -s <session_name> -x 200 -y 50
tmux send-keys -t <session_name> 'gemini -m gemini-3-pro-preview' Enter
sleep 3  # 等待 Gemini CLI 加载
```

## 发送查询并捕获输出

```bash
tmux send-keys -t <session_name> 'Your Reddit query here' Enter
sleep 30  # 等待响应（根据需要调整，复杂搜索最多可达 90 秒）
tmux capture-pane -t <session_name> -p -S -500  # 捕获输出
```

## 如何判断是否发送了 Enter

具体查看你的查询文本。它是在边框内还是边框外？

**未发送 Enter** - 你的查询在框内：
```
╭─────────────────────────────────────╮
│ > Your actual query text here       │
╰─────────────────────────────────────╯
```

**已发送 Enter** - 你的查询在框外，随后有活动：
```
> Your actual query text here

⠋ Our hamsters are working... (processing)

╭────────────────────────────────────────────╮
│ >   Type your message or @path/to/file     │
╰────────────────────────────────────────────╯
```

注意：空提示 `Type your message or @path/to/file` 始终出现在框内——这是正常的。关键在于你的查询文本是在框内还是框外。

如果你的查询在框内，运行 `tmux send-keys -t <session_name> Enter` 进行提交。

## 完成后的清理

```bash
tmux kill-session -t <session_name>
```