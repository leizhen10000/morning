# xshell


####前言：看日志很轻松的，只要掌握两点就好了：怎么去看某个特定的日志，期望的日志在哪看。

###一、首先解决第一个问题，这是工具和Linux常用命令的使用  
**工具**：跳板机（xhell工具必须要有）[空降下载地址](https://www.netsarang.com/download/down_xsh.html)
 所有不同服务的日志都以跳板机作为入口，当连上 192.168.11.21:22的时候，默认进入跳板机，如下

![](http://upload-images.jianshu.io/upload_images/4350463-d6f8e15f5cd1074d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 查看具体日志：（以查看抢单为例）
----
##### 第一步，Opt or ID>: p 进入机器选择界面，选择对应机器，2；

![](http://upload-images.jianshu.io/upload_images/4350463-87c1be7645dcc7e8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#####  第二步：直接进入日志目录，

![](http://upload-images.jianshu.io/upload_images/4350463-cb8c8bf2c737ff84.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
##### 第三步：进入目标模块所在路径：
![](http://upload-images.jianshu.io/upload_images/4350463-e4dfc93c98f8e750.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
##### 第四步：根据 order_id 查看对应订单调度信息：

![](http://upload-images.jianshu.io/upload_images/4350463-2f3a40754e1ea761.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
<br/><br/>

###二、查看日志基础命令
1. less \[参数] 文件  
打开日志，对日志内容全局搜索  
（常用功能）然后输入 /订单编号，通过n进行向下匹配  
（ \[/字符串]：向下搜索“字符串”的功能，具体有关less使用请移步
  http://www.cnblogs.com/peida/archive/2012/11/05/2754477.html)  
其实要看出错日志，最简单的就是通过 less就能定位，所以推荐使用less
 
2. cat \[选项]\[文件]  
cat主要是连接文件并打印输出文件  

3. grep \[选项]\[文件]  
主要用于筛选特定字符所在的行主要的用法就是在日志文件中搜索目标 order-id 所在的行。详见：http://www.cnblogs.com/peida/archive/2012/12/17/2821195.html  
如果特定的字符不明显，可以用 --color后缀，或者xshell搜索功能凸显内容。

4. exit在切换不同的机器时，直接输入exit就能跳出当前机器，重新进入跳板机选择界面  
5. ls,ll 查看文件下所有文件信息  

6. 小技巧  
每次进入不同系统查看日志，都要输入cd /home/appqa/deploy/logs/，为方便起见，可以使用xshell工具：
菜单栏--》查看--》快速命令--》最下面状态栏--》添加快速命 
![](http://upload-images.jianshu.io/upload_images/4350463-91754adae67a4dee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  <br/><br/>
最终结果如下：  <br/><br/>
![效果图](http://upload-images.jianshu.io/upload_images/4350463-774f7cb60a6705b7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

7. 查看出错日志方法：（以对接相关日志为例）  
首先根据下文日志分区，进入对应渠道日志路径 /home/appqa/deploy/logs/sparta-web  
可以看到当前订单是在20:28:50:300产生的，而这个订单出现了问题，在当前截取日志中是无法看到具体内容的，所以再通过时间这个关键字取查找问题。  
通过关键字分组信息来锁定订单：  
![锁定订单](http://upload-images.jianshu.io/upload_images/4350463-ab30dd5a27f82dd9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  



### 三、各个分区日志查看介绍  

- **查看渠道回调信息：**
 **使用场景**：对接渠道订单的过程中，需要确定，是否从点我达实时回调状态给渠道方。此时就需要看回到日志  
&emsp;**日志所在位置**: p-->1-->iron-global  
&emsp;通过渠道id:channel_id 查看，具体过程如下：  
![](http://upload-images.jianshu.io/upload_images/4350463-1cd689b6d5ff2a41.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
*然后使用上面查看日志方法食用更佳。*

- **查看渠道订单下单日志**  
**使用背景**：订单经常会下单不成功，这个时候，需要去看对应的【下单】接口或者【下单订单】  
&emsp;**日志所在位置**：p-->0-->hulk-->hulk-api.log  
&emsp;**现在日志迁移到了新位置**：p-->6-->sparta-web-->sparta-web.log

- **查看骑手相关日志（抢单、订单详情等）**  
&emsp;**命令**：cat flash-day.log | grep rob-order-list.json  --color  
&emsp;**日志所在位置**：p-->0-->flash-->flash-day.log

- **商家相关日志**：p-->0-->spider-->spider-api.log

- **调度相关日志**：p-->2（*用的最多*）  
dispatch 派单模式日志   dispatch-provider 抢单模式日志

- **查看回调日志** : p-->1  
iron-global  



- **日志格式相关**:  
命名规则：***.log.YYYY--MM-dd--hh   
以 sparta服务日志为例 sparta-web.log.2017-05-22-20  
其中最常用的就是 sparta-web.log 日志文件：对应当日，该时间段（整小时）内所有日志文件；  
如果要查看上一个小时的文件，就用命名规则取匹配对应时间日志

- **揽件相关日志**：  
p-->12/app012-->docking-alibaba  


