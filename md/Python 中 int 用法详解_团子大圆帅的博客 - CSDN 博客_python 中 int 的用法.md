> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/jpch89/article/details/89762939)

[Python](https://so.csdn.net/so/search?from=pc_blog_highlight&q=Python) 中 int 用法详解
==================================================================================

欢迎转载，转载请注明出处！

### 文章目录

*   [Python 中 int 用法详解](#Python__int__0)
*   *   [0. 参考资料](#0__9)
    *   [1. int 的无参数调用](#1_int__14)
    *   [2. int 接收数字作为参数](#2_int__28)
    *   [3. int 接收字符串作为参数：类型转换](#3_int__44)
    *   [4. int 用于进制转换](#4_int__83)
    *   [5. 版本特性](#5__191)

0. 参考资料
-------

*   [Python 官方文档](https://docs.python.org/3/library/functions.html?highlight=int#int)

1. int 的无参数调用
-------------

不带参数直接 `int()`，得到的是整数 `0`。

```
num = int()
print(type(num))
print(num)
"""
<class 'int'>
0
"""

```

2. int 接收数字作为参数
---------------

*   `int` 接收整数作为参数，其实没什么好说的，还是得到原来的那个整数。
*   `int` 接收浮点数作为参数，会截取该浮点数的整数部分，返回截取后的整数。

```
print(int(2.9))    # 2
print(int(2.1))    # 2
print(int(-2.9))   # -2
print(int(-2.1))   # -2
print(int(3.0))    # 3

```

> **注意**：`int` 没有四舍五入的功能，它做的事情是 _截断_，_截断_，_截断_！

3. int 接收字符串作为参数：类型转换
---------------------

众所周知，`int` 函数可以将一个字符串转换为整数，这也是它最常见的用法：

```
s = input('请输入一个整数：')
print(type(s))
"""
<class 'str'>
"""

num = int(s)
print(type(num))
"""
<class 'int'>
"""

```

> **注意**：确切来讲 `int` 是一个类，而不是函数，虽然它的首字母没有大写。

假如字符串 `s` 是一个浮点数形式的字符串，比如 `'-3.14'`，直接用 `int` 进行类型转换会报 `ValueError` 错，如下所示：

```
s = '-3.14'
num = int(s)
"""
ValueError: invalid literal for int() with base 10: '-3.14'
"""

```

如果想要把浮点数形式的字符串转换为整数，需要先使用 `float` 把字符串转换为浮点数，再对该浮点数使用 `int`，把浮点数截取为整数，如下所示：

```
s = '-3.14'
num = int(float(s))
print(f'num = {num}')
"""
num = -3
"""

```

4. int 用于进制转换
-------------

到目前为止介绍的都是 `int` 的常见用法，接下来介绍的是相对少见的用法，`int` 可以将 `2` 进制到 `36` 进制的字符串、字节串（`bytes`）或者字节数组（`bytearray`）实例转换成对应的 `10` 进制整数。具体的调用形式为：`int(x, base=10)`，其中 `x` 即为字符串、字节串或字节数组的实例。

**参数说明**

*   参数 `x` 必须是合法的整数字面量 `integer literal`。  
    整数字面量前面可以有正负号，但是正负号与数字之间不能有空格。  
    整数字面量两端可以出现任意数量的空白，包括空格键、制表符、换行符等等。  
    `a-z` 或者 `A-Z` 分别表示 `10-35` 的数字。  
    二进制数字可以用 `0b` 或者 `0B` 做前缀，八进制数字可以用 `0o` 或者 `0O` 做前缀，十六进制数字可以用 `0x` 或者 `0X` 做前缀，前缀是可选的。
*   `base` 默认是 `10`，还可以取 `0` 以及 `2` 到 `36`，不能是 `1`，因为没有 `1` 进制。  
    `base` 为 `0` 的时候会按照代码字面量（`code literal`）解析，即只能把 `2`、`8`、`10`、`16` 进制数字表示转换为 `10` 进制。对于 `2`、`8`、`16` 进制必须指明相应进制的前缀，否则会按照 `10` 进制解析。

**默认情况：`base=10`**

```
x = '6'
num1 = int(x)
num2 = int(x, 10)
print(num1)
print(num2)
"""
6
6
"""

```

**其他进制转为 `10` 进制**

```
x = '10'
num1 = int(x, 2)
num2 = int(x, 8)
num3 = int(x, 16)
print(num1)
print(num2)
print(num3)
"""
2
8
16
"""

x = 'z'
num3 = int(x, 36)
print(num3)
"""
35
"""

```

**带正负号、空格的整数字面量**

```
# 带正号
x = '+a0'
num = int(x, 16)
print(num)
"""
160
"""

# 带负号
x = '-a0'
num = int(x, 16)
print(num)
"""
-160
"""

# 两端带空白
x = '  \t  +a0\r\n  '
num = int(x, 16)
print(num)
"""
160
"""

```

**`base=0` 时按照代码字面量直接解析**

```
x = '10'
num = int(x, 0)
print(num)
"""
10
"""

x = '0b10'
num = int(x, 0)
print(num)
"""
2
"""

x = '0o10'
num = int(x, 0)
print(num)
"""
8
"""

x = '0x10'
num = int(x, 0)
print(num)
"""
16
"""

```

5. 版本特性
-------

**`3.4` 版本变化**

*   如果 `base` 参数不是 `int` 类的实例，那么会调用它的 `base.__index__` 方法，该方法返回一个整数供 `base` 参数使用。
*   之前的版本调用的是 `base.__int__` 而不是 `base.__index__`。

**`3.6` 版本变化**

*   代码字面量（`code literal`）中允许使用下划线 `_` 来对数字进行分组。

**`3.7` 版本变化**

*   `x` 现在变成了强制位置参数 `positional-only parameter`。

> **注意**：也就是说 `x` 不能通过 `x=...` 这种关键字参数形式来指定，以下是我在 `Python 3.6` 当中的试验，可见 `3.6` 版本还是可以的。然而 `Python 3.7` 版本会报错！

```
# Python 3.6
x = 'a'
num = int(x=x, base=16)
print(num)
"""
10
"""
# 调过来也可以
x = 'a'
num = int(base=16, x=x)
print(num)
"""
10
"""

# Python 3.7 会报错
>>> x = 'a'
>>> num = int(x=x, base=16)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'x' is an invalid keyword argument for int()

```

完成于 `2019.5.2`

![](https://img-blog.csdnimg.cn/img_convert/4451e047ef220fb5cbe73bcdf3fca051.png)