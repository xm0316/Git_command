# 廖雪峰的Git教程

## Git的简介

### Git是什么？

Git是目前世界上最先进的分布式版本控制系统（没有之一）。

C语言编写的软件. 2008年，GitHub网站上线了，它为开源项目免费提供Git存储，无数开源项目开始迁移至GitHub，包括jQuery，PHP，Ruby等等。

### 集中式vs分布式

先说集中式版本控制系统，版本库是**集中存放在中央服务器**的，而干活的时候，用的都是自己的电脑，所以要先从中央服务器取得最新的版本，然后开始干活，干完活了，再把自己的活推送给中央服务器。中央服务器就好比是一个图书馆，你要改一本书，必须先从图书馆借出来，然后回到家自己改，改完了，再放回图书馆。

集中式版本控制系统**最大的毛病就是必须联网**才能工作.

分布式版本控制系统根本**没有“中央服务器”**，每个人的电脑上都是一个完整的版本库, 互相工作不影响; **系统的安全性**高很多. 

分布式版本控制系统通常也有一台充当“中央服务器”的电脑，但这个服务器的作用仅仅是用来方便“交换”大家的修改，没有它大家也一样干活，只是交换修改不方便而已。

Git 有着极其强大的**分支管理**.

分布式版本控制系统除了Git以及促使Git诞生的BitKeeper外，还有类似Git的Mercurial和Bazaar等。这些分布式版本控制系统各有特点，但**最快、最简单也最流行**的依然是Git！

## Git 安装

- 在Linux上安装

首先，你可以试着输入`git`，看看系统有没有安装Git：

如果你碰巧用Debian或Ubuntu Linux，通过一条`sudo apt-get install git`就可以直接完成Git的安装，非常简单。

如果是其他Linux版本，可以直接通过源码安装。先从Git官网下载源码，然后解压，依次输入：`./config`，`make`，`sudo make install`这几个命令安装就好了。

- 在 Mac OS上安装

一是安装homebrew，然后通过homebrew安装Git，具体方法请参考homebrew的文档：http://brew.sh/。

第二种方法更简单，也是推荐的方法，就是直接从AppStore安装Xcode，Xcode集成了Git，不过默认没有安装，你需要运行Xcode，选择菜单“Xcode”->“Preferences”，在弹出窗口中找到“Downloads”，选择“Command Line Tools”，点“Install”就可以完成安装了。

### 在Windows上安装Git

在Windows上使用Git，可以从Git官网直接[下载安装程序](https://git-scm.com/downloads)，然后按默认选项安装即可。

安装完成后，在开始菜单里找到“Git”->“Git Bash”，蹦出一个类似命令行窗口的东西，就说明Git安装成功！

#### 设置

安装完成后，还需要最后一步设置，在命令行输入：

```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和Email地址。

注意`git config`命令的`--global`参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。

### **本地仓库与Github关联**

在你的c盘下面有一个.ssh文件夹，进入文件夹里面可以看到有`id_rsa.pub`和`id_rsa`两个文件，第一个文件是id_rsa.pub里面的信息是公钥，而第二个文件是私钥。

加入没有这两个文件，可以使用一下命令进行生成：

```text
$ ssh-keygen -t rsa -C "你注册的邮箱"
```

接着就是把自己的公钥复制粘贴配置到Github上的`SSH Keys`页面中，快捷地址：https://github.com/settings/ssh ，

<img src="https://gitee.com/xm0316/drawingbed/raw/master/202308120303029.jpg" alt="img" style="zoom: 33%;" />

<img src="https://gitee.com/xm0316/drawingbed/raw/master/202308120303030.jpg" alt="img" style="zoom:67%;" />

在Github上配置完自己的公钥后，就可以在Github中创建仓库进行测试，在Github的右上角中找到：`create a new repo`，创建一个新的仓库：

<img src="https://gitee.com/xm0316/drawingbed/raw/master/202308120303031.jpg" alt="img" style="zoom:67%;" />

这样就简单的创建自己的Github的仓库了，创建完后就可以把自己的本地仓库文件同步到GitHub中，使用一下命令：

```text
git remote add origin https://github.com/liduchang/redis.git
git push -u origin master（由于新建的GitHub仓库是空的，所以第一次推送master分支时需要加-u参数，以后再推送就不用加了）
```

这样你本地的Reids目录下的文件与Github进行了关联，只要在Redis目录中修改了文件，就可以使用`git push origin master`推向远程的Github仓库。

这有一点说明的就是这里配置的是https的方式，可以配置成ssh的方式，因为http上的方式每次推向远程仓库的时候都会让你输入密码，有点麻烦：

![](https://gitee.com/xm0316/drawingbed/raw/master/202308120303032.jpg)

切换的方法，如下图所示，只要跟着下面的命令进行操作就能随意进行协议的切换了，还是比较简单的，这里就直接略过：

<img src="https://gitee.com/xm0316/drawingbed/raw/master/202308120303033.jpg" alt="img" style="zoom:67%;" />



## 创建版本库

什么是版本库呢？版本库又名仓库，英文名**repository**，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

第一步，选择一个合适的地方，创建一个空目录：

```sh
$ mkdir learngit
$ cd learngit
$ pwd
/Users/michael/learngit
```

`pwd`命令用于显示当前目录。在我的Mac上，这个仓库位于`/Users/michael/learngit`。

>  如果你使用Windows系统，为了避免遇到各种莫名其妙的问题，请确保目录名（包括父目录）不包含中文。

第二步，通过`git init`命令把这个目录变成Git可以管理的仓库：

```sh
$ git init
Initialized empty Git repository in /Users/michael/learngit/.git/
```

瞬间Git就把仓库建好了，而且告诉你是一个空的仓库（empty Git repository），细心的读者可以发现当前目录下多了一个`.git`的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。

如果你没有看到`.git`目录，那是因为这个目录默认是隐藏的，用`ls -ah`命令就可以看见。

<img src="https://gitee.com/xm0316/drawingbed/raw/master/202308120303034.jpg" alt="img" style="zoom: 50%;" />



### 把文件添加到版本库

首先这里再明确一下，所有的版本控制系统，其实只能跟踪文本文件的改动，比如TXT文件，网页，所有的程序代码等等，Git也不例外。版本控制系统可以告诉你每次的改动，比如在第5行加了一个单词“Linux”，在第8行删了一个单词“Windows”。而**图片、视频这些二进制文件**，虽然也能由版本控制系统管理，但**没法跟踪文件的变化**，只能把二进制文件每次改动串起来，也就是只知道图片从100KB改成了120KB，但到底改了啥，版本控制系统不知道，也没法知道。

不幸的是，Microsoft的Word格式是二进制格式，因此，版本控制系统是没法跟踪Word文件的改动的，前面我们举的例子只是为了演示，如果要真正使用版本控制系统，就要以纯文本方式编写文件。

因为文本是有编码的，比如中文有常用的GBK编码，日文有Shift_JIS编码，如果没有历史遗留问题，强烈建议**使用标准的UTF-8编码**，所有语言使用同一种编码，既没有冲突，又被所有平台所支持。

使用Windows的童鞋要特别注意：

千万不要使用Windows自带的**记事本**编辑任何文本文件。原因是Microsoft开发记事本的团队使用了一个非常弱智的行为来保存UTF-8编码的文件，他们自作聪明地在每个文件开头添加了0xefbbbf（十六进制）的字符，你会遇到很多不可思议的问题，比如，网页第一行可能会显示一个“?”，明明正确的程序一编译就报语法错误，等等，都是由记事本的弱智行为带来的。建议你下载[Visual Studio Code](https://code.visualstudio.com/)代替记事本，不但功能强大，而且免费！

言归正传，现在我们编写一个`readme.txt`文件，内容如下：

```
Git is a version control system.
Git is free software.
```

一定要放到`learngit`目录下（子目录也行），因为这是一个Git仓库，放到其他地方Git再厉害也找不到这个文件。

和把大象放到冰箱需要3步相比，把一个文件放到Git仓库只需要两步。

第一步，用命令`git add`告诉Git，把文件**添加到仓库**：

```
$ git add readme.txt
```

执行上面的命令，没有任何显示，这就对了，Unix的哲学是“没有消息就是好消息”，说明添加成功。

第二步，用命令`git commit`告诉Git，把文件**提交到仓库**：

```
$ git commit -m "wrote a readme file"
[master (root-commit) eaadf4e] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
```

简单解释一下`git commit`命令，`-m`后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

嫌麻烦不想输入`-m "xxx"`行不行？确实有办法可以这么干，但是强烈不建议你这么干，因为输入说明对自己对别人阅读都很重要。实在不想输入说明的童鞋请自行Google，我不告诉你这个参数。

`git commit`命令执行成功后会告诉你，`1 file changed`：1个文件被改动（我们新添加的readme.txt文件）；`2 insertions`：插入了两行内容（readme.txt有两行内容）。

为什么Git添加文件需要`add`，`commit`一共两步呢？

因为`commit`可以一次提交很多文件，所以你可以多次`add`不同的文件，比如：

```
$ git add file1.txt
$ git add file2.txt file3.txt
$ git commit -m "add 3 files."
```

### 疑难解答

Q：输入`git add readme.txt`，得到错误：`fatal: not a git repository (or any of the parent directories)`。

A：Git命令必须**在Git仓库目录内**执行（`git init`除外），在仓库目录外执行是没有意义的。

Q：输入`git add readme.txt`，得到错误`fatal: pathspec 'readme.txt' did not match any files`。

A：添加某个文件时，该文件必须在当前目录下存在，用`ls`或者`dir`命令查看当前目录的文件，看看文件是否存在，或者是否写错了文件名。

### 小结

现在总结一下今天学的两点内容：

初始化一个Git仓库，使用`git init`命令。

添加文件到Git仓库，分两步：

1. 使用命令`git add <file>`，注意，可反复多次使用，添加多个文件；
2. 使用命令`git commit -m <message>`，完成。



## Git原理 - 时光机穿梭

### diff 文件比较

我们已经成功地添加并提交了一个readme.txt文件，现在，是时候继续工作了，于是，我们继续修改readme.txt文件，改成如下内容：

```
Git is a distributed version control system.
Git is free software.
```

现在，运行`git status`命令看看结果：

```sh
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

`git status`命令可以让我们**时刻掌握仓库当前的状态**，上面的命令输出告诉我们，`readme.txt`被修改过了，但还没有准备提交的修改。



虽然Git告诉我们`readme.txt`被修改了，但如果能**看看具体修改了什么内容**，自然是很好的。比如你休假两周从国外回来，第一天上班时，已经记不清上次怎么修改的`readme.txt`，所以，需要用`git diff`这个命令看看：

```sh
$ git diff readme.txt 
diff --git a/readme.txt b/readme.txt
index 46d49bf..9247db6 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,2 @@
-Git is a version control system.
+Git is a distributed version control system.
 Git is free software.
```

`git diff`顾名思义就是查看difference**(比较当前文件和本地仓库内文件的差异)**，显示的格式正是Unix通用的diff格式，可以从上面的命令输出看到，我们在第一行添加了一个`distributed`单词。

知道了对`readme.txt`作了什么修改后，再把它提交到仓库就放心多了，**提交修改和提交新文件是一样的两步**，第一步是`git add`：

```
$ git add readme.txt
```

同样没有任何输出。在执行第二步`git commit`之前，我们再运行`git status`看看当前仓库的状态：

```
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   readme.txt
```

`git status`告诉我们，将要被提交的修改包括`readme.txt`，下一步，就可以放心地提交了：

```
$ git commit -m "add distributed"
[master e475afc] add distributed
 1 file changed, 1 insertion(+), 1 deletion(-)
```

提交后，我们再用`git status`命令看看仓库的当前状态：

```
$ git status
On branch master
nothing to commit, working tree clean
```

Git告诉我们当前没有需要提交的修改，而且，工作区是干净（working tree clean）的。



#### 小结

- 要随时掌握工作区的状态，使用`git status`命令。

- 如果`git status`告诉你有文件被修改过，用`git diff`可以查看修改内容。

- `git diff`: 查看文件在工作目录与暂存区的差别。如果还没add 进暂存区，则查看文件自身修改前后的差别。

    

### reset --hard  版本回退

你不断对文件进行修改，然后不断提交修改到版本库里，每当你觉得文件修改到一定程度的时候，就可以“保存一个快照”，这个快照在Git中被称为`commit`。一旦你把文件改乱了，或者误删了文件，还可以从最近的一个`commit`恢复，然后继续工作，而不是把几个月的工作成果全部丢失。

现在，我们回顾一下`readme.txt`文件一共有几个版本被提交到Git仓库里了：

版本1：wrote a readme file

```
Git is a version control system.
Git is free software.
```

版本2：add distributed

```
Git is a distributed version control system.
Git is free software.
```

版本3：append GPL

```
Git is a distributed version control system.
Git is free software distributed under the GPL.
```

当然了，在实际工作中，我们脑子里怎么可能记得一个几千行的文件每次都改了什么内容，不然要版本控制系统干什么。版本控制系统肯定有某个命令可以告诉我们历史记录，在Git中，我们用`git log`命令查看：

```
$ git log
commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:06:15 2018 +0800

    append GPL

commit e475afc93c209a690c39c13a46716e8fa000c366
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:03:36 2018 +0800

    add distributed

commit eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 20:59:18 2018 +0800

    wrote a readme file
```

`git log`命令显示**从最近到最远的提交日志**，我们可以看到3次提交，最近的一次是`append GPL`，上一次是`add distributed`，最早的一次是`wrote a readme file`。

如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数：

```sh
$ git log --pretty=oneline
1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master) append GPL
e475afc93c209a690c39c13a46716e8fa000c366 add distributed
eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0 wrote a readme file
```

需要友情提示的是，你看到的一大串类似`1094adb...`的是`commit id`（版本号），和SVN不一样，Git的`commit id`不是1，2，3……递增的数字，而是一个`SHA1`计算出来的一个非常大的数字，用十六进制表示，而且你看到的`commit id`和我的肯定不一样，以你自己的为准。为什么`commit id`需要用这么一大串数字表示呢？因为Git是分布式的版本控制系统，后面我们还要研究多人在同一个版本库里工作，如果大家都用1，2，3……作为版本号，那肯定就冲突了。

每提交一个新版本，实际上Git就会把它们自动串成一条时间线。如果使用可视化工具查看Git历史，就可以更清楚地看到提交历史的时间线：

![git-log-timeline](https://gitee.com/xm0316/drawingbed/raw/master/202308120318756.jpeg)

好了，现在我们启动时光穿梭机，准备把`readme.txt`回退到上一个版本，也就是`add distributed`的那个版本，怎么做呢？

首先，Git必须知道当前版本是哪个版本，在Git中，用`HEAD`表示当前版本，也就是最新的提交`1094adb...`（注意我的提交ID和你的肯定不一样），上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`。

现在，我们要把当前版本`append GPL`回退到上一个版本`add distributed`，就可以使用`git reset`命令：

```
$ git reset --hard HEAD^
HEAD is now at e475afc add distributed
```

`--hard`参数有啥意义？这个后面再讲，现在你先放心使用。

看看`readme.txt`的内容是不是版本`add distributed`：

```
$ cat readme.txt
Git is a distributed version control system.
Git is free software.
```

果然被还原了。

还可以继续回退到上一个版本`wrote a readme file`，不过且慢，让我们用`git log`再看看现在版本库的状态：

```
$ git log
commit e475afc93c209a690c39c13a46716e8fa000c366 (HEAD -> master)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:03:36 2018 +0800

    add distributed

commit eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 20:59:18 2018 +0800

    wrote a readme file
```

最新的那个版本`append GPL`已经看不到了！好比你从21世纪坐时光穿梭机来到了19世纪，想再回去已经回不去了，肿么办？

办法其实还是有的，只要上面的命令行窗口还没有被关掉，你就可以顺着往上找啊找啊，找到那个`append GPL`的`commit id`是`1094adb...`，于是就可以指定回到未来的某个版本：

```
$ git reset --hard 1094a
HEAD is now at 83b0afe append GPL
```

版本号没必要写全，前几位就可以了，Git会自动去找。当然也不能只写前一两位，因为Git可能会找到多个版本号，就无法确定是哪一个了。

再小心翼翼地看看`readme.txt`的内容：

```
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
```

果然，我胡汉三又回来了。

Git的版本回退速度非常快，因为Git在内部有个指向当前版本的`HEAD`指针，当你回退版本的时候，Git仅仅是把HEAD从指向`append GPL`：

```ascii
┌────┐
│HEAD│
└────┘
   │
   └──▶ ○ append GPL
        │
        ○ add distributed
        │
        ○ wrote a readme file
```

改为指向`add distributed`：

```ascii
┌────┐
│HEAD│
└────┘
   │
   │    ○ append GPL
   │    │
   └──▶ ○ add distributed
        │
        ○ wrote a readme file
```

然后顺便把工作区的文件更新了。所以你让`HEAD`指向哪个版本号，你就把当前版本定位在哪。

现在，你回退到了某个版本，关掉了电脑，第二天早上就后悔了，想恢复到新版本怎么办？找不到新版本的`commit id`怎么办？

在Git中，总是有后悔药可以吃的。当你用`$ git reset --hard HEAD^`回退到`add distributed`版本时，再想恢复到`append GPL`，就必须找到`append GPL`的commit id。Git提供了一个命令`git reflog`用来记录你的每一次命令：

```
$ git reflog
e475afc HEAD@{1}: reset: moving to HEAD^
1094adb (HEAD -> master) HEAD@{2}: commit: append GPL
e475afc HEAD@{3}: commit: add distributed
eaadf4e HEAD@{4}: commit (initial): wrote a readme file
```

终于舒了口气，从输出可知，`append GPL`的commit id是`1094adb`，现在，你又可以乘坐时光机回到未来了。

#### 小结

现在总结一下：

- `HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。 工作区,暂存区, 本地版本库全部回退
- 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。
- 要重返未来，用`git reflog`查看命令历史记录，以便确定要回到未来的哪个版本。

    - `reflog`翻译：`Reference logs`（参考日志）

    -  `git reflog`命令：可以叫做显示可引用的历史版本记录。

    <img src="https://gitee.com/xm0316/drawingbed/raw/master/202308130039671.webp" alt="reflog和log" style="zoom:67%;" />

    

### 工作区和暂存区

Git和其他版本控制系统如SVN的一个不同之处就是有**暂存区(stage/index)**的概念。

#### 工作区（Working Directory）

就是你在电脑里能看到的目录，比如我的`.\git\`文件夹所在的文件目录就是一个工作区：

![image-20230812032932557](Git 的一些操作.assets/image-20230812032932557.png)



#### 版本库（Repository）

工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。**本地仓库**

Git的版本库里存了很多东西，其中最重要的就是**称为stage（或者叫index）**的`暂存区`，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。

![git-repo](https://gitee.com/xm0316/drawingbed/raw/master/202308120324582.jpeg)

分支和`HEAD`的概念我们以后再讲。

前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：

第一步是用`git add`把文件添加进去，实际上就是**把文件修改添加到暂存区**；

第二步是用`git commit`提交更改，实际上就是**把暂存区的所有内容提交到当前分支**。

因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，现在，`git commit`就是往`master`分支上提交更改。

你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

俗话说，实践出真知。现在，我们再练习一遍，先对`readme.txt`做个修改，比如加上一行内容：

```
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
```

然后，在工作区新增一个`LICENSE`文本文件（内容随便写）。

先用`git status`查看一下状态：

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	LICENSE

no changes added to commit (use "git add" and/or "git commit -a")
```

Git非常清楚地告诉我们，`readme.txt`被修改了，而`LICENSE`还从来没有被添加过，所以它的状态是`Untracked`。

现在，使用两次命令`git add`，把`readme.txt`和`LICENSE`都添加后，用`git status`再查看一下：

```
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   LICENSE
	modified:   readme.txt
```

现在，暂存区的状态就变成这样了：

![git-stage](https://gitee.com/xm0316/drawingbed/raw/master/202308120327659.jpeg)

所以，`git add`命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行`git commit`就可以一次性把暂存区的所有修改提交到分支。

```
$ git commit -m "understand how stage works"
[master e43a48b] understand how stage works
 2 files changed, 2 insertions(+)
 create mode 100644 LICENSE
```

一旦提交后，如果你又没有对工作区做任何修改，那么工作区就是“干净”的：

```
$ git status
On branch master
nothing to commit, working tree clean
```

现在版本库变成了这样，暂存区就没有任何内容了：

![git-stage-after-commit](https://gitee.com/xm0316/drawingbed/raw/master/202308120327090.jpeg)

#### 小结

暂存区(stage/index)是Git非常重要的概念，弄明白了暂存区，就弄明白了Git的很多操作到底干了什么。



### 管理修改

为什么Git比其他版本控制系统设计得优秀，因为Git跟踪并管理的是修改，而非文件。

你会问，什么是修改？比如你新增了一行，这就是一个修改，删除了一行，也是一个修改，更改了某些字符，也是一个修改，删了一些又加了一些，也是一个修改，甚至创建一个新文件，也算一个修改。

为什么说Git管理的是修改，而不是文件呢？我们还是做实验。第一步，对readme.txt做一个修改，比如加一行内容：

```
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes.
```

然后，**添加到暂存区**：

```
$ git add readme.txt
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       modified:   readme.txt
#
```

然后，再修改readme.txt：

```
$ cat readme.txt 
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
```

提交：

```
$ git commit -m "git tracks changes"
[master 519219b] git tracks changes
 1 file changed, 1 insertion(+)
```

提交后，再看看状态：

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

咦，怎么第二次的修改没有被提交？

别激动，我们回顾一下操作过程：

第一次修改 -> `git add` -> 第二次修改 -> `git commit`

你看，我们前面讲了，Git管理的是修改，当你用`git add`命令后，在工作区的第一次修改被放入暂存区，准备提交，但是，在工作区的第二次修改并没有放入暂存区，所以，`git commit`只**负责把暂存区的修改提交**了，也就是第一次的修改被提交了，第二次的修改不会被提交。

提交后，用`git diff HEAD -- readme.txt`命令可以**查看工作区和版本库里面最新版本**的区别：

```sh
$ git diff HEAD -- readme.txt 
diff --git a/readme.txt b/readme.txt
index 76d770f..a9c5755 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,4 +1,4 @@
 Git is a distributed version control system.
 Git is free software distributed under the GPL.
 Git has a mutable index called stage.
-Git tracks changes.
+Git tracks changes of files.
```

可见，第二次修改确实没有被提交。

那怎么提交第二次修改呢？你可以继续`git add`再`git commit`，也可以别着急提交第一次修改，先`git add`第二次修改，再`git commit`，就相当于把两次修改合并后一块提交了：第一次修改 -> `git add` -> 第二次修改 -> `git add` -> `git commit`，现在，把第二次修改提交了.

#### 小结

现在，你又理解了Git是如何跟踪修改的，每次修改，如果不通过`git add`到**暂存区**，那就不会加入到`commit`中。

`git diff HEAD -- readme.txt`命令可以**查看工作区和版本库里面最新版本**的区别.

`git diff ` 可以查看工作区和暂存区的区别.



### 撤销修改

自然，你是不会犯错的。不过现在是凌晨两点，你正在赶一份工作报告，你在`readme.txt`中添加了一行：

```sh
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
My stupid boss still prefers SVN.
```

在你准备提交前，一杯咖啡起了作用，你猛然发现了错误, `stupid boss`可能会让你丢掉这个月的奖金！

既然错误发现得很及时，就可以很容易地纠正它。你可以删掉最后一行，手动把文件恢复到上一个版本的状态。如果用`git status`查看一下：

    $ git status
    On branch master
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)
    
    	modified:   readme.txt
    
    no changes added to commit (use "git add" and/or "git commit -a")

你可以发现，Git会告诉你，`git checkout -- file`可以丢弃工作区的修改：

```
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```



```sh
$ git checkout -- readme.txt
$ git restore readme.txt
```

命令`git checkout -- readme.txt`意思就是，把`readme.txt`文件在工作区的修改全部撤销，这里有两种情况：

一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是**让这个文件回到最近一次**`git commit`或`git add`时的状态。

现在，看看`readme.txt`的文件内容：

    $ cat readme.txt
    Git is a distributed version control system.
    Git is free software distributed under the GPL.
    Git has a mutable index called stage.
    Git tracks changes of files.

文件内容果然复原了。

`git checkout --file`命令中的`--`很重要，没有`--`，就变成了“切换到另一个分支”的命令，我们在后面的分支管理中会再次遇到`git checkout`命令。

现在假定是凌晨3点，你不但写了一些胡话，还`git add`到暂存区了：

    $ cat readme.txt
    Git is a distributed version control system.
    Git is free software distributed under the GPL.
    Git has a mutable index called stage.
    Git tracks changes of files.
    My stupid boss still prefers SVN.
    
    $ git add readme.txt

庆幸的是，在`commit`之前，你发现了这个问题。用`git status`查看一下，修改只是添加到了暂存区，还没有提交：

```sh
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   readme.txt	
	
$ git status	
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   readme.txt
```

Git同样告诉我们，用命令`git reset HEAD <file>`可以把暂存区的修改撤销掉（unstage），重新放回工作区： `git restore  --staged  <file>...`

```sh
$ git reset HEAD readme.txt
Unstaged changes after reset:
M	readme.txt
```

`git reset`命令**既可以回退版本，也可以把暂存区的修改回退到工作区**。**<u>当我们用`HEAD`时，表示最新的版本</u>**。

再用`git status`查看一下，现在暂存区是干净的，工作区有修改：

    $ git status
    On branch master
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)
    
    	modified:   readme.txt

还记得如何丢弃工作区的修改吗？

```sh
$ git checkout -- readme.txt

$ git status
On branch master
nothing to commit, working tree clean
```

整个世界终于清静了！

现在，假设你不但改错了东西，还从暂存区提交到了版本库，怎么办呢？还记得[版本回退](/wiki/896043488029600/897013573512192)一节吗？可以回退到上一个版本。不过，这是有条件的，就是你还没有把自己的本地版本库推送到远程。还记得Git是分布式版本控制系统吗？我们后面会讲到远程版本库，一旦你把`stupid boss`提交推送到远程版本库，你就真的惨了……

#### `小结`

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file>` 丢弃暂存区添加的文件修改，就回到了场景1，第二步按场景1操作。

|                | 旧版本                   | 新版本                       |
| -------------- | ------------------------ | ---------------------------- |
| 丢弃工作区修改 | `git checkout -- <file>` | `git restore <file>`         |
| 丢弃暂存区修改 | `git reset HEAD <file>`  | `git restore --stage <file>` |

场景3：已经提交了不合适的修改到**版本库**时，想要撤销本次提交，参考[版本回退](/wiki/896043488029600/897013573512192)一节，不过前提是没有推送到远程库。 `丢弃版本库更改(版本回退):    git reset --hard <commit_id>`



### 删除文件

在Git中，删除也是一个修改操作，我们实战一下，先添加一个新文件`test.txt`到Git并且提交：


    $ git add test.txt
    
    $ git commit -m "add test.txt"
    [master b84166e] add test.txt
     1 file changed, 1 insertion(+)
     create mode 100644 test.txt

一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用`rm`命令删了： 

    $ rm test.txt

这个时候，Git知道你删除了文件，因此，工作区和版本库就不一致了，`git status`命令会立刻告诉你哪些文件被删除了： 

    $ git status
    On branch master
    Changes not staged for commit:
      (use "git add/rm <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)
    
    	deleted:    test.txt
    
    no changes added to commit (use "git add" and/or "git commit -a")

现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令`git rm <file>`删掉，并且`git commit`：

    $ git rm test.txt
    rm 'test.txt'
    
    $ git commit -m "remove test.txt"
    [master d46f35e] remove test.txt
     1 file changed, 1 deletion(-)
     delete mode 100644 test.txt

现在，文件就从版本库中被删除了。

> 小提示：先手动删除文件，然后使用`git rm <file>`和`git add<file>`效果是一样的。

另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：

    $ git checkout -- test.txt

`git checkout`其实是**用版本库里的版本替换工作区的版本**，无论工作区是修改还是删除，都可以“一键还原”。

> 注意：从来没有被添加到版本库就被删除的文件，是无法恢复的！

#### 小结

命令`git rm <file>`用于删除一个文件 (从版本库中删除), 并且该删除需要提交操作: `git commit -m "..."`。

如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失**最近一次提交后你对工作区修改的内容** 。



## 远程仓库

到目前为止，我们已经掌握了如何在Git仓库里对一个文件进行时光穿梭，你再也不用担心文件备份或者丢失的问题了。

可是有用过集中式版本控制系统SVN的童鞋会站出来说，这些功能在SVN里早就有了，没看出Git有什么特别的地方。没错，如果只是在一个仓库里管理文件历史，Git和SVN真没啥区别。

为了保证你现在所学的Git物超所值，将来绝对不会后悔，同时为了打击已经不幸学了SVN的童鞋，本章开始介绍Git的杀手级功能之一（注意是之一，也就是后面还有之二，之三……）：远程仓库。

Git是分布式版本控制系统，同一个Git仓库，可以分布到不同的机器上。

怎么分布呢？最早，肯定只有一台机器有一个原始版本库，此后，别的机器可以“克隆”这个原始版本库，而且每台机器的版本库其实都是一样的，并没有主次之分。

你肯定会想，至少需要两台机器才能玩远程库不是？但是我只有一台电脑，怎么玩？

其实一台电脑上也是可以克隆多个版本库的，只要不在同一个目录下。不过，现实生活中是不会有人这么傻的在一台电脑上搞几个远程库玩，因为一台电脑上搞几个远程库完全没有意义，而且硬盘挂了会导致所有库都挂掉，所以我也不告诉你在一台电脑上怎么克隆多个仓库。

实际情况往往是这样，找一台电脑充当服务器的角色，每天24小时开机，其他每个人都从这个“服务器”仓库克隆一份到自己的电脑上，并且各自把各自的提交推送到服务器仓库里，也从服务器仓库中拉取别人的提交。

完全可以自己搭建一台运行Git的服务器，不过现阶段，为了学Git先搭个服务器绝对是小题大作。好在这个世界上有个叫[GitHub](https://github.com/)的神奇的网站，从名字就可以看出，这个网站就是提供Git仓库托管服务的，所以，只要注册一个GitHub账号，就可以免费获得Git远程仓库。

在继续阅读后续内容前，请自行注册GitHub账号。由于你的本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以，需要一点设置：

第1步：创建SSHKey。

在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

    $ ssh-keygen -t rsa -C "youremail@example.com"


你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。

如果一切顺利的话，可以在用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。

第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：

然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容：

<img src="https://gitee.com/xm0316/drawingbed/raw/master/202308120303030.jpg" alt="img" style="zoom:67%;" />

点“Add Key”，你就应该看到已经添加的Key：

<img src="https://gitee.com/xm0316/drawingbed/raw/master/202308120303029.jpg" alt="img" style="zoom: 33%;" />

> 为什么GitHub需要SSHKey呢？

因为GitHub需要识别出你推送的提交确实是你推送的,而不是别人冒充的, 而Git支持SSH协议, 所以,GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。

当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。

最后友情提示，在GitHub上免费托管的Git仓库，任何人都可以看到喔（但只有你自己才能改）。所以，不要把敏感信息放进去。

如果你不想让别人看到Git库，有两个办法，一个是交点保护费，让GitHub把公开的仓库变成私有的，这样别人就看不见了（不可读更不可写）。另一个办法是自己动手，搭一个Git服务器，因为是你自己的Git服务器，所以别人也是看不见的。这个方法我们后面会讲到的，相当简单，公司内部开发必备。

确保你拥有一个GitHub账号后，我们就即将开始远程仓库的学习。

-  小结

“有了远程仓库，妈妈再也不用担心我的硬盘了。”——Git点读机

### 添加远程库

现在的情景是，你已经在本地创建了一个Git仓库后，又想在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步，这样，GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作，真是一举多得。

首先，登陆GitHub，然后，在右上角找到“Create a new repo”按钮，创建一个新的仓库：

![github-create-repo-1](https://gitee.com/xm0316/drawingbed/raw/master/202308130635905.png)

在Repository name填入`learngit`，其他保持默认设置，点击“Create repository”按钮，就成功地创建了一个新的Git仓库：

![github-create-repo-2](https://gitee.com/xm0316/drawingbed/raw/master/202308130635766.png)

目前，在GitHub上的这个`learngit`仓库还是空的，GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。

- **关联仓库**

现在，我们根据GitHub的提示，**在本地的`learngit`仓库下运行命令：**  

```sh
$ git remote add origin git@github.com:michaelliao/learngit.git
```


请千万注意，把上面的`michaelliao`替换成你自己的GitHub账户名，否则，你在本地关联的就是我的远程库，关联没有问题，但是你以后推送是推不上去的，因为你的SSH Key公钥不在我的账户列表中。

添加后，**远程库的名字**就是`origin`，这是Git默认的叫法，也可以改成别的，但是`origin`这个名字一看就知道是远程库。



- **首次推送**

下一步，就可以把本地库的所有内容推送到远程库上：

`git push -u origin master`

```sh
$ git push -u origin master
Counting objects: 20, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (15/15), done.
Writing objects: 100% (20/20), 1.64 KiB | 560.00 KiB/s, done.
Total 20 (delta 5), reused 0 (delta 0)
remote: Resolving deltas: 100% (5/5), done.
To github.com:michaelliao/learngit.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```


把本地库的内容推送到远程，用`git push`命令，实际上是**把当前分支`master`推送到远程**。

由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

推送成功后，可以立刻在GitHub页面中看到远程库的内容已经和本地一模一样：

![github-repo](https://gitee.com/xm0316/drawingbed/raw/master/202308130635369.png)

从现在起，只要本地作了提交，就可以通过命令：

    $ git push origin master


把本地`master`分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！

#### SSH警告

当你第一次使用Git的`clone`或者`push`命令连接GitHub时，会得到一个警告：

    The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.
    RSA key fingerprint is xx.xx.xx.xx.xx.
    Are you sure you want to continue connecting (yes/no)?


这是因为Git使用SSH连接，而SSH连接在第一次验证GitHub服务器的Key时，需要你确认GitHub的Key的指纹信息是否真的来自GitHub的服务器，输入`yes`回车即可。

Git会输出一个警告，告诉你已经把GitHub的Key添加到本机的一个信任列表里了：

    Warning: Permanently added 'github.com' (RSA) to the list of known hosts.


这个警告只会出现一次，后面的操作就不会有任何警告了。

如果你实在担心有人冒充GitHub服务器，输入`yes`前可以对照[GitHub的RSA Key的指纹信息](https://help.github.com/articles/what-are-github-s-ssh-key-
fingerprints/)是否与SSH连接给出的一致。

#### 删除远程库

如果添加的时候地址写错了，或者就是想删除远程库，可以用`git remote rm <name>`命令。使用前，建议先用`git remote -v`查看远程库信息：

    $ git remote -v
    origin  git@github.com:michaelliao/learn-git.git (fetch)
    origin  git@github.com:michaelliao/learn-git.git (push)


然后，根据名字删除，比如删除`origin`：

    $ git remote rm origin

此处的“删除”其实是**解除了本地和远程的绑定关系**，并不是物理上删除了远程库。远程库本身并没有任何改动。要真正删除远程库，需要登录到GitHub，在后台页面找到删除按钮再删除。



#### 小结

要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`；

关联一个远程库时**必须给远程库指定一个名字**，`origin`是默认习惯命名；

关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；

分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！

当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！

> 新的关联推送操作 

```sh
git remote add origin git@github.com:xm0316/Git_command.git
git branch -M main
git push -u origin main # origin:远程仓库名, main:本地仓库主分支名
```

 `git branch -M "branch_name"`中 **-M 参数**是用来**给分支改名**的: 

With a -m or -M option, will be renamed to . If exists, -M must be used to force the rename to happen.

把master改名为main(黑人的政治正确运动).



### 从远程库克隆

上次我们讲了先有本地库，后有远程库的时候，如何关联远程库。

现在，假设我们从零开发，那么最好的方式是先创建远程库，然后，从远程库克隆。

首先，登陆GitHub，创建一个新的仓库，名字叫`gitskills`：

![github-init-repo](Git 的一些操作.assets/0-1691881536566-13.png)

我们勾选Initialize this repository with a README，这样GitHub会自动为我们创建一个`README.md`文件。创建完毕后，可以看到`README.md`文件：

![github-init-repo-2](Git 的一些操作.assets/0-1691881542958-16.png)

现在，远程库已经准备好了，下一步是用命令`git clone`克隆一个本地库：

    $ git clone git@github.com:michaelliao/gitskills.git
    Cloning into 'gitskills'...
    remote: Counting objects: 3, done.
    remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 3
    Receiving objects: 100% (3/3), done.


注意把Git库的地址换成你自己的，然后进入`gitskills`目录看看，已经有`README.md`文件了：    

    $ cd gitskills
    $ ls
    README.md


如果有多个人协作开发，那么每个人各自从远程克隆一份就可以了。

你也许还注意到，GitHub给出的地址不止一个，还可以用`https://github.com/michaelliao/gitskills.git`这样的地址。实际上，Git支持多种协议，默认的`git://`使用ssh，但也可以使用`https`等其他协议。

使用`https`除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放http端口的公司内部就无法使用`ssh`协议而只能用`https`。

### 小结

要克隆一个仓库，首先必须知道仓库的地址，然后使用`git clone`命令克隆。

Git支持多种协议，包括`https`，但`ssh`协议速度最快。

## 分支管理

分支就是科幻电影里面的平行宇宙，当你正在电脑前努力学习Git的时候，另一个你正在另一个平行宇宙里努力学习SVN。

如果两个平行宇宙互不干扰，那对现在的你也没啥影响。不过，在某个时间点，两个平行宇宙合并了，结果，你既学会了Git又学会了SVN！

![learn-branches](https://gitee.com/xm0316/drawingbed/raw/master/202308130716299.png)

分支在实际中有什么用呢？假设你准备开发一个新功能，但是需要两周才能完成，第一周你写了50%的代码，如果立刻提交，由于代码还没写完，不完整的代码库会导致别人不能干活了。如果等代码全部写完再一次提交，又存在丢失每天进度的巨大风险。

现在有了分支，就不用怕了。你创建了一个属于你自己的分支，别人看不到，还继续在原来的分支上正常工作，而你在自己的分支上干活，想提交就提交，直到开发完毕后，再一次性合并到原来的分支上，这样，既安全，又不影响别人工作。

其他版本控制系统如SVN等都有分支管理，但是用过之后你会发现，这些版本控制系统创建和切换分支比蜗牛还慢，简直让人无法忍受，结果分支功能成了摆设，大家都不去用。

***但Git的分支是与众不同的，无论创建、切换和删除分支，Git在1秒钟之内就能完成！***无论你的版本库是1个文件还是1万个文件。



### 创建与合并分支

在[版本回退](/wiki/896043488029600/897013573512192)里，你已经知道，每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在Git里，这个分支叫主分支，即`master`分支。`HEAD`严格来说不是指向提交，而是指向`master`，`master`才是指向提交的，所以，`HEAD`指向的就是当前分支。

一开始的时候，`master`分支是一条线，Git用`master`指向最新的提交，再用`HEAD`指向`master`，就能确定当前分支，以及当前分支的提交点：

                      HEAD
                        │
                        │
                        ▼
                     master
                        │
                        │
                        ▼
    ┌───┐     ┌───┐    ┌───┐
    │   │───▶│   │───▶│   │
    └───┘     └───┘    └───┘


每次提交，`master`分支都会向前移动一步，这样，随着你不断提交，`master`分支的线也越来越长。

当我们创建新的分支，例如`dev`时，Git新建了一个指针叫`dev`，指向`master`相同的提交，再把`HEAD`指向`dev`，就表示当前分支在`dev`上： 

                     master
                        │
                        │
                        ▼
    ┌───┐    ┌───┐    ┌───┐
    │   │───▶│   │───▶│   │
    └───┘    └───┘    └───┘
                        ▲
                        │
                        │
                       dev
                        ▲
                        │
                        │
                      HEAD


你看，Git创建一个分支很快，因为除了增加一个`dev`指针，改改`HEAD`的指向，工作区的文件都没有任何变化！

不过，从现在开始，对工作区的修改和提交就是针对`dev`分支了，比如新提交一次后，`dev`指针往前移动一步，而`master`指针不变：

                     master
                        │
                        │
                        ▼
    ┌───┐    ┌───┐    ┌───┐    ┌───┐
    │   │───▶│   │───▶│   │───▶│   │
    └───┘    └───┘    └───┘    └───┘
                                 ▲
                                 │
                                 │
                                dev
                                 ▲
                                 │
                                 │
                               HEAD


假如我们在`dev`上的工作完成了，就可以把`dev`合并到`master`上。Git怎么合并呢？最简单的方法，就是直接把`master`指向`dev`的当前提交，就完成了合并：

                               HEAD
                                 │
                                 │
                                 ▼
                              master
                                 │
                                 │
                                 ▼
    ┌───┐    ┌───┐    ┌───┐    ┌───┐
    │   │───▶│   │───▶│   │───▶│   │
    └───┘    └───┘    └───┘    └───┘
                                 ▲
                                 │
                                 │
                                dev


所以Git合并分支也很快！就改改指针，工作区内容也不变！

合并完分支后，甚至可以删除`dev`分支。删除`dev`分支就是把`dev`指针给删掉，删掉后，我们就剩下了一条`master`分支：

                               HEAD
                                 │
                                 │
                                 ▼
                              master
                                 │
                                 │
                                 ▼
    ┌───┐    ┌───┐    ┌───┐    ┌───┐
    │   │───▶│   │───▶│   │───▶│   │
    └───┘    └───┘    └───┘    └───┘


真是太神奇了，你看得出来有些提交是通过分支完成的吗？

下面开始实战。

首先，我们创建`dev`分支，然后切换到`dev`分支： 

```shell
$ git checkout -b dev
Switched to a new branch 'dev'
```

`git checkout`命令加上`-b`参数**表示创建并切换**，相当于以下两条命令：   

    $ git branch dev
    $ git checkout dev
    Switched to branch 'dev'


然后，用`git branch`命令查看当前分支：    

    $ git branch
    * dev
      master

`git branch`命令会列出所有分支，**当前分支**前面会标一个`*`号。

然后，我们就可以在`dev`分支上正常提交，比如对`readme.txt`做个修改，加上一行：

    Creating a new branch is quick.


然后提交：

    $ git add readme.txt 
    $ git commit -m "branch test"
    [dev b17d20e] branch test
     1 file changed, 1 insertion(+)


现在，`dev`分支的工作完成，我们就可以切换回`master`分支：   

    $ git checkout master
    Switched to branch 'master'


切换回`master`分支后，再查看一个`readme.txt`文件，刚才添加的内容不见了！因为那个提交是在`dev`分支上，而`master`分支此刻的提交点并没有变：

![git-br-on-master](https://gitee.com/xm0316/drawingbed/raw/master/202308130914650.png)

现在，我们把`dev`分支的工作成果合并到`master`分支上：    

    $ git merge dev
    Updating d46f35e..b17d20e
    Fast-forward
     readme.txt | 1 +
     1 file changed, 1 insertion(+)


`git merge`命令用于合并指定分支到当前分支。合并后，再查看`readme.txt`的内容，就可以看到，和`dev`分支的最新提交是完全一样的。

注意到上面的`Fast-forward`信息，Git告诉我们，这次合并是“快进模式”，也就是直接把`master`指向`dev`的当前提交，所以合并速度非常快。

当然，也不是每次合并都能`Fast-forward`，我们后面会讲其他方式的合并。

合并完成后，就可以放心地删除`dev`分支了：

    $ git branch -d dev
    Deleted branch dev (was b17d20e).


删除后，查看`branch`，就只剩下`master`分支了：

    $ git branch
    * master


因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在`master`分支上工作效果是一样的，但过程更安全。

#### switch

我们注意到切换分支使用`git checkout <branch>`，而前面讲过的撤销修改则是`git checkout -- <file>`，同一个命令，有两种作用，确实有点令人迷惑。

实际上，切换分支这个动作，用`switch`更科学。因此，最新版本的Git提供了新的`git switch`命令来切换分支：

**创建并切换到新的**`dev`分支，可以使用：

    $ git switch -c dev


直接切换到已有的`master`分支，可以使用：   

    $ git switch master


使用新的`git switch`命令，比`git checkout`要更容易理解。

#### 小结

Git鼓励大量使用分支：

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`或者`git switch <name>`

创建+切换分支：`git checkout -b <name>`或者`git switch -c <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`











## Git 操作

针对自己的项目更新部分文件

## Git 更新指定部分文件夹

基于sparse clone变通方法

- 创建一个空仓库
- 拉取远程仓库信息
- 开启 sparse clone
- 设置过滤
- 更新仓库

### 创建空仓库

```sh
mkdir devops
cd devops
git init  # 初始化
```

### 拉取远程仓库信息

```sh
git remote add -f origin http://your/git/repo.git  # 拉取远程仓库信息
```

### 开启 sparse clone

```sh
git config core.sparsecheckout true  # 开启 sparse clone
```

Git1.7.0 以后加入了Sparse Checkout模式，该模式可以实现Check Out指定文件或者文件夹.

### 设置过滤

```sh
echo "devops" >> .git/info/sparse-checkout  # 设置过滤条件
```

“devops”为选定的文件夹

### 更新仓库

```sh
git pull origin main # 拉取仓库
```

Git1.7.0以后加入了`Sparse Checkout`模式，该模式可以实现Check Out指定文件或者文件夹.

举个例子：
 	 现在有一个test仓库 `ssh://git@github.com/mygithub/test.git`, 你要`git clone`里面的`myproj`子目录：

```csharp
git init test && cd test     // 新建仓库并进入文件夹
git config core.sparsecheckout true // 设置允许克隆子目录
echo 'myproj' >> .git/info/sparse-checkout // 设置要克隆的仓库的子目录路径   //空格别漏
git remote add origin ssh://git@github.com/mygithub/test.git // 这里换成你要克隆的项目和库
git pull origin main    // 下载代码
```



### Git clone 命令

1. 最简单直接的命令

   `git clone xxx.git`

2. 如果想clone到指定目录

   `git clone xxx.git "指定目录"`

3. clone 时创建新的分支替代默认Origin HEAD（main）

   `git clone -b [new_branch_name]  xxx.git`

4.  clone 远程分支　　

   git clone 命令默认的只会建立`main`分支，如果你想clone指定的某一远程分支(如：dev)的话，可以如下：　　

   A. 查看所有分支(包括隐藏的)  

   git branch -a 显示所有分支，如：　　　　
   
   ```sh
   git branch -a 
   * main  
   remotes/origin/HEAD -> origin/main
   remotes/origin/dev  
   remotes/origin/main
   ```

   　

   B.  在本地新建同名的("dev")分支，并切换到该分支
   
   ```
   git checkout -t origin/dev 该命令等同于： git checkout -b dev origin/dev
   ```
   
   

## [Git分支开发，分支(master)同步主干(main)代码，以及最终分支合并到主干](https://www.cnblogs.com/xinmengwuheng/p/7115549.html)

由于rebase执行速度慢，分支同步主干代码时，分支的每次提交都可能和主干产生冲突，需要解决的次数太多，影响提交效率。 同时，为了保证主干提交线干净(可以安全回溯)，所以采用下面所说的merge法。

列出当前分支

```sh
D:\develop_tools\Gitee_Blog\xm0316.github.io>git branch -a
  main
* master
  remotes/origin/HEAD -> origin/main
  remotes/origin/main
  remotes/origin/master
```



### merge法

核心: `(main) git merge master --squash` 

意思是把`master`分支不同于`main`分支的所有文件罗列出来(无论有几个提交)，然后就可以方便的`git commit`提交了.

```sh
#1 创建功能分支 
(main) git checkout -b master
#2 功能迭代 
(master) git commit ...
```



```sh
#3 合并最新主干代码 
(master) git checkout main 
(main) git pull 
(main) git checkout master 
(master) git merge main
# 解决冲突 
(master) git commit #
```

报错: fatal: refusing to merge unrelated histories, 添加 --allow-unrelated-histories

```sh
fatal: refusing to merge unrelated histories

D:\develop_tools\Gitee_Blog\xm0316.github.io>git merge main --allow-unrelated-histories
```



```sh
# 4 review，修改代码 
(master) git commit 
```



```sh
# 5 提交测试通过后，合并到主分支，先执行一遍第3步 
# 把提交合并成一个
(master) git checkout main 
(main) git merge master --squash 
(main) git commit 
# 推送到远端，正常结束 
(main) git push origin 
```



```sh
# 6 如果上一步被拒绝，是因为main有更新的代码入库了，为了防止main上出现分线，需要重新执行第5步
```



## 其他技巧

