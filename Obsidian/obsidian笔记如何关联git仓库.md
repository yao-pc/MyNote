1. **准备工作：创建远程仓库**
    
    - 在 GitHub 或 Gitee 上，创建一个新的**私有（Private）仓库**，名字自己定就行，比如 `Obsidian-Notes` [](https://itsfoss.com/git-with-obsidian/?ref=dailydev#step-12-pull-changes)[](https://blog.csdn.net/2301_79518550/article/details/147248298)。
        
    - 创建好后，复制下仓库的 HTTPS 地址（例如 `https://github.com/你的用户名/Obsidian-Notes.git`），后面会用到[](https://blog.csdn.net/2301_79518550/article/details/147248298)。
    - https://github.com/yao-pc/MyNote.git
        
2. **初始化本地仓库（关键一步）**
    
    - 打开你的 Obsidian 笔记库（Vault）所在的本地文件夹。
        
    - 在文件夹空白处，右键选择 `Git Bash Here` (Windows) 或打开终端 (Mac/Linux)[](https://blog.csdn.net/2301_79518550/article/details/147248298)[](https://blog.csdn.net/weixin_47711503/article/details/137544921)。
        
    - 依次执行以下命令，初始化仓库并和远程仓库关联：
        
        bash
        
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
        
        - **重点**：`.gitignore` 文件很重要，它可以让 Git 忽略 `workspace.json` 这类本地配置文件。不同电脑的窗口布局不一样，如果不忽略这个文件，每次同步都容易产生冲突[](https://itsfoss.com/git-with-obsidian/?ref=dailydev#step-12-pull-changes)[](https://www.cnblogs.com/lvwd/p/19658082#commentform)[](https://www.rongpm.com/column/obsidian-git-sync-3611.html)。
            
    - **首次提交与推送**：把现有笔记提交到 Git 本地仓库，并推送到 GitHub 远程仓库。
        
        bash
        
        # 添加所有文件到暂存区
        git add .
        # 提交，-m 后面是提交说明
        git commit -m "首次提交"
        # 将本地仓库的主分支名改为 main（GitHub 默认）
        git branch -M main
        # 推送到 GitHub 远程仓库，-u 是建立关联
        git push -u origin main
        
        - 在执行 `git push` 时，终端会提示你登录。**用户名填你的 GitHub 用户名，密码不是 GitHub 登录密码，而是一个叫“个人访问令牌（Personal Access Token，简称 PAT）”的凭证**[](https://blog.csdn.net/2301_79518550/article/details/147248298)。如果还没生成 PAT，可以在 GitHub 设置里的 `Developer settings` -> `Personal access tokens` 下生成一个，记得勾选 `repo` 权限。
            
3. **在 Obsidian 中配置 Git 插件**
    
    - 打开 Obsidian，进入 **设置 (Settings) -> 社区插件 (Community plugins)**，搜索并安装 **Git** 插件，然后启用它[](https://itsfoss.com/git-with-obsidian/?ref=dailydev#step-12-pull-changes)[](https://blog.csdn.net/2301_79518550/article/details/147248298)[](https://www.cnblogs.com/lvwd/p/19658082#commentform)。
        
    - 在 Git 插件的设置页面，你可以按需配置：
        
        - **自动同步间隔**：`Auto commit-and-sync interval (min)` 可以设置每隔多少分钟自动提交并推送一次[](https://www.cnblogs.com/lvwd/p/19658082#commentform)。
            
        - **启动时拉取**：开启 `Pull on startup`，这样每次打开 Obsidian 都会自动从云端拉取最新内容，避免冲突[](https://www.cnblogs.com/lvwd/p/19658082#commentform)[](https://www.rongpm.com/column/obsidian-git-sync-3611.html)。
            
4. **测试与验证**
    
    - 你可以试着新建一篇笔记，然后等几分钟（或点击左侧边栏的“源代码管理”图标，手动点击 Commit-and-sync 按钮）[](https://www.cnblogs.com/lvwd/p/19658082#commentform)。
        
    - 去 GitHub 网页上刷新看看，如果笔记出现了，就说明关联和同步成功了！