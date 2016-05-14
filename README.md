#Git-常用命令总结

**GIT  Dec 21, 2015  mushroomgithub**

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

**git diff拓展**

查看工作区更改内容

`$ git diff index.txt`

**diff 命令查看到的是工作区内文件和缓存区文件的区别**
**如果index.txt 已经通过add命令添加的缓存区，则无法查看**
**如果查看工作区和版本库的区别，可以使用参数 HEAD 或指定版本号**

`$ git diff HEAD -- index.txt`

`$ git diff 287d9bd-- index.txt`

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

**删除本地分支**

`~$ git branch -d <name>` 

**强行删除分支**

`~$ git branch -D <name>`

**删除远程分支**

`git push origin :<name>`

`git branch -d <local branch name>`

###git checkout

**切换分支**

`~$ git checkout <name>` 

**创建+切换分支**

`~$ git checkout -b <name> `

**在本地创建和远程分支对应的分支，本地和远程分支的名称最好一致**

`~$ git checkout -b branch-name origin/branch-name `

**建立本地分支和远程分支的关联**

`~$ git branch --set-upstream branch-name origin/branch-name`

**在当前分支下建立与远程同样分支名的上游关系，比如dev**

`git push origin dev:dev`

或者：

`git push --set-upstream origin developer`

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

`~$ git tag <name> `

**创建新标签，指向commit-id**

`~$ git tag <name> commit—id `

**创建带有说明的标签，用-a指定标签名，-m指定说明文字**

`~$ git tag -a v0.1 -m "version 0.1 released" 3628164 `

** 可以删除一个本地标签 **

`~$ git tag -d <tagname> `

**可以删除一个远程标签。**

`~$ git push origin :refs/tags/<tagname> `

**可以推送一个本地标签**

`~$ git push origin <tagname> `

**可以推送全部未推送过的本地标签**

`~$ git push origin --tags`

###git show

**查看标签信息**

`~$ git show <tagname>`


##Oh My Zsh Plugin:常用git alias

| Alias                | Command                                                                                                                                   |
|:---------------------|:------------------------------------------------------------------------------------------------------------------------------------------|
| g                    | git                                                                                                                                     |
| ga                   | git add                                                                                                                                 |
| gaa                  | git add --all                                                                                                                           |
| gapa                 | git add --patch                                                                                                                         |
| gb                   | git branch                                                                                                                              |
| gba                  | git branch -a                                                                                                                           |
| gbda                 | git branch --merged \| command grep -vE "^(\*\|\s*master\s*$)" \| command xargs -n 1 git branch -d                                      |
| gbl                  | git blame -b -w                                                                                                                         |
| gbnm                 | git branch --no-merged                                                                                                                  |
| gbr                  | git branch --remote                                                                                                                     |
| gbs                  | git bisect                                                                                                                              |
| gbsb                 | git bisect bad                                                                                                                          |
| gbsg                 | git bisect good                                                                                                                         |
| gbsr                 | git bisect reset                                                                                                                        |
| gbss                 | git bisect start                                                                                                                        |
| gc                   | git commit -v                                                                                                                           |
| gc!                  | git commit -v --amend                                                                                                                   |
| gca                  | git commit -v -a
| gcam                 | git commit -a -m                                                                                                                         |
| gca!                 | git commit -v -a --amend                                                                                                                |
| gcan!                | git commit -v -a -s --no-edit --amend                                                                                                   |
| gcb                  | git checkout -b                                                                                                                         |
| gcf                  | git config --list                                                                                                                       |
| gcl                  | git clone --recursive                                                                                                                   |
| gclean               | git reset --hard && git clean -dfx                                                                                                      |
| gcm                  | git checkout master                                                                                                                     |
| gcmsg                | git commit -m                                                                                                                           |
| gco                  | git checkout                                                                                                                            |
| gcount               | git shortlog -sn                                                                                                                        |
| gcp                  | git cherry-pick                                                                                                                         |
| gcs                  | git commit -S                                                                                                                           |
| gd                   | git diff                                                                                                                                |
| gdca                 | git diff --cached                                                                                                                       |
| gdt                  | git diff-tree --no-commit-id --name-only -r                                                                                             |
| gdw                  | git diff --word-diff                                                                                                                    |
| gf                   | git fetch                                                                                                                               |
| gfa                  | git fetch --all --prune                                                                                                                 |
| gfo                  | git fetch origin                                                                                                                        |
| gg                   | git gui citool                                                                                                                          |
| gga                  | git gui citool --amend                                                                                                                  |
| ggpull               | ggl                                                                                                                                     |
| ggpur                | ggu                                                                                                                                     |
| ggpush               | ggp                                                                                                                                     |
| ggsup                | git branch --set-upstream-to = origin/$(current_branch)                                                                                 |
| gignore              | git update-index --assume-unchanged                                                                                                     |
| gignored             | git ls-files -v \| grep "^[[:lower:]]"                                                                                                  |
| git-svn-dcommit-push | git svn dcommit && git push github master:svntrunk                                                                                      |
| gk                   | \gitk --all --branches                                                                                                                  |
| gke                  | \gitk --all $(git log -g --pretty = format:%h)                                                                                          |
| gl                   | git pull                                                                                                                                |
| glg                  | git log --stat --color                                                                                                                  |
| glgg                 | git log --graph --color                                                                                                                 |
| glgga                | git log --graph --decorate --all                                                                                                        |
| glgm                 | git log --graph --max-count = 10                                                                                                        |
| glgp                 | git log --stat --color -p                                                                                                               |
| glo                  | git log --oneline --decorate --color                                                                                                    |
| glog                 | git log --oneline --decorate --color --graph                                                                                            |
| glol                 | git log --graph --pretty = format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit       |
| glola                | git log --graph --pretty = format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --all |
| glp                  | _git_log_prettily                                                                                                                       |
| gm                   | git merge                                                                                                                               |
| gmom                 | git merge origin/master                                                                                                                 |
| gmt                  | git mergetool --no-prompt                                                                                                               |
| gmtvim               | git mergetool --no-prompt --tool = vimdiff                                                                                              |
| gmum                 | git merge upstream/master                                                                                                               |
| gp                   | git push                                                                                                                                |
| gpd                  | git push --dry-run                                                                                                                      |
| gpoat                | git push origin --all && git push origin --tags                                                                                         |
| gpu                  | git push upstream                                                                                                                       |
| gpv                  | git push -v                                                                                                                             |
| gr                   | git remote                                                                                                                              |
| gra                  | git remote add                                                                                                                          |
| grb                  | git rebase                                                                                                                              |
| grba                 | git rebase --abort                                                                                                                      |
| grbc                 | git rebase --continue                                                                                                                   |
| grbi                 | git rebase -i                                                                                                                           |
| grbm                 | git rebase master                                                                                                                       |
| grbs                 | git rebase --skip                                                                                                                       |
| grh                  | git reset HEAD                                                                                                                          |
| grhh                 | git reset HEAD --hard                                                                                                                   |
| grmv                 | git remote rename                                                                                                                       |
| grrm                 | git remote remove                                                                                                                       |
| grset                | git remote set-url                                                                                                                      |
| grt                  | cd $(git rev-parse --show-toplevel \|\| echo ".")                                                                                       |
| gru                  | git reset --                                                                                                                            |
| grup                 | git remote update                                                                                                                       |
| grv                  | git remote -v                                                                                                                           |
| gsb                  | git status -sb                                                                                                                          |
| gsd                  | git svn dcommit                                                                                                                         |
| gsi                  | git submodule init                                                                                                                      |
| gsps                 | git show --pretty = short --show-signature                                                                                              |
| gsr                  | git svn rebase                                                                                                                          |
| gss                  | git status -s                                                                                                                           |
| gst                  | git status                                                                                                                              |
| gsta                 | git stash                                                                                                                               |
| gstaa                | git stash apply                                                                                                                         |
| gstd                 | git stash drop                                                                                                                          |
| gstl                 | git stash list                                                                                                                          |
| gstp                 | git stash pop                                                                                                                           |
| gsts                 | git stash show --text                                                                                                                   |
| gsu                  | git submodule update                                                                                                                    |
| gts                  | git tag -s                                                                                                                              |
| gunignore            | git update-index --no-assume-unchanged                                                                                                  |
| gunwip               | git log -n 1 \| grep -q -c "\-\-wip\-\-" && git reset HEAD~1                                                                            |
| gup                  | git pull --rebase                                                                                                                       |
| gupv                 | git pull --rebase -v                                                                                                                    |
| gvt                  | git verify-tag                                                                                                                          |
| gwch                 | git whatchanged -p --abbrev-commit --pretty = medium                                                                                    |
| gwip                 | git add -A; git rm $(git ls-files --deleted) 2> /dev/null; git commit -m "--wip--"                                                      |

## Deprecated Aliases

These are aliases that have been removed, renamed, or otherwise modified in a way that may, or may not, receive further support.

| Alias  |                                Command                                             |                                             Modification                                            |
| :----- | :----------------------------------------------------------------------------------| --------------------------------------------------------------------------------------------------- |
| gap    | git add --patch                                                                    | new alias `gapa`                                                                                    |
| gcl    | git config --list                                                                  | new alias `gcf`                                                                                     |
| gdc    | git diff --cached                                                                  | new alias `gdca`                                                                                    |
| gdt    | git difftool                                                                       | no replacement                                                                                      |
| ggpull | git pull origin $(current_branch)                                                  | new alias `ggl` (`ggpull` still exists for now though)                                              |
| ggpur  | git pull --rebase origin $(current_branch)                                         | new alias `ggu` (`ggpur` still exists for now though)                                               |
| ggpush | git push origin $(current_branch)                                                  | new alias `ggp` (`ggpush` still exists for now though)                                              |
| gk     | gitk --all --branches                                                              | now aliased to `\gitk --all --branches`                                                             |
| glg    | git log --stat --max-count = 10                                                    | now aliased to `git log --stat --color`                                                             |
| glgg   | git log --graph --max-count = 10                                                   | now aliased to `git log --graph --color`                                                            |
| gwc    | git whatchanged -p --abbrev-commit --pretty = medium                               | new alias `gwch`                                                                                    |
| gwip   | git add -A; git ls-files --deleted -z \| xargs -r0 git rm; git commit -m "--wip--" | now aliased to `git add -A; git rm $(git ls-files --deleted) 2> /dev/null; git commit -m "--wip--"` |

## Functions

### Current

| Command            | Description                             |
|:-------------------|:----------------------------------------|
| current_branch     | Return the name of the current branch   |
| current_repository | Return the names of the current remotes |

### WiP

These features allow to pause a branch development and switch to another one (_"Work in Progress"_,  or wip). When you want to go back to work, just unwip it.

| Command          | Description                                     |
|:-----------------|:------------------------------------------------|
| work_in_progress | Echoes a warning if the current branch is a wip |
| gwip             | Commit wip branch                               |
| gunwip           | Uncommit wip branch                             |

##参考网址

[Git 常用命令总结](http://sunxiaoyang.github.io/2015/11/05/Git-%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4%E6%80%BB%E7%BB%93/?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)

[Git 超级简明手册](https://github.com/shendl1978/blog/wiki/Git%E8%B6%85%E7%BA%A7%E7%AE%80%E6%98%8E%E6%89%8B%E5%86%8C?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)

[IOS开发中的GIT流程](http://www.jianshu.com/p/87e34894a9f9?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)

[使用Git和Github进行协同开发流程](http://livoras.com/post/28?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)

[Git常用命令和Git Flow梳理](http://jonyfang.com/blog/2015/11/12/git_command_and_git_branching_model/?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)

[常用Git命令清单](http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)

[25个Git进阶技巧](https://linux.cn/article-5418-weibo.html?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)
[Oh My Zsh插件篇:git](http://swiftcafe.io/2015/11/29/omz-git/?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)

[Pro Git 2nd](http://git-scm.com/book/zh/v2?f=tt&hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)

[Git Flow](http://blog.csdn.net/uxyheaven/article/details/50373076?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)

[闯过这 54 关，点亮你的 Git 技能树](http://www.codingstyle.cn/topics/51?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)
