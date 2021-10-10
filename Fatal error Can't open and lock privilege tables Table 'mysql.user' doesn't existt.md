Powered by:**NEFU AB-IN**

# <font color=#6495ED size=6>Fatal error: Can't open and lock privilege tables: Table 'mysql.user' doesn't existt</font>


* ## 查看LOG

  ```bash
  cat /var/log/mysql/error.log
  ```
  
  ![image-20210922160558566](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20210922160558566.png)
  
* ## 解决

  ```bash
  sudo mysql_install_db --user=mysql --ldata=/var/lib/mysql
  sudo service mysqld restart
  ```

  还有就是不要轻易改$my.cnf$

