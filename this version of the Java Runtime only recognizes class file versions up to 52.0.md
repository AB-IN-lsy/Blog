<font color=#000000	size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

<font color=#FFA500 size=5 face=楷体>[Link](https://codeforces.com/contest/1592)</font>

@[TOC]

# <font color=#6495ED size=6 >this version of the Java Runtime only recognizes class file versions up to 52.0</font>

* ### <font color=#000000 size=4 face=粗体>问题</font>

  将包放在宝塔的tomcat下的webapp下，无法访问

* ### <font color=#000000 size=4 face=粗体>解决过程</font>

  * 查看日志

    得知

    >java.lang.UnsupportedClassVersionError: com/util/DataSourceUtils has been compiled by a more recent version of the Java Runtime
    >
    >this version of the Java Runtime only recognizes class file versions up to 52.0

  * 通过表可以得知

    ```java
    49 = Java 5
    50 = Java 6
    51 = Java 7
    52 = Java 8
    53 = Java 9
    54 = Java 10
    55 = Java 11
    56 = Java 12
    57 = Java 13
    58 = Java 14
    ```

    当前的JDK版本为8

  * 但通过Java_home得知，Java的版本并不是8，而是14

    ![image-20211217190238992](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211217190238992.png)

  * 全局搜索带有java的文件

    ```bash
    find / -name java
    ```

    发现$/usr/java$，下面有两个jdk，一个Jdk7，一个Jdk8

    推测：**宝塔面板装的tomcat，使用的是/usr下的jdk，不管全局的java_home设置的什么**

  * 删除两个jdk，发现tomcat无法启动，报错

    ```bash
    Cannot locate Java Home
    ```

    证实了我们的猜想

* ### <font color=#000000 size=4 face=粗体>解决方案</font>

  * **第一种**

    可以在/usr/java中装上jdk14，并且卸载掉版本过低的jdk

  * **第二种**

    不用宝塔安装的tomcat，去官网下载，自行解压至服务器，并将包传至此tomcat的webapp下

  * 我采用的第二种，每次开启只需

    ![image-20211217190954731](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211217190954731.png)

    即可




