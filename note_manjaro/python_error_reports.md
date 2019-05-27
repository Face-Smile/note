##  unexpected EOF while parsing (\<string\>, line 0)

这是典型的没有验证函数参数是否有效。

你可以运行如下代码，观察输出。

```python
try:
    print eval("")
except Exception as ex:
    print (ex)
```

输出如下

```python
unexpected EOF while parsing (<string>, line 0)
```

> **所以原因是eval(str)的字符串为空，按照如下修改你的代码就用自定义提示代替系统提示！**

```python
try:
    print eval(str)
except Exception as ex:
    print ("表达式为空，请检查")
```



