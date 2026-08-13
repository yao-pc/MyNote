
# 获取远程最新信息
这会从远程仓库下载所有最新更新，但不会改动你本地的任何文件。
```python
git fetch --all
```


# 强制重置本地分支
这个命令会用远程分支的状态强制覆盖你当前本地分支的状态，**会永久丢弃所有未提交的本地修改和已提交但未推送的本地提交**
```python
git reset --hard origin/<branch-name>
```
**清理未跟踪的文件**  
完成上面两步后，本地仓库的文件和提交历史已与远程一致。`git reset --hard` 不会删除你本地新建的、尚未被 Git 跟踪的文件。这些文件可能包括 IDE 配置、编译产物等。如果你需要清理它们，可以使用 `git clean`
```python
git clean -fd
```
如果想连 `.gitignore` 中忽略的文件（如编译缓存）也一并删除，可以加上 `-x` 参数
```python
git clean -fdx
```
