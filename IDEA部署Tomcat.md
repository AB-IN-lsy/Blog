---
title: IDEA部署Tomcat
tags:
  - IDEA
  - Tomcat
  - Java
categories:
  - [Project]
  - [Java]
toc: true
quicklink: true
math: true
sidebar: true
copyright: true
reward: true
photos:
  - https://oss.ab-in.cn/Pictures/IDEA.png
date: 2022-01-06 22:28:42
---

{% note info %}
**摘要**
关于如何在IDEA中远程操控服务器部署并修改Tomcat项目
{% endnote %}
<!-- more -->


<font color=#000000	size=3 face=楷体>Powered by:**NEFU AB-IN**</font>


@[TOC](文章目录)

# <font color=#6495ED size=6>IDEA远程调控Tomcat</font>

>写在前面
>由于61711是Java远程debug所开设的窗口，由于开放此端口会导致较为严重的hack，hacker会以root身份通过此端口执行命令，导致系统极其不安全，因此我关闭了此端口。
>目前我还没找到关闭此端口，还能让idea顺利连接到云服务器项目的方法，因此以下操作**仅供参考**
>比较正规的方法，是安装宝塔的tomcat服务，并上传包至webapps下面，平常debug，用本地的tomcat调试即可
* ## 准备

  * 云服务器
  * $IDEA$
  * 本地$Tomcat$
  * 云服务器$Tomcat$


* ## 云服务器

  ![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-CEWL2k3S-1631426262023)(C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912130630844.png)\]](https://img-blog.csdnimg.cn/34aad2f0c23747a78d162d8b5c81a06c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATkVGVSBBQl9JTg==,size_20,color_FFFFFF,t_70,g_se,x_16)


* ## IDEA

  首先要有一个$web$项目

  ![!\[\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-e5uRLkNC-1631426262028)(C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912130658601.png)\]\](https://img-blog.csdnimg.cn/2d7b9b89f43d41839648064b3b5fb0d1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATkVGVSBBQl9JTg==,size_20,color_FFFFFF,t_70,g_se,x_16](https://img-blog.csdnimg.cn/f6b4244d58de46f3a4a27d141f4df335.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATkVGVSBBQi1JTg==,size_20,color_FFFFFF,t_70,g_se,x_16)



  $web$项目在$pom$中设置$war$包

  ![在这里插入图片描述](https://img-blog.csdnimg.cn/fd8476805f6240b5b7d0497e31f3d700.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATkVGVSBBQi1JTg==,size_20,color_FFFFFF,t_70,g_se,x_16)
	$pom.xml$源码
	

	```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<project xmlns="http://maven.apache.org/POM/4.0.0"
	         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	    <modelVersion>4.0.0</modelVersion>
	
	    <groupId>org.example</groupId>
	    <artifactId>web-examples</artifactId>
	    <version>1.0-SNAPSHOT</version>
	    <packaging>war</packaging>
	    <properties>
	        <maven.compiler.source>16</maven.compiler.source>
	        <maven.compiler.target>16</maven.compiler.target>
	    </properties>
	
	    <dependencies>
	        <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
	        <dependency>
	            <groupId>javax.servlet</groupId>
	            <artifactId>javax.servlet-api</artifactId>
	            <version>4.0.1</version>
	            <scope>provided</scope>
	        </dependency>
	        <!-- https://mvnrepository.com/artifact/org.apache.taglibs/taglibs-standard-spec -->
	        <dependency>
	            <groupId>org.apache.taglibs</groupId>
	            <artifactId>taglibs-standard-spec</artifactId>
	            <version>1.2.5</version>
	        </dependency>
	        <!-- https://mvnrepository.com/artifact/org.apache.taglibs/taglibs-standard-impl -->
	        <dependency>
	            <groupId>org.apache.taglibs</groupId>
	            <artifactId>taglibs-standard-impl</artifactId>
	            <version>1.2.5</version>
	        </dependency>
	        <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
	        <dependency>
	            <groupId>mysql</groupId>
	            <artifactId>mysql-connector-java</artifactId>
	            <version>8.0.21</version>
	        </dependency>
	        <!-- https://mvnrepository.com/artifact/javax.annotation/javax.annotation-api -->
	        <dependency>
	            <groupId>javax.annotation</groupId>
	            <artifactId>javax.annotation-api</artifactId>
	            <version>1.3.2</version>
	            <scope>provided</scope>
	        </dependency>
	    </dependencies>
	
	    <build>
	        <plugins>
	            <plugin>
	                <groupId>org.apache.maven.plugins</groupId>
	                <artifactId>maven-war-plugin</artifactId>
	                <version>3.3.1</version>
	            </plugin>
	        </plugins>
	    </build>
	</project>
	```


* ## 本地Tomcat

  [本地安装Java环境详细步骤](https://blog.csdn.net/weixin_43905618/article/details/104606815)

  [本地安装Tomcat详细步骤](https://blog.csdn.net/weixin_43905618/article/details/104625430?ops_request_misc=%7B%22request%5Fid%22%3A%22163136239516780265473656%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fall.%22%7D&request_id=163136239516780265473656&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v29_ecpm-1-104625430.pc_search_result_cache&utm_term=配置本地tomcat&spm=1018.2226.3001.4187)

* ## 云服务器Tomcat

  一样在官网下载，下载完用$winscp$或者$btpanel$传到$/opt$下即可

  ```bash
  cd /opt
  tar -zxvf apache-tomcat-10.0.10.tar.gz
  cd /etc/profile.d/
  vim tomcat.sh
    export CATALINA_BASE=/opt/apache-tomcat-10.0.10
    export CATALINA_HOME=$CATALINA_BASE
    export TOMCAT_HOME=$CATALINA_BASE
  source /etc/profile
  ```

  之后就是对**bin/catalina.sh**和**conf/server.xml**的配置

  下面设计**四个**端口

  * **conf/server.xml**中修改$SHUTDOWN$端口，我更改为$8006$

    ![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-OVqeq4Lt-1631426262038)(C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912133500918.png)\]](https://img-blog.csdnimg.cn/74680a32cc534ea8b414ab6e2a0247a1.png)


  * **conf/server.xml**中修改$STARTUP$端口，也就是访问的端口，我更改为$8007$

    ![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-OnlpqKvI-1631426262040)(C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912133602186.png)\]](https://img-blog.csdnimg.cn/40270b48cacd46678af3125a9462c663.png)


  * **bin/catalina.sh**中设置$JMX$端口，默认就是$1099$

  * **bin/catalina.sh**中设置$DEBUG$端口，我设置为$61711$，在脚本中找`JPDA_ADDRESS=”8000”`，将其修改为$61711$即可![在这里插入图片描述](https://img-blog.csdnimg.cn/9d3615f3fef5487ca749ad0e2323af83.png)


    这两个配置，需要在脚本中加入这段话

    ```bash
     JAVA_OPTS="${JAVA_OPTS}-Djava.security.egd=file:/dev/./urandom"
     export CATALINA_BASE=$CATALINA_BASE
     CATALINA_OPTS="${CATALINA_OPTS} -Djava.rmi.server.hostname=主机IP"
     CATALINA_OPTS="${CATALINA_OPTS} -Djavax.management.builder.initial=" #不写
     CATALINA_OPTS="${CATALINA_OPTS} -Dcom.sun.management.jmxremote=true"
     CATALINA_OPTS="${CATALINA_OPTS} -Dcom.sun.management.jmxremote.port=1099"
     CATALINA_OPTS="${CATALINA_OPTS} -Dcom.sun.management.jmxremote.ssl=false"
     CATALINA_OPTS="${CATALINA_OPTS} -Dcom.sun.management.jmxremote.authenticate=false"
     CATALINA_OPTS="${CATALINA_OPTS} -Dcom.sun.management.jmxremote.rmi.port=1099"
     CATALINA_OPTS="${CATALINA_OPTS} -server -Xdebug -Xnoagent -Djava.compiler=NONE -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=61711"
     export CATALINA_OPTS
     export JAVA_OPTS
    ```

  **一定要在控制台放行四个端口**！

  接下来试试启动是否成功

  ![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-FQQpCxK4-1631426262041)(C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912134455777.png)\]](https://img-blog.csdnimg.cn/7c8f552b271e45df8c064f1f27036d9c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATkVGVSBBQl9JTg==,size_20,color_FFFFFF,t_70,g_se,x_16)


  **由于我已经配置新网站了，所以出来的不是欢迎页**

  ![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-SAzzr8Vv-1631426262042)(C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912134526813.png)\]](https://img-blog.csdnimg.cn/060759c342b641a58f0bebc0be381e8d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATkVGVSBBQl9JTg==,size_20,color_FFFFFF,t_70,g_se,x_16)


  查看端口

  ```bash
  netstat -nlpt
  ```

  ![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-eoQEVnEs-1631426262043)(C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912134912302.png)\]](https://img-blog.csdnimg.cn/b66c5548cad24189add6596e4f22bf12.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATkVGVSBBQl9JTg==,size_20,color_FFFFFF,t_70,g_se,x_16)


  端口全部启动

  ![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-5YqiAAeb-1631426262043)(C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912134936956.png)\]](https://img-blog.csdnimg.cn/2489a7a1b9b94689a8159d3f922609f2.png)




* ## IDEA 编辑配置

  ![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-sMoLU7TZ-1631426262044)(C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912135553128.png)\]](https://img-blog.csdnimg.cn/ee530835a0ea418bae8a4ccf50881247.png)


  注意是$Tomcat \ server$ 不是$EE$，在这记录一下配置

  热交换器可以做到实时更新![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-gh6QKG72-1631426262045)(C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912135652821.png)\]](https://img-blog.csdnimg.cn/3bb1c4ebdad84d679ce7d2d49f1a9165.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATkVGVSBBQl9JTg==,size_20,color_FFFFFF,t_70,g_se,x_16)  
  应用程序服务器配置，也就是本机的$Tomcat$，而不是服务器的![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-mjXF2UjS-1631426262045)(C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912135729123.png)\]](https://img-blog.csdnimg.cn/9b26f58ccaaf441dae95952610a4ee99.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATkVGVSBBQl9JTg==,size_20,color_FFFFFF,t_70,g_se,x_16)
  
  服务器部署

  ![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-ZwLeBIZ7-1631426791152)(C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912140222691.png)\]](https://img-blog.csdnimg.cn/04e98fd453e8415191a1ef1495bd2757.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATkVGVSBBQl9JTg==,size_20,color_FFFFFF,t_70,g_se,x_16)


  ![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-XlfISmMC-1631426791157)(C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912140245509.png)\]](https://img-blog.csdnimg.cn/775e9d73ca0f4dd0b632ce17f0619395.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATkVGVSBBQl9JTg==,size_20,color_FFFFFF,t_70,g_se,x_16)


  退回来，部署的包

  ![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-946oKvaL-1631426791161)(C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912140410242.png)\]](https://img-blog.csdnimg.cn/1d544178ad664dcf85374c6cc643a59f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATkVGVSBBQl9JTg==,size_20,color_FFFFFF,t_70,g_se,x_16)


  这里的输出目录最后面我写的$web-examples$代表你项目传在$webapp$里的文件夹名![在这里插入图片描述](https://img-blog.csdnimg.cn/61a355a2daf24dfdb9e2cf7f6ba6ccd5.png)
![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-lLcjQ46a-1631426791162)(C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912140441835.png)\]](https://img-blog.csdnimg.cn/47b817827a3647428ccf26fbbff13deb.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATkVGVSBBQl9JTg==,size_20,color_FFFFFF,t_70,g_se,x_16)  设置编译
  ![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-iZloApxD-1631426791164)(C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912140512432.png)\]](https://img-blog.csdnimg.cn/6771a2f594a444d6afebd196bec82c4a.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATkVGVSBBQl9JTg==,size_20,color_FFFFFF,t_70,g_se,x_16)
  接下来启动即可，下面就算成功了（如果弹出的不是你的网站，可以考虑把$ROOT$文件夹先删了）
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/97a74703c7e847699ca1675298460151.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATkVGVSBBQl9JTg==,size_20,color_FFFFFF,t_70,g_se,x_16)
更新代码时$DEBUG$即可，或者构建项目，可能需要等一会
![在这里插入图片描述](https://img-blog.csdnimg.cn/ea48176baf52417fb9072d84c2c5c6a9.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATkVGVSBBQl9JTg==,size_20,color_FFFFFF,t_70,g_se,x_16)


期间遇到了不少问题，比如无法连接$1099$，是因为没有在$catalina.sh$中配置；或者$1099$超时，是没加`Dcom.sun.management.jmxremote.rmi.port=1099"`；$DEBUG$不行，是因为没有修改默认端口

网上的教程一定要综合的去看，实在不行去看官方文档是如何配置


