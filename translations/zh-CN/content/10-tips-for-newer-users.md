# 给新用户的 10 条 Claude Code 使用技巧

我注意到最近越来越多的人第一次尝试 Claude Code 和 Cowork，所以我想给大家一些入门建议。

包含 45 条技巧的完整仓库： https://github.com/ykdojo/claude-code-tips

## 1. Terminal vs VS Code vs Desktop vs Cowork

终端版本通常是最先进的——Claude Code 最初就是从这里开始的，而且我认为它获得了最多的开发时间，因此功能通常也是最丰富的。有些人更喜欢 VS Code 扩展，不太技术化的用户可能更喜欢 Cowork。不过如果你能使用终端，我建议从那里开始。

## 2. 安装特定版本

对于终端版本，有 npm 选项和原生二进制选项。原生二进制运行良好，但我个人建议安装 2.1.19 而不是最新版本，因为较新的版本可能会有 bug（或者你也可以选择自己喜欢的版本）。你可以这样安装特定版本：

```bash
curl -fsSL https://claude.ai/install.sh | bash -s 2.1.19
```

在 Windows PowerShell 上：

```powershell
& ([scriptblock]::Create((irm https://claude.ai/install.ps1))) 2.1.19
```

你可以用 `claude --version` 验证安装结果。

## 3. 备份重要文件并使用版本控制

不要成为那个丢失整个主目录的人：[that person](https://www.reddit.com/r/ClaudeAI/comments/1pgxckk/claude_cli_deleted_my_entire_home_directory_wiped/)。在开始之前先备份所有重要内容，并在项目中使用版本控制（Git）。Claude Code 可能会犯错，而有了版本控制，一旦出问题你总能回滚。

## 4. 学会充分测试输出结果

Claude Code 和 AI 的一个特点是，它们可能会引入隐蔽的 bug。所以一定要充分测试输出结果，避免最终堆积大量 bug。你甚至可以让 Claude Code 测试它自己写的代码——例如让它编写测试。只要确保这些测试看起来是有效的即可。

## 5. 把重复指令写进 CLAUDE.md

如果你发现自己在一遍遍告诉 Claude Code 同样的事，就把这些指令写到 CLAUDE.md 文件里。你可以直接让 Claude Code 代你完成——项目目录下（`./CLAUDE.md`）用于项目特定指令，或全局位置（`~/.claude/CLAUDE.md`）用于在任何地方都适用的规则。它应该能找到正确的文件并帮你编辑。

如果 Claude Code 不能稳定遵循某条指令，另一个选项是设置 [hook](https://docs.anthropic.com/en/docs/claude-code/hooks)。例如，如果你希望它始终使用 `python3.12` 而不是 `python3`，你可以创建一个 hook，阻止它运行 `python3` 并提示它改用 `python3.12`。

## 6. 设置浏览器集成

如果你需要 Claude Code 与网页交互，我推荐这两个选项：

- **Playwright MCP** - 对大多数任务通常效果更好。安装命令：`claude mcp add -s user playwright npx @playwright/mcp@latest`
- **Claude in Chrome** - 使用 `/chrome` 切换。当你需要自己浏览器配置中的登录态时很有用。

如果你使用 Claude in Chrome，我建议把下面内容加入你的 CLAUDE.md 以提高可靠性：

```markdown
# Claude for Chrome

- Use `read_page` to get element refs from the accessibility tree
- Use `find` to locate elements by description
- Click/interact using `ref`, not coordinates
- NEVER take screenshots unless explicitly requested by the user
```

## 7. 学会快速审查输出结果

我个人为此使用 GitHub Desktop。它有很好的 diff 视图，能让你轻松看清 Claude Code 到底改了什么。你也可以让 Claude Code 创建一个 PR 草稿，先在那里审查，再转成正式 PR。

## 8. 研究、规划、执行、测试

1. **Research** - 先投入足够时间理解问题。建立领域知识，这样你才能有效引导 Claude Code。
2. **Plan** - 先让 Claude Code 在写代码前给出计划。你可以使用计划模式（`/plan` 或 Shift+Tab）。
3. **Execute** - 编写代码。
4. **Test** - 确保一切确实可用。

## 9. 保持每次对话简短

当你开启与 Claude Code 的新对话时，它的表现通常最好，因为没有前序上下文带来的额外复杂性。随着对话越来越长，上下文也会变长，性能往往会下降。

所以每个新主题都开启新对话，或者在性能开始下降时及时重开。与其进行一场很长的对话，不如进行多场短小且聚焦的对话。

## 10. 学会同时处理几个会话

当你熟悉 Claude Code 后，可以尝试在不同终端标签页里同时运行两到三个会话。我的方法叫“级联（cascade）”——每当我开始一个新任务，就在右侧开一个新标签页。然后我从左到右扫过去，按任务从旧到新处理。

我建议同时最多专注三到四个任务。超过这个数量就很难跟踪发生了什么。

## 11. （Bonus）并行分支工作时使用 Git worktrees

如果你在同一个项目里同时处理多件事，又不希望它们相互冲突，Git worktrees 是非常好的方式。你可以直接让 Claude Code 创建一个 git worktree 并在那里开始工作——不用担心具体语法。

核心思路是：你可以在不同目录中处理不同分支。

### 什么是 git worktrees？

git worktree 和其他 git 分支类似，但它会有一个专门分配给它的新目录。

所以如果你同时在 main 分支和 feature-branch-1 上工作，那么在没有 git worktrees 的情况下，你一次只能处理一个，因为项目文件夹一次只能切到一个分支。

而有了 git worktree，你可以在原始项目文件夹中继续处理 main 分支（或者任何其他分支），并且同时在一个新文件夹中处理 feature-branch-1。

![Git worktrees 图示：在独立目录中并行进行分支工作](https://raw.githubusercontent.com/ykdojo/claude-code-tips/main/assets/git-worktrees.png)

---

如果你想看更完整的 25 条技巧列表，欢迎查看我在 Reddit 的另一篇帖子： https://www.reddit.com/r/ClaudeAI/comments/1qgccgs/25_claude_code_tips_from_11_months_of_intense_use/