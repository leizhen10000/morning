# 5.5 selenium 简单实践

###一、环境搭建

#####1、JDK（略）

#####2、IDEA（略）

#####3、MAVEN（略）

#####4、新建MAVEN工程，需要加载的jar包有：
         1）selenium相关：selenium-server、selenium-java、selenium-firefox-driver
         2）chrome驱动相关：chromedriver-dep
         3）testng相关：testng
         4）log4j相关：log4j、log4j-core、slf4j-log4j12
#####5、引入testng.xml文件
         1）下载插件：https://plugins.jetbrains.com/plugin/9556-create-testng-xml
         2）安装插件

  ![引入testng文件](/resource/photo/selenium/引入testng文件.png)
                  
         3）重启IDEA，然后项目右键，就看到了
 ![插件使用](/resource/photo/selenium/插件使用.png)
                
         4）刷新下工程，再根目录下能看到testng.xml文件，并且已自动把当前已经写好的@test用例加入到testng.xml中

#####6、test-output目录设置并输入testng报告

![输出报告](/resource/photo/selenium/输出报告.png)

设置完成<br/>

当运行testng.xml时，会在test-output下生成测试报告<br/>

![测试报告结果](/resource/photo/selenium/测试报告结果.png)

###二、基础框架

#####1、页面类和用例类分开

![测试框架](/resource/photo/selenium/测试框架.png)

#####2、数据驱动

###三、常用知识点

#####1、元素定位方式

- find_element_by_id<br/>
- find_element_by_name<br/>
- find_element_by_xpath（较为通用，别的方式无法获取到时，可采用XPATH）<br/>
- find_element_by_link_text<br/>
- find_element_by_partial_link_text<br/>
- find_element_by_tag_name<br/>
- find_element_by_class_name<br/>
- find_element_by_css_selector<br/>

#####2、常用事件

单击：click<br/>
输入框输入值：sendKeys<br/>
是否显示：isDisplayed<br/>
清楚：clear<br/>

###四、遇到的问题

#####1、无法启动chrome浏览器

        原因：
        1）chromedriver.exe  驱动与浏览器的版本有对应关系，一定要和你本地chrome对应版本的驱动才可以
        2）需要启动配置
        System.setProperty("webdriver.chrome.driver","C:\\Program Files (x86)\\Google\\Chrome\\Application\\chromedriver.exe");

#####2、CRM系统->配送员管理->配送员清单，无法定位到

        原因：
        点击【配送员管理】后，下级页面还没渲染好，就直接去定位【配送员清单】，无法被获取到，需要加一定的渲染等待时间
        Thread.sleep(5000);

#####3、【配送员清单】的搜索项中，城市的下来选择框无法定位到

         用chrome自带的开发者工具去直接定位是无法定位到的，需要
         ctrl+F5，根据关键字搜索下拉选择中的关键字，比如“常用”，才能展开并定位到具体的元素信息；然后直接用XPATH来定位即可


