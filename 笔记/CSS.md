# CSS

### 选择器

##### 伪类选择器

a:link   未点击时

a:visited   点击之后

a:hover   鼠标移入

a:acitve   鼠标点击时

##### :focus选择器

:focus 用于表单获取交单

##### 后代选择器

.nav a 用来选择后代元素 用**空格**分开

##### 子代选择器

.nav>a 用来选择最近一级的元素 用**大于**分开

##### 并集选择器

.nav,.header 用来选择某些相同的样式元素 用**逗号**区别



### 显示模式

##### 常见块元素 

独占一行

宽度是默认父元素的100%

是一个容器及盒子，里面可以放行内或者块级元素

文字类的元素内不能放块级元素  <h1>  <p>

{<h1>  <div>  <ul> <p> <ol> <li>  <div> }最典型！！！

##### 常见的行内元素

相邻行内元素在一行上，一行可以显示多个

高宽直接设置无效

默认宽度是它本身内容的宽度

行内元素只能容纳文本或者其他行内元素

{<span> <a> <strong> <b> <em> <i><del><s><ins><u>}

##### 行内块元素

相邻行内元素(行内块)在一行上，但是他们之间会有白色空隙，一行可以显示多个

默认宽度就是他的内容的本身宽度

宽高是可以自己设置（块元素的特点）

{<img> <input> <td>}

##### 显示模式的转换

转换成块元素 display:block

块元素转换成行内块元素 display:inline

转换为行内块：display:inline-block

### 背景background

##### 背景颜色

background-color:transparent(透明)/color;

##### 背景图片

background-image:none|url();

##### 背景平铺

background-repeat:repeat/no-repeat/repeat-x/repeat-y

##### 背景图片的位置

参数是方名词

center right left bottom top 

第二个参数默认垂直居中

精确单位

第一个是参数是x px 第二个单位 y px

background-position:x y;

##### 背景图像固定

background-attchment: scrool | fixed

##### 背景的复合写法

background:背景颜色 背景图片地址 背景平铺 背景图像滚动 背景图片位置

background：transparent  url(img.jpg)  repeat-y  fixed  top  ;

##### 背景色半透明

background：rgba(0,0,0,0.3)

### 选择器的优先级

!important>id选择器>类选择器/伪类选择器>复合选择器>标签选择器>通配符选择器

计算权重叠加！

### 盒子模型

##### border 边框

border-width/边框宽度

border-style：none solid dash/边框样式

border-color/ 边框颜色

border:1px solid red;（没有顺序）

层叠样式：就近原则

##### content 内容







##### padding 内边距

##### margin 外边距

