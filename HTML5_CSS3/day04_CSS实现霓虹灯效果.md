# 每日CSS一练：霓虹灯按钮

> 注：此部分代码并非本人原创，代码仅用于交流学习，了解CSS样式设计的思想。

## HTML代码

```html
<!-- 非本人编写代码，仅供学习参考 -->
<!-- 原文地址："https://www.cnblogs.com/xiaxiangx/p/14164984.html" -->
<a href="#">
    <span></span>
    <span></span>
    <span></span>
    <span></span>
    霓虹灯按钮
</a>
```

我们使用`a`标签来做按钮，`span`做边框；为了使按钮居中在页面中心，我们使用flex布局；

## 设置基准样式

```scss
$background: #031321;

body {
    margin: 0;
    padding: 0;
    display: flex;
    justify-content: center; // flex横向居中
    align-items: center; // flex纵向居中
    min-height: 100vh;
    background:$background;
}
```

## 修饰`a`标签

```scss
$defaultColor: #2196f3;

a {
    position: relative;
    display: inline-block; //默认为inline元素，不能设置宽高，设置成这个即可设置宽高
    padding: 15px 30px;
    color: $defaultColor;
    letter-spacing: 4px;
    text-decoration: none; // 去除a标签的默认样式
    font-size: 24px;
    font-weight: bold;
    overflow: hidden; // 触发BFC机制
    transition: 0.2s;
}

a:hover {
    color: #255784;
    background: $defaultColor;
    box-shadow: 0 0 10px $defaultColor, 0 0 40px $defaultColor, 0 0 10px $defaultColor, 0 0 80px $defaultColor; // 设置多重阴影，从而达到发光的效果
    transition-delay: 1s;
}
```

## 绘制会动的线条

由于我们的外部容器使用了相对定位，那么为了保证盒子的位置，我们需要使用绝对定位来固定`span`的显示位置；

```scss
a span {
    position: absolute;
    display: block;
}
```

接下来设置线条的样式；

```scss
$defaultColor: #2196f3;

a span:nth-child(1) {
    top: 0;
    left: -100%;
    width: 100%;
    height: 2px;
    background: linear-gradient(90deg, transparent, $defaultColor);
}
```

最有意思的地方来了，怎么让线条动了？只需要将鼠标移到元素上时，将盒子的left由-100%调整为100%，并且加上过渡效果，那么就可以实现会动的发光线了。

```scss
a:hover span:nth-child(1) {
    left: 100%;
    transition: 1s;
}
```

设置完横向的，再说一下纵向的如何设置：

```scss
a span:nth-child(3) {
    bottom: 0;
    left: 100%;
    width: 100%;
    height: 2px;
    background: linear-gradient(270deg, transparent, $defaultColor);
}

a:hover span:nth-child(3) {
    left: -100%;
    transition: 1s .5s;
}
```

## 完整代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>霓虹灯效果</title>
    <style type="text/css">
body {
    margin: 0;
    padding: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    background: #031321;
    font-family: "Microsoft YaHei UI";
}

a {
    position: relative;
    display: inline-block;
    padding: 15px 30px;
    color: #2196f3;
    letter-spacing: 4px;
    text-decoration: none;
    font-size: 24px;
    font-weight: bold;
    overflow: hidden;
    transition: 0.2s;
}

a:hover {
    color: #255784;
    background: #2196f3;
    box-shadow: 0 0 10px #2196f3, 0 0 40px #2196f3, 0 0 10px #2196f3, 0 0 80px #2196f3;
    transition-delay: 1s;
}

a span {
    position: absolute;
    display: block;
}

a span:nth-child(1) {
    top: 0;
    left: -100%;
    width: 100%;
    height: 2px;
    background: linear-gradient(90deg, transparent, #2196f3);
}

a:hover span:nth-child(1) {
    left: 100%;
    transition: 1s;
}

a span:nth-child(3) {
    bottom: 0;
    left: 100%;
    width: 100%;
    height: 2px;
    background: linear-gradient(270deg, transparent, #2196f3);
}

a:hover span:nth-child(3) {
    left: -100%;
    transition: 1s .5s;
}

a span:nth-child(2) {
    right: 0;
    top: -100%;
    width: 2px;
    height: 100%;
    background: linear-gradient(180deg, transparent, #2196f3);
}

a:hover span:nth-child(2) {
    top: 100%;
    transition: 1s .25s;
}

a span:nth-child(4) {
    left: 0;
    top: 100%;
    width: 2px;
    height: 100%;
    background: linear-gradient(360deg, transparent, #2196f3);
}

a:hover span:nth-child(4) {
    top: -100%;
    transition: 1s .75s;
}
    </style>
</head>
<body>
    <a href="#">
        <span></span>
        <span></span>
        <span></span>
        <span></span>
        霓虹灯按钮
    </a>
</body>
</html>
```

## 效果演示

可以访问我的[Code Pen](https://codepen.io/humphrey75432/pen/xxEyKjv)查看示例代码，也可以将完整代码拷贝的自己的html中看效果；

## 原文地址

[1] https://www.cnblogs.com/xiaxiangx/p/14164984.html