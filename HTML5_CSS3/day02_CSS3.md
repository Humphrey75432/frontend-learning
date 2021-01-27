# CSS3 进阶操作

## CSS3 边框

在CSS3中你可以创建圆角边框、阴影边框。属性如下：

- border-radius：边框圆角
- box-shadow：盒子阴影
- border-image：边框图片

## CSS3 圆角

举个例子：

```css
#cornenr1 {
  border-radius: 25px;
  background: #a8bcf4;
  padding: 20px;
  width: 200px;
  height: 150px;
}

#corner2 {
  border-radius:15px 50px 30px 5px;
  background: #fe5cb9;
  padding: 10px;
  width: 150px;
  height: 100px;
}
```

可以发现第二个例子有四个值：顺序为：左上、右上、右下和左下；

一个值就是默认四个角都是一样的，没什么毛病。

初次之外还有其他属性，可以自己熟悉一下：

- border-top-left-radius
- border-top-right-radius
- border-bottom-left-radius
- border-bottom-right-radis

## CSS3 背景

CSS3中新增几个背景属性，提供了更大背景元素控制；

- background-image：添加背景图片，不同背景用逗号分隔开；
- background-size：指定背景图像的大小；
- background-origin：指定背景图像的位置区域，有：border-box, padding-box和content-box;
- background-clip：从指定位置开始绘制背景图形；

## CSS3 渐变

可以在两个或者多个指定的颜色之间显示平稳的过渡，CSS3中定义了两种类型的渐变（gradients）：

- 线性渐变：向上下左右对角方向
- 径向渐变：由中心定义

语法如下：

```css
background-image: linear-gradient(direction, color-stop1, color-stop2, ...)

/* 
  direction: 默认上下，左右为to right
  对角渐变：to bottom right
  还可以使用角度：angle (这里指的是弧度)
*/
```

重复的线性渐变

```css
background-image: repeating-linear-gradient(red, yellow 10%, green 20%);
```

径向渐变

```css
background-image: radial-gradient(shape size at position, start-color, ... ,last-color);
```

shape参数定义了形状。可以是circle和ellipse，圆形和椭圆形；

和重复地线性渐变一样，径向渐变也有重复地属性，这里就不再演示了。

## CSS3 文本效果 & 字体

文本效果包括下面的这几个：

- text-shadow：文本阴影，可以定义多个，可以做字体发光效果
- box-shadow：盒子阴影，可以定义多个，做成发光盒子
- text-overflow：指定用户如何显示溢出内容；
- word-wrap：自动换行属性允许强制文本换行，这意味着分裂它中间的一个字；
- word-break：单词拆分换行属性指定换行规则；

需要使用自定义字体可以使用`@font-face`属性。例子如下：

```css
@font-face {
  font-family: myFirstFont;
  src url(font.ttf);
}

div {
  font-family: myFirstFont;
}
```

## CSS3 2D & 3D转换

### 2D转换

2D变换方法有下面5种：

- translate：根据x轴和y轴位置给定的参数，从当前元素位置移动；
- rotate：在一个给定读书顺时针旋转元素，负值代表逆时针旋转；
- scale：该元素增加或者减少大小，取决于宽度（x轴）和高度（y轴）的参数；
- skew：表示x轴和y轴倾斜的角度，如果第二个参数为空，则默认为0，参数为负数表示像反方向倾斜；
- matrix：和2D变换方法合并成一个，一共有六个参数，包括：旋转、缩放、移动和倾斜的功能；

具体语法可以再查阅

### 3D转换

3D转换会使用到两个方法：

- rotateX：围绕其在一个给定度数X轴旋转的元素；
- rotateY：围绕其在一个给定读书Y轴旋转的元素；

你也可以使用transform属性做2D和3D变换，一般来说建议使用transform属性，特别是在使用一些库来实现高级动画效果的时候；

```css
div {
    transform: none | transform-functions;
}
/*
transform-functions包括：
matrix, matrix3d;
translate, translate3d, translateX, translateY, translateZ;
scale, scale3d, scaleX, scaleY, scaleZ;
rotate, rotate3d, rotateX, rotateY, rotateX;
skew, skewX, skewY;
perspective：为3D转换元素定义透视视图；
*/
```

## CSS3 过渡

CSS3过渡是元素从一种样式逐渐改变为另一种效果，要实现这一点，必须规定两项内容；

- 指定要添加效果的CSS属性；
- 指定效果的持续时间；

例子如下：

```css
div {
    traisition: width 2s;
    -webkit-transition: width 2s; /* Safari or Chrome */
}

/* 要添加多个样式的变换效果，可以使用逗号分隔属性 */
div {
    traisition: width 2s, height 2s, transform 2s;
    -webkit-transform: width 2s, height 2s, transform 2s;
}
```

## CSS3 动画

CSS3可以创建动画，可以取代一些JavaScript实现的动画效果；

CSS3使用`@keyframes`创建动画，语法如下：

```css
@keyframes myfirst {
    from { background: red; }
    to { background: yellow; }
}

@-webkit-keyframes myfirst {
    from { background: red; }
    to { background: yellow; }
}

div {
    animation: myfirst 5s;
}

/* 除此之外还可以这样定义 */
@keyframes myfirst {
    0% { background: red; }
    25% { background: yellow; }
    50% { background: blue; }
    100% { background: green; }
}

@-webkit-keyframes myfirst {
    0% { background: red; }
    25% { background: yellow; }
    50% { background: blue; }
    100% { background: green; }
}

div {
    animation: myfirst 5s;
}
```

## CSS3布局

## CSS3 Flex布局