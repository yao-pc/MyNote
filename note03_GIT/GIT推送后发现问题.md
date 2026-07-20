### 场景一：刚推送完，自己立马发现了（别人还没拉取）
这是最理想的情况。你可以<font color="#ff0000">直接修改并覆盖远程分支</font>，操作相对“暴力”。

**1. 本地修改代码并提交**
git add .
git commit -m "fix: 紧急修复之前的错误"

**2. 强制覆盖远程分支（二选一）**

- **保守（推荐）**：使用 `--force-with-lease`，它会检查远程是否有别人提交的新内容，防止误覆盖别人的代码。
    git push --force-with-lease origin <你的分支名>
- **激进**：直接 `--force`（除非你是分支唯一使用者，否则不推荐）。
    git push --force origin <你的分支名>

# git 推送是提交了不想提交的文件如何撤回
### 方案一：文件刚推上去，且分支只有你在用（最常用）
如果你刚推送完，同事还没来得及拉取，用 ** `git rm` + 修正提交** 是最干净的做法，可以把那个文件的痕迹从提交历史中彻底删掉。

**1. 从 Git 索引中移除该文件（但保留本地文件）**
<font color="#ff0000">git rm --cached</font> <不想提交的文件名>
_（如果该文件已被 `.gitignore` 忽略，但之前被强制添加了，也需要执行此命令）_

**2. 修改刚才的提交（将移除操作合并到上一次提交中）**
git commit --amend
_（这会弹出编辑器，你只需保存退出即可，会生成一个新的 Commit ID）_

**3. 强制推送到远程覆盖**
git push --force-with-lease origin <你的分支名>

---

### 方案二：文件是编译产物/临时文件，且已经推了好几次

如果你不想修改历史（怕麻烦），只想让远程仓库**不再包含这个文件**，可以用反转提交（<font color="#ff0000">Revert</font>）。

**1. 删除远程文件并生成一个新提交**
git rm --cached <不想提交的文件名>
git commit -m "chore: 移除误提交的文件"

**2. 正常推送（不需要 --force）**
git push origin <你的分支名>

> ✅ **优点**：最安全，不影响其他同事。  
> ❌ **缺点**：该文件的“历史记录”仍然存在于 Git 对象中，如果文件很大，仓库体积不会变小（需要后续 GC 清理）。

---

### 方案三：文件包含密钥/密码（已推送）

**千万不要**用方案二，因为即使你删除了文件，密钥仍存于历史中，别人可以通过 Commit ID 随时查看。

**必须彻底重写历史（以删除 `application.properties` 为例）**：

**1. 使用官方工具 `git-filter-repo`（推荐）**

bash

# 安装（如果没有）
pip install git-filter-repo
# 强制从所有历史中删除该文件
git filter-repo --path application.properties --invert-paths

**2. 强制推送所有分支**
git push --force --all

> ⚠️ **致命提醒**：执行完这一步后，**立即去你的服务器/第三方平台更换该密钥**！因为 GitHub/GitLab 的缓存和 CDN 可能仍保留旧内容，密钥已视为完全泄露。

---

### 方案四：文件已合并到主分支（main/master）

**绝对不要强制推送主分支**（通常仓库也设置了保护禁止强推）。此时唯一正确做法是**仅删除当前文件，不去动历史**。
git rm --cached <不想提交的文件名>
git commit -m "docs: 移除误提交的无关文件"
git push origin main

_（这相当于承认“我提交错了，现在把它删掉”，是所有后续协作开发者的共识）_

---

### 关键提醒：如何防止下次再犯？

推送后后悔，不如推送前拦截。在项目根目录创建 `.gitignore` 并配置好，能根治这个问题：

bash

# 常见忽略项
*.class
*.log
.idea/
*.iml
node_modules/
.env
application-dev.yml  # 本地开发配置

如果文件**已经被 Git 追踪**，仅仅加入 `.gitignore` 是无效的，必须先执行：
git rm --cached <文件名>   # 然后重新提交