<font color=#000000	size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

<font color=#FFA500 size=5 face=楷体>[Link](https://github.com/bwhyman/web-course/tree/master/web-experiments/experiment-06)</font>

@[TOC]

# <font color=#6495ED size=6 >WebExperiment-06</font>

* ### <font color=#000000 size=4 face=粗体>需求</font>

  > **实验内容**
  >
  > 创建maven Web项目及模块，experiment-07 声明项目打包类型，声明java版本，添加Servlet依赖，添加JSTL依赖
  > 创建webapp目录，创建WEB-INF安全目录，创建jsp目录
  > entity下的实体类，以及util下的模拟数据类DatabaseUtils可直接复制使用，无需编写。
  >
  > **需求+1**
  > 以表格形式显示所有注册教师姓名及注册时间
  > 且教师姓名为可点击的超链接，点击后跳转至教师详细信息页面
  > 在详细页面以表单形式，基于教师注册过的信息，加载初始化页面
  > 提交表单后，在控制台打印显示修改后的数据
  >
  > **解决方案**
  > 项目结构规范：所有控制层组件servlet，置于com.controller包下；所有视图文档，置于/WEB-INF/jsp/目录下
  > 创建加载显示全部教师的listteachers.jsp页面， 在文档顶部引入JSTL标签库，通过EL表达式与JSTL标签，动态生成表格，动态生成超链接的地址及参数数据
  > 创建处理对应请求的ListTeachersServlet，调用DatabaseUtils中的相应方法，获取全部教师对象并转发至视图页面
  > 创建updateteacher.jsp页面以及表单，在表单标签中加载教师详细信息， 在页面引入核心及格式化标签库，按以下样式加载基本数据，基于逻辑判断， 按教师原注册的职称及课程数据，渲染初始化页面
  > 创建处理对应请求的UpdateTeacherServlet，获取教师ID参数，调用DatabaseUtils中的相应方法， 将：指定教师/全部职称/全部课程对象，转发至页面
  > 由UpdateTeacherServlet，同时处理提交修改post请求，获取请求参数并打印显示
  >
  > ![img](https://github.com/bwhyman/web-course/raw/master/web-experiments/experiment-06/asserts/exp06-01.png)
  >
  > ![img](https://github.com/bwhyman/web-course/raw/master/web-experiments/experiment-06/asserts/exp06-02.png)

* ### <font color=#000000 size=4 face=粗体>项目结构</font>

  ![image-20211104184238187](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211104184238187.png)

* ### <font color=#000000 size=4 face=粗体>experiment6 第一部分解读</font>

  首先创建三个实体类，后创建类似数据库的类

  ```java
  package com.entity;
  
  /**
   * @Author: liusy
   * @Time : 2021/11/4 8:27
   * @Project : web-course
   */
  public class Title {
      private int id;
      private String name;
  
      public Title(int id, String name) {
          this.id = id;
          this.name = name;
      }
  
      public int getId() {
          return id;
      }
  
      public void setId(int id) {
          this.id = id;
      }
  
      public String getName() {
          return name;
      }
  
      public void setName(String name) {
          this.name = name;
      }
  }
  
  ```

  ```java
  package com.entity;
  
  /**
   * @Author: liusy
   * @Time : 2021/11/4 8:26
   * @Project : web-course
   */
  public class Course {
      private int id;
      private String name;
  
      public Course(int id, String name) {
          this.id = id;
          this.name = name;
      }
  
      public int getId() {
          return id;
      }
  
      public void setId(int id) {
          this.id = id;
      }
  
      public String getName() {
          return name;
      }
  
      public void setName(String name) {
          this.name = name;
      }
  }
  
  ```

  ```java
  package com.entity;
  
  import java.util.Date;
  import java.util.List;
  
  /**
   * @Author: liusy
   * @Time : 2021/11/4 8:26
   * @Project : web-course
   */
  public class Teacher {
      private int id;
      private String name;
      private Title title;
      private List<Course> courses;
      private Date date;
  
      public Teacher(int id, String name, Title title, List<Course> courses, Date date) {
          this.id = id;
          this.name = name;
          this.title = title;
          this.courses = courses;
          this.date = date;
      }
  
      public int getId() {
          return id;
      }
  
      public void setId(int id) {
          this.id = id;
      }
  
      public String getName() {
          return name;
      }
  
      public void setName(String name) {
          this.name = name;
      }
  
      public Title getTitle() {
          return title;
      }
  
      public void setTitle(Title title) {
          this.title = title;
      }
  
      public List<Course> getCourses() {
          return courses;
      }
  
      public void setCourses(List<Course> courses) {
          this.courses = courses;
      }
  
      public Date getDate() {
          return date;
      }
  
      public void setDate(Date date) {
          this.date = date;
      }
  }
  
  ```

  

  ```java
  package com.util;
  
  import com.entity.Course;
  import com.entity.Teacher;
  import com.entity.Title;
  
  import java.util.Date;
  import java.util.List;
  
  /**
   * @Author: liusy
   * @Time : 2021/11/4 8:27
   * @Project : web-course
   */
  public class DatabaseUtils {
      private static List<Teacher> teachers;
      private static List<Title> titles;
      private static List<Course> courses;
      static {
  
          Title t1 = new Title(1, "讲师");
          Title t2 = new Title(2, "副教授");
          Title t3 = new Title(3, "教授");
          titles = List.of(t1, t2, t3);
  
  
          Course c1 = new Course(1, "Web开发技术");
          Course c2 = new Course(2, "Java程序设计");
          Course c3 = new Course(3, "数据库原理");
          Course c4 = new Course(4, "软件项目管理");
          courses = List.of(c1, c2, c3, c4);
  
          Teacher teach1 = new Teacher(1, "唐晨阳",t3, List.of(c1, c4), new Date());
          Teacher teach2 = new Teacher(2, "刘春枝", t2, List.of(c2, c3), new Date());
          teachers = List.of(teach1, teach2);
      }
  
      public static List<Teacher> getTeachers(){
          return teachers;
      }
      public static List<Course> getCourses() {
          return courses;
      }
      public static List<Title> getTitles(){
          return titles;
      }
      public static Teacher getTeacher(int id){
          return teachers.stream()
                  .filter(c -> c.getId()==id)
                  .findFirst()
                  .orElse(null);
      }
  }
  
  ```

  * 需要获取的就是$Teanchers$, $Title$,$Courses$三个列表，需要用get函数获取，又是至于类相关的，所以设置成`private static`

  * **三个列表的生成**

    可以看作是类加载时就完成了，相当于预处理，那么就可以把他们放在**静态代码块**中进行加载，这样就不用放在get函数中了

  * 另外就是`getTeacher`这个函数，放在之后讲

* ### <font color=#000000 size=4 face=粗体>experiment6 第二部分解读</font>

  **第一个需求**：访问`localhost:8080/listteachers`时，将所有老师的信息通过表格呈现出来

  首先，**一个jsp文件必须对应一个servlet类**，通过**servlet**转发到jsp，才能让jsp生效，所以这两个需要结合起来写

  ```javascript
  <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
  <%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>listteachers</title>
  </head>
  <body>
  <table>
      <tr>
          <td>#</td>
          <td>姓名</td>
          <td>注册时间</td>
      </tr>
      <c:forEach items="${teachers}" var = "teacher" varStatus="i">
          <tr>
              <td>${i.count}</td>
              <td>
                  <a href="updateteacher?tid=${teacher.id}">
                          ${teacher.name}
                  </a>
              </td>
              <td>
                      <fmt:formatDate
                      pattern="yyyy-MM-dd HH:mm"
                      value="${teacher.date}"
  					/>
              </td>
          </tr>
      </c:forEach>
  </table>
  </body>
  </html>
  
  ```

  首先是jsp文件

  * 这里的${}里的对象，**必须是通过servlet传过来的参数**
  * 其次不用通过对象的get函数来访问对象的属性，直接`.`即可
  * `<c:forEach` 
    * Items，集合数组等对象，**必为通过EL获取的集合对象**
    * Var，每次迭代的元素对象(variable方便记忆)
    * varStatus，迭代状态变量
      * Index，当前迭代的索引，从0开始
      * Count，当前迭代的次数，从1开始
      * First，判断，boolean，是否为第一项
      * Last，判断，boolean，是否为最后一项
  * `<fmt:formatDate` 可以自定义日期的格式
  * `<a>`链接，链接第二个页面，下面说

  其次是对应处理请求的servlet

  ```java
  package com.controller;
  
  import javax.servlet.ServletException;
  import javax.servlet.annotation.WebServlet;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  import java.io.IOException;
  import com.util.DatabaseUtils;
  
  /**
   * @Author: liusy
   * @Time : 2021/11/4 8:25
   * @Project : web-course
   */
  @WebServlet("/listteachers")
  public class ListTeacherServlet extends HttpServlet {
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          req.setAttribute("teachers", DatabaseUtils.getTeachers());
          req.getRequestDispatcher("/WEB-INF/jsp/listteachers.jsp")
                  .forward(req, resp);
      }
  
  }
  
  ```

  * `req.setAttribute("teachers", DatabaseUtils.getTeachers());`

    这里就是给req设定参数$teachers$，从数据库里的函数调出来的列表，包含所有

  

* ### <font color=#000000 size=4 face=粗体>experiment6 第三部分解读</font>

  **第二个需求**：我们想通过第一个界面通过点击老师的信息，进入到第二个界面，查看老师的详细信息

  先写jsp

  ```javascript
  <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>updateteacher</title>
  </head>
  <body>
  <form method="post" action="updateteacher">
      <label>
          用户：
          <input type="text" name = "teacherName" value="${teacher.name}">
      </label>
      <br/>
      <label>
          注册时间：${teacher.date}
      </label>
      <br/>
      <label>
          职称：<br/>
          <select name = "teacherTitle">
              <c:forEach items="${titles}" var="t">
                  <c:set var="s" value=""/>
                  <c:if test="${t.id == teacher.title.id}">
                      <c:set var="s" value="selected"/>
                  </c:if>
                  <option value="${t.id}" ${s}>
                          ${t.name}
                  </option>
              </c:forEach>
          </select>
      </label>
  
      <label>
          授课：<br>
          <ul>
              <c:forEach items="${courses}" var="course">
                  <c:set var="s" value=""/>
                  <c:forEach items="${teacher.courses}" var="tc">
                      <c:if test="${course.id == tc.id}">
                          <c:set var="s" value="checked" />
                      </c:if>
                  </c:forEach>
                  <li>
                      <label>
                              ${course.name}
                          <input type="checkbox"
                                 name = "teacherCourses"
                                 value = "${course.id}"
                              ${s}
                          />
                      </label>
                  </li>
              </c:forEach>
          </ul>
      </label>
      <button type="submit">提交</button>
      <button type="reset">重置</button>
  </form>
  </body>
  </html>
  
  ```

  * 从头开始

  * **如何进到这个界面**？

    是通过**第一个jsp**的`<a href="updateteacher?tid=${teacher.id}">`进入jsp渲染的界面，$?$后是传递的参数，这里是传递教师的$id$，这样就代表传递**唯一**的一名教师，到servlet中被处理

  * `<form method="post" action="updateteacher">`，老师的详细信息通过表单形式展出，所以这里的表单就可以不用向以前这样空着了，就可以向`localhost:8080/updateteacher`发起post请求，提交表单了，需要注意**必须对应的servlet用post请求处理，否则会创建项目错误**

  * `<input type="text" name = "teacherName" value="${teacher.name}">`

    **第一部分**是**对象**$$Teacher$$（单一）

    输入文本框，显示老师的名字，这里有三个属性

    * type：即表明这是一个什么类型的输入，比如text,password,checkbox

    * name：在表单提交后引用表单数据，只有设置了 name 属性的表单元素才能在提交表单时传递它们的值。

      也就是说是提交表单后，post请求被带往servlet处理，这是post请求就带有着**变量名**为`teacherName`，**值**为`teacher.name`的变量

    * value：代表输入框的值，也是传表单其中的变量的值，在输入框中也就是输入的值

    * name和value，就是传递的**变量的名字和值**

  * `<select name = "teacherTitle">`

    **第二部分**是挑选出对象$$Teacher$$的**唯一**的$Title$
  
    所以可以在select里写name传输
  
    **具体思想**：先创建一个变量，记录是否与老师的$$Title$$相同，之后遍历整个$Title$列表，并找到那个和这个老师相同的title（实现方法就是找相同的id），相同了变量记为$selected$
    
    `<option value="${t.id}" ${s}>{t.name}</option>`
    
    然后设置选项的value值，由于表单是将**选中的**传上去，那么就是传$selected$的option
    
  *  `<input type="checkbox" name = "teacherCourses" value = "${course.id}" ${s} />`
  
    **第三部分**是挑选出对象$$Teacher$$的**多个**$Course$
  
    由于是多个对象，所以在input中定义变量，这样传输过去的就是一个接一个的$teacherCourses$，最后可以在servlet中组成列表
  
    **具体思想**：先创建一个变量，记录是否与老师的$$Courses$$相同，先遍历总的$$Courses$$，再遍历老师的$$Courses$$，选出相同的标记为$checked$
  
  * `<c:if`中$test$位必须参数，即布尔值
  
  servlet
  
  ```javascript
  package com.controller;
  
  import javax.servlet.ServletException;
  import javax.servlet.annotation.WebServlet;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  import java.io.IOException;
  import java.util.Arrays;
  import java.util.logging.Logger;
  
  import static com.util.DatabaseUtils.*;
  import static java.lang.Integer.parseInt;
  
  /**
   * @Author: liusy
   * @Time : 2021/11/4 8:25
   * @Project : web-course
   */
  @WebServlet("/updateteacher")
  public class UpdateTeacherServlet extends HttpServlet {
      private static Logger logger = Logger.getLogger(UpdateTeacherServlet.class.getName());
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          int teacherId = parseInt(req.getParameter("tid"));
          req.setAttribute("teacher", getTeacher(teacherId));
          req.setAttribute("courses", getCourses());
          req.setAttribute("titles", getTitles());
  
          req.getRequestDispatcher("/WEB-INF/jsp/updateteacher.jsp")
                  .forward(req, resp);
      }
      @Override
      protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          req.setCharacterEncoding("UTF-8");
          logger.info("姓名：" + req.getParameter("teacherName"));
          logger.info("职称ID：" + req.getParameter("teacherTitle"));
          logger.info("课程ID：" + Arrays.asList(req.getParameterValues("teacherCourses")));
      }
  }
  
  ```
  
  * `logger.info`，即在控制台中将信息输出出来（当然也可以将在第二页面修改的信息输出出来）
  
  * get请求中，先取出从界面1传过来的教师的id，再给jsp传递所需要的参数
  
    * 这个老师的所有信息，通过数据库中的**流操作**进行获取
    * 所有的$Course$
    * 所有的$Title$
  
    之后转发即可
  
    * post请求中设置编码，并输出通过界面2传递过来的表单的数据，可以看到表单中的name：$teacherName$，$teacherTitle$，$teacherCourses$，均作为变量传过来了，并作为参数从req中可以获取



这样就完成了！
