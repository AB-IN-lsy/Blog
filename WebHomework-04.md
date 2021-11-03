<font color=#000000	size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

<font color=#FFA500 size=5 face=楷体>[Link](https://codeforces.com/contest/1592)</font>

@[TOC]

# <font color=#6495ED size=6 >WebHomework-04</font>

* ### <font color=#000000 size=4 face=粗体>需求1</font>

  >
  >**布局**
  >声明弹性容器
  >声明左导航，右侧主区域
  >导航项中，声明可点击的超链接，超链接内容为分别声明的图标与span文本容器
  >右主区域
  >
  >**样式**
  >引入Google Material Icon
  >全局box计算模式
  >弹性容器
  >左导航容器，声明最小宽度180px
  >项的超链接撑开，确定颜色等基本属性
  >超链接内的图标/文本，通过vertical-align居中对齐
  >悬浮时改变背景色
  >当小于600px时，更改导航区最小宽度，将超链接中的文本取消显式，即仅显示图标
  >![在这里插入图片描述](https://img-blog.csdnimg.cn/5f68419c48974144a8f1e67e7717f37a.gif#pic_center)


* ### <font color=#000000 size=4 face=粗体>需求2</font>
	> ![在这里插入图片描述](https://img-blog.csdnimg.cn/d736f210801b4edb8386533672aff6a3.gif#pic_center)

* ### <font color=#000000 size=4 face=粗体>homework4 需求1解读</font>
	

	```html
	<!--
	 * @Author: NEFU AB-IN
	 * @Date: 2021-10-30 20:26:37
	 * @FilePath: \web-homeworks\src\main\webapp\homework-04\nav.html
	 * @LastEditTime: 2021-11-02 00:06:52
	-->
	<!DOCTYPE html>
	<html lang="en">
	    <head>
	        <meta charset="UTF-8" />
	        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
	        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
	        <link
	            href="https://cdn.bootcss.com/material-design-icons/3.0.1/iconfont/material-icons.css"
	            rel="stylesheet"
	            crossorigin="anonymous"
	        />
	        <!-- <link
	            href="https://cdn.bootcss.com/material-design-icons/3.0.1/iconfont/material-icons.css"
	            rel="stylesheet"
	            crossorigin="anonymous"
	        /> -->
	        <style>
	            * {
	                padding: 0;
	                margin: 0;
	                box-sizing: border-box;
	            }
	            nav {
	                position: absolute;
	                background: #f1f1f1;
	                min-width: 180px;
	            }
	            nav ul {
	                list-style-type: none;
	            }
	            nav i {
	                padding: 10px;
	                vertical-align: middle;
	            }
	            nav a {
	                display: block;
	                color: black;
	                text-decoration: none;
	            }
	            nav span {
	                font-size: 1.1em;
	                margin: 5px;
	            }
	            .icon {
	                font-size: 2em;
	                color: rgb(102, 3, 116);
	                text-align: center;
	            }
	            .main {
	                margin-left: 200px;
	            }
	            @media (max-width: 600px) {
	                nav {
	                    min-width: 70px;
	                }
	                nav span {
	                    display: none;
	                }
	            }
	        </style>
	        <title>nav</title>
	    </head>
	    <body>
	        <div class="container">
	            <nav>
	                <ul>
	                    <li>
	                        <a href=""
	                            ><i class="material-icons icon">dashboard</i
	                            ><span>Dashboard</span></a
	                        >
	                    </li>
	                    <li>
	                        <a href=""
	                            ><i class="material-icons icon">desktop_mac</i
	                            ><span>Computer</span></a
	                        >
	                    </li>
	                    <li>
	                        <a href=""
	                            ><i class="material-icons icon">timelapse</i
	                            ><span>Timelapse</span></a
	                        >
	                    </li>
	                    <li>
	                        <a href=""
	                            ><i class="material-icons icon">style</i
	                            ><span>Style</span></a
	                        >
	                    </li>
	                    <li>
	                        <a href=""
	                            ><i class="material-icons icon">dialpad</i
	                            ><span>Dialpad</span></a
	                        >
	                    </li>
	                </ul>
	            </nav>
	            <div class="main">
	                <h1>矿泉水竟然不能这么喝！理由竟然是...</h1>
	                <br />
	                <p>
	                    矿泉水不能大口喝是怎么回事呢？矿泉水相信大家都很熟悉，但是矿泉水不能大口喝是怎么回事呢，下面就让小编带大家一起了解吧。
	                </p>
	                <br />
	                <p>
	                    矿泉水不能大口喝，其实就是大口喝容易噎死，大家可能会很惊讶矿泉水怎么会不能大口喝呢？但事实就是这样，小编也感到非常惊讶。
	                </p>
	                <br />
	                <p>
	                    这就是关于矿泉水不能大口喝的事情了，大家有什么想法呢，欢迎在评论区告诉小编一起讨论哦！
	                </p>
	            </div>
	        </div>
	    </body>
	</html>
	
	```
	* **响应式布局：media**
		![在这里插入图片描述](https://img-blog.csdnimg.cn/c3f5610b27fd4261aacab786b7c758a2.png)
		才执行下面的操作
   *  **注意** 
   		* media一般定义在css样式的最后
   		* media里定义的css样式名称，最好与前面定义的名称**相同**，不然很可能出莫名的bug

 * ### <font color=#000000 size=4 face=粗体>homework4 需求2解读</font>

```html
<!--
 * @Author: NEFU AB-IN
 * @Date: 2021-10-30 20:26:37
 * @FilePath: \web-homeworks\src\main\webapp\homework-04\nav1.html
 * @LastEditTime: 2021-10-30 23:43:33
-->
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <link
            href="https://cdn.bootcss.com/material-design-icons/3.0.1/iconfont/material-icons.css"
            rel="stylesheet"
            crossorigin="anonymous"
        />
        <!-- <link
            href="https://cdn.bootcss.com/material-design-icons/3.0.1/iconfont/material-icons.css"
            rel="stylesheet"
            crossorigin="anonymous"
        /> -->
        <style>
            * {
                padding: 0;
                margin: 0;
                box-sizing: border-box;
            }
            nav {
                position: absolute;
                background: #f1f1f1;
                width: 70px;
                transition: all 0.2s;
            }
            nav ul {
                list-style-type: none;
            }
            nav i {
                padding: 10px;
                vertical-align: middle;
            }
            nav a {
                display: block;
                color: black;
                text-decoration: none;
            }
            nav span {
                display: none;
                font-size: 1.1em;
                margin: 5px;
            }
            .icon {
                font-size: 2em;
                color: rgb(102, 3, 116);
                text-align: center;
            }
            nav:hover {
                width: 200px;
            }
            nav:hover ul {
                width: 200px;
            }
            nav li a:hover {
                background-color: rgb(157, 224, 255);
            }
            nav li a:hover i {
                color: orchid;
            }
            nav li a:hover span {
                font-weight: bold;
            }
            nav:hover span {
                display: inline-block;
            }
            .main {
                margin-left: 100px;
            }
        </style>
        <title>nav</title>
    </head>
    <body>
        <div class="container">
            <nav>
                <ul>
                    <li>
                        <a href=""
                            ><i class="material-icons icon">dashboard</i
                            ><span>Dashboard</span></a
                        >
                    </li>
                    <li>
                        <a href=""
                            ><i class="material-icons icon">desktop_mac</i
                            ><span>Computer</span></a
                        >
                    </li>
                    <li>
                        <a href=""
                            ><i class="material-icons icon">timelapse</i
                            ><span>Timelapse</span></a
                        >
                    </li>
                    <li>
                        <a href=""
                            ><i class="material-icons icon">style</i
                            ><span>Style</span></a
                        >
                    </li>
                    <li>
                        <a href=""
                            ><i class="material-icons icon">dialpad</i
                            ><span>Dialpad</span></a
                        >
                    </li>
                </ul>
            </nav>
            <div class="main">
                <h1>矿泉水竟然不能这么喝！理由竟然是...</h1>
                <br />
                <p>
                    矿泉水不能大口喝是怎么回事呢？矿泉水相信大家都很熟悉，但是矿泉水不能大口喝是怎么回事呢，下面就让小编带大家一起了解吧。
                </p>
                <br />
                <p>
                    矿泉水不能大口喝，其实就是大口喝容易噎死，大家可能会很惊讶矿泉水怎么会不能大口喝呢？但事实就是这样，小编也感到非常惊讶。
                </p>
                <br />
                <p>
                    这就是关于矿泉水不能大口喝的事情了，大家有什么想法呢，欢迎在评论区告诉小编一起讨论哦！
                </p>
            </div>
        </div>
    </body>
</html>
```