# 45 个 Claude Code 技巧：从基础到进阶

以下是我整理的一些技巧，帮助你最大化发挥 Claude Code 的价值，包括自定义状态栏脚本、将 system prompt 缩减一半、把 Gemini CLI 当作 Claude Code 的“小弟”，以及让 Claude Code 在容器里运行自己。还包含了 [dx plugin](#tip-44-install-the-dx-plugin)。

📺 [快速演示](https://www.youtube.com/watch?v=hiISl558JGE) - 查看其中一些技巧在多 Claude 工作流与语音输入中的实际效果：

[![演示视频缩略图](assets/demo-thumbnail.png)](https://www.youtube.com/watch?v=hiISl558JGE)

<!-- TOC -->
## 目录

- [Tip 0：自定义你的状态栏](#tip-0-customize-your-status-line)
- [Tip 1：掌握几个关键斜杠命令](#tip-1-learn-a-few-essential-slash-commands)
- [Tip 2：用语音和 Claude Code 对话](#tip-2-talk-to-claude-code-with-your-voice)
- [Tip 3：把大问题拆成小问题](#tip-3-break-down-large-problems-into-smaller-ones)
- [Tip 4：像高手一样使用 Git 和 GitHub CLI](#tip-4-using-git-and-github-cli-like-a-pro)
- [Tip 5：AI 上下文就像牛奶；越新鲜、越浓缩越好！](#tip-5-ai-context-is-like-milk-its-best-served-fresh-and-condensed)
- [Tip 6：把输出从终端里取出来](#tip-6-getting-output-out-of-your-terminal)
- [Tip 7：设置终端别名以便快速访问](#tip-7-set-up-terminal-aliases-for-quick-access)
- [Tip 8：主动压缩你的上下文](#tip-8-proactively-compact-your-context)
- [Tip 9：为自治任务完成写-测循环](#tip-9-complete-the-write-test-cycle-for-autonomous-tasks)
- [Tip 10：Cmd+A 和 Ctrl+A 是你的好朋友](#tip-10-cmda-and-ctrla-are-your-friends)
- [Tip 11：将 Gemini CLI 作为受限网站的后备方案](#tip-11-use-gemini-cli-as-a-fallback-for-blocked-sites)
- [Tip 12：投资你的个人工作流](#tip-12-invest-in-your-own-workflow)
- [Tip 13：搜索你的对话历史](#tip-13-search-through-your-conversation-history)
- [Tip 14：使用终端标签页多任务处理](#tip-14-multitasking-with-terminal-tabs)
- [Tip 15：瘦身 system prompt](#tip-15-slim-down-the-system-prompt)
- [Tip 16：用 Git worktrees 并行处理分支工作](#tip-16-git-worktrees-for-parallel-branch-work)
- [Tip 17：为长任务手动做指数退避](#tip-17-manual-exponential-backoff-for-long-running-jobs)
- [Tip 18：把 Claude Code 当作写作助手](#tip-18-claude-code-as-a-writing-assistant)
- [Tip 19：Markdown 太强了](#tip-19-markdown-is-the-st)
- [Tip 20：粘贴时用 Notion 保留链接](#tip-20-use-notion-to-preserve-links-when-pasting)
- [Tip 21：用容器处理高风险的长任务](#tip-21-containers-for-long-running-risky-tasks)
- [Tip 22：提升 Claude Code 使用能力的最佳方式就是用它](#tip-22-the-best-way-to-get-better-at-using-claude-code-is-by-using-it)
- [Tip 23：克隆/分叉与半克隆会话](#tip-23-clonefork-and-half-clone-conversations)
- [Tip 24：用 realpath 获取绝对路径](#tip-24-use-realpath-to-get-absolute-paths)
- [Tip 25：理解 CLAUDE.md、Skills、Slash Commands 与 Plugins 的区别](#tip-25-understanding-claudemd-vs-skills-vs-slash-commands-vs-plugins)
- [Tip 26：交互式 PR 审查](#tip-26-interactive-pr-reviews)
- [Tip 27：把 Claude Code 当作研究工具](#tip-27-claude-code-as-a-research-tool)
- [Tip 28：掌握多种验证输出的方法](#tip-28-mastering-different-ways-of-verifying-its-output)
- [Tip 29：Claude Code 作为 DevOps 工程师](#tip-29-claude-code-as-a-devops-engineer)
- [Tip 30：保持 CLAUDE.md 简洁并定期复查](#tip-30-keep-claudemd-simple-and-review-it-periodically)
- [Tip 31：Claude Code 作为通用接口](#tip-31-claude-code-as-the-universal-interface)
- [Tip 32：关键在于选择合适的抽象层级](#tip-32-its-all-about-choosing-the-right-level-of-abstraction)
- [Tip 33：审计你已批准的命令](#tip-33-audit-your-approved-commands)
- [Tip 34：写大量测试（并使用 TDD）](#tip-34-write-lots-of-tests-and-use-tdd)
- [Tip 35：在未知中更勇敢；迭代式问题求解](#tip-35-be-braver-in-the-unknown-iterative-problem-solving)
- [Tip 36：在后台运行 bash 命令和子代理](#tip-36-running-bash-commands-and-subagents-in-the-background)
- [Tip 37：个性化软件时代已经到来](#tip-37-the-era-of-personalized-software-is-here)
- [Tip 38：导航并编辑你的输入框](#tip-38-navigating-and-editing-your-input-box)
- [Tip 39：花点时间规划，但也要快速做原型](#tip-39-spend-some-time-planning-but-also-prototype-quickly)
- [Tip 40：简化过度复杂的代码](#tip-40-simplify-overcomplicated-code)
- [Tip 41：自动化的自动化](#tip-41-automation-of-automation)
- [Tip 42：分享你的知识，并在力所能及处做贡献](#tip-42-share-your-knowledge-and-contribute-where-you-can)
- [Tip 43：持续学习！](#tip-43-keep-learning)
- [Tip 44：安装 dx plugin](#tip-44-install-the-dx-plugin)
- [Tip 45：快速安装脚本](#tip-45-quick-setup-script)

<!-- /TOC -->

## Tip 0：自定义你的状态栏

你可以自定义 Claude Code 底部的状态栏来显示有用信息。我把它设置为显示模型、当前目录、git 分支（如果有）、未提交文件数量、与 origin 的同步状态，以及 token 使用量的可视化进度条。它还会显示第二行我的上一条消息，这样我能快速回忆对话在讲什么：

```
Opus 4.5 | 📁claude-code-tips | 🔀main (scripts/context-bar.sh uncommitted, synced 12m ago) | ██░░░░░░░░ 18% of 200k tokens
💬 This is good. I don't think we need to change the documentation as long as we don't say that the default color is orange el...
```

这对监控上下文使用和记住当前任务特别有帮助。该脚本还支持 10 种配色主题（orange、blue、teal、green、lavender、rose、gold、slate、cyan、gray）。

![颜色预览选项](scripts/color-preview.png)

要进行设置，你可以使用[这个示例脚本](scripts/context-bar.sh)，并查看[安装说明](scripts/README.md)。

## Tip 1：掌握几个关键斜杠命令

内置了很多斜杠命令（输入 `/` 可查看全部）。以下是几个值得掌握的：

### /usage

检查你的速率限制：

```
 Current session
 █████████▌                                         19% used
 Resets 12:59am (America/Vancouver)

 Current week (all models)
 █████████████████████▌                             43% used
 Resets Feb 3 at 1:59pm (America/Vancouver)

 Current week (Sonnet only)
 ███████████████████▌                               39% used
 Resets 8:59am (America/Vancouver)
```

如果你想密切监控用量，可以把它固定在一个标签页里，然后用 Tab + Shift+Tab 或 ← + → 刷新。

### /chrome

切换 Claude 的原生浏览器集成：

```
> /chrome
Chrome integration enabled
```

### /mcp

管理 MCP（Model Context Protocol）服务器：

```
 Manage MCP servers
 1 server

 ❯ 1. playwright  ✔ connected · Enter to view details

 MCP Config locations (by scope):
  • User config (available in all your projects):
    • /Users/yk/.claude.json
```

### /stats

以 GitHub 风格活动图查看使用统计：

```
      Feb Mar Apr May Jun Jul Aug Sep Oct Nov Dec Jan
      ··········································▒█░▓░█░▓▒▒
  Mon ·········································▒▒██▓░█▓█░█
      ·········································░▒█▒▓░█▒█▒█
  Wed ········································░▓▒█▓▓░▒▓▒██
      ········································░▓░█▓▓▓▓█░▒█
  Fri ········································▒░░▓▒▒█▓▓▓█
      ········································▒▒░▓░░▓▒▒░░

      Less ░ ▒ ▓ █ More

  Favorite model: Opus 4.5        Total tokens: 17.6m

  Sessions: 4.1k                  Longest session: 20h 40m 45s
  Active days: 79/80              Longest streak: 75 days
  Most active day: Jan 26         Current streak: 74 days

  You've used ~24x more tokens than War and Peace
```

### /clear

清空当前对话并重新开始。

## Tip 2：用语音和 Claude Code 对话

我发现用语音沟通比手动打字快得多。在本地机器上使用语音转写系统对此非常有帮助。

在我的 Mac 上，我试过几个选项：
- [superwhisper](https://superwhisper.com/)
- [MacWhisper](https://goodsnooze.gumroad.com/l/macwhisper)
- [Super Voice Assistant](https://github.com/ykdojo/super-voice-assistant)（开源，支持 Parakeet v2/v3）

使用托管服务能获得更高准确率，但我发现本地模型对这个用途已经足够。即使转写里有错误或错别字，Claude 也足够聪明，能理解你的真实意图。有时你需要把某些词说得更清楚，但总体来说本地模型完全够用。

例如，在这张截图里你可以看到，Claude 能把误转写词 “ExcelElanishMark” 和 “advast” 正确理解为 “exclamation mark” 和 “Advanced”：

![语音转写错误被正确理解](assets/voice-transcription-mistakes.png)

我觉得最好的理解方式是：这就像你在和朋友沟通。当然你可以发文字消息，这对一些人更容易，或者发邮件，也都可以。这也是大多数人使用 Claude Code 的方式。但如果你想沟通更快，为什么不直接来个快速语音通话呢？你不需要真的和 Claude Code 打电话，只要持续发送语音消息就行。至少对我来说，这明显更快——过去几年我练习了很多口语表达。我觉得对大多数人来说也会更快。

一个常见疑问是：“如果你在有其他人的房间里怎么办？”我会戴耳机轻声说话——我个人喜欢 Apple EarPods（不是 AirPods）。它价格可接受，质量够好，你只需要轻声对着它说。我在别人面前这么做过，效果很好。在办公室里大家本来就会说话——只是你不是和同事说话，而是在轻声对语音转写系统说话。我觉得这没问题。这个方法好到甚至在飞机上也能用。环境噪音大，别人听不到你；但只要你离麦克风够近，本地模型仍然可以识别你说的话。（事实上，我现在就在航班上用这个方法写这段。）

## Tip 3：把大问题拆成小问题

这是最重要、最该掌握的概念之一。它和传统软件工程完全一致——优秀的软件工程师本来就会这么做，这同样适用于 Claude Code。

如果你发现 Claude Code 不能一发解决某个困难问题或编码任务，就让它把问题拆成多个更小的问题。先看它能否解决其中一个部分。如果还是太难，就再拆得更细。持续拆分，直到每个部分都可解。

本质上，不是从 A 直接到 B：

![直接路径](assets/breakdown-direct.png)

而是从 A 到 A1 到 A2 到 A3，再到 B：

![逐步路径](assets/breakdown-steps.png)

我在构建自己的语音转写系统时就是这样做的。我需要让系统支持：用户选择并下载模型、接收快捷键、开始转写、把转写文本插入用户光标位置，并配上漂亮 UI。这太多了，所以我把它拆成小任务。先做一个只负责下载模型的可执行文件；再做一个只负责录音；再做一个只负责转写预录音频。逐个完成后，最后再整合。

高度相关的一点是：在 agentic coding 和 Claude Code 的世界里，你的问题解决能力和软件工程能力仍然非常重要。它确实能独立解决很多问题，但当你叠加自己的通用问题解决与工程能力时，它会强大得多。

## Tip 4：像高手一样使用 Git 和 GitHub CLI

直接让 Claude 处理你的 Git 和 GitHub CLI 任务。这包括 commit（你不用手写提交信息）、branch、pull 和 push。

我个人会自动允许 pull，但不自动允许 push，因为 push 风险更高——pull 出错不会污染 origin。

对于 GitHub CLI（`gh`），可做的事很多。我在使用 Claude Code 后更常做的一件事是创建 draft PR。这样可以低风险地让 Claude Code 处理 PR 创建流程——在标记 ready for review 前你可以先审阅全部内容。

而且 `gh` 非常强大。你甚至可以通过它发送任意 GraphQL 查询。例如，你可以找出 GitHub PR 描述被编辑的精确时间：

```
⏺ Bash(gh api graphql -f query='
      query {
        repository(owner: "...", name: "...") {
          pullRequest(number: ...) {
            userContentEdits(first: 100) {
              nodes { editedAt editor { login } }
            }
          }
        }
      }')

⏺ Here's the full edit history for your PR description:

  | #  | Edited At (UTC)     | Editor |
  |----|---------------------|--------|
  | 1  | 2025-12-01 00:08:34 | ykdojo |
  | 2  | 2025-12-01 15:57:21 | ykdojo |
  | 3  | 2025-12-01 16:24:33 | ykdojo |
  | 4  | 2025-12-01 16:27:00 | ykdojo |
  | 5  | 2025-12-04 00:40:02 | ykdojo |
  ...
```

### 关闭 commit/PR 署名

默认情况下，Claude Code 会在 commit 中添加 `Co-Authored-By` 尾注，并在 PR 中添加归属页脚。你可以在 `~/.claude/settings.json` 中这样禁用：

```json
{
  "attribution": {
    "commit": "",
    "pr": ""
  }
}
```

将两者设置为空字符串会完全移除署名。这会替代旧的 `includeCoAuthoredBy` 设置，后者已弃用。

## Tip 5：AI 上下文就像牛奶；越新鲜、越浓缩越好！

当你在 Claude Code 中开启一段新对话时，它通常表现最好，因为不需要处理此前对话里累积的复杂上下文。但随着对话越来越长，上下文变长，性能往往会下降。

所以，每个新话题最好新开对话；或者一旦性能开始下降，也应考虑新开。

## Tip 6：把输出从终端里取出来

有时你想复制粘贴 Claude Code 的输出，但直接从终端复制不一定干净。下面是几种更容易导出内容的方法：

- **`/copy` 命令**：最简单——输入 `/copy`，将 Claude 的最后一条回复以 markdown 形式复制到剪贴板
- **直接写入剪贴板**：在 Mac 或 Linux 上，让 Claude 使用 `pbcopy` 把输出直接发送到剪贴板
- **写入文件**：让 Claude 把内容写到文件里，再让它用 VS Code（或你喜欢的编辑器）打开，你可从那里复制。你也可以指定行号，让 Claude 打开它刚编辑的具体行。对于 markdown 文件，在 VS Code 打开后可用 Cmd+Shift+P（Linux/Windows 为 Ctrl+Shift+P）选择 “Markdown: Open Preview” 查看渲染版本
- **打开 URL**：如果有 URL 你想亲自查看，让 Claude 在浏览器中打开。Mac 上可以让它用 `open` 命令；一般来说让它在你偏好的浏览器打开在任何平台都可行
- **GitHub Desktop**：你可以让 Claude 在 GitHub Desktop 中打开当前仓库。尤其当它在非根目录工作时这很有用——例如你让它在其他目录创建了 git worktree，而你还没从那个目录启动 Claude Code

这些方法也可以组合使用。比如你想编辑 GitHub PR 描述，与其让 Claude 直接改（可能会改坏），不如先让它把内容复制到本地文件，再让它编辑，你自己检查结果，确认没问题后再让它复制回 GitHub PR。这非常好用。或者你也可以自己操作：让它在 VS Code 打开，或通过 `pbcopy` 给你手动粘贴。

当然你可以自己跑这些命令，但如果你重复做很多次，让 Claude 代劳会更高效。

## Tip 7：设置终端别名以便快速访问

因为用了 Claude Code 后我更常用终端，所以我设置了短别名来快速启动工具。以下是我在用的：

- `c` 对应 Claude Code（我最常用）
- `ch` 对应开启 Chrome 集成的 Claude Code
- `gb` 对应 GitHub Desktop
- `co` 对应 VS Code
- `q` 跳转到我存放大多数项目的目录。到那里后我可以手动 cd 到某个项目，或者直接用 `c` 启动 Claude Code，让它访问任意需要的项目。

要设置这些，在 shell 配置文件（`~/.zshrc` 或 `~/.bashrc`）中添加：

```bash
alias c='claude'
alias ch='claude --chrome'
alias gb='github'
alias co='code'
alias q='cd ~/Desktop/projects'
```

有了这些别名后你还能配合参数使用：`c -c` 继续上一段对话，`c -r` 显示最近对话列表以恢复。`ch` 也一样（`ch -c`、`ch -r`）可用于 Chrome 会话。

## Tip 8：主动压缩你的上下文

Claude Code 里有个 `/compact` 命令，会总结对话以释放上下文空间。可用上下文被占满时也会自动 compact。Opus 4.5 目前总上下文窗口是 200k，其中 45k 预留给自动 compact。总 200k 里大约 10% 会被 system prompt、tools、memory 和动态上下文自动占用。但我发现主动手动调优更好。我通过 `/config` 关闭了 auto-compact，这样主对话能用的上下文更多，也更可控。

我的做法是在“清空重开”前让 Claude 写一个交接文档。比如：

> Put the rest of the plan in the system-prompt-extraction folder as HANDOFF.md. Explain what you have tried, what worked, what didn't work, so that the next agent with fresh context is able to just load that file and nothing else to get started on this task and finish it up.

Claude 会生成一个总结当前工作状态的文件：

```
⏺ Write(experiments/system-prompt-extraction/HANDOFF.md)
  ⎿  Wrote 129 lines to experiments/system-prompt-extraction/HANDOFF.md
     # System Prompt Slimming - Handoff Document
     ## Goal
     Reduce Claude Code's system prompt by ~45% (currently at 11%, need ~34% more).
     ## Current Progress
     ### What's Been Done
     - **Backup/restore system**: `backup-cli.sh` and `restore-cli.sh` with SHA256 verification
     - **Patch system**: `patch-cli.js` that restores from backup then applies patches
     ...
```

Claude 写完后，快速检查一下。如果有缺漏，要求它修改：

> Did you add a note about iteratively testing instead of trying to do everything all at once?

然后开启新对话。对于新 agent，你只要给它文件路径，不需要别的信息，通常就能顺利继续：

```
> experiments/system-prompt-extraction/HANDOFF.md
```

在后续对话里，你可以让 agent 为下一个 agent 持续更新这个文档。

我还创建了一个 `/handoff` 斜杠命令来自动化此流程——它会检查是否已有 HANDOFF.md，若存在则读取，然后创建或更新目标、进展、有效方法、无效方法和下一步。你可以在 [skills folder](skills/handoff/SKILL.md) 中找到它，或通过 [dx plugin](#tip-44-install-the-dx-plugin) 安装。

**替代方案：使用 plan mode**

另一种方式是使用 plan mode。通过 `/plan` 或 Shift+Tab 进入。让 Claude 收集所有相关上下文并为下一个 agent 制定完整计划：

> I just enabled plan mode. Bring over all of the context that you need for the next agent. The next agent will not have any other context, so you'll need to be pretty comprehensive.

Claude 会探索代码库、收集上下文并写出详细计划。完成后你会看到类似选项：

```
Would you like to proceed?

❯ 1. Yes, clear context and auto-accept edits (shift+tab)
  2. Yes, auto-accept edits
  3. Yes, manually approve edits
  4. Type here to tell Claude what to change
```

选项 1 会清除之前上下文，并携带计划重新开始。新的 Claude 实例只看到计划，因此不受旧对话包袱影响，也会获得旧 transcript 文件链接，以便必要时查具体细节。

## Tip 9：为自治任务完成写-测循环

如果你想让 Claude Code 自治运行某件事（例如 `git bisect`），就必须给它一个验证结果的方法。关键是完成写-测循环：写代码、运行、检查输出、重复。

例如，你在开发 Claude Code 本身时发现 `/compact` 突然失效并报 400 错误。定位罪魁提交的经典工具是 `git bisect`。好处是你可以让 Claude Code 对自己执行 bisect，但它需要一种方式来测试每个 commit。

对于涉及交互终端（如 Claude Code）的任务，可以用 tmux。模式如下：

1. 启动 tmux 会话
2. 向会话发送命令
3. 捕获输出
4. 验证是否符合预期

下面是一个测试 `/context` 是否可用的简单示例：

```bash
tmux kill-session -t test-session 2>/dev/null
tmux new-session -d -s test-session
tmux send-keys -t test-session 'claude' Enter
sleep 2
tmux send-keys -t test-session '/context' Enter
sleep 1
tmux capture-pane -t test-session -p
```

有了类似测试后，Claude Code 就能运行 `git bisect` 并自动测试每个 commit，直到找出引入问题的那个。

这也说明为什么软件工程技能依然重要。如果你是工程师，可能本来就知道 `git bisect` 这样的工具。这些知识在 AI 时代仍很有价值，只是换了新的应用方式。

另一个例子就是写测试。让 Claude Code 写完代码后，你可以让它顺便为自己的代码写测试。让它自行运行并在可能情况下自我修复。当然它并不总是走对方向，有时需要你监督，但它能独立完成的编码任务比想象中多。

### 创造性测试策略

有时你需要在完成写-测循环时更有创造性。比如做 web app 时，你可以用 Playwright MCP、Chrome DevTools MCP，或 Claude 原生浏览器集成（通过 `/chrome`）。我还没试过 Chrome DevTools，但试过 Playwright 和 Claude 原生集成。总体上 Playwright 往往更好。它确实会消耗不少上下文，但 200k 上下文窗口通常足够单个任务或几个小任务。

两者主要区别似乎是：Playwright 更关注无障碍树（页面元素的结构化数据），而不是截图。它也能截图，但通常不依赖截图执行动作。相对地，Claude 原生浏览器集成更依赖截图并按坐标点击元素。有时会点到随机位置，整体也可能较慢。

未来可能会改进，但默认我会在大多数非视觉密集任务中优先选 Playwright。只有在需要复用登录态且不想提供凭据（因为它运行在你自己的浏览器配置里），或确实需要按坐标进行可视点击时，我才用 Claude 原生集成。

这就是我默认关闭 Claude 原生浏览器集成，并通过之前定义的 `ch` 快捷方式按需启用的原因。这样多数浏览器任务由 Playwright 处理，只有确实需要时才启用原生集成。

此外，你可以要求它用 accessibility tree refs 而不是坐标。以下是我在 CLAUDE.md 中的写法：

```markdown
# Claude for Chrome

- Use `read_page` to get element refs from the accessibility tree
- Use `find` to locate elements by description
- Click/interact using `ref`, not coordinates
- NEVER take screenshots unless explicitly requested by the user
```

结合个人经历，我还遇到过这样一个场景：我在开发一个 Python 库（[Daft](https://github.com/Eventual-Inc/Daft)），需要在 Google Colab 上测试本地构建版本。问题是：带 Rust 后端的 Python 库在 Colab 上构建很难顺利进行。因此我需要先在本地构建 wheel，再手动上传到 Colab 运行。我也试过 monkey patching，在等待 wheel 本地完整构建前，这在短期内效果不错。我就是和 Claude Code 来回协作提出并执行这些测试策略的。

另一个场景是我需要在 Windows 上测试，但我并没有 Windows 机器。同仓库 CI 在 Windows 上失败，原因涉及 Rust，而我无法本地复现。于是我需要创建一个包含全部变更的 draft PR，再创建一个“同样变更 + 在非 main 分支启用 Windows CI”的 draft PR。我把这些都交给 Claude Code 执行，然后直接在新分支上测试 CI。

## Tip 10：Cmd+A 和 Ctrl+A 是你的好朋友

这几年我一直在说：在 AI 世界里，Cmd+A 和 Ctrl+A 是好朋友。这同样适用于 Claude Code。

有时你想给 Claude Code 一个 URL，但它无法直接访问。可能是私有页面（并非敏感数据，只是公开不可达），也可能像 Reddit 这种 Claude Code 抓取困难的网站。这时你可以直接全选页面内容（Mac 用 Cmd+A，其他平台用 Ctrl+A），复制后粘贴进 Claude Code。这个方法非常强大。

对终端输出也同样适用。当我有来自 Claude Code 自身或其他 CLI 应用的输出时，也能用同样技巧：全选、复制、粘贴回 CC，很实用。

有些页面默认不适合全选，但可以先做一些处理。例如 Gmail 线程，先点 Print All 打开打印预览（再取消实际打印）。该页面会展开线程中所有邮件，你就可以用 Cmd+A 干净地复制整个对话。

这适用于任何 AI，不仅是 Claude Code。

## Tip 11：将 Gemini CLI 作为受限网站的后备方案

Claude Code 的 WebFetch 工具无法访问某些站点，比如 Reddit。但你可以通过创建一个 skill 让 Claude 在受限时回退到 Gemini CLI。Gemini 有网页访问能力，能抓取 Claude 直接拿不到的内容。

这沿用了 Tip 9 的 tmux 模式——启动会话、发送命令、捕获输出。skill 文件放在 `~/.claude/skills/reddit-fetch/SKILL.md`。完整内容见 [skills/reddit-fetch/SKILL.md](skills/reddit-fetch/SKILL.md)。

Skill 的 token 效率更高，因为 Claude Code 只在需要时加载它们。若你想更简单，也可把精简版放在 `~/.claude/CLAUDE.md`，但这样每次对话都会加载，无论你是否需要。

我测试方式是让 Claude Code 查看 Reddit 上大家如何评价 Claude Code skills——有点“元”。它会和 Gemini 来回交互一阵，所以不快，但报告质量出乎意料地好。当然你需要先安装 Gemini CLI 才能使用。你也可以通过 [dx plugin](#tip-44-install-the-dx-plugin) 安装这个 skill。

## Tip 12：投资你的个人工作流

我自己从零开始用 Swift 写了语音转写应用；用 Claude Code + bash 从零做了自定义状态栏；还搭了一个系统，用来精简 Claude Code 压缩 JavaScript 文件中的 system prompt。

但你不必做到这么“重度”。只要把自己的 CLAUDE.md 管理好，尽可能简洁且有助于实现目标——这类工作就很有价值。以及，学习这些技巧、这些工具和关键功能。

这些都是对你用来构建任何东西的工具链的投资。我认为至少应该花一些时间做这件事。

## Tip 13：搜索你的对话历史

你可以直接问 Claude Code 过去聊过什么，它会帮你查找搜索。你的对话历史保存在本地 `~/.claude/projects/`，目录名基于项目路径（斜杠会变成连字符）。

例如，项目路径 `/Users/yk/Desktop/projects/claude-code-tips` 的对话会存储在：

```
~/.claude/projects/-Users-yk-Desktop-projects-claude-code-tips/
```

每段对话都是 `.jsonl` 文件。你可以用基础 bash 命令检索：

```bash
# 查找所有提到 "Reddit" 的对话
grep -l -i "reddit" ~/.claude/projects/-Users-yk-Desktop-projects-*/*.jsonl

# 查找今天关于某个主题的对话
find ~/.claude/projects/-Users-yk-Desktop-projects-*/*.jsonl -mtime 0 -exec grep -l -i "keyword" {} \;

# 仅提取会话中的用户消息（需要 jq）
cat ~/.claude/projects/.../conversation-id.jsonl | jq -r 'select(.type=="user") | .message.content'
```

或者直接问 Claude Code：“今天我们关于 X 聊了什么？”它会帮你搜索历史。

## Tip 14：使用终端标签页多任务处理

当你运行多个 Claude Code 实例时，保持有序比任何具体技术设置（如 Git worktrees）都更重要。我建议同时处理最多三到四个任务。

我个人的方法可称为“级联”：每当开始新任务，就在右侧新开一个标签页。然后左右来回扫，从最老任务到最新任务。整体方向保持一致，除非需要检查特定任务、查看通知等。

我的典型布局大概如下：

![终端标签页多任务工作流](assets/multitasking-terminal-tabs.png)

这个例子中：
1. **最左标签页** - 常驻运行我的语音转写系统（始终放这里）
2. **第二个标签页** - 设置 Docker 容器
3. **第三个标签页** - 检查本机磁盘使用情况
4. **第四个标签页** - 处理某个工程项目
5. **第五个标签页（当前）** - 正在写这一条技巧

## Tip 15：瘦身 system prompt

Claude Code 的 system prompt 和工具定义在你开始工作前就会占用约 19k tokens（约 200k 上下文的 10%）。我做了一个 patch 系统，把它降到约 9k tokens——节省约 10,000 tokens（约 50% 的额外开销）。

| Component | Before | After | Savings |
|-----------|--------|-------|---------|
| System prompt | 3.0k | 1.8k | 1,200 tokens |
| System tools | 15.6k | 7.4k | 8,200 tokens |
| **Total** | **~19k** | **~9k** | **~10k tokens (~50%)** |

下面是 patch 前后 `/context` 的样子：

**Unpatched (~20k, 10%)**

![未打补丁的上下文](assets/context-unpatched.png)

**Patched (~10k, 5%)**

![打补丁后的上下文](assets/context-patched.png)

这些补丁通过从压缩 CLI bundle 中删去冗长示例和冗余文本来实现，同时保留核心指令。

我做了大量测试，效果很好。它感觉更“原始”——更强，但可能约束稍少，这也合理，因为系统指令更短了。这样用起来更像专业工具。我很喜欢从更低上下文起步，因为在填满之前余量更大，你就能把对话延续更久。这绝对是这套策略最好的部分。

查看 [system-prompt folder](system-prompt/) 获取补丁脚本和裁剪细节。

**为什么要 patch？** Claude Code 有参数可从文件提供简化 system prompt（`--system-prompt` 或 `--system-prompt-file`），这也是一种方式。但对于工具描述，没有官方自定义选项。只能 patch CLI bundle。由于我的 patch 系统统一处理了这些部分，目前先保持这种做法。未来我可能会用 flag 重做 system prompt 那部分。

**支持安装方式：** npm 与 native binary（macOS、Linux）。

**重要**：如果你要保留 patch 后的 system prompt，请在 `~/.claude/settings.json` 添加以下内容关闭自动更新：

```json
{
  "env": {
    "DISABLE_AUTOUPDATER": "1"
  }
}
```

这适用于所有 Claude Code 会话，不受 shell 类型影响（交互式、非交互式、tmux）。你可以在准备好后手动更新，并重新对新版本应用补丁。

### 延迟加载 MCP 工具

如果你使用 MCP 服务器，它们的工具定义默认会加载到每次对话中——即使你并不使用。这会增加显著开销，尤其在配置了多个服务器时。

启用延迟加载，使 MCP 工具仅在需要时加载：

```json
{
  "env": {
    "ENABLE_TOOL_SEARCH": "true"
  }
}
```

将其添加到 `~/.claude/settings.json`。Claude 将按需搜索并加载 MCP 工具，而不是从一开始全部加载。自 2.1.7 版本起，当 MCP 工具描述超过上下文窗口的 10% 时会自动这样处理。

## Tip 16：用 Git worktrees 并行处理分支工作

如果你在同一个项目中同时做多个事情，又不希望相互冲突，Git worktrees 是非常好的方式。你可以直接让 Claude Code 创建 git worktree 并在那边开工——不必自己记具体语法。

基本思路是：在不同目录中处理不同分支。本质上就是“分支 + 目录”。

你可以把 Git worktrees 这一层叠加在我在多任务技巧中提到的级联方法之上。

### 什么是 git worktrees？

git worktree 和普通 git branch 一样，只是它会绑定一个专属新目录。

比如你同时处理 main 分支和 feature-branch-1。没有 git worktrees 时，你一次只能处理一个，因为项目目录同一时刻只能切到一个分支。

而有了 git worktree，你可以在原始项目目录继续 main（或任何其他分支），同时在新目录处理 feature-branch-1。

![Git worktrees 并行分支示意图](assets/git-worktrees.png)

## Tip 17：为长任务手动做指数退避

等待长任务（如 Docker 构建或 GitHub CI）时，你可以让 Claude Code 执行手动指数退避。指数退避是软件工程中的常见技巧，在这里也适用。让 Claude 以逐步增加的 sleep 间隔检查状态——1 分钟、2 分钟、4 分钟，依次递增。虽然不是传统意义上的程序自动重试，而是 AI 手动执行，但效果很好。

这样 agent 可以持续检查状态并在完成后通知你。

（针对 GitHub CI，虽然有 `gh run watch`，但会持续输出大量行，浪费 tokens。使用手动指数退避配合 `gh run view <run-id> | grep <job-name>` 实际更省 tokens。这也是通用技巧，即使没有专门的 wait 命令也很好用。）

例如，如果你有一个 Docker build 在后台运行：

![手动指数退避检查 Docker 构建进度](assets/manual-exponential-backoff.png)

它会持续检查直到任务完成。

## Tip 18：把 Claude Code 当作写作助手

Claude Code 是非常优秀的写作助手和合作伙伴。我的写作方式是：先给它完整背景，再用语音给出详细指令。这样能得到初稿。如果不够好，就多试几轮。

然后我会逐行过稿。比如“我们一起看这句。我喜欢这句因为……这句应移到那边。这句要按这个方式改。”我也会让它参考资料。

这是一种来回协作的过程，可以是终端在左、编辑器在右：

![Claude Code 并排写作工作流](assets/writing-assistant-side-by-side.png)

这通常效果很好。

## Tip 19：Markdown 太强了

一般大家写新文档会用 Google Docs 或 Notion 之类。但现在我真心觉得最高效的方式是 markdown。

在 AI 时代之前 markdown 就已经很好了，而配合 Claude Code（尤其在我前面提到的写作高效率场景）之后，我认为 markdown 的价值更高。无论你想写博客还是 LinkedIn 帖子，都可以直接和 Claude Code 对话，保存为 markdown，再往下处理。

这个技巧的小提示：如果你想把 markdown 粘贴到不太支持 markdown 的平台，可以先粘贴到一个全新的 Notion 页面，再从 Notion 复制到目标平台。Notion 会把它转成更多平台可接受的格式。如果普通粘贴不行，试试 Command + Shift + V（无格式粘贴）。

## Tip 20：粘贴时用 Notion 保留链接

反过来也成立。如果你从其他地方（比如 Slack）复制了带链接的文本，直接粘贴进 Claude Code 可能看不到链接。但如果先放到 Notion 文档，再从 Notion 复制，就能得到 markdown 格式，Claude Code 当然可以识别。

## Tip 21：用容器处理高风险的长任务

普通会话更适合有节奏的工作：你控制权限授予并仔细审查输出。容器化环境则非常适合 `--dangerously-skip-permissions` 会话，不需要每个小操作都授权，可以让它自己跑一段时间。

这对研究或实验很有用，尤其是耗时且可能有风险的任务。一个好例子是 Tip 11 的 Reddit 研究流程：reddit-fetch skill 通过 tmux 与 Gemini CLI 来回交互。让它在主系统无人值守运行有风险，但在容器里即使出问题，影响也被隔离。

另一个例子是我在这个仓库里创建 [system prompt patching scripts](system-prompt/) 的过程。每次 Claude Code 出新版本，我都要更新压缩 CLI bundle 的补丁。与其在宿主机上用 `--dangerously-skip-permissions` 跑 Claude Code（它能访问所有内容），我会在容器里跑。这样 Claude Code 可以探索压缩 JavaScript、找变量映射、生成新补丁文件，而不必每一步都让我审批。

事实上，它几乎是完全自主完成这次迁移的。它会尝试应用补丁，发现某些不兼容新版本，就迭代修复，甚至基于学习结果改进了面向未来实例的 [instruction document](system-prompt/UPGRADING.md)。

我还做了 [SafeClaw](https://github.com/ykdojo/safeclaw)，让运行容器化 Claude Code 会话变得简单。它支持启动多个隔离会话（每个都有 web 终端），并在一个仪表盘里统一管理。它用了本仓库的多项定制，包括优化后的 system prompt、[DX plugin](#tip-44-install-the-dx-plugin) 和 [status line](#tip-0-customize-your-status-line)。

### 进阶：在容器中编排一个 worker Claude Code

你还可以更进一步：让本地 Claude Code 控制另一个运行在容器里的 Claude Code 实例。关键是把 tmux 作为控制层：

1. 本地 Claude Code 启动一个 tmux 会话
2. 在该 tmux 会话中运行或连接容器
3. 容器内的 Claude Code 使用 `--dangerously-skip-permissions` 运行
4. 外层 Claude Code 用 `tmux send-keys` 发送提示词，用 `capture-pane` 读取输出

这样你会得到一个完全自治的“worker” Claude Code，可执行实验性或长耗时任务，无需你逐步审批。完成后，本地 Claude Code 可把结果拉回。若出问题，也都被沙箱化在容器中。

### 进阶：多模型编排

不止 Claude Code，你还可以在容器中运行不同 AI CLI：Codex、Gemini CLI 等。我试过用 OpenAI Codex 做代码审查，效果不错。重点不是你不能直接在宿主机上跑这些 CLI——当然可以。价值在于 Claude Code 的 UI/UX 足够顺滑，你只需和它对话，让它负责编排：拉起不同模型、在容器与宿主机间传数据。你不必手动切终端、复制粘贴；Claude Code 成为统一调度界面。

## Tip 22：提升 Claude Code 使用能力的最佳方式就是用它

最近我看到一位世界级攀岩运动员接受另一位攀岩运动员采访。她被问到：“如何提升攀岩水平？”她回答很简单：“去攀岩。”

我对这件事也是同样感受。当然你可以做一些辅助性学习：看视频、看书、学技巧。但最好的学习方式就是使用 Claude Code。本质上，学习用 AI 的最好方式就是用 AI。

我喜欢把它称作“十亿 token 法则”，而不是“1 万小时法则”。如果你想真正提升 AI 能力，并建立对其工作方式的直觉，最好的方式是消费大量 tokens。现在这已经可行。我发现尤其是 Opus 4.5，能力够强而且成本够低，你可以同时开多个会话，不必过度担心 token 消耗，这会让你轻松很多。

## Tip 23：克隆/分叉与半克隆会话

有时你想从会话中的某个节点尝试不同路径，又不想丢失原始线程。[clone-conversation script](scripts/clone-conversation.sh) 可以用新 UUID 复制会话，让你分叉。

**内置替代方案（新版本）：** Claude Code 现在支持原生分叉：
- `/fork` - 在当前会话内分叉
- `--fork-session` - 配合 `--resume` 或 `--continue` 使用（如 `claude -c --fork-session`）

由于 `--fork-session` 没有短参数，你可以在 `~/.zshrc` 或 `~/.bashrc` 加这个函数，用 `--fs` 作为快捷方式：

```bash
claude() {
  local args=()
  for arg in "$@"; do
    if [[ "$arg" == "--fs" ]]; then
      args+=("--fork-session")
    else
      args+=("$arg")
    fi
  done
  command claude "${args[@]}"
}
```

它会拦截所有 `claude` 命令，把 `--fs` 展开成 `--fork-session`，其余参数保持不变。也可配合别名使用（见 [Tip 7](#tip-7-set-up-terminal-aliases-for-quick-access)）：`c -c --fs`、`ch -c --fs` 等。

下面这个 clone 脚本早于这些内置功能，但其后介绍的 half-clone 脚本在“降低上下文”方面依然独特。

第一条消息会打上 `[CLONED <timestamp>]` 标签（如 `[CLONED Jan 7 14:30]`），它会显示在 `claude -r` 列表和会话内部。

手动安装时，为两个文件建软链接：
```bash
ln -s /path/to/this/repo/scripts/clone-conversation.sh ~/.claude/scripts/clone-conversation.sh
ln -s /path/to/this/repo/skills/clone ~/.claude/skills/clone
```

或者通过 [dx plugin](#tip-44-install-the-dx-plugin) 安装——无需软链接。

然后在任意会话输入 `/clone`（若用 plugin 则 `/dx:clone`），Claude 会自行查找 session ID 并运行脚本。

我做了大量测试，克隆效果很好。

### 用 half-clone 降低上下文

当会话太长时，[half-clone-conversation script](scripts/half-clone-conversation.sh) 只保留后半段。这会降低 token 使用，同时保留你最近的工作内容。第一条消息会打上 `[HALF-CLONE <timestamp>]` 标签（如 `[HALF-CLONE Jan 7 14:30]`）。

手动安装时，为两个文件建软链接：
```bash
ln -s /path/to/this/repo/scripts/half-clone-conversation.sh ~/.claude/scripts/half-clone-conversation.sh
ln -s /path/to/this/repo/skills/half-clone ~/.claude/skills/half-clone
```

或者通过 [dx plugin](#tip-44-install-the-dx-plugin) 安装——无需软链接。

### 用 hook 自动建议 half-clone

可选地，你可以用 [hook](https://docs.anthropic.com/en/docs/claude-code/hooks) 在上下文过长时自动触发 `/half-clone`。[check-context script](scripts/check-context.sh) 在每次 Claude 回复后运行并检查上下文使用率。若超过 85%，它会让 Claude 执行 `/half-clone`，从而创建只含后半段的新会话，供新 agent 接续。

设置方法：先复制脚本：
```bash
cp /path/to/this/repo/scripts/check-context.sh ~/.claude/scripts/check-context.sh
chmod +x ~/.claude/scripts/check-context.sh
```

然后在 `~/.claude/settings.json` 中添加 hook：
```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "~/.claude/scripts/check-context.sh"
          }
        ]
      }
    ]
  }
}
```

这要求关闭 auto-compact（`/config` > Auto-compact > false），否则 Claude Code 可能在 hook 触发前就先 compact。触发后，hook 会阻止 Claude 停止并让它执行 `/half-clone`。相比 auto-compact，half-clone 的优势是确定性强且速度快——它保留真实消息而不是摘要化。

### clone 脚本推荐权限

两个 clone 脚本都需要读取 `~/.claude`（会话文件和历史）。为了避免在任何项目里弹权限请求，把以下配置加入全局设置（`~/.claude/settings.json`）：
```json
{
  "permissions": {
    "allow": ["Read(~/.claude)"]
  }
}
```

## Tip 24：用 realpath 获取绝对路径

当你需要告诉 Claude Code 某个其他目录下的文件时，用 `realpath` 获取完整绝对路径：

```bash
realpath some/relative/path
```

## Tip 25：理解 CLAUDE.md、Skills、Slash Commands 与 Plugins 的区别

这些功能有相似之处，我一开始也觉得很容易混淆。最近我一直在梳理它们，尽力把这套东西吃透，所以想分享我的理解。

**CLAUDE.md** 最简单。它是一组被当作默认 prompt 的文件，不管什么对话都会在开头加载。它的优点是简单。你可以在特定项目里说明项目背景（`./CLAUDE.md`），也可以做全局说明（`~/.claude/CLAUDE.md`）。

**Skills** 像是结构更好的 CLAUDE.md。它们可以在相关时被 Claude 自动调用，也可以由用户手动通过斜杠调用（如 `/my-skill`）。例如你可以做一个 skill：当你问某语言某词怎么读时，自动按正确格式打开 Google Translate 链接。如果这些指令放在 skill 里，就只会在需要时加载；若放在 CLAUDE.md 里，则每次都会占空间。因此理论上 skills 更省 token。

**Slash Commands** 和 skills 类似，都是把指令打包为独立单元。它们可由用户手动调用，也可由 Claude 自行调用。如果你需要更精确、按自己节奏在正确时机触发，slash commands 更合适。

Skills 和 slash commands 在功能上非常接近。区别主要在设计意图——skills 主要给 Claude 使用，slash commands 主要给用户使用。不过它们现在已经[开始融合](https://www.reddit.com/r/ClaudeAI/comments/1q92wwv/merged_commands_and_skills_in_213_update/)，这也是我此前[建议过的改动](https://github.com/anthropics/claude-code/issues/13115)。

**Plugins** 是把 skills、slash commands、agents、hooks、MCP servers 打包在一起的方式。不过插件不一定要包含全部。Anthropic 官方的 `frontend-design` plugin 本质上几乎只是一个 skill。它也可以以独立 skill 形式分发，但 plugin 格式更易安装。

例如我做了一个叫 `dx` 的插件，把本仓库里的若干 slash commands 和一个 skill 打包在一起。可在 [Install the dx plugin](#tip-44-install-the-dx-plugin) 部分查看。

## Tip 26：交互式 PR 审查

Claude Code 非常适合 PR 审查。流程很简单：让它通过 `gh` 命令获取 PR 信息，然后你可以按任意方式推进审查。

你可以做总览审查，也可以按文件逐步审查。节奏由你控制。你决定要看多细、在多高复杂度层面工作。你可能只想理解整体结构，也可能希望它顺便跑测试。

关键区别是：Claude Code 是“可对话的交互式审查者”，而不仅是“一次性输出机器”。有些 AI 工具擅长 one-shot review（包括最新 GPT 模型），但 Claude Code 支持你持续对话迭代。

## Tip 27：把 Claude Code 当作研究工具

Claude Code 对各种研究工作都非常强。它本质上像 Google 替代品或 deep research 替代品，而且在若干维度上更高级。无论你是在研究某些 GitHub Actions 为什么失败（我最近做得很多）、在 Reddit 上做情绪或市场分析、探索代码库，还是挖掘公开信息，它都能做。

关键在于给它正确的信息片段和访问这些信息的指令。可以是 `gh` 终端访问、容器方案（Tip 21）、通过 Gemini CLI 访问 Reddit（Tip 11）、通过 Slack MCP 访问私有信息、或 Cmd+A / Ctrl+A 方法（Tip 10）——都可以。此外，如果 Claude Code 加载某些 URL 有困难，你可以尝试 Playwright MCP 或 Claude 原生浏览器集成（见 Tip 9）。

事实上，我甚至用 Claude Code 做研究[省下了 $10,000](content/how-i-saved-10k-with-claude-code.md)。

## Tip 28：掌握多种验证输出的方法

如果输出是代码，一种验证方式是让它写测试，并确认测试整体合理。这是一种方式；当然你也可以在 Claude Code UI 里随时检查它生成的代码。另一个方法是使用可视化 Git 客户端，例如 GitHub Desktop。我个人在用。它不是完美产品，但足够快速检查变更。另外让它生成 PR（我前文也提过）也是很好的方式。先让它创建 draft PR，审查内容后再转成正式 PR。

还有一种是让它自查。如果它给出某些输出，比如研究结论，你可以说：“你确定吗？能再核查一次吗？”我最常用的提示之一是：“double check everything, every single claim in what you produced and at the end make a table of what you were able to verify”——效果很好。

## Tip 29：Claude Code 作为 DevOps 工程师

我想单独写这一条，因为对我来说它真的非常惊艳。每当 GitHub Actions CI 失败，我就把链接给 Claude Code 并说“深入排查这个问题，找到根因”。有时它会给表层答案，但你只要继续追问——是某个 commit 导致？某个 PR 导致？还是 flaky 问题？——它就能帮你深入这些手工排查很痛苦的棘手问题。你本来得手动翻大量日志，非常折磨，但 Claude Code 能处理其中很大一部分。

我把这个流程打包成了 `/gha` 斜杠命令——直接运行 `/gha <url>`，给任意 GitHub Actions URL，它会自动调查失败、检查是否 flaky、定位破坏性提交并给出修复建议。你可以在 [skills folder](skills/gha/SKILL.md) 找到它，或通过 [dx plugin](#tip-44-install-the-dx-plugin) 安装。

一旦定位出具体问题，你就可以创建 draft PR，并按我前面提到的技巧继续——检查输出、确认质量、让它自证结果，然后转成正式 PR 来真正修复问题。对我个人来说非常有效。

## Tip 30：保持 CLAUDE.md 简洁并定期复查

保持 CLAUDE.md 简洁、尽可能精炼很重要。你完全可以从“没有 CLAUDE.md”开始。如果你发现某些话反复告诉 Claude Code，就把它写进 CLAUDE.md。我知道可以通过 `#` 符号添加，但我更喜欢直接让 Claude Code 帮我加到项目级 CLAUDE.md 或全局 CLAUDE.md，它会知道该改哪里。

![保持简单 meme](assets/keep-it-simple-meme.jpg)

定期复查 CLAUDE.md 也很重要，因为它会随时间过时。以前合理的指令可能不再适用，或者你有新模式应该记录。我做了一个 skill：[`review-claudemd`](skills/review-claudemd/SKILL.md)，会分析你近期对话并给出 CLAUDE.md 改进建议。

## Tip 31：Claude Code 作为通用接口

我以前认为对 Claude Code 而言，CLI 就是新 IDE，这在某种意义上依然正确。它非常适合作为你打开项目做快速修改的第一站。但根据项目重要性，你仍需要比“纯 vibe coding”更严格地审查输出。

更广义地说，Claude Code 实际上是你电脑、数字世界以及各种数字问题的通用接口。很多情况下你可以让它来搞定。比如你要快速剪个视频，可以直接让它做——它大概率会用 ffmpeg 之类工具完成。你要转写一批本地音频或视频文件，也可以直接让它做——它可能会建议用 Python 调 Whisper。你要分析 CSV 数据，它可能建议用 Python 或 JavaScript 做可视化。再加上互联网访问能力——Reddit、GitHub、MCP——可能性几乎无限。

它也很适合本机运维类操作。比如存储快满了，你可以让它给清理建议。它会检查本地目录和文件、找出占空间的大头，并给你清理建议——比如删除特别大的文件。对我来说，它指出了我一些应当清理的超大 Final Cut Pro 文件。也可能建议你用 `docker system prune` 清理无用镜像和容器，或者清理你没意识到仍存在的缓存。现在无论我想在电脑上做什么，Claude Code 都是我的第一入口。

这挺有意思：计算机起源于文本界面，而我们正在某种意义上回到这种文本界面——你可以同时开三四个标签页，就像我前面提到的。对我来说这非常令人兴奋。它像是你有了第二大脑。但因为它只是终端标签页的结构，你还可以再开第三、第四、第五、第六个大脑。随着模型变强，你可委托给它们的思考比例会越来越高——不是最关键的决策，而是你不想做、觉得无聊或繁琐的事，都可以交给它们。正如我提到的，GitHub Actions 排查就是典型例子。谁想手动做这个？但这些 agent 恰好很擅长这种“无聊活”。

## Tip 32：关键在于选择合适的抽象层级

正如前文所说，有时停留在 vibe coding 层面是可以的。对于一次性项目或代码库中非关键部分，你不一定要关心每一行代码。但有时你又需要更深入——看文件结构和函数、逐行代码，甚至检查依赖。

![Vibe coding 光谱](assets/vibe-coding-spectrum.png)

关键是它不是二元的。有人说 vibe coding 不好，因为你不知道自己在做什么；但有些场景下它完全可行。只是另一些场景下，深入挖掘会更有帮助：调用你的软件工程能力，在细粒度层面理解代码，或复制粘贴代码片段/错误日志，让 Claude Code 回答具体问题。

这有点像在探索一座巨大冰山。你要是想停留在 vibe coding 层面，可以从上空掠过、远远观察。然后你可以更靠近一点。你可以进入潜水模式。你还能越潜越深，而 Claude Code 就是你的向导。

## Tip 33：审计你已批准的命令

我最近看到[这个帖子](https://www.reddit.com/r/ClaudeAI/comments/1pgxckk/claude_cli_deleted_my_entire_home_directory_wiped/)，有人的 Claude Code 执行了 `rm -rf tests/ patches/ plan/ ~/`，把整个 home 目录删了。你可以轻易把这归因于 vibe coder 的失误，但这种错误任何人都可能遇到。所以定期审计你批准过的命令很重要。为此我做了 **cc-safe**——一个 CLI，用于扫描 `.claude/settings.json` 中有风险的已批准命令。

它能检测这类模式：
- `sudo`、`rm -rf`、`Bash`、`chmod 777`、`curl | sh`
- `git reset --hard`、`npm publish`、`docker run --privileged`
- 以及更多——它支持容器场景，会跳过 `docker exec` 命令

它会递归扫描所有子目录，因此你可以把项目根目录传给它，一次性检查全部。你可以手动运行，也可以让 Claude Code 代劳：

```bash
npm install -g cc-safe
cc-safe ~/projects
```

或者直接用 npx：

```bash
npx cc-safe .
```

GitHub: [cc-safe](https://github.com/ykdojo/cc-safe)

## Tip 34：写大量测试（并使用 TDD）

随着你用 Claude Code 写的代码越来越多，出错也会更容易。PR 审查和可视化 Git 客户端有助于发现问题（前面提过），但随着代码库变大，写测试至关重要。

你可以让 Claude Code 给自己的代码写测试。有人说 AI 无法测试自己的工作，但事实证明它可以——有点像人脑。写测试时，你是在用另一种方式思考同一个问题。AI 也是一样。

我发现 TDD（测试驱动开发）和 Claude Code 非常契合：

1. 先写测试
2. 确认测试失败
3. 提交这些测试
4. 再写代码使其通过

这正是我构建 [cc-safe](https://github.com/ykdojo/cc-safe) 的方式。先写失败测试并在实现前提交，你就定义了清晰契约：代码应做什么。Claude Code 因此有了明确目标，而你可以通过跑测试验证实现正确性。

若你想更稳妥，自己也要审查测试，确保它们不会做蠢事，比如只是返回 true。

## Tip 35：在未知中更勇敢；迭代式问题求解

自从我更高强度地使用 Claude Code，我发现自己在未知领域越来越大胆。

例如我加入 [Daft](https://github.com/Eventual-Inc/Daft) 后，发现前端代码有问题。我不是 React 专家，但我还是决定深入。就从问代码库和问题本身开始。最终我解决了它，因为我知道如何与 Claude Code 迭代式地解决问题。

最近也有类似经历。我在给 Daft 用户写指南时遇到非常具体的问题：cloudpickle 与 Google Colab + Pydantic 不兼容；另一个是 Python + 一点 Rust，在 JupyterLab 里输出异常，但在终端正常。我之前没做过 Rust。

我本可以提 issue 交给其他工程师处理。但我想，不如自己深入代码库看看。Claude Code 给了初版方案，但不够好。于是我放慢节奏。同事建议直接禁用那部分，但我不想引入回归。能不能有更好的方案？

接下来是协作 + 迭代过程。Claude Code 提出可能根因和方案；我做实验验证；有些是死路就换方向。整个过程中我始终控制节奏：有时加速，让它探索不同方案空间或代码库区域；有时放慢，追问“这行到底是什么意思？”——控制抽象层级，控制速度。

最终我找到了一个很优雅的方案。结论是：即使在未知领域，借助 Claude Code，你能做到的比你想象得更多。

## Tip 36：在后台运行 bash 命令和子代理

当 Claude Code 里有长时间运行的 bash 命令时，你可以按 Ctrl+B 将其移到后台。Claude Code 知道如何管理后台进程——之后可以通过 BashOutput 工具检查它们。

这在你发现命令比预期更久、但希望 Claude 同时做别的事情时非常有用。你可以让它采用 Tip 17 的指数退避法检查进度，或者让它在进程运行期间完全转去做其他工作。

Claude Code 也可以在后台运行子代理。如果你需要长时间研究，或让 agent 周期性检查某事，不必一直前台挂着。直接让 Claude Code 在后台运行 agent/任务，它会处理，你可以继续做别的事。

### 有策略地使用子代理

除了后台运行外，子代理在“拆大任务”时也很有价值。比如你需要分析一个超大代码库，可以让多个子代理并行从不同角度分析，或分别检查不同模块。直接让 Claude 生成多个子代理处理不同部分即可。

你可以通过提问来自定义子代理：
- **数量** - 让 Claude 按你指定数量生成
- **后台或前台** - 让它们在后台运行，或按 Ctrl+B
- **模型选择** - 根据任务复杂度指定 Opus、Sonnet 或 Haiku（子代理默认是 Sonnet）

## Tip 37：个性化软件时代已经到来

我们正在进入个性化、定制化软件时代。AI 出现后——泛指 ChatGPT，但尤其是 Claude Code——我发现自己能做出更多软件，有时只给自己用，有时用于小项目。

正如前文提到，我做了一个每天都在用的定制转写工具来和 Claude Code 对话；我做了定制 Claude Code 自身的方法；我还用 Python 以远快于以往的速度完成了很多数据可视化与分析任务。

再举个例子：[korotovsky/slack-mcp-server](https://github.com/korotovsky/slack-mcp-server) 是一个很热门的 Slack MCP，接近 1,000 stars，设计为 Docker 容器运行。我在自己的 Docker 容器里用它时不够顺畅（Docker-in-Docker 问题）。与其和这套配置死磕，我直接让 Claude Code 用 Slack Node SDK 写了一个 CLI。效果非常好。

这是一个令人兴奋的时代。你想做的事都可以让 Claude Code 去做。如果任务足够小，一两个小时就能做出来。我甚至做了一个[幻灯片模板](https://ykdojo.github.io/claude-code-tips/content/spectrum-slides.html)——一个单 HTML 文件（含 CSS 和 JavaScript），可嵌入交互式、可持久化的终端进程。

## Tip 38：导航并编辑你的输入框

Claude Code 的输入框模拟了常见终端/readline 快捷键，如果你习惯终端工作会非常自然。以下是一些实用快捷键：

**导航：**
- `Ctrl+A` - 跳到行首
- `Ctrl+E` - 跳到行尾
- `Option+Left/Right`（Mac）或 `Alt+Left/Right` - 按词向后/向前跳转

**编辑：**
- `Ctrl+W` - 删除前一个单词
- `Ctrl+U` - 从光标删到行首
- `Ctrl+K` - 从光标删到行尾
- `Ctrl+C` / `Ctrl+L` - 清空当前输入
- `Ctrl+G` - 在外部编辑器中打开你的 prompt（粘贴长文本很有用，因为直接粘贴到终端可能较慢）

如果你熟悉 bash、zsh 或其他 shell，会很快上手。

对于 `Ctrl+G`，编辑器由你的 `EDITOR` 环境变量决定。你可以在 shell 配置（`~/.zshrc` 或 `~/.bashrc`）里设置：

```bash
export EDITOR=vim      # or nano, code, nvim, etc.
```

或在 `~/.claude/settings.json` 中设置（需重启）：

```json
{
  "env": {
    "EDITOR": "vim"
  }
}
```

**输入换行（多行输入）：**

最快且无需配置的方法：输入 `\` 后按 Enter 来创建换行。若要使用快捷键，在 Claude Code 里运行 `/terminal-setup`。在 Mac Terminal.app 上，我用 Option+Enter。

**粘贴图片：**
- `Ctrl+V`（Mac/Linux）或 `Alt+V`（Windows）- 从剪贴板粘贴图片

注意：在 Mac 上是 `Ctrl+V`，不是 `Cmd+V`。

## Tip 39：花点时间规划，但也要快速做原型

你需要花足够时间做规划，让 Claude Code 知道要构建什么、怎么构建。也就是尽早做高层决策：用什么技术、项目如何组织、每个功能放哪里、哪些内容进哪些文件。尽早做出好决策非常重要。

有时做原型能帮助你做这些决策。快速做一个简单原型后，你可能就能判断“这个技术适合这个用途”或“另一个技术更合适”。

比如我最近尝试做一个 diff viewer。先用 tmux + lazygit 做了简单 bash 原型，又尝试用 Ink + Node 自己做 git viewer。过程中踩了很多坑，最终没有发布这些结果。但这个项目再次提醒了我规划和原型的重要性。我发现，只要在让它写代码前先做一点更好的规划，你就能更好地引导它。写码过程中仍需要持续引导，但先让它做点规划非常有帮助。

你可以按 Shift+Tab 切到 plan mode 来做这件事。或者直接让 Claude Code 在写任何代码前先出计划。

## Tip 40：简化过度复杂的代码

我发现 Claude Code 有时会把事情复杂化，写太多代码，改了你没要求改的东西。它似乎有一种“多写代码”的偏好。如果你遵循本指南其他技巧，代码可能仍是正确的，但会更难维护、更难审查。如果你审查不充分，这会很痛苦。

所以有时你需要检查代码并让它简化。你可以自己改，也可以直接让它简化。你可以问：“你为什么做这个改动？”或“你为什么加这行？”

有人说只靠 AI 写代码你永远理解不了代码。但这只在你不问问题时成立。如果你确保每件事都理解，你反而可能比以前更快理解代码，因为可以直接问 AI。尤其在大型项目里。

注意这同样适用于 prose。Claude Code 经常喜欢在最后一段总结前文，或在最后一句总结前句，容易变重复。有时有帮助，但大多数情况下你会需要让它删掉或简化。

## Tip 41：自动化的自动化

归根结底，这一切都是“自动化的自动化”。我的意思是，这不仅能提高效率，也能让过程更有趣。至少对我来说，自动化的自动化非常有乐趣。

我最初从 ChatGPT 开始，想自动化“复制粘贴 ChatGPT 给的命令并在终端执行”的流程。我做了一个 ChatGPT 插件 [Kaguya](https://github.com/ykdojo/kaguya) 来自动化这整套流程。之后我一直在追求更多自动化。

现在幸运的是，我们甚至不用自己造这类工具，因为 Claude Code 这样的工具已经存在且很好用。随着我越来越常用它，我又开始想：如果我能把“输入打字”也自动化呢？于是我用 Claude Code 自己构建了语音转写应用，前面也提到了。

然后我又想到：有些话我会反复说。那就把它们放进 CLAUDE.md。再接着想：有些命令我总在重复执行。怎么自动化？也许让 Claude Code 去做；也许放进 skills；也许让它写脚本，把重复流程彻底自动化。

我认为这就是未来方向。当你发现自己反复做同一个任务或命令，做几次没问题；但如果一直重复，就该考虑把整套流程自动化。

## Tip 42：分享你的知识，并在力所能及处做贡献

这一条和前面的有点不同。我发现只要你尽可能多学，就能把知识分享给身边的人。可以是像这样的帖子，也可以是书、课程、视频。我最近还给 Daft 同事做了一场[内部分享](https://www.daft.ai/blog/how-we-use-ai-coding-agents)，很有成就感。

而且每次我分享技巧，往往也会得到反馈信息。比如我分享了压缩 system prompt 和工具描述的方法（Tip 15）后，有人告诉我可以用 `--system-prompt` 作为替代。又比如我分享 slash commands 和 skills 的差异（Tip 25）后，我从那条 Reddit 帖子的评论里学到了新东西。

所以分享知识不仅是打造个人品牌或巩固学习，也是通过这个过程继续学习。它并不总是单向输出。

在贡献方面，我一直在给 Claude Code 仓库提 issue。我想的是：他们采纳了很好，不采纳也完全没关系。我没有预期。但在 2.0.67 版本里，我看到他们采纳了我报告中的多个建议：

- 修复 `/permissions` 中删除权限规则后滚动位置重置的问题
- 为 `/permissions` 命令增加搜索功能

团队对功能请求和 bug 报告的响应速度非常惊人。但这也合理，因为他们在用 Claude Code 构建 Claude Code 本身。

## Tip 43：持续学习！

持续学习 Claude Code 有几种有效方法：

**直接问 Claude Code 本身** - 如果你对 Claude Code 有问题，直接问它。Claude Code 有专门的子代理来回答其自身功能、斜杠命令、设置、hooks、MCP servers 等问题。

**查看发布说明** - 输入 `/release-notes` 查看当前版本新内容。这是了解最新功能的最佳方式。

**向社区学习** - [r/ClaudeAI](https://www.reddit.com/r/ClaudeAI/) subreddit 是学习他人经验、观察工作流的好地方。

**关注 Ado 的技巧分享** - Ado ([@adocomplete](https://x.com/adocomplete)) 是 Anthropic 的 DevRel，他在 2025 年 12 月的 “Advent of Claude” 系列中每天分享 Claude Code 技巧。虽然该系列已结束，他仍持续在 X 分享实用技巧。

- [Twitter/X: Advent of Claude posts](https://x.com/search?q=from%3Aadocomplete%20advent%20of%20claude&src=typed_query&f=live)
- [LinkedIn: Advent of Claude posts](https://www.linkedin.com/search/results/content/?fromMember=%5B%22ACoAAAFdD3IBYHwKSh6FsyGqOh1SpbrZ9ZHTjnI%22%5D&keywords=advent%20of%20claude&origin=FACETED_SEARCH&sid=zDV&sortBy=%22date_posted%22)

## Tip 44：安装 dx plugin

这个仓库本身也是一个名为 `dx`（developer experience）的 Claude Code 插件。它把上面提到的几个工具整合成一次安装：

| Skill | Description |
|-------|-------------|
| `/dx:gha <url>` | 分析 GitHub Actions 失败（Tip 29） |
| `/dx:handoff` | 创建交接文档以保持上下文连续（Tip 8） |
| `/dx:clone` | 克隆会话以便分叉（Tip 23） |
| `/dx:half-clone` | 半克隆以降低上下文（Tip 23） |
| `/dx:reddit-fetch` | 通过 Gemini CLI 获取 Reddit 内容（Tip 11） |
| `/dx:review-claudemd` | 审查会话并改进 CLAUDE.md 文件（Tip 30） |

**使用两条命令安装：**

```bash
claude plugin marketplace add ykdojo/claude-code-tips
claude plugin install dx@ykdojo
```

安装后，可用命令包括 `/dx:clone`、`/dx:half-clone`、`/dx:handoff`、`/dx:gha`。`reddit-fetch` skill 会在你询问 Reddit URL 时自动调用。`review-claudemd` skill 会分析近期对话并给出 CLAUDE.md 改进建议。关于 clone 命令，参见[推荐权限](#recommended-permission-for-clone-scripts)。

**推荐搭配：** [Playwright MCP](https://github.com/microsoft/playwright-mcp) 用于浏览器自动化 - 通过 `claude mcp add -s user playwright npx @playwright/mcp@latest` 添加

## Tip 45：快速安装脚本

如果你希望一次性配置本仓库里的多项建议，提供了一个安装脚本可处理其中许多项：

```bash
bash <(curl -s https://raw.githubusercontent.com/ykdojo/claude-code-tips/main/scripts/setup.sh)
```

脚本会展示所有将配置的内容，并允许你跳过任意项：

```
INSTALLS:
  1. DX plugin - slash commands (/dx:gha, /dx:clone, /dx:handoff) and skills (reddit-fetch)
  2. cc-safe - scans your settings for risky approved commands like 'rm -rf' or 'sudo'

SETTINGS (~/.claude/settings.json):
  3. Status line - shows model, git branch, uncommitted files, token usage at bottom of screen
  4. Disable auto-updates - prevents Claude Code from auto-updating (useful for system prompt patches)
  5. Lazy-load MCP tools - only loads MCP tool definitions when needed, saves context
  6. Read(~/.claude) permission - allows clone/half-clone commands to read conversation history
  7. Read(//tmp/**) permission - allows reading temporary files without prompts
  8. Disable attribution - removes Co-Authored-By from commits and attribution from PRs

SHELL CONFIG (~/.zshrc or ~/.bashrc):
  9. Aliases: c=claude, ch=claude --chrome, cs=claude --dangerously-skip-permissions
 10. Fork shortcut: --fs expands to --fork-session (e.g., claude -c --fs)

Skip any? [e.g., 1 4 7 or Enter for all]:
```

---

📺 **相关演讲**: [Claude Code Masterclass](https://youtu.be/9UdZhTnMrTA) - 来自 31 个月 agentic coding 的经验与项目示例

📝 **故事**: [我是如何通过 Claude Code 获得全职工作的](content/how-i-got-a-job-with-claude-code.md)

📰 **Newsletter**: [Agentic Coding with Discipline and Skill](https://agenticcoding.substack.com/) - 将 agentic coding 的实践提升到下一个层级