> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [chinese.freecodecamp.org](https://chinese.freecodecamp.org/news/python-create-file-how-to-append-and-write-to-a-text-file/)

> 在编码中，文件用于存储数据。

在编码中，文件用于存储数据。然后，你可以随时轻松访问该数据。

在 Python 中读取、写入和编辑文件是一项常见任务，因为该语言为我们提供了允许我们这样做的内置函数。

在本文中，我将创建一个简单的项目，我将在其中写入、追加并最终从 Python 中的文本文件中读取，以向你展示它是如何完成的。

你可以跟我一起完成和我一样的步骤。让我们开始吧！

**如何设置项目的结构**
-------------

第一步是设置项目的目录结构。选择要创建新目录的位置，然后按照以下步骤操作。我正在我的主目录中创建项目。

```
cd 


mkdir python_text


cd python_text


touch text.txt scripts.py


code .


```

```
with open("path_to_and_name_of_file","mode") as variable_name:
    variable_name.write('What I want to write goes here')


```

```
with open("text.txt","w") as file:
    file.write("I am learning Python!\n")
    file.write("I am really enjoying it!\n")
    file.write("And I want to add more lines to say how much I like it")


```

```
with open("text.txt","w") as file:
    file.write("I am learning Python!\n")
    file.write("I am really enjoying it!\n")
    file.write("And I want to add more lines to say how much I like it")


```

```
with open("text.txt","a") as file:
    file.write("What I want to add on goes here")


```

```
with open("text.txt","a") as file:
    file.write("I am adding in more lines\n")
    file.write("And more…")


```

```
with open("text.txt","a") as file:
    file.write("\n")
    file.write("I am adding in more lines\n")
    file.write("And more…")


```

```
with  open('text.txt','r') as file:
    print(file.read())


```

```
with open("text.txt","r") as file:
    for line in file:
        print(line)


```

通过这种方法，每一行都被单独打印出来。

**小结**
------

本文向你展示了一些关于如何使用 Python 写入、编辑和读取文件的简单示例。

如果你想了解有关 Python 编程语言的更多信息，freeCodeCamp 有一个免费的 [Python 认证](https://chinese.freecodecamp.org/learn/scientific-computing-with-python/)，你可以从基础开始学习该语言的更复杂方面。最后，你将构建五个项目来实践你所学的知识。

感谢你阅读本文，祝你学习愉快！

原文：[Python Create File – How to Append and Write to a Text File](https://www.freecodecamp.org/news/python-create-file-how-to-append-and-write-to-a-text-file/)，作者：[Dionysia Lemonaki](https://www.freecodecamp.org/news/author/dionysia/)