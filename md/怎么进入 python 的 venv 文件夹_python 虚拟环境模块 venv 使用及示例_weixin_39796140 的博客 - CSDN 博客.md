> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/weixin_39796140/article/details/110399588)

相信只要学习 python 的同学对于[虚拟环境](https://so.csdn.net/so/search?q=%E8%99%9A%E6%8B%9F%E7%8E%AF%E5%A2%83&spm=1001.2101.3001.7020)这个概念肯定不会太陌生，虚拟环境指的是一个个单独隔离的 python 开发环境。各个虚拟环境之间互不干扰，都有自己独立的开发包。就像是在电脑上装了很多个虚拟机，每个虚拟机里面你随便折腾，不会影响到物理机，也不会影响到其他虚拟机。

既然这么有用，那么 Python 里面用来创建虚拟环境的模块 virtualenv 是怎么使用的呢？我们一起来看一下。

virtualenv 基本命令格式

这个模块比较简单，它只有一条命令，但是跟的选项比较多，就是通过不同的选项来控制生成不同的虚拟环境。我们先来简单看一下它的基本命令格式：

virtualenv [--version] [--with-traceback] [-v | -q] [--app-data APP_DATA] [--clear-app-data] [--discovery {builtin}] [-p py]

[--creator {builtin,cpython3-win,venv}] [--seeder {app-data,pip}] [--no-seed] [--activators comma_sep_list][--clear]

[--system-site-packages] [--copies] [--download | --no-download] [--extra-search-dir d [d ...]] [--pip version]

[--setuptools version] [--wheel version] [--no-pip] [--no-setuptools] [--no-wheel]

[--symlink-app-data] [--prompt prompt] [-h]

dest

乍一看，选项怎么这么多，一下子就能把人看晕，这还怎么学。但是在最开始使用的时候，常用的参数就只有下面两个：

-p py 用于选择 python 解释器版本的

dest 要存放虚拟环境的目录，一般是项目目录

--prompt prompt 设置虚拟环境命令提示符前端提示。

参数比较抽象，我们还是通过具体的例子来实践，

示例

先打开 cmd 命令行窗口，执行 python 命令，确认你的 python 包安装正确。如下所示：

C:\Users\Administrator>python

Python 3.7.3 (v3.7.3:ef4ec6ed12, Mar 25 2019, 22:22:05) [MSC v.1916 64 bit (AMD64)] on win32

Type "help", "copyright", "credits" or "license" for more information.

>>>

然后我们来安装好 virualenv 模块，命令是：

C:\Users\Administrator>pip install virtualenv

Looking in indexes: http://pypi.doubanio.com/simple

Collecting virtualenv

Downloading http://pypi.doubanio.com/packages/c1/61/7506ddd79ef6f09beeefb81c4c55bf395a8ad96b33ff1c6b06e40f8aa101/virtualenv-20.0.7-py2.py3-none-any.whl (8.0 MB)

|████████████████████████████████| 8.0 MB 6.8 MB/s

......

Installing collected packages: virtualenv

Successfully installed virtualenv-20.0.7

中间省略了一部分输出容，但是可以看到 virtualenv 模块安装成功。然后我们就来创建一个新的虚拟环境：

C:\Users\Administrator>virtualenv myproject

created [virtual](https://so.csdn.net/so/search?q=virtual&spm=1001.2101.3001.7020) environment CPython3.7.3.final.0-64 in 6317ms

creator CPython3Windows(dest=C:\Users\Administrator\myproject, clear=False, global=False)

seeder FromAppData(download=False, pip=latest, setuptools=latest, wheel=latest, via=copy, app_data_dir=C:\Users\Administrator\AppData\Local\Temp\tmpsvdy5dr2\seed-app-data\v1)

activators BashActivator,BatchActivator,FishActivator,PowerShellActivator,PythonActivator,XonshActivator

这里的虚拟环境目录名词是 myproject，就这么简单的命令创建了一个虚拟环境。它位于 myproject 目录下。创建过程中如果没有报错就说明创建成功，同时它还指出了虚拟环境的 Python 版本，可以看到，就是我们系统当前安装的虚拟版本。

我们查看一下这个目录：

C:\Users\Administrator\dir myproject

2020/03/02 17:04

2020/03/02 17:04

2020/03/02 17:04

2020/03/02 17:04 316 pyvenv.cfg

2020/03/02 17:04

可以看到里面有两个目录和一个文件，

Lib 目录是必须的库文件目录，

Scripts 是虚拟环境的二进制命令文件目录以及启动器所在目录。启动器就是待会我们要用来激活虚拟环境的目录。

pyvenv.cfg 文件是这个虚拟环境的配置文件

我们来尝试激活这个虚拟环境，命令如下：

# 切换到虚拟环境目录

C:\Users\Administrator>cd myproject

激活虚拟环境

C:\Users\Administrator\myproject>Scripts\activate

此时没有任何输出，而且命令提示符的窗口也没有任何改变，怎么判断虚拟环境已经激活成功了呢？我们使用 pip 命令来判断一下，：

C:\Users\Administrator\myproject>pip list

Package Version

---------- -------

pip 20.0.2

setuptools 45.2.0

wheel 0.34.2

可以看到，这个时候的 pip 安装的包只有三个，分别是 pip、setuptools、wheel。而我们实际的系统环境因为我一直在使用，我重新打开一个命令提示符窗口，直接执行 pip list 命令结果如下：

C:\Users\Administrator>pip list

Package Version

------------------ ---------

-ip 19.0.3

appdirs 1.4.3

attrs 19.3.0

backcall 0.1.0

bleach 3.1.0

colorama 0.4.3

decorator 4.4.1

defusedxml 0.6.0

distlib 0.3.0

Django 2.2

....

说明我们的虚拟环境确实激活成功了，只是看起来不明显。但是这个时候虚拟环境确实已经可以使用了。那么使用完毕后该怎么退出呢？启动是 activate，那么退出就是 deactivate。如下所示：

C:\Users\Administrator\myproject>Scripts\deactivate.bat

C:\Users\Administrator\myproject>pip list

Package Version

------------------ ---------

-ip 19.0.3

appdirs 1.4.3

attrs 19.3.0

backcall 0.1.0

bleach 3.1.0

colorama 0.4.3

decorator 4.4.1

defusedxml 0.6.0

distlib 0.3.0

Django 2.2

entrypoints 0.3

执行完 deactivate 命令后，再执行 pip list 命令，可以看到输出的是系统 python 的路径，这个时候说明已经从虚拟环境退出了。

但是这个过程确实不太明显，那么能不能把 虚拟环境弄的更明显一点呢，我们来看看 --prompt 这个选项的用法：

C:\Users\Administrator>virtualenv --prompt myproject2 myproject2

创建一个新的虚拟环境叫做 myproject2，但是使用了 --prompt myproject2 的选项参数，我们再来尝试激活一下看看：

C:\Users\Administrator>cd myproject2

C:\Users\Administrator\myproject2>Scripts\activate

myproject2C:\Users\Administrator\myproject2>Scripts\deactivate.bat

C:\Users\Administrator\myproject2>

这个时候很明显就可以看到，在命令提示符前端出现了一个 myproject2 的提示，而当我们执行 deactivate 脚本后，提示符消失，说明回到了正常的环境。

这样就比较明显了。

上面的两个例子演示的是直接使用系统版本 python 安装的虚拟环境，既然是虚拟环境，那么 python 的版本能不能切换呢？我们来尝试一下 - p 选项的用法。

C:\Users\Administrator>virtualenv --prompt (myproject3) -p python3.8 myproject3

PEP-514 violation in Windows Registry at HKEY_LOCAL_MACHINE/PythonCore/2.7/InstallPath error: missing

RuntimeError: failed to find interpreter for Builtin discover of python_spec='python3.8'

在上面的示例中，我尝试创建以 python3.8 来创建 myproject3 这个项目，但是失败了。它在系统里没有找到 python3.8 的版本。

我们知道，在 windows 中，不同的 python 是可以安装在不同的目录中，但是你系统使用哪个版本，就把哪个版本的路径添加到系统的环境变量最前面即可。刚才的例子里看到我目前使用的主版本是 3.7.3，这个时候如果想要使用 python3.8 来创建虚拟环境的话，就需要告知 3.8 版本的实际路径：

C:\Users\Administrator>virtualenv --prompt (myproject3) -p "D:\Program Files (x86)\Python38\python.exe" myproject3

created virtual environment CPython3.8.2.final.0-64 in 6798ms

creator CPython3Windows(dest=C:\Users\Administrator\myproject3, clear=False, global=False)

seeder FromAppData(download=False, pip=latest, setuptools=latest, wheel=latest, via=copy, app_data_dir=C:\Users\Administrator\AppData\Local\Temp\tmpxuz4d8ys\seed-app-data\v1)

activators BashActivator,BatchActivator,FishActivator,PowerShellActivator,PythonActivator,XonshActivator

这个时候就可以创建成功了，这里有两点需要注意：

必须给出 python.exe 文件的绝对路径

路径中如果有空格，则必须使用双引号括起。

创建成功以后，我们尝试激活

C:\Users\Administrator>cd myproject3

C:\Users\Administrator\myproject3>Scripts\activate

(myproject3)C:\Users\Administrator\myproject3>python

Python 3.8.2 (tags/v3.8.2:7b3ab59, Feb 25 2020, 23:03:10) [MSC v.1916 64 bit (AMD64)] on win32

Type "help", "copyright", "credits" or "license" for more information.

>>>

可以看到，确实是 3.8.2 版本的 Python，此时新版本的虚拟环境创建成功。

基本用法就是这么两个，linux 系统下的操作和这里的类似，大家可以自己去尝试一下，如果有遇到什么问题可以留言和阿飞一起讨论。

实际使用的其他参数可以看一下下面的参数解析，写的比较详细，大家可以根据自己的需要来查看。

参数解析

如果将 virtualenv 的所有参数进行分类，主要分为下面几类：

1. 只用于输出信息的，包括下面几个：

--version 输出 virtualenv 版本信息

-h, --help 输出 virtualenv 帮助信息

--with-traceback，在创建虚拟环境失败的时候，输出追踪信息。

-v, --verbose，设置输出信息等级，一般用于增加信息输出等级，默认值是 2

-q, --quit，设置输出信息等级，一般用于降低信息输出等级，默认值是 0

最后两个选项这里要解释一下，信息输出等级指的是 virtualenv 在执行命令过程中输出的信息等级，分为 5 个类，对应的值分别是：

CRITICAL 0 灾难级信息

ERROR 1 错误级信息

WARNING 2 警告级信息

INFO 3 普通信息

DEBUG 4 调试信息

NOTEST 5 非测试信息

也就是说 - q/--quit 是将信息级别设置为 CRITICAL，即只有灾难级信息才会输出，-v/--verbose 后面跟上对应的值，即可调整对应的信息级别。例如 - v 4，即将信息级别调整为 DEBUG 级别。

2. 通用设置

即在使用 virualenv 命令的时候全局通用的配置，主要有：

--app-data APP_DATA 用于配置 virutalenv 使用的缓存目录，默认是，对于 windows 系统通常是 %USERPROFILE%\AppData\Local\Temp 这个目录，对于 linux 系统是 / tmp 目录。

--clear-app-data virtualenv 启动的时候使用空的 app 数据目录，默认值 False，即默认使用缓存目录，便于加快虚拟环境创建。

3. 发现

发现这个配置是用来查找目标 Python 解释器的，当你创建虚拟环境的时候，你的虚拟环境以哪个 Python 解释器版本作为模板来创建呢？这个时候就需要用到发现这个组件来查找了。对应的选项有：

--discovery {builtin} 解释器发现方法，默认值 builtin，即内置的发现组件。现在好像只有这一个可以选择。也就是说，这个选项目前对于普通用户来说没什么用。除非你能够自己实现一个发现组件。

-p py, --python py，用于创建虚拟环境的目标 python 解释器，即以这个参数指定的 python 解释器版本来创建虚拟环境。默认值是你当前安装 virutalenv 的 python 解释器。它有两种格式，分别是：

I. python 解释器的绝对路径。

II. python 版本的字符串表示，字符串的格式是 {实现名称}{版本}{架构}，例如 python3.8.1, cpython3 等。

4. 创造器（creator）

创造器在 virtualenv 里的作用是，实际执行虚拟环境创建过程的工具。它对应的选项有：

--creator {builtin, cpyhon3-win, venv}，即通过哪个 python 解释器来创建虚拟环境，默认是 builtin，即安装 virtualenv 的 python 版本。

dest，即安装虚拟环境的目标目录，可以使用相对路径和绝对路径

--clear，当目标目录存在的时候，会移除目标目录，或者覆盖目标文件，默认值 False

--system-site-packages， 允许虚拟环境访问系统 site-packages 目录，默认值 False

--copies， --always-copy，使用复制而不是链接来创建虚拟环境，即使平台的默认配置是使用链接的方式。默认值 True。创建虚拟环境有两种方式，第一种是复制模板 Python 里的文件到虚拟环境中，第二种方式是从模板 Python 那里创建软链接到虚拟环境中，第二种速度更快，但是虚拟环境受到模板 Python 的影响也更大。因此一般推荐使用 copy 的方式。

5. 种子器（seeder）

种子器指的是什么呢？我们知道 python 的包管理软件是 pip，但是还有另外两个管理软件，分别是 setuptools 和 wheel。而种子器就是决定虚拟环境中，安装这三个软件中的哪几个。有了包管理软件，才能安装更多的包。就像种子发芽成熟后能结出更多的种子。所以叫 seeder 这样一个名字。对应的选项有：

--seeder {app-data, pip}，种子包安装方法，默认值 app-data，即从缓存目录里安装。

--no-seed， --without-pip，即不安装种子包（默认值 False）

--download 从 PyPI 上下载安装最新版本的 pip/setuptools/wheel，默认值 False

--no-download， --never-download，不下载最新版的 pip/setuptools/wheel，默认值 True

--extrea-search-dir，种除了默认的 Python 安装的包以外，种子器可能会使用的包含其他包的路径，默认值是 []

--pip version, --wheel version， --setuptools version，要安装的 pip，wheel，setuptools 版本，默认值都是 latest

--no-pip，--no-wheel，--no-setuptools，不安装 pip，wheel，setuptools，默认值都是 False

--symlink-app-data，从 app-data 目录链接 python 包到虚拟环境（目前还不支持，要求种子 pip>=19.3，默认值 False）

6. 启动器（activator）

虚拟环境配置好以后，我们怎么进入到虚拟环境，以及怎么让虚拟环境里的配置生效呢？这个时候就需要用到启动器了，启动器的作用是，在 shell 或者系统的环境变量里面，把虚拟环境的可执行命令路径添加到最前面。保证在虚拟环境中，最先找到的命令是虚拟环境里的命令。同样的，当虚拟环境用完以后，我们需要回到正常的环境，这个时候还需要提供虚拟环境的关闭功能。这两个功能都是在启动器里实现的。它对应的配置选项有：

--activator comma_sep_list， 要生成的启动器列表，用逗号分隔。因为你在不同的系统中使用的是不同的 shell，例如 linux 中有 bash、tcsh、dash、zsh 等，windows 中有 powershell，cmd 等。因此针对不同的环境需要使用不同的启动器。这个参数就是让你说明你要生成哪些环境的启动器。

--prompt prompt，在进入到虚拟环境的时候，一般都会在命令提示符的最前面标识出虚拟环境的名称，便于区分实际环境和虚拟环境。而这个名称就是通过这个参数来配置的。

到此这篇关于 python 虚拟环境模块 venv 使用及示例的文章就介绍到这了, 更多相关 python 虚拟环境模块 venv 内容请搜索菜鸟教程 www.piaodoo.com 以前的文章或继续浏览下面的相关文章希望大家以后多多支持菜鸟教程 www.piaodoo.com！