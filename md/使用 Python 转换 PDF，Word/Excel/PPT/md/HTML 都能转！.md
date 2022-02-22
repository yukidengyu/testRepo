> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [zhuanlan.zhihu.com](https://zhuanlan.zhihu.com/p/147498949)

**往期精选**

[Python 办公自动化｜从 Word 到 Excel](http://mp.weixin.qq.com/s?__biz=MzI1MTUyMjc1Mg==&mid=2247485400&idx=1&sn=c322d457a6e4ef712b884eeedf90f6f6&chksm=e9f0fc77de8775612aea34585ad78e337bf98229a709e9e35f8e1ccba47b9c8701c18b189a0c&scene=21#wechat_redirect)[Python 办公自动化｜从 Excel 到 Word](http://mp.weixin.qq.com/s?__biz=MzI1MTUyMjc1Mg==&mid=2247485450&idx=1&sn=37461f609a0e89c9e205abf60683b224&chksm=e9f0f3a5de877ab3c52d282aac8382a85a2e2558a1d5ecdbdfbf6731a7ea2fea426c49d0aa56&scene=21#wechat_redirect)[Python 办公自动化｜光速对比并提取两份 Word/Excel 中的不同元素](http://mp.weixin.qq.com/s?__biz=MzI1MTUyMjc1Mg==&mid=2247486235&idx=1&sn=78b841c47863e545511b0dd925e7d371&chksm=e9f0f0b4de8779a231298c1e04940bc730ffdee202006a19aaeee5502b98a8a81db3113ff7d0&scene=21#wechat_redirect)

> 同一个操作执行两次，就要考虑自动化！

大家好，又到了 Python 办公自动化专题。

今天讲的是各位一定会接触到的 PDF 转换，关于各种格式的文件转换为 PDF 有很多第三方工具与网站可以实现，但是使用 Python 的好处不仅可以**批量转换**，同时一旦脚本写完了以后就可以**一键执行**，彻底解放双手，那么本文就来盘一盘如何使用 Python 来将 **Word/Excel/PPT/Markdown/Html** 等各种格式的文件转换为 PDF！

**Word 转 PDF**
--------------

Word 转 PDF 应该是最常见的需求了，毕竟使用 PDF 格式可以更方便展示文档，虽然在 Word 中可以直接导出为 PDF 格式，但是使用 Python 可以批量转换，更加高效。  

目前在 Python 中针对 Word 转换为 PDF 的库有很多，比如 win32 就可以调用 word 底层 vba，将 word 转成 pdf，或者 comtypes 等，但是这些常用的库仅能在 Windows 机器上运行，所以为了照顾 mac 用户本文使用一个比较小众的库 docx2pdf，看名字就能知道这是专门用于 word 转 pdf，安装很简单

```
pip install docx2pdf

```

使用也比 win32 等库更简洁，一行代码导入一行代码转换即可  

```
from docx2pdf import convert
convert("input.docx", "output.pdf")

```

但是有人就会说虽然简单，但是这个操作 word 本身就可以完成，好的接下来放大招，我们可以使用下面的代码**找到当前或者指定文件夹下的全部 word 文件**

```
#查找当前目录下的全部word文件
import os
import glob
from pathlib import Path

path = os.getcwd() + '/'
p = Path(path) #初始化构造Path对象
FileList=list(p.glob("**/*.docx")) 

```

![](https://pic3.zhimg.com/v2-11b7535403063603ca6e163787adc36a_r.jpg)

接下来只要写一个循环就可以将该目录下的全部 word 一次性转换为 PDF

```
for file in FileList:
    convert(file,f"{file}.pdf")

```

就这样，**不到 10 行代码，只要一秒**，指定文件夹中 5 份 Word 就轻松转换为 PDF，现在还能使用我们之前自动化系列文章写过的批量合并 PDF 结合一键合并这 5 份 PDF！

**Excel 转 PDF**
---------------

Excel 转 PDF 可能平时用的不多，但是作为 Office 全家桶中的重要工具，并且转换完的表格可以复制所以我们也讲一下。使用到的工具既不是常用的 openpyxl 也不是 pandas，而是另一个专门用于处理 PDF 的库 fpdf  

```
import pandas as pd
import numpy as np
df_1 = pd.DataFrame(np.random.randn(10, 2), columns=list('AB'))

```

为了方便讲解我们使用 Pandas 和 NumPy 来创建一个示例数据文件，当然也可以使用从本地读取

![](https://pic3.zhimg.com/v2-9d2de24ee04ed0e0f1e633124b68fdb6_r.jpg)

现在可以使用下面的代码将这个表格转换为 PDF  

```
from fpdf import FPDF
pdf = FPDF()
pdf.add_page()
pdf.set_xy(0, 0)
pdf.set_font('arial', 'B', 14)
pdf.cell(60)
pdf.cell(70, 10, 'Excel to PDF', 0, 2, 'C')
pdf.cell(-40)
pdf.cell(50, 10, 'Index Column', 1, 0, 'C')
pdf.cell(40, 10, 'A', 1, 0, 'C')
pdf.cell(40, 10, 'B', 1, 2, 'C')
pdf.cell(-90)
pdf.set_font('arial', '', 12)
for i in range(0, len(df_1)):
    col_ind = str(i)
    col_a = str(df_1.A.iloc[i])
    col_b = str(df_1.B.iloc[i])
    pdf.cell(50, 10, '%s' % (col_ind), 1, 0, 'C')
    pdf.cell(40, 10, '%s' % (col_a), 0, 0, 'C')
    pdf.cell(40, 10, '%s' % (col_b), 0, 2, 'C')
    pdf.cell(-90)
pdf.output('Excel2PDF.pdf', 'F')

```

![](https://pic2.zhimg.com/v2-4b4f7800e2288ac93e18c0b09e92b7d5_r.jpg)

，其实思路和 openpyxl 类似，遍历每一个单元格并写入数据，只不过现在是往 PDF 文件中写入。

**PPT 转 PDF**
-------------

本节介绍一下 PPT 如何转换为 PDF，但是我搜了一大圈都没有 MAC 用户可以实现的方法，所以只能针对 Windows 去操作，使用到的就是在 word2pdf 中讲到的 comtypes

```
import sys
import os
import comtypes.client
#设置路径
input_file_path = sys.argv[1]
output_file_path = sys.argv[2]
input_file_path = os.path.abspath(input_file_path)
output_file_path = os.path.abspath(output_file_path)
#创建PDF
powerpoint = comtypes.client.CreateObject("Powerpoint.Application")
powerpoint.Visible = 1
slides = powerpoint.Presentations.Open(input_file_path)
#保存PDF
slides.SaveAs(output_file_path, 32)
slides.Close()

```

相关参数与细节可以查阅 comtypes 官方文档，因为我是 mac 所以没有过多研究，在成功转换之后就可以和我们之前的批量操作与合并进行结合实现自动化了！

**md 转 pdf**
------------

关于 markdown 转 pdf，几乎所有 markdown 编辑器都支持导出为 pdf 格式，本以为这个需求并不高，但是研究了一圈发现很多老外造了很多 md 转 pdf 的轮子，比如 md2pdf、markdown2pdf、md2pdf-client 等。因为大多数博客使用的是 markdown 格式，使用这些库可以很好的将博客文章批量转换为 PDF 文档存储。  

早起都试了一圈，找到一个语法最简单的 markdown2pdf3，直接 pip 安装即可，使用两行代码即可将一个 md 文件转换为 pdf

```
from markdown2pdf3 import *
convert_markdown_to_pdf('test.md') #你的markdown文件路径

```

但是要注意的是如果有中文，还需要进行一些额外的设置，可以查阅官方文档，不过现在就能和之前讲的 Word 转 PDF 结合，**批量转换**指定路径下的全部 markdown 文件为 pdf，比如可以使用下面的代码找到当前文件夹下的全部 md 文件  

```
import os
import glob
from pathlib import Path

path = os.getcwd() + '/'
p = Path(path) #初始化构造Path对象
FileList=list(p.glob("**/*.md")) 

```

**html 转 pdf**
--------------

关于 html 也就是网页转为 PDF 是来问我最多的问题，其实很简单，之前在 **[Selenium 爬取公众号全部文章](http://mp.weixin.qq.com/s?__biz=MzI1MTUyMjc1Mg==&mid=2247485602&idx=1&sn=5140a8f6cea0948d42c67d31b0701a6b&chksm=e9f0f30dde877a1b1810aaad41b544bb775f0b72723d8083f4cb4123cd6dcfc90674c349f571&scene=21#wechat_redirect)**这篇文章中就提到使用 PDFKIT 即可，但是并不是直接 pip 安装 pdfkit 就行，我们需要提前进入下面的网站选择自己电脑系统对应的 wkhtmltopdf 下载安装

```
https://wkhtmltopdf.org/downloads.html

```

![](https://pic2.zhimg.com/v2-0cc80c89cc5552b94cd090e7e035a53d_r.jpg)

安装完使用 pip 安装 pdfkit

```
pip install pdfkit

```

现在我们就能使用两行代码**转换指定网页为 PDF 格式**，比如将我的第一篇自动化文章转为 PDF  

![](https://pic1.zhimg.com/v2-0faf9b13e69c5398153ad1521ce5c7bc_r.jpg)![](https://pic1.zhimg.com/v2-deeedaebf31bf6b66f4eb5951c8f1dac_r.jpg)![](https://pic4.zhimg.com/v2-576d9f63cab5dee885fe8020e38e6c97_r.jpg)

看起来效果还是非常好的，所有格式包括代码都完整的保存了下来，接下来怎么做就不用我多说了，比如你想下载一个公众号所有文章为 PDF 格式，那就先将历史文章 URL 提取出来，接着使用 pdfkit 转换即可，而这两步骤我们都已经详细讲解过了！