
### 创建版本库
```
pwd                        显示当前目录
git init                   初始化仓库
git add <file>             将工作区的文件添加到暂存区
git add .                  将工作区所有修改文件添加到暂存区
git commit -m "comment"    将暂存区的内容提交仓库
git status                 查看工作区和暂存区的状态
git diff <file>            查看file具体修改的内容
```
<br/>

### 连接远程仓库
```
git remote                 查看远程库的信息
git remote -v              查看远程库的详细信息
git config --global user.name "浦倩"                  添加全局用户名
git config --global user.email "puqian1992@163.com"   添加全局邮箱
git push origin master     推送至master分支
git clone git@git-KVM:puqian/wms.git  拷贝远端内容至本地
```
<br/>

### 版本回退
```
git log       查看提交历史
git log --pretty=online   显示简洁的历史记录
git relog     查看命令历史

git reset --hard HEAD^    暂存区回退到上一个版本
git reset --hard HEAD^^   暂存区会退到上两个版本
```
<br/>

### 撤销修改
```
git reset HEAD file   撤销暂存区文件内容的修改
git checkout --file   撤销工作区修改内容
```
<br/>

### 删除文件
```
rm file       从文件管理器中删除文件
git rm file   从版本库中删除文件
```
<br/>

### 分支管理
```
git checkout -b dev     创建并切换到dev分支（相当于 git branch dev; git checkout dev;）
git branch              查看当前所有分支
git checkout master     切换到master分支
git merge dev           将dev分支的工作成果合并到master分支上
git branch -d dev       删除dev分支
```
<br/>

### 标签
```
git tag                      查看所有标签信息
git tag <name>               打开一个新标签
git push origin <tagname>    推送一个本地标签
git push origin --tags       推送全部未推送过的本地标签
```
<br/>

### 实例
```
--- Git global setup ---
git config --global user.name  "浦倩"
git config --global user.email "xxx@xxx"

--- Create a new repository ---
git clone git@git-KVM:puqian/wms.git
cd wms
touch README.md
git add README.md
git commit -m "add README"
git push -u origin master

--- Existing  folder or Git repository ---
cd existing_folder
git init
git remote add origin git@git-KVM:puqian/wms.git
git add .
git commit -m "xxx"
git push -u origin master
```
<br/>

## Git 和 SVN 的对比
### SVN属于集中式的版本控制系统
集中式的版本控制系统都有一个单一的集中管理的服务器，保存所有文件的修订版本，而协同工作的人们都通过客户端连到这台服务器，取出最新的文件或者提交更新。
SVN的特点概括起来主要由以下几条：
1）每个版本库有唯一的URL（官方地址），每个用户都从这个地址获取代码和数据；
2）获取代码的更新，也只能连接到这个唯一的版本库，同步以取得最新数据；
3）提交必须有网络连接（非本地版本库）；
4）提交需要授权，如果没有写权限，提交会失败；
5）提交并非每次都能够成功。如果有其他人先于你提交，会提示 “改动基于过时的版本，先更新再提交” 诸如此类；
6）冲突解决是一个提交速度的竞赛：手快者，先提交，平安无事；手慢者，后提交，可能遇到麻烦的冲突解决。

### Git属于分布式的版本控制系统
Git记录版本历史只关心文件数据的整体是否发生变化。Git 不保存文件内容前后变化的差异数据。
实际上，Git 更像是把变化的文件作快照后，记录在一个微型的文件系统中。每次提交更新时，它会纵览一遍所有文件的指纹信息并对文件作一快照，然后保存一个指向这次快照的索引。为提高性能，若文件没有变化，Git 不会再次保存，而只对上次保存的快照作一连接。

在分布式版本控制系统中，客户端并不只提取最新版本的文件快照，而是把原始的代码仓库完整地镜像下来。这么一来，任何一处协同工作用的服务器发生故障，事后都可以用任何一个镜像出来的本地仓库恢复。

另外，因为Git在本地磁盘上就保存着所有有关当前项目的历史更新，并且Git中的绝大多数操作都只需要访问本地文件和资源，不用连网，所以处理起来速度飞快。

Git特点：
1）Git中每个克隆(clone)的版本库都是平等的。你可以从任何一个版本库的克隆来创建属于你自己的版本库，同时你的版本库也可以作为源提供给他人，只要你愿意。
2）Git的每一次提取操作，实际上都是一次对代码仓库的完整备份。
3）提交完全在本地完成，无须别人给你授权，你的版本库你作主，并且提交总是会成功。
4）甚至基于旧版本的改动也可以成功提交，提交会基于旧的版本创建一个新的分支。
5）Git的提交不会被打断，直到你的工作完全满意了，PUSH给他人或者他人PULL你的版本库，合并会发生在PULL和PUSH过程中，不能自动解决的冲突会提示您手工完成。
6）冲突解决不再像是SVN一样的提交竞赛，而是在需要的时候才进行合并和冲突解决。

### 用法上的理解
1）Git把内容按元数据方式存储，而SVN是按文件；git目录拥有中心版本库上的所有东西；
2）Git没有一个全局版本号，而 SVN 有；
3）Git 的内容的完整性要优于 SVN；
4）分支以及提交的不同。


参考资料：[廖雪峰的官方网站](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000) | [散尽浮华博客](http://www.cnblogs.com/kevingrace/p/5904595.html)
