# git tutorial 
![git](./git.jpeg) 
这是来自廖雪峰的官方网站的git教程的学习笔记。  
据说是史上最浅显易懂的Git教程！

**目录**
1. git简介
2. 版本库
3. 版本控制
4. 远程仓库

## git简介
git是分布式版本控制系统。

### 版本控制系统
什么是版本控制系统？  
针对文件的版本控制，系统将自动记录每次文件的改动，支持多人协作编辑。  
当查看某次改动，只需要在系统里查询即可，如下所示：

| 版本 | 文件名 | 用户 | 说明 |日期 |
| ----- | -------- | ----- | ----- | ----- |
| 1 | service1.doc | 张三 | 删除了软件服务条款5       | 7/12 10:38 |
| 2 | service2.doc | 张三 | 增加了License人数限制  | 7/12 18:09 |
| 3 | service1.doc | 李四 | 财务部门调整了合同金额 | 7/13 09:51 |
| 4 | service2.doc | 张三 | 延长了免费升级周期          | 7/14 15:17 |

### 分布式
什么是分布式？分布式是相对集中式而言的。  
集中式是将版本库存在中央服务器中。多人协同开发时从服务器中提取最新的版本库到本地进行作业，各自将修改提交至服务器。  
分布式是将版本库存放在各个作业的机器中，每个机器的版本库都是最新的。多人协同开发时将修改提交至所有版本库，保持所有版本库都经过同步更新。在实际执行中，通常使用一台中央服务器负责把各自的修改推送至所有的版本库。

## 版本库
什么是版本库？
版本库可以认为是代码仓库，英文名repository。理解为版本的历史仓库，git将记录其中每个文件的修改、删除，为了在任何时候都可以进行版本还原等操作。

### 创建版本库
尝试创建版本库`hello-world`，首先创建一个空目录。
注意，`$LOGNAME`是我的登陆名：jason。
```
$ cd /home/$LOGNAME/Repository
$ mkdir hello-world
$ cd hello-world
$ pwd
/home/jason/Repository/hello-world
```

使用`git init`创建一个空的 git 仓库或重新初始化一个已存在的仓库。
```
$ pwd
/home/jason/Repository/hello-world
$ git init
已初始化空的 Git 仓库于 /home/jason/Repository/hello-world/.git/
```

使用`ls -ah`发现当前目录下新建了`.git`目录，该目录存放git管理版本库的文件。  
在已存在仓库的目录使用`git init`将重新初始化仓库。  
另外，`git init`不要求在空目录执行。
```
$ pwd
/home/jason/Repository/hello-world
$ $ ls -ah
.  ..  .git
$ git init
重新初始化已存在的 Git 仓库于 /home/jason/Repository/hello-world/.git/
```

### 添加文件到版本库
尝试添加文件`txt_1.txt`到刚创建的版本库`hello-world`中。  
添加文件到版本库并不是简单把文件移动到指定目录就可以的。  
首先在目录`hello-world`创建文件`txt_1.txt`，内容如下：
```
txt_1
```

使用`git add <pathspec>`添加文件内容至索引。  
注意添加的文件要求存放在版本库目录下（子目录也可以）。
执行命令没有任何返回，说明添加成功。
> “没有消息就是好消息。”——*Unix的哲学*
```
$ pwd
/home/jason/Repository/hello-world
$ git add txt_1.txt
```

使用`git commit -m <msg>`记录变更到仓库。  
在提交中使用给定的`<msg>`作为提交说明。  
此处的提交说明非常重要，在版本控制中需要提交说明才能准确回滚。  
执行命令返回`1 file changed, 1 insertion(+)`。  
说明修改1个文件，插入1行文本。
```
$ pwd
/home/jason/Repository/hello-world
$ git commit -m 'wrote a txt file txt_1.txt'
 [master（根提交） bd9fb3c] wrote a txt file txt_1.txt
 Committer: jason <jason@Jason.PC>
您的姓名和邮件地址基于登录名和主机名进行了自动设置。请检查它们正确
与否。您可以对其进行设置以免再出现本提示信息：

    git config --global --edit

设置完毕后，您可以用下面的命令来修正本次提交所使用的用户身份：

    git commit --amend --reset-author

 1 file changed, 1 insertion(+)
 create mode 100644 txt_1.txt
```

根据提示，使用`git config --global --edit`设置登录名和主机名。  
取消`name = jason`和`email = jason@Jason.PC`两行的注释。
```
 # This is Git's per-user configuration file.
 [user]
 # Please adapt and uncomment the following lines: 
        name = jason
        email = jason@Jason.PC
```

使用`git commit --amend --reset-author -m 'wrote a txt file txt_1.txt and reset anthor'`修正上一次提交。
```
$ pwd
/home/jason/Repository/hello-world
$ git commit --amend --reset-author -m 'wrote a txt file txt_1.txt and reset anthor'
 [master d78f63a] wrote a txt file txt_1.txt and reset anthor
 1 file changed, 1 insertion(+)
 create mode 100644 txt_1.txt
```

注意使用`git add <pathspec>`可一次添加多个文件，  
然后使用`git commit -m <msg>`一次提交。  
参考`txt_1.txt`在`hello-world`目录下创建`txt_2.txt`和`txt_3.txt`。
```
$ pwd
/home/jason/Repository/hello-world
$ git add txt_2.txt txt_3.txt
$ git commit -m 'add two files: txt_2.txt, txt_3.txt'
 [master 8c7be40] add two files: txt_2.txt, txt_3.txt
 2 files changed, 2 insertions(+)
 create mode 100644 txt_2.txt
 create mode 100644 txt_3.txt
```

## 版本控制
使用git对版本库进行管理，查看版本库状态，版本回退，管理修改，撤销修改，删除修改等。

### 版本库状态
使用`git status` 显示工作区状态。
```
$ pwd
/home/jason/Repository/hello-world
$ git status
位于分支 master
无文件要提交，干净的工作区
```

对目录`hello-world`的`txt_1.txt`进行修改：
```
txt_1 is modified.
```

使用`git status`，显示`txt_1.txt`被修改了。  
但是已修改的`txt_1.txt`未添加到版本库中，同时无法得知具体的修改。
```
$ pwd
/home/jason/Repository/hello-world
$ git status
位于分支 master
尚未暂存以备提交的变更：
  （使用 "git add <文件>..." 更新要提交的内容）
  （使用 "git checkout -- <文件>..." 丢弃工作区的改动）

	修改：     txt_1.txt

修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
```

使用`git diff`显示版本库中和工作区之间等的差异。  
根据输出，得知`txt_1.txt`当前版本与上一个在版本库中提交版本的具体修改。
```
$ pwd
/home/jason/Repository/hello-world
$ git diff
diff --git a/txt_1.txt b/txt_1.txt
index cc4de1c..d7a0f81 100644
--- a/txt_1.txt
+++ b/txt_1.txt
@@ -1 +1 @@
-txt_1
+txt_1 is modified.
```

使用`git add <pathspec>`把当前版本的`txt_1.txt`添加到版本库。  
使用`git status`，显示`txt_1.txt`被修改了。与上文相比，少了最后一行输出。
> 修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）

使用`git diff`，没有任何输出，得知版本库中与工作区间没有差异。
```
$ pwd
/home/jason/Repository/hello-world
$ git add txt_1.txt 
$ git status
位于分支 master
要提交的变更：
  （使用 "git reset HEAD <文件>..." 以取消暂存）

	修改：     txt_1.txt
$ git diff
```

使用`git commit -m <msg>`提交修改。
使用`git status`显示工作区已没有文件被修改。
使用`git diff`，再次没有任何输出。
```
$ pwd
/home/jason/Repository/hello-world
$ git commit -m 'modify txt_1'
 [master 5cf1eba] modify txt_1
 1 file changed, 1 insertion(+), 1 deletion(-)
$ git status
位于分支 master
无文件要提交，干净的工作区
$ git diff
```

### 版本回退
对目录`hello-world`的`txt_1.txt`进行再次修改：
```
txt_1 is modified again.
```

添加已修改的`txt_1.txt`到版本库并提交。
```
$ pwd
/home/jason/Repository/hello-world
$ git add txt_1.txt 
$ git commit -m 'modify txt_1 again'
 [master 1659240] modify txt_1 again
 1 file changed, 1 insertion(+), 1 deletion(-)
```

使用`git log`查看版本库在过去进行修改的历史记录。
```
$ pwd
/home/jason/Repository/hello-world
$ git log
commit 165924055d0d5c4760d7fce9b994cd1846cd1cfd (HEAD -> master)
Author: jason <jason@Jason.PC>
Date:   Sat Nov 18 22:12:36 2017 +0800

    modify txt_1 again

commit 5cf1eba7b419c5149c6b094e6e375736b59b66f9
Author: jason <jason@Jason.PC>
Date:   Sat Nov 18 22:02:58 2017 +0800

    modify txt_1

commit 8c7be406876ddbd4a9ec9a0b5a9200ac0ab9002e
Author: jason <jason@Jason.PC>
Date:   Sat Nov 18 19:53:58 2017 +0800

    add two files: txt_2.txt, txt_3.txt

commit d78f63a66f0848a7ed4ede10aa2b82e7e2a1e303
Author: jason <jason@Jason.PC>
Date:   Sat Nov 18 19:40:52 2017 +0800

    wrote a txt file txt_1.txt and reset anthor
(END)
```

可使用`git log --pretty=oneline`仅输出`commit id`和`commit msg`。  
`commit id `是通过SHA1计算得到的十六进制数，可理解为版本库的版本号。  
`commit msg`是关于该提交的说明。  
`HEAD`指当前版本库为该提交记录提交后的版本库，可以理解为指向版本库历史的指针。
```
$ pwd
/home/jason/Repository/hello-world
$ git log
165924055d0d5c4760d7fce9b994cd1846cd1cfd (HEAD -> master) modify txt_1 again
5cf1eba7b419c5149c6b094e6e375736b59b66f9 modify txt_1
8c7be406876ddbd4a9ec9a0b5a9200ac0ab9002e add two files: txt_2.txt, txt_3.txt
d78f63a66f0848a7ed4ede10aa2b82e7e2a1e303 wrote a txt file txt_1.txt and reset anthor
```

使用`git reset [--soft | --mixed [-N] | --hard | --merge | --keep] [<commit>]`进行版本回退。  
**当前`--hard`选项是指回退，同时工作区移动至点。**
`[<commit>]`项可输入历史记录中版本对应的`commit id`。  
`HEAD^`指当前上一个版本的`commit id`。
```
$ pwd
/home/jason/Repository/hello-world
$ git reset --hard HEAD^
HEAD 现在位于 5cf1eba modify txt_1
$ cat txt_1.txt
txt_1 is modified.
```

使用`git log --pretty=oneline`查看回退后的历史记录。  
与回退前相比，少了一个记录，版本实现回退。
> 165924055d0d5c4760d7fce9b994cd1846cd1cfd (HEAD -> master) modify txt_1 again

同时`HEAD`移动到`modify txt_1`的记录上。
```
$ pwd
/home/jason/Repository/hello-world
$ git log --pretty=oneline
5cf1eba7b419c5149c6b094e6e375736b59b66f9 (HEAD -> master) modify txt_1
8c7be406876ddbd4a9ec9a0b5a9200ac0ab9002e add two files: txt_2.txt, txt_3.txt
d78f63a66f0848a7ed4ede10aa2b82e7e2a1e303 wrote a txt file txt_1.txt and reset anthor
```

使用`git reset --hard [<commit>]`，  
其中`[<commit>]`项输入`8c7be40`，  
实现版本回退至`add two files: txt_2.txt, txt_3.txt`。
```
$ pwd
/home/jason/Repository/hello-world
$ git reset --hard 8c7be40
HEAD 现在位于 8c7be40 add two files: txt_2.txt, txt_3.txt
$ git log --pretty=oneline
8c7be406876ddbd4a9ec9a0b5a9200ac0ab9002e (HEAD -> master) add two files: txt_2.txt, txt_3.txt
d78f63a66f0848a7ed4ede10aa2b82e7e2a1e303 wrote a txt file txt_1.txt and reset anthor
```

可见，版本回退同时，`git log`的历史记录同步回退，因此部分记录将丢失。
使用`git reflog`可查所有在版本库上进行的`commit`或`reset`操作。
```
$ pwd
/home/jason/Repository/hello-world
$ git reflog
8c7be40 (HEAD -> master) HEAD@{0}: reset: moving to 8c7be40
5cf1eba HEAD@{1}: reset: moving to HEAD^
1659240 HEAD@{2}: commit: modify txt_1 again
5cf1eba HEAD@{3}: commit: modify txt_1
8c7be40 (HEAD -> master) HEAD@{4}: commit: add two files: txt_2.txt, txt_3.txt
d78f63a HEAD@{5}: commit (amend): wrote a txt file txt_1.txt and reset anthor
bd9fb3c HEAD@{6}: commit (initial): wrote a txt file txt_1.txt
```

根据`git reflog`的记录，把版本回退至`modify txt_1 again`。  
此处使用`git reset --hard HEAD@{2}`替代`git reset --hard 1659240`亦可。
```
$ pwd
/home/jason/Repository/hello-world
$ git reset --hard 1659240
HEAD 现在位于 1659240 modify txt_1 again
$ git reflog
1659240 (HEAD -> master) HEAD@{0}: reset: moving to 1659240
8c7be40 (HEAD -> master) HEAD@{1}: reset: moving to 8c7be40
5cf1eba HEAD@{2}: reset: moving to HEAD^
1659240 HEAD@{3}: commit: modify txt_1 again
5cf1eba HEAD@{4}: commit: modify txt_1
8c7be40 (HEAD -> master) HEAD@{5}: commit: add two files: txt_2.txt, txt_3.txt
d78f63a HEAD@{6}: commit (amend): wrote a txt file txt_1.txt and reset anthor
bd9fb3c HEAD@{7}: commit (initial): wrote a txt file txt_1.txt
```

### 工作区与暂存区
前文提到，版本库是版本的历史仓库。  
版本库的工作区是指版本库所管理的目录。  
这也是每段代码都有`pwd`的原因。  
版本库的暂存区是指在当前版本基础上已进行`git add <file>`所添加的文件的修改，未进行`git commit -m <msg>`提交的版本。  
添加文件到版本库实际是添加文件到版本库的暂存中。

#### 工作区
在`hello-world`目录下修改`txt_2.txt`，创建`txt_4.txt`。  
使用`git status`，  
`txt_2.txt`的对应输出是：
> 尚未暂存以备提交的变更
  
`txt_4.txt`的对应输出是：
> 未跟踪的文件

其中，`txt_2.txt`是版本库当前版本已添加的文件，  
`txt_4.txt`则是版本当前版本未添加的文件。  
由于两个文件都在`hello-world`的目录，即工作区中。  
因此`git status`输出的是工作区上的状态。
```
$ pwd
/home/jason/Repository/hello-world
$ git status
位于分支 master
尚未暂存以备提交的变更：
  （使用 "git add <文件>..." 更新要提交的内容）
  （使用 "git checkout -- <文件>..." 丢弃工作区的改动）

	修改：     txt_2.txt

未跟踪的文件:
  （使用 "git add <文件>..." 以包含要提交的内容）

	txt_4.txt
```

#### 暂存区
根据提示，把`txt_2.txt`和`txt_4.txt`添加到版本库的暂存区。  
使用`git status`，两者输出都是
> **要提交的变更**

而不是
> *尚未暂存以备提交的变更*

与
> *未跟踪的文件*

具体显示了`txt_2.txt`是修改，`txt_4.txt`是新文件。
```
$ pwd
/home/jason/Repository/hello-world
$ git add txt_2.txt txt_4.txt
$ git status
位于分支 master
要提交的变更：
  （使用 "git reset HEAD <文件>..." 以取消暂存）

	修改：     txt_2.txt
	新文件：   txt_4.txt
```

把已添加`txt_2.txt`和`txt_4.txt`的暂存区版本进行提交。  
使用`git status`显示，工作区与版本库当前版本一致，即**干净**的工作区。
```
$ pwd
/home/jason/Repository/hello-world
$ git commit -m 'modify txt_2 and wrote txt_4'
 [master 66fe425] modify txt_2 and wrote txt_4
 2 files changed, 2 insertions(+), 1 deletion(-)
 create mode 100644 txt_4.txt
$ git status
位于分支 master
无文件要提交，干净的工作区
```

以内存、硬盘缓存、硬盘近似比喻版本库的工作区、暂存区、当前版本的关系。  
对硬盘的文件进行修改，首先把该文件储存至内存，  
如未修改的工作区实际就是当前版本，即干净的工作区是指文件目录中的状态与版本库当前一致。  
然后把经过修改的文件写入硬盘缓存，如把工作区的修改添加到暂存区。  
最后把硬盘缓存的结果写入硬盘，如把暂存区提交至版本库，创建新版本。
区别在于：实际写入硬盘缓存的修改是不可撤销的，而暂存区的修改是可撤销的。

### 管理修改
git管理的是文件的修改，不是文件本身。  
在`hello-world`目录下修改`txt_3.txt`，如下：
```
txt_3 is modified.
```

添加修改，查看状态。
```
$ pwd
/home/jason/Repository/hello-world
$ git add txt_3.txt
$ git status
位于分支 master
要提交的变更：
  （使用 "git reset HEAD <文件>..." 以取消暂存）

	修改：     txt_3.txt
```

再次修改`txt_3.txt`，如下：
```
txt_3 is modified again.
```

不添加第二次修改进行提交，查看状态可见存在未添加的修改。
```
$ git commit -m 'modify txt_3'
$ git status
位于分支 master
尚未暂存以备提交的变更：
  （使用 "git add <文件>..." 更新要提交的内容）
  （使用 "git checkout -- <文件>..." 丢弃工作区的改动）

	修改：     txt_3.txt

修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
$ git diff -- txt_3.txt
diff --git a/txt_3.txt b/txt_3.txt
index e3916a1..6451ff9 100644
--- a/txt_3.txt
+++ b/txt_3.txt
@@ -1 +1 @@
-txt_3 is modified.
+txt_3 is modified again.
```

### 撤销修改
使用`git checkout -- <paths>`撤销已在工作区发生，未添加至暂存区的修改。  
与`git add <file>`同样遵循：
> “没有消息就是好消息。”——*Unix的哲学*
```
$ pwd
/home/jason/Repository/hello-world
$ cat txt_3.txt
txt_3 is modified again.
$ git status
位于分支 master
尚未暂存以备提交的变更：
  （使用 "git add <文件>..." 更新要提交的内容）
  （使用 "git checkout -- <文件>..." 丢弃工作区的改动）

	修改：     txt_3.txt

修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
$ git checkout -- txt_3.txt
$ cat txt_3.txt
txt_3 is modified.
$ git status
位于分支 master
无文件要提交，干净的工作区
```

对`hello-world`目录下`txt_3.txt`再次修改，并把修改添加至版本库。  
使用`git reset HEAD <paths>`将暂存区的指定文件回退至当前版本， 即撤销已添加至暂存区的修改，但不撤销工作区的修改。  
组合使用`git checkout -- <paths>`和`git reset HEAD <paths>`可撤销未提交的修改。
```
$ pwd
/home/jason/Repository/hello-world
$ cat txt_3.txt
txt_3 is modified again.
$ git add txt_3.txt
$ git status
位于分支 master
要提交的变更：
  （使用 "git reset HEAD <文件>..." 以取消暂存）

	修改：     txt_3.txt
$ git reset HEAD txt_3.txt
重置后取消暂存的变更：
M	txt_3.txt
$ git status
位于分支 master
尚未暂存以备提交的变更：
  （使用 "git add <文件>..." 更新要提交的内容）
  （使用 "git checkout -- <文件>..." 丢弃工作区的改动）

	修改：     txt_3.txt

修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
$ git checkout -- txt_3.txt
$ git status
位于分支 master
无文件要提交，干净的工作区
```
### 删除文件
把删除文件的修改添加至暂存区有如下两种方式：

* 组合使用`rm [file]`与`git rm <file>`
* 直接使用`git rm <file>`

组合使用`rm [file]`与`git rm <file>`实际与修改文件一致。  
`rm [file]`相当于对工作区进行修改。  
`gir rm <file>`相当于把工作区的修改添加至暂存区。
```
$ pwd
/home/jason/Repository/hello-world
$ rm txt_4.txt
$ ls
txt_1.txt  txt_2.txt  txt_3.txt
$ git status
位于分支 master
尚未暂存以备提交的变更：
  （使用 "git add/rm <文件>..." 更新要提交的内容）
  （使用 "git checkout -- <文件>..." 丢弃工作区的改动）

	删除：     txt_4.txt

修改尚未加入提交（使用 "git add" 和/或 "git commit -a"）
$ git rm txt_4.txt
rm 'txt_4.txt'
$ git status
位于分支 master
要提交的变更：
  （使用 "git reset HEAD <文件>..." 以取消暂存）

	删除：     txt_4.txt
$ git commit -m 'delete txt_4.txt'
 [master 2daa693] delete txt_4.txt
 1 file changed, 1 deletion(-)
 delete mode 100644 txt_4.txt
```

首先使用`git reset <commit>`与`git checkout -- <paths>`还原`txt_4.txt`。  
然后直接使用`git rm <file>`添加删除至暂存区。  
直接使用`git rm <file>`实际与组合使用`rm [file]`与`git rm <file>`没有区别。  
考虑到工作区、暂存区、当前版本的关系，个人建议采取第一种方式删除文件。
```
$ pwd
/home/jason/Repository/hello-world
$ git reset HEAD^
重置后取消暂存的变更：
D	txt_4.txt
$ git checkout -- txt_4.txt
$ ls
txt_1.txt  txt_2.txt  txt_3.txt  txt_4.txt
$ git rm txt_4.txt 
rm 'txt_4.txt'
$ ls
txt_1.txt  txt_2.txt  txt_3.txt
$ 位于分支 master
要提交的变更：
  （使用 "git reset HEAD <文件>..." 以取消暂存）
$ git commmit -m 'delete txt_4.txt'
 [master dda03d5] delete txt_4.txt
 1 file changed, 1 deletion(-)
 delete mode 100644 txt_4.txt
```

## 远程仓库
本地git仓库和远程[github](https://github.com)仓库之间的传输是可以通过SSH加密的。  
首先根据github官方的引导[checking-for-existing-ssh-keys](https://help.github.com/articles/checking-for-existing-ssh-keys)使用`ls -al ~/.ssh`检查是否已存在**SSH key**  。  
如果`.ssh`目录已存在，同时目录下存在类似`id_rsa.pub`和`id_rsa`的文件，表示已存在**SSH key**。  
前者`id_rsa.pub`为公钥，后者`id_rsa`为私钥。  
可见我的砖头是没有**SSH key**的。。。
```
$ ls -al ~./ssh
ls: 无法访问'~./ssh': No such file or directory
```

然后根据github官方的引导[generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)创建**SSH key**。
使用`ssh-keygen`创建，根据提示直接`<enter>`确定新建的**SSH key**保存目录。  
默认保存目录为`/home/$LOGNAME/.ssh/id_rsa`。
```
$ ssh-keygen -t rsa -b 4096 -C "623409222@qq.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/jason/.ssh/id_rsa): 
Created directory '/home/jason/.ssh'.
```

更进一步，可以为新建的**SSH key**添加密码`passphrase`。  
此处我不添加了，毕竟我的砖头也马上要退役了，直接`<enter>`。
```
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/jason/.ssh/id_rsa.
Your public key has been saved in /home/jason/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:***************************************************
623409222@qq.com
The key's randomart image is:
+---[RSA 4096]----+
|                 |
|                 |
|                 |
|                 |
|                 |
|                 |
|                 |
|                 |
|                 |
+----[SHA256]-----+
```

再次使用`ls -al ~/.ssh`查看创建是否成功。  
创建成功后，把新建的**SSH key**添加至`ssh-agent` 。
```
$ ls -al ~/.ssh
总用量 16
drwx------.  2 jason jason 4096 11月 22 17:49 .
drwx------. 41 jason jason 4096 11月 22 17:48 ..
-rw-------.  1 jason jason 3243 11月 22 17:49 id_rsa
-rw-r--r--.  1 jason jason  742 11月 22 17:49 id_rsa.pub
$ eval "$(ssh-agent -s)"
Agent pid ****
$ ssh-add ~/.ssh/id_rsa
Identity added: /home/jason/.ssh/id_rsa (/home/jason/.ssh/id_rsa)
```

最后根据github官方的引导[adding-a-new-ssh-key-to-your-github-account](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account)把新建的**SSH key**添加至github账号。  
砖头是Fedora的，因此使用`dnf install -y xclip`安装`xclip`。  
```
$ sudo dnf install -y xclip
```

执行`xclip -sel clip < ~/.ssh/id_rsa.pub`，粘贴板将复制`id_rsa.pub`的内容。  
个人的做法是直接`cat ~./ssh/id_rsa.pub`复制的。  
暂时不了解两者区别。
```
$ xclip -sel clip < ~/.ssh/id_rsa.pub
 # or
$ cat ~/.ssh/id_rsa.pub
```

粘贴板已存有`id_rsa.pub`的内容，打开[github设置SSH key](https://github.com/settings/keys)的页面，点击按钮`New SSH key`。

![adding-a-new-ssh-key-to-your-github-account-1.png](./adding-a-new-ssh-key-to-your-github-account-1.png)

在`key`输入框中粘贴`id_rsa.pub`的内容，点击按钮`Add SSH key`。

![adding-a-new-ssh-key-to-your-github-account-2.png](./adding-a-new-ssh-key-to-your-github-account-2.png)

添加成功！

![adding-a-new-ssh-key-to-your-github-account-3.png](./adding-a-new-ssh-key-to-your-github-account-3.png)
