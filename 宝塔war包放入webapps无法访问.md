<font color=#000000	size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

@[TOC]

# <font color=#6495ED size=6 >宝塔war包放入webapps无法访问</font>

* ### <font color=#000000 size=4 face=粗体>问题</font>

  ![image-20211116111205351](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211116111205351.png)

  将$war$包放入$webapps$后，访问`ip:8080/project` 出现`404 Not found`

  而访问`ip:8080`正常

* ### <font color=#000000 size=4 face=粗体>解决方法</font>

  由于问题不普遍，最通俗的做法就是查看log文件

  ![image-20211116111402195](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211116111402195.png)

  查看log文件对应具体问题，由于启动的问题截图找不到了，我在此简单说一下遇到的问题的基本含义

  * **JDBC未注册**
  * **JAR包不全**

  所以我开始想**三**种可能性

  * **服务器的tomcat配置出错**
  * **war包缺少配置（war包出错）**
  * **war包和tomcat对应关系出错**

  ****

  开始验证猜想

  **第一种**猜想：**服务器的tomcat配置出错**

  * 将war包项目放入本地的tomcat服务器（服务器和本地的tomcat版本均为9）

  * 部署

    ![image-20211116112220316](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211116112220316.png)

  * 运行startup.bat

    ![image-20211116112310778](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211116112310778.png)

    发现项目正常部署，不像服务器的会报错

  * 查看页面

    ![image-20211116112443146](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211116112443146.png)

    没用问题，说明不是war包的错

  ****

  **第二种**猜想：**war包缺少配置（war包出错）**

  * 那么将包放到老师的平台上部署

    ![image-20211116121145838](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211116121145838.png)

    没有问题，说明也不是

  **第三种**猜想：**war包和tomcat对应关系出错**

  * 不知道怎么验证这个错误，但是直觉告诉我，tomcat版本都对应了，是不是该检查Java版本了？

  * 这让我想起可能**项目的java版本可能于服务器的Java版本不对应，导致兼容出错，jar包版本不对应**

  * 经过下面的**具体操作**，证明猜想正确

    

* ### <font color=#000000 size=4 face=粗体>具体操作</font>

  查看**项目的Java环境**

  ![image-20211116111754548](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211116111754548.png)

  显然是**11**版本

  ****

  查看**服务器的Java版本**

  ![image-20211116111934466](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211116111934466.png)

  发现是**8**版本

  ****

  **显然版本不一样！！！**

  那么这些可能就说的通了

  **开始调试**

  ****

  #### <font color=#000000 size=3 face=粗体>服务器端</font>

  * 更换服务器的Java版本

    ![image-20211116122034223](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211116122034223.png)

  * 启动tomcat，并观察是否对应

    ![image-20211116122215639](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211116122215639.png)

  

  #### 	<font color=#000000 size=3 face=粗体>war包</font>

  * 确定war包版本，并更改pom.xml的版本号

    ![image-20211116122407259](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211116122407259.png)

  * 执行`mvn clean package`

  #### 	<font color=#000000 size=3 face=粗体>部署</font>

  * 将包放入webapps下并解压

  * 实现nginx反向代理到域名

  * 查看效果

    ![image-20211116122622094](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211116122622094.png)

    **成功！！！**



* ### <font color=#000000 size=4 face=粗体>总结</font>

  * 部署web项目时要注意，war包Java版本是否与服务器端兼容
  * 学会查考log文件解决问题

  

