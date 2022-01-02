> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/benjonc/article/details/79870947)

有限[状态机](https://so.csdn.net/so/search?q=%E7%8A%B6%E6%80%81%E6%9C%BA)在维基百科中的解释是：

有限状态机（英语：finite-state machine，[缩写](https://zh.wikipedia.org/wiki/%E7%B8%AE%E5%AF%AB)：FSM）又称有限状态自动机，简称状态机，是表示有限个[状态](https://zh.wikipedia.org/wiki/%E7%8A%B6%E6%80%81)以及在这些状态之间的转移和动作等行为的[数学模型](https://zh.wikipedia.org/wiki/%E6%95%B0%E5%AD%A6%E6%A8%A1%E5%9E%8B)。

至于为什么要学 FSM，FSM 可以把模型的多状态、多状态间的转换条件解耦。可以使维护变得容易，代码也更加具有可读性，也更加艺术。

理解有限状态机首先需要理解整个模型。我们把灯的开关作为示例，先看图：

![](https://img-blog.csdn.net/2018040919143227)

通过抽象化，我们提取出了其中几个关键点：

1. 状态

2. 动作

3. 状态过渡

4. 转换事件

5.FSM

首先分为状态和动作，一般状态都会对应一个动作，所以我们把状态和动作放在状态里面，然后状态过渡和过渡事件放在状态过

渡里面，最后再由状态机驱动。于是一个状态机模型就出来了，状态、状态过渡、状态机。

**他们三个之间的关系是：由状态机管理和驱动状态，然后由状态和状态内的状态过渡**

**确定每个状态之间的转换。（重点、**重点、重点**）**

其中我们最容易忽略的一点就是状态过渡，这个是最不容易想到和抽象出来的一个对象。为什么要把状态过渡抽象出来呢？因为状态过渡就是每个状态之间互相转换的条件，这个抽象出来后，以后就可以解耦每个状态之间的转换条件，少写许多 if - else ，并且会好维护许多。

我们现在开始设计整个状态机。

（一）设计状态机的接口
===========

属性：
---

    名字 (string Name{get;})

    标记 (string Tag{get;})

    运行时间 (float Timer{get;})

    状态转换容器 (List<ITransition> Transitions{get;})

    状态机父节点 (IStateMachine Parent;)

方法：
---

    进入状态 (void OnEnter(IState prev));

    离开状态 (void OnExit(IState next));

    持续执行状态 (每帧更新)(void OnUpdate(float delta))

    添加状态 (void AddTransition(ITransition t));

    设置父状态机 (void SetStateMachine(IStateMachine machine));

```
public interface IState
    {
        float Timer { get; }
        string Name { get; }
        string Tag { get; set; }
        void OnEnter(IState prev);
        void OnExit(IState next);
        void OnUpdate(float deltaTime);
        List<ITransition> Transitions { get; }
        void AddTransition(ITransition transition);
        IStateMachine Parent { get; }
        void SetStateMachine(IStateMachine stateMachine);
    }
```

(二) 设计状态转换接口
============

属性：
---

    目标状态 (IState ToState{get;})

    原状态 (IState FromState{get;})

    名字 (string Name{get;})

方法：
---

    检测条件 (bool OnCheck();)

    转换回调 (bool OnCompleteCallback();)

```
public interface ITransition
    {
        string Name { get; }
        IState FromState { get; }
        IState ToState { get; }
        bool OnCheck();
        bool OnCompleteCallBack();
    }
```

(三) 设计状态机接口
===========

属性：

    默认状态 (IState DefaultState{get;})

    当前状态 (IState CurState{get;})

    状态容器 (Dictionary<string, IState> States{get;})

方法：

    添加状态 (void AddState(IState state));

    移除状态 (void RemoveState(IState state));

    获取状态 (IState GetState(string name));

```
public interface IStateMachine
    {
        IState CurState { get; }
        IState DefaultState { get; }
        void AddState(IState state);
        void RemoveState(IState state);
        void SetCurState(IState state);
        IState GetStateWithTag(string tag);
        IState GetStateWithName(string name);
        Dictionary<string, IState> States { get; }        
    }
```

先写在这里，后续再添加。gitee 上已经把案例发上去了，着急使用和学习的朋友可以去上面提取。用的 C# 和 Unity。基本原理明白了，哪里都好用。欢迎大家提出意见和建议，虚心接受。

[gitee 的链接](https://gitee.com/chenbojun/JyFSM)