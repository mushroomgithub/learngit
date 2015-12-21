#Git-常用命令总结

**GIT  Nov 5, 2015  sunxiaoyang avatar sunxiaoyang**

接触 Git 有段时间了，一直想写一篇有关 Git 的学习文档但却无从写起。因为平时都仅简单地使用一些 Git 常用命令，也未深究其原理。

最近看了一些有关 Git 的学习资源，推荐给大家

1. try-git
2. 游戏式学习Git 
3. 廖雪峰 
4. Pro Git

仅将 Git 中常用命令做一汇总，以便查询。

#Git 基本命令

##git config

安装完 Git 后，首先要进行基本的配置。

**用户信息设置**

`~$ git config --global user.name "Your Name"`

`~$ git config --global user.email "email@example.com"`

**高亮Git**

`~$ git config --global color.ui true `

**Git 别名**

`~$ git config --global alias.co checkout`

`~$ git config --global alias.ci commit`

`~$ git config --global alias.br branch`

`~$ git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"`


**git init**

Git 仓库初始化。在工作区创建隐藏目录 .git，这是 Git 的版本库，其中重要的就是称为stage（或者叫index）的暂存区和当前分支。

**git add \<file\>**

将工作区文件修改提交至版本库的暂存区中，可以使用 . 或者 -A 参数。

**git commit**

将版本库的暂存区中所有内容提交至版本库的当前分支中，使用 -m 参数进行修改说明。

**git status**

查看工作区状态。

**git diff**

查看修改内容，显示的格式是Unix通用的diff格式。

##git log

**查看提交历史**

`~$ git log`

**查看合并情况**

`~$ git log --graph`

`~$ git log --graph --pretty=oneline --abbrev-commit`

**git reflog** 

查看命令历史。

**git checkout -- file**

撤销工作区的修改，无论是修改还是删除文件。

**git reset**

1. 撤销暂存区的修改：

`~$ git reset HEAD file`

2. 撤销本次提交（版本回退）：

**撤销至上一次提交**

`~$ git reset --hard HEAD^`

**撤销至上两次提交**

`~$ git reset --hard HEAD^^`

**撤销至上100次提交**

`~$ git reset --hard HEAD~100`

**撤销至某一次提交，可以回退到未来**

`~$ git reset --hard commit_id`

##分支管理

git branch

**查看分支**

`~$ git branch`

**创建分支**

`~$ git branch <name>` 

**删除分支**

`~$ git branch -d <name>` 

**强行删除分支**

`~$ git branch -D <name>`

###git checkout

**切换分支**

`~$ git checkout <name>` 

**创建+切换分支**

`~$ git checkout -b <name> `

**在本地创建和远程分支对应的分支，本地和远程分支的名称最好一致**

`~$ git checkout -b branch-name origin/branch-name `

**建立本地分支和远程分支的关联**

`~$ git branch --set-upstream branch-name origin/branch-name`

**git merge** 

Git 分支合并出现冲突时，必须首先解决冲突。解决冲突后，再提交，合并完成。

**合并某分支到当前分支**

`~$ git merge <name> `

**加上--no-ff参数就可以用普通模式合并，合并时生成一个新的commit.**
**合并后的历史有分支，能看出来曾经做过合并**

`~$ git merge --no-ff -m "merge with no-ff" <name>`

###git stash

用于正在期间，解决临时 bug 。<br>
可以先使用git stash 将工作区保存起来，然后新建 bug 分支修复 bug，然后合并至 dev 分支，删除 bug 分支，回到以前的分支后，用git stash pop回到工作现场。

**将工作区的内容临时保存起来**

`~$ git stash `

**查看临时保存的工作区**

`~$ git stash list`

**恢复工作区，但stash内容不删除**

`~$ git stash apply`

**删除stash内容**

`~$ git stash drop `

**恢复工作区并删除stash内容**

`~$ git stash pop`

##远程仓库

1. 首先查看用户主目录下隐藏目录.ssh中有无 id_rsa 和 id_rsa.pub 这两个文件。 
若没有则可以通过以下命令进行创建SSH Key ~$ ssh-keygen -t rsa -C “youremail@example.com” 由于此 key 不会用于军事，无需对key设置密码，一路NEXT，所有都是默认值就可以了。 
id_rsa : 私钥，不要告诉任何人。 
id_rsa.pub ： 公钥，可以告诉其他人。

2. 登陆 Github 账号，将 id_rsa.pub 的内容粘贴至 SSH Keys 处。

3. GitHub 支持多种协议，如果你使用 ssh 协议进行推送的话，只要 GitHub 知道你的公钥，是不需要输入密码的。

###git remote 

远程仓库管理，例如：

**查看远程库信息**

`~$ git remote`

**查看远程库详细信息**

`~$ git remote -v `

**将本地库与远程库建立链接**

`~$ git remote add origin URL`

**修改本地库的远程库链接**

`~$ git remote set-url origin URL`

远程库的名字就是origin，这是Git默认的叫法。

###git clone
从远端仓库克隆，例如：

`~$ git clone git@github.com:michaelliao/gitskills.git`

###git push 
将本地库推送至远端。例如：

`~$ git push origin master`

`~$ git push -u origin master`

第一次推送时需加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

###git pull 
抓取远程的新提交

###标签操作

发布一个版本时，我们通常先在版本库中打一个标签，这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。

Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（标签不能移动），所以，创建和删除标签都是瞬间完成的。

###git tag

**查看所有标签**

`~$ git tag `

**创建新标签，默认为HEAD**

`~$ glonta°本地库推送至远端。例如：

`~$ git push origin agter`

`~$ git push -u origin master`

第一次推送时需劌定标签名，-m指定说明文字**

`~$ git tag -a v0.1 -m "的远程新的master分支，还会把本地的master分支 签 **

`~$ git tag -d <tagname> `

**可以删除一个远程拉取时就可以简化命令。

###git pull 
抓取远程ç¸¸先在版本库中打一个标签，这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取pu了打标签时刻的版本。将来无论什么时候，取git show <tagname>`

##参考网址

[Git 常用命令总结](h史版本取出来。所以，标签也是版本库的一ä快照。
                        
                        Git的标签虽然是版本库的快照，但其edium=toutiao.io&utm_source=toutiao.io)

[Git 超级简明手å¼所以，创建和删除标签都是瞬间完成的。

BA%A7%E7%AE%80%E6%98%8E%E6%89%8B%E5%86%8C?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)

[IOS开发中的GIT。例如：

`~$ git push origin agter`

`~$ git push -u oro&gin master`

第一次推送时需劌定标签名，-m指å说明文字**

`~$ git tag -a v0.1 -m "的远程新的massrr分支，还会把本地的master分支 签 **
 
 `~$ git tGiag -d <tagname> `
 
 **可以删除一个远程拉取时就20¯以简化命令。

 ###git pull 
 抓取远程ç¸¸先在o.本库中打一个标签，这样，就唯一确定了打æ签时刻的版本。将来无论什么时候，取pu了æa标签时刻的版本。将来无论什么时候，取giturce=toutiao.io)

 [25个Git进阶技巧](https://linux.cn/article-5418-weibo.html?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)
 [Oh My Zsh插件篇:git](http://swiftcafe.utiao.io&utm_source=toutiao.io)

 [Git 超级简明手å¼æu»¥，创建和删除æM`H0`]
 `"`]]
`
`"`
