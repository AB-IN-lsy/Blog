* * <font color=#000000	size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

    <font color=#FFA500 size=5 face=楷体>[Link](https://github.com/bwhyman/web-course/tree/master/web-experiments/experiment-04)</font>
    
    @[TOC]
    
    # <font color=#6495ED size=6 >WebExperiment-04</font>
    * ### <font color=#000000 size=4 face=粗体>需求</font>
  
    
    	> **需求+1**
    模拟二级菜单无法全部展开的，多二级菜单的侧边栏。 通过jquery实现点击一级标题展开其下的二级菜单，同时收缩其他已展开的二级菜单。即，同一时间只有一个二级菜单展开。
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/9df236fafa704bbba93bfabd4087b501.gif#pic_center)
    **需求+1**
    必须同意协议，方可填写注册表单
    用户名必须大于等于6位，否则弹出警告框
    未来意向，支持取消选中的单选框
    喜欢的课程，能且仅能选择2项
    当用户名大于等于6字符，喜欢的课程小于等于2项时，不可提交表单
    **需求+1**
    输入地址后，点击添加地址按钮，将输入信息动态添加至列表
    **需求+1**
    意向，再次点击radio时取消radio的选中状态。![在这里插入图片描述](https://img-blog.csdnimg.cn/2e2bdb87c62c405aaabd4fccff383fb9.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATkVGVSBBQi1JTg==,size_8,color_FFFFFF,t_70,g_se,x_16#pic_center)
    
    
    * ### <font color=#000000 size=4 face=粗体>experiment4 需求1解读</font>
    
      
    
      ```html
      <!--
       * @Author: NEFU AB-IN
       * @Date: 2021-10-21 10:45:33
       * @FilePath: \web-experiments\experiment-04\src\main\webapp\sidebar1.html
       * @LastEditTime: 2021-10-21 11:41:29
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
                  /* 左侧边栏容器宽度 */
                  aside {
                      width: 300px;
                      background: #f1f1f1;
                  }
                  /* 导航标题 */
                  .sidebar > h2 {
                      padding: 15px 20px;
                      background: #3185bf;
                      color: white;
                  }
                  /* 添加标题悬浮样式 */
                  .sidebar > h2:hover {
                      cursor: pointer;
                      background: #000;
                  }
                  /* 初始化时，将二级菜单隐藏，滑动时jquery自动修改display属性 */
                  .sidebar ul {
                      display: none;
                  }
                  /* 二级菜单样式 */
                  .sidebar li a {
                      display: block;
                      color: black;
                      text-decoration: none;
                      padding: 10px 20px;
                  }
                  /* 二级菜单悬浮样式 */
                  .sidebar li a:hover {
                      font-weight: bold;
                      background-color: #555;
                      color: white;
                  }
              </style>
          </head>
          <body>
              <aside>
                  <div class="sidebar">
                      <h2>云技术管理</h2>
                      <ul>
                          <li><a href="#">云服务器</a></li>
                          <li><a href="#">云数据库</a></li>
                          <li><a href="#">负载均衡</a></li>
                      </ul>
                  </div>
                  <div class="sidebar">
                      <h2>安全管理</h2>
                      <ul>
                          <li><a href="#">云盾控制台</a></li>
                          <li><a href="#">DDoS高防IP</a></li>
                          <li><a href="#">Web应用防火墙</a></li>
                          <li><a href="#">CA证书服务</a></li>
                      </ul>
                  </div>
              </aside>
              <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
              <script>
                  $(".sidebar h2").click(function () {
                      let ul = $(this).next("ul");
                      ul.slideToggle("slow");
                      $(".sidebar ul").not(ul).slideUp();/*让除了现在处理的一个ul，其他的全部上滑*/
                  });
              </script>
          </body>
      </html>
      ```
    
      
    
      * 可在事件回调函数中直接使用**jquery**对象，当需要使用$this$对象时，不能使用**箭头函数**，要使用$function()$
    
        比如此处就是找**这个函数对象**的下一个$ul$，相当于精确到个体了，如果把$this$换成$.sidebar \ h2$，会导致选中所有的$h2$，而$this$是就那一个$h2$
    
        
    
      * 常用的函数
    
        ![\[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-1Ou3AP2o-1634789558532)(C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211021120935254.png)\]](https://img-blog.csdnimg.cn/ec262f332f8449ddb47c4cf9987ae575.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBATkVGVSBBQi1JTg==,size_20,color_FFFFFF,t_70,g_se,x_16)
    
    * ### <font color=#000000 size=4 face=粗体>experiment4 需求2解读</font>
    
      实现要求
    
      * **必须同意协议，方可填写注册表单**
      * **用户名必须大于等于6位，否则弹出警告框**
      * **未来意向，支持取消选中的单选框，即再次点击radio时取消radio的选中状态**
      * **喜欢的课程，能且仅能选择2项**
      * **输入地址后，点击添加地址按钮，将输入信息动态渐变添加至列表**
      * **当用户名大于等于6字符，喜欢的课程小于等于2项时，不可提交表单**
    
      源码
    
      ```html
      <!--
       * @Author: NEFU AB-IN
       * @Date: 2021-10-21 16:06:20
       * @FilePath: \web-experiments\experiment-04\src\main\webapp\register1.html
       * @LastEditTime: 2021-10-21 21:07:50
      -->
      <!DOCTYPE html>
      <html lang="en">
          <head>
              <meta charset="UTF-8" />
              <title>Title</title>
          </head>
          <body>
              <h1>注册</h1>
              <label>
                  <input
                      type="checkbox"
                      id="legal"
                  />我已阅读相关说明并遵守相关法律</label
              >
              <form id="register">
                  <div>
                      用户名:
                      <input type="text" name="username" />
                      <br />
                      未来意向：
                      <label
                          ><input
                              type="radio"
                              name="purp"
                              value="1"
                          />Java工程师</label
                      >
                      <label
                          ><input
                              type="radio"
                              name="purp"
                              value="2"
                          />测试工程师</label
                      >
                      <label
                          ><input
                              type="radio"
                              name="purp"
                              value="3"
                          />前端工程师</label
                      >
                      <!-- 添加隐藏域 -->
                      <input type="hidden" name="purpose" />
                      <br />
                      <br />
                      请从以下课程中选择2项最喜欢的课程
                      <ul>
                          <li>
                              <label>
                                  <input type="checkbox" name="courses" />Web开发技术
                              </label>
                          </li>
                          <li>
                              <label>
                                  <input type="checkbox" name="courses" />软件项目管理
                              </label>
                          </li>
                          <li>
                              <label>
                                  <input type="checkbox" name="courses" />数据库原理
                              </label>
                          </li>
                          <li>
                              <label>
                                  <input
                                      type="checkbox"
                                      name="courses"
                                  />系统分析与设计
                              </label>
                          </li>
                      </ul>
                      地址：
                      <ul id="ul_address"></ul>
                      <input name="address" />
                      <button type="button" id="button_address">添加地址</button>
                      <br />
                  </div>
                  <button type="submit">提交</button>
              </form>
          </body>
          <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
          <script>
              let regiserInput = $("#register :input").prop("disabled", true);
              let usernameFlag = false;
              let coursesFlag = false;
              $("#legal").click(function () {
                  let flag = !$(this).prop("checked");
                  regiserInput.not($("button:submit")).prop("disabled", flag);
              });
              const len = 6;
              $("input[name = username]").change(function () {
                  let flag = $(this).val().trim().length;
                  if (flag < 6) {
                      alert("用户名长度必须大于等于6");
                      usernameFlag = false;
                  } else {
                      usernameFlag = true;
                  }
              });
              const purpBox = $("input[name = purp]");
              purpBox.click(function () {
                  let purpFlag = $(this).data("status");
                  if (purpFlag) {
                      $(this).data("status", false);
                      $(this).prop("checked", false);
                  } else {
                      $(this).data(
                          "status",
                          true
                      ); /* 可能点亮2时，1也在亮，但是又不能同时两个亮
                      就会起冲突，导致按2不亮，反而1亮的灭了 */
                      purpBox.not(this).data("status", false);
                  }
              });
              const cnt = 2;
              const coursesBox = $("input[name = courses]");
              coursesBox.click(function () {
                  coursesFlag = coursesBox.filter(":checked").length >= cnt;
                  coursesBox.not(":checked").prop("disabled", coursesFlag);
              });
              $("input[name=username], input[name=courses]").change(function () {
                  let flag = usernameFlag && coursesFlag;
                  $("button:submit").prop("disabled", !flag);
              });
              $("#button_address").click(function () {
                  const addressBox = $("input[name = address]");
                  let flag = addressBox.val().length > 0;
                  //console.log(flag);
                  if (flag) {
                      let content = addressBox.val();
                      let newLi = $(`<li>${content}</li>`);
                      addressBox.val("");
                      newLi.css("display", "none");
                      $("#ul_address").append(newLi);
                      newLi.fadeIn();
                  }
              });
          </script>
      </html>
      
      ```
    
      * **必须同意协议，方可填写注册表单**
    
        ```javascript
        let regiserInput = $("#register :input").prop("disabled", true);
        $("#legal").click(function () {
            let flag = !$(this).prop("checked");
            regiserInput.not($("button:submit")).prop("disabled", flag);
        });
        ```
    
        思路就是先将所有$type$是$input$的对象，用$prop$将其变成不可编辑的状态
    
        然后当点击按钮时，用$prop$获取按钮是否处于$checked$状态，然后将除了$submit$的按钮与$checked$状态相反
    
        * `let regiserInput = $("#register :input").prop("disabled", true);` 
    
          **链式结构**，最后得到的取决于**可获得值的最后一部分**
    
          比如此处，$prop$两个参数，是设置，所以往前找，前面是获得$input$，所以返回值就是所有$type$是$input$的对象
    
        * $prop$，一个参数是得返回值，两个参数是设置
    
      * **用户名必须大于等于6位，否则弹出警告框**
    
        ```javascript
        let usernameFlag = false;
        const len = 6;
        $("input[name = username]").change(function () {
            let flag = $(this).val().trim().length;
            if (flag < 6) {
                alert("用户名长度必须大于等于6");
                usernameFlag = false;
            } else {
                usernameFlag = true;
            }
        });
        ```
    
        * 当监听的某个状态改变，执行某项操作的函数，用**change()**
        * **val()**，获取文本框的输入值
        * **trim()**，去除空格
    
      * **未来意向，支持取消选中的单选框，即再次点击radio时取消radio的选中状态**
    
        ```javascript
        const purpBox = $("input[name = purp]");
        purpBox.click(function () {
            let purpFlag = $(this).data("status");
            if (purpFlag) {
                $(this).data("status", false);
                $(this).prop("checked", false);
            } else {
                $(this).data(
                    "status",
                    true
                ); /* 可能点亮2时，1也在亮，但是又不能同时两个亮
                        就会起冲突，导致按2不亮，反而1亮的灭了 */
                purpBox.not(this).data("status", false);
            }
        });
        ```
    
        > 无法基于radio checked状态判断，因为每次点击radio时该radio均为checked状态	
    
        所以考虑为按钮再添加一个状态，表示是否$checked$
    
        * **data()**
          * 一个属性表示添加状态并返回空  **或者** 获取状态
          * 两个属性表示设置这个属性的值
          * **注意**! 新增的属性不能用$prop$，只能用$data$
        * 注意当点亮一个灯时，要让其他的灯熄灭
        * 另外，可以将输入框对象用$const$定义，这样可以避免重复查询
    
      * **喜欢的课程，能且仅能选择2项**
    
        ```javascript
        let coursesFlag = false;
        const cnt = 2;
        const coursesBox = $("input[name = courses]");
        coursesBox.click(function () {
            coursesFlag = coursesBox.filter(":checked").length >= cnt;
            coursesBox.not(":checked").prop("disabled", coursesFlag);
        });
        ```
    
        那么就是选择两项之后，将别的按钮禁止访问即可
    
        注意这里也有链式结构，后面是判断，所以是布尔类型的
    
      * **输入地址后，点击添加地址按钮，将输入信息动态渐变添加至列表**
    
        ```javascript
        $("#button_address").click(function () {
            const addressBox = $("input[name = address]");
            let flag = addressBox.val().length > 0;
            //console.log(flag);
            if (flag) {
                let content = addressBox.val();
                let newLi = $(`<li>${content}</li>`);
                addressBox.val("");
                newLi.css("display", "none");
                $("#ul_address").append(newLi);
                newLi.fadeIn();
            }
        });
        ```
    
        * 先判断是否有内容可以添加
        * 再获取文本框里的值
        * 定义新的$list \ item$，利用**反引号**，将属性值添加在字符串中
        * 将文本框清空 ` addressBox.val("");`
        * 将新的$list \ item$隐藏，注意此处的$none$需要用引号，**因为不是基本类型**
        * 再进行添加操作，并让$list \ item$淡入
    
      * **当用户名大于等于6字符，喜欢的课程小于等于2项时，不可提交表单**
    
        ```javascript
        $("input[name=username], input[name=courses]").change(function () {
            let flag = usernameFlag && coursesFlag;
            $("button:submit").prop("disabled", !flag);
        });
        ```
    
        同时监听**2**个状态的改变，当两个都改变的时候，才进行
    
        但无需重写以上校验代码，仅需判断之前校验产生的状态标识即可