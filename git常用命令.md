# Git 常用命令

- git config --global user.name <用户名>
- git config --global user.email <邮箱>



- git reflog
- git log



- git reset --hard <版本号>
- git checkout <分支名称>



- git status
- git add .
- git commit -m "(message)"
- git rm --cached <文件名>



- git push
- git pull
- git pull <ssh链接> <分支名s>
- git push <别名> <分支>
- 分支A：git merge <分支名> 	
  - 分支A合并分支B，分支B不改变



- git remote -v 	查看当前所有远程地址别名
- git remote add <别名> <远程地址>



- git clone 远程地址
  - clone 会做如下操作。1、拉取代码。2、初始化本地仓库。3、创建别名

- which git 	查看git安装路径

------

### 配置文件

- IDEA 默认有 .gitignore 文件

- git.ignore

```
# Compiled class file
*.class

# Log file
*.log

# BlueJ files
*.ctxt

# Mobile Tools for Java (J2ME)
.mtj.tmp/

# Package Files #
*.jar
*.war
*.nar
*.ear
*.zip
*.tar.gz
*.rar

# virtual http://www.java.com/en/download/help/error_hotspot.xml 
hs_err_pid*
.classpath
.project
.settings
target
.idea
```

- .gitconfig

```
[user]
    name = Layne
    email = Layne@atguigu.com
[core]
excludesfile = C:/Users/asus/git.ignore
注意:这里要使用“正斜线(/)”，不要使用“反斜线(\)”
```

------

### Git 各命令详解

- 参考文章：https://segmentfault.com/a/1190000006185954

- 3 层指针：HEAD -> Branch -> Commit
- **checkout**：将 HEAD 从当前分支移向其他分支（HEAD -> Branch2 -> Commit2）。此时 Branch 仍指向 Commit
- **reset**：将 Branch 移向其他 Commit（HEAD -> Branch -> Commit2）。此时 Branch、Branch2 都指向 Commit2
