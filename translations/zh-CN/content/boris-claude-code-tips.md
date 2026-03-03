# 由 Claude Code 创建者 Boris 总结的 10 条 Claude Code 技巧

Claude Code 的创建者 Boris Cherny 最近在 [X 上分享了 10 条技巧](https://x.com/bcherny/status/2017742741636321619)，这些技巧来自 Claude Code 团队。下面是我借助 Claude Code 和 Opus 4.5 整理的快速总结。

## 1. 并行做更多事

启动 3-5 个 git worktree，每个都运行各自独立的 Claude 会话。这是团队里提升生产力最显著的方法。有些人会设置 shell 别名（za、zb、zc），用一次按键就在 worktree 之间切换。

## 2. 每个复杂任务都先进入计划模式

把精力投入到计划阶段，这样 Claude 才能一次性完成实现。如果过程出现偏差，就切回计划模式重新规划，而不是硬着头皮继续推进。还有人会额外启动第二个 Claude，以 staff engineer 的视角来审查计划。

## 3. 投资你的 `CLAUDE.md`

每次纠正后，都告诉 Claude：“Update your CLAUDE.md so you don't make that mistake again.” Claude 在为自己编写规则方面出奇地擅长。持续迭代，直到 Claude 的出错率出现可衡量的下降。

## 4. 创建你自己的 skills，并提交到 git

如果某件事你每天会做不止一次，就把它做成一个 skill 或 slash command。团队里的例子包括：用于查找重复代码的 `/techdebt` 命令、把 Slack/GDrive/Asana/GitHub 同步成一个上下文 dump 的命令，以及用于编写 dbt models 的 analytics agents。

## 5. Claude 大多数 bug 都能自己修好

把 Slack 里的 bug 讨论串贴给 Claude，然后只说一句 “fix.”。或者说 “Go fix the failing CI tests.”。不要事无巨细地指导它怎么做。你也可以让 Claude 分析 docker logs 来排查分布式系统问题。

## 6. 提升你的 prompting 水平

挑战 Claude——比如说 “Grill me on these changes and don't make a PR until I pass your test.”。如果它给出的修复很一般，就说 “Knowing everything you know now, scrap this and implement the elegant solution.”。编写详细规格并减少歧义——越具体，输出越好。

## 7. 终端和环境配置

团队很喜欢 Ghostty。使用 `/statusline` 显示上下文使用情况和 git 分支。给终端标签页加颜色区分。使用语音输入——你的语速大约是打字的 3 倍（在 macOS 上连按两次 fn）。

## 8. 使用 subagents

当你希望 Claude 在某个问题上投入更多算力时，就说 “use subagents”。把任务卸载给 subagents，可以让你的主上下文窗口保持干净。你还可以通过 hook 把权限请求路由到 Opus 4.5，自动批准安全请求。

## 9. 用 Claude 做数据和分析

将 Claude 与 `bq` CLI（或任意 database CLI/MCP/API）结合，拉取并分析指标。Boris 说他已经 6 个多月一行 SQL 都没写过了。

## 10. 用 Claude 学习

在 `/config` 中启用 “Explanatory” 或 “Learning” 输出风格，让 Claude 解释其修改背后的原因。你还可以让 Claude 生成可视化 HTML 演示文稿、绘制代码库的 ASCII 图，或构建一个间隔重复学习 skill。

---

我对其中很多技巧都很有共鸣，所以我建议你至少试试其中几条。如果你还想看更多 Claude Code 技巧，我在这里有一个包含我自己 45 条技巧的仓库：[claude-code-tips](https://github.com/ykdojo/claude-code-tips)