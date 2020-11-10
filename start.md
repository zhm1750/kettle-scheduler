背景
Kettle作为用户规模最多的开源ETL工具，强大简洁的功能深受广大ETL从业者的欢迎。但kettle本身的调度监控功能却非常弱。Pentaho官方都建议采用crontab(Unix平台)和计划任务(Windows平台)来完成调度功能。所以大家在实施kettle作业调度功能的时候，通常采用以下几种方式：使用spoon程序来启动Job，使用crontab或计划任务，自主开发java程序来调用kettle的类库。

项目介绍
Kettle调度监控平台（以下简称KS）是一个自主开发的javaweb程序，专门用来调度和监控由kettle客户端创建的job和transformation。KS整体的框架是由spring+sprin gmvc +beetlsql整合而成，通过调用kettle的API来执行转换和作业，并且使用quartz框架完成调度工作。

此版本基于kettle-8.0.0.0-28版本的API开发的，目前可以基本支持所有的组件，包括大数据组件（hbase、hive、hdfs等）。

项目源码：https://github.com/zhaxiaodong9860/kettle-scheduler（不要忘了给个star哦）

发布版本：https://pan.baidu.com/s/1DX2aCLlOIieHjuNcwn2_-w 提取码 提取码: 52r8 

kettle8.0工具下载地址：点击下载

部署
1.基础环境

         操作系统：windows（linux类似）

         预装软件：jdk1.8、mysql、tomcat、kettle8.0

2.将源码中kettle-scheduler.sql导入mysql数据库。



3.将源码编译打包后解压到tomcat下的webapps目录下。



4.配置km\WEB-INF\classes\resource\db.properties

jdbc.driver=com.mysql.jdbc.Driver   //mysql驱动
jdbc.url=jdbc:mysql://192.22.107.97:3306/kettle-master?serverTimezone=UTC&characterEncoding=utf8&useUnicode=true&useSSL=false   //mysql的jdbc url
jdbc.username=root  //mysql用户名
jdbc.password=123456   //mysql密码
 

5.配置km\WEB-INF\classes\resource\ kettle.properties

# Kettle Properties  
#绝对路径，用于初始化kettle环境变量（.kettle/kettle.properties所在路径）,指向kettle根目录（例如 D:\data-integration）
kettle.home=D:\\data-integration
#绝对路径kettle下plugins文件
kettle.plugin=E:\\zhaxiaodong\\apache-tomcat-9.0.12\\bin\\plugins
#相对路径，不需要改，暂时没有查出有什么用
kettle.script=Html\\js\\libs\\url
#日志级别
kettle.loglevel=detail
#kettle日志存放路径
kettle.log.file.path=D:\\data-integration\\logs
#保存上传文件转换(.ktr)或作业(.kjb)的路径,此功能未调试,暂时停用,待开发
kettle.file.repository=D:\\data-integration\\test
 

6.需要用到大数据组件的：将data-integration目录下的simple-jndi、system和plugins文件夹拷贝到apache-tomcat-9.0.12\bin目录下

不需要用到大数据组件的：将kettle-scheduler/src/main/resources目录下kettle-lifecycle-listeners.xml和kettle-registry-extensions.xml删除。

7.启动tomcat

          Windows:apache-tomcat-9.0.12\bin\startup.bat;

                      Linux: apache-tomcat-9.0.12\bin\startup.sh;

8.访问http://localhost:8080/km进入系统。注意：km为解压到tomcat/webapps下的项目的文件夹名称，一般源码编译后为kettle-scheduler，即可访问http://localhost:8080/kettle-scheduler。localhost为部署的服务器IP。

 

使用说明
1.登陆

        访问http://localhost:8080/km进入登陆界面，用户名admin,密码admin



2.首页

首页主要是显示监控信息，当一个任务（作业或转换）启动后，这个任务就处于被系统的监控状态下，首页展示了总监控任务数、监控作业数、监控转换数、转换监控记录（仅显示5条）、作业监控记录（仅显示5条）以及7天内作业和转换的监控状况。

 

3.资源库管理

管理kettle数据库资源库的信息，可以新增、修改、删除数据库资源库。



4.任务管理 – 作业管理

管理作业定时任务，可以新增、修改、删除作业定时任务，启动后作业即开始运行。



5.任务管理 – 转换管理

管理转换定时任务，可以新增、修改、删除转换定时任务，启动后转换即开始运行。



6.任务管理 – 执行策略

管理执行策略，可以新增、修改、删除执行策略（定时执行策略）。



7.监控管理 – 作业监控

处于运行的作业会被系统监控，此处显示被监控的作业的监控信息，包括总作业任务数、总执行成功次数、总执行失败次数以及每个作业的成功次数和失败次数。查看详情页面还可以查看每次执行的日志及执行时间，日志还可下载。



8.监控管理 – 转换监控

处于运行的转换会被系统监控，此处显示被监控的转换的监控信息，包括总转换任务数、总执行成功次数、总执行失败次数以及每个转换的成功次数和失败次数。查看详情页面还可以查看每次执行的日志及执行时间，日志还可下载。



9.用户管理

此菜单只有admin用户登陆时显示，用户管理用户，admin用户可以新增用户、编辑用户、删除用户。



最后希望大家可以一起维护此项目，如有问题可加入qq群提问

