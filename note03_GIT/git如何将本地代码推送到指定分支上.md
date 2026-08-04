### 场景一：本地分支名 = 远程分支名（最常用）

如果你本地在 `feature` 分支上，想推到远程的 `feature` 分支（同名），最简单的命令：
确保你在正确的分支上
git checkout feature
# 推送并建立上下游关联（第一次推送时加 -u，以后可以直接 git push）
git push -u origin feature

- `-u` 是 `--set-upstream` 的缩写，它会记住关联关系，之后你再执行 `git push` 就能直接推送到这个远程分支，不用每次写全。
    

---

### 场景二：本地分支名 ≠ 远程分支名（推送到不同名的分支）

比如你在本地的 `dev` 分支上开发，但需要推送到远程的 `release` 分支：
语法：git push <远程仓库名> <本地分支名>:<远程分支名>
git push origin dev:release

这条命令的意思是：**把本地的 `dev` 分支内容，推送到远程仓库的 `release` 分支**。

如果想建立关联，方便下次直接用 `git push`：
git push -u origin dev:release

---

### 场景三：把本地任意代码推送到远程的 `main` 分支

如果你现在在某个功能分支 `feature/login` 上，但想直接推到 `main`（通常不推荐，但有时紧急修复需要）：
# 方法一：直接指定推送
git push origin feature/login:main
# 方法二：先切换到 main，合并后再推（更安全）
git checkout main
git pull origin main          # 拉取最新
git merge feature/login       # 合并功能分支
git push origin main          # 推送

⚠️ **强烈建议用方法二**，因为直接推送 `feature/login:main` 如果远程 `main` 有更新，会直接报错或被强制覆盖（如果用 `-f` 的话），非常危险。

---

### 附加技巧：强制推送（慎用！）

如果推送时提示“远程有更新，被拒绝”，而你**确定**要用自己的代码覆盖远程分支（比如修复错误提交），可以强制推送：
git push -f origin feature

⚠️ **警告**：强制推送会覆盖远程分支的历史，**绝对不要在 `main` 或 `release` 这类公共分支上使用**，否则队友拉取时会出大问题。只在个人开发分支上用。

---

### 常用命令速查表

|你的需求|命令|
|---|---|
|当前分支推送到同名的远程分支| `git push origin <分支名>` |
|第一次推送并建立关联| `git push -u origin <分支名>` |
|本地 A 分支推到远程 B 分支| `git push origin A:B` |
|查看本地配置了哪些远程仓库| `git remote -v` |
|查看本地分支与远程分支的关联| `git branch -vv` |

---

### 结合你之前的场景（合入 release）

假设你现在本地在 `main` 分支上做了修改，想直接推到远程 `release` 分支：
# 1. 确保修改已提交
git status          # 检查工作区是否干净
# 2. 推送本地 main 到远程 release
git push origin main:release

但如果远程 `release` 已经有人更新了，这个推送会被拒绝。正确的做法还是之前说的：**先切到 release 拉取最新，合并 main，再推送**。

git checkout release
git pull origin release
git merge main
git push origin release