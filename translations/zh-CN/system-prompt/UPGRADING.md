# 升级到新的 Claude Code 版本

该项目会对 Claude Code CLI 打补丁，以减少系统提示词的 token 使用量。当 Claude Code 更新时，文本内容可能会变化，需要更新补丁。本指南将带你为新版本更新补丁。

**好消息：**`patch-cli.js` 对 `${...}` 变量模式使用正则匹配，因此补丁会自动适配压缩后变量名变化（例如 `${n3}` → `${XYZ}`）。只有当实际文本内容变化时，你才需要更新补丁。

**关键文件：**
- `patch-cli.js` - 应用补丁以减少提示词大小
- `backup-cli.sh` - 创建原始 CLI 备份（带哈希校验）
- `restore-cli.sh` - 从备份恢复 CLI
- `patches/*.find.txt` - 在 bundle 中要查找的文本
- `patches/*.replace.txt` - 替换文本（更短）
- `native-extract.js` - 从原生二进制中提取 cli.js（需要 `npm install node-lief`）
- `native-repack.js` - 将打好补丁的 cli.js 重新打包进原生二进制
- `patch-native.sh` - 一键进行原生二进制补丁

**支持的安装类型：**
- npm（Linux/macOS）- 直接给 `cli.js` 打补丁
- 原生二进制（Linux ELF、macOS Mach-O）- 提取、打补丁、重新打包

## 快速方法：让 Claude 在容器中处理

升级最快的方法是让 Claude Code 在容器内自主修复补丁。这样很安全，因为任何错误都只会隔离在容器中。

### 为什么使用容器？

1. **安全** - 打补丁出错不会破坏你的主 Claude 安装
2. **自主** - Claude 可以使用 `--dangerously-skip-permissions` 自由迭代
3. **易于恢复** - 如果出问题，可以重置容器
4. **完成后再复制** - 只把已验证的补丁移动到主机

**容器设置：**参见 `~/.claude/CLAUDE.md` 中的容器配置。下方示例使用 `safeclaw-upgrade`。

### 给外层 Claude（或人类）

如果你是在主机上运行并协助升级的 Claude Code，请不要在每条 docker 命令上都单独请求用户批准。应改为：
1. 运行步骤 1-3 来设置容器
2. 在步骤 4 启动容器内的 Claude
3. 使用 `tmux capture-pane` 监控直到完成
4. 然后把结果复制回去

容器内 Claude 会自主处理所有迭代——这正是核心目的。

### 步骤 1：在容器中更新 Claude

```bash
docker exec -u root safeclaw-upgrade npm install -g @anthropic-ai/claude-code@latest
docker exec safeclaw-upgrade claude --version  # 验证新版本
```

### 步骤 2：设置新版本目录

```bash
# 重要：先删除现有目录！docker cp 是合并而不是替换，
# 这会导致之前升级会话遗留的旧补丁文件不断累积。
docker exec safeclaw-upgrade rm -rf /home/sclaw/projects/2.X.OLD /home/sclaw/projects/2.X.NEW

# 将上一版本的补丁复制到容器
docker cp system-prompt/2.X.OLD safeclaw-upgrade:/home/sclaw/projects/

# 基于上一版本创建新版本目录
# 注意：需要 chown，因为从主机复制的文件会保留主机 UID
docker exec -u root safeclaw-upgrade bash -c "
  cp -r /home/sclaw/projects/2.X.OLD /home/sclaw/projects/2.X.NEW
  chown -R sclaw:sclaw /home/sclaw/projects/"

# 创建新 cli.js 的备份（重要：使用系统中新安装的 cli.js）
docker exec safeclaw-upgrade bash -c "
  cp /usr/local/lib/node_modules/@anthropic-ai/claude-code/cli.js \
     /home/sclaw/projects/2.X.NEW/cli.js.backup"

# 获取用于 patch-cli.js 的哈希
docker exec safeclaw-upgrade sha256sum /home/sclaw/projects/2.X.NEW/cli.js.backup
```

### 步骤 3：更新 patch-cli.js 中的版本和 npm 哈希

更新版本和 npm 哈希（原生哈希将在步骤 8 更新）：
```bash
# 在 patch-cli.js 中更新 EXPECTED_VERSION 和 npm 哈希
docker exec safeclaw-upgrade sed -i \
  -e "s/EXPECTED_VERSION = '2.X.OLD'/EXPECTED_VERSION = '2.X.NEW'/" \
  -e "s/npm: '[^']*'/npm: 'NEW_HASH_HERE'/" \
  /home/sclaw/projects/2.X.NEW/patch-cli.js
```

### 步骤 4：让 Claude 修复补丁

在 tmux 中启动 Claude 会话（便于监控进度）：

```bash
docker exec safeclaw-upgrade tmux new-session -d -s upgrade \
  'cd /home/sclaw/projects/2.X.NEW && claude --dangerously-skip-permissions'

# 等待启动，然后发送任务
sleep 4
docker exec safeclaw-upgrade tmux send-keys -t upgrade \
  'Read UPGRADING.md for context. Update all patches for the new version.
   The backup is cli.js.backup. Test with: node patch-cli.js cli.js
   Keep fixing until all patches apply successfully.' Enter
```

监控进度：
```bash
docker exec safeclaw-upgrade tmux capture-pane -t upgrade -p -S -50
```

Claude 将会：
1. 用 `node patch-cli.js cli.js` 测试补丁（使用本地备份）
2. 对失败的补丁，定位文本内容发生分歧的位置
3. 更新 `.find.txt` 和 `.replace.txt` 文件（变量名会通过正则自动适配）
4. 持续迭代直到所有补丁应用成功

### 步骤 5：测试真实安装

当补丁在本地可用后，将其应用到实际 Claude 安装，并**执行完整验证清单**（见本文末）：

```bash
# 将补丁应用到真实 cli.js（需要 root）
docker exec -u root safeclaw-upgrade node /home/sclaw/projects/2.X.NEW/patch-cli.js

# 测试 /context 是否可用
docker exec safeclaw-upgrade tmux new-session -d -s test 'claude --dangerously-skip-permissions'
sleep 4
docker exec safeclaw-upgrade tmux send-keys -t test '/context' Enter
sleep 3
docker exec safeclaw-upgrade tmux capture-pane -t test -p -S -30
```

**重要：**不要跳过验证！在将补丁复制到主机前，请运行最终验证清单中的所有测试。

### 步骤 6：将已验证补丁复制到主机

```bash
# 在主机上创建目录
mkdir -p system-prompt/2.X.NEW/patches

# 从容器复制（排除大的 cli.js.backup）
docker cp safeclaw-upgrade:/home/sclaw/projects/2.X.NEW/patch-cli.js system-prompt/2.X.NEW/
docker cp safeclaw-upgrade:/home/sclaw/projects/2.X.NEW/patches/. system-prompt/2.X.NEW/patches/

# 复制并更新 backup/restore 脚本
cp system-prompt/2.X.OLD/backup-cli.sh system-prompt/2.X.NEW/
cp system-prompt/2.X.OLD/restore-cli.sh system-prompt/2.X.NEW/

# 更新 backup-cli.sh 中的版本和哈希（与 patch-cli.js 使用相同哈希）
sed -i '' \
  -e 's/EXPECTED_VERSION="2.X.OLD"/EXPECTED_VERSION="2.X.NEW"/' \
  -e 's/EXPECTED_HASH="[^"]*"/EXPECTED_HASH="NEW_HASH_HERE"/' \
  system-prompt/2.X.NEW/backup-cli.sh

# 复制原生补丁脚本并更新 patch-native.sh 中的默认版本
cp system-prompt/2.X.OLD/patch-native.sh system-prompt/2.X.NEW/
cp system-prompt/2.X.OLD/native-extract.js system-prompt/2.X.NEW/
cp system-prompt/2.X.OLD/native-repack.js system-prompt/2.X.NEW/
sed -i '' 's/versions\/2.X.OLD/versions\/2.X.NEW/' system-prompt/2.X.NEW/patch-native.sh

# 可选：清理未使用的补丁文件（未在 patch-cli.js 中列出的补丁）
# 查找 patch-cli.js 中使用的补丁
grep "file: '" system-prompt/2.X.NEW/patch-cli.js | sed "s/.*file: '\([^']*\)'.*/\1/" | sort > /tmp/used.txt
# 查找所有补丁文件
ls system-prompt/2.X.NEW/patches/*.find.txt | xargs -n1 basename | sed 's/\.find\.txt$//' | sort > /tmp/all.txt
# 删除未使用补丁
for p in $(comm -23 /tmp/all.txt /tmp/used.txt); do
  rm -v "system-prompt/2.X.NEW/patches/${p}.find.txt" "system-prompt/2.X.NEW/patches/${p}.replace.txt"
done
```

### 步骤 7：应用到主机和其他容器

```bash
# 主机
npm update -g @anthropic-ai/claude-code
cd system-prompt/2.X.NEW && ./backup-cli.sh && node patch-cli.js

# 其他容器（先更新 Claude，再打补丁）
# 查看 ~/.claude/CLAUDE.md 获取当前容器列表
for container in eager_moser delfina; do
  docker exec -u root $container npm install -g @anthropic-ai/claude-code@latest
  docker cp system-prompt/2.X.NEW $container:/tmp/
  docker exec -u root $container cp /usr/local/lib/node_modules/@anthropic-ai/claude-code/cli.js \
    /usr/local/lib/node_modules/@anthropic-ai/claude-code/cli.js.backup
  docker exec -u root $container node /tmp/2.X.NEW/patch-cli.js
done
```

**注意：**上面的循环语法在某些 shell 中可能不可用。若失败，请分别对每个容器执行，或使用 `&&` 串联命令。

### 步骤 8：测试并更新原生哈希

`patch-cli.js` 有三个哈希：npm、native-linux、native-macos。在修复 npm 补丁后，测试原生构建并更新其哈希。

**警告：**运行原生安装脚本（`curl ... | bash`）会移除 npm 安装。请先测试 npm，或在原生测试后重新安装 npm。

```bash
# 在容器中安装原生版本（这会移除 npm 安装！）
docker exec safeclaw-upgrade bash -c 'curl -fsSL https://claude.ai/install.sh | bash'

# 提取 cli.js 并获取哈希
docker exec safeclaw-upgrade bash -c "cd /home/sclaw/projects/2.X.NEW && npm install node-lief"
docker exec safeclaw-upgrade node /home/sclaw/projects/2.X.NEW/native-extract.js \
  /home/sclaw/.local/share/claude/versions/2.X.NEW /tmp/native-cli.js
docker exec safeclaw-upgrade sha256sum /tmp/native-cli.js

# 在 patch-cli.js 中更新 native-linux 哈希，然后测试
docker exec safeclaw-upgrade bash -c "cp /tmp/native-cli.js /tmp/native-cli.js.backup && \
  node /home/sclaw/projects/2.X.NEW/patch-cli.js /tmp/native-cli.js"
```

对于 macOS 原生版本，在主机上重复：
```bash
curl -fsSL https://claude.ai/install.sh | bash
cd system-prompt/2.X.NEW && npm install node-lief
node native-extract.js ~/.local/share/claude/versions/2.X.NEW /tmp/mac-cli.js
shasum -a 256 /tmp/mac-cli.js
# 更新 native-macos 哈希，然后测试打补丁
```

---

# 原生二进制补丁

原生 Claude Code 二进制（通过 `curl -fsSL https://claude.ai/install.sh | bash` 安装）将 cli.js 嵌入在二进制内部。我们使用 `node-lief` 进行提取、打补丁和重新打包。

## 快速方法

```bash
cd system-prompt/2.X.NEW
npm install node-lief  # 一次性设置
./patch-native.sh      # 提取、打补丁、重新打包
```

## 手动步骤

```bash
# 1. 安装依赖
npm install node-lief

# 2. 从二进制中提取 cli.js
node native-extract.js ~/.local/share/claude/versions/2.1.17 /tmp/native-cli.js

# 3. 为补丁器创建备份
cp /tmp/native-cli.js /tmp/native-cli.js.backup

# 4. 应用补丁
node patch-cli.js /tmp/native-cli.js

# 5. 重新打包回二进制
node native-repack.js ~/.local/share/claude/versions/2.1.17.backup /tmp/native-cli.js ~/.local/share/claude/versions/2.1.17

# 6. 测试
~/.local/bin/claude --version
~/.local/bin/claude -p "Any broken content? Yes or no."
```

## 平台说明

| Platform | Binary Format | cli.js Location |
|----------|--------------|-----------------|
| Linux | ELF | Overlay (appended) |
| macOS | Mach-O | `__BUN/__bun` section |

macOS 二进制在重新打包后会自动通过 `codesign -s -f` 重新签名。

## 哈希差异

原生二进制的 cli.js 哈希与 npm 不同：
- npm 和原生的压缩方式不同
- macOS 原生在变量名中使用 `$`（例如 `f$`、`u$`）
- Linux 原生使用标准命名（例如 `t5`、`jD`）

`patch-cli.js` 接受所有已知哈希，并自动适配正则模式。

---

# 故障排查

## 当存在其他版本时，原生安装器会生成损坏的二进制

安装特定原生版本时（例如 `bash -s 2.1.32`），如果 `~/.local/share/claude/versions/` 中已存在其他版本，安装器会生成损坏的混合二进制。结果二进制中的系统提示词文本被修改，既不匹配请求版本也不匹配已有版本，导致大多数补丁失败（例如 2/63 而不是 63/63）。

**症状：**
- 哈希与该版本任何已知哈希都不匹配
- 大多数补丁显示 "not found" 或 "regex not found"
- 提取出的 cli.js 出现混杂：压缩后的提示词文本与原始文本混在一起

**解决方案：**安装前清空 versions 目录：
```bash
rm ~/.local/share/claude/versions/*
curl -fsSL https://claude.ai/install.sh | bash -s 2.1.32
```

**原因：**安装脚本总是先下载最新的 bootstrapper 二进制，然后运行 `claude install <version>`。当存在其他版本二进制时，bootstrapper 看起来会合并或转换现有二进制中的内容，而不是下载请求版本的干净构建。

## 上一会话遗留的旧备份

如果容器中有上一轮升级会话遗留文件，项目目录中的 `cli.js.backup` 可能与刚安装的系统 CLI 哈希不同。这会导致补丁在应用到真实安装时失败。

**症状：**
- 补丁可成功应用到本地测试文件，但在系统 CLI 上失败
- 在真实安装上运行 `patch-cli.js` 时出现哈希不匹配错误
- 容器 Claude 报告 "58/58 patches applied"，但系统显示 "44/58"

**解决方案：**始终将系统中新鲜备份复制到项目目录：
```bash
docker exec -u root safeclaw-upgrade cp \
  /usr/local/lib/node_modules/@anthropic-ai/claude-code/cli.js \
  /home/sclaw/projects/2.X.NEW/cli.js.backup
docker exec -u root safeclaw-upgrade chown sclaw:sclaw /home/sclaw/projects/2.X.NEW/cli.js.backup
```

然后更新 `patch-cli.js` 中的哈希以匹配，并让容器内 Claude 重新修复补丁。

## 正则匹配如何工作

`patch-cli.js` 会自动检测占位符模式并转换为正则捕获组：

| Placeholder | Matches | Use Case |
|-------------|---------|----------|
| `${varName}` | Template literal vars like `${n3}` | Tool references in prompts |
| `__NAME__` | Plain identifiers like `kY7` | Function names in code |

**示例：**
- `"Use ${T3} to read..."` 即使在新版本中 `${T3}` 变为 `${XYZ}` 也能匹配
- `function __FUNC__(A){...}` 可匹配任意函数名，如 `kY7`、`aBC` 等

对于 VAR 补丁，正则会捕获 bundle 中实际存在的内容，并在替换时复用。

**当补丁失败时**，原因是文本内容变了，不是变量名变了。使用下方二分查找技术定位文本分歧位置。

## 查找补丁文本分歧位置

当补丁显示 "not found in bundle" 时，定位不匹配点：

```javascript
// 运行：node -e '<paste this>'
const fs = require('fs');
const bundle = fs.readFileSync('/opt/homebrew/lib/node_modules/@anthropic-ai/claude-code/cli.js', 'utf8');
const patch = fs.readFileSync('patches/PATCHNAME.find.txt', 'utf8');

let lo = 10, hi = patch.length;
while (lo < hi) {
  const mid = Math.floor((lo + hi + 1) / 2);
  bundle.indexOf(patch.slice(0, mid)) !== -1 ? lo = mid : hi = mid - 1;
}
console.log('Match up to char:', lo, 'of', patch.length);
console.log('Patch:', JSON.stringify(patch.slice(lo-20, lo+30)));
const idx = bundle.indexOf(patch.slice(0, lo));
console.log('Bundle:', JSON.stringify(bundle.slice(idx + lo - 20, idx + lo + 30)));
```

## 无需 root 测试补丁

直接传入 cli.js 路径即可本地测试：
```bash
cp /path/to/cli.js.backup ./cli.js.backup
cp /path/to/cli.js.backup ./cli.js
node patch-cli.js ./cli.js
```

## 调试运行时崩溃

使用 bisect 模式查找哪个补丁导致问题：

```bash
node patch-cli.js --max=10  # 仅应用前 N 个补丁

# 用 tmux 测试
tmux new-session -d -s test 'claude -p "Say hello" 2>&1 > /tmp/claude-test.txt'
sleep 12 && cat /tmp/claude-test.txt
# 二分搜索：可用 = 尝试更多，崩溃 = 尝试更少
```

**症状：**
- 无输出的 "Execution error" = 变量指向不存在的函数
- `TypeError: Cannot read properties of undefined` = 同样原因
- Claude 立即卡住 = 同样原因
- 提示词中出现 `[object Object]` = 变量解析为错误类型（见下文）

**根因：**`*.replace.txt` 包含旧变量名。

## 检测损坏的系统提示词

**损坏迹象：**
- 本应是工具名的位置出现 `[object Object]`
- 文本中泄漏压缩后的 JS
- API 错误："text content blocks must contain non-whitespace text"

**注意：**某些问题在未打补丁的 Claude Code 中也存在（例如空项目符号）。请与未打补丁版本对比，以区分补丁 bug 和上游 bug。

## 空替换会破坏 /context

当要完全移除某个部分时，**不能**使用空的 `.replace.txt` 文件。API 要求文本块包含非空白内容。

**错误示例：**空的 `code-references.replace.txt` 会导致 `/context` 失败：
```
Error: 400 "text content blocks must contain non-whitespace text"
```

**正确做法：**在 `code-references.replace.txt` 中使用最小占位符，如 `# .`：
```
# .
```

这会显示为一个无害的孤立章节标题，但可满足 API 要求。

**为什么移除 code-references？**原文会提醒 Claude 按 `file_path:line_number` 格式引用代码。移除后可节省约 360 个字符。Claude 在没有这条指令时通常也能自然做到——该补丁只是移除了显式提醒。

## 基于函数的补丁

有些补丁会替换整个函数（如 `allowed-tools`）。对函数名和辅助名使用 `__NAME__` 占位符：

**步骤 1：通过其唯一字符串内容定位函数：**
```bash
# 查找唯一字符串的字节偏移
grep -b 'You can use the following tools without requiring user approval' \
  "$(which claude | xargs realpath | xargs dirname)/cli.js"
```

**步骤 2：提取该偏移附近上下文：**
```bash
# 使用 dd 获取周围字节（根据 grep 输出调整 skip 值）
dd if="$(which claude | xargs realpath | xargs dirname)/cli.js" \
  bs=1 skip=10482600 count=500 2>/dev/null
```

这样可以看到完整函数签名，包括新的函数名和辅助变量。

**步骤 3：更新 find 和 replace 文件**，填入新函数名及所有辅助变量。

**关键：`replace.txt` 必须使用新函数名！**如果 `allowed-tools.find.txt` 查找的是 `function n75(A)`，那么 `allowed-tools.replace.txt` 必须定义 `function n75(A){return""}`，而不是旧名字。使用旧名字（如 `S85`）会导致重复声明错误：
```
SyntaxError: Identifier 'S85' has already been declared
```
这是基于函数的补丁中最常见的错误。

## 使用容器内 Claude 排查补丁

Claude Code 可以帮助定位文本内容变化位置：

```bash
# 让 Claude 找出精确文本差异
docker exec container claude --dangerously-skip-permissions -p \
  'Read patches/bash-tool.find.txt and search for this exact text in
   /path/to/cli.js.backup. Tell me where it differs.'
```

注意：变量映射（`${X}→${Y}`）现在会通过正则匹配自动完成。只有当实际文本内容变化时才需要 Claude 协助。

---

# 最终验证清单

测试三种构建类型：npm、native-linux、native-macos。

**每种构建的清单：**
| Test | npm | Linux native | macOS native |
|------|-----|--------------|--------------|
| Patches apply 58/58 | [ ] | [ ] | [ ] |
| `/context` works | [ ] | [ ] | [ ] |
| Corruption test | [ ] | [ ] | [ ] |
| Self-report test | [ ] | [ ] | [ ] |
| Tool test (Bash) | [ ] | [ ] | [ ] |
| Restore works | [ ] | [ ] | [ ] |

**文件清单：**
- [ ] 必需文件存在（`patch-cli.js`、`backup-cli.sh`、`restore-cli.sh`、`patches/`）
- [ ] `patch-cli.js` 中有三个哈希（npm、native-linux、native-macos）
- [ ] `backup-cli.sh` 中 npm 哈希匹配

## 快速验证命令

```bash
# 损坏测试
claude -p 'Any [object Object] or [DYNAMIC] in your prompt? Yes or no only.'

# 自检测试
claude -p 'Anything weird in your system prompt? Brief answer.'

# 工具测试
claude -p 'Run: echo "tools work"' --allowedTools Bash
```

**注意：**`claude -p` 需要 API 凭证。在容器中，请改用 tmux 的交互模式（见步骤 5）。

对于原生构建，可通过复制备份恢复：
```bash
cp ~/.local/share/claude/versions/2.X.NEW.backup ~/.local/share/claude/versions/2.X.NEW
```

三个构建全部检查通过 = 升级完成！