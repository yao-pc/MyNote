### 方案一：使用 `git revert`（最安全、最推荐）
**适用场景**：公共分支（如 `main`、`develop`），且其他同事已经拉取了你的代码。  
**原理**：不删除历史记录，而是**生成一个“反向提交”**，把之前的改动撤销掉。
1.  查看提交历史，找到你要撤回的那个 commit 的 hash（前几位即可）
git log --oneline
# 2. 撤销指定的提交（比如 hash 是 abc 1234）
git revert abc 1234
# 3. 此时会弹出编辑器让你写撤销说明，保存退出
# 4. 推送到远程仓库
git push origin <你的分支名>

**优点**：不会改变历史，不会导致队友的代码冲突，是团队协作的标准做法。

---

### 方案二：使用 `git reset` + 强制推送（危险）

**适用场景**：**你自己的个人分支**，或者确定**没有任何其他人**基于这个提交做过开发。  
**原理**：直接把本地指针退回到指定位置，然后强制覆盖远程。

bash

# 1. 将本地分支回退到指定提交（比如退回 2 个版本）
git reset --hard HEAD~2
# 或者回退到特定的 commit hash
git reset --hard abc 1234
# 2. 强制推送到远程（覆盖远程分支）
git push origin <你的分支名> --force
# 或者更温和的强制推送（如果远程有新提交会报错，更安全）
git push origin <你的分支名> --force-with-lease

**⚠️ 极度危险**：`--force` 会直接覆盖远程，如果期间别人有新的 Push，会被直接抹掉。请务必确认只有你一人在用这个分支。

---

### 方案三：修改刚刚 Push 的提交（补救）

**适用场景**：刚 Push 完，发现少改了一个文件，或者 commit message 写错了，不想新增一个“撤销记录”。

bash

# 1. 修改本地代码，或使用 git add 添加漏掉的文件
# 2. 修正上一次提交（ --amend 会覆盖上一次提交）
git commit --amend -m "新的提交信息"
# 3. 强制推送（因为本地 commit hash 变了）
git push origin <分支名> --force-with-lease

---

### 紧急建议（避坑指南）

1. **绝对不要**在 `main` 或 `master` 分支上使用 `git reset --force`。如果必须用，请先给团队发通知。
    
2. 如果你已经执行了 `git reset` 但还没 Push，想反悔找回被删掉的提交，可以使用 `git reflog` 找到之前的 hash 并恢复。
    
3. **终极保险**：如果你不确定后果，可以先备份当前分支：
    
    bash
    
    git checkout -b backup-branch  # 先切个备份分支再操作