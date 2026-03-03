# 项目说明
- 写作：保持用户的语气，口语化，紧贴用户所说内容，不要凭空编造，但修正小的语法错误
- 在添加或重命名提示后，运行 `node scripts/generate-toc.js` 以更新目录
- `~/.claude/CLAUDE.md` 是指向此仓库中 `GLOBAL-CLAUDE.md` 的符号链接
- 提交插件（skills、plugin.json 等）的更改时，请在 `.claude-plugin/plugin.json` 和 `.claude-plugin/marketplace.json` 中同时增加补丁版本。对于非插件的更改（如系统提示补丁），不要提升版本。
- Git 标签/发布（例如 `v0.25.1`）与插件版本（例如 `0.14.9`）是分开的。Git 标签遵循仓库的发布进程。插件版本仅在 `plugin.json` 和 `marketplace.json` 中。