# 给新手的 10 条 Claude Code 提示

我注意到最近越来越多的人第一次尝试使用 Claude Code 和 Cowork，所以我想给他们一些入门指引。

包含 45 条提示的完整仓库：https://github.com/ykdojo/claude-code-tips

## 1. Terminal vs VS Code vs Desktop vs Cowork

终端版本通常是最先进的——Claude Code 就是从这里起步的，我认为它获得了最多的开发时间，因此往往功能最丰富。有些人更喜欢 VS Code 扩展，技术水平较低的用户可能更喜欢 Cowork。但如果你能使用终端，我建议从那里开始。

## 2. 安装指定版本

对于终端版本，有 npm 选项和原生二进制选项。原生二进制运行良好，但我个人推荐安装 2.1.19 版本而非最新版本，因为新版本可能有 bug（或者选择你喜欢的版本）。你可以这样安装指定版本：

```bash
curl -fsSL https://claude.ai/install.sh | bash -s 2.1.19
```

在 Windows PowerShell 上：

```powershell
& ([scriptblock]::Create((irm https://claude.ai/install.ps1))) 2.1.19
```

你可以通过 `claude --version` 验证安装。

## 3. 备份重要文件并使用版本控制

不要当那个[把整个 home 目录搞丢的人](https://www.reddit.com/r/ClaudeAI/comments/1pgxckk/claude_cli_deleted_my_entire_home_directory_wiped/)。开始前备份所有重要内容，并为你的项目使用版本控制（Git）。Claude Code 可能会犯错，使用版本控制意味着一旦出问题你随时可以回滚。

## 4. 学会充分测试输出

Claude Code 和 AI 的问题在于它们可能引入隐蔽的 bug。所以务必充分测试输出，避免留下大量 bug。你甚至可以让 Claude Code 测试它自己的代码——例如让它编写测试。只要确保测试看起来有效即可。

## 5. 将重复的指令放到 CLAUDE.md 中

如果你发现自己一次又一次地对 Claude Code 说同样的事情，请把这些指令放进 CLAUDE.md 文件。你可以让 Claude Code 代劳——要么放在项目文件夹（`./CLAUDE.md`）中用于项目特定的指令，要么放在全局（`~/.claude/CLAUDE.md`）中用于通用指令。它应该能找到正确的文件并帮你编辑。

如果 Claude Code 没能持续遵循某条指令，另一个选择是设置一个[hook](https://docs.anthropic.com/en/docs/claude-code/hooks)。例如，如果你想让它总是用 `python3.12` 而不是 `python3`，你可以创建一个 hook 来阻止它运行 `python3`，并告诉它改用 `python3.12`。

## 6. 设置浏览器集成

如果你需要 Claude Code 与网页交互，我推荐以下两种方式：

- **Playwright MCP** —— 对大多数任务通常效果更好。安装命令：`claude mcp add -s user playwright npx @playwright/mcp@latest`
- **Claude in Chrome** —— 使用 `/chrome` 切换。当你需要从自己的浏览器配置中保持登录状态时很有用。

如果你使用 Claude in Chrome，建议在你的 CLAUDE.md 中添加以下内容以提高可靠性：

```markdown
# Claude for Chrome

- 使用 `read_page` 从无障碍树中获取元素引用
- 使用 `find` 按描述定位元素
- 使用 `ref` 点击/交互，而非坐标
- 除非用户明确要求，否则绝不要截图
```

## 7. 学会快速审查输出

我个人使用 GitHub Desktop。它有一个很好的 diff 视图，能轻松查看 Claude Code 具体修改了什么。你也可以让 Claude Code 创建一个草稿 PR，在那里审查后再变成正式 PR。

## 8. 调研、规划、执行、测试

1. **调研** —— 先花足够时间理解问题。积累领域知识，这样你才能有效引导 Claude Code。
2. **规划** —— 在写代码前，让 Claude Code 提出一个计划。你可以使用 plan 模式（`/plan` 或 Shift+Tab）。
3. **执行** —— 编写代码。
4. **测试** —— 确保一切确实能正常工作。

## 9. 保持每次对话简短

当你与 Claude Code 开始新对话时，它的表现最佳，因为没有之前上下文带来的复杂性。随着对话变长，上下文变长，性能往往会下降。

所以每个新主题都开启一个新对话，或者一旦性能开始下降就重开。相比一个冗长的对话，多个简短而聚焦的对话更好。

## 10. 学会同时处理多个会话

一旦你对 Claude Code 感到熟练，试着在不同终端标签页中同时运行两到三个会话。我的方法叫做“级联”——每当我开始新任务，就在右边打开一个新标签。然后我从左到右轮询，从最旧的任务到最新的任务。

建议最多同时关注三到四个任务。超过这个数量就很难跟踪发生了什么。

## 11.（额外）为并行分支工作使用 Git worktree

如果你在同一个项目中同时处理多个事项，又不希望它们互相冲突，Git worktree 是个好方法。你可以直接让 Claude Code 创建一个 git worktree 并在其上开始工作——不用担心具体语法。

基本思路是你可以在不同目录中处理不同分支。

### 什么是 git worktree？

git worktree 就像任何其他 git 分支，但它有一个专门分配给它的新目录。

所以，如果你在同时处理 main 分支和 feature-branch-1，那么没有 git worktree 的情况下，你一次只能处理其中一个，因为你的项目文件夹一次只能设置为一个分支。

然而，通过 git worktree，你可以在原始项目文件夹中继续处理 main 分支（或任何其他分支），同时在新文件夹中处理 feature-branch-1。

![Git worktrees diagram showing parallel branch work in separate directories](https://raw.githubusercontent.com/ykdojo/claude-code-tips/main/assets/git-worktrees.png)

---

想查看更多 25 条提示，欢迎查看我的另一篇 Reddit 帖子：https://www.reddit.com/r/ClaudeAI/comments/1qgccgs/25_claude_code_tips_from_11_months_of_intense_use/