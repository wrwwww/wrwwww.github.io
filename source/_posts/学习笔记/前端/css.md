---
title: css
date: 2023-05-05 21:30:42
tags:  
---
# css

#### 导入css的三种方法

- 使用style标签添加样式

- 在标签的后面添加css样式

- 使用link标签导入外部css文件

  ```html
  <!-- 第一种-->
  <style>h1{
  	background="red";    
  } </style>
  <!-- 第二种-->
  <p style=""></p>
  <!-- 第三种-->
  <link rel="stylesheet" href="css位置">
  ```

  ## 常用的标签

  ```html
  <h1></h1>1~6都是标题
  <p></p> 段落
  <br>换行
  <hr>水平线标签
  <!-- 字体样式标签-->
  粗体<strong> 1234</strong>
  斜体<em>1234</em>
  特殊符号
  &开头;结尾
  <!--图片标签-->
  <img src = "path" alt="图片加载失败显示的文字"
       title="悬停时候显示的文字" width="" height="">
  <!--链接文本或者图片 _self,_blank-->
  <a href="path" target="目标窗口的位置">
  <!--锚链接-->
  <a name="#top"></a>
  <a href="#top" >点击回到顶部</a>    
  <!--列表-->    
  <!--有序-->
  <ol>
      <li>java</li>
      <li>ja</li>
  </ol>
  <!--无序-->
  <ul>
      <li>java</li>
      <li>ja</li>
  </ul>
  <!--自定义序-->
  <dl>
      <dt>java</dt>
      <dt>c</dt>
      <dd>sss</dd>
      <dt>linux</dt>
  </dl>    
  <!--表格-->    
  <table border="1px" style="text-align:center;font-family:verdana,sans-serif;color:red;font-size:20px;">
      <tr>
          <td colspan="4">科目</td>
      </tr>
      <tr>行内
          <td rowspan="2" >班级</td>列内
          <td>2-2</td>
          <td>2-3</td>
          <td>2-4</td>
      </tr>
      <tr>
          <td>2-2</td>
          <td>2-3</td>
          <td>2-4</td>
      </tr>
  </table>    
  <label>标签指向输入框,点击字体跳到输入框  
  placeholder="书籍的数量"输入框提示信息
  <form  action="/h1/addBook" method="get">表单    
  ```

  ## css样式

  ```css
  style{
  background-color:red;背景图片
  font-family:arial,sans-serif;color:red;font-size:20px;font-weight:bold; text-align:center;居中
  background-image: linear-gradient(to right, #6025f5, #ff5555);从左到右渐变
  /*水平阴影，垂直阴影，模糊的距离，以及阴影的颜色*/
  text-shadow: 10px 10px 4px grey;    
  /*盒子阴影*/    
  box-shadow: 5px 5px 5px grey;
  text-decoration: None;去掉a链接的下划线    
  }
  /* 三列,以及中间的间距*/
  column-count:3;
  column-gap:40px;
  /*列中分割线,以及厚度,颜色*/
  column-rule-style:dotted;
  column-rule-width: 1px;
  column-rule-color: lightblue;
  /*指定控件跨越几个列*/
  column-span: all;
  /*列的宽度*/
  column-width: 100px;
  .btn-group button {
      background-color: #04AA6D; /* 绿色背景 */
      border: 1px solid green; /* 绿色边框 */
      color: white; /* 白色文本 */
      padding: 10px 24px; /* 内边距离、 */
      cursor: pointer; /* 指针/手形图标 */
      float: left; /* 并排浮动按钮 */
  }
  ```
  
  ### 动画
  
  ```css
  @keyframes 规则是创建动画。
  
  @keyframes 规则内指定一个 CSS 样式和动画将逐步从目前的样式更改为新的样式。
  ```
  
  
  
  

