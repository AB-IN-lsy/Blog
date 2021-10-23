<font color=#000000	size=3 face=楷体>Powered by:**NEFU AB-IN**</font>

<font color=#FFA500 size=5 face=楷体>[Link](https://github.com/bwhyman/web-course)</font>

@[TOC]

# <font color=#6495ED size=6 >WebIntro</font>

* ### <font color=#000000 size=4 face=粗体>声明全局</font>

  ```css
  * {
  	padding: 0;
  	margin: 0;
  	box-sizing: border-box; /*协助调整元素占用尺寸*/
  }
  .area {
      padding: 10px;
      border: 1px solid #f08c00;
      background-color: #ffec99;
      border-radius: 5px;
      /*框架样式，即画一条线包裹框架*/
  }
  .row {
      display: flex;
      align-items: flex-start;
      /*定义弹性的行容器*/
  }
  .row .grow-1 {
      flex-grow: 1;
      /*占剩余全部空间*/
  }
  /*-------- 12栅格 -----------  */
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
  ```

* ### <font color=#000000 size=4 face=粗体>Input</font>

  ```css
  input[type="text"], input[type="password"]  /*根据type选择*/
  input {
      display: flex; /*弹性布局*/	
      border: none;  /*无边框*/
      border-bottom: 1px black solid; /*加个底线*/
      padding: 5px;
      transition-property: border-bottom; /* 需要实现过渡的元素有哪些*/
      transition-duration: 1s;			/* 过渡的时间，与上面一一对应*/
      margin: 20px;
  }		
  ```
