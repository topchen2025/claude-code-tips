# 写作风格

- 标题使用句式大小写，不要使用标题式大小写
- 永远不要使用长破折号（—）。请改用带空格的短横线 ` - `（空格、短横线、空格）。
- 不要编造或添加我没说过的内容。严格基于我说的内容。可以改写，但不要润色发挥。

# 关于我

- 名字：YK
- GitHub：ykdojo
- 当前年份：2026（将你的研究重点放在过去三个月）

# 行为

当我粘贴大段内容但没有给出指令时，直接总结它。

对于复杂的 bash 命令，把它拆分成多个简单命令，这样用户就不必逐个批准。或者，把它放进一个 bash 脚本文件中，并使用 `bash /tmp/<script>.sh` 运行。

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

还要避免复杂管道。不要这样：
```bash
grep "file: '" patch-cli.js | sed "s/.*file: '\([^']*\)'.*/\1/" | sort > /tmp/used.txt
```

要么逐步单独执行每一步，要么放进脚本文件并用 `bash /tmp/script.sh` 运行。

在其他目录执行 git 操作时，使用 `cd <path> && git ...`，不要用 `git -C <path>`。

在 bash 命令中永远不要使用 `2>&1`。保持 stderr 和 stdout 分离。

# 安全

**绝不要在主机上使用 `--dangerously-skip-permissions`。**

对于高风险操作，使用 Docker 容器。在容器内部，可以使用 YOLO 模式和 `--dangerously-skip-permissions`。

运行 `npx cc-safe <directory>` 以扫描 Claude Code 设置中的安全问题。

## 容器

使用 [safeclaw](https://github.com/ykdojo/safeclaw) 容器（本地路径：`/Users/yk/Desktop/projects/safeclaw`）。容器命名为 `safeclaw-<session-name>`（例如：`safeclaw-work`、`safeclaw-research`）。

列出正在运行的 safeclaw 容器：`docker ps --filter "name=safeclaw-"`

创建新容器：`cd /Users/yk/Desktop/projects/safeclaw && ./scripts/run.sh -s <name> -n`

对于公共仓库的只读 `gh` API 调用，使用正在运行的 safeclaw 容器：`docker exec safeclaw-<name> bash -c 'gh api <endpoint>'`。如果容器中的 `gh` 权限不足，则回退到主机上的 `gh`。

## URL 获取

对于 URL，通过 safeclaw 容器获取：
`docker exec safeclaw-<name> curl -sL <url>`

如果页面高度依赖 JavaScript（curl 返回最少内容或空内容），改用 Playwright（safeclaw 容器已安装 Playwright）。

## Tmux

对于交互式 Claude Code 会话：

```bash
tmux new-session -d -s <name> '<command>'
tmux send-keys -t <name> '<input>' Enter  # 别忘了 Enter！
tmux capture-pane -t <name> -p
```

注意：对于 Claude Code 会话，你可能需要在短暂延迟后再次发送 Enter，以确保提示被提交。

## 长时间运行的任务

如果你需要等待一个长时间运行的任务，使用 sleep 命令并手动指数退避：先等待 1 分钟，然后 2 分钟，再 4 分钟，依此类推。

# Claude for Chrome

- 使用 `read_page` 从无障碍树中获取元素引用
- 使用 `find` 按描述定位元素
- 使用 `ref` 点击/交互，不要使用坐标
- 除非用户明确要求，否则绝不要截图