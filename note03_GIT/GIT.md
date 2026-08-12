**初始化本地仓库
在对应文件下打开 open git bash here
```
# 1. 初始化本地 Git 仓库
git init
# 2. 关联远程仓库（粘贴你刚才复制的地址）
git remote add origin https://github.com/你的用户名/Obsidian-Notes.git
# 3. (可选但强烈推荐) 创建 .gitignore 文件，忽略 Obsidian 的临时配置文件，避免多设备冲突
# 在文件夹里新建一个 .gitignore 文件，把下面内容复制进去保存
.obsidian/workspace.json
.obsidian/workspace-mobile.json
.trash/
.DS_Store
```
**首次提交与推送**
```
# 1. 初始化本地 Git 仓库
git init
# 2. 关联远程仓库（粘贴你刚才复制的地址）
git remote add origin https://github.com/你的用户名/Obsidian-Notes.git
# 3. (可选但强烈推荐) 创建 .gitignore 文件，忽略 Obsidian 的临时配置文件，避免多设备冲突
# 在文件夹里新建一个 .gitignore 文件，把下面内容复制进去保存
.obsidian/workspace.json
.obsidian/workspace-mobile.json
.trash/
.DS_Store
```
**首次提交与推送**
```
# 添加所有文件到暂存区
git add .
# 提交，-m 后面是提交说明
git commit -m "首次提交"
# 将本地仓库的主分支名改为 main（GitHub 默认）
git branch -M main
# 推送到 GitHub 远程仓库，-u 是建立关联
git push -u origin main
```
sigen上库令牌：z-e6scurzYxDAb65hTJ6

git 修改账号信息
git config --global user.name "你的名字"
git config --global user.email "你的邮箱@company.com"


### 查看**当前项目**的配置
git config user.name
git config user.email

# 强制和远程主分支同步
**获取远程最新信息**  
这会从远程仓库下载所有最新更新，但不会改动你本地的任何文件。
git fetch --all
**强制重置本地分支**  
这个命令会用远程分支的状态强制覆盖你当前本地分支的状态，**会永久丢弃所有未提交的本地修改和已提交但未推送的本地提交**
git reset --hard origin/<branch-name>
**清理未跟踪的文件**  
完成上面两步后，本地仓库的文件和提交历史已与远程一致。`git reset --hard` 不会删除你本地新建的、尚未被 Git 跟踪的文件。这些文件可能包括 IDE 配置、编译产物等。如果你需要清理它们，可以使用 `git clean`
git clean -fd
如果想连 `.gitignore` 中忽略的文件（如编译缓存）也一并删除，可以加上 `-x` 参数
git clean -fdx
