# CSS3 基础回顾

## 简介

全称“层叠样式表”（Cascade Style Sheet），用来修饰HTML元素的外观：包括大小、位置、颜色和形状等等；

## 语法

语法如下：

```css
selector {
  property-type: property-value;
}
```

简单来说：需要对哪个元素做修饰，首先要选中需要修饰的元素，然后再使用CSS对其作修改。

## 选择器

### 基本

比较常用的选择器有：class选择器和id选择器，除此之外还有一些高级的选择器，在日常使用中我们可以通过查阅文档来了解一些更高级的选择器用法。

- class选择器
- id选择器
- nth-child选择器等

一个例子来说明这些选择器的用法：

```css
.className {
  width: 200px;
  height: 150px;
}

#id-name {
  width: 200px;
  height: 150px;
}

.box:nth-child(2n) {
  background: #4f5b62;
}
```

上面列举了三个选择器的语法：可以看到class选择器的语法是`.className`，id选择器的语法是`#idName`；掌握这两个基本就能满足90%以上的应用场景了；

最后还有一个选择器值得关注：它的语义是：选择className为box并且顺序为偶数序的元素。这属于高级选择器中的其中一个，还有其他的一些可以通过上网查阅来了解；

一般在前端开发中会遵循一个编码规范：就是class选择器专门用来修饰样式；id选择器专门用来编写逻辑；

### 嵌套和分组

## 盒子模型

所有的HTML元素都可以看作是盒子，在CSS中，“box model”是用来设计和布局使用的；

一个盒子有下列几部分构成：

- 外边距(margin)：清除边框外的区域，外边距是透明的；
- 边框(border)：围绕在内边距和内容外的边框，这部分是可以看见的；
- 内边距(padding)：清除内容周围的区域，内边距是透明的；
- 内容(content)：盒子的内容，显示文本和图像；

所以你需要知道：给一个元素设置宽高的时候要加上它的外边距、边框、内边距的宽高，这样才是一个元素的宽高；

盒子模型可以通过浏览器的开发者工具看到，如果需要检视页面上的HTML元素，你可以通过查看元素在控制台的信息来查看它的边距情况。

总结一下：最终元素的总宽高计算公式是这样的；

- 总元素的宽度：宽度 + 左右填充 + 左右边框 + 左右边距；
- 总元素的高度：高度 + 上下填充 + 上下边框 + 上下边距；

## 样式设置

### 颜色

可以给字体，盒子设置颜色，下面举一部分例子：

- 盒子背景：`background-color`，背景有一个简写的属性叫`background`;
- 字体颜色：`font-color`，同样地，字体也有一个简写属性叫`font`;
- 边框：边框使用`border`属性也可以给其上色，具体语法可以查阅相关文档；
- 还有一个CSS属性叫`color`，这个是给文本设置颜色的；

### 字体

字体是HTML中的重要显示元素，CSS中字体的设置有下列几个：

- font：all in one属性，可以设置字体的所有属性
- font-family：字体
- font-size：字体大小
- font-style：字体样式，粗体和斜体之类的
- font-variant：以小型大写字体或者正常字体显示文本；
- font-weight：指定字体的粗细；

介绍一下all-on-one属性的语法：

```css
font: "<font-style> <font-variant> <font-weight> <font-size> / <line-height> <font-family>"
```

### All-in-one 属性语法

#### 外边距（margin）

语法：

```css
h1 {
  marign: 15px 30px 15px 30px; // 次序分别为上、右、下、左
}

h1 {
  margin: 15px 30px 15px; // 次序分别为上，左右，下
}

h1 {
  margin: 15px 30px; // 上下，左右
}

h1 {
  margin: 15px; //上下左右全都一样
}
```

#### 填充（padding）

语法：

```css
h1 {
  padding: 15px 30px 15px 30px; // 次序分别为上、右、下、左
}

h1 {
  padidng: 15px 30px 15px; // 次序为上，左右，下
}

h1 {
  padding: 15px; 30px; // 上下，左右
}

h1 {
  padding: 25px; // 上下左右全都一样
}
```



## 显示

## 定位

## 浮动

## 伪类



