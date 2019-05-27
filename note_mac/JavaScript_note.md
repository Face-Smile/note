## JavaScript 使用

#### 1. 脚本放置位置

HTML中脚本必须位于`<script>`与`</script>`标签之间。

脚本可被放置在HTML页面的`<body>`和`<head>`之间。

#### 2. 引用外部的JavaScript

Javascript 文件的扩展名是`.js`。

如需使用外部文件，在 `<script>`标签的`src`属性中设置该.js文件

**example**

```html
<script src="myScript.js"></script>
```

> **tip** : 外部脚本 不能包含`<script>`标签



## JS 语句

- 分号 `;`

分号用于分隔 JavaScript 语句。

通常我们在每条可执行的语句结尾添加分号。

使用分号的另一用处是在一行中编写多条语句。

> **提示**：您也可能看到不带有分号的案例。在 JavaScript 中，用分号来结束语句是可选的。



## JS 注释

- `//` 单行注释

- `/*` `*/`多行注释



## JS 变量

#### 1. `var` 声明变量

```javascript
var carname;
```

#### 2. 变量命名

- 变量必须以字母开头
- 变量也能以$和_符号开头（不过不推荐这么做）
- 变量名称对大小写敏感

#### 3. 数据类型

变量类型根据赋值的类型而定，如果没有赋值就是`undefined`类型 

##### (1)javascript 拥有动态类型

​	相同的变量可用作不同的类型

```javascript
var x;			//x 为undefined
var x = 8;		//x 为数字
var x ="Bill";	//x 为字符串
```



##JS DOM

#### 1. document.write()

- 写入HTML输入流

> **note**: *您只能在HTML输出中使用document.write中使用。如果在文档加载后使用该方法，会覆盖整个文档。*
>
> 用javascript编写的代码只能通过html/xhtml文档才能执行，代码一行一行解析，文档在加载的过程中实际是一边加载一边用document.write写出内容到屏幕上，而加载完成后，document就关闭。如果再调用document.write往网页上写内容的话，就会重写document。

**example**

```
document.write("<h1>This is a heading</h1>");
```

覆写整个文档样例：

```html
<!DOCTYPE html>
<html>
<body>
	<h1>我的第一张网页</h1>
	<p>我的第一个段落。</p>
	<button onclick="myFunction()">点击这里</button>
	<script>
        function myFunction()
        {
            document.write("糟糕！文档消失了。");
        }
	</script>
</body>
</html>
```

#### 2. document.getElementById()

- 通过id来访问HTML元素

##### (1) innerHTML

- 改变HTML元素的内容，讲覆盖标签元素原有的内容

  **example**

  ```javascript
  document.getElementById('id_name').innerHTML="Hello World!";
  ```




## JS 事件反应

### onclick

- 点击事件，执行其中的函数方法

**example**

```html
<button type="button" onclick="alert("welcome")">点击这里</button>
```





