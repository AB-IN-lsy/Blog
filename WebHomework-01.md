<font color=#000000	size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

<font color=#FFA500 size=5 face=楷体>[Link](https://github.com/bwhyman/web-course/tree/master/web-homeworks)</font>

@[TOC]

# <font color=#6495ED size=6 >WebHomework-01</font>
* ### <font color=#000000 size=4 face=粗体>需求1</font>


	>![在这里插入图片描述](https://img-blog.csdnimg.cn/c94245eee95648b8bd3552941a71ec8f.png#pic_center)
	
	> 所有文本，密码的input元素，全局横向占用最大，圆角，内边距；
	> 声明form-group类，放置说明与输入域的行容器，横向弹性布局，元素居中对齐；
	> 声明form-label类，放置说明，文本右对齐，占20%，注意弹性百分比的占用属性；
	> 声明form-input类，放置输入域，互交的输入域可能是多个，因此是容器，占80%； 元素间，通过内边距推开

* ### <font color=#000000 size=4 face=粗体>需求2</font>

 	![result](https://img-blog.csdnimg.cn/img_convert/ebba3664567e1d79c826dcc7dec1e114.gif)

* ### <font color=#000000 size=4 face=粗体>homework1 需求1解读</font>

  ```html
  <!--
   * @Author: NEFU AB-IN
   * @Date: 2021-10-12 16:37:55
   * @FilePath: \web-homeworks\src\main\webapp\homework-01\form1.html
   * @LastEditTime: 2021-10-12 21:15:00
  -->
  <!DOCTYPE html>
  <html lang="en">
      <head>
          <meta charset="UTF-8" />
          <title>form1</title>
          <style>
              * {
                  padding: 0;
                  margin: 0;
                  box-sizing: border-box;
              }
  
              input[type="text"],
              input[type="password"] {
                  border: 1px solid #666;
                  border-radius: 5px;
                  width: 100%;
                  padding: 5px;
                  box-shadow: inset 1px 1px 1px #ccc; /*设置内阴影*/
                  outline: none
              }
  
              /* 容器 */
              .main {
                  border: 1px solid red;
                  width: 800px;
                  padding: 15px;
              }
  
              .form-group,
              .form-input,
              .form-label {
                  padding: 5px;
              }
  
              .form-group {
                  display: flex;
                  justify-content: center; /*交叉轴对齐*/
                  align-items: center; /*主轴对齐*/
              }
  
              .form-label {
                  text-align: end; /*文字右对齐*/
                  flex-basis: 20%;
              }
  
              .form-input {
                  flex-basis: 80%;
              }
          </style>
      </head>
      <body>
          <div class="main">
              <form>
                  <div class="form-group">
                      <label class="form-label">用户名</label>
                      <div class="form-input">
                          <input type="text" />
                      </div>
                  </div>
  
                  <div class="form-group">
                      <label class="form-label">密码</label>
                      <div class="form-input">
                          <input type="password" />
                      </div>
                  </div>
  
                  <div class="form-group">
                      <label class="form-label">性别</label>
                      <div class="form-input">
                          <input type="radio" name="sex" />男
                          <input type="radio" name="sex" />女
                      </div>
                  </div>
              </form>
          </div>
          <p>
              掌握以上给定的HTML布局方法 <br />
              基于以上已实现的form布局，通过添加CSS代码实现需要的样式
              <br />
              参考：https://v3.bootcss.com/css/#forms
              <br />
              <b>原理与思路</b><br />
              所有文本，密码的input元素，全局横向占用最大，圆角，内边距；<br />
              声明form-group类，放置说明与输入域的行容器，横向弹性布局，元素居中对齐；<br />
              声明form-label类，放置说明，文本右对齐，占20%，注意弹性百分比的占用属性；<br />
              声明form-input类，放置输入域，互交的输入域可能是多个，因此是容器，占80%；<br />
              元素间，通过内边距推开
          </p>
      </body>
  </html>
  ```

  * $form-group$声明之后，后面的子元素就自动成为了弹性项，即$form-label,form-input$
  * 通过$.xxx$在直接选择类的样式
  * **全局横向占用最大**：也就是$width=100\%$，相对于父元素占比

* ### <font color=#000000 size=4 face=粗体>homework1 需求2解读</font>

  

  ```html
  <!--
   * @Author: NEFU AB-IN
   * @Date: 2021-10-12 16:47:01
   * @FilePath: \web-homeworks\src\main\webapp\homework-01\form2.html
   * @LastEditTime: 2021-10-12 18:15:37
  -->
  <!DOCTYPE html>
  <html lang="en">
      <head>
          <meta charset="UTF-8" />
          <meta http-equiv="X-UA-Compatible" content="IE=edge" />
          <meta name="viewport" content="width=device-width, initial-scale=1.0" />
          <title>form2</title>
  
          <style>
              * {
                  padding: 0;
                  margin: 0;
                  box-sizing: border-box;
              }
              .main {
                  border: 1px solid red;
                  width: 800px;
                  padding: 15px;
              }
              .form-group {
                  position: relative;
              }
              input {
                  display: flex;
                  border: none;
                  border-bottom: 1px black solid;
                  padding: 5px;
                  transition-property: border-bottom;
                  transition-duration: 1s;
                  margin: 20px;
              }
              label {
                  position: absolute;
                  top: 0px;
                  left: 0;
                  color: #000;
                  font-size: 1em;
                  pointer-events: none; /* 穿透作用，点文字穿透到文本框*/
                  transition: all 1s;
                  padding: 5px;
              }
              input:focus {
                  outline: none;
                  border-bottom: 1px blue solid;
              }
              input:focus + label,
              input:valid + label {
                  font-size: 0.5em;
                  top: -20px;
                  color: green;
              }
          </style>
      </head>
      <body>
          <div class="main">
              <h2>登录</h2>
              <br />
              <form>
                  <div class="form-group">
                      <input
                          type="text"
                          class="form-control"
                          id="Xuehao"
                          required
                      />
                      <label for="Xuehao">学号</label>
                  </div>
                  <div class="form-group">
                      <input
                          type="password"
                          class="form-control"
                          id="Password"
                          required
                      />
                      <label for="Password">密码</label>
                  </div>
              </form>
          </div>
      </body>
  </html>
  ```

  * 首先在布局方面，$input$应在$label$的上面，因为只有这样才能通过$input$的动作从而影响到$label$的变大变小

  * 其次就是要调整$label$的位置，因为要变大变小，为了不影响别的元素，需要设定**绝对位置**$absolute$，而绝对位置要找**最近的一个可定位的位置容器**，那么$form-group$就需要声明$relative$。之后调整位置，让学号在那条横线上

  * $font-size: 1em;$字体用$em$作为单位，默认$1em=16px$，$rem$相对于根

  * ![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-fKBjAoXb-1634047589662)(C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211012213336371.png)\]](https://img-blog.csdnimg.cn/c71115405b9d449ea45d4cff140a61df.png)

    * 伪类$focus$代表元素成为焦点时

      ```css
      input:focus {
          outline: none; // 当输入时，四周没有框
          border-bottom: 1px blue solid;
      }
      ```

      * $focus$ 代表当$input$ 被写入时，$label$的反应

      * $label$ 代表 当$input$被写入**合法**时，$label$的状态，这里的不合法，就相当于没写，所以要规定这种状态，就需要在$input$中加上$required$选项，也就是说必须写

        由于$label$就可以**使块一直保持$focus$时的状态**

        ![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-nAj4aOED-1634047589664)(C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211012211101598.png)\]](https://img-blog.csdnimg.cn/e108fdffab3647fb9cee5645865dbbe5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATkVGVSBBQi1JTg==,size_20,color_FFFFFF,t_70,g_se,x_16)

        所以这里$+$改成~也可以

        ```css
        input:focus + label,
            input:valid + label {
                font-size: 0.5em;
                top: -20px;
                color: green;
        }
        ```