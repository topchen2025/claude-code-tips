---
name: gha
description: 分析 GitHub Actions 失败并识别根本原因
argument-hint: <url>
---

调查此 GitHub Actions URL：$ARGUMENTS

使用 gh CLI 分析此 workflow 运行。你的调查应当：

1. **获取基础信息并识别实际失败**：
   - 哪个 workflow/job 失败，何时，在哪个 commit 上？
   - **关键**：仔细阅读完整日志，找到具体是什么导致了 exit code 1
   - 区分警告/非致命错误与实际失败
   - 查找诸如“failing:”、“fatal:”或脚本逻辑中决定何时 exit 1 的模式
   - 如果你同时看到“非致命”和“致命”错误，请聚焦于实际导致失败的原因

2. **检查是否易波动**：检查完全相同的失败 job 过去 10-20 次运行情况：
   - **重要**：如果一个 workflow 有多个 job，你必须检查该特定失败 job 的历史，而不仅仅是 workflow
   - 使用 `gh run list --workflow=<workflow-name>` 获取 run ID，然后 `gh run view <run-id> --json jobs` 检查特定 job 的状态
   - 这是一次性失败还是此特定 job 的重复模式？
   - 此 job 最近的成功率是多少？
   - 此 job 最近一次通过是什么时候？

3. **识别引入问题的 commit**（如果特定 job 存在失败模式）：
   - 找到此特定 job 首次失败和最后通过的运行
   - 确定引入失败的 commit
   - 验证方法：该 commit 之后所有运行此 job 是否都失败？之前是否都通过？
   - 如果验证成立，以高置信度报告此引入问题的 commit

4. **根本原因**：基于日志、历史记录以及任何引入问题的 commit，失败的可能原因是什么？
   - 聚焦于实际导致失败的原因（而不仅仅是看到的任何错误）
   - 将你的假设与日志和失败逻辑进行验证

5. **检查是否已有修复 PR**：搜索可能已处理此问题的开放 PR：
   - 使用 `gh pr list --state open --search "<keywords>"`，结合相关错误信息或文件名
   - 检查是否有开放 PR 修改了失败的文件/workflow
   - 如果存在修复 PR，在报告中注明，并跳过建议部分

撰写最终报告，包括：
- 失败摘要（具体是什么触发了 exit code 1）
- 易波动性评估（一次性 vs. 重复，成功率）
- 引入问题的 commit（如果已识别并验证）
- 根本原因分析（基于实际的失败触发因素）
- 现有修复 PR（如果找到——包含 PR 编号和链接）
- 建议（若已有修复 PR 则跳过）