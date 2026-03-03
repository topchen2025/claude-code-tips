# 来自 Claude Code 创作者 Boris 的 10 条 Claude Code 提示总结

Claude Code 的创作者 Boris Cherny 最近在 [X 上分享了 10 条提示](https://x.com/bcherny/status/2017742741636321619)，来自 Claude Code 团队。这里是我在 Claude Code 和 Opus 4.5 的帮助下制作的快速总结。

## 1. 并行做更多事情

启动 3-5 个 git 工作树，每个运行自己的 Claude 会话。这是团队最大的生产力提升。有人设置了 shell 别名（za、zb、zc）来一键切换工作树。

## 2. 将每个复杂任务从计划模式开始

把精力投入到计划中，让 Claude 一次性完成实现。如果事情出问题，切回计划模式重新规划，而不是硬着头皮推进。甚至有人启动第二个 Claude 来作为资深工程师审阅计划。

## 3. 投入到你的 CLAUDE.md

每次纠正后，告诉 Claude：“Update your CLAUDE.md so you don't make that mistake again.” Claude 在为自己编写规则方面出奇地好。不断迭代，直到 Claude 的错误率显著下降。

## 4. 创建自己的技能并提交到 git

如果每天某件事做多次，把它变成一个技能或斜杠命令。团队中的例子：用于查找重复代码的 `/techdebt` 命令，将 Slack/GDrive/Asana/GitHub 同步到一个上下文转储的命令，以及编写 dbt 模型的分析代理。

## 5. Claude 会自己修复大多数 bug

将 Slack bug 讨论贴到 Claude，只需说“fix”。或者说“Go fix the failing CI tests.”不要对细节进行微观管理。你也可以让 Claude 查看 docker 日志来排查分布式系统。

## 6. 提升你的提示技巧

挑战 Claude——说“Grill me on these changes and don't make a PR until I pass your test.”在平庸的修复后说“Knowing everything you know now, scrap this and implement the elegant solution.”撰写详细的规格说明并减少歧义——越具体，输出越好。

## 7. 终端和环境配置

团队喜欢 Ghostty。使用 `/statusline` 显示上下文使用情况和 git 分支。对终端标签进行颜色编码。使用语音输入——你的语速是打字的 3 倍（在 macOS 上按两次 fn）。

## 8. 使用子代理

当你想让 Claude 投入更多算力来解决问题时，说“use subagents”。将任务卸载给子代理，以保持主上下文窗口整洁。你也可以通过一个钩子将权限请求路由到 Opus 4.5，自动批准安全的请求。

## 9. 将 Claude 用于数据和分析

将 Claude 与 `bq` CLI（或任何数据库 CLI/MCP/API）一起使用，提取并分析指标。Boris 说他已有 6+ 个月没写过一行 SQL 了。

## 10. 与 Claude 一起学习

在 `/config` 中启用 “Explanatory” 或 “Learning” 输出风格，让 Claude 解释其更改背后的原因。你还可以让 Claude 生成可视化的 HTML 演示文稿，绘制代码库的 ASCII 图，或构建一个间隔重复的学习技能。

---

这些提示里很多我都很认同，所以建议至少尝试其中几条。如果你想要更多 Claude Code 的提示，我这里有一个包含 45 条我自己总结的仓库：[claude-code-tips](https://github.com/ykdojo/claude-code-tips)