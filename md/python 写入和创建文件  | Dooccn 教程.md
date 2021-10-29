> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [www.dooccn.com](https://www.dooccn.com/jc/python/python_file_write.html)

> python 写入和创建文件

写入现有文件
------

要写入现有文件，必须向`open()`函数添加参数 ：

`"a"` - 追加 - 将追加到文件末尾

`"w"` - 写 - 将覆盖任何现有内容

### 例

打开文件 “demofile2.txt”，并将内容附加到该文件：

f = open("demofile2.txt", "a")  
f.write("Now the file has more content!")  
f.close()

f = open("demofile2.txt", "r")  
print(f.read())

[运行示例 »](https://www.dooccn.com/python3/#id/aecdda4e2edd916b9305d62f150e2ffb)

### 例

打开文件 “demofile3.txt” 并覆盖内容：

f = open("demofile3.txt", "w")  
f.write("Woops! I have deleted the content!")  
f.close()

f = open("demofile3.txt", "r")  
print(f.read())

[运行示例 »](https://www.dooccn.com/python3/#id/9adc00fd4a7ce740c283e3d279cb39c1)

**注意：** “w” 方法将覆盖整个文件。

创建一个新文件
-------

要在 Python 中创建新文件，请使用`open()`带有以下参数之一的方法：

`"x"` - 创建 - 将创建文件，如果文件存在则返回错误

`"a"` - 附加 - 如果指定的文件不存在，将创建一个文件

`"w"` - 写入 - 如果指定的文件不存在，将创建一个文件

### 例

创建一个名为 “myfile.txt” 的文件：

f = open("myfile.txt", "x")

结果：创建了一个新的空文件！

### 例

如果不存在，请创建一个新文件：

f = open("myfile.txt", "w")