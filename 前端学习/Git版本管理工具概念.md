![image-20240304161429657](C:\Users\86156\Desktop\Blog图片\image-20240304161429657.png)

---

# 版本管理工具概念

我在大学毕业写论文的时候的时候碰到过如下的现象

```
<<毕业论文第一版.doc>>
<<毕业论文第二版.doc>>
<<毕业论文最终版.doc>>
<<毕业论文最终版2.doc>>
```

类似的问题我曾经也碰到过很多,例如:

```
领导让写文档,写好了,领导让修改,改好了,领导觉得第一版不错,改回来吧,此时内心一脸懵,第一版长啥样没存档啊
```

实际上,代码开发中也需要这样的软件来管理我们的代码. 例如我们经常会碰到如下的现象:

```
改之前好好的,改完就报错了,也没怎么修改啊
```

在这种情况下如果不能查看修改之前的代码,查找问题是非常困难的.

如果有一个软件能记录我们对文档的所有修改,所有版本,那么上面的问题讲迎刃而解.而这类软件我们一般叫做版本控制工具

版本管理工具一般具有如下特性:

```
1) 能够记录历史版本,回退历史版本
2) 团队开发,方便代码合并
```

### 浅谈SVN

####  SVN(SubVersion)

工作流程

```
SVN是集中式版本控制系统，版本库是集中放在中央服务器的.
工作流程如下:
	1.从中央服务器远程仓库下载代码
	2.修改后将代码提交到中央服务器远程仓库
```

优缺点:

```
 优点: 简单,易操作
 缺点:所有代码必须放在中央服务器  
  	   1.服务器一旦宕机无法提交代码,即容错性较差
       2.离线无法提交代码,无法及时记录我们的提交行为
```

svn流程图

![image-20240304162809516](C:\Users\86156\AppData\Roaming\Typora\typora-user-images\image-20240304162809516.png)

### 浅谈Git

Git是分布式版本控制系统（Distributed Version Control System，简称 DVCS），分为两种类型的仓库：
本地仓库和远程仓库
工作流程如下
    1．从远程仓库中克隆或拉取代码到本地仓库(clone/pull)
    2．从本地进行代码修改
    3．在提交前先将代码提交到暂存区
    4．提交到本地仓库。本地仓库中保存修改的各个历史版本
    5．修改完成后，需要和团队成员共享代码时，将代码push到远程仓库

![image-20240304170636026](C:\Users\86156\AppData\Roaming\Typora\typora-user-images\image-20240304170636026.png)

总结:git和svn的区别

1. svn 是集中式版本控制工具,git 是分布式版本控制工具
2. svn 不支持离线提交,git 支持离线提交代码

## Git 工作流程

![image-20240304170918875](C:\Users\86156\AppData\Roaming\Typora\typora-user-images\image-20240304170918875.png)

### 概念解释

本地仓库：是在开发人员自己电脑上的Git仓库,存放我们的代码(.git 隐藏文件夹就是我们的本地仓库)		
远程仓库：是在远程服务器上的Git仓库,存放代码(可以是github.com或者gitee.com 上的仓库,或者自己该公司的服务器)
工作区: 我们自己写代码(文档)的地方

暂存区: 在 本地仓库中的一个特殊的文件(index) 叫做暂存区,临时存储我们即将要提交的文件

Clone：克隆，就是将远程仓库复制到本地仓库
Push：推送，就是将本地仓库代码上传到远程仓库
Pull：拉取，就是将远程仓库代码下载到本地仓库,并将代码 克隆到本地工作区

### 文件状态讲解

```
Git工作目录下的文件存在两种状态：
1 untracked 未跟踪（未被纳入版本控制） :  比如新建的文件(此时文件夹上没有图标或者有一个"问号")
2 tracked 已跟踪（被纳入版本控制）     
    2.1 Staged 已暂存状态            : 添加 但未提交状态(此时文件夹上有一个"加号")
	2.2 Unmodified 未修改状态        : 已提交(此时文件夹上有一个"对号")
	2.3 Modified 已修改状态          : 修改了,但是还没有提交 (此时文件夹上有一个"红色感叹号")
```

![image-20240304171625574](C:\Users\86156\AppData\Roaming\Typora\typora-user-images\image-20240304171625574.png)

这些文件的状态会随着我们执行Git的命令发生变化

![image-20240304171652540](C:\Users\86156\AppData\Roaming\Typora\typora-user-images\image-20240304171652540.png)

# 命令行-- git基本操作

- 环境配置

当安装Git后首先要做的事情是设置用户名称和email地址。这是非常重要的，因为每次Git提交都会使用该用户信息

```shell
#设置用户信息 
   git config --global user.name “...”
   git config --global user.email “...”
#查看配置信息
   git config --list
   git config user.name
#通过上面的命令设置的信息会保存在~/.gitconfig文件中

```

-  初始化本地仓库 init


```shell
# 初始化仓库带工作区
git init
# 初始化仓库不带工作区
git init --bare  
```

- 克隆 clone


```shell
# 从远程仓库克隆
git clone 远程Git仓库地址 
例如: git clone [url]
```

-   查看状态 status


```shell
# 查看状态
git status 
#查看状态 使输出信息更加简洁
git status –s 
```

-  add 


```shell
# 将未跟踪的文件加入暂存区
git add  <文件名>  
# 将暂存区的文件取消暂存 (取消 add )
git reset  <文件名>  
```

- commit


```shell
# git commit 将暂存区的文件修改提交到本地仓库
git commit -m "日志信息"  <文件名>  
```

-  删除 rm


```shell
# 从本地工作区 删除文件
git rm <文件名>  
# 如果本工作区库误删, 想要回退
git checkout head <文件名>  
```

命令行--git 远程仓库操作

####  查看远程 

```shell
# 查看远程  列出指定的每一个远程服务器的简写
git remote 
# 查看远程 , 列出 简称和地址
git remote  -v  
# 查看远程仓库详细地址
git remote show  <仓库简称>
```

#### 添加/移除远测仓库

```shell
# 添加远程仓库
git remote add <shortname> <url>
# 移除远程仓库和本地仓库的关系(只是从本地移除远程仓库的关联关系，并不会真正影响到远程仓库)
git remote rm <shortname> 
```

#### 从远程仓库获取代码

```shell
# 从远程仓库克隆
git clone <url> 
# 从远程仓库拉取 (拉取到.git 目录,不会合并到工作区,工作区发生变化)
git fetch  <shortname>  <分支名称>
# 手动合并  把某个版本的某个分支合并到当前工作区
git merge <shortname>/<分支名称>
# 从远程仓库拉取 (拉取到.git 目录,合并到工作区,工作区不发生变化) = fetch+merge
git pull  <shortname>  <分支名称>
git pull  <shortname>  <分支名称>  --allow-unrelated-histories  #  强制拉取合并
```

注意：如果当前本地仓库不是从远程仓库克隆，而是本地创建的仓库，并且仓库中存在文件，此时再从远程仓库拉取文件的时候会报错（fatal: refusing to merge unrelated histories ），解决此问题可以在git pull命令后加入参数--allow-unrelated-histories (如上 命令)

```shell
# 将本地仓库推送至远程仓库的某个分支
git push [remote-name] [branch-name]
```

## 分支的概念

​		  几乎所有的版本控制系统都以某种形式支持分支。 使用分支意味着你可以把你的工作从开发主线上分离开来，避免影响开发主线。多线程开发,可以同时开启多个任务的开发,多个任务之间互不影响.

​	分支的创建等同于，我们新开启一条自己的开发线，所以在合并后会删除，这里有涉及到合并从冲突的问题，后序会出相关处理方法Blog

###  命令行-- 分支

```shell
# 默认 分支名称为 master
# 列出所有本地分支
git branch
# 列出所有远程分支
git branch -r
# 列出所有本地分支和远程分支
git branch -a
# 创建分支
git branch <分支名>
# 切换分支 
git checkout <分支名>
# 删除分支(如果分支已经修改过,则不允许删除)
git branch -d  <分支名>
# 强制删除分支
git branch -D  <分支名>
```

```shell
# 提交分支至远程仓库
git push <仓库简称> <分支名称>	
# 合并分支 将其他分支合并至当前工作区
git merge <分支名称>
# 删除远程仓库分支
git push origin –d branchName
```

## tag  标签 

```
如果你的项目达到一个重要的阶段，并希望永远记住那个特别的提交快照，你可以给它打上标签(tag)
比如说，我们想为我们的项目发布一个"1.0"版本。 我们给最新一次提交打上（HEAD）"v1.0"的标签。
标签可以理解为项目里程碑的一个标记,一旦打上了这个标记则,表示当前的代码将不允许提交
```

```shell
# 列出所有tag
git tag
# 查看tag详细信息 
git show [tagName]
# 新建一个tag
git tag [tagName]
# 提交指定tag
$ git push [仓库简称] [tagName]
# 新建一个分支，指向某个tag
$ git checkout -b [branch] [tag]
# 删除本地tag
$ git tag -d [tag]
# 删除远程tag (注意 空格)
$ git push origin :refs/tags/[tag]
```

## 企业中我们是如何开发的?

```
1) 入职第一天,管理人员分配/git账号密码 
2) 开发人员下载代码即文档/ 根据文档将环境搭建成功
3) 团队一般会给你讲讲项目相关的支持
----
4) 你接到第一个需求(或者某个功能,一般要经过沟通,分析,设计...等过程)
5) 创建feature分支(一般一个需求对应一个feature,命名格式上标注该需求的id)
6) 开发需求,本地测试,提交代码到当前需求对应的feature分支,
	一般来讲为了避免将测试代码提交,需要提交前,检查如下步骤
	6.1) 是否多提交了某个文件,比如测试文件
	6.2) 是否漏提交文件
	6.3) 打开每一个应该提交的文件,判断是否多提交了一行代码,是否少提交了一行代码,是否删除了本应该存在的代码 
	检查完毕提交代码
7) 合并分支至test分支-- 测试人员会在test分支中测试
8) 测试人员测试bug ,开发者在feature分支上继续修改,提交
9) 测试人员测试通过 ,test分支会被测试人员合并到develop开发分支,再次测试
10)develop分支最终会被合并到master主分支
```

