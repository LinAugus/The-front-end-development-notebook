## Git 常用操作学习总结
### 几个重要的名词
- Workspace：工作区
- Index / Stage：暂存区
- Repository：仓库区（或本地仓库）
- Remote：远程仓库

### Git 配置
设定 用户名／电子邮件地址
```
$ git config --global user.name "Your username"
$ git config --global user.email "Your@email.com"
```
输出彩色  
```
$ git config --global color.ui true
```  
设定命令别名
```
$ git config --global alias.<aliasname> <command>
```  
显示设定菜单  
```
$ git config --global list
```

### 基本操作
将当前目录初始化为 git 数据库  
```
$ git init
```  
查看状态  
```
$ git status
```  
查看日志  
```
$ git log
$ git log --pretty=oneline  (单行模式)
$ git log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short (漂亮的输出格式)
```
添加标签  
```
$ git tag [tagname]
```  
显示暂存区和工作区的差异  
```
$ git diff
```  
添加文件或目录到索引（暂存区）  
```
$ git add [filename]
```
将暂存区生成快照并提交  
```
$ git commit -m 'commit message' (提交到仓库)
$ git commit -v (提交时显示所有diff信息)
$ git commit -a (提交工作区自上次commit的变化)
$ git commit --amend -m 'commit message' (替代上一次提交)
```  
停止跟踪指定文件，但该文件保留在工作区
```
$ git rm --cached [filename]
```
删除工作区文件并放入暂存区
```
$ git rm [filename]
```


### 撤销
撤销本地修改
```
$ git checkout -- [filename]
$ git checkout -- .
```
重置暂存区和工作区，与上一次commit保持一致
```
$ git reset --hard ［filename］
```

### 分支
创建分支
```
$ git branch [name]
```  
切换分支  
```
$ git checkout [name]
```  
创建并切换分支  
```
$ git checkout -b [name]
```  
合并分支  
```
$ git merge [name]
```  
删除分支  
```
$ git branch -d [name]
```  
查看分支  
```
$ git branch
```

### 远程同步
添加远程库  
```
$ git remote add origin git@github.com:yourname/yourrepoistry.git
```
推送本地仓库到远程库
```
$ git push -u origin master (第一次push使用 -u，将本地的master分支与远程的master分支关联起来，以后推送或拉取就可以简化命令)
$ git push origin master 
```
克隆现有的远程数据库  
```
$ git clone [url]
```  
查看远程仓库
```
$ git remote -v
```
查看远程数据库分支的修改内容  
```
$ git fetch [repository]
```
拉取远程数据库的分支的变化，并于本地分支合并  
```
$ git pull [remote]
```