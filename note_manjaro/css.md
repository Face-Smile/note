## CSS 背景属性

| Property                                                     | 描述                                         |
| ------------------------------------------------------------ | -------------------------------------------- |
| [background](https://www.w3cschool.cn/cssref/css3-pr-background.html) | 简写属性，作用是将背景属性设置在一个声明中。 |
| [background-attachment](https://www.w3cschool.cn/cssref/pr-background-attachment.html) | 背景图像是否固定或者随着页面的其余部分滚动。 |
| [background-color](https://www.w3cschool.cn/cssref/pr-background-color.html) | 设置元素的背景颜色。                         |
| [background-image](https://www.w3cschool.cn/cssref/pr-background-image.html) | 把图像设置为背景。                           |
| [background-position](https://www.w3cschool.cn/cssref/pr-background-position.html) | 设置背景图像的起始位置。                     |
| [background-repeat](https://www.w3cschool.cn/cssref/pr-background-repeat.html) | 设置背景图像是否及如何重复。                 |



## 文本的对齐方式

文本排列属性是用来设置文本的水平对齐方式。

文本可居中或对齐到左或右,两端对齐.

当text-align设置为"justify"，每一行被展开为宽度相等，左，右外边距是对齐（如杂志和报纸）。



## 文本修饰

text-decoration 属性用来设置或删除文本的装饰。

从设计的角度看 text-decoration属性主要是用来删除链接的下划线：

h1 {text-decoration:overline;} 
h2 {text-decoration:line-through;} 
h3 {text-decoration:underline;}



## 文本缩进

文本缩进属性是用来指定文本的第一行的缩进。

CSS 提供了 text-indent 属性，该属性可以方便地实现文本缩进。

通过使用 text-indent 属性，所有元素的第一行都可以缩进一个给定的长度。

p {text-indent:50px;}



## 文本间隔

word-spacing 属性可以改变字（单词）之间的标准间隔。其默认值 normal 与设置值为 0 是一样的。



## 所有CSS文本属性。

| 属性                                                         | 描述                     |
| ------------------------------------------------------------ | ------------------------ |
| [color](https://www.w3cschool.cn/cssref/pr-text-color.html)  | 设置文本颜色             |
| [direction](https://www.w3cschool.cn/cssref/pr-text-direction.html) | 设置文本方向。           |
| [letter-spacing](https://www.w3cschool.cn/cssref/pr-text-letter-spacing.html) | 设置字符间距             |
| [line-height](https://www.w3cschool.cn/cssref/pr-dim-line-height.html) | 设置行高                 |
| [text-align](https://www.w3cschool.cn/cssref/pr-text-text-align.html) | 对齐元素中的文本         |
| [text-decoration](https://www.w3cschool.cn/cssref/pr-text-text-decoration.html) | 向文本添加修饰           |
| [text-indent](https://www.w3cschool.cn/cssref/pr-text-text-indent.html) | 缩进元素中文本的首行     |
| [text-shadow](https://www.w3cschool.cn/cssref/css3-pr-text-shadow.html) | 设置文本阴影             |
| [text-transform](https://www.w3cschool.cn/cssref/pr-text-text-transform.html) | 控制元素中的字母         |
| [unicode-bidi](https://www.w3cschool.cn/cssref/pr-text-unicode-bidi.html) | 设置或返回文本是否被重写 |
| [vertical-align](https://www.w3cschool.cn/cssref/pr-pos-vertical-align.html) | 设置元素的垂直对齐       |
| [white-space](https://www.w3cschool.cn/cssref/pr-text-white-space.html) | 设置元素中空白的处理方式 |
| [word-spacing](https://www.w3cschool.cn/cssref/pr-text-word-spacing.html) | 设置字间距               |







## CSS Display(显示) 与 Visibility（可见性）

CSS display 属性和 visibility属性都可以用来隐藏某个元素，但是这两个属性有不同的定义，请详细阅读以下内容。



## 隐藏元素 - display:none或visibility:hidden

隐藏一个元素可以通过把display属性设置为"none"，或把visibility属性设置为"hidden"。但是请注意，这两种方法会产生不同的结果。

visibility:hidden可以隐藏某个元素，但隐藏的元素仍需占用与未隐藏之前一样的空间。也就是说，该元素虽然被隐藏了，但仍然会影响布局。

display:none可以隐藏某个元素，且隐藏的元素不会占用任何空间。也就是说，该元素不但被隐藏了，而且该元素原本占用的空间也会从页面布局中消失。



## CSS Display - 块和内联元素

块元素是一个元素，占用了全部宽度，在前后都是换行符。

内联元素只需要必要的宽度，不强制换行。





## CSS Outline



- 取消按钮点击时的蓝色边框

  ```css
  button{
      outline: none;
  }
  ```





## CSS a

- a:link ：是未被访问的样式，可以在里面加很多东西，比如说去掉下划线，换颜色等功能都能在这里实现；
- a:visited ：是已被点击后的样式，也可以在里面加很多元素，可以去下划线，改颜色，放大等功能；
- a:hover ：这个是鼠标悬停的样式，这个等下有实例介绍，我们先来认识一下，是把鼠标停在超链接的位置的时候可以设置变颜色；
- a:active ：这个说是已被激活的样式，简单得说就是能把鼠标点上去的时候，瞬间出的样式，在很多网站上都有这种样式的；

>  提示：在  CSS 定义中，a:hover 必须被置于 a:link 和 a:visited 之后，才是有效的。
>
>  提示：在  CSS 定义中，a:active 必须被置于 a:hover 之后，才是有效的。