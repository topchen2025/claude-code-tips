# 主动式编码的光谱：从 vibe coding 到高质量软件工程

在过去两年半里，我在 AI 辅助编码上花费了超过十亿个 token —— 自从我创建了第一个主动式编码工具 [Kaguya](https://github.com/ykdojo/kaguya) 以来 —— 我逐渐意识到：vibe coding 和传统软件工程并非对立。你可以通过注入软件工程知识与传统实践来提升 vibe coding。这样一来，vibe coding 会变成*有纪律的主动式编码*、*主动式软件工程*，最终就是高质量的软件工程。换句话说，vibe coding 是一个光谱。

![Spectrum of agentic coding](https://raw.githubusercontent.com/ykdojo/claude-code-tips/main/content/spectrum-chart.png)

## 主动式/vibe coding 的四个层级

![50 shades of agentic coding](https://raw.githubusercontent.com/ykdojo/claude-code-tips/main/content/50-shades-of-agentic-coding.jpeg)

### Level 1: Vibe coding

在最浅的层面上，vibe coding 意味着你甚至忘记代码的存在。你只管让 AI 自由发挥、写啊写。错误日志？压根不用看——直接扔给 AI。让 AI 接管一切。

版本控制？那是什么？安全？是的，我晚上会锁门。

这一层级非常适合一次性项目、快速原型、一次性脚本。但别指望它能在长期维护上站得住脚。

### Level 2: 有纪律的主动式编码

在这一层级，你开始引入一些结构：

- **版本控制**：你在使用 Git 和 GitHub 或类似工具
- **文件级理解**：你大致知道每个文件的作用以及它们如何交互
- **基本的安全防护**
- **测试**：确保应用在大多数情况下确实能运行

### Level 3: 主动式软件工程

在这里，代码开始具备可投入生产的水准。

- **Tests**：无论由 AI 还是人类编写，但你会确认它们确实在测试该测的内容。你不一定读每行，但会确保它们看起来没问题。
- **Pre-commit hooks**：这些解决了我有时见到人们用 Claude Code hooks 处理的问题——格式化、lint、简单检查。
- **CI jobs**：用于持续测试
- **函数级理解**：你知道每个类和函数的作用。你可能不读每行，但理解比文件级更深入。
- **质量控制措施**：仔细的手动测试、端到端测试、一次性 AI 代码审查、回归检查

### Level 4: 高质量软件工程

即便仍然是 AI 辅助编码，其质量与一位资深工程师手写代码无异——理想情况下速度更快。

- **逐行理解**：当提交 PR 时，你可能先以草稿形式提交，并确保每行都正确且合理
- **自我反思循环**：通过提问“你确定吗？换种方法呢？”来让模型自我反思，捕捉错误并进行宏观权衡讨论。
- **AI 驱动的交互式代码审查**：不仅仅一次性 AI 审查。你会在本地拉取改动先确认可运行，然后采访 AI 关于每个文件、函数、类甚至每行——需要多深入就多深入。
- **高级研究方法**：用于架构决策：
  - Postgres vs. Cassandra？
  - AWS vs. Google Cloud vs. Render？
  - 在 Mac 上运行本地转录模型的最佳方式？

  你会使用 deep research 或带网页搜索和 [Reddit fetch](https://github.com/ykdojo/claude-code-tips?tab=readme-ov-file#tip-11-use-gemini-cli-as-a-fallback-for-blocked-sites) 的 Claude Code 来做调研，同时必要时也会核查来源。
- **AI 驱动的质量控制**：除了前面讨论的所有内容之外，如果你在创建 CLI，可能还会有类似[在后台运行 Claude Code 任务来测试 CLI](https://github.com/ykdojo/claude-code-tips?tab=readme-ov-file#tip-9-complete-the-write-test-cycle-for-autonomous-tasks)；如果在做 Web 应用，则可能有[具备浏览器访问能力的 AI 测试 UI](https://github.com/ykdojo/claude-code-tips?tab=readme-ov-file#creative-testing-strategies)。此外，你可能还有多平台测试（Windows、Linux、Mac）、安全与性能检查等。
- **加速学习**：不只是让 AI 写代码，还会问“为什么这样写？”或“这一行具体是什么意思？”本质上，你利用 AI 来加深领域专业知识，而不是让它包办一切。

## 主动式编码矩阵

为了总结，我创建了这个简单矩阵来概括这四个层级：

| Level | Code Quality | Speed | Testing | Code Reviews |
|-------|--------------|-------|---------|--------------|
| **Vibe Coding** | Low | Lightning | Bare minimum | None |
| **Agentic Coding with Discipline** | Okay | Faster | Sufficient | Brief |
| **Agentic Software Engineering** | Medium | Fast | Thorough | One-shot |
| **High-Quality Software Engineering** | High | Decent | Rock solid | Interactive |

## 保持灵活

归根结底，你应该掌握所有四个层级，并根据需要灵活使用。

有时保持在 vibe coding 层级就够了——快速原型、一次性脚本、实验。有时你需要高质量的软件工程——医疗软件、大规模系统、其他关键任务软件。

代码的第一版可能是“slop”——低质量的 AI 生成代码。没关系。你可以在提交 PR 或将草稿 PR 标记为 ready 前逐步改进。

## “Slop” 的真正问题

![Midwit meme about AI-assisted coding](https://raw.githubusercontent.com/ykdojo/claude-code-tips/main/content/midwit-meme.png)

人们抱怨 AI 生成的代码是 slop。但问题在于，它之所以是 slop，仅仅因为他们投入不够。

更多的 token 消耗不一定意味着会产出更多垃圾。取决于你如何使用 AI，它同样可能意味着更高的代码质量、更好的调研，以及对自己工作的更好理解。