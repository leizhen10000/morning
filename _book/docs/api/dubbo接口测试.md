
**前言 dubbo 接口测试和普通的HTTP接口测试过程是一样，只不过连接方式不同，
下文主要就介绍怎么样连接一个dubbo服务**<br/>
**提示：当前 dubbo 服务只能在开发环境先调通，然后推到测试环境跑，目前限制如此**<br/>
#### 主要内容
1. 了解 dubbo 概念以及简单调用方法
2. 在当前测试框架中，利用 Spring 框架连接 dubbo 接口

### 一、 Dubbo 架构简介
Dubbo 是阿里巴巴提供的分布式、高性能、透明化的 RPC 服务框架.<br/>
Dubbo 依赖 zookeeper 服务，在本框架中使用 Maven 对项目进行管理，所以对应的服务
jar包都要在 pom.xml 文件中引入；具体在先买你内容会详细介绍。

下面的 demo 主要介绍 Dubbo 之间服务如何进行远程通信的：
- 首先 定义一个远程服务接口
- 提供者 provider 发布远程服务到注册中心 zookeeper
- 消费者 consumer 发现远程服务并完成服务调用

###### 定义接口并实现：
&emsp;<font color="#FFB86C">DemoServiceImpl.java</font><br/>
```
public class DemoServiceImpl implements DemoService{
    public String sayHello(String name){
        return "Hello" + name;
    }
}
```
##### 暴露接口
&emsp;<font color="#FFB86C">dubbo-demo-provider.xml</font>
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://code.alibabatech.com/schema/dubbo http://code.alibabatech.com/schema/dubbo/dubbo.xsd">

    <!--  提供方应用信息，命名应用名称 -->
    <dubbo:application name="demo-provider"/>

    <!--  使用zookeeper广播注册中心，暴露服务地址-->
    <dubbo:registry address="zookeeper://10.0.0.10:2181?backup=10.0.0.17:2181,10.0.0.5:2181"/>

    <!--  用 dubbo 协议在 20880 端口暴露服务 -->
    <dubbo:protocol name="dubbo" port="20880"/>

    <!--  申明需要暴露的服务接口-->
    <dubbo:service interface="com.alibaba.dubbo.demo.DemoService" ref="demoService"/>

    <!--  和本地一样实现服务-->
    <bean id="demoService" class="com.alibaba.dubbo.demo.provider.DemoServiceImpl/>

 </beans>
```
##### 消费者调用服务
&emsp;dubbo-demo-consumer.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://code.alibabatech.com/schema/dubbo http://code.alibabatech.com/schema/dubbo/dubbo.xsd">

    <!--  消费方提供应用名，尽量跟提供方不一样-->
    <dubbo:application name="demo-consumer"/>

    <!--  使用 zookeeper 广播注册中心暴露发现服务地址-->
    <dubbo:registry address="zookeeper://10.0.0.10:2181?backup=10.0.0.17:2181,10.0.0.5:2181"/>

    <!--  生成远程服务代理，可以和本地bean一样使用 demoService-->
    <dubbo:reference id="demoService" interface="com.alibaba.dubbo.demo.DemoService/>

</beans/
```
##### 本地调用远程服务
&emsp;Consumer.java
```
public class Consumer{
    public static void main(String[] args) throws Exception{
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(
        new String[] {"META-INF/spring/dubbo-demo-consumer.xml"});
        context.stat();

        DemoService demoService = (DemoService)context.getBean("demoService");  // 获取远程服务代理
        String hello = demoService.sayHello("world");   //执行远程方法

        System.out.println(hello);  // 显示调用结果
    }
```

### 二、框架具体运用
<font color="#1DB0B8">以调度抢单服务的一个 RPC 接口为例</font><br/>
<font color=gray>Dubbo接口对应的名称，最好找对应服务的开发</font><br/>
##### 1.确定接口所在服务
进入 DubboMonitor 中http://192.168.11.39:7069/services 账号和密码admin/admin<br/>
进入 Service 查找服务，例如 **GrabProviderV2**<br/>
![使用DubboMointor查找服务](/resource/photo/查找服务.jpg)
##### 2.获取服务对应jar包
 进入 Sonatype Nexus中 http://192.168.1.177:8081/nexus/#nexus-search;quick~ <br/>
 同样通过关键字查找对应jar包<br/>
 ![查找jar包依赖地址](/resource/photo/查找jar包依赖地址.jpg)
 ##### 3.通过构建消费者调用服务
 &emsp;当前框架中暂定为 app-context-dubbo.xml<br/>
 ```
 <?xml version="1.0" encoding="UTF-8"?>
 <beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
        xmlns:context="http://www.springframework.org/schema/context"
        xsi:schemaLocation="http://www.springframework.org/schema/beans
 		http://www.springframework.org/schema/beans/spring-beans.xsd
         http://code.alibabatech.com/schema/dubbo
         http://code.alibabatech.com/schema/dubbo/dubbo.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    <!--  消费者 consumer 名称-->
    <dubbo:application name="Auto-test-comsumer"/>

    <!--  zookeeper 地址-->
    <dubbo:registry address="zookeeper://192.168.11.29:2181?backup=192.168.11.32:2181,192.168.11.20:2181"/>

    <!--  指定远程服务 -->
    <dubbo:reference id="dispatchGrabProvider"
                         interface="com.dianwoba.redcliff.dispatch.provider.V2.GrabProviderV2" timeout="10000"/>

</beans>
 ```
 配置文件详解：<br/>
 &emsp;首先这个 消费者对应的 name 和 address 都已经设置好了，所以要做的就是添加
 远程服务。<br/>
 添加远程服务中，id 自己命名，但是最好跟下面的 interface 意思相近，interface 中
 属性，为步骤一中查询的结果，对应的Service复制进来就可以。<br/>
 注意，复制完后，直接会报错，因为当前 maven 没有引入对应的服务 jar 包，这个时候就
 要在pom.xml文件中添加jar依赖。<br/>
 ![添加pom文件](/resource/photo/添加pom文件.jpg)<br/>
 只要在 <dependencies></dependencies>标签内添加步骤二中的依赖就可以。这个时候
 返回之前xml文件就会发现包引入成功，不再报错<br/>

 ##### 4.本地调用远程服务
 上一个步骤中已经构建了远程服务的消费者，需要通过 Spring 来获取对应的服务并调用。<br/>
```
/* 在接口调用类中可以直接写入*/
public void test() {
        ApplicationContext context = new ClassPathXmlApplicationContext("app-context-dubbo.xml");
        // 接口名称构建对应的 Service
        GrabProviderV2 grabProviderV2 = (GrabProviderV2) context.getBean("dispatchGrabProvider");
        List<Long> idList = new ArrayList<>();
        idList.add(230597L);
        // 然后直接通过接口调用对应要测的方法名，在上一步过程中已经实现远程获取，当前可以作为本地的一个类调用
        System.out.println("结果>>>>>\n" + grabProviderV2.appendOrdersToGroup(554, "1503915821115", 36, 1, idList));
    }
```
最终输出的结果如下<br/>
```
....
2017-08-29 14:33:53 [main]-[INFO]  [DUBBO] Start NettyClient dwd-leizhen/192.168.100.35 connect to the server /192.168.11.40:21882, dubbo version: 2.5.3, current host: 192.168.100.35
结果>>>>>
FAILED_OTHER
```
“....” 代表的 Dubbo 相关的调用过程，不用去管，最后返回的结果就是 FAILED_OTHER,
返回了失败报错码。<br/>
整个调用dubbo RPC 接口就完成了，过程非常简单明了。

部分内容参考至 http://dubbo.io/ <br/>
@Powered by GrayHouse

