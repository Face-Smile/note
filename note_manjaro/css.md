## CSS 背景属性

| Property                                                     | 描述                                                      |
| ------------------------------------------------------------ | --------------------------------------------------------- |
| [background](https://www.w3cschool.cn/cssref/css3-pr-background.html) | 简写属性，作用是将背景属性设置在一个声明中。              |
| [background-attachment](https://www.w3cschool.cn/cssref/pr-background-attachment.html) | 背景图像是否固定或者随着页面的其余部分滚动。              |
| [background-color](https://www.w3cschool.cn/cssref/pr-background-color.html) | 设置元素的背景颜色。                                      |
| [background-image](https://www.w3cschool.cn/cssref/pr-background-image.html) | 把图像设置为背景。 # background-image: url('imgpath.jpg') |
| [background-position](https://www.w3cschool.cn/cssref/pr-background-position.html) | 设置背景图像的起始位置。                                  |
| [background-repeat](https://www.w3cschool.cn/cssref/pr-background-repeat.html) | 设置背景图像是否及如何重复。                              |

### `backgound-attachment`

|   值    |            说明            |
| :-----: | :------------------------: |
| scroll  | 背景图片随页面其余部分滚动 |
|  fixed  |      背景图像固定不动      |
| inherit |     从父元素继承此属性     |

### `background-positioin`

| <span style="white-space:nowrap;">&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;值&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;</span> | 描述                                                         |
| :----------------------------------------------------------: | :----------------------------------------------------------- |
| `left top`<br> `left center`<br/> `left bottom`<br/> `right top`<br/>` right center`<br/>` right bottom`<br/> `center top `<br/>`center center` <br/>`center bottom` | 如果仅指定一个关键字，其他值将会是"center"                   |
|                           *x% y%*                            | 第一个值是水平位置，第二个值是垂直。左上角是0％0％。右下角是100％100％。如果仅指定了一个值，其他值将是50％。 。默认值为：0％0％ |
|                         *xpos ypos*                          | 第一个值是水平位置，第二个值是垂直。左上角是0。单位可以是像素（0px0px）或任何其他 [CSS单位](https://www.runoob.com/try/css-units.html)。如果仅指定了一个值，其他值将是50％。你可以混合使用％和positions |
|                           inherit                            | 指定background-position属性设置应该从父元素继承              |

### `background-repeat`

| 值        | 说明                                         |
| :-------- | :------------------------------------------- |
| repeat    | 背景图像将向垂直和水平方向重复。这是默认     |
| repeat-x  | 只有水平位置会重复背景图像                   |
| repeat-y  | 只有垂直位置会重复背景图像                   |
| no-repeat | background-image不会重复                     |
| inherit   | 指定background-repea属性设置应该从父元素继承 |

### `background-size`

| 值         | 描述                                                         |
| :--------- | :----------------------------------------------------------- |
| length     | 设置背景图片高度和宽度。第一个值设置宽度，第二个值设置的高度。如果只给出一个值，第二个是设置为 **auto**(自动) |
| percentage | 将计算相对于背景定位区域的百分比。第一个值设置宽度，第二个值设置的高度。如果只给出一个值，第二个是设置为"auto(自动)" |
| cover      | 此时会保持图像的纵横比并将图像缩放成将完全覆盖背景定位区域的最小大小。 |
| contain    | 此时会保持图像的纵横比并将图像缩放成将适合背景定位区域的最大大小。 |



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





## CSS Position

### `static` 定位

HTML元素的默认值,即没有定位,遵循正常的文档流对象.

静态定位的元素不会受到`top`, `bottom`, `left`, `right`影响

### `fixed`定位

元素的位置相对于浏览器窗口是固定位置.

即使窗口是滚动的,它也不会移动.

> **注意：** Fixed 定位在 IE7 和 IE8 下需要描述 !DOCTYPE 才能支持。
>
> Fixed定位使元素的位置与文档流无关，因此不占据空间。
>
> Fixed定位的元素和其他元素重叠。
>

### `relative`定位

相对定位的元素的定位是相对于其正常定位.

移动相对定位元素,但他原本所占用的空间不会改变.

相对定位元素经常被用来作为绝地定位元素的容器块.

### `absolute`定位

绝对定位的元素位置相对于最近的已定位父元素,如果元素没有已定位的父元素,那么它的位置相对于`<html>`

### `sticky`定位

有点类似于Excel的固定表头

sticky 英文字面意思是粘，粘贴，所以可以把它称之为粘性定位。

**position: sticky;** 基于用户的滚动位置来定位。

粘性定位的元素是依赖于用户的滚动，在 **position:relative** 与 **position:fixed** 定位之间切换。

它的行为就像 **position:relative;** 而当页面滚动超出目标区域时，它的表现就像 **position:fixed;**，它会固定在目标位置。

元素定位表现为在跨越特定阈值前为相对定位，之后为固定定位。

这个特定阈值指的是 top, right, bottom 或 left 之一，换言之，指定 top, right, bottom 或 left 四个阈值其中之一，才可使粘性定位生效。否则其行为与相对定位相同。

**注意:** Internet Explorer, Edge 15 及更早 IE 版本不支持 sticky 定位。 Safari 需要使用 -webkit- prefix (查看以下实例)。

### 重叠的元素

元素的定位与文档流无关，所以它们可以覆盖页面上的其它元素

z-index属性指定了一个元素的堆叠顺序（哪个元素应该放在前面，或后面）

一个元素可以有正数或负数的堆叠顺序

### 所有的CSS定位属性

"CSS" 列中的数字表示哪个CSS(CSS1 或者CSS2)版本定义了该属性。 

| 属性                                                         | 说明                                                         | 值                                                           | CSS  |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- | :--- |
| [bottom](https://www.runoob.com/cssref/pr-pos-bottom.html)   | 定义了定位元素下外边距边界与其包含块下边界之间的偏移。       | auto *length%*inherit                                        | 2    |
| [clip](https://www.runoob.com/cssref/pr-pos-clip.html)       | 剪辑一个绝对定位的元素                                       | *shape*auto inherit                                          | 2    |
| [cursor](https://www.runoob.com/cssref/pr-class-cursor.html) | 显示光标移动到指定的类型                                     | *url* auto crosshair default pointer move e-resize ne-resize nw-resize n-resize se-resize sw-resize s-resize w-resize text wait help | 2    |
| [left](https://www.runoob.com/cssref/pr-pos-left.html)       | 定义了定位元素左外边距边界与其包含块左边界之间的偏移。       | auto *length%*inherit                                        | 2    |
| [overflow](https://www.runoob.com/cssref/pr-pos-overflow.html) | 设置当元素的内容溢出其区域时发生的事情。                     | auto hidden scroll visible inherit                           | 2    |
| [overflow-y](https://www.runoob.com/css/cssref/css3-pr-overflow-y.html) | 指定如何处理顶部/底部边缘的内容溢出元素的内容区域            | auto hidden scroll visible no-display no-content             | 2    |
| [overflow-x](https://www.runoob.com/css/cssref//cssref/css3-pr-overflow-x.html) | 指定如何处理右边/左边边缘的内容溢出元素的内容区域            | auto hidden scroll visible no-display no-content             | 2    |
| [position](https://www.runoob.com/cssref/pr-class-position.html) | 指定元素的定位类型                                           | absolute fixed relative static inherit                       | 2    |
| [right](https://www.runoob.com/cssref/pr-pos-right.html)     | 定义了定位元素右外边距边界与其包含块右边界之间的偏移。       | auto *length%*inherit                                        | 2    |
| [top](https://www.runoob.com/cssref/pr-pos-top.html)         | 定义了一个定位元素的上外边距边界与其包含块上边界之间的偏移。 | auto *length%*inherit                                        | 2    |
| [z-index](https://www.runoob.com/cssref/pr-pos-z-index.html) | 设置元素的堆叠顺序                                           | *number*auto inherit                                         | 2    |



## 鼠标属性设置

```html
<p>请把鼠标移动到单词上，可以看到鼠标指针发生变化：</p>
<span style="cursor:auto">auto</span><br>
<span style="cursor:crosshair">crosshair</span><br>
<span style="cursor:default">default</span><br>
<span style="cursor:e-resize">e-resize</span><br>
<span style="cursor:help">help</span><br>
<span style="cursor:move">move</span><br>
<span style="cursor:n-resize">n-resize</span><br>
<span style="cursor:ne-resize">ne-resize</span><br>
<span style="cursor:nw-resize">nw-resize</span><br>
<span style="cursor:pointer">pointer</span><br>
<span style="cursor:progress">progress</span><br>
<span style="cursor:s-resize">s-resize</span><br>
<span style="cursor:se-resize">se-resize</span><br>
<span style="cursor:sw-resize">sw-resize</span><br>
<span style="cursor:text">text</span><br>
<span style="cursor:w-resize">w-resize</span><br>
<span style="cursor:wait">wait</span><br>
```

