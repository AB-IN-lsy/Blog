<font color=#000000	size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

<font color=#FFA500 size=5 face=楷体>[Link](https://github.com/bwhyman/web-course/tree/master/web-experiments/experiment-07)</font>

@[TOC]

# <font color=#6495ED size=6 >WebExperiment-07</font>

* ### <font color=#000000 size=4 face=粗体>需求</font>

  > 创建maven项目experiment-07
  > 声明项目打包类型，java版本，Servlet/JSTL/MySQL驱动/Annotation依赖，添加打包插件
  > 远程IP：xx.xx.xx.xx。视频Remote MySQL Connections
  >
  > - 基于idea database视图，个人学号/密码登录远程MySQL数据库
  > - 在已经默认创建的以个人学号命名的数据库下，创建数据表user，添加id/name/inserttime数据段，并声明合适类型
  > - inserttime段为时间戳类型，默认值为随段信息更新而自动更新
  > - 添加若干测试数据记录
  >
  > 在webapp下，创建META-INF目录，直接复制context.xml资源配置文件，修改配置数据
  > 在com.entity下创建User实体类，声明id/name/insertTime等合适数据类型属性，对应数据库字段
  > 在com.util下，创建容器启动监听器DataSourceUtils，丛JNDI树获取DataSource对象，暴露连接对象获取方法
  > 创建全局过滤器，修改请求/响应编码，否则提交的数据会按ISO编码并保存在数据库
  >
  > **需求+1**
  > 在com.controller下，创建IndexServlet，重写doGet()方法。 基于JDBC查询全部用户信息，封装每一条记录为对象，创建集合封装对象，推送到index.jsp页面
  > 在/WEB-INF/jsp/下，创建index.jsp，基于JSTL标签，加载全部用户为列表
  > 页面声明动态获取部署路径，作为页面基本路径
  >
  > **需求+1**
  > 在index.jsp添加form表单，添加基于用户集合动态创建的下拉框；添加输入框
  > 在com.controller下，创建UpdateUserServlet类，接收用户ID与新用户名，通过JDBC修改指定ID的用户名
  > 并重定向回index，在Servlet中可通过req.getContextPath()方法，获取项目部署路径，拼接重定向地址(部署路径可能无法确定)
  >
  > **需求+1**
  > 在index.jsp用户名列表，添加跳转超链接 在com.controller下，创建GetUserServlet类，基于接收的用户ID，查询用户新建，封装。转发至query.jsp视图 创建query.jsp，显式用户详细信息
  >
  > 运行部署项目至Tomcat，向index发起请求，查看结果，并测试功能
  >
  > ![](C:\Users\liusy\Pictures\jdbc-01.png)

* ### <font color=#000000 size=4 face=粗体>项目结构</font>

  ![image-20211112111022704](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211112111022704.png)

* ### <font color=#000000 size=4 face=粗体>experiment7 第一部分解读</font>

  首先创建User实体类

  ```java
  package com.entity;
  
  import java.util.Date;
  
  /**
   * @Author: liusy
   * @Time : 2021/11/11 16:49
   * @Project : web-experiments
   */
  public class User {
      private int id;
      private String name;
      private Date insertTime;
      private Date updateTime;
  
      public User() {
      }
      public User(int id, String name, Date insertTime, Date updateTime) {
          this.id = id;
          this.name = name;
          this.insertTime = insertTime;
          this.updateTime = updateTime;
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
  
      public Date getInsertTime() {
          return insertTime;
      }
  
      public void setInsertTime(Date insertTime) {
          this.insertTime = insertTime;
      }
  
      public Date getUpdateTime() {
          return updateTime;
      }
  
      public void setUpdateTime(Date updateTime) {
          this.updateTime = updateTime;
      }
  }
  
  ```

  ****

  之后配置**数据库**的两个文件，即配置DataSource接口

  * **DataSourceUtils.java**

    ![image-20211112112040692](C:\Users\liusy\AppData\Roaming\Typora\typora-user-images\image-20211112112040692.png)

  * **context.xml**

    项目只要声明了context.xml配置文件，Servlet容器(Tomcat)启动时，就基于此配置连接数据库，创建数据源对象。

* <font color=#000000 size=4 face=粗体>experiment7 第二部分解读</font>

  先编写过滤器

  **EncodingFilter**

  **目的：**将所有页面的编码改成UTF-8

  ```java
  package com.filter;
  
  import javax.servlet.FilterChain;
  import javax.servlet.ServletException;
  import javax.servlet.annotation.WebFilter;
  import javax.servlet.http.HttpFilter;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  import java.io.IOException;
  
  
  @WebFilter("/*")
  public class EncodingFilter extends HttpFilter {
      @Override
      protected void doFilter(HttpServletRequest req, HttpServletResponse resp, FilterChain chain) throws IOException, ServletException{
          req.setCharacterEncoding("UTF-8");
          resp.setCharacterEncoding("UTF-8");
          chain.doFilter(req, resp);
      }
  }
  
  ```

  **注意！**

  * filter与servlet不一样，有**三个参数**，最后一个用来传递req和resp，因为它只是个过滤器，最后目的地址和源地址都是得原封不动传下去的！

  

  ****

  编写jsp文件

  **index.jsp**

  ```javascript
  <%@ page pageEncoding="UTF-8" %>
  <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <c:url var="base" value="/"/>
      <base href="${base}">
      <meta charset="UTF-8">
      <title>Title</title>
  </head>
  
  <body>
  <h3>Query</h3>
  
  <h4>全部用户</h4>
  <ul>
      <c:forEach items="${users}" var="user">
          <li>
                  ${user.name}
          </li>
      </c:forEach>
  </ul>
  <hr>
  
  
  <form action="update" method="post">
      <h3>Update</h3>
      <h4>修改指定用户</h4>
      <select name="uid"> <%--必须写在select中--%>
          <c:forEach items="${users}" var="user">
              <option value="${user.id}">
                      ${user.name}
              </option>
          </c:forEach>
      </select>
      <label>
          newname
      </label>
      <input type="text" name="name" required>
      <br>
      <button type="submit">提交</button>
  </form>
  
  
  
  <hr>
  <h3>Query</h3>
  
  <ul>
      <c:forEach items="${users}" var="user">
          <li>
              <a href="getuser?uid=${user.id}"> <%--不能加空格！！！！ --%>
                      ${user.name}
              </a>
          </li>
      </c:forEach>
  </ul>
  
  </body>
  </html>
  ```

  **注意：**

  * `<select name="uid"> <%--必须写在select中--%>` 

    **必须写在select里，不能写在下面的option中**

  * `<a href="getuser?uid=${user.id}"> <%--不能加空格！！！！ --%>`

    加空格会导致解析网址错误，会有`%20`这样的字符出现

  这里呈现了**三个部分**

  * 列出用户名称
  * 更新用户名字
  * 转到用户详情页

  既然写完了jsp，那么就得先写对应的servlet

  

  ****

  **IndexServlet**

  **目的：** 在访问`/index`时，从数据库中获取所有的用户，形成一个列表转发给index.jsp

  ```java
  package com.controller;
  
  import com.entity.User;
  import com.util.DataSourceUtils;
  
  import javax.servlet.ServletException;
  import javax.servlet.annotation.WebServlet;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  import java.io.IOException;
  import java.lang.reflect.Array;
  import java.sql.Connection;
  import java.sql.PreparedStatement;
  import java.sql.ResultSet;
  import java.sql.SQLException;
  import java.util.ArrayList;
  import java.util.List;
  
  /**
   * @Author: liusy
   * @Time : 2021/11/11 18:47
   * @Project : web-experiments
   */
  @WebServlet("/index")
  public class IndexServlet extends HttpServlet {
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          List<User> users = new ArrayList<>();
          String sql = "select * from user";
          try(Connection conn = DataSourceUtils.getConnection();
              PreparedStatement st = conn.prepareStatement(sql);
              ResultSet rs = st.executeQuery()){
              while (rs.next()) {
                  User user = new User(rs.getInt("id"),
                          rs.getString("name"),
                          rs.getTimestamp("insert_time"),
                          rs.getTimestamp("update_time"));
                  users.add(user);
              }
          }
          catch (SQLException e) {
              e.printStackTrace();
          }
          req.setAttribute("users", users);
          req.getRequestDispatcher("/WEB-INF/jsp/index.jsp")
                  .forward(req, resp);
      }
  }
  
  ```

  这里就用到了和数据库进行交互的知识，从表user中获取所有的对象，这里会用到比较固定的三条语句

  * `Connection conn = DataSourceUtils.getConnection();`

    意思就是通过暴露的connection对象方法，对数据库进行连接

  *  `PreparedStatement st = conn.prepareStatement(sql);`

    获取执行sql的对象，即处理sql语句

    比如

    * `String sql = "select * from user";`

    * `String sql = "update user set name = ? where id = ?"`，这里的`"?"`就代表后面要设置的参数，第一个问号的下标就是1

      后面就会进行

      ```java
      st.setString(1, newName);
      st.setInt(2, id);
      st.executeUpdate();
      ```

      **注意！**，如果像`update`, `insert`这种没有返回结果集的操作，**一定不要忘了最后一句话执行！**

  *  `ResultSet rs = st.executeQuery();`

    像这种有结果集的，就要进行获取

* <font color=#000000 size=4 face=粗体>experiment7 第三部分解读</font>

  接下来编写查询板块的servlet（查询的表单在index.jsp中提交了）

  **UpdateUserServlet**

  **目的：**处理index.jsp中提交的表单，通过数据库将对象的名字修改成传过来的新名字

  ```java
  package com.controller;
  
  import com.entity.User;
  import com.util.DataSourceUtils;
  
  import javax.servlet.ServletException;
  import javax.servlet.annotation.WebServlet;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  import java.io.IOException;
  import java.sql.Connection;
  import java.sql.PreparedStatement;
  import java.sql.SQLException;
  
  import static java.lang.Integer.parseInt;
  
  /**
   * @Author: liusy
   * @Time : 2021/11/11 19:36
   * @Project : web-experiments
   */
  @WebServlet("/update")
  public class UpdateUserServlet extends HttpServlet {
      @Override
      protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          String newName = req.getParameter("name");
          int id = parseInt(req.getParameter("uid"));
          String sql = "update user set name = ? where id = ?";
          try (Connection conn = DataSourceUtils.getConnection();
               PreparedStatement st = conn.prepareStatement(sql)) {
              st.setString(1, newName);
              st.setInt(2, id);
              st.executeUpdate();
          } catch (SQLException e) {
              e.printStackTrace();
          }
          resp.sendRedirect(req.getContextPath() + "/index");
      }
  }
  
  ```

  ****

  编写查询的jsp

  **query**

  ```javascript
  <%@ page pageEncoding="UTF-8" %>
  <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>query</title>
  </head>
  <body>
  <label>id：</label>${user.id }<br>
  <label>name：</label>${user.name } <br>
  <label>insert_time：</label>${user.insertTime } <br>
  <label>update_time：</label>${user.updateTime } <br>
  </body>
  </html>
  ```

  ****

  编写对应的servlet

  **GetUserServlet**

  **目的：** 接收index.jsp传的uid参数，通过uid在数据库中找到对应的对象，将对象的信息转发给query.jsp

  ```java
  package com.controller;
  
  import com.entity.User;
  import com.util.DataSourceUtils;
  
  import javax.servlet.ServletException;
  import javax.servlet.annotation.WebServlet;
  import javax.servlet.http.HttpServlet;
  import javax.servlet.http.HttpServletRequest;
  import javax.servlet.http.HttpServletResponse;
  import java.io.IOException;
  import java.sql.Connection;
  import java.sql.PreparedStatement;
  import java.sql.ResultSet;
  import java.sql.SQLException;
  
  import static java.lang.Integer.parseInt;
  
  /**
   * @Author: liusy
   * @Time : 2021/11/11 19:36
   * @Project : web-experiments
   */
  @WebServlet("/getuser")
  public class GetUserServlet extends HttpServlet {
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          User user = null;
          int uid = parseInt(req.getParameter("uid"));
          String sql = "select * from user where id=?";
          try (Connection conn = DataSourceUtils.getConnection();
               PreparedStatement st = conn.prepareStatement(sql)) {
              st.setInt(1, uid);
              try (ResultSet rs = st.executeQuery()) {
                  while (rs.next()) {
                      user = new User();
                      user.setId(rs.getInt("id"));
                      user.setName(rs.getString("name"));
                      user.setInsertTime(rs.getTimestamp("insert_time"));
                      user.setUpdateTime(rs.getTimestamp("update_time"));
                  }
              }
          } catch (SQLException e) {
              e.printStackTrace();
          }
          req.setAttribute("user", user);
          req.getRequestDispatcher("/WEB-INF/jsp/query.jsp")
                  .forward(req, resp);
      }
  }
  
  ```

  * `st.setInt(1, uid);`

    id是可以按string传递的，可自动转换匹配，也就是也可以

    `st.setString(1, req.getParameter("uid"));`

  

