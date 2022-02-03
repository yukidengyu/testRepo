> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/k417699481/article/details/118795846)

问题描述：
=====

跟着文档示例想简单显示一个 html 页面的，结果渲染出来的是 html 源码。。。  
看着输出的内容想着应该是没有识别出是 html 代码的原因，对比 html 页面的 response 发现果然不一样，然后开始改 response 的 Content-type

```
# habit.py内容
# -*- coding: utf-8 -*-
# !/usr/bin/env python

from flask import Blueprint, render_template
habit = Blueprint('habit',__name__)


@habit.route('/')
def index():
    api_list = {}
    return api_list


@habit.route('/get/')
def get(name=None):
    return render_template('cs.html', name=name)


# cs.html内容

<!doctype html>
<title>Hello from Flask</title>
<h1>{{ name }}：</h1>
<h1>Hello, World!</h1>

```

##### 请求 api 直接显示 html 源码

![](https://img-blog.csdnimg.cn/20210716141540306.png)

##### response 信息

![](https://img-blog.csdnimg.cn/20210716142411730.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2s0MTc2OTk0ODE=,size_16,color_FFFFFF,t_70)

原因分析：
=====

浏览器没有识别到 html 代码

解决方案：
=====

改 response 的 Content-type  
out_html = render_template(‘cs.html’, name=name)

*   return 多个值  
    `return out_html , "200 Ok", {"Content-type": "text/html"}`  
    return 后面的值说明：index.html 表示字符串，也就是网站页面显示的内容  
    ‘200 ok’ 表示状态码和对状态码的解析  
    {“Content-type”:“text/html”} 表示请求头  
    请求头不是必须要的，但是前面两个必须要，前提你是 retrun 多个值，还有就是顺序不能改变。
    
*   return make_response  
    `return out_html , 200, {"Content-type": "text/html"})`  
    make_response 它可以传递三个参数 第一个还是一个字符串，第二个传状态码，第三个传请求头，
    
*   return Response 对象  
    response = Response()  
    response.data= out_html  
    response.headers = {“Content-type”: “text/html”}  
    return response
    

三种方法都试过，都是可以的  
这里贴一下最简单的 return 多个值 的结果

```
# habit.py内容
# -*- coding: utf-8 -*-
# !/usr/bin/env python

from flask import Blueprint, render_template
habit = Blueprint('habit',__name__)


@habit.route('/')
def index():
    api_list = {}
    return api_list


@habit.route('/get/')
def get(name=None):
    out_html = render_template('cs.html', name=name)
    return out_html , "200 Ok", {"Content-type": "text/html"}

```

##### 请求 api 直接显示 html 源码

![](https://img-blog.csdnimg.cn/202107161426582.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2s0MTc2OTk0ODE=,size_16,color_FFFFFF,t_70)

##### response 信息

![](https://img-blog.csdnimg.cn/20210716142728224.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2s0MTc2OTk0ODE=,size_16,color_FFFFFF,t_70)