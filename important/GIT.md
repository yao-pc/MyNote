- git clone  
    
- 远端新建仓库，本地执行git init让git接管项目  
    
- 执行git remote add orign...  
    
- 工作区->git add->暂存区->git commit->本地仓库->git push->远程仓库  
    
- git add .所有修改 README.md具体名  
    
- git commit -m "提交说明" 本地仓会多一个版本记录  
    
- 第一次推git push -u origin main  
    
- u-upstream tracking relationship  
    
- 之后git push就行了  
    
- 团队项目，每次修改代码前执行git pull拉取最新代码并和本地合并  
    
- git restore .回退  
    
- git log查看修改记录  
    
- git branch查看有哪些分支  
    
- git switch -c "hhh"创建一个分支  
    
- git switch master切换主分支  
    
- git branch -M main把当前分支改为main  
    
- git remote add origin 远程仓，进行远程仓绑定  
    
- git reset --hard 0000回退特定分支，适用于没有提交到远端仓库，已提交就要强制推送，有搞丢代码风险，多人协作禁止使用  
    
- git revert <commit id>仅撤销本次修改，使用反向提交进行抵消，建议使用revert回退更改  
    
- git diff比较不同  
    
- git checkout -b newbranch  
    
- git status查看修改