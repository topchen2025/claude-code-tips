# 45 条 Claude Code 技巧：从基础到进阶

以下是我在使用 Claude Code 时获得最大收益的技巧，包括自定义状态行脚本、将系统提示词减半、将 Gemini CLI 作为 Claude Code 的“小弟”，以及让 Claude Code 在容器中运行自身。还包括 [dx 插件](#tip-44-install-the-dx-plugin)。

📺 [快速演示](https://www.youtube.com/watch?v=hiISl558JGE) - 通过多 Claude 工作流与语音输入观看部分技巧的实际效果：

[![演示视频缩略图](assets/demo-thumbnail.png)](https://www.youtube.com/watch?v=hiISl558JGE)

<!-- TOC -->
## 🌐 翻译

[简体中文](./translations/zh-CN/README.md)

---


## 目录

- [Tip 0: Customize your status line](#tip-0-customize-your-status-line)
- [Tip 1: Learn a few essential slash commands](#tip-1-learn-a-few-essential-slash-commands)
- [Tip 2: Talk to Claude Code with your voice](#tip-2-talk-to-claude-code-with-your-voice)
- [Tip 3: Break down large problems into smaller ones](#tip-3-break-down-large-problems-into-smaller-ones)
- [Tip 4: Using Git and GitHub CLI like a pro](#tip-4-using-git-and-github-cli-like-a-pro)
- [Tip 5: AI context is like milk; it's best served fresh and condensed!](#tip-5-ai-context-is-like-milk-its-best-served-fresh-and-condensed)
- [Tip 6: Getting output out of your terminal](#tip-6-getting-output-out-of-your-terminal)
- [Tip 7: Set up terminal aliases for quick access](#tip-7-set-up-terminal-aliases-for-quick-access)
- [Tip 8: Proactively compact your context](#tip-8-proactively-compact-your-context)
- [Tip 9: Complete the write-test cycle for autonomous tasks](#tip-9-complete-the-write-test-cycle-for-autonomous-tasks)
- [Tip 10: Cmd+A and Ctrl+A are your friends](#tip-10-cmda-and-ctrla-are-your-friends)
- [Tip 11: Use Gemini CLI as a fallback for blocked sites](#tip-11-use-gemini-cli-as-a-fallback-for-blocked-sites)
- [Tip 12: Invest in your own workflow](#tip-12-invest-in-your-own-workflow)
- [Tip 13: Search through your conversation history](#tip-13-search-through-your-conversation-history)
- [Tip 14: Multitasking with terminal tabs](#tip-14-multitasking-with-terminal-tabs)
- [Tip 15: Slim down the system prompt](#tip-15-slim-down-the-system-prompt)
- [Tip 16: Git worktrees for parallel branch work](#tip-16-git-worktrees-for-parallel-branch-work)
- [Tip 17: Manual exponential backoff for long-running jobs](#tip-17-manual-exponential-backoff-for-long-running-jobs)
- [Tip 18: Claude Code as a writing assistant](#tip-18-claude-code-as-a-writing-assistant)
- [Tip 19: Markdown is the s**t](#tip-19-markdown-is-the-st)
- [Tip 20: Use Notion to preserve links when pasting](#tip-20-use-notion-to-preserve-links-when-pasting)
- [Tip 21: Containers for long-running risky tasks](#tip-21-containers-for-long-running-risky-tasks)
- [Tip 22: The best way to get better at using Claude Code is by using it](#tip-22-the-best-way-to-get-better-at-using-claude-code-is-by-using-it)
- [Tip 23: Clone/fork and half-clone conversations](#tip-23-clonefork-and-half-clone-conversations)
- [Tip 24: Use realpath to get absolute paths](#tip-24-use-realpath-to-get-absolute-paths)
- [Tip 25: Understanding CLAUDE.md vs Skills vs Slash Commands vs Plugins](#tip-25-understanding-claudemd-vs-skills-vs-slash-commands-vs-plugins)
- [Tip 26: Interactive PR reviews](#tip-26-interactive-pr-reviews)
- [Tip 27: Claude Code as a research tool](#tip-27-claude-code-as-a-research-tool)
- [Tip 28: Mastering different ways of verifying its output](#tip-28-mastering-different-ways-of-verifying-its-output)
- [Tip 29: Claude Code as a DevOps engineer](#tip-29-claude-code-as-a-devops-engineer)
- [Tip 30: Keep CLAUDE.md simple and review it periodically](#tip-30-keep-claudemd-simple-and-review-it-periodically)
- [Tip 31: Claude Code as the universal interface](#tip-31-claude-code-as-the-universal-interface)
- [Tip 32: It's all about choosing the right level of abstraction](#tip-32-its-all-about-choosing-the-right-level-of-abstraction)
- [Tip 33: Audit your approved commands](#tip-33-audit-your-approved-commands)
- [Tip 34: Write lots of tests (and use TDD)](#tip-34-write-lots-of-tests-and-use-tdd)
- [Tip 35: Be braver in the unknown; iterative problem solving](#tip-35-be-braver-in-the-unknown-iterative-problem-solving)
- [Tip 36: Running bash commands and subagents in the background](#tip-36-running-bash-commands-and-subagents-in-the-background)
- [Tip 37: The era of personalized software is here](#tip-37-the-era-of-personalized-software-is-here)
- [Tip 38: Navigating and editing your input box](#tip-38-navigating-and-editing-your-input-box)
- [Tip 39: Spend some time planning, but also prototype quickly](#tip-39-spend-some-time-planning-but-also-prototype-quickly)
- [Tip 40: Simplify overcomplicated code](#tip-40-simplify-overcomplicated-code)
- [Tip 41: Automation of automation](#tip-41-automation-of-automation)
- [Tip 42: Share your knowledge and contribute where you can](#tip-42-share-your-knowledge-and-contribute-where-you-can)
- [Tip 43: Keep learning!](#tip-43-keep-learning)
- [Tip 44: Install the dx plugin](#tip-44-install-the-dx-plugin)
- [Tip 45: Quick setup script](#tip-45-quick-setup-script)

<!-- /TOC -->

## Tip 0: Customize your status line

你可以自定义 Claude Code 底部的状态行来显示有用信息。我将其设置为显示模型、当前目录、git 分支（如果有）、未提交文件数、与 origin 的同步状态，以及一个标记 token 使用的进度条。它还会显示第二行，写上我最后一条消息，方便回忆对话内容：

```
Opus 4.5 | 📁claude-code-tips | 🔀main (scripts/context-bar.sh uncommitted, synced 12m ago) | ██░░░░░░░░ 18% of 200k tokens
💬 This is good. I don't think we need to change the documentation as long as we don't say that the default color is orange el...
```

这对监控上下文使用和记住你正在做什么特别有帮助。脚本还支持 10 种配色主题（orange、blue、teal、green、lavender、rose、gold、slate、cyan 或 gray）。

![颜色预览选项](scripts/color-preview.png)

要设置此功能，可以使用[这个示例脚本](scripts/context-bar.sh)，并查看[设置说明](scripts/README.md)。

## Tip 1: Learn a few essential slash commands

有一堆内置的斜杠命令（输入 `/` 查看全部）。这里有几个值得了解的：

### /usage

查看你的速率限制：

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

如果想密切关注用量，保持它在一个标签页里，用 Tab 然后 Shift+Tab 或 ← 然后 → 刷新。

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

查看你的使用统计，并有 GitHub 风格的活跃度图：

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

清空对话，重新开始。

## Tip 2: Talk to Claude Code with your voice

我发现用语音交流比用手打字快得多。在本地使用语音转录系统对此很有帮助。

在我的 Mac 上，我试过几个选项：
- [superwhisper](https://superwhisper.com/)
- [MacWhisper](https://goodsnooze.gumroad.com/l/macwhisper)
- [Super Voice Assistant](https://github.com/ykdojo/super-voice-assistant)（开源，支持 Parakeet v2/v3）

使用托管服务可以获得更高的准确率，但我发现本地模型已经足够用于此目的。即使转录里有错误或拼写问题，Claude 也足够智能能理解你想说什么。有时你需要更清晰地说某些东西，但总体而言本地模型足够好用。

例如，在下面的截图里，你可以看到 Claude 能正确把错误转录的 “ExcelElanishMark”和“advast” 解释成 “exclamation mark”和 “Advanced”：

![语音转录错误被正确理解](assets/voice-transcription-mistakes.png)

我认为最好的思路是把这当作与你朋友交流。当然，你可以通过文字交流，对一些人或邮件来说可能更容易，对吧？完全没问题。这也是多数人似乎在用 Claude Code 的方式。但如果你想更快交流，为什么不打个电话呢？你可以直接发语音消息。你不需要真的给 Claude Code 打电话，只要发一串语音消息就行。至少对我来说，这更快，因为我过去几年练习过大量的口语。我想对大多数人来说也会更快。

常见的反对意见是“如果你在有人的房间里怎么办？”我会用耳机小声说话——我个人喜欢 Apple EarPods（不是 AirPods）。它们价格实惠，质量够用，只需轻声对着它们说。我在其他人面前也这样做，效果很好。在办公室里人们也会说话——只是你不是跟同事说话，而是在对着语音转录系统小声说。我觉得这没什么问题。这种方法好用到甚至在飞机上也行。声音足够小，别人听不到，但如果你离麦足够近，本地模型仍然能理解你在说什么。（事实上，我正在飞机上用这种方法写下这一段。）

## Tip 3: Break down large problems into smaller ones

这是最重要的概念之一。它和传统软件工程完全一样——最优秀的软件工程师已经会这样做，这同样适用于 Claude Code。

如果你发现 Claude Code 无法一次性解决一个难题或编码任务，就让它把问题拆分成多个更小的问题。看看它能否解决问题的某个部分。如果仍然太难，再看能否解决更小的子问题。一直拆分，直到所有部分都可解。

本质上，与其从 A 一步跳到 B：

![直接方式](assets/breakdown-direct.png)

不如走 A → A1 → A2 → A3，再到 B：

![逐步方式](assets/breakdown-steps.png)

一个很好的例子是我在构建自己的语音转录系统时，需要搭建一个系统让用户选择并下载模型、接收键盘快捷键、开始转录、把转录文本放在用户光标处，并用一个漂亮的 UI 包裹起来。这很多。所以我把它拆分成更小的任务。首先创建一个可执行文件，只下载模型，其他不做。然后创建另一个只负责录音，别的都不做。再创建一个只负责转录预录音频。这样一个个完成，最后再把它们组合起来。

高度相关的一点是：你的问题解决能力和软件工程技能在智能化编码和 Claude Code 的世界里依然非常重要。它可以自己解决很多问题，但当你把通用的问题解决和软件工程技能应用到它上面，会让它更强大。

## Tip 4: Using Git and GitHub CLI like a pro

只要让 Claude 处理你的 Git 和 GitHub CLI 任务。这包括提交（不用手写提交信息）、创建分支、pull 和 push。

我个人默认允许自动 pull，但不允许自动 push，因为 push 风险更大——如果 pull 出问题不会污染 origin。

对于 GitHub CLI（`gh`），能做的事情很多。用 Claude Code 之后，我更常用的一件事是创建草稿 PR。这样 Claude Code 可以低风险地处理 PR 创建流程——你可以在标记为 ready for review 前先审阅一切。

结果发现，`gh` 其实很强大。你甚至可以通过它发送任意 GraphQL 查询。例如，你还可以找到 GitHub PR 描述被编辑的确切时间：

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

### 禁用 commit/PR 归因

默认情况下，Claude Code 会在提交里添加 `Co-Authored-By`，在 PR 里添加署名尾注。你可以在 `~/.claude/settings.json` 中添加：

```json
{
  "attribution": {
    "commit": "",
    "pr": ""
  }
}
```

把两者设为空字符串即可完全去除归因。这取代了旧的 `includeCoAuthoredBy` 设置，该设置现已弃用。

## Tip 5: AI context is like milk; it's best served fresh and condensed!

开始与 Claude Code 新对话时效果最好，因为它不必处理前面对话的复杂上下文。但随着对话时间越长，上下文越长，性能往往下降。

因此，每个新主题最好开启一个新对话，或在性能开始下降时换新对话。

## Tip 6: Getting output out of your terminal

有时你想复制 Claude Code 的输出，但直接从终端复制并不总是干净。以下方法能更容易提取内容：

- **`/copy` 命令**：最简单的选项——只需输入 `/copy`，将 Claude 最近的响应以 markdown 形式复制到剪贴板
- **直接剪贴板**：在 Mac 或 Linux 上，让 Claude 用 `pbcopy` 将输出直接发送到剪贴板
- **写入文件**：让 Claude 把内容写入文件，再让它在 VS Code（或你喜欢的编辑器）中打开，你即可从那里复制。还能指定行号，这样就能让 Claude 打开它刚编辑的具体行。对 markdown 文件，打开 VS Code 后可以使用 Cmd+Shift+P（或 Linux/Windows 上 Ctrl+Shift+P），选择 “Markdown: Open Preview” 查看渲染版本
- **打开 URL**：如果有想自己检查的 URL，让 Claude 在浏览器中打开。在 Mac 上，可让它用 `open` 命令，但通常让它在你喜欢的浏览器中打开即可适用于任何平台
- **GitHub Desktop**：可以让 Claude 在 GitHub Desktop 中打开当前仓库。当它在非根目录工作时尤其有用——例如，你让它在其他目录创建 git worktree，而你尚未从那里启动 Claude Code

你也可以组合使用这些方法。例如，如果想编辑 GitHub PR 描述，与其让 Claude 直接改（可能会弄乱），不如先让它把内容复制到本地文件里。让它先编辑，再自己检查，满意后再让它复制粘贴回 GitHub PR。效果很好。或者如果你想自己做，只要让它在 VS Code 中打开，或通过 pbcopy 提供给你，然后手动复制粘贴。

当然你可以自己运行这些命令，但如果发现自己重复做这些事，不妨让 Claude 来帮忙。

## Tip 7: Set up terminal aliases for quick access

由于使用 Claude Code，我在终端中的使用频率提高了，设置简短的别名可以快速启动工具。以下是我用的：

- `c` 对应 Claude Code（我用得最多）
- `ch` 对应开启 Chrome 集成的 Claude Code
- `gb` 对应 GitHub Desktop
- `co` 对应 VS Code
- `q` 对应跳转到放置大多数项目的目录。从那里可以手动 cd 进入具体项目，或者直接用 `c` 启动 Claude Code，让它访问所需项目。

在 shell 配置文件（`~/.zshrc` 或 `~/.bashrc`）中添加如下行即可：

```bash
alias c='claude'
alias ch='claude --chrome'
alias gb='github'
alias co='code'
alias q='cd ~/Desktop/projects'
```

有了这些别名后，可以结合参数使用：`c -c` 继续上一对话，`c -r` 显示最近对话列表以恢复。这些也适用于 `ch`（`ch -c`，`ch -r`）用于 Chrome 会话。

## Tip 8: Proactively compact your context

Claude Code 有一个 `/compact` 命令，用来总结对话以释放上下文空间。当可用上下文满时，也会自动压缩。Opus 4.5 的总可用上下文窗口目前是 200k，其中 45k 预留给自动压缩。总 200k 中约 10% 会自动用于系统提示、工具、记忆和动态上下文。但我发现主动手动压缩更好，能更精细地调节。我通过 `/config` 关闭了自动压缩，这样主对话有更多上下文可用，并且可以控制何时以及如何压缩。

我的做法是在开始新对话前，让 Claude 写一个交接文档。类似这样：

> Put the rest of the plan in the system-prompt-extraction folder as HANDOFF.md. Explain what you have tried, what worked, what didn't work, so that the next agent with fresh context is able to just load that file and nothing else to get started on this task and finish it up.

Claude 会创建一个文件总结当前工作状态：

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

Claude 写完后，快速检查一下。如果缺少内容，要求它修改：

> Did you add a note about iteratively testing instead of trying to do everything all at once?

然后开始新的对话。对新的 agent，你只需给出这个文件路径即可，如此即可正常工作：

```
> experiments/system-prompt-extraction/HANDOFF.md
```

在之后的对话中，可以让 agent 更新这个文档给下一个 agent。

我还创建了一个 `/handoff` 斜杠命令来自动化这一流程——它会检查是否已有 HANDOFF.md，如有则读取，然后创建或更新，包含目标、进展、有效与无效之处、下一步。你可以在 [skills 文件夹](skills/handoff/SKILL.md) 找到，或通过 [dx 插件](#tip-44-install-the-dx-plugin) 安装。

**替代方案：使用计划模式**

另一种选择是使用计划模式。通过 `/plan` 或 Shift+Tab 进入。让 Claude 收集所有相关上下文，为下一位 agent 制定全面计划：

> I just enabled plan mode. Bring over all of the context that you need for the next agent. The next agent will not have any other context, so you'll need to be pretty comprehensive.

Claude 会探索代码库，收集上下文，写出详细计划。完成后你会看到类似这样的选项：

```
Would you like to proceed?

❯ 1. Yes, clear context and auto-accept edits (shift+tab)
  2. Yes, auto-accept edits
  3. Yes, manually approve edits
  4. Type here to tell Claude what to change
```

选项 1 会清空之前的上下文，并使用这个计划重新开始。新的 Claude 实例只看到这个计划，因此可以专注，无需背负旧对话包袱。它还能获得旧的转录文件链接，必要时可以查具体细节。

## Tip 9: Complete the write-test cycle for autonomous tasks

如果你想让 Claude Code 自主运行某些任务，比如 `git bisect`，你需要给它验证结果的方法。关键是完成“写-测”循环：写代码、运行、检查输出、再循环。

例如，假设你在开发 Claude Code 本身，发现 `/compact` 失效并报 400。经典工具 `git bisect` 能找出导致问题的提交。好处是你可以让 Claude Code 对自身运行 bisect，但它需要测试每次提交的方法。

对涉及交互式终端的任务（如 Claude Code），你可以用 tmux。模式如下：

1. 启动 tmux 会话
2. 向其中发送命令
3. 捕获输出
4. 验证是否符合预期

下面是测试 `/context` 是否可用的简单示例：

```bash
tmux kill-session -t test-session 2>/dev/null
tmux new-session -d -s test-session
tmux send-keys -t test-session 'claude' Enter
sleep 2
tmux send-keys -t test-session '/context' Enter
sleep 1
tmux capture-pane -t test-session -p
```

有了这种测试，Claude Code 就可以运行 `git bisect`，自动测试每个提交直到找到破坏它的提交。

这也说明了为什么你的软件工程技能仍然重要。如果你是软件工程师，可能知道 `git bisect` 之类的工具。这些知识在与 AI 合作时仍然很有价值，只是以新的方式应用。

另一个例子是写测试。让 Claude Code 写完代码后，如果你想测试，也可以让它写测试并自行运行，能修就修。当然它不一定总是走对路，有时你需要监督，但它能独立完成的编码任务令人惊讶。

### 创意的测试策略

有时你需要用创造性方式完成“写-测”循环。例如如果你在构建 web 应用，可以使用 Playwright MCP、Chrome DevTools MCP，或者 Claude 的原生浏览器集成（通过 `/chrome`）。我没试过 Chrome DevTools，但试过 Playwright 和 Claude 的原生集成。总体而言，Playwright 通常更好。它确实消耗大量上下文，但 200k 的窗口通常足够用于单个任务或几个小任务。

这两者的主要区别似乎是，Playwright 更关注可访问性树（页面元素的结构化数据），而不是截图。它可以截图，但通常不会用截图来决定动作。另一方面，Claude 的原生浏览器集成更依赖截图并通过具体坐标点击元素。它有时会点到随机东西，过程也可能比较慢。

这点可能会随时间改进，但默认情况下，我会选择 Playwright 来处理大多数非视觉密集的任务。只有在需要使用登录状态且不想提供凭据（因为它运行在你自己的浏览器配置文件里），或者确实需要按坐标视觉点击时，才会用 Claude 的原生集成。

这就是我默认禁用 Claude 原生浏览器集成，并使用之前定义的 `ch` 快捷方式的原因。这样 Playwright 处理大部分浏览器任务，只有特别需要时才启用原生集成。

此外，你可以让它使用可访问性树的 ref 而非坐标。以下是我放在 CLAUDE.md 中的内容：

```markdown
# Claude for Chrome

- Use `read_page` to get element refs from the accessibility tree
- Use `find` to locate elements by description
- Click/interact using `ref`, not coordinates
- NEVER take screenshots unless explicitly requested by the user
```

以我的个人经验，我还遇到过一个情况：我在一个 Python 库（[Daft](https://github.com/Eventual-Inc/Daft)）上工作，需要在 Google Colab 上测试我本地构建的版本。麻烦在于很难在 Google Colab 上构建带 Rust 后端的 Python 库——看起来不太好用。所以我需要在本地构建一个 wheel，再手动上传到 Colab 来运行。我也尝试过 monkey patch，在本地等待整个 wheel 构建前短期很有效。我想出这些测试策略，并通过反复与 Claude Code 交互来执行。

另一种情况是我需要在 Windows 上测试，但我没有 Windows 机器。相同仓库的 CI 测试在 Windows 上失败了，因为我们有一些 Rust on Windows 的问题，我本地无法测试。所以我需要创建一个草稿 PR 放入所有更改，另一个草稿 PR 放入相同更改并启用非主分支的 Windows CI 运行。我让 Claude Code 完成这一切，然后直接在新分支里跑 CI 测试。

## Tip 10: Cmd+A and Ctrl+A are your friends

这些年来我一直在说：在 AI 世界里，Cmd+A 和 Ctrl+A 是好朋友。这同样适用于 Claude Code。

有时你想给 Claude Code 一个 URL，但它不能直接访问。也许是私有页面（不一定敏感，只是不公开），或像 Reddit 帖子这种 Claude Code 难以获取的情况。这时你可以直接全选页面内容（Mac 上 Cmd+A，其他平台 Ctrl+A），复制，粘贴到 Claude Code。非常好用。

对于终端输出也很有效。当我有 Claude Code 本身或其他 CLI 应用的输出时，可以用同样技巧：全选、复制，再粘贴回 CC。非常有用。

有些页面默认不便于全选——但有办法先让它们处于更好的状态。例如 Gmail 线程，点击 Print All 查看打印预览（取消实际打印）。那一页会展开所有邮件，可以一键 Cmd+A 完整复制对话。

这对任何 AI 都适用，不止 Claude Code。

## Tip 11: Use Gemini CLI as a fallback for blocked sites

Claude Code 的 WebFetch 工具无法访问某些网站，比如 Reddit。但你可以通过创建一个 skill，指示 Claude 使用 Gemini CLI 作为后备来绕过。Gemini 具备网络访问，能获取 Claude 无法直接访问的站点内容。

这使用了 Tip 9 中相同的 tmux 模式——启动会话、发送命令、捕获输出。skill 文件放在 `~/.claude/skills/reddit-fetch/SKILL.md`。完整内容见 [skills/reddit-fetch/SKILL.md](skills/reddit-fetch/SKILL.md)。

Skills 更节省 token，因为 Claude Code 仅在需要时加载它们。如果想要更简单的方式，可以把精简版本放在 `~/.claude/CLAUDE.md`，但那会在每个对话开头加载，无论是否需要。

我通过让 Claude Code 检查 Reddit 上对 Claude Code skills 的看法来测试过——有点元。它会和 Gemini 来回交互一阵子，所以不快，但报告质量惊人地不错。当然，你需要安装 Gemini CLI 才能工作。你也可以通过 [dx 插件](#tip-44-install-the-dx-plugin) 安装这个 skill。

## Tip 12: Invest in your own workflow

我个人用 Swift 从零写了自己的语音转录应用。用 Claude Code（bash）从零做了自定义状态行。我还创建了简化 Claude Code 压缩版 JavaScript 文件的系统提示词的系统。

但你不必如此极端。照顾好自己的 CLAUDE.md，确保足够简洁同时能帮你达成目标——类似这些就很有帮助。当然还有学习这些技巧、工具，以及一些最重要的功能。

这些都是你用来构建任何东西的工具上的投资。我认为至少应该花些时间在这些上面。

## Tip 13: Search through your conversation history

你可以让 Claude Code 查询过去的对话，它会帮你查找和搜索。你的对话历史保存在 `~/.claude/projects/`，文件夹名基于项目路径（斜杠转成连字符）。

例如，对应 `/Users/yk/Desktop/projects/claude-code-tips` 的对话会存放在：

```
~/.claude/projects/-Users-yk-Desktop-projects-claude-code-tips/
```

每个对话是一个 `.jsonl` 文件。你可以用基本的 bash 命令搜索：

```bash
# 找到所有提到 "Reddit" 的对话
grep -l -i "reddit" ~/.claude/projects/-Users-yk-Desktop-projects-*/*.jsonl

# 找到当天关于某个主题的对话
find ~/.claude/projects/-Users-yk-Desktop-projects-*/*.jsonl -mtime 0 -exec grep -l -i "keyword" {} \;

# 只提取对话中的用户消息（需要 jq）
cat ~/.claude/projects/.../conversation-id.jsonl | jq -r 'select(.type=="user") | .message.content'
```

或者直接问 Claude Code：“我们今天关于 X 讨论了什么？”它会帮你搜索历史。

## Tip 14: Multitasking with terminal tabs

运行多个 Claude Code 实例时，保持有序比任何具体的技术设置（如 Git worktree）更重要。我建议最多同时关注三四个任务。

我的个人方法是我称之为“级联”——每当开始新任务，就在右侧打开一个新标签。然后从左到右、左到右扫，从最旧的任务到最新。大致方向保持一致，除非需要查看某些任务或接收通知等。

我的常见布局如下：

![显示多任务工作流的终端标签页](assets/multitasking-terminal-tabs.png)

示例中：
1. **最左标签** - 运行我的语音转录系统的常驻标签（始终在此）
2. **第二个标签** - 设置 Docker 容器
3. **第三个标签** - 检查本机磁盘使用
4. **第四个标签** - 做工程项目
5. **第五个标签（当前）** - 编写这一条提示

## Tip 15: Slim down the system prompt

Claude Code 的系统提示词和工具定义在你开始工作前就占了约 19k tokens（200k 上下文的 ~10%）。我创建了一个补丁系统，将其降到约 9k tokens——节省约 10,000 tokens（~50% 开销）。

| Component | Before | After | Savings |
|-----------|--------|-------|---------|
| System prompt | 3.0k | 1.8k | 1,200 tokens |
| System tools | 15.6k | 7.4k | 8,200 tokens |
| **Total** | **~19k** | **~9k** | **~10k tokens (~50%)** |

以下是 `/context` 打补丁前后的对比：

**未打补丁（~20k，10%）**

![未打补丁的上下文](assets/context-unpatched.png)

**已打补丁（~10k，5%）**

![已打补丁的上下文](assets/context-patched.png)

补丁通过裁剪压缩版 CLI 包中冗长的示例和重复文本，同时保留所有必要指令来工作。

我广泛测试过，效果很好。感觉更“原始”——更强大，但可能稍微少点管控，这也合理，因为系统指令更短。使用起来更像专业工具。我特别喜欢起始上下文更小，因为填满前有更多空间，可以延长对话，这是这种策略最棒的部分。

查看 [system-prompt 文件夹](system-prompt/)获取补丁脚本和详细裁剪内容。

**为何要打补丁？** Claude Code 有参数可以从文件提供简化的系统提示词（`--system-prompt` 或 `--system-prompt-file`），这是另一个方法。但对工具描述，没有官方选项自定义。给 CLI 包打补丁是唯一方式。由于我的补丁系统统一处理所有部分，目前我保持这种方式。未来可能用该参数重新实现系统提示词部分。

**支持的安装方式：** npm 和原生二进制（macOS 与 Linux）。

**重要**：如果想保留已打补丁的系统提示词，在 `~/.claude/settings.json` 中禁用自动更新：

```json
{
  "env": {
    "DISABLE_AUTOUPDATER": "1"
  }
}
```

这适用于所有 Claude Code 会话，无论 shell 类型（交互、非交互、tmux）。你可在准备好对新版本重新打补丁时手动更新。

### 延迟加载 MCP 工具

如果你使用 MCP 服务器，它们的工具定义默认在每个对话中加载——即使未使用。这会增加开销，尤其在配置多个服务器时。

启用延迟加载，仅在需要时加载 MCP 工具：

```json
{
  "env": {
    "ENABLE_TOOL_SEARCH": "true"
  }
}
```

将此添加到 `~/.claude/settings.json`。Claude 会按需搜索和加载 MCP 工具，而不是一开始全部加载。截至 2.1.7 版本，当 MCP 工具描述超过上下文窗口的 10% 时会自动这样做。

## Tip 16: Git worktrees for parallel branch work

如果你在同一项目中同时处理多项任务，不希望互相冲突，Git worktree 是很好的选择。你可以直接让 Claude Code 创建 git worktree 并在那里开始工作——无需操心具体语法。

基本思路是在不同目录中处理不同分支。本质上是一个分支 + 一个目录。

你可以在我之前多任务提示中提到的级联方法之上，再加一层 Git worktree。

### 什么是 git worktree？

git worktree 就像其他 git 分支，但专门有一个新目录分配给它。

所以如果你在处理 main 分支和 feature-branch-1，没有 git worktree 时，只能一次处理一个分支，因为项目文件夹只能处于一个分支。

有了 git worktree，你可以在原始项目文件夹继续处理 main（或其他）分支，同时在新文件夹处理 feature-branch-1。

![Git worktree 示意：在独立目录中并行处理分支](assets/git-worktrees.png)

## Tip 17: Manual exponential backoff for long-running jobs

等待 Docker 构建或 GitHub CI 等长任务时，可以让 Claude Code 手动做指数退避。指数退避是常见的工程技术，在此同样适用。让 Claude Code 以递增的 sleep 间隔检查状态——一分钟、两分钟、四分钟，以此类推。这不是传统意义上代码实现的退避，而是由 AI 手动执行，但效果不错。

这样 agent 就能持续检查状态，并在完成时通知你。

（对于 GitHub CI，虽然有 `gh run watch`，但会连续输出很多行，浪费 tokens。手动指数退避结合 `gh run view <run-id> | grep <job-name>` 反而更节省 token。这也是在没有专用等待命令时的通用好方法。）

例如，如果有一个 Docker 构建在后台运行：

![手动指数退避检查 Docker 构建进度](assets/manual-exponential-backoff.png)

它会一直检查，直到任务完成。

## Tip 18: Claude Code as a writing assistant

Claude Code 是出色的写作助手和伙伴。我用它写作的方式是，先给它所有上下文（我要写什么），然后用语音给它详细指令，生成第一稿。如果不够好，再试几次。

然后我几乎会逐行检查。一起看看：我喜欢这行的原因，那行需要挪到那里，那行需要这样改。我也可能询问参考资料。

于是形成这种来回的过程，也许左边是终端，右边是代码编辑器：

![Claude Code 并排写作工作流](assets/writing-assistant-side-by-side.png)

这种方式效果很好。

## Tip 19: Markdown is the s**t

通常人们写新文档可能用 Google Docs 或 Notion。但现在我真心认为最有效的是 markdown。

Markdown 在 AI 之前就已经很不错了，尤其有了 Claude Code，我前面提到过写作效率，markdown 的价值更高。无论写博客还是 LinkedIn 帖子，你都可以直接和 Claude Code 交谈，让它保存为 markdown，再继续处理。

一个快速小技巧：如果要把 markdown 内容粘贴到不方便接收 markdown 的平台，可以先粘贴到全新的 Notion 文档，然后再从 Notion 复制到其他平台。Notion 会把它转换成其他平台能接受的格式。如果普通粘贴不行，试试 Command + Shift + V 去掉格式粘贴。

## Tip 20: Use Notion to preserve links when pasting

反过来也成立。如果你有带链接的文本（比如来自 Slack），复制后直接粘贴到 Claude Code，链接不会显示。但如果先放进 Notion 文档，再从那里复制，就会得到 markdown，Claude Code 自然能读。

## Tip 21: Containers for long-running risky tasks

常规会话更适合需要更谨慎的工作，你会控制权限并更仔细审查输出。容器化环境非常适合 `--dangerously-skip-permissions` 会话，这样无需对每件小事都授权。可以让它自主运行一段时间。

这对研究或实验很有用，耗时长且可能有风险。一个好例子是 Tip 11 的 Reddit 研究流程，reddit-fetch skill 通过 tmux 与 Gemini CLI 来回。将其无人值守地在主系统跑很危险，但在容器里，如果出了问题也被限制住。

另一个例子是我在本仓库创建的 [系统提示词打补丁脚本](system-prompt/)。当 Claude Code 出新版本时，我需要更新压缩版 CLI 包的补丁。与其在主机上用 `--dangerously-skip-permissions` 运行（可访问一切），我会在容器里运行。Claude Code 可以探索压缩 JavaScript，找到变量映射，创建新补丁文件，而无需我批准每件小事。

事实上，它几乎能自行完成迁移。它尝试应用补丁，发现部分与新版本不兼容，便迭代修复，甚至根据所学改进了供未来使用的[说明文档](system-prompt/UPGRADING.md)。

我还创建了 [SafeClaw](https://github.com/ykdojo/safeclaw)，让在容器中运行 Claude Code 会话变得简单。它可以启动多个隔离会话，每个都有 web 终端，并从仪表板管理。它使用了本仓库的几个自定义内容，包括优化的系统提示词、[DX 插件](#tip-44-install-the-dx-plugin)和[状态行](#tip-0-customize-your-status-line)。

### 进阶：在容器中编排一个工作 Claude Code

你可以更进一步，让本地 Claude Code 控制另一个在容器中运行的 Claude Code。诀窍是用 tmux 作为控制层：

1. 本地 Claude Code 启动一个 tmux 会话
2. 在 tmux 会话里运行或连接到容器
3. 容器内运行 `--dangerously-skip-permissions` 的 Claude Code
4. 外层 Claude Code 用 `tmux send-keys` 发送 prompt，用 `capture-pane` 读取输出

这样你就有一个完全自主的“worker” Claude Code，可进行实验或长任务，无需对每个动作授权。完成后，本地 Claude Code 能把结果取回。如果出问题，一切都在容器沙盒中。

### 进阶：多模型编排

不仅是 Claude Code，你可以在容器里跑不同的 AI CLI——Codex、Gemini CLI 等。我尝试过用 OpenAI Codex 做代码审查，效果不错。关键不是这些 CLI 不能在宿主机直接跑——当然可以。而是 Claude Code 的 UI/UX 足够顺畅，你可以直接和它对话，让它处理编排：启动不同模型，在容器与宿主之间发送数据。无需手动切换终端和复制粘贴，Claude Code 作为中心接口协调一切。

## Tip 22: The best way to get better at using Claude Code is by using it

最近我看到一位世界级攀岩者被另一位攀岩者采访。被问到“如何变得更会攀岩？”她简单回答：“多攀岩。”

我对这件事的感受也是如此。当然，你可以辅以其他事情，比如看视频、读书、学技巧。但使用 Claude Code 就是学习使用它的最好方法。使用 AI 本身就是学习 AI 的最佳方式。

我喜欢把它看作“十亿 token 法则”，而非“10000 小时法则”。如果你想在 AI 上更厉害，真正获得直觉，最好的方式就是消耗大量 token。如今这成为可能。尤其是 Opus 4.5，既强大又价格合适，你可以同时运行多个会话。不必过于担心 token 使用，能让你轻松很多。

## Tip 23: Clone/fork and half-clone conversations

有时你想从对话的某个节点尝试不同方法，又不想丢失原线程。[clone-conversation 脚本](scripts/clone-conversation.sh)能将对话复制并生成新的 UUID，以便分支。

**内置替代（近期版本）：** Claude Code 现在有原生分叉功能：
- `/fork` - 在对话中分叉当前会话
- `--fork-session` - 配合 `--resume` 或 `--continue` 使用（例如 `claude -c --fork-session`）

由于 `--fork-session` 没有简写，可以在 `~/.zshrc` 或 `~/.bashrc` 添加这个函数，用 `--fs` 作为快捷：

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

它会拦截所有 `claude` 命令，把 `--fs` 展开为 `--fork-session`，其他参数保持不变。配合别名也有效（见 [Tip 7](#tip-7-set-up-terminal-aliases-for-quick-access)）：`c -c --fs`、`ch -c --fs` 等。

下述 clone 脚本早于这些内置选项，但后面的 half-clone 脚本依旧独特，用于减少上下文。

第一条消息会标记 `[CLONED <timestamp>]`（如 `[CLONED Jan 7 14:30]`），会同时出现在 `claude -r` 列表和对话内。

手动设置：为两个文件创建符号链接：
```bash
ln -s /path/to/this/repo/scripts/clone-conversation.sh ~/.claude/scripts/clone-conversation.sh
ln -s /path/to/this/repo/skills/clone ~/.claude/skills/clone
```

或通过 [dx 插件](#tip-44-install-the-dx-plugin) 安装——无需符号链接。

然后在任意对话中输入 `/clone`（使用插件则为 `/dx:clone`），Claude 会自动查找 session ID 并运行脚本。

我广泛测试过，克隆效果很好。

### Half-clone 减少上下文

当对话过长时，[half-clone-conversation 脚本](scripts/half-clone-conversation.sh)只保留后半部分。这样减少 token 使用，同时保留近期工作。首条消息标记为 `[HALF-CLONE <timestamp>]`（如 `[HALF-CLONE Jan 7 14:30]`）。

手动设置：为两个文件创建符号链接：
```bash
ln -s /path/to/this/repo/scripts/half-clone-conversation.sh ~/.claude/scripts/half-clone-conversation.sh
ln -s /path/to/this/repo/skills/half-clone ~/.claude/skills/half-clone
```

或通过 [dx 插件](#tip-44-install-the-dx-plugin) 安装——无需符号链接。

### 用 hook 自动建议 half-clone

可选地，你可以使用一个 [hook](https://docs.anthropic.com/en/docs/claude-code/hooks) 在上下文过长时自动触发 `/half-clone`。[check-context 脚本](scripts/check-context.sh) 在每次 Claude 响应后运行，检查上下文使用量。若超过 85%，它会让 Claude 运行 `/half-clone`，创建只保留后半部分的新对话，供新 agent 继续。

设置方法，先复制脚本：
```bash
cp /path/to/this/repo/scripts/check-context.sh ~/.claude/scripts/check-context.sh
chmod +x ~/.claude/scripts/check-context.sh
```

然后在 `~/.claude/settings.json` 添加 hook：
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

这要求关闭自动压缩（`/config` > Auto-compact > false），否则 Claude Code 可能在 hook 触发前压缩上下文。触发时，hook 会阻止 Claude 停止，让它执行 `/half-clone`。相比自动压缩，half-clone 的优势是确定且快速——保留你真实消息而不是摘要。

### 克隆脚本推荐的权限

两个克隆脚本需要读取 `~/.claude`（获取对话文件和历史）。为避免在任何项目中出现权限提示，把以下内容添加到全局设置（`~/.claude/settings.json`）：
```json
{
  "permissions": {
    "allow": ["Read(~/.claude)"]
  }
}
```

## Tip 24: Use realpath to get absolute paths

当你需要告诉 Claude Code 关于其他文件夹里的文件时，用 `realpath` 获取完整绝对路径：

```bash
realpath some/relative/path
```

## Tip 25: Understanding CLAUDE.md vs Skills vs Slash Commands vs Plugins

这些功能有些类似，我起初也很困惑。一直在拆解理解，想分享我的认识。

**CLAUDE.md** 最简单。是一堆文件，当作默认提示词，不管怎样都会加载到每次对话的开头。简单就是它的优点。你可以解释某个项目在干什么（`./CLAUDE.md`）或全局说明（`~/.claude/CLAUDE.md`）。

**Skills** 类似更有结构的 CLAUDE.md。相关时 Claude 可自动调用，或用户用斜杠手动调用（如 `/my-skill`）。例如你可以有一个 skill，当你问某个语言的发音时，它会打开格式正确的 Google 翻译链接。如果这些指令在 skill 里，只有需要时才加载；若在 CLAUDE.md，则总会占空间。理论上 skill 更节省 token。

**Slash Commands** 在将指令单独打包方面与 skill 类似。可由用户手动调用，也可由 Claude 自行调用。如果你需要更精确、在合适时机按自己的节奏调用的东西，slash 命令是首选。

Skills 和 slash 命令在功能上很相似。区别在设计意图——skills 主要设计供 Claude 使用，slash 命令主要设计给用户用。不过他们已经[合并了](https://www.reddit.com/r/ClaudeAI/comments/1q92wwv/merged_commands_and_skills_in_213_update/)，正如我[建议的改动](https://github.com/anthropics/claude-code/issues/13115)。

**Plugins** 是把 skills、slash commands、agents、hooks、MCP servers 打包在一起的一种方式。但插件不必包含全部。例如 Anthropic 官方的 `frontend-design` 插件本质上只是一个 skill。它可以作为独立 skill 分发，但插件格式更易安装。

例如，我构建了一个名为 `dx` 的插件，将本仓库里的 slash 命令和一个 skill 打包。详情见[安装 dx 插件](#tip-44-install-the-dx-plugin)。

## Tip 26: Interactive PR reviews

Claude Code 很适合做 PR 评审。流程很简单：让它用 `gh` 命令获取 PR 信息，然后按你想要的方式进行审查。

你可以做总体审查，或文件逐个、步骤逐步。由你掌控节奏和细节深度。也许你只想了解总体结构，或者想让它跑测试。

关键区别在于 Claude Code 充当互动式 PR 审查者，而不仅仅是一次性机器。有些 AI 工具擅长一次性审查（包括最新 GPT 模型），但用 Claude Code 可以持续对话。

## Tip 27: Claude Code as a research tool

Claude Code 对任何研究都很棒。它本质上是 Google 或深度研究的替代品，但在几个方面更高级。不论是研究为何某些 GitHub Actions 失败（我最近常做）、在 Reddit 上做情绪或市场分析、探索代码库，还是查找公开信息——它都能做到。

关键是给它合适的信息和获取方式。可能是 `gh` 命令行访问，或容器方法（Tip 21），或通过 Gemini CLI 访问 Reddit（Tip 11），或通过 Slack MCP 访问私有信息，或 Cmd+A / Ctrl+A 方法（Tip 10）——无论何种方式。此外，如果 Claude Code 加载某些 URL 有问题，可试用 Playwright MCP 或 Claude 原生浏览器集成（见 Tip 9）。

事实上，我甚至能通过 Claude Code 做研究[节省 1 万美元](content/how-i-saved-10k-with-claude-code.md)。

## Tip 28: Mastering different ways of verifying its output

验证输出是否代码的一种方式是让它写测试，确保测试看起来合理。这是一种方法，但你也可以直接在 Claude Code UI 里检查它生成的代码。另一件事是使用可视化 Git 客户端，比如 GitHub Desktop。我个人用它。不是完美产品，但足够快速检查变更。如前文所说，让它生成 PR 也很棒。让它创建草稿 PR，检查内容后再转成正式 PR。

另一个办法是让它自查。如果它给你某些输出，比如研究结果，你可以说“你确定吗？能否再检查？”我最喜欢的提示之一是：“double check everything, every single claim in what you produced and at the end make a table of what you were able to verify”——效果很好。

## Tip 29: Claude Code as a DevOps engineer

我专门为此写一条提示，因为效果太好了。每当 GitHub Actions CI 失败时，我就给 Claude Code：“深入这个问题，找根因”。有时它给出表面答案，但如果你继续追问——是特定提交、特定 PR 引起的吗，还是偶发问题？——它能帮你深入挖掘难以手动分析的棘手问题。否则你得看一堆日志，手动很痛苦，但 Claude Code 可以处理很多。

我把这个流程打包成一个 `/gha` 斜杠命令——只需对任意 GitHub Actions URL 运行 `/gha <url>`，它会自动调查失败、检查是否偶发、找出破坏性提交，并给出修复建议。可在 [skills 文件夹](skills/gha/SKILL.md) 找到，或通过 [dx 插件](#tip-44-install-the-dx-plugin) 安装。

一旦找到问题所在，就可以创建草稿 PR，按我之前提到的提示检查输出，确保看起来不错，让它自检，然后转成正式 PR 修复问题。对我来说效果非常好。

## Tip 30: Keep CLAUDE.md simple and review it periodically

保持 CLAUDE.md 简洁且尽量精炼很重要。你可以从没有 CLAUDE.md 开始。如果发现自己总反复跟 Claude Code 说同样的事，就把它加到 CLAUDE.md。我知道可以通过 `#` 符号做到，但我更喜欢直接让 Claude Code 把内容加到项目级或全局 CLAUDE.md，它会知道要编辑什么。

![保持简单 meme](assets/keep-it-simple-meme.jpg)

定期审查 CLAUDE.md 也很重要，因为它可能随时间过时。曾经合理的指令可能不再相关，或出现应记录的新模式。我创建了一个名为 [`review-claudemd`](skills/review-claudemd/SKILL.md) 的 skill，会分析你近期对话并为 CLAUDE.md 提供改进建议。

## Tip 31: Claude Code as the universal interface

我曾认为 Claude Code + CLI 就是新 IDE，这依然在某种程度上成立。我认为每当要快速修改项目，打开它是很好的第一步。但根据项目重要性，或许需要比“vibe coding”更谨慎地看输出。

更通用的说法是，Claude Code 真的是连接你的电脑、数字世界、任何数字问题的通用接口。在许多情况下，你可以让它自己搞定。比如需要快速编辑视频，可以让它通过 ffmpeg 或类似工具搞定；想转录本地一堆音频或视频文件，可以让它做，可能建议用 Python 的 Whisper；想分析 CSV 数据，可能建议用 Python 或 JavaScript 可视化。当然，带网络访问——Reddit、GitHub、MCP——可能性无限。

它也非常适合在本地电脑上执行各种操作。例如如果存储空间不足，可以让它提供清理建议。它会查看本地文件夹，找出占用空间的东西，然后给清理建议——也许删除特别大的文件。以我为例，我有一些很大的 Final Cut Pro 文件应该清理，是 Claude Code 告诉我的。也许它会告诉你用 `docker system prune` 清理未用的 Docker 镜像和容器，或者提示清掉某些你没意识到还存在的缓存。现在不管你想在电脑上做什么，Claude Code 都是我的首选。

有趣的是，计算机最初是文本界面。某种意义上我们又回到了这种文本界面，你可以一次打开三四个标签，如我之前提到的。对我来说，这很令人兴奋，感觉像有了第二个大脑。由于它只是终端标签，你可以打开第三、第四、第五、第六个大脑。随着模型更强大，可以委托给它们的“思考”比例——不是重要的事，而是你不想做或觉得无聊、太琐碎的事——你都可以交给它们。正如我提到的，一个好例子是查看 GitHub Actions。谁愿意做那个？但事实证明，这些 agent 非常擅长这些无聊任务。

## Tip 32: It's all about choosing the right level of abstraction

如我之前提到的，有时停留在 vibe coding 水平是可以的。如果是一次性项目或非关键代码部分，不一定要关心每行代码。但其他时候，你需要更深入——查看文件结构和函数、具体代码行，甚至检查依赖。

![Vibe coding 光谱](assets/vibe-coding-spectrum.png)

关键不在于二元。有些人说 vibe coding 不好，因为你不知道在做什么，但有时完全没问题。而在其他时候，深入理解代码、运用软件工程技能、对特定错误日志或代码片段提问 Claude Code 是有帮助的。

这就像在探索一座巨大的冰山。如果想保持 vibe coding，你可以在顶部飞过，从远处看看。然后可以靠近一点，潜入模式，深入探索，由 Claude Code 引导。

## Tip 33: Audit your approved commands

我最近看到[这篇帖子](https://www.reddit.com/r/ClaudeAI/comments/1pgxckk/claude_cli_deleted_my_entire_home_directory_wiped/)，有人用 Claude Code 运行了 `rm -rf tests/ patches/ plan/ ~/`，把家目录清空了。容易被当作 vibe coder 的错误，但这种错谁都可能犯。所以定期审查已批准的命令很重要。为此我写了 **cc-safe**——一个 CLI，扫描你的 `.claude/settings.json` 里的高风险批准命令。

它能检测诸如：
- `sudo`、`rm -rf`、`Bash`、`chmod 777`、`curl | sh`
- `git reset --hard`、`npm publish`、`docker run --privileged`
- 还有更多——具备容器识别能力，会跳过 `docker exec` 命令

它会递归扫描所有子目录，因此你可以指向 projects 目录一次性检查全部。可手动运行或让 Claude Code 帮你跑：

```bash
npm install -g cc-safe
cc-safe ~/projects
```

或者直接用 npx 运行：

```bash
npx cc-safe .
```

GitHub: [cc-safe](https://github.com/ykdojo/cc-safe)

## Tip 34: Write lots of tests (and use TDD)

随着你用 Claude Code 写更多代码，犯错的机会也会增多。PR 审查和可视化 Git 客户端能帮忙发现问题（如前所述），但写测试在代码库变大时至关重要。

你可以让 Claude Code 为它自己写的代码写测试。有人说 AI 不能测试自己的工作，但事实证明它可以——就像人脑一样。写测试时，你是用不同方式思考同样的问题，AI 也如此。

我发现 TDD（测试驱动开发）与 Claude Code 配合很好：

1. 先写测试
2. 确认它们失败
3. 提交测试
4. 再写使其通过的代码

我就是这样构建 [cc-safe](https://github.com/ykdojo/cc-safe) 的。先写失败的测试，在实现前提交，可以为代码制定明确的契约。Claude Code 有了具体目标，跑测试即可验证实现是否正确。

如果想更保险，自己审查测试，确保它们不是简单 return true 之类的蠢测试。

## Tip 35: Be braver in the unknown; iterative problem solving

自从更深入使用 Claude Code，我发现自己在未知领域变得更勇敢了。

例如，加入 [Daft](https://github.com/Eventual-Inc/Daft) 时，我注意到前端代码有问题。我并不精通 React，但仍决定深入。开始对代码库和问题提问。最终我能解决，因为我知道如何用 Claude Code 迭代解决问题。

最近有类似经历。我在为 Daft 用户写指南，遇到一些非常具体的问题：cloudpickle 在 Google Colab 和 Pydantic 一起用时出问题，以及 Python+少量 Rust 的问题——在 JupyterLab 中无法正确打印，而在终端正常。我从未用过 Rust。

我本可以只开个 issue，让其他工程师处理。但我想自己看看代码库。Claude Code 给出了初步方案，但不太好。我放慢了节奏。一位同事建议干脆禁用那部分，但我不想引入回归。能否找到更好的解决方案？

接下来是协作和迭代的过程。Claude Code 提出潜在根因和方案，我尝试。有些是死路，我们转向其他方向。期间我控制节奏。有时加快，让它探索不同解决空间或代码部分；有时放慢，问“这行究竟是什么意思？”控制抽象层级，控制速度。

最终我找到了相当优雅的方案。教训是：即便在未知领域，借助 Claude Code 你能做的比想象中多。

## Tip 36: Running bash commands and subagents in the background

当 Claude Code 中有长时间运行的 bash 命令时，可以按 Ctrl+B 将其移到后台运行。Claude Code 知道如何管理后台进程——它可以使用 BashOutput 工具稍后检查。

当你发现命令比预期耗时更长又想让 Claude 同时做别的事情时，这很有用。你可以让它用 Tip 17 的指数退避方式检查进度，或者让它在进程运行时处理别的任务。

Claude Code 也能在后台运行子 agent。如果你需要长时间研究或定期检查某些事情，不必让它在前台运行。只需让 Claude Code 在后台运行 agent 或任务，它会在你继续其它工作时处理好。

### 战略性使用子 agent

除了后台运行，子 agent 在拆分大型任务时很有用。例如有一个巨大代码库需要分析，你可以让子 agent 并行分析不同部分或用不同方式分析。让 Claude 生成多个子 agent 处理不同部分即可。

你可以通过直接提问来自定义子 agent：
- **数量** - 让 Claude 生成你想要的数量
- **后台 vs 前台** - 让它们后台跑，或按 Ctrl+B
- **使用的模型** - 询问使用 Opus、Sonnet 或 Haiku，视任务复杂度而定（子 agent 默认用 Sonnet）

## Tip 37: The era of personalized software is here

我们正进入个性化、定制软件的时代。自 AI 出现——总体上是 ChatGPT，尤其是 Claude Code——我注意到自己能创作更多软件，有时只是自用，有时是小项目。

如前文所述，我做了每天用的自定义转录工具与 Claude Code 对话；自定义了 Claude Code 本身；还用 Python 做了大量数据可视化和分析，比以往更快。

再举一例：[korotovsky/slack-mcp-server](https://github.com/korotovsky/slack-mcp-server)，一个近千星的热门 Slack MCP，设计为 Docker 容器运行。我在自己的 Docker 容器中顺畅使用它遇到了问题（Docker-in-Docker 复杂性）。与其苦战这个设置，我直接让 Claude Code 用 Slack 的 Node SDK 编写一个 CLI。效果很好。

这是激动人心的时代。无论你想做什么，都可以请 Claude Code 去做。如果足够小，一两小时就能做完。我甚至创建了一个[幻灯片模板](https://ykdojo.github.io/claude-code-tips/content/spectrum-slides.html)——单个 HTML 文件，包含 CSS 和 JavaScript，可以嵌入交互式、可持久的终端进程。

## Tip 38: Navigating and editing your input box

Claude Code 的输入框设计模拟常见终端/readline 快捷键，如果你习惯终端会觉得很自然。以下是一些实用快捷键：

**导航：**
- `Ctrl+A` - 跳到行首
- `Ctrl+E` - 跳到行尾
- `Option+Left/Right`（Mac）或 `Alt+Left/Right` - 按词后退/前进

**编辑：**
- `Ctrl+W` - 删除前一个词
- `Ctrl+U` - 删除光标到行首
- `Ctrl+K` - 删除光标到行尾
- `Ctrl+C` / `Ctrl+L` - 清除当前输入
- `Ctrl+G` - 在外部编辑器打开你的提示（贴长文本时有用，因为直接贴到终端可能很慢）

如果你熟悉 bash、zsh 等 shell，会感觉很顺手。

对于 `Ctrl+G`，编辑器由 `EDITOR` 环境变量决定。可以在 shell 配置（`~/.zshrc` 或 `~/.bashrc`）中设置：

```bash
export EDITOR=vim      # or nano, code, nvim, etc.
```

或者在 `~/.claude/settings.json` 中设置（需要重启）：

```json
{
  "env": {
    "EDITOR": "vim"
  }
}
```

**输入换行（多行输入）：**

最快的方法无需任何设置即可使用：输入 `\` 后按 Enter 创建换行。若要快捷键，运行 `/terminal-setup`。在 Mac Terminal.app，我用 Option+Enter。

**粘贴图片：**
- `Ctrl+V`（Mac/Linux）或 `Alt+V`（Windows）- 从剪贴板粘贴图片

注意：在 Mac 上是 `Ctrl+V`，不是 `Cmd+V`。

## Tip 39: Spend some time planning, but also prototype quickly

你需要花足够时间规划，让 Claude Code 知道要建什么、怎么建。这意味着尽早做出高层决策：用什么技术、项目结构如何、功能放在哪、文件要放哪里。尽早做出好决策很重要。

有时快速原型有帮助。通过快速做个简单原型，你可能就能判断“这个技术适合这个用途”或“另一个技术更好”。

例如，我最近尝试创建一个 diff 查看器。先试了 tmux + lazygit 的简单 bash 原型，再试了用 Ink 和 Node 自制 git viewer。我遇到不少问题，最后没有发布这些成果。但这个项目提醒了规划和原型的重要性。发现只要在写代码前稍微规划一下，就能更好引导它。即便在编码过程中仍需引导，但先让它计划一点很有帮助。

你可以按 Shift+Tab 切换到计划模式来做这件事。或者直接让 Claude Code 在写代码前先制定计划。

## Tip 40: Simplify overcomplicated code

我发现 Claude Code 有时会把事情复杂化，写太多代码。它会做你没要求的改动，似乎偏好写更多代码。如果你按本指南其他提示操作，代码可能能正常运行，但会难以维护、难检查。如果审查不够，可能会很痛苦。

所以有时你需要检查代码并让它简化。你可以自己修，也可以直接让它简化。可以问诸如“你为什么做这个改动？”或“为什么加了这一行？”之类的问题。

有人说如果只用 AI 写代码，你永远无法理解。但只有在你不问足够问题时才成立。如果确保理解每一件事，其实可以更快理解代码，因为你能向 AI 询问。尤其在处理大型项目时。

注意这对写作也适用。Claude Code 常常在最后一段总结前面段落，或最后一句总结前面句子，会很重复。有时有用，但多数时候需要让它删除或简化。

## Tip 41: Automation of automation

归根结底，这一切都是“自动化的自动化”。我的意思是，我发现这不仅能提升效率，还让过程更有趣。至少对我而言，自动化的自动化很有意思。

我起初用 ChatGPT，想自动化将它给的命令复制粘贴并在终端运行的过程。我通过构建 ChatGPT 插件 [Kaguya](https://github.com/ykdojo/kaguya) 自动化了整个流程。从那时起，我一直在追求更进一步的自动化。

如今，幸运的是我们无需构建类似工具，因为 Claude Code 等工具已经存在且非常好用。随着使用越来越多，我在想，如果可以自动化打字呢？于是我用 Claude Code 自己构建了语音转录应用，如前所述。

后来我开始想，我有时会重复自己。于是把那些内容放入 CLAUDE.md。再后来想，偶尔会反复运行同样命令，如何自动化？也许让 Claude Code 来做，或者放到 skills，甚至让它写个脚本，这样就不用重复同样的流程。

我认为这就是未来方向。每当你发现自己反复执行同样任务或命令，重复几次没关系，但如果一再重复，就该思考如何自动化整个流程。

## Tip 42: Share your knowledge and contribute where you can

这条提示有点不同。我发现通过尽可能多地学习，你能与周围人分享知识。或许通过类似这篇文章、甚至书籍、课程、视频。我最近还为 Daft 同事做了一个[内部分享](https://www.daft.ai/blog/how-we-use-ai-coding-agents)。非常有收获。

每当我分享技巧，也常能得到反馈。例如当我分享缩短系统提示词和工具描述的技巧（Tip 15）时，有人告诉我可以用 `--system-prompt` 参数作为替代。另一次，我分享了 slash 命令和 skills 的区别（Tip 25），从 Reddit 评论里学到新东西。

所以分享知识不仅是打造个人品牌或巩固学习，也是在过程中学习新东西。它不总是单向的。

关于贡献，我一直在给 Claude Code 仓库提 issue。我想，他们听就好，不听也没关系，没有期待。但在 2.0.67 版本中，我注意到他们采纳了多项建议：

- 修复了在 `/permissions` 删除权限规则后滚动位置重置的问题
- 为 `/permissions` 命令添加了搜索功能

团队对功能请求和 bug 反馈的响应之快令人惊讶。但这也合理，因为他们自己用 Claude Code 开发 Claude Code。

## Tip 43: Keep learning!

有几种有效方式持续学习 Claude Code：

**问 Claude Code 本身** - 如果你有关于 Claude Code 的问题，直接问它。Claude Code 有专门的子 agent 回答自身功能、斜杠命令、设置、hooks、MCP 等问题。

**查看发布说明** - 输入 `/release-notes` 查看当前版本的新功能。这是了解最新特性的最佳方式。

**向社区学习** - [r/ClaudeAI](https://www.reddit.com/r/ClaudeAI/) 子版是向其他用户学习、了解大家工作流的好地方。

**关注 Ado 的技巧** - Ado（[@adocomplete](https://x.com/adocomplete)）是 Anthropic 的 DevRel，在 2025 年 12 月发布了每日 Claude Code 技巧“Advent of Claude”系列。虽然该系列已结束，他仍在 X 上分享有用技巧。

- [Twitter/X: Advent of Claude 帖子](https://x.com/search?q=from%3Aadocomplete%20advent%20of%20claude&src=typed_query&f=live)
- [LinkedIn: Advent of Claude 帖子](https://www.linkedin.com/search/results/content/?fromMember=%5B%22ACoAAAFdD3IBYHwKSh6FsyGqOh1SpbrZ9ZHTjnI%22%5D&keywords=advent%20of%20claude&origin=FACETED_SEARCH&sid=zDV&sortBy=%22date_posted%22)

## Tip 44: Install the dx plugin

本仓库也是一个名为 `dx`（developer experience）的 Claude Code 插件。它将上述多个提示中的工具打包到一次安装中：

| Skill | Description |
|-------|-------------|
| `/dx:gha <url>` | 分析 GitHub Actions 失败（Tip 29） |
| `/dx:handoff` | 创建交接文档以保持上下文连续（Tip 8） |
| `/dx:clone` | 克隆对话以便分支（Tip 23） |
| `/dx:half-clone` | Half-clone 以减少上下文（Tip 23） |
| `/dx:reddit-fetch` | 通过 Gemini CLI 抓取 Reddit 内容（Tip 11） |
| `/dx:review-claudemd` | 审查对话以改进 CLAUDE.md 文件（Tip 30） |

**两条命令安装：**

```bash
claude plugin marketplace add ykdojo/claude-code-tips
claude plugin install dx@ykdojo
```

安装后，命令可通过 `/dx:clone`、`/dx:half-clone`、`/dx:handoff` 和 `/dx:gha` 使用。`reddit-fetch` skill 在你询问 Reddit URL 时自动调用。`review-claudemd` skill 会分析最近对话，为 CLAUDE.md 提出改进建议。关于克隆命令，参见[推荐的权限](#recommended-permission-for-clone-scripts)。

**推荐搭档：** [Playwright MCP](https://github.com/microsoft/playwright-mcp) 用于浏览器自动化——用 `claude mcp add -s user playwright npx @playwright/mcp@latest` 添加

## Tip 45: Quick setup script

如果想一次设置本仓库的多项推荐，有一个设置脚本能搞定：

```bash
bash <(curl -s https://raw.githubusercontent.com/ykdojo/claude-code-tips/main/scripts/setup.sh)
```

脚本会展示要配置的内容，并允许跳过任意项：

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

📺 **相关演讲**: [Claude Code Masterclass](https://youtu.be/9UdZhTnMrTA) - 31 个月智能化编码的经验与项目示例

📝 **故事**: [How I got a full-time job with Claude Code](content/how-i-got-a-job-with-claude-code.md)

📰 **新闻简报**: [Agentic Coding with Discipline and Skill](https://agenticcoding.substack.com/) - 将智能化编码的实践提升到新水平