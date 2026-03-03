# 写作风格

- 标题使用句首大写，不使用标题式大小写
- 切勿使用长破折号（—）。请改用带空格的连字符 ` - `（空格、连字符、空格）
- 不要编造或添加我未提及的内容。坚持我说的即可。可改写，但不要润饰。

# 关于我

- 名字：YK
- GitHub：ykdojo
- 当前年份：2026（请侧重最近三个月的研究）

# 行为

当我粘贴大量内容且未给出指令时，只需对其进行总结。

对于复杂的 bash 命令，将其拆分为多个简单命令，这样用户无需逐一批准。或者，将其写入 bash 脚本文件，并用 `bash /tmp/<script>.sh` 运行。

示例 - 不要这样：
```bash
sleep 60 && ps aux | grep foo | wc -l && echo "---" && ls -la /some/path
```

请这样做：
```bash
sleep 60
```
```bash
ps aux | grep foo | wc -l
```
```bash
ls -la /some/path
```

也要避免复杂的管道。不要这样：
```bash
grep "file: '" patch-cli.js | sed "s/.*file: '\([^']*\)'.*/\1/" | sort > /tmp/used.txt
```

要么单独运行每个步骤，要么将其放入脚本文件并使用 `bash /tmp/script.sh` 运行。

对于其他目录的 git 操作，使用 `cd <path> && git ...` 而不是 `git -C <path>`。

在 bash 命令中切勿使用 `2>&1`。保持 stderr 和 stdout 分离。

# 安全

**绝不要在主机上使用 `--dangerously-skip-permissions`。**

对于有风险的操作，请使用 Docker 容器。在容器内，YOLO 模式和 `--dangerously-skip-permissions` 可以使用。

运行 `npx cc-safe <directory>` 扫描 Claude Code 设置的安全问题。

## 容器

使用 [safeclaw](https://github.com/ykdojo/safeclaw) 容器（本地路径：`/Users/yk/Desktop/projects/safeclaw`）。容器命名为 `safeclaw-<session-name>`（例如，`safeclaw-work`、`safeclaw-research`）。

列出正在运行的 safeclaw 容器：`docker ps --filter "name=safeclaw-"`

创建新容器：`cd /Users/yk/Desktop/projects/safeclaw && ./scripts/run.sh -s <name> -n`

对公共仓库的只读 `gh` API 调用，请使用正在运行的 safeclaw 容器：`docker exec safeclaw-<name> bash -c 'gh api <endpoint>'`。如果容器内的 `gh` 权限不足，请退回使用主机 `gh`。

## URL 获取

对于 URL，通过 safeclaw 容器获取：
`docker exec safeclaw-<name> curl -sL <url>`

如果页面高度依赖 JavaScript（curl 返回内容很少或为空），请改用 Playwright（safeclaw 容器已安装 Playwright）。

## Tmux

对于交互式 Claude Code 会话：

```bash
tmux new-session -d -s <name> '<command>'
tmux send-keys -t <name> '<input>' Enter  # 别忘了回车！
tmux capture-pane -t <name> -p
```

注意：对于 Claude Code 会话，可能需要在短暂延迟后再次发送 Enter 以确保提交提示。

## 长时间运行的作业

如果需要等待长时间运行的作业，请使用手动指数退避的 sleep 命令：等待 1 分钟，然后 2 分钟，再 4 分钟，依此类推。

# Claude for Chrome

- 使用 `read_page` 从无障碍树获取元素引用
- 使用 `find` 按描述定位元素
- 使用 `ref` 而非坐标来点击/交互
- 切勿截屏，除非用户明确要求