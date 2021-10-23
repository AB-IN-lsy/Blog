<font color=#000000	size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

<font color=#FFA500 size=5 face=楷体>[Link](https://codeforces.com/contest/1592)</font>

@[TOC]

# <font color=#6495ED size=6 >WebHomework-02</font>

* ### <font color=#000000 size=4 face=粗体>homework2 解读</font>

  实现效果
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/51262e31e85d4aabb286953e01d61a0a.gif#pic_center)

  

  源码

  ```html
  <!--
   * @Author: NEFU AB-IN
   * @Date: 2021-10-18 15:23:11
   * @FilePath: \web-homeworks\src\main\webapp\homework-02\card3.html
   * @LastEditTime: 2021-10-18 16:57:15
  -->
  <!DOCTYPE html>
  <html lang="en">
      <head>
          <meta charset="UTF-8" />
          <title>Title</title>
          <style>
              * {
                  padding: 0;
                  margin: 0;
                  box-sizing: border-box;
              }
              /* 容器 */
              .container {
                  display: flex;
              }
              /* card容器 */
              .card {
                  width: 270px;
                  margin: 15px;
                  padding: 5px;
              }
              /* header */
              .card .header {
                  text-align: center;
                  position: relative;
                  transform: translateY(100px); /* 先沿着Y轴方向向下平移100px */
                  transition: transform 0.5s;
                  background: rgba(255, 255, 255, 1);/* 背景透明度为 1 */
                  z-index: 99;
              }
              /* 圆角加在图片上，因为header容器需要把背景盖住问题内容 */
              .card .header img {
                  border-radius: 50%;
                  opacity: 0.5;
                  transition: opacity 0.5s;
                  width: 100%;
              }
  
              .card .content {
                  text-align: justify;
                  transform: translateY(-100px); /* 同样的文字往上移，这样header就与文字重叠 */
                  opacity: 0.5;
                  transition: transform 0.5s, opacity 0.5s;
              }
              .card .content p {
                  height: 200px;
                  overflow: hidden;
              }
  
              .card .content a {
                  display: inline-block; /* 必须要有这个属性 ，这样才能调用width，height，
                  padding-top, padding-bottom, margin-top, margin-bottom，等属性*/
                  padding: 10px 15px;
                  background: #f44336;
                  color: white;
                  text-decoration: none;
                  border-radius: 5px;
              }
  
              .card:hover .header {
                  transform: translateY(0px); /* 回归到原始位置 */
              }
              .card:hover .header > img {
                  opacity: 1;
              }
              .card:hover .content {
                  transform: translateY(0px);
                  opacity: 1;
              }
              .card .content a:hover {
                  background: red;
              }
          </style>
      </head>
      <body>
          <div class="container">
              <div class="card">
                  <div class="header">
                      <img src="https://picsum.photos/250?random=1" alt="face" />
                  </div>
                  <div class="content">
                      <p>
                          Lorem ipsum dolor sit amet, consectetur adipisicing
                          elit. Asperiores eos fugit harum hic maxime neque,
                          obcaecati possimus rerum soluta tenetur. Accusamus amet
                          ea mollitia nisi quis tenetur! Accusantium facilis quis
                          quod voluptates.
                      </p>
                      <a href="">Read More</a>
                  </div>
              </div>
  
              <div class="card">
                  <div class="header">
                      <img src="https://picsum.photos/250?random=2" alt="face" />
                  </div>
                  <div class="content">
                      <p>
                          Lorem ipsum dolor sit amet, consectetur adipisicing
                          elit. Asperiores eos fugit harum hic maxime neque,
                          obcaecati possimus rerum soluta tenetur. Accusamus amet
                          ea mollitia nisi quis tenetur! Accusantium facilis quis
                          quod voluptates.
                      </p>
                      <a href="">Read More</a>
                  </div>
              </div>
              <div class="card">
                  <div class="header">
                      <img src="https://picsum.photos/250?random=3" alt="face" />
                  </div>
                  <div class="content">
                      <p>
                          Lorem ipsum dolor sit amet, consectetur adipisicing
                          elit. Asperiores eos fugit harum hic maxime neque,
                          obcaecati possimus rerum soluta tenetur. Accusamus amet
                          ea mollitia nisi quis tenetur! Accusantium facilis quis
                          quod voluptates.
                      </p>
                      <a href="">Read More</a>
                  </div>
              </div>
          </div>
      </body>
  </html>
  ```

  * 不用$card$容器设置$flex$，因为如果设置了，它的子元素们就会成为弹性容器，就会成为水平排列，为了达到效果还需要变成纵向排列，相对比较麻烦。所以只需要对总容器进行$flex$即可，这样三个$card$就自动弹性横向布局
  * **内部元素居中** ： `text-align: center`，即元素居中(不单单是文字)
  * **两端对齐**：`justify`

  * **尺寸按容器缩放**：`width: 100%`，即随父元素比例变化
  * **圆形** ：`border-radius: 50%`
  * 关于**按钮**的设置：
    * `display: inline-block` **必须**要有这个属性 ，这样才能调用$width，height，padding-top, padding-bottom, margin-top, margin-bottom$等属性
    * `text-decoration: none` 取消文字样式
    * `padding: 10px 15px`
    * **悬浮时**
      * 更改字的颜色，或者背景颜色
      * `font-weight: bold;` 加粗
      * 如果不是链接，可以设定`cursor: pointer`，即更改鼠标悬浮时的样式
  * **对于折叠问题的实现**
    * 首先就是先让$content$与$header$重叠，就是让文字上升，图片下降，运用到`transform`，这个属性就是为了让对象发生变化
    * `transform`，所有转换，均为先绘制元素及其他正常元素，再计算并转换元素。因此，其仍占用原位置，且，不影响其他元素的位置。即**转换后，为绝对定位**
    * 然后就是覆盖问题，可以用`z-index`，解决
    * `z-index` 属性设定了一个**定位元素**及其后代元素或 **`flex` 项目**的 `z-order`。 当元素之间重叠的时候， `z-index` 较大的元素会覆盖较小的元素在上层进行显示
    * 所以就可以将$header$的`z-index`调大，并且**定位**（$relative$）！这样`z-index`才起作用
    * 遮盖的问题解决之后，就是$hover$时，回到原位置，以及不透明了