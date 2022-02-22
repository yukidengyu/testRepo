> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [sspai.com](https://sspai.com/post/47110#!)

> 如何批量 Markdown 转为 PDF？作者提供了一个方案，让你迅速搞定 Markdown-PDF 文档的转换。文章步骤非常详细，还有现成的工具供你取用。

编注：文档格式转换，一直都是个烦心事儿。比如简单轻便的 Markdown 文档，想导出 PDF 时却要手工操作。如果不巧遇上一堆文件需要处理，就很让人头大。本文作者是一位大学教师，同时他也是一位自动化高玩，擅长使用自动化工具来解决问题。这篇文章中，作者将带你迅速搞定 Markdown-PDF 文档的转换。虽然文中有一些代码，但放心，文章步骤非常详细，并且作者还提供了现成的工具供你取用。

![](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADIAAAAyCAYAAAAeP4ixAAACbklEQVRoQ+2aMU4dMRCGZw6RC1CSSyQdLZJtKQ2REgoiRIpQkCYClCYpkgIESQFIpIlkW+IIcIC0gUNwiEFGz+hlmbG9b1nesvGW++zxfP7H4/H6IYzkwZFwQAUZmpJVkSeniFJKA8ASIi7MyfkrRPxjrT1JjZ8MLaXUDiJuzwngn2GJaNd7vyP5IoIYY94Q0fEQIKIPRGS8947zSQTRWh8CwLuBgZx479+2BTkHgBdDAgGAC+fcywoyIFWqInWN9BSONbTmFVp/AeA5o+rjKRJ2XwBYRsRXM4ZXgAg2LAPzOCDTJYQx5pSIVlrC3EI45y611osMTHuQUPUiYpiVooerg7TWRwDAlhSM0TuI+BsD0x4kGCuFSRVzSqkfiLiWmY17EALMbCAlMCmI6IwxZo+INgQYEYKBuW5da00PKikjhNNiiPGm01rrbwDwofGehQjjNcv1SZgddALhlJEgwgJFxDNr7acmjFLqCyJuTd6LEGFttpmkYC91Hrk3s1GZFERMmUT01Xv/sQljjPlMRMsxO6WULwnb2D8FEs4j680wScjO5f3vzrlNJszESWq2LYXJgTzjZm56MCHf3zVBxH1r7ftU1splxxKYHEgoUUpTo+grEf303rPH5hxENJqDKQEJtko2q9zGeeycWy3JhpKhWT8+NM/sufIhBwKI+Mta+7pkfxKMtd8Qtdbcx4dUQZcFCQ2I6DcAnLUpf6YMPxhIDDOuxC4C6djoQUE6+tKpewWZ1wlRkq0qUhXptKTlzv93aI3jWmE0Fz2TeujpX73F9TaKy9CeMk8vZusfBnqZ1g5GqyIdJq+XrqNR5AahKr9CCcxGSwAAAABJRU5ErkJggg==)

需求
--

不少人喜欢用 Markdown 来写文章，不过给别人看时，我们一般会导出 PDF 文件，便于直观地展示最终效果。有个朋友提出，希望把文件夹中的许多 Markdown 文件，批量转换为对应名称的 pdf 格式文件。于是我编写了一个 Python 脚本，并且分享给你。如果你有类似的需求，欢迎使用。

本文使用 pandoc 作为转换工具，这是一个命令行下的格式转换工具，支持 Markdown、pdf、txt 等多种格式文档之间的相互转换。Pandoc 可以自动解析 Markdown 文件里的图片链接，所以不论是本地存储的图片，还是图床上的图片，都可以正确转换并且显示到 pdf 文件里。

![](http://cdn.sspai.com/2018/11/05/at-10-50-20-newfile-2.png)

注：本地图片只测试了图片是绝对路径的情况，Ulysses 等用户导出的 Markdown 文件用了相对路径，需自行测试。

工具下载
----

我已经把代码和样例 Markdown 文件都为你放在了 [这个 github repo](https://github.com/wshuyi/demo-batch-markdown-to-pdf "这个 github repo") 中。

![](http://cdn.sspai.com/2018/11/05/at-10-37-18-2018-11-01-19-13-22-602079.jpeg)

你可以直接点击 [这个链接](https://github.com/wshuyi/demo-batch-markdown-to-pdf/archive/master.zip "这个链接")，下载压缩包 `demo-batch-markdown-to-pdf-master.zip`。

![](http://cdn.sspai.com/2018/11/05/at-10-38-08-2018-11-01-19-48-23-071378.jpeg)

在 macOS 上默认的下载位置是 `~/Downloads`。下载后，解压该压缩包，咱们的演示目录（文件夹）就准备好了。名称是 `~/Downloads/demo-batch-markdown-to-pdf-master` 。

压缩包里面有 4 个文件。

![](http://cdn.sspai.com/2018/11/05/at-10-38-19-2018-11-01-19-12-52-753144.jpeg)

其中的`batch-markdown-to-pdf.py`是运行脚本；`temp_qiniu.md` 和 `README.md` 是咱们的两个示例 Markdown 文件。你尝试之后，可以换成自己的一批 Markdown 文件。

`template.tex`是转换是采用的模板，这个模板并非我做的，它来自于[这个](https://github.com/chengjun90/markdown2pdf "这个") github 项目。

![](http://cdn.sspai.com/2018/11/05/at-10-38-28-2018-11-01-19-15-00-142052.jpeg)

如果你对 latex 有研究，可以自行修改 `template.tex` 的内容，以控制输出 pdf 的样式。

环境准备
----

因为提出需求的朋友，使用的是 macOS 系统，因此这里我们以 macOS 系统的安装方式为准。注意下述工具实际上都是**跨平台**的，因此如果你使用的是 Windows 或者 Linux ，理论上也都是可以使用的。总的来说，我们需要做这几方面的准备：

1.  Python 环境：用于调用 pandoc 进行批量处理
2.  Pandoc：用于把 Markdown 转换为 PDF
3.  tinytex：让 Pandoc 支持中文文档的转换

看起来步骤有点多，但是第一次配置好了，以后再转换格式就不用再费力气。我编写的脚本在 macOS 下测试通过，欢迎你把其他平台测试的结果告诉我。

### 批量处理文件：Python 3

macOS 自带的 Python 版本较老，我们需要最新的 Python 3，这里介绍一个简单的安装方法：HomeBrew。HomeBrew 是一个 macOS 下的包管理器，你可以把它简单理解为命令行版本的软件市场，输入命令就能安装、管理和卸载各种软件。

直接打开终端窗口，在里面输入脚本安装 Homebrew。期间可能检测到你尚未安装 Xcode Command Line Tool，按系统提示安装就行。此 Xcode Command Line Tool 非彼 Xcode，不会占用几个 GB 的空间。

```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"


```

之后，在 Finder 里找到 `~/.profile`（可以通过快捷键 `⇧Shift - ⌘Command -G` 打开「前往」窗口，输入 `~/.profile` 来直达目的地），把下面这一条语句添加到你的 `~/.profile` 文件末尾：

```
export PATH=/usr/local/bin:/usr/local/sbin:$PATH


```

保存退出，新开一个窗口。此时 Homebrew 已经安装好了，你可以接着执行以下命令安装 Python 3：

```
brew install python


```

关于 HomeBrew，可以参考以下文章：

*   [像 Mac 高手一样管理应用，从 Homebrew 开始](https://sspai.com/post/42924 "像 Mac 高手一样管理应用，从 Homebrew 开始")
*   [9 条进阶命令，把 HomeBrew 打造成管理第三方应用的 App Store](https://sspai.com/post/43451 "9 条进阶命令，把 HomeBrew 打造成管理第三方应用的 App Store")

### 格式转换利器：Pandoc

Pandoc 是一个格式转换的工具，Markdown、PDF、TXT、Doc…… 都能够胜任。和安装 Python 一样，可以使用 HomeBrew 来安装它。

```
brew install pandoc


```

### 给 Pandoc 添加中文支持：tinytex

因为需要转换的 markdown 文件，大部分都是中文文档，因此转换到 pdf 的时候，需要 xelatex 的支持。

xelatex 可以用各种 latex 集成包来安装使用，例如 texlive 等。但是这里推荐谢益辉的 [tinytex](https://yihui.name/tinytex/ "tinytex") 包，简单小巧。

![](http://cdn.sspai.com/2018/11/05/at-10-39-31-2018-11-01-19-28-26-942079.jpeg)

不过使用之前，建议删除掉系统里面原有的 texlive 等包。否则可能会造成冲突。

这次 HomeBrew 派不上用场了，在终端窗口下，我们换一个命令：

```
curl -sL "https://yihui.name/gh/tinytex/tools/install-unx.sh"


```

tinytex 就安装好了。

之后，为了能够更好地辅助我们进行转换，需要执行下列命令，安装扩展：

```
tlmgr install unicode-math filehook xecjk xltxtra realscripts fancyhdr lastpage ctex ms cjk ulem environ trimspaces zhnumber collection-fontsrecommended


```

好了，至此准备工作结束，我们该开始执行命令了。

运行
--

准备妥当，现在我们要在终端中转到演示目录的位置 `demo-batch-markdown-to-pdf-master`（不如 Python 会找不到所需的配置文件）。如果你下载的演示目录还在下载文件夹中的话，可以用下面的命令：

```
cd Downloads/demo-batch-markdown-to-pdf-master


```

![](http://cdn.sspai.com/2018/11/05/at-10-39-40-2018-11-01-19-24-27-283167-1.jpeg)

接着，执行目录查看命令：

```
ls


```

如果你看到返回的是如下信息（里面就是我们的演示文件），证明一切正常。

![](http://cdn.sspai.com/2018/11/05/at-10-39-50-2018-11-01-19-31-20-511654.jpeg)

确认好位置没出错后，就可以试着转换文件了。执行：

```
python batch-markdown-to-pdf.py


```

如果顺利，你会看到程序在运行，不过没有什么输出提示的。

![](http://cdn.sspai.com/2018/11/05/at-10-40-00-2018-11-01-19-32-10-331741.jpeg)

因为转换 pdf 的工作需要一些时间。所以如果你的 Markdown 文件很多，可能需要等一会儿。

请不要着急。去喝杯茶，看看书，休息一下。

当你回来的时候，（但愿）已经转换完毕了。

![](http://cdn.sspai.com/2018/11/05/at-10-40-09-2018-11-01-19-33-18-092731.jpeg)

回到「访达」（Finder） ，在我们的演示目录（`~/Downloads/demo-batch-markdown-to-pdf-master`）下面，你会看到新生成了一个文件夹，叫做 `pdf` 。

![](http://cdn.sspai.com/2018/11/05/at-10-40-17-2018-11-01-19-54-51-324878.jpeg)

你的转换后 PDF 文件，应该已经在里面了。

![](http://cdn.sspai.com/2018/11/05/at-10-40-24-2018-11-01-19-55-09-050992.jpeg)

双击打开，看看效果：

![](http://cdn.sspai.com/2018/11/05/at-10-40-30-2018-11-01-19-56-04-572685.jpeg)

如果遇到问题，欢迎反馈给我。祝使用愉快！

如果你对 Python 与数据科学感兴趣，不妨阅读我的系列教程索引贴《[如何高效入门数据科学？](https://www.jianshu.com/p/85f4624485b9 "如何高效入门数据科学？")》，里面还有更多的有趣问题及解法。

> 下载[少数派客户端](https://sspai.com/page/client "少数派客户端")、关注 [少数派公众号](http://sspai.com/s/KEPQ "少数派公众号")，学会更多效率技巧 💪