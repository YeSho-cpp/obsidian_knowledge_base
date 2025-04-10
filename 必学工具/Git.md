## 1. Git概述
Git是一个免费的、开源的**分布式版本控制系统**，可以快速高效地处理从小型到大型的各种项目。
**何为版本控制**
* 版本控制是一种记录文件内容**变化**，可以记录文件修改历史记录，可以查看历史版本。

	<img src="https://article.biliimg.com/bfs/article/1c53bcdc662b32020b3d60d3ecec81b9653f83f1.png" alt="image.png" style="zoom:50%;" />

### 版本控制系统分类

#### 集中式 (SVN)

<img src="https://article.biliimg.com/bfs/article/5754334302c89c2eff3eb2191cad39ff7e4b8a0f.gif" alt="image.png" style="zoom:50%;" />




#### 分布式 (git) 


<img src="https://article.biliimg.com/bfs/article/2c797c280956357ac2ea35043a5e14c11b3a70a8.gif" alt="image.png" style="zoom:40%;" />





### 1.1 Git 工作机制
<img src="https://article.biliimg.com/bfs/article/d2e73e9d21b89393be3f9845163135e9b65098ab.png" alt="image.png" style="zoom:50%;" />

<img src="https://article.biliimg.com/bfs/article/48fb52546b22a8fa3d36c8b4e84f24a30b8b1d76.png" alt="image.png" style="zoom:40%;" />

* **工作区**：本地存储项目的目录，开发者编辑、修改、删除代码的主要区域,当`git init`之后就这个目标就成为工作区了
* **暂存区(index)**：中间区域，充当了提交到Git仓库之前的缓冲作用。
	* 位置：一般存放在`.git`目录下的index文件（`.git/index`）中，所以我们把暂存区有时也叫作索引（==index==）。
	* 只是一个文件,包含在版本库中.
* **本地库(objects)**：保存项目的完整历史记录,是git对象库，是用来存储各种创建的对象以及内容。
* **仓库区（或版本库）:**，就是安全存放数据的位置，这里面有你提交到所有版本的数据。其中HEAD指向最新放入仓库的版本。就是工作区有一个隐藏目录 .git，它不算工作区，而是Git的版本库
* **远程仓库**:托管代码的服务器，常用github、码云、gitlab。


#### git文件的四种状态

<img src="https://article.biliimg.com/bfs/article/9dfa32c5c8ed223705749c31c5898c6b28f9b5ce.png" alt="image.png" style="zoom:40%;" />


## Git 基础操作命令

|作用|命令名称 |
|:-:|:-:|
|git config --global user.name 用户名  |设置用户签名|
|git config --global user.email 邮箱|设置用户签名|
|git init |初始化本地库|
|git status  |查看本地库状态|
|git add 文件名|添加到暂存区|
|git commit -m "日志信息" 文件名 |提交到本地库|
|git reflog  |查看历史记录|
|git reset  版本号 |版本穿梭|
|git -files|列出git被追踪的文件，包括工作区和暂存区|

### 查看本地库状态 
```bash
Layne@LAPTOP-Layne MINGW64 /d/Git-Space/SH0720 (master)
$ git status
On branch master
No commits yet
nothing to commit (create/copy files and use "git add" to track)
```
再次查看时
```shell
Layne@LAPTOP-Layne MINGW64 /d/Git-Space/SH0720 (master)
$ git status
On branch master
No commits yet
Untracked files:
(use "git add <file>..." to include in what will be committed)
hello.txt
nothing added to commit but untracked files present (use "git add" to track)
```
### 添加暂存区

```shell
Layne@LAPTOP-Layne MINGW64 /d/Git-Space/SH0720 (master)
$ git add hello.txt
warning: LF will be replaced by CRLF in hello.txt.
The file will have its original line endings in your working
directory.
```

```sh
git rm --cached <file>   将暂存区的文件移除，相当于git add <file> 的回退
```

```sh
git restore <file>  当暂存区的文件处于修改修改或被删除时，这个命令将修改回退未修改之前的状态
```


```sh
git ls-files  # 查看暂存区文件
```


### 2.3 提交本地库
`git commit -m` " 日志信息" 文件名
提交将把所有暂存区的更改保存到Git仓库中，并==清空==暂存区。

```sh
git log 可以查看当前的提交的历史记录 # 可以使用--oneline参数来查看简洁的提交记录 
git reflog 可以查看所有的提交的信息(包括回退前和回退后)
```

```sh
$ git log
commit 0b7ac4814b88e1274dee0213dc9b7bc1a4084e97 (HEAD -> master)  
Author: YeSho-cpp <2465968725@qq.com>
Date:   Fri Dec 22 11:45:32 2023 +0800

    second commit

commit 9e1995fc0fbe0b8db94010b7f0dfd324b89ca681
Author: YeSho-cpp <2465968725@qq.com>
Date:   Fri Dec 22 11:42:14 2023 +0800

    first commit
$ git reflog
0b7ac48 (HEAD -> master) HEAD@{0}: commit: second commit
9e1995f HEAD@{1}: commit (initial): first commit

# 这里commit后面这个16进制的字符串就是每次提交的唯一ID
```

这里commit后面这个16进制的字符串就是每次提交的<font color=#ff0000>唯一ID</font>

### git版本
#### 版本信息

```bash
Layne@LAPTOP-Layne MINGW64 /d/Git-Space/SH0720 (master)
$ git reflog
087a1a7 (HEAD -> master) HEAD@{0}: commit: my third commit  
ca8ded6 HEAD@{1}: commit: my second commit
86366fa HEAD@{2}: commit (initial): my first commit
```
- `087a1a7`：版本号   
- `HEAD`:当前指针 
- `master`: 当前分支
- 指针在0说明在当前`087a1a7`版本

#### 版本穿梭:
- 基本语法
	- `git reset --hard` 版本号:重置stage区和工作目录里的内容，就是你的在当前版本号没有**commit**的修改会被全部擦掉
	- `git reset`版本号:只重置stage区
	- `git reset --soft` 版本号:用于版本的回退，只进行对commit操作的回退，不影响工作区的文件 

	<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20231222150041.png?" alt="image.png" style="zoom:40%;" />


> 在提交代码的时候，commit之后，然后我又在工作区添加了东西，这时候突然发现，上一次的commit有错误的文件，需要重新修改，但是我添加的东西友不想丢失，而且我想修改上一次的提交，这时候可进行`git reset --soft` 版本号

#### **版本撤销**:
`git revert` 版本号:只撤回指定的提交，并保留后续的提交。
Git 切换版本，底层其实是**移动的 HEAD 指针**，具体原理如下图所示。
	<img src="https://article.biliimg.com/bfs/article/fb71553104f1f32a2d6caa1b1e9e2ba96ae1a704.png" alt="image.png" style="zoom:50%;" />
### 差异比较 git diff

git diff的作用如下：
1. 查看工作区、暂存区、本地仓库之间的差异 
2. 查看不同版本之前的差异
3. 查看不同分支的差异

|  命令           |  作用            |
|:-------------:|:--------------:|
|  git diff     |  比较工作区和暂存区的差别  |
|git diff HEAD(代表当前的版本)| 比较工作区和本地库的差别   |
|git diff --cached |比较暂存区和本地库的差别|
|git diff 版本id 版本id|比较两个版本之前的差异|
|git diff 分支名 分支名|比较两个分支之前的差异|



```sh
$ git diff
diff --git a/file3.txt b/file3.txt  # 第一行 `diff --git a/file3.txt b/file3.txt` 提示我们正在比较的是 `file3.txt` 文件
index 55bd0ac..a64d9e2 100644  # 是文件索引信息，它显示了比较的两个版本的哈希值和文件模式。
--- a/file3.txt  # 表示旧版本的 `file3.txt` 文件。
+++ b/file3.txt  # 表示新版本的 `file3.txt` 文件
@@ -1 +1 @@  #行指示了文件中发生更改的位置。例如 `-1` 表示旧版本的第一行，`+1` 表示新版本的第一行
-333
+一键三连了吗？
```




使用 `git diff`来比较暂存区、本地库与工作区的内容

-   工作区暂存区比较
    
     命令:`git diff readme.txt`
    
    ![](https://article.biliimg.com/bfs/article/fe58caa0298ada96d571b51b6a3a6e17b2d886b5.png)
    
-   工作区本地库比较
    
     命令:`git diff HEAD readme.txt`
    
    ![](https://article.biliimg.com/bfs/article/e0e966fe2c39eded6cc1618c9955811160d9405d.png)
    
-   暂存区本地库比较
    
     命令:`git diff --cached readme.txt`
    
    ![](https://article.biliimg.com/bfs/article/1dfaa655cdac726bafe6d3c7ca9125372aed5939.png)
    
    这里缓存区和本地库没有不同所以没有内容
    
-   **补充：可以第二次提交到暂存区和本地仓库**

### git删除

|  命令                        |  描述                                                        |
|:--------------------------:|:----------------------------------------------------------:|
|  `rm file; git add file`   |  先从工作区删除文件，然后再暂存删除内容                                       |
|`git rm <file>`|  把文件从工作区和暂存区同时删除                                           |
|  `git rm --cached <file>`  |  把文件从暂存区删除，但保留在当前工作区中                                      |
| `git rm -r *`              |  递归删除某个目录下的所有子目录和文件                                        |  

删除后不要忘记提交


### 2.5git本地操作-修改撤销
当我们工作区内容想要提交到缓存区时【add】，突然发现有问题，想要撤销该如何处理？
当我们已提交到缓存区的内容，发现出现了bug，这时又应该如何处理哪？
以上操作我们可以使用GIT提供的撤销命令来完成
- 工作区撤销修改
	在你提交缓存区前，你突然发现这个修改是有问题的，你打算恢复到原来的样子。怎么办？
	使用git status 命令查看当前状态
	![](https://article.biliimg.com/bfs/article/ec1b422d8efb1a5df893ad3c3019524e2f6ee468.png)

	 命令:`git checkout  文件名称 `  
	     撤销工作区修改
	![](https://article.biliimg.com/bfs/article/34ddfde6ed1735b5160b8bf7b58ddf5d2524c2a1.png)
	我们撤销后，在查看文件中内容，发现工作区内容已经撤销，并查看状态，发现状态很干净
- 暂存区撤销修改
	使用 vim 命令 编辑readme.txt添加“我是第五行” 使用git add提交文件至暂存区
	![image-20210428095340143](https://article.biliimg.com/bfs/article/6fc29fde72389f1b8e17792cb46c8ea7ae8c9119.png)
	撤销到工作区
	 命令:`git reset HEAD readme.txt` 撤销到工作区
	
	![image-20210428095450657](https://article.biliimg.com/bfs/article/d00c50b2e529cdc63faf10dbda3389fbbebdffe2.png)
	![image-20210519111446908](https://article.biliimg.com/bfs/article/c669c9ed150ee18c0c875bc6d2d8df905b8adcb3.png)
	工作区撤销 `git checkout readme.txt`
	![image-20210428095544031](https://article.biliimg.com/bfs/article/0e5c01891e83d6d2beb3e14e4709e6dee695c2fc.png)
	我们在查看文件，发现已经恢复到最初始样子
## Git 分支操作
### 什么是分支
在版本控制过程中，同时推进多个任务，为每个任务，我们就可以创建每个任务的单独分支。使用分支意味着程序员可以把自己的工作从开发主线上分离开来，开发自己分支的时候，不会影响主线分支的运行。（分支底层其实也是指针的引用）
<img src="https://article.biliimg.com/bfs/article/a2b648bc0077a89b85fd27915741e00ae3e77e9e.png" alt="image.png" style="zoom:50%;" />
### 分支的好处
同时并行推进多个功能开发，提高开发效率。各个分支在开发过程中，如果某一个分支开发失败，不会对其他分支有任何影响。失败的分支删除重新开始即可。
### 分支 的操作

|命令名称| 作用                       |
|:-:|:-:|
|`git branch 分支名`| 创建分支                   |
| `git branch -v`        | 查看分支                   |
|`git switch 分支名` | 切换分支                   |
|`git merge 分支名`|把指定的分支合并到当前分支上|
|`git branch -d 分支名`|删除分支(前提是这个分支被合并了) |
|`git merge --abort`|停止当前正在进行的合并操作|
#### 查看分支
**基本语法**
`git branch -v`
```shell
Layne@LAPTOP-Layne MINGW64 /d/Git-Space/SH0720 (master)
$ git branch -v
* master 087a1a7 my third commit （*代表当前所在的分区）
```
#### 创建分支
**基本语法**
`git branch 分支名`
```shell
Layne@LAPTOP-Layne MINGW64 /d/Git-Space/SH0720 (master)
$ git branch hot-fix
Layne@LAPTOP-Layne MINGW64 /d/Git-Space/SH0720 (master)
$ git branch -v
hot-fix 087a1a7 my third commit （刚创建的新的分支，并将主分支 master
的内容复制了一份）
* master 087a1a7 my third commit
```
####  修改分支
```shell
--在 maste 分支上做修改
Layne@LAPTOP-Layne MINGW64 /d/Git-Space/SH0720 (master)
$ vim hello.txt
--添加暂存区
Layne@LAPTOP-Layne MINGW64 /d/Git-Space/SH0720 (master)
```
####  删除分支

合并完分支之后，如果不再使用dev分支，则可以删除此分支,先查看当前分支：
	命令 `git branch` 查看分支情况
	![](https://article.biliimg.com/bfs/article/98fb1f853d208921511766b430d941138f11c12d.png)

当前有两个分支dev与master，我们当前是在master分支上，如何删除dev分支
	命令 `git branch -d 分支名`
	![](https://article.biliimg.com/bfs/article/e8c0ae1777a27c957b660238e1643e10bf40d205.png)

我们使用`git branch`查看，发现dev分支已经被删除
#### 合并分支
**基本语法**
	`git merge` 分支名
**案例实操** 
	在master分支上合并hot-fix分支
```shell
Layne@LAPTOP-Layne MINGW64 /d/Git-Space/SH0720 (master)
$ git merge hot-fix
Auto-merging hello.txt
CONFLICT (content): Merge conflict in hello.txt
Automatic merge failed; fix conflicts and then commit the result.
```
==产生冲突时==
合并分支时，两个分支在 同一个文件的**同一个位置**有两套完全不同的修改。Git 无法替我们决定使用哪一个。必须人为决定新代码内容。
```shell
Layne@LAPTOP-Layne MINGW64 /d/Git-Space/SH0720 (master|MERGING)
$ git status
On branch master
You have unmerged paths.
(fix conflicts and run "git commit")
(use "git merge --abort" to abort the merge)
Unmerged paths:
(use "git add <file>..." to mark resolution)
both modified: hello.txt
no changes added to commit (use "git add" and/or "git commit -a")
```
解决冲突,编辑有冲突的文件，删除特殊符号，决定要使用的内容
特殊符号：`<<<<<<< `HEAD 当前分支的代码 ======= 合并过来的代码 `>>>>>>>` hot-fix
创建分支和切换分支的底层原理

<img src="https://article.biliimg.com/bfs/article/459321c366bcd21990b9099d2d39959bb550aae9.png" alt="image.png" style="zoom:40%;" />

## 回退和rebase

### rebase和merge的区别

下面是merge的过程

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20231222201427.png?" alt="image.png" style="zoom:100%;" />
合并之后的结果即使main分支多出一个提交记录

下面是rebase的过程
<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/GIF%202023-12-22%2020-13-27.gif?" alt="image.png" style="zoom:30%;" />
dev的操作都会变基到main分支上，最后的结果就是一条直线

**原理**：
在git中每个分支都有一个**指针**，指向当前记录的**最新提交记录**，当执行rebase操作时会把从当前分支的公共祖先(也就是main:3节点)到最新提交记录的所有提交，都移动到目标分支上去，就像**嫁接**一样

### rebase和merge的优缺点

- merge:
- 优点：不会破坏原分支的提交历史，方便回溯和查看。 
- 缺点：会产生额外的提交节点，分支图比较复杂。 

- Rebase 
- 优点：不会新增额外的提交记录，形成线性历史，比较直观和干净； 
- 缺点：会改变提交历史，改变了当前分支branch out的节点。避免在共享分支使用。 


## 4.Git 团队 协作
### 4.1 团队内协作

<img src="https://article.biliimg.com/bfs/article/f3c8939f326fff5e4fa9809a2b23df5f902f6df6.png" alt="image.png" style="zoom:50%;" />

* push:本地代码变更更新传送到远程仓库
* clone:用于将远程代码仓库的代码拷贝到本地，以便进行修改或查看
* pull:从远程仓库拉取**最新**的代码更改并==合并==到您当前的本地工作平台上。
* fetch:从远程存储库获取数据而不进行合并的命令。它允许你在本地存储库中获取新的提交和分支，但不会将它们合并到当前所在的分支。
### 4.2 跨团队协作

<img src="https://article.biliimg.com/bfs/article/fa2fd9a9db9833c4cd431ee9243f9388985c1838.png" alt="image.png" style="zoom:50%;" />

* fork:克隆一个开源项目，会在您的个人远程代码仓库中创建该项目的副本。
* Pull request:向开发者发送一封消息，告诉他们您已经对源代码进行了修改，并请求他们合并这些修改。
* merge:将你的更改合并到他们的代码库中去
## 5. GitHub 操作
### 5.1 远程仓库操作

|   命令名称                    |  作用                                |
|:-------------------------:|:----------------------------------:|
|  git remote -v            |       查看当前所有远程地址别名                 |
|  git remote add 别名 远程地址   |  起别名,同时建立了本地库跟远程仓库的关系              |
|git push 别名 本地分支:远程分支|推送本地分支上的内容到远程仓库 -u参数用来将本地分支与远程分支进行关联|
|git clone 远程地址 |将远程仓库的内容克隆到本地|
| git pull 远程库地址别名 远程分支名    |   将远程仓库对于分支最新内容拉下来后与当前本地分支直接合并     |
|  git fetch                |  将远程仓库的最新代码拉取到本地仓库，但并不会合并到你当前的代码中  |
|  git remote rm 别名         |  删除本地库与远程仓库的连接                     |  

- git clone 与git pulld的不同点：
	- git clone(克隆)：是在本地没有**版本库**的时候，从远程服务器克隆整个版本库到本地，是一个本地从无到有的过程。
	- git pull(拉取)：在本地有版本库的情况下，从远程库获取最新commit数据（如果有的话），并merge（合并）到本地。

<font color=#ff0000>注意:</font>在**推送**代码前必须**先拉取**代码，否则无法推送本地仓库代码到远程仓库，并且首次拉取需要添加:`--allow-unrelated-histories`

当`git clone` 别人的项目后,进行更改后`git push` 需要权限,也就是需要别人将你加入它的**库**的成员

<img src="https://article.biliimg.com/bfs/article/f1400b60ab79e83bed7028b6ce402fd82ae04378.png" alt="image.png" style="zoom:50%;" />

 **填入邀请的合作人**
 ![image.png](https://article.biliimg.com/bfs/article/4007754a320452d79d0b40931dcdcd532a2a7585.png)
这样操作之后会产生一个邀请链接,将这个链接发给对方就行
![image.png](https://article.biliimg.com/bfs/article/829e026a835c74e3381e7edb8efabb94b9078e4c.png)
## 6.VScode集成git
### vscode原始git
vscode的git init

<img src="https://article.biliimg.com/bfs/article/a957ccc8c57ecb3686d49d76287d485bb64e92ea.png" alt="image.png" style="zoom:67%;" />
在vscode的git add
<img src="https://article.biliimg.com/bfs/article/4d98d8fe2abcfcb4eaf831f8d8ea443fe25ae2bb.png" alt="image.png" style="zoom: 67%;" />
状态有三种：

-   **U：** 表示文件存在于本地文件系统中，但是还未被添加(add)到git版本控制,就是在本地**新建了**这个文件，还未提交到github上，就会标记U
-   **M：** 表示文件已被修改。你需要提交(commit)这个修改才能保存这个变化，并将其推送(push)到远程git仓库。
-   **A：** 表示一个新文件已经被添加(add)到git版本控制中。 一般情况下，当你新建了一个文件并执行完`git add.`命令之后，就会看到这个标志状态。
当我们添加修改后,点击修改变化可以看到改变的部分
<img src="https://article.biliimg.com/bfs/article/9d6c99380c6602f2bfbd6543574c865e7f1ed4ea.png" alt="image.png" style="zoom:50%;" />

还可以lnline View内联显示,并且在下面的main分支的git图标可以**新建、重命名和切换**分支
<img src="https://article.biliimg.com/bfs/article/d3d50ddb5f93809c66ffae57a3bbd582b09a1fd2.png" alt="image.png" style="zoom:45%;" />

git commit 后vscode会出现`Sync Changes` ,这代表这push 1 commits to orgin/main,这里的 orgin/main 在使用git fetch或者git pull命令从远程仓库获取代码更新时，通常情况下会产生一些追踪分支。这些追踪分支的名称格式为： 远程主机名/远程分支名。
<img src="https://article.biliimg.com/bfs/article/32fe11c29bfdad92aba710880a34c70511a8a545.png" alt="image.png" style="zoom:80%;" />
git的其他命令都可以找到
<img src="https://article.biliimg.com/bfs/article/2c624a8e54f49421f6af2a4c6123fe3d6780c942.png" alt="image.png" style="zoom:50%;" />

### GitLens插件
当前行的末尾添加一个非显眼的 blame 注释，以显示这一行的贡献者及其 Git 提交信息（包括提交人的姓名以及提交时间，SHA 码等）,点击还可以对比代码
<img src="https://article.biliimg.com/bfs/article/7ebcfb9c35df9d0dbbd98467a93fb6b664b9f959.png" alt="image.png" style="zoom:50%;" />
在状态栏添加作者信息
<img src="https://article.biliimg.com/bfs/article/3cbde3c82e17ca2e773d389f9d72293b45d60b18.png" alt="image.png" style="zoom: 50%;" />
在GitLens的面板中我们可以看到所有的提交并且进行对比---方法是右键点击`Select for compare` 另外一个点击`Compare for  Select`
<img src="https://article.biliimg.com/bfs/article/7ced597fe637cba2bd8fa4c805022085620b32a4.png" alt="image.png" style="zoom:80%;" />
在对比不同提交是,alt+这个图标可以任意选择commit对比
![image.png](https://article.biliimg.com/bfs/article/a11ad001a5f42f5eff117909ca234dc5419ab301.png)

## 7.IDEA集成git
### 初始化
- 基本操作-初始化工作区
	- 点击VCS --> Create Git Repository
	- ![](https://article.biliimg.com/bfs/article/ad46a5d21f572b358165cc4ef28600f63bd9dfc1.png)
	**点击左下角，Git菜单，此时day0901_git下所有的文件都变成棕色,说明我们的工作区添加完成了**
	<img src="https://article.biliimg.com/bfs/article/c9a5e091c022e30d2038783b3f12356b824677d9.png" alt="image-20211110160452517" style="zoom: 50%;" />
	<font color=#ff0000>注意：</font>棕色代表文件未被**未追踪**（untracked)的文件。
- 忽略文件类型
	**从version control中我们可以看到有一部分文件，我们是不需要提交到本地仓库中去的**
	<img src="https://article.biliimg.com/bfs/article/e6b10e5258aee5a5bfe6f4e4df6070e9b7571ae3.png" alt="image-20210523200225766" style="zoom:50%;" />
	**那我们怎么做呢？可以拷贝"资料"中.gitignore文件，到gitProject的根目录：**
	<img src="https://article.biliimg.com/bfs/article/e13df5dd0934a3ebaad4800403aec02b1420d178.png" alt="image-20211110160640332" style="zoom: 67%;" />
	**这个时候你会发现，多余的不需要提交的文件类型被忽略了。如果有新的要忽视的文件类型，你可以在.gitignore中添加**
### 基本操作add与commit
- 工作区提交暂存区 add
	选中gitProject项目,右键
	![image-20210523203451621](https://article.biliimg.com/bfs/article/1af83f299e6223dd4b10b2f5fb1e5f928af485b8.png)
	<font color=#ff0000>注意：</font>可以看到Git中的文件颜色由棕色变成的==绿色==，被**追踪**了
	![image.png](https://article.biliimg.com/bfs/article/90078c3ee1ad1fc5fe0e0d99be514845017f26fd.png)

- 暂存区提交本地仓库 commit
	点击右下角Version control面板中，选中你要提交的文件，这里我都需要提交，使用全部选中点击鼠标右键
	![image-20211110160949435](https://article.biliimg.com/bfs/article/1b2951955b133fa6774164dc2124defc2f38f253.png)
	<font color=#ff0000>注意：</font>书写日志信息后，文件变为==黑色==，本地changes面板也会被清空
### 差异化比较

- 工作区与本地仓库比较

	在Version Control中选中HelloWorld.java右键：
	![image-20211110161306919](https://article.biliimg.com/bfs/article/c7c2e4b82f54bfdfa6c7c2b5a9fb69b0a0a075f6.png)

	![image-20210523222504160](https://article.biliimg.com/bfs/article/035f0bc290b199f2a284e138ca7b246fc53028f0.png)
	![image-20210523225324524](https://article.biliimg.com/bfs/article/80d229ddc98156510649a47e412ef7c8bc188bc0.png)
	点击左下角Git--->log,就可以查看提交记录
	![image-20211110161450315](https://article.biliimg.com/bfs/article/4fcf932c61afefd8b779d5fabbb67f4baaa3c8f4.png)
	<font color=#ff0000>注意：</font>==蓝色==的文件代表该文件已经被追踪（tracked）了，但是当前工作目录下的文件内容与 Git 仓库中存储的版本不一样。也就是说，该文件在你所在的本地分支上有了修改，但还没有被添加到Git暂存区。
### 版本回退及撤消
再提交两次后
在右下方Git点击log，此时我们可以看到3个提交的版本
- 本地仓库回退撤消
	![image-20211110162000916](https://article.biliimg.com/bfs/article/249e1cf40a5d4d04680eb5ee8cf33aff7945a9ac.png)
	现在我们在本地仓库中回退到第二次提交,**选择第二次提交的标记**，右键
	![image-20211110162054092](https://article.biliimg.com/bfs/article/c6295c18e9d5434c99fa080cfe161608a02a9b64.png)
	选择Hard
	<img src="https://article.biliimg.com/bfs/article/7f48e6df7d0e052460758b93413bda00d1231c4d.png" alt="image-20210428163800014" style="zoom:50%;" />
	代码也回退了
	<img src="https://article.biliimg.com/bfs/article/3b041df1b43b6d3d1512458a0b9342cc83f27be1.png" alt="9" style="zoom:50%;" />
- 工作区撤消

	当我们在工作区编辑代码时候，希望撤销未提交**本地仓库**的代码时候
	![](https://article.biliimg.com/bfs/article/a995c5fb70a7718a59606e0b8b0258331c85890e.png)
	弹出如下窗口
	<img src="https://article.biliimg.com/bfs/article/daae3f4895f5ed5fc47907309a007fffb842852c.png" alt="image-20211110162620907" style="zoom:50%;" />
	点击Rollback,代码则撤销
![image-20211110162719338](https://article.biliimg.com/bfs/article/99e647739196346544a4ef967997ebd992a79a58.png)

### 创建与关联远程仓库

- 关联远程仓库
	Git--->Manage Remotes ....
	<img src="https://article.biliimg.com/bfs/article/744255754c1cfb03f5d4a080451ce61171e72227.png" alt="image-20211110162832989" style="zoom:50%;" />
	
	点击之后弹出窗口，点击+
	<img src="https://article.biliimg.com/bfs/article/bfa4f9b0f5c63729b6ebcf1105ba1e4880760e07.png" alt="image-20211110162944914" style="zoom:50%;" />
	复制github的https地址

### 远程仓库-拉取、推送、克隆
点击菜单栏git可以看到一系列菜单
![image.png](https://article.biliimg.com/bfs/article/5a4d7973db4d7f8011102a5ad1ef991fb22e5dfa.png)
注意pull失败后需要merge后
【1】

![image-20211206144219222](https://article.biliimg.com/bfs/article/1799c20d309b36fe2b5fe89706a2301f7af6dbaf.png)

【2】

![image-20211206144251019](https://article.biliimg.com/bfs/article/eb075ac67451ba20604126652f6c2b441a6ef060.png)

【3】

<img src="https://article.biliimg.com/bfs/article/ed1e0a17a6c34976686e44ffa06549f34e456009.png" alt="image-20211206144324527" style="zoom:50%;" />

### 分支-创建、合并、删除分支
分支操作都IDEA在右下角
<img src="https://article.biliimg.com/bfs/article/76f409b4c50e0ba6e52117a5ee0cea846c5f8d79.png" alt="image.png" style="zoom:50%;" />







