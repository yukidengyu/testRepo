> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [www.jianshu.com](https://www.jianshu.com/p/4fa7c107fe9c)

口胡一段，可忽略  
  第一次接触到 markdown 这一新鲜事物都是因为简书，直接使用键盘键入字体格式的感觉，对我这样一个喜欢用字处理软件来记笔记的强迫症来讲，实在爽的飞起，就 word 那一套不断使用鼠标来更换标题样式的骚操作真是让人累的吐血。  
  如果你和我一样，是一个对电脑有着迷之占有欲望，不喜欢在硬盘中安装乱七八糟软件而且拥有 Rstudio 的人，可以尝试使用 R markdown 来完成离线书写任务，直接插入代码在最终文本里就可以得到图表结果的功能对有特殊需要的群体来讲也着实很赞。  
  但是网络上有关 R markdown 的教程实在少得可怜（难道是用 R 的人太少了么？），仅有的内容也过于零散，不过好在网络发达，东拼西凑之下折腾了一下午，总算是成功安装，立此贴记录注意事项，也为造福后来者。

R markdown 安装
-------------

  与安装其它 R 包类似。

```
install.packages('rmarkdown')


```

R markdown 配置—— tinytex 包
-------------------------

  安装之后只是拥有了离线写作的功能，但是并不能将你完成的作品保存成你想要的格式，所谓配置，就是安装其他拓展包让使用者可以将利用 R markdown 编写的内容进行保存输出。  
  网上常用的办法是在桌面安装 miktex 或 tex live 等 tex 管理工具，但能用 R 包完成的事为什么还去麻烦别人呢？联网状态下输入以下两行，安装益辉大神的 tinytex 拓展包（这一步耗时较长，请耐心等待）：

```
install.packages('tinytex')
tinytex::install_tinytex()


```

>   TinyTeX 是益辉大神基于 TeX Live 管理工具修改的 R 拓展包，更多关于 TinyTeX 的内容可以点击下方链接去益辉大神的主页深入了解。  
> [https://yihui.name/tinytex/cn/](https://links.jianshu.com/go?to=https%3A%2F%2Fyihui.name%2Ftinytex%2Fcn%2F)

>   至此，需要软件资源全部安装完毕

R markdown 配置 2 —— 设置字体引擎与编码
----------------------------

按顺序打开 toosl→global options  
点击 sweave，然后在 weave rnw files using 选择 knitr, 在 typeset latex into PDF using 选择 XeLaTex  
点击 code，然后在 saving 中选择 text encoding 为 UFT-8

![](http://upload-images.jianshu.io/upload_images/7998236-c5bfed31c06e9665.png)

![](http://upload-images.jianshu.io/upload_images/18675259-44b17ac2433efba2.png)

> 前期准备工作结束

R markdown 写作篇
--------------

### 新建 R markdown 环境

>   1.File→new filw→r markdown

![](http://upload-images.jianshu.io/upload_images/7998236-f0134a19c3827438.png)

>   2. 在窗口中编辑文件名称，文件作者以及文件类型

![](http://upload-images.jianshu.io/upload_images/7998236-1ea0fbb490d71ee8.png)

>   3. 在 Knit 可选择不同的文件类型进行浏览，同时将对应格式保存至工作目录下。

![](http://upload-images.jianshu.io/upload_images/7998236-021fc1e0206e9c2d.png)

>   4. 使用 PDF 格式并输出中文时，请点击小齿轮，并修改字体引擎为 xelatex。

![](http://upload-images.jianshu.io/upload_images/18675259-900d521260310b51.png)

> 至此所有设置工作完成！

R markdown 工作预览
---------------

  从另一位大神那里盗了一张图，来说明一下 R markdown 工作区的组成。

> 图片来自 [https://www.jianshu.com/p/72c449797f38](https://www.jianshu.com/p/72c449797f38)  
> 
> ![](http://upload-images.jianshu.io/upload_images/12888637-eff80c2de324a5dc.jpg)

工作区各部分的只能和其名称相称。

*   YAML 为标题栏，记录你的题目和作者信息，以及输出格式和字体引擎等。
*   代码段为 R 代码，其输出结果会在预览中显示，如果不需要可以直接删除。
*   正文区正常添加文字，代码与简书 markdown 代码基本一致。

教程结束，开始和你的 R markdown 愉快玩耍吧！