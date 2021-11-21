> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [www.cnblogs.com](https://www.cnblogs.com/JackyPeng/articles/6830229.html)

**1. 排除文件打开方式错误**

r **只读**，r+ **读写，**不创建

w **新建只写**，w+ **新建读写**，二者都会将文件内容清零

（以 w 方式打开，不能读出。w + 可读写）

**w + 与 r + 区别：**

r+：可读可写，若文件不存在，报错；w+: 可读可写，若文件不存在，创建

**r + 与 a + 区别：**

[![](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

```
1     fd = open("1.txt",'w+')  
2     fd.write('123')  
3     fd = open("1.txt",'r+')  
4     fd.write('456')  
5     fd = open("1.txt",'a+')  
6     fd.write('789')  

```

[![](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

结果：456789

说明 r + 进行了覆盖写。

**以 a,a + 的方式打开文件，附加方式打开**  

（**a**：**附加写**方式打开，不可读；**a+**: **附加读写**方式打开）  

以'U' 标志打开文件, 所有的行分割符通过 [Python](http://lib.csdn.net/base/python "Python知识库") 的输入方法 (例 #如 read*() )，返回时都会被替换为换行符 \ n. ('rU' 模式也支持'rb' 选项) . 

r 和 U 要求文件必须存在

**不可读的打开方式**：**w 和 a**

**若不存在会创建新文件的打开方式：a，a+，w，w+**

[![](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

```
 1     >>> fd=open(r'f:\mypython\test.py','w')    #只读方式打开，读取报错  
 2     >>> fd.read()  
 3     Traceback (most recent call last):  
 4       File "<stdin>", line 1, in <module>  
 5     IOError: File not open for reading  
 6     >>> fd=open(r'f:\mypython\test.py','a')#附加写方式打开，读取报错  
 7     >>> fd.read()  
 8     Traceback (most recent call last):  
 9       File "<stdin>", line 1, in <module>  
10     IOError: File not open for reading  
11     >>></span></span></span>  

```

[![](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

**2. 正确读写方式打开，出现乱码**

```
1     >>> fd=open(r'f:\mypython\test.py','a+')  
2     >>> fd.write('123')  
3     >>> fd.read()  
4     >>> fd.close()  

```

close 之前，手动打开文件，什么都没写入；close 后，手动打开文件，乱码：123嚅?     

原因分析：指针问题。open() 以 a + 模式开启了一个附加读写模式的文件，由于是 a，所以指针在文件末尾。此时如果做

在写入 123 之后，指针的位置是 4，仍然是文件尾，文件在内存中是但看起来 read() 的时候，Python 仍然去试图在磁盘的文件上，将指针从文件头向后跳 3，再去读取到 EOF 为止。

也就是说，你实际上是跳过了该文件真正的 EOF，为硬盘底层的数据做了一个 dump，一直 dump 到了一个从前存盘文件的

 解决方案：读取**之前**将指针重置为文件头（如果读取之后重置再读，无效）

```
1     >>> fd=open(r'f:\mypython\test.py','a+')  
2     >>> fd.seek(0)  
3     >>> fd.read()  
4     '123'<span>           </span>#顺利读出</span></span>  

```

**3. 文件里有内容，却读出空字符**

[![](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

```
1     >>> fd=open(r'f:\mypython\test.py','w+') #清空内容，重新写入  
2     >>> fd.write('456')  
3     >>> fd.flush()<span>     </span>#确定写入，此时文件内容为“456”  
4     >>> fd.read()  
5     '' #读出空  

```

[![](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

原因：同样是指针问题，写入后指针指向末尾 [EOF]，因此读出空

解决方案一、调用 close 后重新打开，指针位于开头。(r,r+,a+,U 都可以，注意不要用 w，w+，a 打开)

[![](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

```
    >>> fd.close()  
    >>> fd=open(r'f:\mypython\test.py','a+')  
    >>> fd.read()  
    '456'  
    >>> fd.close()  
    >>> fd=open(r'f:\mypython\test.py','r+')  
    >>> fd.read()  
    '456'<pre >>>> fd.close()  
    >>> fd=open(r'f:\mypython\test.py','r')  
    >>> fd.read()  
    '456'  
    >>> fd.close()  
    >>> fd=open(r'f:\mypython\test.py','U')  
    >>> fd.read()  
    '456'  

```

[![](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

解决方案二、调用 seek 指向开头

[![](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

```
    >>> fd=open(r'f:\mypython\test.py','w+')  
    >>> fd.write('456')  
    >>> fd.seek(0)  
    >>> fd.read()  
    '456'  

```

[![](http://common.cnblogs.com/images/copycode.gif)](javascript:void(0); "复制代码")

seek 函数

    seek(offset[, whence]) ，offset 是相对于某个位置的偏移量。位置由 whence 决定，默认 whence=0，从开头起；whence=1，从当前位置算起；whence=2 相对于文件末尾移动，通常 offset 取负值。