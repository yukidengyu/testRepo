> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [www.jianshu.com](https://www.jianshu.com/p/7e0ab4201fcc)

PDF 作为可移植文档格式 (Portable Document Format)，在日常生活中经常接触到，最近处理一些数据更是频繁接触一些需要批量处理 pdf 文件的需求，因此便想整理一下自己实践的用 Python 处理 PDF 格式数据的笔记。本文会保持更新。  
PDF 处理的高频需求有：读取、写入、格式转换（pdf 提取文本写入 txt、根据 url 写入 pdf 等）  
、批处理（多个 pdf 合并为 1 个、切分 pdf）等等。  
查了下相关资料，Python 操作 PDF 的库有（只是应用的话肯定不至于造轮子从二进制数据开始读）：pdfminer、pdfminer3k、PyPDF、PyPDF2、pdf2htmlex、pdf2image、pdf2xlsx 等。

![](http://upload-images.jianshu.io/upload_images/2473543-1cba26c8d410aa37.png) pdf 库

### 用 pdf2 合并和切分 PDF

比较几个库之后打算先从 PyPDF2 快速实现一些功能。其官方文档为 [PyPDF2 Documentation](https://links.jianshu.com/go?to=https%3A%2F%2Fpythonhosted.org%2FPyPDF2%2F)，根据文档，PDF2 库包含了 PdfFileReader PdfFileMerger PageObject PdfFileWriter 四个常用的主要的调用类，意思也很明确。  
先用`pip install PyPDF2`安装库。

#### 批量合并 pdf

```
import os
from PyPDF2 import PdfFileReader, PdfFileWriter #导入需要的类（库）
wp='D:/doc_of_pdf/' #work_path

#合并同一个文件夹下的pdf文件
flst=[] #获得pdf文件路径
for root, dirs, files in os.walk(wp):
    flst=files
flst=[wp+f for f in flst]
out_pdf=PdfFileWriter()
for pf in flst:
    in_pdf=PdfFileReader(open(pf, 'rb')) #二进制打开
    page_count=in_pdf.getNumPages() #输入pdf的页数
    for pc in range(page_count): 
        out_pdf.addPage(in_pdf.getPage(pc)) #逐页循环
with open(wp+'合并笔记_1-3章.pdf','wb') as wf:
    out_pdf.write(wf)
#out_pdf.getNumPages()


```

![](http://upload-images.jianshu.io/upload_images/2473543-572df5717e5638f5.png) 执行前后对比

#### 切分 pdf 为多个 pdf

```
#将一个pdf文件根据一定规则切分为多个

sc_pdf=PdfFileReader(open(flst[0], 'rb')) #对第一章笔记进行处理
count_sc=sc_pdf.getNumPages()
#每7页切分为1个PDF文件
out_pdf=PdfFileWriter() #用以输出pdf
for c in range(count_sc):
    if c%7==0 and c>0:
        with open(wp+'切分_{0}.pdf'.format(c),'wb') as wf:
            out_pdf.write(wf)
        out_pdf=PdfFileWriter() #重建一个空对象
    else:
        out_pdf.addPage(sc_pdf.getPage(c))


```

![](http://upload-images.jianshu.io/upload_images/2473543-086fb2e6b96fd5a6.png) 切分测试结果截图

通过上面的实践，可以看到实现这几个需求高频使用到的方法就是新建一个 Reader 或 Writer 对象，通过

`.getNumPages()`

获取一共的页码，通过

`.getPage(page)`

获取特定页，

`.addPage()`

写入页码。

图片转 pdf
-------

需要用到额外的 PIL 库；处理图片的神器。

```
#单张图片转pdf
from PIL import Image
from PyPDF2 import PdfFileReader, PdfFileWriter
img = Image.open('D:/docOfStu/pypdf2-mindmap-01.JPG')
img.save('D:/docOfStu/pypdf2-mindmap-01.pdf', 'PDF') #通过PIL库保存为pdf格式

#多张图片转pdf
ilst=['D:/docOfStu/pypdf2-mindmap-01.jpg','D:/docOfStu/pypdf2-mindmap-02.jpg'] #图片列表 
# for root, dirs, files in os.walk(wpt): ilst=files  #也可以通过os.walk(wpt) 读取文件夹wpt下所有图片
out_pdf=PdfFileWriter()
for f in ilst:
    img = Image.open(f)
    fw=f.replace('.jpg','.pdf')
    img.save(fw)
    out_pdf.appendPagesFromReader(PdfFileReader(open(fw,'rb'))) #也可拆这句为 sc_pdf=PdfFileReader(open(fw,'rb')); out_pdf.addPage(sc_pdf.getPage(0))
out_pdf.write(open('D:/docOfStu/pypdf2-mindmap-04.pdf','wb'))


```

![](http://upload-images.jianshu.io/upload_images/2473543-6a184c7c219a1314.PNG) 图片转 pdf 对比效果

代码同步更新于：[QLWeilcf_pdfProccWithpy.ipynb](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2FQLWeilcf%2FStack_lcf%2Fblob%2Fmaster%2FpdfProccWithpy.ipynb)。

to be continue…

参考资料
----

*   [PyPDF2 Documentation](https://links.jianshu.com/go?to=https%3A%2F%2Fpythonhosted.org%2FPyPDF2%2F)
*   [pdfminer](https://links.jianshu.com/go?to=https%3A%2F%2Fpypi.org%2Fproject%2Fpdfminer%2F)