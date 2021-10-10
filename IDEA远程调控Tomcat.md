Powered by:**NEFU AB_IN**

@[TOC](文章目录)

# <font color=#6495ED size=6>IDEA远程调控Tomcat</font>


* ## 准备

  * 云服务器
  * $IDEA$
  * 本地$Tomcat$
  * 云服务器$Tomcat$


* ## 云服务器
	
	![image-20210912130630844](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912130630844.png)
	
* ## IDEA

  首先要有一个$web$项目

  ![image-20210912130658601](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912130658601.png)

  $web$项目在$pom$中设置$war$包

  ![image-20210912130800618](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912130800618.png)

* ## 本地$Tomcat$

  [本地安装Java环境详细步骤](https://blog.csdn.net/weixin_43905618/article/details/104606815)

  [本地安装Tomcat详细步骤](https://blog.csdn.net/weixin_43905618/article/details/104625430?ops_request_misc=%7B%22request%5Fid%22%3A%22163136239516780265473656%22%2C%22scm%22%3A%2220140713.130102334.pc%5Fall.%22%7D&request_id=163136239516780265473656&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v29_ecpm-1-104625430.pc_search_result_cache&utm_term=配置本地tomcat&spm=1018.2226.3001.4187)

* ## 云服务器$Tomcat$

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

    ![image-20210912133500918](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912133500918.png)

  * **conf/server.xml**中修改$STARTUP$端口，也就是访问的端口，我更改为$8007$

    ![image-20210912133602186](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912133602186.png)

  * **bin/catalina.sh**中设置$JMX$端口，默认就是$1099$

  * **bin/catalina.sh**中设置$DEBUG$端口，我设置为$61711$

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

  ![image-20210912134455777](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912134455777.png)

  **由于我已经配置新网站了，所以出来的不是欢迎页**

  ![image-20210912134526813](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912134526813.png)

  查看端口

  ```bash
  netstat -nlpt
  ```

  ![](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912134912302.png)

  端口全部启动

  ![image-20210912134936956](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912134936956.png)



* ## IDEA 编辑配置

  ![image-20210912135553128](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912135553128.png)

  注意是$Tomcat \ server$ 不是$EE$，在这记录一下配置

  热交换可以做到实时更新

  ![image-20210912135652821](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912135652821.png)

  应用程序服务器配置，也就是本机的$Tomecat$，而不是服务器的

  ![image-20210912135729123](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912135729123.png)

  服务器部署

  ![image-20210912140222691](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912140222691.png)

  ![image-20210912140245509](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912140245509.png)

  退回来，部署的包

  ![image-20210912140410242](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912140410242.png)

  ![image-20210912140441835](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912140441835.png)

  设置编译

  ![image-20210912140512432](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210912140512432.png)

