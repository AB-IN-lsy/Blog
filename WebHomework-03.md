
* <font color=#000000	size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

  <font color=#FFA500 size=5 face=楷体>[Link](https://github.com/bwhyman/web-course/tree/master/web-homeworks)</font>

  @[TOC]

  # <font color=#6495ED size=6 >WebHomework-03</font>

  * ### <font color=#000000 size=4 face=粗体>需求</font>

    > **布局**
    > badge整体容器，包含：badge-content容器，badge-wrapper容器
    > badge-content容器，内容为图标
    > badge-wrapper容器，内容为数字
    >
    > **样式**
    > 引入Google Material Icon
    > badge-content容器中图标，声明合适颜色尺寸等
    > badge-wrapper容器，定位在badge整体容器右上角；背景色/字体色/字体尺寸
    > badge-wrapper容器如何变成圆形？数字叠加在图标上，有一圈白色空间？
    > ![在这里插入图片描述](https://img-blog.csdnimg.cn/0caff630438e4c7fb4794c1d41b03522.png#pic_center)


  * ### <font color=#000000 size=4 face=粗体>homework3 解读</font>

    ```html
    <!--
     * @Author: NEFU AB-IN
     * @Date: 2021-10-23 15:32:28
     * @FilePath: \web-homeworks\src\main\webapp\homework-03\fixed1.html
     * @LastEditTime: 2021-10-23 16:02:22
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
            <link
                href="https://cdn.bootcss.com/material-design-icons/3.0.1/iconfont/material-icons.css"
                rel="stylesheet"
            />
            <style>
                * {
                    padding: 0;
                    margin: 0;
                    box-sizing: border-box;
                }
                .content {
                    display: inline-block;
                    position: relative;
                }
                .badge-content .icons {
                    position: relative;
                    display: inline-block;
                    color: deepskyblue;
                    font-size: 16em;
                }
                .badge-wrapper {
                    padding: 10px;
                    text-align: center;
                    font-size: 4em;
                    position: absolute;
                    top: 0;
                    right: 0;
                    background-color: red;
                    color: white;
                    border-radius: 50%;
                    border: 8px solid white;
                }
            </style>
            <title>Document</title>
        </head>
        <body>
            <div class="container">
                <div class="content">
                    <div class="badge-content">
                        <i class="material-icons icons">bug_report</i>
                    </div>
                    <div class="badge-wrapper">12</div>
                </div>
            </div>
        </body>
    </html>
    ```

    * **对于图标大小问题**
      * `<i class="material-icons icons">bug_report</i>`
      * 由于本身就是文字，因为用了$CSS$转变成立$icon$，所以控制大小的还是用$font-size$

