# 表格

1、`<table>`标签：声明一个表格，它的常用属性如下：

- border属性 定义表格的边框，设置值是数值
- cellpadding属性 定义单元格内容与边框的距离，设置值是数值
- cellspacing属性 定义单元格与单元格之间的距离，设置值是数值
- align属性 设置整体表格相对于浏览器窗口的水平对齐方式,设置值有：left | center | right

2、`<tr>`标签：定义表格中的一行

3、`<td>`和`<th>`标签：定义一行中的一个单元格，td代表普通单元格，th表示表头单元格，它们的常用属性如下：

- align 设置单元格中内容的水平对齐方式,设置值有：left | center | right
- valign 设置单元格中内容的垂直对齐方式 top | middle | bottom
- colspan 设置单元格水平合并，设置值是数值
- rowspan 设置单元格垂直合并，设置值是数值



td的colspan跨列是所有上下行相对比才能体现的，并不是colspan是几就一定占其它格几倍的宽度。
比如所有行的某一列都设置colspan="2"，上下行没有对比，就和colspan="1"没区别。

```html
<!DOCTYPE html>
<html lang="zh">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        tr {
            text-align: center;
        }
    </style>
</head>

<body>
    <table border="1px" width="650" align="center">
        <tr>
            <th colspan="13" width="100%">基本情况</th>
        </tr>
        <tr>
            <td colspan="2" width="100xp">姓名</td>
            <td colspan="3" width="150xp"></td>
            <td colspan="2" width="100xp">性别</td>
            <td colspan="3" width="150xp"></td>
            <td colspan="3" rowspan="5" width="150xp"></td>
        </tr>
        <tr>
            <td colspan="2">民族</td>
            <td colspan="3"></td>
            <td colspan="2">出生日期</td>
            <td colspan="3"></td>
        </tr>
        <tr>
            <td colspan="2">政治面貌</td>
            <td colspan="3"></td>
            <td colspan="2">健康状况</td>
            <td colspan="3"></td>
        </tr>
        <tr>
            <td colspan="2">籍贯</td>
            <td colspan="3"></td>
            <td colspan="2">学历</td>
            <td colspan="3"></td>
        </tr>
        <tr>
            <td colspan="2">电子信箱</td>
            <td colspan="3"></td>
            <td colspan="2">联系电话</td>
            <td colspan="3"></td>
        </tr>
    </table>
</body>

</html>
```



# 表单

表单用于搜集不同类型的用户输入，表单由不同类型的标签组成，相关标签及属性用法如下：

1、<form>标签 定义整体的表单区域

- action属性 定义表单数据提交地址,如果没有action属性，则表单默认提交给当前的url
- method属性 定义表单提交的方式，一般有“get”方式和“post”方式

2、<label>标签 为表单元素定义文字标注

3、<input>标签 定义通用的表单元素

- type属性
  - type="text" 定义单行文本输入框
  - type="password" 定义密码输入框
  - type="radio" 定义单选框
  - type="checkbox" 定义复选框
  - type="file" 定义上传文件
  - type="submit" 定义提交按钮
  - type="reset" 定义重置按钮
  - type="button" 定义一个普通按钮
  - type="image" 定义图片作为提交按钮，用src属性定义图片地址
  - type="hidden" 定义一个隐藏的表单域，用来存储值
- value属性 定义表单元素的值
- name属性 定义表单元素的名称，此名称是提交数据时的键名

4、<textarea>标签 定义多行文本输入框

5、<select>标签 定义下拉表单元素

6、<option>标签 与<select>标签配合，定义下拉表单元素中的选项

**注册表单实例：**

```html
<form action="http://www..." method="get">
<p>
<label>姓名：</label><input type="text" name="username" />
</p>
<p>
<label>密码：</label><input type="password" name="password" />
</p>
<p>
<label>性别：</label>
<input type="radio" name="gender" value="0" /> 男
<input type="radio" name="gender" value="1" /> 女
</p>
<p>
<label>爱好：</label>
<input type="checkbox" name="like" value="sing" /> 唱歌
<input type="checkbox" name="like" value="run" /> 跑步
<input type="checkbox" name="like" value="swiming" /> 游泳
</p>
<p>
<label>照片：</label>
<input type="file" name="person_pic">
</p>
<p>
<label>个人描述：</label>
<textarea name="about"></textarea>
</p>
<p>
<label>籍贯：</label>
<select name="site">
    <option value="0">北京</option>
    <option value="1">上海</option>
    <option value="2">广州</option>
    <option value="3">深圳</option>
</select>
</p>
<p>
<input type="submit" name="" value="提交">
<!-- input类型为submit定义提交按钮  
     还可以用图片控件代替submit按钮提交，一般会导致提交两次，不建议使用。如：
     <input type="image" src="xxx.gif">
-->
<input type="reset" name="" value="重置">
</p>
</form>
```



> input类型为submit定义提交按钮  
> 还可以用图片控件代替submit按钮提交，一般会导致提交两次，不建议使用。如：
> <input type="image" src="xxx.gif">



### label标签中for属性的妙用

当设置正确的label标签及for属性后,for属性的值为对应的输入框或者选择框的id值, **只要点击`label`标签就可以激活对应的表单输入框或者选择框**

```html
<label for="username">用户名：</label>  <!-- 点击此标签,就能激活username输入框 -->
<input type="text" name="username" id="username" />
<label for="password">密&nbsp;&nbsp;&nbsp;码：</label>
<input type="password" name="password" id="password">
```



# a 标签

## href 属性

### 1. 值为空

表示重新加载此页

### 2. 值为`#`

表示回到顶部

### 3. 值为 `javascript:;`

表示什么都不做

#### 关于`javascript:;`

它表示执行`:`后面的javascript语句

```html
<a href="javascript:alert('点击');"></a>
```

