# SourceTree 使用

##### 背景：从公司的gitlab上clone托管的项目，并通过SourceTree查看并管理项目。（以autotestSharing项目为例）

## 前提操作——添加公钥

##### 1. 首先贴上GitLab对应的项目地址 http://192.168.1.66:9090/test/autotestSharing ,如果没有权限请联系管理员。
##### 2. 添加 SSH Key
1. 首先点击左下角名字进入个人中心，然后进入 Profile Setting，在菜单栏选择 SSH Keys；
2. 在页面上点击 generate it；
 ![generate sshkey](/resource/photo/gitlab设置ssh.jpg)
3. 生成 ssh key 的命令在dos中无法执行，最好在git中执行命令，所以需要安装git，空降 [git下载地址](https://git-scm.com/downloads)；
4. 安装完成后，就可以使用 git 工具了，在资源管理器任意位置右键，点击 Git Bash Here，启动命令窗口，输入命令 ssh-keygen -t rsa ，一直回车直到结束；
5. 进入 C:\Users\用户名\.ssh\id_rsa.pub 中，右键使用notePad或者editPlus查看，复制文本。
![ssh](/resource/photo/ssh所在位置.png)
![ssh内容](/resource/photo/ssh内容.png)
6. 返回第二步进入的页面，添加后结果如下图：
![result](/resource/photo/添加sshkey及结果.png)
7. 由此添加公钥成功，以后这台设备就可以无密码操作git。
8. 使用窗口工具 SourceTree 来查看并管理项目。


## SourceTree 使用教程
#### SourceTree [下载地址](https://www.sourcetreeapp.com/),下载后需要翻墙注册；
#### 克隆git库
![clone](/resource/photo/克隆代码.png)
![](/resource/photo/输入git库地址.png) 
###### 注意：clone远程git库的过程中，可能会碰到无法加载的情况，按下面方法操作
1. 问题描述：  
![问题描述](/resource/photo/clone时问题.jpg)
2. 解决方法：首先进入SourceTree设置，然后修改公钥获取方式  
![设置](/resource/photo/sourceTree设置.jpg)![](/resource/photo/设置sshkey.png) 

#### 界面展示介绍
界面左侧是分支情况，主要包含远程分支和本地分支，开发过程中主要对本地分支进行，待阶段性成果完成后合并到主干分支master，并推送到远程分支即可。
![clone reuslt](/resource/photo/界面展示.jpg)  

## 小提示
 
##### *如果不知道git地址，可以通过搜索的结果进入项目,在gitlab任意界面搜索项目group：test，进入查询结果后，项目主界面默认显示ssh地址*
![搜索项目](/resource/photo/gitlab搜索项目.png)
![git地址](/resource/photo/git地址.jpg)

#### 具体切分支或者合并推送代码可以参考 http://blog.csdn.net/chenyufeng1991/article/details/51347083 如果教程中有不懂的地方可以询问测试开发同学。