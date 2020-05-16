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



## JS 操作元素方法

可以使用内置对象document上的getElementById方法来获取页面上设置了id属性的元素，获取到的是一个html对象，然后将它赋值给一个变量，比如：

```
<script type="text/javascript">
    var oDiv = document.getElementById('div1');
</script>
....
<div id="div1">这是一个div元素</div>
```

上面的语句，如果把javascript写在元素的上面，就会出错，因为页面上从上往下加载执行的，javascript去页面上获取元素div1的时候，元素div1还没有加载，解决方法有两种：

第一种方法：将javascript放到页面最下边

```
....
<div id="div1">这是一个div元素</div>
....

<script type="text/javascript">
    var oDiv = document.getElementById('div1');
</script>
</body>
```

第二种方法：将javascript语句放到window.onload触发的函数里面,获取元素的语句会在页面加载完后才执行，就不会出错了。

```
<script type="text/javascript">
    window.onload = function(){
        var oDiv = document.getElementById('div1');
    }
</script>

....

<div id="div1">这是一个div元素</div>
```



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



## JS 输出

- alert 弹窗输出,显示信息

  ```javascript
  <script>
      alert("弹出窗口");
  </script>
  ```

> 不点击确定,JavaScript不会往后继续执行

- console 控制台输出,一般用来调试程序

```javascript
<script>
	console.log("控制台日志输出");
	console.warn("警告输出");
	console.error("错误输出");
</script>
```

- prompt 网页弹框输出,一般用于接受用户输入数据

```javascript
<script>
	prompt("Hello, JavaScript!");
</script>
```

- confirm 网页弹出提示框,显示信息 ,该方法一般与if判断语句结合使用

```javascript
<script>
	confirm("弹窗确认!");
</script>
```



## JS 数据类型

#### Nmber

#### String

#### Array

```javascript
var cars = ["saab", "volvo", "BMW"];
```



#### Object

```javascript
var person = {firstName : "John", lastName:"Doe"};
```





- typeof 查看变量数据类型

```javascript
<script>
    var num = 1;
    console.log(typeof num);
</script>
```

```
output: 
number
```







## JS DOM

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



## JS 封闭函数

封闭函数是javascript中匿名函数的另外一种写法，创建一个一开始就执行而不用命名的函数。

一般定义的函数和执行函数：

```
function myalert(){
    alert('hello!');
};

myalert();
```

封闭函数：

```
(function myalert(){
    alert('hello!');
})();
```

还可以在函数定义前加上“~”和“!”等符号来定义匿名函数

```
!function myalert(){
    alert('hello!');
}()
```

**封闭函数的好处**
封闭函数可以创造一个独立的空间，在封闭函数内定义的变量和函数不会影响外部同名的函数和变量，可以避免命名冲突，在页面上引入多个js文件时，用这种方式添加js文件比较安全，比如：

```
var iNum01 = 12;
function myalert(){
    alert('hello!');
}
(function(){
    var iNum01 = 24;
    function myalert(){
        alert('hello!world');
    }
    alert(iNum01);
    myalert()
})()
alert(iNum01);
myalert();
```

> 有时候会在封闭函数前加一个`;`，是为了防止压缩js文件时，出现某些没加封号结尾的其他js语句和它并在一起出现语法错误。








## JS 事件反应

### onclick

- 点击事件，执行其中的函数方法

**example**

```html
<button type="button" onclick="alert("welcome")">点击这里</button>
```





## JS表单

onsubmit():表单验证





## JS 正则表达式

##### RegExp对象

正则表达式是描述字符模式的对象。

正则表达式用于对字符串模式匹配及检索替换，是对字符串执行模式匹配的强大工具。



#### 语法

```javascript
var patt = new RegExp(pattern, modifiers);
```

或者更简单的方式:

```javascript
var patt=/pattern/modifiers;
```

> **注意：**当使用构造函数创造正则对象时，需要常规的字符转义规则（在前面加反斜杠 \）。比如，以下是等价的：
>
> ```javascript
> var re = new RegExp("\\w+");
> var re = /\w+/;
> ```



#### 修饰符

修饰符用于执行区分大小写和全局匹配:

| 修饰符 | 描述                                                     |
| ------ | -------------------------------------------------------- |
| i      | 执行对大小写不敏感的匹配。                               |
| g      | 执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）。 |
| m      | 执行多行匹配。                                           |



#### 匹配不以str字符开头的字符串

```
^(?!str)
```



#### 匹配不以str结尾的字符串

```
(?<!str)$
```





## JS常用内置对象

1、document

```
document.getElementById //通过id获取元素
document.getElementsByTagName //通过标签名获取元素
document.referrer  //获取上一个跳转页面的地址(需要服务器环境)
```

2、location

```
window.location.href  //获取或者重定url地址
window.location.search //获取地址参数部分
window.location.hash //获取页面锚点或者叫哈希值
```

3、Math

```
Math.random 获取0-1的随机数
Math.floor 向下取整
Math.ceil 向上取整
```



