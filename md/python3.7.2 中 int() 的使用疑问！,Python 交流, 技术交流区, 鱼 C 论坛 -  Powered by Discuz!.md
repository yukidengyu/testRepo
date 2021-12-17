> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [fishc.com.cn](https://fishc.com.cn/thread-131138-1-1.html) ![](https://fishc.com.cn/ucenter/avatar.php?uid=638966&size=middle)XaRocky ### **马上注册，结交更多好友，享用更多功能 ^_^**

您需要 [登录](member.php?mod=logging&action=login) 才可以下载或查看，没有帐号？[立即注册](member.php?mod=REGFishC "注册帐号")

x 刚开始学小甲鱼零基础入门的第二节课，有一个  
a=input('请输入 0 到 100 的数字:\n')  
print(a)  
print('a type is',type(a))  
#b=float(a)  
#print(b,'b type is',type(b))  
#temp=int(b)  
temp=int(a) #当程序运行到这里的时候会报错  
print(temp)  
if temp in range(0,100):  
    print('你妹的真棒，^_^')  
else:  
    print('你大爷的，真丑！T_T')  
================================================  
Python 3.7.2 (tags/v3.7.2:9a3ffc0492, Dec 23 2018, 23:09:28) [MSC v.1916 64 bit (AMD64)] on win32  
Type "help", "copyright", "credits" or "license()" for more information.  
>>>  
================ RESTART: D:/Program Files/Python37/1st-3.py ================  
请输入 0 到 100 的数字:  
3.14  
3.14  
a type is <class 'str'>  
Traceback (most recent call last):  
  File "D:/Program Files/Python37/1st-3.py", line 8, in <module>  
    temp=int(a)  
ValueError: invalid literal for int() with base 10: '3.14'  
=================================================  
所以我的问题是为什么输入小数的时候，int（a）取整会报错？？ 最佳答案 [月排行榜](https://fishc.com.cn/bestanswer.php?mod=ranking#study_month) / [总排行榜](https://fishc.com.cn/bestanswer.php?mod=ranking#study_total) ![](https://fishc.com.cn/ucenter/avatar.php?uid=247015&size=small) **[hehex](https://fishc.com.cn/space-uid-247015.html)** 2019-2-21 11:15:40 这是 python 强类型语言的特性。对于 python2 来说 input 语句输入的就是数字，所以就是数值类型的转换。（好久不用 python2 记不特别清楚了，是不是直接就是整数，反正是数字），你要输入字符串必须用 raw_input  
对于 python3 的引擎取消了原来的 raw_input ，而 input 输入进来就是个字符串 (class_str)。要实现字符串的数字 -> 整数，估计是内部先进行遍历看看是不是都是 digit 字符，这时候出现的 "." 就是非法字符，所以直接抛出错误了。  
所以解决的方案是 int(float(input())) 即先将小数字符串 ->float 类型的数字 -> 取整。 [跳转到最佳答案楼层](https://fishc.com.cn/forum.php?mod=redirect&goto=findpost&ptid=131138&pid=3769461) .best_avatar img{width: 30px;height: 30px;border-radius: 30px;} ![](https://fishc.com.cn/ucenter/avatar.php?uid=482672&size=middle)jackz007  _ 本帖最后由 jackz007 于 2019-2-21 01:17 编辑_  
        用 int(v) 做测试，当 v 为数值型变量时，整型、浮点都可以接受，但是，当 v 为字符串型变量时， 只能接受整型数的字符串。  
        根据楼主提供的信息，a 的类型是字符串，显然，'3.14' 并非整数串，此即出错的原因。  
        在 Python 2.7 下，同样输入 3.14，显示 a 的类型是浮点数，代码能正常执行，恐怕这是 Python 2.x 和 Python 3.x 下 input() 函数存在的显著区别。  
        以下是测试运行实况：  
F:\[2019]\00.00.Exercise\Python\int>python x.py  
请输入 0 到 100 的数字:  
3.14  
3.14  
('a type is', <type 'float'>)  
3  
你妹的真棒，^_^  
F:\[2019]\00.00.Exercise\Python\int>  
![](https://fishc.com.cn/ucenter/avatar.php?uid=428663&size=middle)BngThea  直接转换字符串为 int，字符串中不能有小数点 ![](https://fishc.com.cn/ucenter/avatar.php?uid=568108&size=middle) yanyongyu 用 float（）把字符串转换成浮点数 ![](https://fishc.com.cn/ucenter/avatar.php?uid=247015&size=middle) hehex 对于 python3 的引擎取消了原来的 raw_input ，而 input 输入进来就是个字符串 (class_str)。要实现字符串的数字 -> 整数，估计是内部先进行遍历看看是不是都是 digit 字符，这时候出现的 "." 就是非法字符，所以直接抛出错误了。  
所以解决的方案是 int(float(input())) 即先将小数字符串 ->float 类型的数字 -> 取整。![](https://fishc.com.cn/ucenter/avatar.php?uid=411139&size=middle)13572044595  _ 本帖最后由 13572044595 于 2019-2-21 13:03 编辑_  
**修改第一行代码：**  
**方法 1：**  

1.  a = eval(input('请输入 0 到 100 的数字:\n'))   # eval()-- 把元素当代码来处理，或者理解为 去掉两边的 引号

_复制代码_**方法 2：**  

1.  a = float(input('请输入 0 到 100 的数字:\n'))   # float() -- 把元素 转化 为 浮点型

_复制代码_ **示例：**  

1.  >>>  
    
2.  >>>  
    
3.  >>> a = '3.14'  
    
4.  >>>  
    
5.  >>> float(a)  
    
6.  3.14  
    
7.  >>>  
    
8.  >>> eval(a)  
    
9.  3.14  
    
10.  >>>  
    
11.  >>>

_复制代码_ ![](https://fishc.com.cn/ucenter/avatar.php?uid=638966&size=middle) XaRocky 

> [13572044595 发表于 2019-2-21 12:59](https://fishc.com.cn/forum.php?mod=redirect&goto=findpost&pid=3769575&ptid=131138)  
> 修改第一行代码：  
> 方法 1：

非常感谢，学到了 eval()![](https://fishc.com.cn/ucenter/avatar.php?uid=638966&size=middle)XaRocky 

> [yanyongyu 发表于 2019-2-21 10:09](https://fishc.com.cn/forum.php?mod=redirect&goto=findpost&pid=3769326&ptid=131138)  
> 用 float（）把字符串转换成浮点数

嗯嗯，这个我也试验出来了，只是不懂的原理啊![](https://fishc.com.cn/static/image/smiley/ARU/aru-1x-1_019.png) ![](https://fishc.com.cn/ucenter/avatar.php?uid=638966&size=middle) XaRocky 

> [jackz007 发表于 2019-2-21 01:05](https://fishc.com.cn/forum.php?mod=redirect&goto=findpost&pid=3769192&ptid=131138)  
> 用 int(v) 做测试，当 v 为数值型变量时，整型、浮点都可以接受，但是，当 v 为字符串型变量时，  ...

分析的很有道理哦![](https://fishc.com.cn/static/image/smiley/lovely/20080925104601861.gif) ![](https://fishc.com.cn/ucenter/avatar.php?uid=638966&size=middle) XaRocky 

> [BngThea 发表于 2019-2-21 08:44](https://fishc.com.cn/forum.php?mod=redirect&goto=findpost&pid=3769246&ptid=131138)  
> 直接转换字符串为 int，字符串中不能有小数点

是的呐