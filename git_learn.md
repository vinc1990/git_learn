### 安装git

- [git下载页面](https://git-scm.com/downloads)下载对应平台的安装程序
- linux下可以使用`sudo apt-get install git`命令安装
- Mac下可以使用`brew install git`命令安装
### 设置用户和邮箱
安装完git后，在命令行输入
```
$ git config --global user.name "your name"
$ git config --global user.email "your email"
```

设置完成后，通过在命令行输入下列命令来查看是否设置正确

```
$ git config user.name
$ git config user.email
```

### 创建版本库

```
$ mkdir learngit
$ cd learngit
$ git init
Initialized empty Git repository in C:/Users/xxx/Desktop/learngit/.git/
```

通过以上命令即可将本地目录变成git管理的空仓库。其中`.git`目录是来跟踪管理版本库的，没事不要动手修改里面的文件，否则会破坏git仓库。

### 添加文件到版本库

所有的版本控制系统只能跟踪文本文件的改动，比如TXT文件，网页，程序代码等等。对于这些文件，版本控制系统可以告诉你每次的具体改动；而对于图片、视频和word这些二进制的文件，版本控制系统不能跟踪具体变化。

新建一个文件`readme.txt`并添加内容，`readme.txt`文件一定要在`learngit`目录下（子目录也可以）

```
$ touch readme.txt
$ vim readme.txt
```

将一个文件放到git仓库只需下面两步。

1. 用`git add`命令将文件添加到仓库，使用`git add .`可以添加所有修改

```
$ git add readme.txt
```

2. 用命令`git commit`命令提交到仓库
```
$ git commit -m "wrote a readme file"
[master (root-commit) ddfb4e9] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
```

`-m`后面输入本次提交的说明

`git commit`命令执行成功后会显示，`1 file changed`：1个文件被改动；`2 insertions(+)`：插入了两行内容

通过上面两步已经成功添加并提交了一个`readme.txt`文件，继续修改里面的内容

```
Git is a distributed version control system.
Git is free software.
```

运行`git status`命令查看结果

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

`git status`命令可以让我们时刻掌握仓库的状态，上面命令输出显示`readme.txt`已经修改，但是还没有提交修改

如果想查看修改的具体内容，可以通过`git diff`命令查看

```
$ git diff readme.txt
warning: LF will be replaced by CRLF in readme.txt.
The file will have its original line endings in your working directory
diff --git a/readme.txt b/readme.txt
index 46d49bf..9247db6 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,2 @@
-Git is a version control system.
+Git is a distributed version control system.
 Git is free software.
```

提交修改

```
$ git add readme.txt
$ git commit -m "add distributed"
```

### 版本回退

使用`git log`或`git log --pretty=oneline`查看提交日志

```
$ git log
commit 873ff020d9efae0c65d300ac4f58e9e0e48a394e (HEAD -> master)
Author: vinc1990 <vinc2012@163.com>
Date:   Thu Nov 29 11:18:00 2018 +0800

    add diatributed

commit ddfb4e934708c7f7720ac3b31cbb02281f60f020
Author: vinc1990 <vinc2012@163.com>
Date:   Thu Nov 29 10:50:47 2018 +0800

    wrote a readme file
    
$ git log --pretty=oneline
873ff020d9efae0c65d300ac4f58e9e0e48a394e (HEAD -> master) add diatributed
ddfb4e934708c7f7720ac3b31cbb02281f60f020 wrote a readme file
```

其中`HEAD`表示当前版本，ID为`873ff020d9efae0c65d300ac4f58e9e0e48a394e`

回退到上一个版本使用`git reset`命令，上一次版本是`HEAD^`，上上一个版本就是`HEAD^^`，100个版本就是`HEAD~100`

```
$ git reset --hard HEAD^ 或 git reset --hard ddfb4e9
HEAD is now at ddfb4e9 wrote a readme file
$ cat readme.txt
Git is a version control system.
Git is free software.
```

如果想回退到刚才的版本，可以通过`commit id`来指定

```
$ git reset --hard 873ff02
$ cat readme.txt
Git is a distributed version control system.
Git is free software.
```

`873ff02`可以通过`git reflog`命令来查看，此命令记录每一条的操作

```
$ git reflog
873ff02 (HEAD -> master) HEAD@{0}: reset: moving to 873ff02
ddfb4e9 HEAD@{1}: reset: moving to ddfb4e93
873ff02 (HEAD -> master) HEAD@{2}: reset: moving to 873ff020
ddfb4e9 HEAD@{3}: reset: moving to HEAD^
873ff02 (HEAD -> master) HEAD@{4}: commit: add diatributed
ddfb4e9 HEAD@{5}: commit (initial): wrote a readme file
```

### 工作区和暂存区

- 工作区就是你电脑上的目录，比如`learngit`

- `.git`是git的版本库，里面有很多的东西，其中最重要的是称为stage（或者index）的暂存区，还有git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针`HEAD`

![](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384907702917346729e9afbf4127b6dfbae9207af016000/0)

文件添加到git版本库的两步：

第一步用`git add`将文件添加进去，实际上就是把文件修改添加到暂存区

第二步用`git commit`提交修改，实际上就是把暂存区的所有内容提交到当前分支

### 撤销修改

#### 撤销工作区的修改

使用命令`git checkout -- file`可以丢弃工作区的修改

```
$ git checkout -- readme.txt  将工作区的修改全部撤销
```

两种情况：

1. 修改后没有添加到暂存区，撤销后就回到和版本库一模一样的状态

2. 修改后已经添加到暂存区，又做了修改，撤销后就回到添加到暂存区后的状态

总之，就是回到最近一次`git add`时的状态

#### 撤销暂存区的修改

```
$ git reset HEAD readme.txt
```

使用命令`git reset HEAD <file>`可以把暂存区的修改撤销掉，重新放回工作区，再使用`git checkout -- file`丢弃工作区

```
$ git checkout -- readme.txt  
```

### 删除文件

如果在工作区将一个文件删除，比如`test.txt`

```github
$ rm test.txt
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        deleted:    test.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

如果删除操作是我们想要的，可以通过下面命令提交即可将版本库中文件删除

```
$ git rm test.txt
$ git commit -m "delete test.txt"
```

如果是误操作，可以通过以下命令恢复

```
$ git checkout -- test.txt
```

此命令无论工作区是修改还是删除，都可以还原。

