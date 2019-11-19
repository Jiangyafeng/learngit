## 一、了解和熟悉git基本用法的使用
![git流程图](https://img-blog.csdnimg.cn/20191105104720297.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzY3NzkzNg==,size_16,color_FFFFFF,t_70)
Workspace：工作区； Index / Stage：暂存区；     Repository：仓库区（或本地仓库）；    Remote：远程仓库。
1. 新建代码库
```
# 在当前目录新建一个Git代码库
$ git init

# 新建一个目录，将其初始化为Git代码库
$ git init [project-name]

# 下载一个项目和它的整个代码历史
$ git clone [url]
```
2. 配置

Git的设置文件为.gitconfig，它可以在用户主目录下（全局配置），也可以在项目目录下（项目配置）。
```
# 显示当前的Git配置
$ git config --list

# 编辑Git配置文件
$ git config -e [--global]

# 设置提交代码时的用户信息
$ git config [--global] user.name "[name]"
$ git config [--global] user.email "[email address]"
```
3. 增加/删除文件
```
# 添加指定文件到暂存区
$ git add [file1] [file2] ...

# 添加指定目录到暂存区，包括子目录
$ git add [dir]

# 添加当前目录的所有文件到暂存区
$ git add .

# 添加每个变化前，都会要求确认
# 对于同一个文件的多处变化，可以实现分次提交
$ git add -p

# 删除工作区文件，并且将这次删除放入暂存区
$ git rm [file1] [file2] ...

# 停止追踪指定文件，但该文件会保留在工作区
$ git rm --cached [file]

# 改名文件，并且将这个改名放入暂存区
$ git mv [file-original] [file-renamed]
```
4. 代码提交
```
# 提交暂存区到仓库区
$ git commit -m [message]

# 提交暂存区的指定文件到仓库区
$ git commit [file1] [file2] ... -m [message]

# 提交工作区自上次commit之后的变化，直接到仓库区
$ git commit -a

# 提交时显示所有diff信息
$ git commit -v

# 使用一次新的commit，替代上一次提交
# 如果代码没有任何新变化，则用来改写上一次commit的提交信息
$ git commit --amend -m [message]

# 重做上一次commit，并包括指定文件的新变化
$ git commit --amend [file1] [file2] ...
```
5. 分支
```
# 列出所有本地分支
$ git branch

# 列出所有远程分支
$ git branch -r

# 列出所有本地分支和远程分支
$ git branch -a

# 新建一个分支，但依然停留在当前分支
$ git branch [branch-name]

# 新建一个分支，并切换到该分支
$ git checkout -b [branch]

# 新建一个分支，指向指定commit
$ git branch [branch] [commit]

# 新建一个分支，与指定的远程分支建立追踪关系
$ git branch --track [branch] [remote-branch]

# 切换到指定分支，并更新工作区
$ git checkout [branch-name]

# 切换到上一个分支
$ git checkout -

# 建立追踪关系，在现有分支与指定的远程分支之间
$ git branch --set-upstream [branch] [remote-branch]

# 合并指定分支到当前分支
$ git merge [branch]

# 选择一个commit，合并进当前分支
$ git cherry-pick [commit]

# 删除分支
$ git branch -d [branch-name]

# 删除远程分支
$ git push origin --delete [branch-name]
$ git branch -dr [remote/branch]
```
6. 标签
```
# 列出所有tag
$ git tag

# 新建一个tag在当前commit
$ git tag [tag]

# 新建一个tag在指定commit
$ git tag [tag] [commit]

# 删除本地tag
$ git tag -d [tag]

# 删除远程tag
$ git push origin :refs/tags/[tagName]

# 查看tag信息
$ git show [tag]

# 提交指定tag
$ git push [remote] [tag]

# 提交所有tag
$ git push [remote] --tags

# 新建一个分支，指向某个tag
$ git checkout -b [branch] [tag]
```
7. 查看信息
```
# 显示有变更的文件
$ git status

# 显示当前分支的版本历史
$ git log

# 显示commit历史，以及每次commit发生变更的文件
$ git log --stat

# 搜索提交历史，根据关键词
$ git log -S [keyword]

# 显示某个commit之后的所有变动，每个commit占据一行
$ git log [tag] HEAD --pretty=format:%s

# 显示某个commit之后的所有变动，其"提交说明"必须符合搜索条件
$ git log [tag] HEAD --grep feature

# 显示某个文件的版本历史，包括文件改名
$ git log --follow [file]
$ git whatchanged [file]

# 显示指定文件相关的每一次diff
$ git log -p [file]

# 显示过去5次提交
$ git log -5 --pretty --oneline

# 显示所有提交过的用户，按提交次数排序
$ git shortlog -sn

# 显示指定文件是什么人在什么时间修改过
$ git blame [file]

# 显示暂存区和工作区的差异
$ git diff

# 显示暂存区和上一个commit的差异
$ git diff --cached [file]

# 显示工作区与当前分支最新commit之间的差异
$ git diff HEAD

# 显示两次提交之间的差异
$ git diff [first-branch]...[second-branch]

# 显示今天你写了多少行代码
$ git diff --shortstat "@{0 day ago}"

# 显示某次提交的元数据和内容变化
$ git show [commit]

# 显示某次提交发生变化的文件
$ git show --name-only [commit]

# 显示某次提交时，某个文件的内容
$ git show [commit]:[filename]

# 显示当前分支的最近几次提交
$ git reflog
```
8. 远程同步
```
# 下载远程仓库的所有变动
$ git fetch [remote]

# 显示所有远程仓库
$ git remote -v

# 显示某个远程仓库的信息
$ git remote show [remote]

# 增加一个新的远程仓库，并命名
$ git remote add [shortname] [url]

# 取回远程仓库的变化，并与本地分支合并
$ git pull [remote] [branch]

# 上传本地指定分支到远程仓库
$ git push [remote] [branch]

# 强行推送当前分支到远程仓库，即使有冲突
$ git push [remote] --force

# 推送所有分支到远程仓库
$ git push [remote] --all
```
9. 撤销
```
# 恢复暂存区的指定文件到工作区
$ git checkout [file]

# 恢复某个commit的指定文件到暂存区和工作区
$ git checkout [commit] [file]

# 恢复暂存区的所有文件到工作区
$ git checkout .

# 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变
$ git reset [file]

# 重置暂存区与工作区，与上一次commit保持一致
$ git reset --hard

# 重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变
$ git reset [commit]

# 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
$ git reset --hard [commit]

# 重置当前HEAD为指定commit，但保持暂存区和工作区不变
$ git reset --keep [commit]

# 新建一个commit，用来撤销指定commit
# 后者的所有变化都将被前者抵消，并且应用到当前分支
$ git revert [commit]

# 暂时将未提交的变化移除，稍后再移入
$ git stash
$ git stash pop
```
10. 其他
```
# 生成一个可供发布的压缩包
$ git archive
```

## 二、把本地仓库推送到github远程仓库

1. 首先先进行账户注册。

* 指令为：git config  --global user.name "账号"     git config  --global user.email "youxiang.com"

2. 建立一个空的文件夹，创建一个git仓库。

* 指令为：mkdir +文件名、cd ~/文件名、git init

3. 创建一个new.txt文件并打开，添加内容，之后保存到刚创建得文件夹里面；

* 指令为：touch +文件名、nano +文件名（在里面按【Ctrl+o】回车，按【Ctrl+x】退出）

4. 可以查看相关文件状态，然后在修改文件状态

* 指令为：git status 、git add +文件名（添加指定文件到暂存区）、再次输入git status

5. 将暂存区的内容正式提交到本地仓库

* 指令为：git commit -m "提交信息"

* ==注意:== 当你注册过git的账户，没有进行第一步操作，将会出现ERROR，这时候你可以重新输入自己已经注册过得账户和密码就OK。

6. 之后的每一次修改文件内容，然后都要将其提交到本地仓库。
  
* 指令为：nano +文件名、git commit -m "提交信息"

7. 可以查看提交的版本，以及退回之前的版本。

* 指令为：git log、git reset --hard  HEAD～1

8.创建SSH KEY，并打开KEY文件框里粘贴id_rsa.pub文件内容；

* 指令为：ssh-keygen -t rsa -C "youxiang.com"、cat ~/.ssh/id_rsa.pub

* 注意：如果没有安装github，需要安装sudo apt install git-hub

9.登陆github，打开setting，然后进入SSH and GPG keys页面，最后点击New SSHKey，填上Title，在Key文件框里粘贴id_rsa.pub文件内容；

* ==注意：首先要在"create a new repository"按钮，创建一个新的仓库，在Repository name填入leangit(文件名），其他保持默认设置，点击"Create repository"按钮，就创建了一个新的git仓库（HTTP/SSH，这里选择了SSH）。==

10.建立本地仓库和github仓库联系。

* 指令为：git remote add origin git@github.com:michaelliao/learngit.git

      git push -u origin master

* ==注意：把上面的michaelliao替换成自己的github账户名，否则，你在本地关联的只是远程库，但是你以后推送是推送不上去，因为你的SSH Key公钥不在自己的账户列表中。由于远程库是空的，所以第一次推送master分支是，加上了-u参数，之后就可以不用此参数就可以。还需要注意的是，当你出现“fatal:远程origin已经存在”这个时,需要将此移除，指令为：git remote rm origin，否则你将无法推送上去，是因为你一直在运行之前SSH key。当推送过程的时候出现报错是git@github.com:permission denied(publickey)，则可以用ssh -T git@github.com去测试一下，检查是否和github连通，然后重新输入git remote rm origin，再进行git remote add origin git@github.com:michaelliao/learngit.git。==

## 三、其他
### 1. 代码回退
* (1) 首先你要用git log 查看你要回到的那个本版，
然后用
```
git reset --hard HEAD^        回退到上个版本
git reset --hard commit_id    退到/进到 指定commit_id
```
把你的本地代码回到你复制的某个版本上
如果你要把回退的某个版本提交的远程的话
```
git push origin HEAD --force
```
当你想恢复到新的版本怎么办？用git reflog打印你记录你的每一次操作记录

git reflog 可以查看所有分支的所有操作记录（包括（包括commit和reset的操作），包括已经被删除的commit记录，git log则不能察看已经删除了的commit记录，而且跟进结果可以回退道某一个修改

* (2) 如果你要回吧本地的代码回到最新的并且你回退的版本没有提交到远程 就用
```
git checkout master
```
### 2. 自己常用指令
git status 查看状态
git branch 查看分支和切换分支
git log 查看提交代码的记录
git diff HEAD^ HEAD 执行 git diff 来查看执行 git status 的结果的详细信息
git reset --soft HEAD^^ 退回前两个版本
[git相关资料](https://www.yiibai.com/git/git_environment.html)

### 3. 格式命名规则  
* [Commit message 和 Change log 编写指南](http://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html)

### 4. 生成客户文件
* [git patch](https://blog.csdn.net/liuhaomatou/article/details/54410361)
```
git log 
git format-patch
git am --abort
git am
git apply --stat file.path
git app;y --check file.path
```

3. reset三种不同
![reset的原理图](https://upload-images.jianshu.io/upload_images/4428238-fcad08ebe26933a6.png)

reset三种模式区别和使用场景
* 区别：
--hard：==重置位置的同时，直接将 working Tree工作目录、 index 暂存区及 repository 都重置成目标Reset节点的內容,所以效果看起来等同于清空暂存区和工作区。==
--soft：==重置位置的同时，保留working Tree工作目录和index暂存区的内容，== 只让repository中的内容和 reset 目标节点保持一致，因此原节点和reset节点之间的【差异变更集】会放入index暂存区中(Staged files)。所以效果看起来就是工作目录的内容不变，暂存区原有的内容也不变，只是原节点和Reset节点之间的所有差异都会放到暂存区中。
--mixed（默认）：==重置位置的同时，只保留Working Tree工作目录的內容，但会将 Index暂存区 和 Repository 中的內容更改和reset目标节点一致，== 因此原节点和Reset节点之间的【差异变更集】会放入Working Tree工作目录中。所以效果看起来就是原节点和Reset节点之间的所有差异都会放到工作目录中。

* 使用场景:
--hard：(1) 要放弃目前本地的所有改变時，即去掉所有add到暂存区的文件和工作区的文件，可以执行 git reset -hard HEAD 来强制恢复git管理的文件夹的內容及状态；(2) 真的想抛弃目标节点后的所有commit（可能觉得目标节点到原节点之间的commit提交都是错了，之前所有的commit有问题）。
--soft：原节点和reset节点之间的【差异变更集】会放入index暂存区中(Staged files)，所以假如我们之前工作目录没有改过任何文件，也没add到暂存区，那么使用reset --soft后，我们可以直接执行 git commit 將 index暂存区中的內容提交至 repository 中。为什么要这样呢？这样做的使用场景是：假如我们想合并「当前节点」与「reset目标节点」之间不具太大意义的 commit 记录(可能是阶段性地频繁提交,就是开发一个功能的时候，改或者增加一个文件的时候就commit，这样做导致一个完整的功能可能会好多个commit点，这时假如你需要把这些commit整合成一个commit的时候)時，可以考虑使用reset --soft来让 commit 演进线图较为清晰。总而言之，可以使用--soft合并commit节点。
--mixed（默认）：(1)使用完reset --mixed后，我們可以直接执行 git add 将這些改变果的文件內容加入 index 暂存区中，再执行 git commit 将 Index暂存区 中的內容提交至Repository中，这样一样可以达到合并commit节点的效果（与上面--soft合并commit节点差不多，只是多了git add添加到暂存区的操作）；(2)移除所有Index暂存区中准备要提交的文件(Staged files)，我们可以执行 git reset HEAD 来 Unstage 所有已列入 Index暂存区 的待提交的文件。(有时候发现add错文件到暂存区，就可以使用命令)。(3)commit提交某些错误代码，或者没有必要的文件也被commit上去，不想再修改错误再commit（因为会留下一个错误commit点），可以回退到正确的commit点上，然后所有原节点和reset节点之间差异会返回工作目录，假如有个没必要的文件的话就可以直接删除了，再commit上去就OK了

### 5. 文件对比

* 安装 meld
```
sudo apt-get install meld
```
* (1) 先复制一个旧文件
```
cp + old files + new files -rf 
```
* (2) 然后切回新分支，取远程仓库
```
git branch + 目的分支
cd + new files
git reset --hard HEAD
git pull 
```
* (3) 选择旧文件和新文件的对比文件
```
meld + old files + new files
```

