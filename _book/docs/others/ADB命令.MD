# 7.7 ADB常用命令

###1、显示当前运行的全部模拟器(含真机设备)：
      adb devices

###2、从PC端安装包
      adb install   路径\***.apk

###3、包从PC端推送到手机端
     adb push E:\123.zip /sdcard/123/xxx.zip

###4、查看某个应用下的日志：
     adb logcat | grep "xxx", xxx是包名

###5、查看包名：
     adb shell pm list packages

###6、查看某包下的日志：
     adb shell "logcat|grep com.dwd.rider"

###7、log的日志级别
#####Log信息级别
- Log.v - VERBOSE  : 黑色
- Log.d - DEBUG  : 蓝色
- Log.i- INFO   : 绿色
- Log.w- WARN   : 橙色
- Log.e- ERROR   : 红色
###8、日志信息输出加上日志级别
adb shell "logcat *:E |grep com.dwd.rider"
###9、查看某个包下的PID
adb shell  "ps|grep 包名"
###10、获取设备的ID和序列号：
     adb get-product 
     adb get-serialno
###11、访问数据库SQLite3
     adb shell 
     sqlite3