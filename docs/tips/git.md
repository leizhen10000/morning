# 6.1 git的使用



# git 使用
git 是现在比较流行的版本控制工具。跟传统的 CVS、Subversion 一类的集中式版本控制工具不同，它不需要服务器段软件，就可以做版本控制，换句话说就是处处皆仓库，你可以断开网络独立对对应分支做操作。同时git拥有合并追踪（merge tracing）能力。

## 主要使用过程
__git 日常使用的精髓: “一提交，二拉取，三推送”commit pull and push__ 
### 检出仓库
执行如下命令可以创建一个本地仓库的克隆版本：  
> git clone /path/to/repository  

如果是远程的服务器仓库，你的命令就是ssh地址或者https地址：   
> git clone username@host:/path/to/repository  

### 工作流
操作上面的 clone 之后，就有了本地仓库，本地仓库由 git 维护的三棵“树”组成。  
第一个是你的<font color=red>工作目录</font>，它持有实际文件；第二个是<font color=red>暂存区(Index)</font>，它像是一个缓存区域，临时保存你的改动；最后是<font color=red>HEAD</font>，它指向你最后一次提交的结果。  
![git 工作流](http://rogerdudler.github.io/git-guide/img/trees.png)  

### 添加和提交
你可以提出更改（把他们添加到暂存区），使用如下命令：  
> git add <filename>  
> git add *  

这是 git 基本工作流程的第一步；    
使用如下命令以提交实际改动：  
> git commit -m "代码提交信息"  

现在，你的改动已经提交到了 HEAD，但是还没有到你的远程仓库。

### 推送改动
执行如下命令，将当前所有改动提交到远程仓库：
> git push origin master


#### 上面的三个流程都是正常情况下进行的操作，实际上，日常工作中，经常会碰到处理冲突和合并分支的情况。

### 分支
通过下面命令创建分支：  
> git checkout -b feature_x  

然后可以通过下面命令切换到主分支：  
> git checkout master  

注意：在切换分支前，必须要验证本地修改  ```git status```  

### 合并分支  
一般先通过更新远程分支，如果远程分支由改动，就需要合并到自己本地中  
> git pull 更新本地仓库至最新改动  
> geit merge <branch> 合并 branch 分支到当前分支

### 处理冲突
如果本地修改某个文件 a.txt，同时远程也对该文件进行修改，那么在进行上面的合并分支操作是就会发生confilcts。  
这个时候就需要处理 冲突confilct，修改好冲突后继续使用“一提交，二拉取，三推送”操作

### 查看日志
如果想了解日志内容：
> git log  
> git log --author=bob  
> git log --author=bob -- pretty=oneline  
> git log --name-status


## 
# gitignore 的使用方法

## .gitignore 文件的创建
在git仓库下创建一个.gitignore文件，但是win环境中，无法命名.开头的文件，所以需要通过git环境，按照Linux的方式来创建
1. 打开gitbash
2. 输入 **touch .gitignore**，就生成了".gitignore"文件
3. 输入 **vim .gitignore**启动编辑器

## gitignore使用
有些时候，我们必须把某些工作放到 Git 工作目录中，但又不能commit上，比如.idea/下的文件，每次提交都会有修改。

为解决这个问题，就是通过.gitignore文件让Git自动忽略这些文件。Git已经十分强大，甚至不需要我们自己编写该文件，GitHub中有各种模板，然后根据自己需求，更改一下模板就好了，对应的地址为 https://github.com/github/gitignore .

#### 忽略文件的原则是：
1. 忽略操作系统自动生成的文件（这个暂时没有接触到）；
2. 忽略编译生成的中间文件、可执行文件（如java产生的.class文件）；
3. 忽略带特殊信息的文件（如数据库的配置）。

#### 忽略文件的写法：
- 若test文件下所有内容：
test/
- 若test下有test1、test2且不track test3   
    test/test1  
    test/test2  
    !test/test3  

#### Git 中文件忽略形式
1. 共享式忽略  
新建 .gitignore 文件，房子啊工程目录任意位置均可。.gitignore 文件可以忽略自己，同时忽略的内容*只针对未跟踪文件有效*，对已经加入版本库的文件无效。

2. 独享式忽略  
针对具体版本库：.git/info/exclude 针对本地全局 git config --global core.exclude ~/.gitignore


#### 忽略的语法规则(类似于正则)
(#) 表示注释

(*) 表示任意多个字符

(?) 表示一个字符

([abc]) 代表可选字符范围


## **Git忽略规则不生效怎么办**
如果在项目开发的过程中，突然想忽略某个文件，但修改.gitignore后发现并没有成功

原因在于：这些文件已经被纳入了版本管理中，所以修改.gitignore后是无效的

解决方法：把本地的缓存删除（改为track状态），然后再commit
```
git rm -r --cached .
git add .
git commit -m 'update .gitignore'
```

# git 设置别名
git 中有很多命令使用频率较高，而且经常会输错，这个时候启用别名 Alias就可以起到很好的效果  
以 git status 为例  
```git config --global alias.st status```  
然后就可以查看效果了   
> git st  
> On branch dev  
> nothing to commit, working tree clean  

同时checkout，commit，等命令都可以通过简写来完成  
```
git config --global alias.ch checkout  
git config --global alias.co commit
```

# 配置文件
配置 Git 的时候，加上 --global配置的是全局参数，对当前计算机所有的Git仓库都适用。  
如果不加就只针对当前的仓库起作用，每个仓库Git配置文件都放在./git/config文件中。  
如果配置不想在用了，就通过
```
git config --unset alias.co
```

本文参考至 维基百科 git，[git简明教程](http://rogerdudler.github.io/git-guide/index.zh.html)