<font color=#000000	size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

<font color=#FFA500 size=5 face=楷体>[Link](https://docs.docker.com/)</font>

@[TOC]

# <font color=#6495ED size=6 >Docker&Django记录</font>

* ### <font color=#000000 size=4 face=粗体>将当前用户添加到docker用户组</font>

  > 为了避免每次使用docker命令都需要加上sudo权限，可以将当前用户加入安装中自动创建的docker用户组
  >
  > sudo usermod -aG docker $USER

* ### <font color=#000000 size=4 face=粗体>docker命令</font>

  > 镜像
  >
  > docker images：列出本地所有镜像

  > 容器
  >
  > docker [container] create -it ubuntu:20.04：利用镜像ubuntu:20.04创建一个容器。
  >
  > Ctrl-p，再按Ctrl-q可以挂起容器
  >
  > docker [contaienr] run -itd ubuntu:20.04 创建并启动一个容器
  >
  > docker ps -a：查看本地的所有容器

* ### <font color=#000000 size=4 face=粗体>ssh</font>

  服务器用户：$root,acs$

  docker用户：$root,acs\_in\_docker$

  此时从别的终端登录

  ```bash
  ssh ab-in
  ssh acs
  ssh root@xx.xx.xxx.xxx -p 20000 或者 ssh ab-in -p 20000
  (因为此时的root与docker里root重名)
  ssh acs_in_docker@xx.xx.xxx.xxx -p 20000
  (这时候就不能 ssh acs -p -20000了，因为dockers里没有acs这个用户
  证明这里的用户名是dockers里的，当然也可以配置免密登录 ssh acs_in_docker)
  ```
  
  

