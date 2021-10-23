<font color=#000000	size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

<font color=#FFA500 size=5 face=楷体>[Link](https://github.com/bwhyman/web-course/tree/master/web-experiments/experiment-03)</font>

@[TOC]

# <font color=#6495ED size=6 >WebExperiment-03</font>
* ### <font color=#000000 size=4 face=粗体>需求</font>
	

	> 为各部分区域设计统一样式；全局取消元素内外边距，按box尺寸计算 	 	
	> **需求0**
	> 	基于当前HTML代码，弹性栅格布局页面。header/footer占1行；main中sidebar最小宽度200px；article占剩余全部。
	> 行中空间不足元素不换行，由响应式布局决定小尺寸显式效果 	 	
	> **上导航+1**
	> 	于弹性布局，为header中的上导航添加背景色/悬浮字体颜色等样式，且将logout推至最右侧 	 	
	> **页脚+1**
	> 	元素居中，字体居中 	
	>
	> 	**左侧边栏+1**
	> 	为左侧边栏添加样式，添加背景色/字体色/悬浮等等样式 	 	运行显示结果
	> ![在这里插入图片描述](https://img-blog.csdnimg.cn/0c2db4e2da3a4e69ae4ecc7616983f6e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATkVGVSBBQi1JTg==,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)


* ### <font color=#000000 size=4 face=粗体>experiment3 解读</font>

  * 弹性容器：

    **父元素属性**

    ![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-AuCK6FWy-1634305164611)(C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211015204218388.png)\]](https://img-blog.csdnimg.cn/b7d19c746f404950aeb89d231a086432.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATkVGVSBBQi1JTg==,size_14,color_FFFFFF,t_70,g_se,x_16)	

    **子元素属性**

      ![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-tvGcQMJz-1634305164615)(C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211015204235728.png)\]](https://img-blog.csdnimg.cn/e571f8d83afe402cb9439475b4cef30d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATkVGVSBBQi1JTg==,size_20,color_FFFFFF,t_70,g_se,x_16)

    注意不要使用错对象了

    * 实现样式

      ![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-5rJh6Bwp-1634305164616)(C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211015204322328.png)\]](https://img-blog.csdnimg.cn/e0d4161c84c641c7a7d057ac27b01454.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATkVGVSBBQi1JTg==,size_20,color_FFFFFF,t_70,g_se,x_16)

    * 基本布局

      ![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-ERwB684n-1634305164619)(C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211015204723412.png)\]](https://img-blog.csdnimg.cn/0470a52fc75344f88d5bacbda372c8d3.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATkVGVSBBQi1JTg==,size_17,color_FFFFFF,t_70,g_se,x_16)

      ![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-wlj7mC4q-1634305164620)(C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211015204757872.png)\]](https://img-blog.csdnimg.cn/83323a559ff94f8baf094f114aa70f51.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATkVGVSBBQi1JTg==,size_20,color_FFFFFF,t_70,g_se,x_16)

  * 代码

    ```html
    <!--
     * @Author: NEFU AB-IN
     * @Date: 2021-10-14 11:02:35
     * @FilePath: \web-experiments\experiment-03\src\main\webapp\layout-css.html
     * @LastEditTime: 2021-10-15 20:35:45
    -->
    <!DOCTYPE html>
    <html lang="en">
        <head>
            <meta charset="UTF-8" />
            <meta http-equiv="X-UA-Compatible" content="IE=edge" />
            <meta
                name="viewport"
                content="width=device-width, initial-scale=1.0, user-scalable=no, maximum-scale=1.0,minimum-scale=1.0"
            />
            <style>
                * {
                    padding: 0;
                    margin: 0;
                    box-sizing: border-box;
                }
                .area {
                    padding: 10px;
                    border: 1px solid #f08c00;
                    background-color: #ffec99;
                    border-radius: 5px;
                }
                .row {
                    display: flex;
                    align-items: flex-start; /*交叉轴(纵轴)的对齐方式*/
                }
                .row .grow-1 {
                    flex-grow: 1;
                }
                .col-md-1 {
                    width: 8.33%;
                }
                .col-md-2 {
                    width: 16.67%;
                }
                .col-md-3 {
                    width: 25%;
                }
                .col-md-4 {
                    width: 33.33%;
                }
                .col-md-5 {
                    width: 41.67%;
                }
                .col-md-6 {
                    width: 50%;
                }
                .col-md-7 {
                    width: 58.33%;
                }
                .col-md-8 {
                    width: 66.67%;
                }
                .col-md-9 {
                    width: 75%;
                }
                .col-md-10 {
                    width: 83.33%;
                }
                .col-md-11 {
                    width: 91.67%;
                }
                .col-md-12 {
                    width: 100%;
                }
                /*-------------- header ------------ */
                /*即实现头部导航的功能*/
                .nav ul {
                    display: flex; /*将ul转为弹性布局，使内部li变为弹性项*/
                    padding: 0;
                    list-style-type: none; /*即取消小圆点*/
                    background: #333;
                }
                .nav a {
                    color: white;
                    display: inline-block; /*横着并使超链接撑满li*/
                    padding: 14px 16px; /*撑开li*/
                    text-decoration: none; /*即取消文字的装饰*/
                }
                .nav a:hover {
                    background: #111;
                }
                .nav .right { /*right是赋给li的*/
                    margin-left: auto; /*当弹性项声明左/右外边距auto时，则，项尽可能向另一侧浮动*/
                }
                /*-------------- sidebar ------------ */
                .sidebar {
                    min-width: 200px;
                }
                .bar {
                    background: #f1f1f1;
                }
                .bar h2 {
                    padding: 5px;
                    color: white;
                    background: #3185bf;
                }
                .bar ul {
                    padding: 0;
                    list-style-type: none;
                }
                .bar a {
                    text-decoration: none;
                    display: block; /*撑满单元，若是inline-block撑不满*/
                    padding: 14px 16px;
                    color: black;
                    transition: all 0.5s;
                }
                .bar a:hover {
                    background: #03a9f4;
                    color: white;
                    transform: scale(1.1); /*允许移动、旋转、缩放和倾斜元素，这里是放大*/
                }
                /*-------------- article ------------ */
                /*-------------- footer ------------ */
                .footer {
                    text-align: center;
                    margin: auto; /*弹性项总的margin为auto，说明在中间*/
                }
            </style>
            <title>Title</title>
        </head>
        <body>
            <div class="container col-md-12">
                <!-- header -->
                <div class="area row col-md-12">
                    <div class="nav col-md-12">
                        <ul>
                            <li><a href="#">Home</a></li>
                            <li><a href="#">News</a></li>
                            <li><a href="#">Contact</a></li>
                            <li><a href="#">About</a></li>
                            <li class="right"><a href="#">Logout</a></li>
                        </ul>
                    </div>
                </div>
                <!-- main  -->
                <div class="area row col-md-12">
                    <!-- sidebar -->
                    <div class="area sidebar col-md-2">
                        <div class="bar">
                            <h2>云技术管理</h2>
                            <ul>
                                <li><a href="#">云服务器</a></li>
                                <li><a href="#">云数据库</a></li>
                                <li><a href="#">负载均衡</a></li>
                            </ul>
                        </div>
                        <div class="bar">
                            <h2>安全管理</h2>
                            <ul>
                                <li><a href="#">云盾控制台</a></li>
                                <li><a href="#">DDoS高防IP</a></li>
                                <li><a href="#">Web应用防火墙</a></li>
                                <li><a href="#">CA证书服务</a></li>
                            </ul>
                        </div>
                    </div>
                    <!-- article -->
                    <div class="area grow-1">
                        <h1>设计内容</h1>
                        <p>
                            为各部分区域设计统一样式；全局取消元素内外边距，按box尺寸计算
                        </p>
                        <h3>需求0</h3>
                        <p>
                            基于当前HTML代码，弹性栅格布局页面。header/footer占1行；main中sidebar最小宽度200px；article占剩余全部。
                            行中空间不足元素不换行，由响应式布局决定小尺寸显式效果
                        </p>
                        <h3>上导航+1</h3>
                        <p>
                            基于弹性布局，为header中的上导航添加背景色/悬浮字体颜色等样式，且将logout推至最右侧
                        </p>
                        <h3>页脚+1</h3>
                        <p>元素居中，字体居中</p>
                        <h3>左侧边栏+1</h3>
                        <p>为左侧边栏添加样式，添加背景色/字体色/悬浮等等样式</p>
                    </div>
                </div>
                <!-- footer -->
                <div class="area row col-md-12">
                    <div class="footer col-md-12">
                        <p>
                            东北林业大学<br />
                            软件工程专业 2046&copy;
                        </p>
                    </div>
                </div>
            </div>
        </body>
    </html>
    
    ```

    

  * **首先**，明显感觉有了一点框架意识，就是很多类可以用固定的$css$装饰

    * 比如设置$header$

      * **第一层$div$，大区域框架**
        * $area$声明这是一块大区域（如$header$），设置区域的属性

        * $row$声明这是一个弹性父容器
        * $col-mb-x$，基于$flex$的栅格布局(模拟$bootstrap$的实现)，即规定这个大区域占总页面的$x$列

      * **第二层$div$，元素的框架**
        * 声明区域
          * $col-mb-x$，也就是说，如果有多个元素，那么规定它自己**占$x$列**
          * $grow-1$，或者不用规定占多少列，直接占满剩余空间
        * $?$，即元素自定义的属性
        * $area$，也可以画个$area$表示区域

      * 剩下的就是之前学的内容了

    * **另外**，就是**横列表和竖列表**的规范

      * 最外面**框架**，设置宽度

        ```css
        .sidebar {
            min-width: 200px;
        }
        ```

      * **列表**

        * 声明是弹性的，**并区分主轴的方向**，方便调试$li$
        * 取消内边距，这样$li$可以撑满$ul$
        * 取消外边距，这样竖着的标题可以无缝链接到列表
        * **取消列表的样式，即小圆点**

        ```css
        .bar ul {
            display: flex;
            flex-direction: column;
            padding: 0;
            margin: 0;
            list-style-type: none;
        }
        ```

    * **元素**，$list \ item$

      * **取消文字装饰**，比如是超链接，即取消下划线

        * $block$，撑满单元，换行，竖列表

        * $inline-block$，撑不满，不换行，横列表

          ```css
          .bar a {
              text-decoration: none;
              display: block; /*撑满单元，若是inline-block撑不满*/
              padding: 14px 16px;
              color: black;
              transition: all 0.5s;
          }
          ```