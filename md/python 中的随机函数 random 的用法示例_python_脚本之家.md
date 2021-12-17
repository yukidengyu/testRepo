> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [www.jb51.net](https://www.jb51.net/article/133820.htm)

> 这篇文章主要介绍了 python 中的随机函数 random 的用法示例，详细的介绍了 python 随机函数 random 的用法和示例，具有一定的参考价值，感兴趣的小伙伴们可以参考一下

这篇文章主要介绍了 python 中的随机函数 random 的用法示例，详细的介绍了 python 随机函数 random 的用法和示例，具有一定的参考价值，感兴趣的小伙伴们可以参考一下

**一、random 模块简介**

Python 标准库中的 random 函数，可以生成随机浮点数、整数、字符串，甚至帮助你随机选择列表序列中的一个元素，打乱一组数据等。

**二、random 模块重要函数**

1 )、random() 返回 0<=n<1 之间的随机实数 n；

2 )、choice(seq) 从序列 seq 中返回随机的元素；  

<table><tbody><tr><td><p>1</p><p>2</p><p>3</p></td><td><p><code>import</code> <code>random</code></p><p><code>a </code><code>=</code> <code>random.choice([</code><code>1</code><code>, </code><code>2</code><code>, </code><code>3</code><code>, </code><code>4</code><code>])</code></p><p><code>print</code><code>(a)</code></p></td></tr></tbody></table>

3 )、getrandbits(n) 以长整型形式返回 n 个随机位；

4 )、shuffle(seq[, random]) 原地指定 seq 序列；

5 )、sample(seq, n) 从序列 seq 中选择 n 个随机且独立的元素；

**三、random 模块方法说明**

1.  random.random() 函数是这个模块中最常用的方法了，它会生成一个随机的浮点数，范围是在 0.0~1.0 之间。
2.  random.uniform() 正好弥补了上面函数的不足，它可以设定浮点数的范围，一个是上限，一个是下限。
3.  random.randint() 随机生一个整数 int 类型，可以指定这个整数的范围，同样有上限和下限值，python random.randint。
4.  random.choice() 可以从任何序列，比如 list 列表中，选取一个随机的元素返回，可以用于字符串、列表、元组等。
5.  random.shuffle() 如果你想将一个序列中的元素，随机打乱的话可以用这个函数方法。
6.  random.sample() 可以从指定的序列中，随机的截取指定长度的片断，不作原地修改。

**四、需要导入 random 模块**

1、random.random

random.random() 用于生成一个 0 到 1 的随机符小数: 0 <= n < 1.0

2、random.uniform

random.uniform 的函数原型为：random.uniform(a, b)，用于生成一个指定范围内的随机符点数，两个参数其中一个是上限，一个是下限。如果 a > b，则生成的随机数 n: a <= n <= b。如果 a <b， 则 b <= n <= a。

<table><tbody><tr><td><p>1</p></td><td><p><code>&gt;&gt;&gt; random.uniform(</code><code>1</code><code>, </code><code>10</code><code>)</code></p></td></tr></tbody></table>

![](https://img.jbzj.com/file_images/article/202104/2021416141955635.png)

3、random.randint

random.randint() 的函数原型为：random.randint(a, b)，用于生成一个指定范围内的整数。其中参数 a 是下限，参数 b 是上限，生成的随机数 n: a <= n <= b

<table><tbody><tr><td><p>1</p></td><td><p><code>&gt;&gt;&gt; random.randint(</code><code>10</code><code>, </code><code>100</code><code>)</code></p></td></tr></tbody></table>

4、random.randrange

random.randrange 的函数原型为：random.randrange([start], stop[, step])，从指定范围内，按指定基数递增的集合中 获取一个随机数。如：random.randrange(10, 100, 2)，结果相当于从 [10, 12, 14, 16, ... 96, 98] 序列中获取一个随机数。random.randrange(10, 100, 2)在结果上与 random.choice(range(10, 100, 2) 等效。

随机选取 0 到 100 间的偶数：   

<table><tbody><tr><td><p>1</p><p>2</p></td><td><p><code>&gt;&gt;&gt; </code><code>import</code> <code>random</code></p><p><code>&gt;&gt;&gt; random.randrange(</code><code>0</code><code>, </code><code>101</code><code>, </code><code>2</code><code>)</code></p></td></tr></tbody></table>

5、random.choice

random.choice 从序列中获取一个随机元素。其函数原型为：random.choice(sequence)。参数 sequence 表示一个有序类型。这里要说明 一下：sequence 在 python 不是一种特定的类型，而是泛指一系列的类型。list, tuple, 字符串都属于 sequence。有关 sequence 可以查看 python 手册数据模型这一章。

<table><tbody><tr><td><p>1</p><p>2</p><p>3</p><p>4</p><p>5</p></td><td><p><code>&gt;&gt;&gt; random.choice(</code><code>'abcdefg&amp;#%^*f'</code><code>) #随机字符</code></p><p><code>'d'</code></p><p><code>&gt;&gt;&gt; random.choice ( [</code><code>'apple'</code><code>, </code><code>'pear'</code><code>, </code><code>'peach'</code><code>, </code><code>'orange'</code><code>, </code><code>'lemon'</code><code>] )</code></p><p><code>'lemon'</code></p></td></tr></tbody></table>

6、random.shuffle

random.shuffle 的函数原型为：random.shuffle(x[, random])，用于将一个列表中的元素打乱。如:

<table><tbody><tr><td><p>1</p><p>2</p><p>3</p><p>4</p><p>5</p><p>6</p></td><td><p><code>p </code><code>=</code> <code>[</code><code>"Python"</code><code>, </code><code>"is"</code><code>, </code><code>"powerful"</code><code>, </code><code>"simple"</code><code>, </code><code>"and so on..."</code><code>]</code></p><p><code>random.shuffle(p)</code></p><p><code>print</code> <code>p</code></p><p><code>p </code><code>=</code> <code>[</code><code>"Python"</code><code>, </code><code>"is"</code><code>, </code><code>"powerful"</code><code>, </code><code>"simple"</code><code>, </code><code>"and so on..."</code><code>]</code></p></td></tr></tbody></table>

7、random.sample

random.sample 的函数原型为：random.sample(sequence, k)，从指定序列中随机获取指定长度的片断。sample 函数不会修改原有序列。 如果 k 大于 sequence 元素个数的话会报错。   

以上就是本文的全部内容，希望对大家的学习有所帮助，也希望大家多多支持脚本之家。