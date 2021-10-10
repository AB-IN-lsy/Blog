<font color=#000000 size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

@[TOC]

# <font color=#6495ED size=6 >Windows Vscode C++17 配置教程</font>

## <font color=#FFA500 size=5>起因</font>

在博主打比赛时，对$map$进行$auto$的遍历操作，会导致编译警告，即$Warnning$，显示只有$c++17$可以用这个特性，但是可以编译，只是会用错误波浪线和编译警告

由于一开始不知道$gcc$版本十分旧，一直用的是$codeblocks$里自带的$mingw64$，而且也不知道$mingw$常年不更新版本，就冒然在$coderunner$中修改了命令，将$c++11$改成了$c++17$

```powershell
cd "d:\Code\Vscode\ACM\CF\2021.10.9\" ; if ($?) { g++ -std=c++17 a.cpp -o a } ; if ($?) { .\a }
```

结果：（由于博主已经配置完了，就拿$codeblocks$做个错误演示）

![image-20211009193659059](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211009193659059.png)

貌似就是$fs\_pash.h$有问题，搜了很久也没个说法

于是博主开始研究怎么升级$gcc$版本，可以使用$c++17$

* 目前$gcc$版本：$8.1.0$

## <font color=#FFA500 size=5>下载MSYS2</font>

* ### <font color=#000000 size=4 face=粗体>介绍MSYS2</font>

  > 由于 MinGW 本身仅代表工具链，而在 Windows 下，由于Windows的terminal cmd窗口使用感受太差，以及配套的命令行工具不够齐全，因此，MinGW 开发者从曾经比较旧的 Cygwin 创建了一个分支，也用于提供类 Unix 环境。但与 Cygwin 的大而全不同，MSYS 是冲着小巧玲珑的目标去的，所以整套 MSYS 以及 MinGW，主要以基本的 Linux 工具为主，大小在 200M 左右，并且没有多少扩展能力。
  >
  > 由于 MinGW 万年不更新，MSYS 更是，Cygwin的许多新功能 MSYS 没有同步过来，于是 Alex 等人建立了新一代的 MSYS 项目。仍然是 fork 了 Cygwin（较新版），但有个更优秀的包管理器 pacman，有活跃的开发者跟用户组，有大量预编译的软件包（虽然肯定没有Cygwin多）

  **msys2是一款跨平台编译套件，它模拟linux编译环境，支持整合mingw32和mingw64，能很方便的在windows上对一些开源的linux工程进行编译运行。**

  

* ### <font color=#000000 size=4 face=粗体>官网下载</font>

  [MSYS2](https://www.msys2.org/)

  ![image-20211009192455303](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211009192455303.png)

  接着跟着教程走

  安装: [msys2-x86_64-20210725.exe](https://github.com/msys2/msys2-installer/releases/download/2021-07-25/msys2-x86_64-20210725.exe)

  我的安装路径为 `D:\msys64`

* ### <font color=#000000 size=4 face=粗体>安装mingw64</font>

  由于$msys2$是个工具链，我们还是要从这个编译套件中下载$mingw64$
  
  ![image-20211009194452711](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211009194452711.png)
  
* ### <font color=#000000 size=4 face=粗体>安装gcc gdb make</font>

  查找$gcc$，找到$win$版本的$gcc$

  ![image-20211009194731566](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211009194731566.png)

  根据自己的电脑的$OS$选择版本，这里我选择$mingw-w64-x86\_64-gcc$

  ![image-20211009194853029](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211009194853029.png)

  可以看到安装需要的命令

  ![image-20211009195103816](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211009195103816.png)

  如果安装完成，打开$msys2$，并进行更新（如果需要换源，可以百度自行搜索）

  ```bash
  pacman -Syu --disable-download-timeout
  ```

  ![image-20211009195201764](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211009195201764.png)

  

  之后去往安装的路径，可以看到$msys2.exe$

  ![image-20211009195339471](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211009195339471.png)

  

  打开并继续进行更新

  ```bash
  pacman -Syu --disable-download-timeout
  ```

  之后进行$gcc,gdb,make$的安装

  ```bash
  pacman -S mingw-w64-x86_64-gcc  --disable-download-timeout
  pacman -S mingw-w64-x86_64-make  --disable-download-timeout
  pacman -S mingw-w64-x86_64-gdb  --disable-download-timeout
  ```

  最后，再进行一次更新

  ```bash
  pacman -Syu --disable-download-timeout
  ```

* ### <font color=#000000 size=4 face=粗体>设置环境变量</font>

  之前大家应该都设置过，这里就不细说了

  直接将原有的路径替换为`D:\msys64\mingw64\bin`即可

## <font color=#FFA500 size=5>Vscode配置</font>

* ### <font color=#000000 size=4 face=粗体>Vscode插件</font>

  ![image-20211009195855186](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211009195855186.png)

插件首先要配置好，这里推荐$coderunner$，自定义命令

* ### <font color=#000000 size=4 face=粗体>配置cpp</font>

  ![image-20211009200115528](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211009200115528.png)

* ### <font color=#000000 size=4 face=粗体>配置 coderunner</font>

  打开$settings.json$，找到$coderunner$的配置选项处

  ![image-20211009200237900](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211009200237900.png)

  ```json
  "code-runner.executorMap": {
      "javascript": "node",
      "java": "cd $dir && javac $fileName && java $fileNameWithoutExt",
      "c": "cd $dir && gcc $fileName -o $fileNameWithoutExt && $dir$fileNameWithoutExt",
      "cpp": "cd $dir && g++ -std=c++17 $fileName -o $fileNameWithoutExt && $dir$fileNameWithoutExt",
      "objective-c": "cd $dir && gcc -framework Cocoa $fileName -o $fileNameWithoutExt && $dir$fileNameWithoutExt",
      "php": "php",
      "python": "cd $dir && python -u $fileName"
  }
  ```

* ### <font color=#000000 size=4 face=粗体>配置 C/C++ IntelliSense</font>

  为了不让波浪线的出现，要设置标准

  

  ![image-20211009200428645](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211009200428645.png)

* ### <font color=#000000 size=4 face=粗体>配置 Debug</font>

  修改本地文件夹下的$launch.json$和$task.json$

  ![image-20211009200930260](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211009200930260.png)

  ![image-20211009200938234](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211009200938234.png)

大功告成



## <font color=#FFA500 size=5>效果</font>

![image-20211009201510630](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211009201510630.png)

![image-20211009201537573](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211009201537573.png)
