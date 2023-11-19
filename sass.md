> 总结一些常用的使用方法，[详细请参考官方中文文档](https://www.sasscss.com/documentation/syntax)

## 简介

> Sass (英文全称：Syntactically Awesome Stylesheets) 是一个最初由 Hampton Catlin 设计并由 Natalie Weizenbaum 开发的层叠样式表语言。
>
> Sass 是一个 CSS 预处理器。
>
> Sass 是 CSS 扩展语言，可以帮助我们减少 CSS 重复的代码，节省开发时间。
>
> Sass 完全兼容所有版本的 CSS。
>
> Sass 扩展了 CSS3，增加了规则、变量、混入、选择器、继承、内置函数等等特性。
>
> Sass 生成良好格式化的 CSS 代码，易于组织和维护。
>
> Sass 文件后缀为 **.scss**



# Sass 变量

变量用于存储一些信息，它可以重复使用。

Sass 变量可以存储以下信息：

- 字符串
- 数字
- 颜色值
- 布尔值
- 列表
- null 值

Sass 变量使用 **$** 符号：



```scss
// 因为scss可以嵌套，自然也就会有变量的作用域啦
// 这个作用域和其他编辑语言相同

$mycolor: red;
div{
    $mycolor: pink;
    background: $mycolor;
}
h1{
    color: $mycolor;
}

// ----------------------- 转换成Css后
div{
    background: pink;
}
h1{
    color: red;
}
```





# Sass 嵌套规则与属性

```scss
//实例中，ul, li, 和 a 选择器都嵌套在 nav 选择器中
nav {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }
  li {
    display: inline-block;
  }
  a {
    display: block;
    padding: 6px 12px;
    text-decoration: none;
  }
}

//转换成Css后
nav ul {
  margin: 0;
  padding: 0;
  list-style: none;
}
nav li {
  display: inline-block;
}
nav a {
  display: block;
  padding: 6px 12px;
  text-decoration: none;
}
```

## Sass 嵌套属性

> 很多 CSS 属性都有同样的前缀，例如：font-family, font-size 和 font-weight ， text-align, text-transform 和 text-overflow。
>
> 所以为了方便scss也可以将属性嵌套 nice

```scss
font: {
  family: Helvetica, sans-serif;
  size: 18px;
  weight: bold;
}

text: {
  align: center;
  transform: lowercase;
  overflow: hidden;
}

// 转换成CSS后
font-family: Helvetica, sans-serif;
font-size: 18px;
font-weight: bold;

text-align: center;
text-transform: lowercase;
text-overflow: hidden;
```



# Sass @extend 与 继承

> @extend 指令告诉 Sass 一个选择器的样式从另一选择器继承。
>
> 如果一个样式与另外一个样式几乎相同，只有少量的区别，则使用 @extend 就显得很有用。
>
> 以下 Sass 实例中，我们创建了一个基本的按钮样式 .button-basic，接着我们定义了两个按钮样式 .button-report 与 .button-submit，它们都继承了 .button-basic ，它们主要区别在于背景颜色与字体颜色，其他的样式都是一样的。
>
> 说白了就是和函数一样，可以减少重复代码

```scss
.button-basic  {
  border: none;
  padding: 15px 30px;
  text-align: center;
  font-size: 16px;
  cursor: pointer;
}

// 通过@extend 来继承 .button-basic中的属性
.button-report  {
  @extend .button-basic;
  background-color: red;
}

.button-submit  {
  @extend .button-basic;
  background-color: green;
  color: white;
}

// 转换为CSS后
.button-basic, .button-report, .button-submit {
  border: none;
  padding: 15px 30px;
  text-align: center;
  font-size: 16px;
  cursor: pointer;
}

.button-report  {
  background-color: red;
}

.button-submit  {
  background-color: green;
  color: white;
}
```

