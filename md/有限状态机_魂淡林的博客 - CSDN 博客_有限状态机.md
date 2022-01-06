> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/u013528298/article/details/88948525)

**目录**

[1. 概念和术语](#t0)

 [2. 特征](#t1)

[3. 几个重要概念](#t2)

[4. 分层有限状态机](#t3)

[5. 状态机的实现方式](#t4)

[6.. 有限状态机的缺点](#t5)

[参考：](#t6)

**有限状态机**（英语：finite-state machine，[缩写](https://zh.wikipedia.org/wiki/%E7%B8%AE%E5%AF%AB)：**FSM**）又称**有限状态自动机**，简称**状态机**，是表示有限个[状态](https://zh.wikipedia.org/wiki/%E7%8A%B6%E6%80%81)以及在这些状态之间的转移和动作等行为的[数学模型](https://zh.wikipedia.org/wiki/%E6%95%B0%E5%AD%A6%E6%A8%A1%E5%9E%8B)。是一种用来进行对象行为建模的工具，其作用主要是描述对象在它的[生命周期](https://so.csdn.net/so/search?q=%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)内所经历的状态序列，以及如何响应来自外界的各种事件

### 1. 概念和术语

![](https://upload.wikimedia.org/wikipedia/commons/thumb/c/cf/Finite_state_machine_example_with_comments.svg/225px-Finite_state_machine_example_with_comments.svg.png) 图一

状态存储关于过去的信息，就是说：它反映从系统开始到现在时刻的输入变化。转移指示状态变更，并且用必须满足确使转移发生的条件来描述它。动作是在给定时刻要进行的活动的描述。**有多种类型的动作**：

进入动作（entry action）：在进入状态时进行

退出动作：在退出状态时进行

输入动作：依赖于当前状态和输入条件进行

转移动作：在进行特定转移时进行

FSM（有限[状态机](https://so.csdn.net/so/search?q=%E7%8A%B6%E6%80%81%E6%9C%BA)）可以使用上面图 1 那样的[状态图](https://zh.wikipedia.org/wiki/%E7%8A%B6%E6%80%81%E5%9B%BE)（或状态转移图）来表示。此外可以使用多种类型的[状态转移表](https://zh.wikipedia.org/wiki/%E7%8A%B6%E6%80%81%E8%BD%AC%E7%A7%BB%E8%A1%A8)。下面展示最常见的表示：当前状态（B）和条件（Y）的组合指示出下一个状态（C）。完整的动作信息可以只使用脚注来增加。包括完整动作信息的 FSM 定义可以使用[状态表](https://zh.wikipedia.org/w/index.php?title=%E8%99%9A%E6%8B%9F%E6%9C%89%E9%99%90%E7%8A%B6%E6%80%81%E6%9C%BA&action=edit&redlink=1)。

![](https://img-blog.csdnimg.cn/20190401151211689.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM1MjgyOTg=,size_16,color_FFFFFF,t_70)

###  2. 特征

 总的来说有三种特征：

*   状态总数（state）是有限的。
*   任一时刻，只处在一种状态之中。
*   某种条件下，会从一种状态转变（transition）到另一种状态。

### 3. 几个重要概念

*   状态（State）：表示对象的某种形态，在当前形态下可能会拥有不同的行为和属性。
*   转移（Transition）：表示状态变更，并且必须满足确使转移发生的条件来执行。
*   动作（Action）：表示在给定时刻要进行的活动。
*   事件（Event）：事件通常会引起状态的变迁，促使状态机从一种状态切换到另一种状态。

### 4. 分层有限状态机

简单说，就是状态太多时，不好维护，于是将状态分类，将同类的状态作为一个状态机，然后再做一个大的状态机，来维护这些子状态机。

比如：对小狗来说，我们可以把小狗的状态先定义为疲劳，开心，愤怒，然后这些状态里再定义小状态，比如在开心的状态中，有撒桥，摇尾巴等小状态，这样我们在外部只需要关心三个状态的跳转（疲劳，开心，愤怒）。如果不用 HFSM 的话，就会有很多跳转链接；如果用 HFSM 的话，在每个状态的内部只需要关心自己的小状态的跳转就可以了，这样就大大的降低了状态机的复杂度和跳转链接数。如下图：

![](https://img-blog.csdn.net/20180402235124307?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvQmlsbENZSg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 5. 状态机的实现方式

       有很多种方法来实现有限状态机，读者可能想到**使用一串的 if...else，或者加上枚举类型的 switch 来进行状态的判断**。的确，这样的方法是最直观的，也是最简单的实现方法。**在一个智能体的状态数量很少的情况**下，**使用这样的实现方法是可以的**。但是，当一个智能体的状态有很多个的时候，在这样实现，代码就会显得很臃肿，就会跟上一长串的 case 语句或者 if...else 语句。但是对于开发人员来说，将是一个噩梦。因为你不得不自己来维护这么一长串的代码，而且不易于调试和阅读。

        这里推荐使用的方法是使用状态设计模式来完成整个有限状态机的实现。（关于状态设计模式，是属于设计模式的范畴，读者可自行查阅相关的资料进行了解，推荐看 Head First 设计模式中关于状态设计模式的章节）

       有限状态机的一般实现是将每个状态写成类，再用一个载体（也就是所谓的状态机）管理这些状态的切换。

### 6.. 有限状态机的缺点

*   各个状态类之间互相依赖很严重，耦合度很高。
*   结构不灵活，可扩展性不高，难以脚本化 / 可视化。

### 参考：

1. 维基百科 

2. [游戏 AI 之状态机](https://blog.csdn.net/i_dovelemon/article/details/10012911)

3. [游戏 AI 之 有限状态机、分层有限状态机、行为树](https://blog.csdn.net/BillCYJ/article/details/79797228)