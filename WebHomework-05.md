* * * <font color=#000000	size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

      <font color=#FFA500 size=5 face=楷体>[Link](https://github.com/bwhyman/web-course/tree/master/web-homeworks)</font>
    
      @[TOC]
    
      # <font color=#6495ED size=6 >WebHomework-05	</font>
    
      * ### <font color=#000000 size=4 face=粗体>需求</font>
    
        > 实现效果：当悬浮在浮动按钮，动态改变按钮图标，过渡旋转180度，渐入二级图标列表
        >
        > **布局/样式**
        > fab浮动容器，绝对定位到右上角，内容居中；包含：fab-btn按钮容器，fab-menu容器
        > fab-btn容器，包含浮动按钮图标，颜色/尺寸大小等等；按钮图标最好选用实心图标，空心最好加背景，否则浮动按钮可能不明显
        > fab-menu容器，隐藏。包含列表，每个列表项中超链接，内容为图标
        > 可声明特定的二级图标样式，例如删除为红色
        > 悬浮在fab容器时，按钮图标过渡旋转180度
        >
        > jquery 容器悬浮监听，进入，修改按钮图标，fab-menu渐入；移出，改回按钮图标，fab-menu渐出![在这里插入图片描述](https://img-blog.csdnimg.cn/895e52d5264e4e4ab19cd8b4bb889e61.gif#pic_center)
    
    
      * ### <font color=#000000 size=4 face=粗体>homework5 解读</font>
    
        ```html
        <!--
         * @Author: NEFU AB-IN
         * @Date: 2021-10-23 16:13:17
         * @FilePath: \web-homeworks\src\main\webapp\homework-04\flaot1.html
         * @LastEditTime: 2021-10-23 17:05:32
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
                    .container {
                        position: relative;
                    }
                    .fab {
                        position: absolute;
                        top: 20px;
                        right: 20px;
                        text-align: center;
                    }
                    .fab-btn i {
                        transition: all 0.5s;
                    }
                    .fab:hover .fab-btn i {
                        transform: rotate(0.5turn);
                    }
                    .fab-menu {
                        display: none;
                    }
                    .fab-menu ul {
                        list-style-type: none;
                    }
                    .fab-menu i {
                        text-decoration: none;
                    }
                    .icons {
                        position: relative;
                        font-size: 4em;
                        display: inline-block;
                        color: skyblue;
                    }
                </style>
                <title>float</title>
            </head>
            <body>
                <div class="container">
                    <div class="fab">
                        <div class="fab-btn">
                            <a href=""
                                ><i class="material-icons icons">account_circle</i></a
                            >
                        </div>
                        <div class="fab-menu">
                            <ul>
                                <li>
                                    <a href=""
                                        ><i class="material-icons icons">edit</i></a
                                    >
                                </li>
                                <li>
                                    <a href=""
                                        ><i class="material-icons icons"
                                            >color_lens</i
                                        ></a
                                    >
                                </li>
                                <li>
                                    <a href=""
                                        ><i
                                            class="material-icons icons"
                                            style="color: red"
                                            >delete_forever</i
                                        ></a
                                    >
                                </li>
                            </ul>
                        </div>
                    </div>
                </div>
                <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
                <script>
                    $(".fab").on({
                        mouseenter: function () {
                            $(this)
                                .children(".fab-btn")
                                .find("i")
                                .text("remove_circle");
                            $(this).children(".fab-menu").fadeIn();
                        },
                        mouseleave: function () {
                            $(this)
                                .children(".fab-btn")
                                .find("i")
                                .text("account_circle");
                            $(this).children(".fab-menu").fadeOut();
                        },
                    });
                    /*$(".fab").hover(
                        function () {
                            $(this)
                                .children(".fab-btn")
                                .find("i")
                                .text("remove_circle");
                            $(this).children(".fab-menu").fadeIn();
                        },
                        function () {
                            $(this)
                                .children(".fab-btn")
                                .find("i")
                                .text("account_circle");
                            $(this).children(".fab-menu").fadeOut();
                        }
                    );*/
                </script>
            </body>
        </html>
        
        ```
    
        * `script`加在`body`后面
    
        * 当一个元素需要监听多个事件时，可使用$on()$方法同时注册，这里就是当大容器被访问时
    
          * 定义鼠标**浮上**和**浮下**的操作
    
          * 也可以选择像注释里这样，直接在$hover$时定义两个函数
    
        * `transform: rotate()` 单位
          * 角度值$deg$
          * 弧度值$rad$
          * 梯度$gard$
          * 转/圈$turn$