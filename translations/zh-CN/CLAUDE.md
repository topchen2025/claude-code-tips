# 项目说明
- 写作：保持用户的语气，使用对话式风格，紧贴用户所说内容，不要编造信息，但可修正小的语法错误
- 在添加或重命名 tips 后，运行 `node scripts/generate-toc.js` 以更新目录
- `~/.claude/CLAUDE.md` 已符号链接到此仓库中的 `GLOBAL-CLAUDE.md`
- 提交插件更改（skills、plugin.json 等）时，请同时提升 `.claude-plugin/plugin.json` 和 `.claude-plugin/marketplace.json` 中的补丁版本。对于非插件更改（如系统提示词补丁）不要提升版本。
- Git 标签/发布（例如 `v0.25.1`）与插件版本（例如 `0.14.9`）是分开的。Git 标签遵循仓库发布进程。插件版本仅存在于 `plugin.json` 和 `marketplace.json` 中。