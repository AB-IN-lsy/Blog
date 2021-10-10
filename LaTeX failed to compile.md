<font color=#000000	size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

<font color=#FFA500 size=5 face=楷体>[Link](https://codeforces.com/contest/1579)</font>

@[TOC]

# <font color=#6495ED size=6 >LaTeX failed to compile</font>

* ## 问题

  ![image-20211005000236249](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211005000236249.png)

* ## 方案

  * https://yihui.org/tinytex/r/#debugging

    打开官方解决文档，一般都是没有安装$tinytex$导致的，所以

    ```R
    tinytex::install_tinytex()
    ```

  * 查看错误信息中含有乱码，打开$.tex$文件

    ![image-20211005000658931](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211005000658931.png)

    发现含有汉字，所以是某个$R$块运行时会输出汉字，然后编码不匹配会有乱码，更改即可

    我这里是$require$改成了$library$

  * 查看$.log$文件分析错误原因

  