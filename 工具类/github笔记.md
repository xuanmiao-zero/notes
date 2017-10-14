# github笔记
## 配置
### 密钥配置
* ssh-keygen -t rsa -C "1297146548@qq.com" //生成密钥
* 复制id-rsa.pub中的内容-->访问 github.com/-->右上角头像-->Settings-->SSH and GPG keys-->New SSH key-->粘贴

* ssh -T git@github.com  //验证密钥是否绑定成功
    * 密钥绑定成功：现You have successfully authenticated, but GitHub does not provide shell access 

### 账户邮箱配置
1. git config --global user.name "xuanmiao-zero"

2. git config --global user.email "******@gmail.com" "1297146548@qq.com" //配置github用户名和Email
   
3. git config --global user.name 查看用户名
4. git config --global user.email 查看邮箱
5. git config --list                            //查看 git config配置
6. git config --global credential.helper store  //长期存储密码
7. git config --global color.ui true //git 显示颜色
8. git config --global alias.st status //为命令起别名
   1. git config --global alias.last 'log -1' //查看最后一个版本
   2. git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit" //查看历史版本信息升级版

### Are you Ready?
1. 在github上新建仓库
2. https://github.com/xuanmiao-zero/notes.git           //复制仓库地址
3. git clone https://github.com/xuanmiao-zero/notes.git //克隆仓库
4. Settings-->Options-->Delete this repository(最底部)  //删除仓库
5. git add .    //添加所有文件至暂存区
6. git status   //查看工作区和暂存区状态
7. git commit -a -m "版本说明"  //提交至版本库
8. git log                      //查看历史版本
    1. git log --pretty=oneline --abbrev-commit //简略历史版本
9. 对比
    1. git diff         //对比工作区和暂存区
    2. git diff --cached(--staged)     //对比暂存区和工作区
    3. git diff master //对比版本区和暂存区
4. 撤销
    1. git reset HEAD <fileName>    //撤销暂存区提交
    2. git checkout --<fileName>    //丢弃工作区的修改
    4. 彻底删除本地某个版本

        git reset --hard HEAD  //回退到版本

        git reflog expire --expire=now --all //清理本地版本库

        git gc --aggressive --prune=now //从object数据库中删除所有不可达的object
11. 删除
    1. git rm --cached <fileName> //只删除暂存区文件
    2. git rm -f fileName //删除工作区和暂存区文件
12. 恢复
    1. git checkout 历史记录编码(git log获取) 文件名(要恢复的文件名)   //指定的文件还原。
    2.  git reset --hard HEAD 版本号 //回滚到指定的版本号
        1. git reset --hard HEAD^   //回滚到上一版本，上上一个版本是HEAD^^，往前100个版本是HEAD~100
        2. git reflog //查看版本更改记录
13. 同步到远程仓库
    1. git remote -v  //查看远程仓库对应地址
    2. git remote add 创建仓库的名字 git push origin master （将版本库中的文件提交到远程仓库）
    3. git push origin master
    4. 解决冲突
        1. git fetch 
        2. git diff master origin/master //查看冲突
        3. git merge origin/master //人为解决冲突
        4. git pull //直接拉取会覆盖
14. 协作开发流程
    1. 给协作者开发权限

        进入项目-->settings-->collabrate-->添加协作者-->等待协作者确认
    2. 协作者：
    
        确认协作-->clone项目-->参与项目开发-->提交上传
    3. 遇到冲突
    
        git pull //覆盖本地代码
        
        git fetch //把远程仓库代码拉下来
        
    4. 查看哪里有冲突
        
        git diff master origin/master //绿色为远程库的内容，红色为本地文件内容
    5. 合并冲突(master --> 变为master[MERGEING])
    
        git merge origin/master
    6. 人为修改代码，解决冲突，重新提交，push之后，push(master[MERGEING]就变成master)

15. 给别人推送你的修改
    1. 找到想要协作开发的项目fork
    2. 把项目 git clone 到本地-->修改-->提交
    3. 点击pull request-->new pull requests-->create pull requests
    4. 等待回复
    5. 收到别人的pull request后-->点击files changed（查看修改内容）-->Merge pull request（回复）。
16. Git分支
    1. git branch 分支名 //新建分支
    2. git branch //查看分支
        1. git log --graph --pretty=oneline --abbrev-commit //查看图形化分支情况
    3. git checkout 分支名 //切换分支
    4. git checkout -b 分支名  //新建并切换到新建分支
    5. git merge 分支名 //fast forward模式合并分支 ，这种模式下(没有冲突的合并不会显示)。 建议每次合并后git commit一下方式
    6. git merge --no-ff -m "merge with no-ff" 要合并的分支 //普通模式合并分支 用git log --graph --pretty=oneline --abbrev-commit可查看到分支合并
    7. 删除
        1. git branch -d //删除分支
        2. git branch -D //删除没有合并的分支
    8. 分支开发流程
        1. git checkout -b 分支 //创建并切换到要干活的分支
        2. 进行开发
        3. 项目被干掉了
            1. git checkout master //切换回主分支
            2. git branch -D 分支 //删除分支
        4. 项目开发完成并通过
            1. git checkout master //切换回主分支
            2. git merge 分支 //合并分支
            3. 解决冲突
            4. git commit -m "合并开发的分支" 
17. 存储当前工作区状态和恢复
    1. git stash //保存当前状态
    2. git stash list //查看stash清单
    3. git stash apply //恢复但不删除stash
    4. git stash pop   //恢复并删除stash
    5. git stash apply stash@{0} //恢复指定stash
18. 解决bug流程
    1. git stash //保存当前工作区状态
    2. git checkout 要修改bug的分支 //切换到要修改bug的分支
    3. git checkout -b bug_branch //创建修改bug临时分支并切换过去
    4. 进行修复
    5. git git checkout 要修改bug的分支 //切换回要修改bug的分支
    6. git merge --no-ff -m "merged bug fix branch" bug_branch //合并修改bug临时分支
    7. git branch -d bug_branch //删除修改bug临时分支
    8. git checkout //切换回要干活的分支
    9. git stash list //查看状态保存清单
    10. git stash pop //恢复工作状态并删除stash
    11. 继续干活
20. 标签
    1. git tag v1.0 //创建标签   默认标签是打在最新提交的commit上
    2. git tag v0.9 6224937 //对指定commit创建标签
    3. git tag  //查看所有标签
    4. git show <tagname>  //查看标签信息
    5. git tag -a v0.1 -m "version 0.1 released" 3618164 //创建带有说明的标签，用-a指定标签名，-m指定说明文字
    6. git tag -d v0.1 //删除标签
    7. git push origin <tagname> //推送某个标签
    8. git push origin --tags 一次推送全部尚未推送的本地标签
    9. 删除远程标签:
        1. git tag -d v0.9 //首先先删除本地标签
        2. git push origin :refs/tags/v0.9 //然后，从远程删除 

### Git bash 快捷键
* 复制快捷键：ctr + insert     
* 粘贴快捷键：shift + insert