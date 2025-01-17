﻿Git is a distributed version control system.
Git is free software distributed under the GPL

git init	初始化代码库
git add [file]	添加文件到代码库
git commit -m "message" 上传文件
git reset --hard HEAD^ 回退到上一次修改，回退几次就加几个^
git reset --hard [cmmit NO] 回到指定的版本号
git reflog 查看以往的命令，以便确定要回到哪个版本
git status 查看代码库的状态
git checkout -- <file> 让文件回到最近一次的git commit或者git add时候的状态
git reset HEAD <file> 把暂存区的修改撤销掉
	当改乱了工作区的某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- <file>
	当不但改乱了工作区的文件内容，还添加到了缓存区的时候，用命令git reset HEAD <file>，回到最近一次的commit或者add的状态，然后再执行git checkout
	已经提交了修改的文件，回滚到对应的版本号
git rm 从项目中删除文件,删错了像恢复,git checkout -- <file>



/*******************连接远程库*************************/

创建SSH key
ssh-keygen -t rsa -C "youemail@example.com"
在github账户下面创建账户级别的公钥
	将.ssh目录下面的id_rsa.pub文件内容粘贴到Key文本框，Title随意
git remote add <origin> git@server-name:username/repo-name.git 关联一个远程库
git push u <origin> master

git checkout -b <branch> 创建分支+切换
git branch 查看分支
git branch <branch> 创建分支
git checkout <branch> 切换分支
git merge <branch> 将分支的修改合并到master上
git branch -d <branch> 删除不需要的分支
//因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在master分支上工作效果是一样的，但过程更安全。

解决分支冲突，

	修改冲突文件后在提交，如果遇到error:
	cannot do a partial commit during a merge.
	用 git commit -i <fileName> 或者 git commit -am "Message"

分支策略
在实际开发中，我们应该按照几个基本原则进行分支管理：
首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；
你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。

修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；
当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场。
git stash 保存现场
git stash list 查看保存现场列表
git stash pop 回复栈顶
如果多次保存了现场回复的时候就用
git stash apply stash@{0}

开发一个新feature，最好新建一个分支；
如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。

git remote 查看远程库信息 git remote -v 显示详细的信息

git push origin master 推送分支

多人协作的工作模式通常是这样：
0.首先，可以试图用git push origin branch-name推送自己的修改；
1.如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；
2.如果合并有冲突，则解决冲突，并在本地提交；
3.没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！
4.如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream branch-name origin/branch-name。
这就是多人协作的工作模式，一旦熟悉了，就非常简单。


I believe this occurs when you are trying to checkout a remote branch that your local git repo is not aware of yet. Try:
git remote show origin 
If the remote branch you want to checkout is under “New remote branches” and not “Tracked remote branches” then you need to fetch them first:
git remote update 
git fetch 
Now it should work:
git checkout -b local-name origin/remote-name


git 标签(版本管理的快照)
创建标签的步骤
1.切换到需要打标签的分支上
2.git tag <name>
可以通过git tag指令查看所有标签
默认的标签是打在最新的commit上的，如果想打再之前的commit上就需要找到历史的commit id
通过git log --pretty=oneline --abbrev-commit指令查看
git tag <name> <commit id>
git show <tagName>
git tag -a <name> -m "Message" 带说明的标签
git tag -s <name> -m "Message" 带签名的标签
git tag -d <name>删除标签
git push origin <tagName>
git push origin --tags 推送本地标签到远程
git push origin :refs/tages/<tagName> 删除远程的标签