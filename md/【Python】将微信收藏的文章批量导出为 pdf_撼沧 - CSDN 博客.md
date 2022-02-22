> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/qq_32832803/article/details/122508085)

### 【Python】将微信收藏的文章批量导出为 pdf

*   [第一步：导出微信收藏的链接](#_4)
*   [第二步：将所有网址导出为 pdf](#pdf_15)

> **写在前面**  
> 微信收藏了大量的文章，一直没时间看。乘飞机或火车时有闲暇时间但网络不行。本文提供了一种基于 python 的将微信收藏的公众号推文批量导出为 pdf 的方法，方便在平板等移动设备离线阅读。

第一步：导出微信收藏的链接
=============

这一步我并没有找到一键导出的办法，选择的方法是：

1.  先将所有要导出的推文批量多选，并分享给好友或者文件传输助手；  
    ![](https://img-blog.csdnimg.cn/e031787d111c4549a2933b49c2964dbe.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5pK85rKn,size_20,color_FFFFFF,t_70,g_se,x_16)
    
2.  然后在聊天界面多选并使用邮箱发送；
    
3.  可以发送给自己的其他邮箱。邮件的内容便包含了每篇文章的标题的网址；  
    ![](https://img-blog.csdnimg.cn/79c3de4f03624b7eafcad061eece2268.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5pK85rKn,size_20,color_FFFFFF,t_70,g_se,x_16)
    
4.  将邮件内容粘贴保存为. txt 格式，编码类型为 ANSI。
    

第二步：将所有网址导出为 pdf
================

pdf 文件名即微信推文标题，部分特殊字符进行了替换。此方法仅适用于微信推文，筛除了不是微信推文的链接。  
代码：

```
import pdfkit
import requests
import os
import re
from bs4 import BeautifulSoup

# set the font style
font = '''
    <style type="text/css">
         @font-face{font-family: "微软雅黑";src:url("‪C:\\Windows\\Fonts\\msyh.ttc")
    </style>
     <style type = "text/css">
        p { font-family: '微软雅黑', cursive;}
    </style>
    '''
# 选项
options = {
    'page-size': 'A6',
#     'margin-top': '0.75in',
#     'margin-right': '0.75in',
#     'margin-bottom': '0.75in',
#     'margin-left': '0.75in',
    'encoding': "UTF-8",
#      'custom-header': headers,
#     'debug-javascript': [''],
#     'javascript-delay': 10000,
#     'no-stop-slow-scripts': "",
#     'load-media-error-handling': 'abort',
 } 

path_wkthmltopdf = r'C:\Program Files\wkhtmltopdf\bin\wkhtmltopdf.exe'
config = pdfkit.configuration(wkhtmltopdf=path_wkthmltopdf)

# 替换特殊字符
def validate_title(title):
    rstr = r"[\/\\\:\*\?\"\<\>\|]"  # '/ \ : * ? " < > |'
    new_title = re.sub(rstr, "_", title)  # 替换为下划线
    new_title = new_title.replace('：',' _')
    new_title = new_title.replace('？','_')
    return new_title

f = open("E:\\user\\Desktop\\wechat\\url.txt")          # 返回一个文件对象   
line = f.readline() 
while line:
    if line[0]=='[':
        # 读取网址和标题
        line2 = line[1:-2]
        urls = line2.split(' : ')
        url = urls[-1]
        
        title = ''.join(urls[0:-1])
        title = validate_title(title)
        
        if url.find('mp.weixin.qq.com') > 0:
            res = requests.get(url)
            # data-src替换为src 有时候返回的正文被隐藏了，将hidden去掉
            html = res.text.replace("data-src", "src").replace('style="visibility: hidden;"',"")
             
            soup = BeautifulSoup(html)
            # 选择正文（去除javascrapt等）
            html = soup.select('div#img-content')[0]
            
            # html_str = str(html)
            # i = 0
            # title = html_str.splitlines()[i]
            # while title[0] == '<' or title.isspace():
            #     i = i+1
            #     title = html_str.splitlines()[i]
            #     title = title.replace('|','-')	# 将字符串里的k全部替换为8
            
            # 修改字体
            html = font + str(html)
            
            output = title+'.pdf'
            
            pdfkit.from_string(str(html), output, configuration=config, options=options)
        else:
            print(title)
        
    line = f.readline() 
f.close

```

代码主要参考了[【实战】Python 将微信文章保存为 PDF](https://blog.csdn.net/qq_38316655/article/details/105986082)，使用的是 [“wkhtmltopdf” 命令行工具](https://wkhtmltopdf.org/)，下载地址：[https://wkhtmltopdf.org/downloads.html](https://wkhtmltopdf.org/downloads.html)。  
在 11 寸左右的平板上阅读的话，实测将页面保存为 A6 是比较合适的，可以在 options 里设置，具体参数参考 [Paper Sizes](https://pdfkit.org/docs/paper_sizes.html)。

在读取 txt 文档时，参考了一些操作关于 python 的方法文章，具体如下：

1.  [python 逐行读取文本](https://www.cnblogs.com/zb-ml/p/8926463.html)
2.  [python 字符串截取、查找、分割](https://www.cnblogs.com/hellofengying/p/10400104.html)
3.  [Python3 读取字符串的第一行](https://techlife.com.cn/blog/archives/2462)
4.  [Python 拼接字符串的 7 种方法总结](https://www.jb51.net/article/149991.htm)
5.  [python 字符串去掉特殊符号](https://blog.csdn.net/weixin_39872334/article/details/110345834)
6.  [Python - 替换或去除不能用于文件名的字符](https://www.cnblogs.com/math98/p/13513765.html)