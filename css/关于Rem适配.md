# 关于Rem适配

#### 问题描述：

pc端设计图width1920px 

移动端设计图width 750px

如何适配？

#### 方法一

rem是根据html下`font-size`为比例：

```css
/* 设置html元素的字体大小为 16px，即 1rem = 16px */
html {
    font-size: 16px;
}
/* 设置box元素宽 160px，10rem = 160px */
.box {
    width: 10rem; /* 160px */
}
```

