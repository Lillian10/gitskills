Git简介
====
---
参考：http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000  
Git是目前世界上最先进的分布式版本控制系统。

###Git的诞生###
* Linus花了两周时间自己用C写了一个分布式版本控制系统，这就是Git！
* Git迅速成为最流行的分布式版本控制系统，尤其是2008年，GitHub网站上线了，它为开源项目免费提供Git存储，无数开源项目开始迁移至GitHub，包括jQuery，PHP，Ruby等等。
###集中式vs分布式###
相比，CVS及SVN都是集中式的版本控制系统；而Git是分布式版本控制系统。

集中式版本控制系统
>* 集中式版本控制系统，版本库是集中存放在中央服务器的，而干活的时候，用的都是自己的电脑，所以要先从中央服务器取得最新的版本，然后开始干活，干完活了，再把自己的活推送给中央服务器。
>* 集中式版本控制系统最大的毛病就是必须联网才能工作，如果在局域网内还好，带宽够大，速度够快，可如果在互联网上，遇到网速慢的话，可能提交一个10M的文件就需要5分钟，这还不得把人给憋死啊。 

分布式版本控制系统与集中式版本控制系统有何不同?
>* 分布式版本控制系统根本没有“中央服务器”，每个人的电脑上都是一个完整的版本库。工作时不需要联网，版本库就在你自己的电脑上。
>* 分布式版本控制系统的安全性要高很多，因为每个人电脑里都有完整的版本库，某一个人的电脑坏掉了不要紧，随便从其他人那里复制一个就可以了。而集中式版本控制系统的中央服务器要是出了问题，所有人都没法干活了。
>* Git的优势不单是不必联网这么简单，后面我们还会看到Git极其强大的分支管理。

版本控制的种类
>* CVS作为最早的开源而且免费的集中式版本控制系统，直到现在还有不少人在用。由于CVS自身设计的问题，会造成提交文件不完整，版本库莫名其妙损坏的情况。
>* 同样是开源而且免费的SVN修正了CVS的一些稳定性问题，是目前用得最多的集中式版本库控制系统。
>* 除了免费的外，还有收费的集中式版本控制系统，比如IBM的ClearCase（以前是Rational公司的，被IBM收购了），特点是安装比Windows还大，运行比蜗牛还慢，能用ClearCase的一般是世界500强，他们有个共同的特点是财大气粗，或者人傻钱多。
>* 微软自己也有一个集中式版本控制系统叫VSS，集成在Visual Studio中。由于其反人类的设计，连微软自己都不好意思用了。
>* 分布式版本控制系统除了Git以及促使Git诞生的BitKeeper外，还有类似Git的Mercurial和Bazaar等。

安装Git
===
---
最早Git是在Linux上开发的，很长一段时间内，Git也只能在Linux和Unix系统上跑。不过，慢慢地有人把它移植到了Windows上。
###在Linux上安装Git###
创建版本库
===
---
版本库又名仓库，英文名repository，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。
#####1.创建版本库
创建一个空目录：  

    [root@FirstPro learngit]# pwd  
    /usr/michael/learngit
#####2.通过git init命令把这个目录变成Git可以管理的仓库
[root@FirstPro learngit]# git init  
Initialized empty Git repository in /usr/michael/learngit/.git/
#####3.把文件添加到版本库
把一个文件放到Git仓库只需要两步。  
1. 用命令git add告诉Git，把文件添加到仓库  
[root@FirstPro learngit]# git add readme.txt  
[root@FirstPro learngit]#   
2. 用命令git commit告诉Git，把文件提交到仓库  
如果报错：
fatal: unable to auto-detect email address (got 'root@FirstPro.(none)')  
please run:  
[root@FirstPro learngit]# git config --global user.name "Yanli"  
[root@FirstPro learngit]# git config --global user.email "yanli10@leju.com"  
[root@FirstPro learngit]# git commit -m "wrote a readme file"              
[master (root-commit) d4ad733] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
###小结###
初始化一个Git仓库，使用git init命令。  
添加文件到Git仓库，分两步：  
* 第一步，使用命令git add <file>，注意，可反复多次使用，添加多个文件；  
* 第二步，使用命令git commit，完成。  
时光机穿梭
===
---
###git status命令###
* 没有修改的情况下  
  [root@FirstPro learngit]# git status  
  # On branch master  
  nothing to commit (working directory clean)  
Git告诉我们当前没有需要提交的修改，而且，工作目录是干净（working directory clean）的。
* 修改了的情况  
no changes added to commit (use "git add" and/or "git commit -a")  
git status命令可以让我们时刻掌握仓库当前的状态，上面的命令告诉我们，readme.txt被修改过了，但还没有准备提交的修改。  
###git diff命令###
查看文件修改了什么内容。
###小结###
要随时掌握工作区的状态，使用git status命令。  
如果git status告诉你有文件被修改过，用git diff可以查看修改内容。  
用git diff HEAD -- readme.txt命令可以查看工作区和版本库里面最新版本的区别。
##版本回退##
1. 使用git log命令，为了屏蔽更多的信息，可以加上--pretty=oneline参数  
2. Git的commit id是一个SHA1计算出来的一个非常大的数字，用十六进制表示  
3. 为什么commit id需要用这么一大串数字表示呢？  
因为Git是分布式的版本控制系统，后面我们还要研究多人在同一个版本库里工作，如果大家都用1，2，3……作为版本号，那肯定就冲突了。
4. 回退上一个版本
>首先，Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，也就是最新的提交3628164...882e1e0（注意我的提交ID和你的肯定不一样），上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。  

[root@FirstPro learngit]# git reset --har HEAD^   
HEAD is now at ac15fe9 append GPL
>--hard参数有啥意义？这个后面再讲，现在你先放心使用。 
 
如果要在恢复未来的某个版本？  

* 办法其实还是有的，只要上面的命令行窗口还没有被关掉，你就可以顺着往上找啊找啊，找到那个append GPL的commit id是3628164...，于是就可以指定回到未来的某个版本：  
$ git reset --hard 3628164  
HEAD is now at 3628164 append GPL  
* git reflog命令记录每次命令。
###小结###
* HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。
* 穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。
* 要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。

##工作区和暂存区##
###工作区###
在你的电脑里能看到的目录，比如建立的learngit文件夹就是一个工作区。
###版本库###
工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。  
Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。
###小结###
我们把文件往Git版本库里添加的时候，是分两步执行的：  
第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；  
第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。
我们创建Git版本库时，Git自动为我们创建了唯一一个master分支，所以，现在，git commit就是往master分支上提交更改。  
可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。
##管理修改##
Git跟踪并管理的是修改，并非是文件。正因为如此，Git比其他版本的控制系统设计的优秀。
###小结###
Git是如何跟踪修改的？  
每次修改，如果不add到暂存区，那就不会加入到commit中。
##撤销修改##
###git add前撤销修改###
如果修改文件错误，在没有git add之前，可以用手动删除错误的地方。  
另外，用git status查看，还可以用git checkout -- file 丢弃工作区的修改的方法。  
PS：git checkout -- file命令中的--很重要，没有--，就变成了“切换到另一个分支”的命令。
###git add后，commit之前撤销修改###
用命令git reset HEAD file可以把暂存区的修改撤销掉（unstage），重新放回工作区。  
>[root@FirstPro learngit]# git reset HEAD readme.txt  
>Unstaged changes after reset:  
>M       readme.txt  
>[root@FirstPro learngit]# git status  
>\# On branch master  
>\# Changes not staged for commit:  
>\#   (use "git add <file>..." to update what will be committed)  
>\#   (use "git checkout -- <file>..." to discard changes in working directory)  
>\#
>\#       modified:   readme.txt  
>\#  
>no changes added to commit (use "git add" and/or "git commit -a")
>从状态可以看到，现在暂存区是干净的，工作区有修改。
###提示###
已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。
Git是分布式版本控制系统。
##删除文件##
1. rm  
如果此时本地误删，可以用git checkout -- file从版本库恢复此文件。
2. git rm
3. git commit

远程仓库
===
---
如果只是在一个仓库里管理文件历史，Git和SVN真没啥区别。  
远程仓库，Git杀手级功能之一。  
Git是分布式版本控制系统，同一个Git仓库，可以分布到不同的机器上。怎么分布呢？最早，肯定只有一台机器有一个原始版本库，此后，别的机器可以“克隆”这个原始版本库，而且每台机器的版本库其实都是一样的，并没有主次之分。
###注册GitHub账号，免费获得Git远程仓库###
1. 去[GitHub](https://github.com/ "github网址")网站上注册账号。这个网站就是提供Git仓库托管服务的。
2. 创建SSH Key。  
在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。  
如果没有，执行命令 ssh-keygen -t rsa -C "youremail@example.com"  
    >使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。
2. 登陆GitHub，打开“Account settings”，“SSH Keys”页面：  
然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容。
3. 保存完成。
    >GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。
###友情提示###
在GitHub上免费托管的Git仓库，任何人都可以看到（但只有你自己才能改）。所以，不要把敏感信息放进去。
##添加远程库##
* 首先，登陆GitHub，然后，在右上角找到“Create a new repo”按钮，创建一个新的仓库：  
![创建新仓库](http://www.liaoxuefeng.com/files/attachments/0013849084639042e9b7d8d927140dba47c13e76fe5f0d6000/0 "github-create-repo-1")
* 在Repository name填入learngit，其他保持默认设置，点击“Create repository”按钮，就成功地创建了一个新的Git仓库：  
![创建仓库成功](http://www.liaoxuefeng.com/files/attachments/0013849084720379a3eae576b9f417da2add578c8612a2e000/0 "github-create-repo-2")  
* 目前，在GitHub上的这个learngit仓库还是空的，GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。
* 根据GitHub的提示，在本地的learngit仓库下运行命令：  
    >1. [yanli@FirstPro learngit]$ git remote add origin https://github.com/xxxx/learngit.git  
    [yanli@FirstPro learngit]$   
    >2. [yanli@FirstPro learngit]$ git push -u origin master  
    (gnome-ssh-askpass:4594): Gtk-WARNING **: cannot open display:   
    error: unable to read askpass response from '/usr/libexec/openssh/gnome-ssh-askpass'
    >3. [yanli@FirstPro learngit]$ git push -u origin master  
    >Username for 'https://github.com': xxxx  
    >Password for 'https://xxxx@github.com':  
    >Counting objects: 21, done.  
    >Compressing objects: 100% (16/16), done.  
    >Writing objects: 100% (21/21), 1.66 KiB, done.  
    >Total 21 (delta 6), reused 0 (delta 0)  
    >To https://github.com/xxxx/learngit.git  
    >\* [new branch]      master -> master  
    >Branch master set up to track remote branch master 
    >from origin.  
    
    把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程。  
    由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。
* 推送成功后，可以立刻在GitHub页面中看到远程库的内容已经和本地一模一样：  
![推送成功图](http://www.liaoxuefeng.com/files/attachments/00138490848464619aebd9a2bb0493c83e132ca1eed6f66000/0 "github-repo")
* 只要本地作了提交，就可以通过命令：
git push origin master  
把本地master分支的最新修改推送至GitHub。
###SSH警告###
当你第一次使用Git的clone或者push命令连接GitHub时，会得到一个警告：
>The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.
RSA key fingerprint is xx.xx.xx.xx.xx.
Are you sure you want to continue connecting (yes/no)?  
>这是因为Git使用SSH连接，而SSH连接在第一次验证GitHub服务器的Key时，需要你确认GitHub的Key的指纹信息是否真的来自GitHub的服务器，输入yes回车即可。
>Git会输出一个警告，告诉你已经把GitHub的Key添加到本机的一个信任列表里了：  
>Warning: Permanently added 'github.com' (RSA) to the list of known hosts.  
>这个警告只会出现一次，后面的操作就不会有任何警告了。  
>如果你实在担心有人冒充GitHub服务器，输入yes前可以对照GitHub的RSA Key的指纹信息是否与SSH连接给出的一致。
###小结###
* 要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；  
* 关联后，使用命令git push -u origin master第一次推送master分支的所有内容；  
* 此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；  
* 分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作。
##从远程库克隆##
假设我们从零开发，那么最好的方式是先创建远程库，然后，从远程库克隆。  
1. 首先，登陆GitHub，创建一个新的仓库，名字叫gitskills：  
![创建新仓库远程](http://www.liaoxuefeng.com/files/attachments/0013849085474010fec165e9c7449eea4417512c2b64bc9000/0 "github-init-repo")
2. 我们勾选Initialize this repository with a README，这样GitHub会自动为我们创建一个README.md文件。创建完毕后，可以看到README.md文件：  
![创建完成远程](http://www.liaoxuefeng.com/files/attachments/0013849085607106c2391754c544772830983d189bad807000/0 "github-init-repo-2")
3. 远程库已经准备好了，下一步是用命令git clone克隆一个本地库：  
[yanli@FirstPro learngit]$ git clone git@github.com:xxxx/gitskills.git  
    Cloning into 'gitskills'...  
    The authenticity of host 'github.com (192.30.252.130)' can't be established.  
    RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.  
    Are you sure you want to continue connecting (yes/no)? yes  
    Warning: Permanently added 'github.com,192.30.252.130' (RSA) to the list of known hosts.  
    remote: Counting objects: 3, done.  
    remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0  
    Receiving objects: 100% (3/3), 219 bytes, done.  
4. 多个人协作开发，那么每个人各自从远程克隆一份就可以了。  
###克隆地址介绍###
* GitHub给出的地址不止一个，还可以用https://github.com/xxxx/gitskills.git这样的地址。实际上，Git支持多种协议，默认的git://使用ssh，但也可以使用https等其他协议。  
* 使用https除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放http端口的公司内部就无法使用ssh协议而只能用https。
###小结###
* 要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。
* Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快。

分支管理
===
---

分支在实际中有什么用呢？  
假设你准备开发一个新功能，但是需要两周才能完成，第一周你写了50%的代码，如果立刻提交，由于代码还没写完，不完整的代码库会导致别人不能干活了。如果等代码全部写完再一次提交，又存在丢失每天进度的巨大风险。  
现在有了分支，就不用怕了。你创建了一个属于你自己的分支，别人看不到，还继续在原来的分支上正常工作，而你在自己的分支上干活，想提交就提交，直到开发完毕后，再一次性合并到原来的分支上，这样，既安全，又不影响别人工作。  
但Git的分支是与众不同的，无论创建、切换和删除分支，Git在1秒钟之内就能完成！无论你的版本库是1个文件还是1万个文件。
##创建与合并分支##
首先，我们创建dev分支，然后切换到dev分支：

    [yanli@FirstPro learngit]$ git checkout -b dev
    Switched to a new branch 'dev'
git checkout命令加上-b参数表示创建并切换，相当于以下两条命令：

    $ git branch dev
	$ git checkout dev
	Switched to branch 'dev'
然后，用git branch命令查看当前分支：(git branch命令会列出所有分支，当前分支前面会标一个*号。)

    [yanli@FirstPro learngit]$ git branch
    * dev
    master
然后，我们就可以在dev分支上正常提交，比如对readme.txt做个修改，加上一行：

	Creating a new branch is quick.
提交：

	[yanli@FirstPro learngit]$ git add readme.txt
	[yanli@FirstPro learngit]$ git commit -m "branch dev test"
	[dev 57ed911] branch dev test
	 1 file changed, 1 insertion(+), 1 deletion(-)
dev分支的工作完成，我们就可以切换回master分支：

	$ git checkout master
	Switched to branch 'master'
切换回master分支后，再查看一个readme.txt文件，刚才添加的内容不见了！因为那个提交是在dev分支上，而master分支此刻的提交点并没有变：  
现在，我们把dev分支的工作成果合并到master分支上：

	[yanli@FirstPro learngit]$ git merge dev
	Updating 93222cb..57ed911
	Fast-forward
	 readme.txt | 2 +-
	 1 file changed, 1 insertion(+), 1 deletion(-)
git merge命令用于合并指定分支到当前分支。  
注意到上面的Fast-forward信息，Git告诉我们，这次合并是“快进模式”，也就是直接把master指向dev的当前提交，所以合并速度非常快。  
当然，也不是每次合并都能Fast-forward，我们后面会将其他方式的合并。  
合并完成后，就可以放心地删除dev分支了：

	[yanli@FirstPro learngit]$ git branch -d dev
	Deleted branch dev (was 57ed911).
	[yanli@FirstPro learngit]$ git branch
	* master
###小结###
Git鼓励大量使用分支：

查看分支：git branch

创建分支：git branch <name>

切换分支：git checkout <name>

创建+切换分支：git checkout -b <name>

合并某分支到当前分支：git merge <name>

删除分支：git branch -d <name>

##解决冲突##
master分支和feature1分支各自都分别有新的提交，就会出现冲突  
用带参数的git log也可以看到分支的合并情况

    $ git log --graph --pretty=oneline --abbrev-commit
###小结###
当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

用git log --graph命令可以看到分支合并图。

##分支管理策略##
在实际开发中，我们应该按照几个基本原则进行分支管理：  
首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；  
干活都在dev分支上，也就是说，dev分支是不稳定的。  
你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。  
所以，团队合作的分支看起来就像这样：  
![分支合作图](http://www.liaoxuefeng.com/files/attachments/001384909239390d355eb07d9d64305b6322aaf4edac1e3000/0 "git-br-policy")

###小结###
参考：http://www.oschina.net/question/31384_157479  
合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。

##Bug分支##
在dev分支正在有add的情况下，急需修复一个Bug。

    [yanli@FirstPro learngit]$ git status
	# On branch dev
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#       new file:   hello.py
	#
	# Changes not staged for commit:
	#   (use "git add <file>..." to update what will be committed)
	#   (use "git checkout -- <file>..." to discard changes in working directory)
	#
	#       modified:   readme.txt
	#
	[yanli@FirstPro learngit]$ git branch
	* dev
  	master

解决办法：  
Git还提供了一个stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：

    [yanli@FirstPro learngit]$ git stash
	Saved working directory and index state WIP on dev: e862204 conflict fixed
	HEAD is now at e862204 conflict fixed

现在，用git status查看工作区，就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复bug。

    [yanli@FirstPro learngit]$ git status
	# On branch dev
	nothing to commit (working directory clean)
首先确定要在哪个分支上修复bug，假定需要在master分支上修复，就从master创建临时分支：

    [yanli@FirstPro learngit]$ git checkout master
	Switched to branch 'master'
	Your branch is ahead of 'origin/master' by 3 commits.

	[yanli@FirstPro learngit]$ git checkout -b issue-101
	Switched to a new branch 'issue-101'
现在新分支上进行修复bug，并且提交，修复完成后，切换到master分支，完成合并，最后删除issue-101分支。  

修复完BUG，接着回到dev分支干活。  

    [yanli@FirstPro learngit]$ git checkout dev
	Switched to branch 'dev'
	[yanli@FirstPro learngit]$ git branch
	* dev
  	master
	[yanli@FirstPro learngit]$ git status
	# On branch dev
	nothing to commit (working directory clean)
用git stash list命令看看，工作现场还在，Git把stash内容存在某个地方了。

    [yanli@FirstPro learngit]$ git stash list
	stash@{0}: WIP on dev: e862204 conflict fixed
有两个办法可以恢复：  

* 用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；
* 用git stash pop，恢复的同时把stash内容也删了。  
    >[yanli@FirstPro learngit]$ git stash list  
    >stash@{0}: WIP on dev: e862204 conflict fixed  
	>[yanli@FirstPro learngit]$ git stash pop  
	\# On branch dev  
	\# Changes to be committed:  
	\#   (use "git reset HEAD <file>..." to unstage)  
	\#  
	\#       new file:   hello.py  
	\#
	\# Changes not staged for commit:  
	\#   (use "git add <file>..." to update what will be committed)  
    \#   (use "git checkout -- <file>..." to discard changes in working directory)
    \#
    \#       modified:   readme.txt
    \#
    \Dropped refs/stash@{0} (636a3af71b27453505372ab356be6ca853f340d2)
恢复了之后，再用git stash list查看，就看不到任何stash内容了。  
* 你可以多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash，用命令：  
    git stash apply stash@{0}

###小结###
修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；  
当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场。

##Feature分支##
开发一个新feature，最好新建一个分支；

在新分支没有被合并，用git branch -d xxx删除时，会报错：

    $ git branch -d feature-vulcan
	error: The branch 'feature-vulcan' is not fully merged.
	If you are sure you want to delete it, run 'git branch -D feature-vulcan'.
如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。

##多人协作##
查看远程库的信息，用git remote：  

    [yanli@FirstPro gitskills]$ git remote
    origin
或者，用git remote -v显示更详细的信息：

    [yanli@FirstPro learngit]$ git remote -v
    origin  https://github.com/xxxx/learngit.git (fetch)
    origin  https://github.com/xxxx/learngit.git (push)
上面显示了可以抓取和推送的origin的地址。如果没有推送权限，就看不到push的地址。

###推送分支（到远程）###
推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支。

    $ git push origin master
比如要推送dev分支到远程：

	$ git push origin dev
###抓取分支？###

###多人协作的工作模式###
1. 首先，可以试图用git push origin branch-name推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；  
**推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，解决办法也很简单，Git已经提示我们，先用git pull把最新的提交从origin/dev抓下来，然后，在本地合并，解决冲突，再推送**  
**git pull也失败了，原因是没有指定本地dev分支与远程origin/dev分支的链接，根据提示，设置dev和origin/dev的链接**  
    >$ git branch --set-upstream dev origin/dev  
	>Branch dev set up to track remote branch dev from origin.
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！

###小结###
* 查看远程库信息，使用git remote -v；
* 本地新建的分支如果不推送到远程，对其他人就是不可见的；
* 从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；
* 在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；
* 建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；
* 从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。

标签管理
===
---
发布一个版本时，我们通常先在版本库中打一个标签，这样，就唯一确定了打标签时刻的版本。  
Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针。和分支很像，不同的是：分支可以移动，标签不能移动，所以，创建和删除标签都是瞬间完成的。

##创建标签##
首先，切换到需要打标签的分支上：  
然后，使用命令git tag <name>就可以打一个新标签：  
    
    [yanli@FirstPro learngit]$ git tag v1.0
	[yanli@FirstPro learngit]$ git tag
	v1.0
默认标签是打在最新提交的commit上的。  
有时候，如果忘了打标签，如何补打标签。  
方法是找到历史提交的commit id，然后打上就可以了：  
    
    [yanli@FirstPro learngit]$ git log --pretty=oneline --abbrev-commit
	e186f56 test.txt add add2 in dev1
	5157f62 add1 in dev
	0508ff4 test.txt
	f2480cd commit dev branch conntent
	e862204 conflict fixed
	bbd91f5 master add
	cfc3e6a branch featurel add
	57ed911 branch dev test
	93222cb test right github
	539325a add test.txt
	0f764c9 remove test.txt
	436d365 add test.txt
	56a6f9c commit 2 fles
	a401cde understand how stage works
	ac15fe9 append GPL
	768dfb1 for add second
	d4ad733 wrote a readme file
比方：bbd91f5 master add这次忘记打标签，就可以输入命令：
 
    [yanli@FirstPro learngit]$ git tag v0.9 bbd91f5
	[yanli@FirstPro learngit]$ git tag
	v0.9
	v1.0
注意，标签不是按时间顺序列出，而是按字母排序的。可以用git show <tagname>查看标签信息。  
###标签扩展说明###
创建带有说明的标签，用-a指定标签名，-m指定说明文字：
    
    [yanli@FirstPro learngit]$ git tag -a y0.1 -m "first version" d4ad733
	[yanli@FirstPro learngit]$ git tag
	v0.9
	v1.0
	y0.1
用命令git show <tagname>可以看到说明文字：
   
    [yanli@FirstPro learngit]$ git show y0.1
	tag y0.1
	Tagger: Yanli <yanli10@leju.com>
	Date:   Thu Jan 14 10:35:40 2016 +0800
	
	first version
	
	commit d4ad73367d0f534178a9f05c9301e99d6feb5c0e
	Author: Yanli <yanli10@leju.com>
	Date:   Mon Jan 11 16:41:15 2016 +0800
	
	    wrote a readme file
	
	diff --git a/readme.txt b/readme.txt
	new file mode 100644
	index 0000000..46d49bf
	--- /dev/null
	+++ b/readme.txt
	@@ -0,0 +1,2 @@
	+Git is a version control system.
	+Git is free software.
还可以通过-s用私钥签名一个标签：
    
    $ git tag -s v0.2 -m "signed version 0.2 released" fec145a
签名采用PGP签名，因此，必须首先安装gpg（GnuPG），如果没有找到gpg，或者没有gpg密钥对，就会报错：

    gpg: signing failed: secret key not available
	error: gpg failed to sign the data
	error: unable to sign the tag
本地测试报错：

	gpg: skipped "Yanli <yanli10@leju.com>": No secret key
	gpg: signing failed: No secret key
	error: gpg failed to sign the data
	error: unable to sign the tag
如果报错，请参考GnuPG帮助文档配置Key。  
同样，用命令git show <tagname>可以看到PGP签名信息。  
用PGP签名的标签是不可伪造的，因为可以验证PGP签名。验证签名的方法也比较复杂。
###小结###
* 命令git tag <name>用于新建一个标签，默认为HEAD，也可以指定一个commit id；
* git tag -a <tagname> -m "blablabla..."可以指定标签信息；
* git tag -s <tagname> -m "blablabla..."可以用PGP签名标签；
* 命令git tag可以查看所有标签。

##操作标签##
如果标签打错，可以删除：

	[yanli@FirstPro learngit]$ git tag -d v1.0
	Deleted tag 'v1.0' (was e186f56)
	[yanli@FirstPro learngit]$ git tag
	v0.9
	y0.1
因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。  
如果要推送某个标签到远程，使用命令git push origin <tagname>：  

	$ git push origin v1.0
	Total 0 (delta 0), reused 0 (delta 0)
	To git@github.com:michaelliao/learngit.git
	 * [new tag]         v1.0 -> v1.0
或者，一次性推送全部尚未推送到远程的本地标签：

    [yanli@FirstPro learngit]$ git push origin --tags
	Counting objects: 2, done.
	Compressing objects: 100% (2/2), done.
	Writing objects: 100% (2/2), 284 bytes, done.
	Total 2 (delta 0), reused 0 (delta 0)
	To https://github.com/fuwanini/learngit.git
	 * [new tag]         y0.1 -> y0.1
	 * [new tag]         y0.3 -> y0.3
如果标签已经推送到远程，要删除：  

1. 先从本地删除  
	>[yanli@FirstPro learngit]$ git tag -d v0.9  
	Deleted tag 'v0.9' (was bbd91f5)  
	[yanli@FirstPro learngit]$ git tag  
	y0.1  
	y0.3  

2. 从远程删除。删除命令也是push，但是格式如下：  
    >[yanli@FirstPro learngit]$ git push origin :refs/tags/v0.9  
    To https://github.com/fuwanini/learngit.git  
     \- [deleted]         v0.9

###小结###
* 命令git push origin <tagname>可以推送一个本地标签；
* 命令git push origin --tags可以推送全部未推送过的本地标签；
* 命令git tag -d <tagname>可以删除一个本地标签；
* 命令git push origin :refs/tags/<tagname>可以删除一个远程标签。

使用GitHub
===
---
在GitHub上，可以任意Fork开源仓库；  
自己拥有Fork后的仓库的读写权限；  
可以推送pull request给官方仓库来贡献代码。

等待pull request成功？？？

自定义Git
===
---
Git有很多可配置的项，以便使你的工作更高效。
##忽略特殊文件##
有些时候，你必须把某些文件放到Git工作目录中，但又不能提交它们，比如保存了数据库密码的配置文件啦，等等，每次git status都会显示Untracked files ...  
好在Git考虑到了大家的感受，这个问题解决起来也很简单，在Git工作区的根目录下创建一个特殊的.gitignore文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。
  
忽略文件的原则是：  
1. 忽略操作系统自动生成的文件，比如缩略图等；
2. 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的.class文件；
3. 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

###小结###
* 忽略某些文件时，需要编写.gitignore；
* .gitignore文件本身要放到版本库里，并且可以对.gitignore做版本管理！
* 该.gitignore只对当前分支起作用。

##配置别名##
举例：

	$ git config --global alias.st status
    $ git config --global alias.co checkout
	$ git config --global alias.ci commit
	$ git config --global alias.br branch
    $ git config --global alias.unstage 'reset HEAD'
    #显示最后一次提交信息
    $ git config --global alias.last 'log -1'
--global参数是全局参数，也就是这些命令在这台电脑的所有Git仓库下都有用。
###配置文件###
配置Git的时候，加上--global是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。  
每个仓库的Git配置文件都放在.git/config文件中。  
而当前用户的Git配置文件放在用户主目录下的一个隐藏文件.gitconfig中。  
别名就在[alias]后面，要删除别名，直接把对应的行删掉即可。  

搭建Git服务器
===
---
###第一步 安装git###

    [root@GitServer lier]# git --version
	git version 1.7.12.4
安装成功，安装的过程省略。
###第二步 创建git用户，来运行git服务###
	[root@GitServer lier]# adduser git
###第三步 创建证书登录###
把需要登录的用户公钥id_rsa.pub文件，导入到/home/git/.ssh/authorized_keys文件里  
id_rsa.pub文件在用户主目录的.ssh文件夹下。  
如果有多个，一行一个。  
PS，  
这个git主目录下，原本是没有.ssh，使用git用户创建并创建authorized_keys文件。
###第四步 初始化Git仓库###
	[root@GitServer srv]# git init --bare sample.git
	Initialized empty Git repository in /srv/sample.git/
Git就会创建一个裸仓库，裸仓库没有工作区，因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都以.git结尾。  
然后，把owner改为git：

	# chown -R git:git sample.git/
###第五步 git用户禁用shell登录###
可以通过编辑/etc/passwd文件完成。  

    git:x:1001:1001:,,,:/home/git:/bin/bash
改成：

    git:x:1001:1001:,,,:/home/git:/bin/git-shell
###第六步 克隆远程仓库###
在自己的电脑上运行： 
 
	[yanli@FirstPro michael]$ git clone git@ServerName:/srv/sample.git
	Cloning into 'sample'...
	warning: You appear to have cloned an empty repository.  
**如果进行第5步，那么在自己的电脑上就无法克隆成功。**
###小结###
* 要方便管理公钥，用Gitosis；
* 要像SVN那样变态地控制权限，用Gitolite。
这里略过。。。

Git 代码部署
===
---
##手动##
如果手动部署代码，首次的话，在git服务器上运行：

	[root@GitServer git]# git clone /srv/sample.git /home/git/sample
	Cloning into '/home/git/sample'...
	done.
/home/git/sample 这个目录是代码部署的目录。  
再次手动，直接在部署的目录下，执行git pull即可。

##Hooks 实现自动##
为了保证不被肆意部署，特加了一个对需要部署 commit 的判断，利用读取 commit subject 并匹配想要的字符串才去部署，这样我认为是一个比较安装的部署方案。  

Git的Hook主要包含：  

* applypatch-msg
* post-update
* pre-rebase
* commit-msg
* pre-applypatch
* update
* post-commit
* pre-commit
* post-receive
* prepare-commit-msg

自动部署功能，需要使用 post-receive 这个 Hook：在接收 post(push)
请求之后执行。

编辑post-receive文件，赋予执行权限。

	#!/bin/sh
	# Check the remote git repository whether it is bare
	IS_BARE=$(git rev-parse --is-bare-repository)
	if [ -z "$IS_BARE" ]; then
	        echo >&2 "fatal: post-receive: IS_NOT_BARE"
	        exit 1
	fi
	# Check the deploy dir whether it exists
	DEPLOY_DIR=/home/git/sample/
	if [ ! -d $DEPLOY_DIR ] ; then
	        echo >&2 "fatal: post-receive: DEPLOY_DIR_NOT_EXIST: \"$DEPLOY_DIR\""
	        exit 1
	fi
	
	# Goto the deploy dir and pull the latest sources
	cd $DEPLOY_DIR
	#env -i git reset --hard
	env -i git pull

经测试，客户端push的代码，服务器可以自动部署。
