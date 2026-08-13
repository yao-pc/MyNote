### 方案一：使用 `git revert`（最安全、最推荐）
**适用场景**：公共分支（如 `main`、`develop`），且其他同事已经拉取了你的代码。  
**原理**：不删除历史记录，而是**生成一个“反向提交”**，把之前的改动撤销掉。
1.  查看提交历史，找到你要撤回的那个 commit 的 hash（前几位即可）
```python
git log --oneline
```
2. 撤销指定的提交（比如 hash 是 abc 1234）
```python
# 撤销单个提交
git revert <commit-hash>
# 例如git revert a1b2c3d

# 撤销从 `abc` 到 `1234` 之间的所有提交
git revert abc..1234
#如果要合并成一个提交（避免产生大量 revert 记录）
git revert -n abc..1234
git commit -m "Revert multiple commits from abc to 1234"
# 想撤销两个特定的、不相邻的提交
git revert abc 1234
```
3. 此时会弹出编辑器让你写撤销说明，保存退出
4. 推送到远程仓库
```python
git push origin <你的分支名>
```

**优点**：不会改变历史，不会导致队友的代码冲突，是团队协作的标准做法。

---

### 方案二：使用 `git reset` + 强制推送（危险）

**适用场景**：**你自己的个人分支**，或者确定**没有任何其他人**基于这个提交做过开发。  
**原理**：直接把本地指针退回到指定位置，然后强制覆盖远程。
```python 将本地分支回退到指定提交（比如退回 2 个版本）
# 强制将当前分支回退到 2 次提交之前的状态
git reset --hard HEAD~2

# 或者回退到特定的 commit hash
git reset --hard abc 1234

# 2. 强制推送到远程（覆盖远程分支）
git push origin <你的分支名> --force

# 或者更温和的强制推送（如果远程有新提交会报错，更安全）
git push origin <你的分支名> --force-with-lease
```
**⚠️ 极度危险**：`--force` 会直接覆盖远程，如果期间别人有新的 Push，会被直接抹掉。请务必确认只有你一人在用这个分支。

---

### 方案三：修改刚刚 Push 的提交（补救）

**适用场景**：刚 Push 完，发现少改了一个文件，或者 commit message 写错了，不想新增一个“撤销记录”。

```python
# 1. 修改本地代码，或使用 git add 添加漏掉的文件
git add

# 2. 修正上一次提交（ --amend 会覆盖上一次提交）
git commit --amend -m "新的提交信息"

# 3. 强制推送（因为本地 commit hash 变了）
git push origin <分支名> --force-with-lease
```

---

### 紧急建议（避坑指南）

1. **绝对不要**在 `main` 或 `master` 分支上使用 `git reset --force`。如果必须用，请先给团队发通知。
    
2. 如果你已经执行了 `git reset` 但还没 Push，想反悔找回被删掉的提交，可以使用 `git reflog` 找到之前的 hash 并恢复。
    
3. **终极保险**：如果你不确定后果，可以先备份当前分支：
    git checkout -b backup-branch  # 先切个备份分支再操作

## 场景 1：刚刚 commit 了，但还没 push（最常见）

您刚执行了 `git commit`，发现多提交了一个文件，想从这次提交中移除它。

### 方法：修改上一次提交（推荐）


```python
# 1. 从暂存区移除该文件（但保留工作区的修改）
git reset --soft HEAD~1
# 2. 取消暂存这个文件
git reset HEAD <文件名>
# 3. 重新提交剩下的文件
git commit -m "提交信息"
```

**完整示例**：
```python
# 假设您错误地提交了 unwanted.txt
git add .                           # 暂存了所有文件
git commit -m "feat: 新功能"        # 提交了，包含 unwanted.txt
# 发现错误，开始修复
git reset --soft HEAD~1             # 撤回提交，但保留所有改动在暂存区
git reset HEAD unwanted.txt         # 把 unwanted.txt 从暂存区移除
git commit -m "feat: 新功能"        # 重新提交（不包含 unwanted.txt）
```
### 更简单的方法：直接修改提交（只移除一个文件）
```python
# 直接从上次提交中删除该文件
git rm --cached <文件名>
git commit --amend --no-edit
```
**注意**：如果该文件在提交前已经存在，这个操作会把它从提交中删除，但工作区仍保留。

---

## 场景 2：已经 push 到远程仓库

如果已经 `git push` 了，不能修改历史，需要**新增一个反向提交**。

### 方法 1：用 revert 撤销整个提交（不推荐单文件）

```python
# 撤销整个提交（会移除所有改动）
git revert HEAD
git push
```

### 方法 2：直接删除远程文件（推荐）
```python
# 1. 从版本库中删除该文件（但保留本地文件）
git rm --cached <文件名>
# 2. 提交删除操作
git commit -m "移除误提交的文件：<文件名>"
# 3. 推送到远程
git push
```

---

## 场景 3：刚 add 了，但还没 commit（还没提交）

如果只是 `git add` 了，还没 `git commit`：
```python
# 取消暂存这个文件
git reset HEAD <文件名>
# 或者取消暂存所有文件
git reset HEAD
```

---

## 场景 4：已经 commit 了，但想保留提交历史，只从提交中移除文件

### 方法：使用 `git restore`（Git 2.23+）
```python
# 从上次提交中移除该文件，但保留在工作区
git restore --staged <文件名>
git commit --amend --no-edit
```

---

## 场景 5：想彻底从所有历史中删除该文件（敏感信息）

如果误提交了密码、密钥等敏感文件：

```python
# 使用 BFG Repo-Cleaner 或 git filter-branch
# 示例：彻底删除文件（慎用！会重写历史）
git filter-branch --force --index-filter \
  "git rm --cached --ignore-unmatch <文件名>" \
  --prune-empty --tag-name-filter cat -- --all
```

**⚠️ 警告**：这会重写整个仓库历史，所有协作者都需要重新克隆。