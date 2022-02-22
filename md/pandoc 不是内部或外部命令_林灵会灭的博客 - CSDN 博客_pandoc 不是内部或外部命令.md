> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/qq_35159009/article/details/105039784)

1. 安装 pandoc
------------

win7，在 [Anaconda](https://so.csdn.net/so/search?q=Anaconda&spm=1001.2101.3001.7020) prompt 中

```
pandoc 

```

显示  
![](https://img-blog.csdnimg.cn/20200323004005793.png)

```
‘pandoc’不是内部或外部命令，也不是可运行的程序

```

分析原因，应该是没添加路径，  
环境变量中有 “安装环境 \ Anaconda3\Scripts”，也不知道应该添加啥  
我的情况是：

*   安装环境 \ Anaconda3\Scripts 没有 pandoc.exe 文件
*   在‘\Anaconda3\Lib\site-packages\pandoc’文件夹下也没有执行文件

怎么办呢？  
干脆一不做二不休  
https://u8266128.pipipan.com/fs/8266128-344511561  
在这个网址下载 zip 压缩包  
解压之后，把所有文件粘贴到 “安装环境 \ Anaconda3\Scripts” 中，  
![](https://img-blog.csdnimg.cn/20200323005826905.png)  
接着再运行

```
pandoc --help

```

![](https://img-blog.csdnimg.cn/20200323004034745.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1MTU5MDA5,size_16,color_FFFFFF,t_70)  
就成功啦。

2.latex 文件转 word
----------------

```
cd  进入想要转换的文件所在的文件夹

输入pandoc 想要转换的文件名.tex -o 新生成的文件名.docx

```

![](https://img-blog.csdnimg.cn/2020032301052738.png)  
便成功了  
![](https://img-blog.csdnimg.cn/20200323010603207.png)

_**但是段落**_  
![](https://img-blog.csdnimg.cn/20200323010707738.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1MTU5MDA5,size_16,color_FFFFFF,t_70)  
vs latex 的格式  
![](https://img-blog.csdnimg.cn/20200323010805836.png)  
公式方面  
![](https://img-blog.csdnimg.cn/2020032301073241.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM1MTU5MDA5,size_16,color_FFFFFF,t_70)  
vs latex  
![](https://img-blog.csdnimg.cn/20200323010833548.png)  
还是会有些问题。这就考察 latex 中公式编写的严谨性，十分常用的才可识别。  
注意 latex 要是 utf-8 编码的，不然会报这个错。  
![](https://img-blog.csdnimg.cn/20200323010947265.png)  
不知大神们还有什么好方法，可以留言哦。  
有帮助的话，带个赞吧