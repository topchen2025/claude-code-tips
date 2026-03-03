# agentic 编码的光谱：从 vibe coding 到高质量软件工程

在过去两年半里，我在 AI 辅助编码上花费了超过十亿 token——自从我创建了第一个 agentic 编码工具 [Kaguya](https://github.com/ykdojo/kaguya) 以来——我逐渐意识到：vibe coding 和传统软件工程并不是对立面。你可以通过注入自己的软件工程知识和传统软件工程实践来增强 vibe coding。这样一来，vibe coding 会变成*有纪律的 agentic 编码*、*agentic 软件工程*，最终变成高质量的软件工程。换句话说，vibe coding 是一个光谱。

![agentic 编码的光谱](https://raw.githubusercontent.com/ykdojo/claude-code-tips/main/content/spectrum-chart.png)

## agentic/vibe coding 的四个层级

![agentic coding 的 50 种层次](https://raw.githubusercontent.com/ykdojo/claude-code-tips/main/content/50-shades-of-agentic-coding.jpeg)

### Level 1: Vibe coding

在最浅层的情况下，vibe coding 意味着你甚至忘了代码这个东西的存在。你只管让 AI 放飞自我，让它一直写、写、写。错误日志？根本不用看——直接丢给 AI。让 AI 处理一切。

版本控制？那是什么？安全性？是啊，我晚上会锁门。

这个层级非常适合一次性项目、快速原型、用完即弃的脚本。但别指望它长期可维护。

### Level 2: 有纪律的 agentic 编码

在这个层级，你开始引入一些结构：

- **Version control**：你在使用 Git 和 GitHub 或类似工具
- **文件级理解**：你大致知道每个文件做什么，以及它们如何相互交互
- **基础安全防护**
- **Testing**：你会确保应用在大多数情况下确实可用

### Level 3: Agentic 软件工程

从这里开始，它逐渐具备生产可用性。

- **Tests**：可以是 AI 写的，也可以是人写的，但你会验证它们是否真的在测试应该测试的内容。你不一定逐行阅读，但会确保它们看起来没问题。
- **Pre-commit hooks**：这能解决很多我有时看到人们用 Claude Code hooks 来做的事——格式问题、linting、简单检查。
- **CI jobs**：用于持续测试
- **函数级理解**：你知道每个 class 和 function 的作用。你可能不会逐行阅读，但相比仅停留在文件级，你的理解更深入。
- **质量控制措施**：细致的手工测试、端到端测试、一次性 AI 代码审查、回归检查

### Level 4: 高质量软件工程

尽管这依然是 AI 辅助编码，但其质量已经与由资深工程师手写的代码难以区分——理想情况下速度还更快。

- **逐行理解**：在提交 PR 时，你可能会先以 draft 形式提交，并确保每一行都正确且有意义
- **自我反思循环**：通过提出诸如“你确定这样吗？这些不同方案呢？”之类的问题，你可以让模型对自身进行反思，以捕捉它自己的错误，并进行全局层面的权衡讨论。
- **AI 驱动的交互式代码审查**：不只是一次性的 AI review。你会先在本地拉取变更并确认能运行。然后你会就每个文件、function、class，甚至单独某一行去“采访”AI——需要多深入就多深入。
- **高级研究方法**：用于架构决策：
  - Postgres vs. Cassandra?
  - AWS vs. Google Cloud vs. Render?
  - 在 Mac 上运行本地转录模型的最佳方式？

  你会使用 deep research 或带有 web search 和 [Reddit fetch](https://github.com/ykdojo/claude-code-tips?tab=readme-ov-file#tip-11-use-gemini-cli-as-a-fallback-for-blocked-sites) 的 Claude Code 来做研究，但在必要时也会检查原始信息源。
- **AI 驱动的质量控制**：除了目前讨论的一切之外，如果你在开发 CLI，可能还会有类似[后台 Claude Code job 测试你的 CLI](https://github.com/ykdojo/claude-code-tips?tab=readme-ov-file#tip-9-complete-the-write-test-cycle-for-autonomous-tasks)；如果你在做 web app，可能会有[可访问浏览器的 AI 测试你的 UI](https://github.com/ykdojo/claude-code-tips?tab=readme-ov-file#creative-testing-strategies)。此外，你还可能会做多平台测试（Windows、Linux、Mac）、安全与性能检查等等。
- **加速学习**：你不只是让 AI 写代码，还会问“你为什么这样写？”或“这一行具体是什么意思？”本质上，你是在用 AI 加深自己的领域专长，而不是把一切都交给它。

## agentic 编码矩阵

总结一下，我创建了这个简单矩阵，作为上述四个层级的速记：

| Level | Code Quality | Speed | Testing | Code Reviews |
|-------|--------------|-------|---------|--------------|
| **Vibe Coding** | 低 | 闪电般 | 最低限度 | 无 |
| **Agentic Coding with Discipline** | 还可以 | 更快 | 充足 | 简短 |
| **Agentic Software Engineering** | 中等 | 快 | 全面 | 一次性 |
| **High-Quality Software Engineering** | 高 | 尚可 | 坚如磐石 | 交互式 |

## 保持灵活

归根结底，你应该掌握这四个层级，并且能灵活选择使用哪一个。

有时停留在 vibe coding 层级完全没问题——快速原型、一次性脚本、实验。有时你需要高质量软件工程——医疗软件、大规模系统、其他任务关键型软件。

你代码的第一版可能是“slop”——低质量的 AI 生成代码。没关系。在你发送 PR 或把 draft PR 标记为 ready 之前，你可以随着时间持续改进它。

## “slop”的真正问题

![关于 AI 辅助编码的 Midwit 梗图](https://raw.githubusercontent.com/ykdojo/claude-code-tips/main/content/midwit-meme.png)

人们抱怨 AI 生成的代码是 slop。但问题在于，它之所以是 slop，只是因为他们没有投入足够的工作。

花费更多 token 并不一定意味着你会产出更多 s**t。取决于你如何使用 AI，它也可能意味着更高质量的代码、更好的研究，以及对你自己工作的更深入理解。