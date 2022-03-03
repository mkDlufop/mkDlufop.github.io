---
layout: post
title: Git内部原理
subtitle:
tags: [Git]
---

* TOC
{:toc}

# 1   低层命令与上层命令

由于 Git 最初是一套面向版本控制系统的**工具集**，而不是一个完整的、用户友好的**版本控制系统**， 所以它还包含了一部分用于完成底层工作的子命令。 这些命令被设计成能以 UNIX 命令行的风格连接在一起，抑或藉由脚本调用，来完成工作。 这部分命令一般被称作“底层（plumbing）”命令。而那些更友好的命令，如checkout、branch、remote 等约 30 个 Git 的子命令则被称作“上层（porcelain）”命令。

# 2   .git 目录的典型结构

新初始化的 .git 目录的典型结构如下：

```shell
config			### config 文件包含项目特有的配置选项
description		### description 文件仅供 GitWeb 程序使用
HEAD			### HEAD 文件指向目前被检出的分支
hooks/			### hooks 目录包含客户端或服务端的钩子脚本（hook scripts）
info/			### info 目录包含一个全局性排除（global exclude）文件 ，用以放置那些不希望被记录在
 			    .gitignore文件中的忽略模式（ignored patterns）
objects/		### objects 目录存储所有数据内容
refs/			### refs 目录存储指向数据（分支、远程仓库和标签等）的提交对象的指针
```

# 3   Git对象

Git 是一个**内容寻址文件系统**。 这意味着，Git 的核心部分是一个简单的**键值对数据库（key-value data store）**。 你可以向 Git 仓库中**插入任意类型的内容**，它会**返回一个唯一的键**，**通过该键**可以在任意时刻再次**取回该内容**。

> Git具体如何存储对象见《ProGit》中 Git内部原理 章节的 Git对象小节。

## 类型及作用

| Git对象类型               | 作用                                            |
| ------------------------- | ----------------------------------------------- |
| 数据对象（blob object）   | 保存文件的内容                                  |
| 树对象（tree object）     | 保存目录名和文件名                              |
| 提交对象（commit object） | 保存项目快照、父提交、作者/提交者信息和提交注释 |
| 标签对象（tag object）    | 保存标签创建者信息、日期、注释，以及指针        |

## 一个例子

> 每次我们运行 git add 和 git commit 命令时，Git 所做的工作实质就是将被改写的文件保存为数据对象，
> 更新暂存区，记录树对象，最后创建一个指明了顶层树对象和父提交的提交对象。

| 在test0目录下新建test.txt，然后git add，git commit           | 在test1目录下新建test.txt，将test.txt保存为blob，更新缓存区，记录树对象，创建提交对象，创建引用 |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| $ cd ~ && mkdir test0 && cd test0                            | $ cd ~ && mkdir test1 && cd test1                            |
| $ git init                                                   | $ git init                                                   |
| ===== 在test.txt中写入test.txt ====                          | =============================                                |
| $ echo 'version 1' > test.txt                                | $ echo 'version 1' > test.txt                                |
| ===== 把test.txt添加到暂存区 ====                            | =============================                                |
| $ git add test.txt                                           | $ git hash-object -w test.txt<br />83baae61804e65cc73a7201a7252750c76066a30 |
|                                                              | $ git cat-file -p 83baae<br />version 1                      |
|                                                              | $ git update-index --add --cacheinfo 100644 83baae61804e65cc73a7201a7252750c76066a30 test.txt |
| ===== 提交 ====                                              | =============================                                |
| $ find .git/refs<br />.git/refs<br/>.git/refs/heads<br/>.git/refs/tags | $ find .git/refs<br />.git/refs<br/>.git/refs/heads<br/>.git/refs/tags |
| // 当我们执行 git commit 时，该命令会创建一个提交对象，并用 **HEAD** **文件**中那个**引用**所指向的 **SHA-1 值**设置其**父提交字段**。 |                                                              |
| $ git commit -m 'first commit'<br />[master (root-commit) 352b30e] first commit                        <br/> 1 file changed, 1 insertion(+)                                    <br/> create mode 100644 test.txt | $ git write-tree<br/>d8329fc1cc938780ffdd9f94e0d364e0ea74f579 |
| $ find .git/objects -type f<br />.git/objects/83/baae61804e65cc73a7201a7252750c76066a30             <br/>.git/objects/d8/329fc1cc938780ffdd9f94e0d364e0ea74f579             <br/>.git/objects/35/2b30e1f0d5b47bad3bd5999ba2c59b4acb2c03 | $ git cat-file -p d8329f<br/>100644 blob 83baae61804e65cc73a7201a7252750c76066a30 test.txt |
|                                                              | $ echo 'first commit'  \| git commit-tree d8329f<br />baced8777d850523523cc5ed1528273ccd7744c4 |
|                                                              | $ find .git/objects -type f<br />.git/objects/ba/ced8777d850523523cc5ed1528273ccd7744c4<br/>      .git/objects/83/baae61804e65cc73a7201a7252750c76066a30<br/>.git/objects/d8/329fc1cc938780ffdd9f94e0d364e0ea74f579 |
|                                                              | $ git update-ref refs/heads/master baced8777d850523523cc5ed1528273ccd7744c4 |
| $ find .git/refs<br />.git/refs<br/>.git/refs/heads<br/>.git/refs/heads/master<br/>.git/refs/tags | $ find .git/refs<br />.git/refs<br/>.git/refs/heads<br/>.git/refs/heads/master<br/>.git/refs/tags |





## 相关命令

**数据对象**

```shell
$ echo 'version 1' > test.txt
$ git hash-object -w test.txt
83baae61804e65cc73a7201a7252750c76066a30 
# 根据文件test.txt创建一个新的数据对象，并返回指向该数据对象的唯一的键（即 对应的 SHA-1 值），-w 选项的作用是
  将该对象写入.git/objects 目录（即 对象数据库）中。
```

>$ git cat-file -p 83baae61804e65cc73a7201a7252750c76066a30  # 输出该 SHA-1 值所对应的对象的内容
>
>version 1
>
>$ git cat-file -s 83baae61804e65cc73a7201a7252750c76066a30  # 查看该对象有多大
>
>
>
>// 该SHA-1值（长度为40个字符）是一个将待存储的数据外加一个头部信息（header）一起做 SHA-1 校验运算而得的校验和。

**树对象**

通常，Git 根据某一时刻暂存区（即 index 区域）所表示的状态创建并记录一个对应的树对象， 如此重复便可依次记录（某个时间段内）一系列的树对象。 因此，为创建一个树对象，首先需要通过暂存一些文件来创建一个暂存区。

```shell
$ git update-index --add --cacheinfo 100644 83baae61804e65cc73a7201a7252750c76066a30 test.txt   
# 将.git/objects 目录（即 对象数据库）中的test.txt文件加入到一个新的暂存区，加--add选项是因为test.txt文件并不在
暂存区中，加--cacheinfo选项是因为将要添加的文件test.txt位于 Git 数据库（即 对象数据库）中。
83baae61804e65cc73a7201a7252750c76066a30是test.txt存储到Git数据库后所对应的 SHA-1 值。
# 本例中，我们指定的文件模式为 100644，表明这是一个普通文件。 其他选择包括：100755，
表示一个可执行文件；120000，表示一个符号链接。 这里的文件模式参考了常见的 UNIX 文件模式，
但远没那么灵活——上述三种模式即是 Git 文件（即数据对象）的所有合法模式（当然，还有其他一些模式，
但用于目录项和子模块）。

$ git write-tree
d8329fc1cc938780ffdd9f94e0d364e0ea74f579
$ git cat-file -p d8329fc1cc938780ffdd9f94e0d364e0ea74f579
100644 blob 83baae61804e65cc73a7201a7252750c76066a30 test.txt
```

**提交对象**

```shell
$ echo 'first commit' | git commit-tree d8329f
baced8777d850523523cc5ed1528273ccd7744c4
# 创建一个提交对象。d8328f是某个树对象的 SHA-1 值。有父提交的话还要在命令后面加“-p 父提交的SHA-1值”。
```

**标签对象**

标签对象通常指向一个提交对象，而不是一个树对象。 它像是一个永不移动的分支引用（永远指向同一个提交对象），只不过给这个提交对象加上一个更友好的名字罢了。注意，标签对象并不是只能指向某个提交对象，我们可以对任意类型的Git对象打标签。

标签有两种：附注标签和轻量标签。创建一个轻量标签：`git update-ref refs/tags/v1.0 baced8777d850523523cc5ed1528273ccd7744c4`。可以看到轻量标签其实就是一个固定的引用。

```shell
$ git tag -a v1.1 352b30e1f0d5b47bad3bd5999ba2c59b4acb2c03 -m 'test tag'
# 创建一个附注标签。352b30e1f0d5b47bad3bd5999ba2c59b4acb2c03为某个提交对象的 SHA-1 值。-a表明创建一个附注标签
# 若要创建一个附注标签，Git 会创建一个标签对象，并记录一个引用来指向该标签对象，该标签对象再指向提交对象，而不是直接指向提交对象。
```

# 4   Git 引用

> 如果你对仓库中从一个提交（比如 baced8）开始往前的历史感兴趣，那么可以运行 git log baced8 这样的
> 命令来显示历史，不过你需要记得 baced8 是你查看历史的起点提交。 如果我们**有一个文件**来**保存 SHA-1 值**，而该文件有一个**简单的名字**， 然后用这个**名字指针**来**替代原始的 SHA-1 值**的话会更加简单。
>
> 在 Git 中，这种简单的名字被称为“引用（references，或简写为 refs）”。你可以在 .git/refs 目录下找到
> 这类含有 SHA-1 值的文件。

> Git 分支的本质：一个指向某一系列提交之首的指针或引用。

| 类型      | 作用                                   |
| --------- | -------------------------------------- |
| HEAD 引用 | 指向目前所在的分支                     |
| 标签引用  | 永远指向同一个对象                     |
| 远程引用  | 记录远程服务器上各分支最后已知位置状态 |



```shell
################# 相关命令 ###############

$ git update-ref refs/heads/master baced8
# 创建一个引用，baced8为某个提交对象的 SHA-1 值。
# 使用git update-ref创建引用要比直接将 SHA-1 的值写入引用文件中更加安全。是因为引用日志（reflog）会通过git update-ref命令更新。通过git reflog可以查到我们提交或改变分支后git记录的一些信息。

$ git symbolic-ref HEAD refs/heads/test
# 设置 HEAD 引用的值

$ git symbolic-ref HEAD
# 查看 HEAD 引用对应的值
```

# 5   包文件

> Git 最初向磁盘中存储对象时所使用的格式被称为“松散（loose）”对象格式。 但是，Git 会时不时地将多个这些对象打包成一个称为“包文件（packfile）”的二进制文件，以节省空间和提高效率。在打包这些对象时，Git 只**完整保存**最新版的那个，再保存旧版本对象与最新版本的差异内容。因为大部分情况下需要快速访问文件的最新版本，所以只有最新版的对象保存完整的。当版本库中有太多的松散对象，或者你手动执行 git gc 命令，或者你向远程服务器执行推送时，Git 都会这样做。

```shell
$ git gc
# 让Git对对象进行打包

$ git verify-pack -v .git/objects/pack/pack-a7bb32334eb3fe7a3c5e9f8b1d6e8ad47cd9bfa5.idx
# 查看包文件的内容
```

# 6   数据恢复

在一些情况下我们可能会丢失一些提交，如使用git reset --hard硬重置仓库的某个分支到一个旧提交后，这时可使用以下方法来恢复丢失的提交：

1，找到丢失提交的SHA-1值。

- 使用git reflog工具。添加-g选项可以让git reflog以标准日志格式输出引用日志。

  > 当你正在工作时，Git 会默默地记录每一次你改变 HEAD 时它的值。 每一次你提交或改变分支，引用日志都会被更新。 引用日志（reflog）也可以通过git update-ref 命令更新。

- 若丢失的提交不在引用日志中，可以使用git fsck工具，它会检查数据库的完整性。同时，添加--ful选项可以让它显示出所有没有被其他对象指向的对象。

2，创建一个新的分支指向这个丢失的提交。例如，创建一个名为recover的分支指向这个丢失的提交（123456）：`git branch recover 123456`。



# 参考资料：

{:.no_toc}
[1] 《Pro Git》第10章 Git内部原理

