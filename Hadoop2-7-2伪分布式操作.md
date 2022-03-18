---
title: Hadoop2.7.2 伪分布式操作
tags:
  - Hadoop
  - 大数据
categories:
  - [Linux]
  - [Hadoop]
  - [Project]
toc: true
quicklink: true
math: true
sidebar: true
copyright: true
reward: true
photos:
  - https://oss.ab-in.cn/Pictures/hadoop.png
date: 2022-01-06 23:07:16
---

{% note info %}
**摘要**
操作详细步骤，以及遇到的问题
{% endnote %}
<!-- more -->


<font color=#000000	size=3 face=楷体>Powered by:**NEFU AB-IN**</font>


@[TOC](文章目录)	

# <font color=#6495ED size=6>Pseudo-Distributed Operation</font>

Hadoop 也可以在伪分布式模式下的单节点上运行

官方文档：[https://hadoop.apache.org/docs/r2.7.2/hadoop-project-dist/hadoop-common/SingleCluster.html](https://hadoop.apache.org/docs/r2.7.2/hadoop-project-dist/hadoop-common/SingleCluster.html)

官方文档介绍的很详细，我在此记一下重点

## **八个文件**

1. **core-site.xml**
2. **hadoop-env.sh**
3. **hdfs-site.xml**
4. **yarn-env.sh**
5. **yarn-site.xml**
6. **mapred-env.sh**
7. **mapred-site.xml**
8. **slaves**

## **五个端口**

[更详细的端口说明](https://blog.csdn.net/lzm1340458776/article/details/42394357?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522162367874916780255291893%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=162367874916780255291893&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~baidu_landing_v2~default-1-42394357.first_rank_v2_pc_rank_v29&utm_term=hadoop%20%2050010&spm=1018.2226.3001.4187)

  1. **50070** : hadoop-dfs.http.address，查看NameNode状态，该端口的定义位于core-default.xml中，**HDFS Web UI端口**（可访问）
  2. **50075** : 查看DataNode的,该地址和端口的定义位于hdfs-default.xml中. **帮助下载50070界面的文件**
     **除此之外还需要在本机的hosts文件中，配置自己主机名和公网ip的映射**
  3. **50010** : DataNode控制端口
  4. **8088** : hadoop-resourcemanager.webapp.address，**YARN Web UI端口**（可访问）
   5. **19888** : hadoop-jobhistory.webapp.address，**历史服务器Web UI端口**（可访问）
   6. **50090**：Hadoop **secondary NameNode服务与Web UI端口**（可访问）

![](https://img-blog.csdnimg.cn/20210616104606888.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1ODU5MTg4,size_16,color_FFFFFF,t_70)



* **core-site.xml**
  配置两个：

  * fs.defaultFS： 在local模式下是本地文件系统，需要更改成hdfs

  * hadoop.tmp.dir：配置到比较大的地方

  * 注意：

    * 第一是改**主机名**，改成自己的主机名
    * 第二是这两部分都要被`<configuration>`扩起来

    ```bash
      <configuration>
    	<property>
             <name>fs.defaultFS</name>
             <value>hdfs://AB-IN:9000</value>
    	</property>
     
    	<property>
        	<name>hadoop.tmp.dir</name>
    		<value>/opt/hadoop-2.7.2/data/tmp</value>
    	</property>
      
      </configuration> 
    ```

* **hadoop-env.sh**

  ```bash
  export JAVA_HOME=/opt/jdk1.8.0_144
  ```

* **hdfs-site.xml**

  ```bash
   <!-- 默认副本数量为3，即是完全分布式的要求
  -->
  <configuration>
      <property>
          <name>dfs.replication</name>
          <value>1</value>
      </property>
      <property>
          <name>dfs.http.address</name>
          <value>0.0.0.0:50070</value>
      </property>
  </configuration>
  ```

* **启动ssh**
	配置文件/etc/ssh/sshd_config
	```bash
	PasswordAuthentication yes
	PermitRootLogin yes
	```

  ```bash
  sudo apt-get install openssh-server
  service ssh start
  ```
  	
	

  相关的 : [ssh问题](https://blog.csdn.net/jszhangyili/article/details/8881807?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522162194471716780269860377%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=162194471716780269860377&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~baidu_landing_v2~default-2-8881807.first_rank_v2_pc_rank_v29&utm_term=localhost:%20ssh:%20connect%20to%20host%20localhost%20port%2022:%20Connection%20refused&spm=1018.2226.3001.4187)  [ssh免密问题](https://blog.csdn.net/kunlong0909/article/details/7284174?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522162194661716780274158741%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=162194661716780274158741&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-7284174.first_rank_v2_pc_rank_v29&utm_term=root@localhost%27s%20password:&spm=1018.2226.3001.4187)

  比较关键的一句话 ，root@ 后面是要加的主机名，密码是root密码

  ```bash
   ssh-copy-id -i ~/.ssh/id_rsa.pub root@localhost
  ```

* **格式化NameNode**
  一次就好，千万别每次上机都来一次，会导致namenode和datanode不匹配
  如果真的需要格式化的话，就把两个进程结束，再去删/opt/hadoop-2.7.2/data 和 /opt/hadoop-2.7.2/logs

  ```bash
   bin/hdfs namenode -format
  ```

  出现0了才算成功![在这里插入图片描述](https://img-blog.csdnimg.cn/20210525200728254.png)

* **启动NameNode，启动DataNode**

  ```bash
   sbin/hadoop-daemon.sh start namenode
   sbin/hadoop-daemon.sh start datanode
  ```

* **jps** 可以查看集群启动状况

* **创建（多级）路径**
  **可以不加bin/**，因PATH中配置了HADOOP_HOME

  ```bash
  bin/hdfs dfs -mkdir -p /user/root/input
  ```

* **（递归）列出**

  ```bash
  bin/hdfs dfs -ls /user/root/input
  bin/hdfs dfs -ls -R /
  ```

* **向HDFS上传文件**
  将之前在本机上做的$WordCount$案例的输入上传

  ```bash
  bin/hdfs dfs -put ./wcinput/wc.input /user/root
  ```

 * **基于HDFS运行官方给出的例子：Grep和WordCount程序**
   现在是伪分布，所以要写hdfs的地址。而且output文件夹不要提前建立

   ```bash
   bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.2.jar grep input output 'dfs[a-z.]+'
   bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.2.jar wordcount /user/root/input /user/root/output
   ```

* **输出HDFS上文件的内容**
  同时在output文件里_SUCCESS，看是否成功

  ```bash
  bin/hdfs dfs -cat /user/root/output/p*
  ```

* **yarn-env.sh**
  配置JAVA_HOME

  ```bash
   export JAVA_HOME=/opt/jdk1.8.0_144
  ```

* **yarn-site.xml**

  ```bash
  <configuration>
    <!-- Site specific YARN configuration properties -->
    <!--Reducer获取数据的方式-->
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
   
    <!--指定Yarn的ResourceManager的地址-->
    <property>
        <name>yarn.resourcemanager.hostname</name>
        <value>AB-IN</value>
    </property>
   </configuration>
  ```

* **mapred-env.sh**
  配置JAVA_HOME

  ```bash
   export JAVA_HOME=/opt/jdk1.8.0_144
  ```

* **mapred-site.xml**
  先将mapred-site.xml.template重命名为mapred-site.xml

  ```bash
    <configuration>
    <!--指定MR运行在Yarn上，默认数值是local，即本地资源调配，将其改成yarn-->
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
    </configuration>
  ```

* **启动ResourceManager，NodeManager**
  保证NameNode和DataNode已经启动

  ```bash
  sbin/yarn-daemon.sh start resourcemanager
  sbin/yarn-daemon.sh start nodemanager
  ```

  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210527172723215.png)

* **再执行wordcount job**

  ```bash
   hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.2.jar wordcount /user/root/input /user/root/output
  ```

  期间我遇到了这个问题
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210527184342189.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1ODU5MTg4,size_16,color_FFFFFF,t_70)
  挑重点说

  ```bash
  769.4 GB of 10 GB virtual memory used. Killing container.
  Container killed on request. Exit code is 143
  Container exited with a non-zero exit code 143
  ```

  看出来时虚拟内存太大了，杀死了container进程
  所以在etc/hadoop/yarn-site.xml 加入下面


  ```bash
      <!--增大默认大小-->
    <property>
        <name>yarn.nodemanager.vmem-pmem-ratio</name>
        <value>5</value>
    </property>
    <!--不对容器强制执行虚拟内存限制-->                   
    <property>
 	    <name>yarn.nodemanager.vmem-check-enabled</name>
 	    <value>false</value>
 	</property>
  </configuration>
 	```
 	并重启**yarn**！！！！
 	
 	结果
 	![在这里插入图片描述](https://img-blog.csdnimg.cn/20210527184833763.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1ODU5MTg4,size_16,color_FFFFFF,t_70)

* **配置历史服务器**

  * **mapred-site.xml**


  ```bash
    <!--历史服务器端地址，即工作端口-->
    <property>
        <name>mapreduce.jobhistory.address</name>
        <value>AB-IN:10020</value>
    </property>
   
    <!--历史服务器Web端地址，即访问端口-->
    <property>
        <name>mapreduce.jobhistory.webapp.address</name>
        <value>0.0.0.0:19888</value>
    </property>
    </configuration>                                                             
  ```

  * **启动历史服务器**


  ```bash
  sbin/mr-jobhistory-daemon.sh start historyserver
  ```

  * **查看JobHistory**


  ```bash
  http://主机名:19888
  http://IP:19888
  ```

* **配置日志的聚合**

  * 日志聚合的概念：应用运行完成以后，将程序运行的日志信息上传的HDFS。

    日志聚合的好处：可以方便地查看程序运行的详情，方便开发调试。

  * **yarn-site.xml**


  ```bash
  <!--日志聚合功能使能-->
  <property>
          <name>yarn.log-aggregation-enable</name>
          <value>true</value>
  </property>
  
  <!--日志保留时间设置为七天-->
  <property>
          <name>yarn.log-aggregation.retain-seconds</name>
          <value>604800</value>
  </property>
  <property>
          <name>yarn.resourcemanager.webapp.address</name>
          <value>0.0.0.0:8088</value>
  </property>
  ```

  * 重启NodeManager、ResourceManager、HistoryManager


  ```bash
  sbin/mr-jobhistory-daemon.sh stop historyserver
  sbin/yarn-daemon.sh stop nodemanager
  sbin/yarn-daemon.sh stop resourcemanager
  
  sbin/yarn-daemon.sh start resourcemanager
  sbin/yarn-daemon.sh start nodemanager
  sbin/mr-jobhistory-daemon.sh start historyserver
  ```

最后关于使用**服务器**而非**wsl**的，端口一定要在安全组中放开，如果配置了宝塔服务，在宝塔中端口需要进行二次放行，需要注意。

  

