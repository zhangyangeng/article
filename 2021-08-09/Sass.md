# 介绍

- 本文主要记录在学习 Sass 课程时的一些笔记

# 一、基础内容

## 1.文档

- 官方 [文档](https://sass-lang.com/)
- 中文 [文档1](https://sass.bootcss.com/)
- 中文 [文档2](https://sass-china.com/)
- 中文 [文档3](https://www.sass.hk/)

## 2.介绍

- Sass 是一种 CSS 的预编译语言。
- 它提供了 [变量（variables）](https://sass.bootcss.com/documentation/variables)、[嵌套（nested rules）](https://sass.bootcss.com/documentation/style-rules#nesting)、 [混合（mixins）](https://sass.bootcss.com/documentation/at-rules/mixin)、 [函数（functions）](https://sass.bootcss.com/documentation/modules)等功能，并且完全兼容 CSS 语法
- Sass 能够帮助复杂的样式表更有条理，并且易于在项目内部或跨项目共享设计

## 3.安装编译

- 如果电脑中有 Node.JS，那么可以通过以下命令来安装

```
npm install -g sass
```

- 安装好以后可以使用该命令进行编译
```
sass input.scss output.css
sass app/sass:public/stylesheets	// 使用文件夹路径作为输入输出
```

## 4.变量

- sass 中使用 `$` 来定义变量，变量中可以存储字符串、数字、颜色值、布尔值、列表、null值
- 变量的作用于只在当前层级上有效果，但我们也可以使用 `!global` 来设置变量为全局的
- 在 sass 中变量名可以使用中划线分隔也可以使用下划线分隔，两个通用
- <strong style="color:red;">注意：所有的全局变量我们一般定义在同一个文件，如：`_globals.scss`，然后我们使用 `@include` 来包含该文件</strong>

## 5.嵌套

### (1).嵌套规则

- Sass 嵌套 CSS 选择器类似于 HTML 的嵌套规则，如下：
```scss
$width: 100px;
$heigth: 200px;
ul{
    width: $width;
    height: $heigth;
    li{
        width: $heigth;
        height: $width;
    }
}
```

### (2).&标识符

- 当给父元素设置伪类时，sass 会以后代选择器的方式进行拼接，这样时不对的，所以我们可以在内部使用 `&` 来表示父选择器
```scss
article a{
    color: blue;
    &:hover{
        color: red;
    }
}
```

### (3).嵌套属性

- 很多 CSS 属性都有同样的前缀，例如：`font-family, font-size 和 font-weight ， text-align, text-transform 和 text-overflow`
- 在 Sass 中，我们可以使用嵌套属性来编写它们，如下：
```scss
ul{
    // 嵌套属性
    font: {
        family: Helvetica, sans-serif;
        size: 18px;
        weight: bold;
    }
}
```

# 二、@规则

## 1.@import

- sass 使用 `@import` 语法来导入文件
  - 使用 css 导入时会创建一个额外的 HTTP 请求
  - 但是使用 sass 导入式不需要额外的 HTTP 请求
- 语法：`@import filename`

### Sass Partials

- 如果不想让某个 sass 文件编译为 css 文件，可以在该文件的开头加一个下划线
- 语法：`_filename` 
- 导入该文件时不需要使用下划线
- <strong style="color:red;">注意：请不要将带下划线与不带下划线的同名文件放置在同一个目录下，比如 _colors.scss 和 colors.scss 不能同时存在于同一个目录下，否则带下划线的文件将会被忽略</strong>

## 2.@mixin

- 混入：允许我们定义一个可以在整个样式表中重复使用的样式
- 语法：
```scss
@mixin important-text{
    color: red;
    font-size: 25px;
    font-weight: bold;
    border: 1px solid blue;
}
```

- 混入中也可以包含混入
- 混入中可以传入参数（同时可以设置默认参数）：
```scss
@mixin bordered($color,$width) {
    border: $width solid $color;
}
.myArticle{
    @include bordered(red, 10px);
}
````

- 混入中也可以传入可变参数，即不能确定使用多少个参数时，可以使用 `...` 来设置可变参数
```scss
@mixin box-shadow($shadows...) {
    -moz-box-shadow: $shadows;
    -webkit-box-shadow: $shadows;
    box-shadow: $shadows;
}
.shadows{
    @include box-shadow(0px 4px 5px #666, 2px 6px 10px #999);
}
```

- 混入也可以使用在浏览器前缀上
```scss
@mixin transform($property) {
    -webkit-transform: $property;
    -ms-transform: $property;
    transform: $property;
}
.myBox{
    @include transform(rotate(20deg));
}
```

## 3.@include

- 使用该指令可以使用上面定义好的混入
- 语法；
```scss
@mixin important-text{
    color: red;
    font-size: 25px;
    font-weight: bold;
    border: 1px solid blue;
}
.danger{
    @include important-text;
    background-color: green;
}
```

## 4.@extend

- 继承：可以使改选择器的样式从另一个选择器继承
- 语法：
```scss
.button-basic{
    border: none;
    padding: 15px 30px;
    text-align: center;
    font-size: 16px;
    cursor: pointer;
}
.button-report{
    @extend .button-basic;
    background-color: red;
}
.button-submit{
    @extend .button-basic;
    background-color: green;
    color: #fff;
}
```

# 三、函数

- https://www.runoob.com/sass/sass-functions.html

## 1.字符串

- 

## 2.数字

- 

## 3.列表

- 

## 4.映射

- 

## 5.选择器

- 

## 6.Introspection

- 

## 7.颜色

- 

# 二、语法

## 1.SCSS语法

- SCSS 语法使用 `.scss` 文件扩展名，格式中使用花括号来表示：
```scss
// scss样式
$font-stack: Helvetica, sans-serif;
$primary-color: #333;
body{
    font: 100% $font-stack;
    color: $primary-color;
}
```

## 2.SASS语法

- SASS 语法即缩进语法是原始语法，使用 `.sass` 文件扩展名，格式中没有括号和分号：
```scss
// sass样式
$font-stack: Helvetica, sans-serif
$primary-color: #333
body
    font: 100% $font-stack
    color: $primary-color
```

## 3.解析

- Sass 样式表是经由 Unicode 编码序列解析而来的。 解析是直接进行的，没有转换为标记流（token stream）的过程
- Sass 执行解码的流程 如下：
  - 如果字节序列以 U+FEFF 字节顺序开头，则表示是 UTF-8 或 UTF-16 编码，然后 使用对应的编码
  - 如果字节序列以纯 ASCII 字符串 `@charset` 开头，那么 Sass 使用 CSS 规则的第 2 步来确定编码，最终 [determining the fallback encoding](https://drafts.csswg.org/css-syntax-3/#input-byte-stream)
  - 否则，使用UTF-8

## 4.注释

### (1).单行注释

- 使用 `//` 来指定，被称为 **静默注释**，因为不生成 CSS 语句

### (2).多行注释

- 使用 `/* */` 来指定，多行注释会被编译为 CSS 注释，所以被称为 **显示注释**

### (3).文档注释

- 文档注释是静默注释，用三个斜杠（`///`）直接写在文档的正上方

## 5.函数

### (1).url()

- 可以接受带引号的 url，也可以不带引号的 url
- 如果`url()`的参数是一个有效的未引用URL，那么Sass将按原样解析它，不过也可以使用插值注入SassScript值
- 如果它不是一个有效的未引用URL（如果它包含变量或函数调用）它将被解析为一个原生的CSS函数调用

### calc()/element()

- `calc()`的数学表达式与Sass的算法冲突，而且`element()`的id可以被解析为颜色，所以它们需要特殊的解析
```scss
$width: 800px;
left: calc(50% - #{$width / 2});
```

### min()/max()

- 

# 三、样式

## 1.嵌套

- 在 sass 中的样式可以嵌套使用
```scss
$width: 100px;
$heigth: 200px;
ul{
    width: $width;
    height: $heigth;
    li{
        width: $heigth;
        height: $width;
    }
}
```

## 2.插值

- 可以使用插值将变量和函数调用等表达式中的值注入选择器
