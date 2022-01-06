> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/weixin_42319496/article/details/119371840)

**目录**

[一、快速理解](#t0)

[1、有限状态机 (FSM)](#t1)

[2、有限状态机的设计](#t2)

[二、详细说明](#t3)

[1、有限状态机 FSM](#t4)

[1)FSM 概念](#t5)

[2)FSM 的 3 特点](#t6)

[3)FSM 的 4 要素](#t7)

[4)FSM 状态转换图](#t8)

[2、FSM 的设计和实现](#t9)

[1) 设计思路](#t10)

[2) 两种实现方式](#t11)

[3、通用 FSM 的设计](#t12)

一、快速理解
======

[有限状态机 (FSM) 的设计与实现(一)](https://www.cnblogs.com/chencheng/archive/2012/06/25/2562660.html)

1、有限[状态机](https://so.csdn.net/so/search?q=%E7%8A%B6%E6%80%81%E6%9C%BA) (FSM)
----------------------------------------------------------------------------

有限状态机 (FSM) 是表示有限个状态及在这些状态之间的转移和动作等行为的数学模型，在计算机领域有着广泛的应用。通常 FSM 包含几个要素：状态的管理、状态的监控、状态的触发、状态触发后引发的动作。

2、[有限状态机的设计](https://www.cnblogs.com/chencheng/archive/2012/06/25/2562660.html)
-------------------------------------------------------------------------------

本文主要阐述一下状态机的几种设计方法。

1：switch case/if else 设计方法

```
curEvent = getEvent();
curState = getCurState();
switch(curState)
{
    case state1:
    {
        switch(curEvent )
        {
             TODO...
             setCurState();
             break; 
        }
        break;
    }
    ...
}
```

这种设计方法最简单，通过一大堆判断来处理，适合小规模的状态切换流程，但如果规模扩大难以扩展和维护。

2：基于表结构的状态机设计方法：建立相应的状态表和动作查询表，根据状态表、事件、动作表定位相应的动作处理函数，执行完成后再进行状态的切换。

一个通用的状态机处理模块的设计如下：

```
/*状态表注册*/
void FSM_Regist(FSM_T* pFsm,STATE_TABLE_S* pStateTable)
{
   pFsm->FsmTable =  pStateTable;
   return;
}
/*状态迁移*/
void FSM_MoveState(FSM_T* pFsm,int state)
{
   pFsm->curState = state;
   return;
}
/*事件处理*/
void FSM_EventHandle(FSM_T* pFsm,int event)
{
    ACT_TABLE_T* pActTable = NULL;
    ActFun eventActFun = NULL;
    /*获取当前状态动作表*/
    pActTable = FSM_getActTable(pFsm);
    /*获取当前动作函数*/
    for(int i=0;i<MAX_ACT_NUM;i++)
    {
        if(event == pActTable[i].event)
        {
            eventActFun = pActTable[i].eventActFun;
            break;
        }
    }
    /*动作执行*/
    if(eventActFun)
    {
        eventActFun(pFsm);
    }
}
```

假设我们的状态图如下：

![](https://img-blog.csdnimg.cn/img_convert/f47deca7fd35229ec390596fff05a3a3.png)

  
相应的状态机设置如下：

```
/*状态1的动作表*/
ACT_TABLE_T state1ActTable[] = {
    {EVENT1,state1Event1Fun},
    {EVENT3,state1Event3Fun},
};
/*状态2的动作表*/
ACT_TABLE_T state2ActTable[] = {
    {EVENT2,state2Event2Fun},
};
/*状态表*/
STATE_TABLE_T FsmTable[] = {
    {STATE1,state1ActTable},
    {STATE2,state2ActTable},
};
 
int main(int argc, _TCHAR* argv[])
{
    FSM_T fsm;
    /*状态表注册*/
    FSM_Regist(&fsm,FsmTable);
    FSM_MoveState(&fsm,STATE1);
    FSM_EventHandle(&fsm,EVENT1);
    FSM_EventHandle(&fsm,EVENT2);
    return 0;
}
/*客户端提供的状态处理函数*/
void state1Event1Fun(void* pFsm)
{
    FSM_MoveState((FSM_T*)pFsm,STATE2);
    return;
}
void state1Event3Fun(void* pFsm)
{
    FSM_MoveState((FSM_T*)pFsm,STATE3);
    return;
}
void state2Event2Fun(void* pFsm)
{
    FSM_MoveState((FSM_T*)pFsm,STATE3);
    return;
}
```

通过设计一个通用的基于表结构的状态机模块，针对不同的状态图，我们只需要根据状态图得到其状态表结构，然后通过 FSM_Regist 注册，就可以方便的使用了状态机的功能了。这种机制便于我们添加新的状态流程，并且可以很好的进行分层状态机的设计。

二、详细说明
======

1、有限状态机 FSM
-----------

现实问题

在平时的工作中，有时候我们会遇到需要处理诸多状态的情况，譬如订单状态，简单罗列就有 12 种状态

```
"0": "交易关闭",
"1": "待支付",
"2": "支付确认中",
"3": "支付中",
"4": "待收货",
"5": "交易成功", // 已签收
"-1": "交易关闭", // 订单已取消
"-2": "交易关闭,退款中",
"-3": "交易关闭,已退款",
"-4": "订单异常",
"-5": "退货退款中",
"-6": "已退货退款"
```

而且这些状态之间存在联系。比如待支付状态下一个可能是支付确认中，可能是交易关闭，也可能是交易成功。

如何处理这么多的状态以及之间的关系，我们需要一个状态管理的机制，来统一处理，这里就需要 FSM.

### 1)FSM 概念

FSM(finite-state machine): 表示有限个状态以及在这些状态之间的转移和动作等行为的数学模型。

### 2)FSM 的 3 特点

1.  一个时刻，只有一个状态。
2.  某种条件下，会从一种状态转变 (transition) 到另一种状态。(比如待支付的下个状态是支付成功)
3.  状态有限 (finite)

### 3)FSM 的 4 要素

1.  currentState(当前状态)
2.  nextState(下一个状态)
3.  transition(currentState,action)。当前状态和下个状态的关系。接收当前的状态，根据不同的 action 返回不同的 nextState.
4.  action.state 变动的 event

![](https://img-blog.csdnimg.cn/img_convert/7e343bc80ad2a74203592fab33fb14ca.png)

### **4)FSM** 状态转换图

假设一个状态机的输入是 字符：

有限状态机, 也称为 **FSM(Finite State Machine)**，其在任意时刻都处于有限状态集合中的某一状态。当其获得一个**输入字符**时，将从**当前状态**转换到**另一个状态**，或者仍然**保持在当前状态**。任何一个 FSM 都可以用状态转换图来描述，图中的节点表示 FSM 中的一个状态，有向加权边表示输入字符时状态的变化。如果图中不存在与当前状态与输入字符对应的有向边，则 FSM 将进入 **“消亡状态** **(Doom State)**”，此后 FSM 将一直保持 “消亡状态”。状态转换图中还有两个特殊状态：状态 1 称为 **“起始状态”**，表示 FSM 的初始状态。状态 6 称为 **“结束状态”**，表示成功识别了所输入的字符序列。

在启动一个 FSM 时，首先必须将 FSM 置于 “起始状态”，然后输入一系列字符，最终，FSM 会到达“结束状态” 或者“消亡状态”。

![](https://img-blog.csdnimg.cn/img_convert/ff7e47fc336f1fd5737bc28be5d1107b.png)

**说明：**

在通常的 FSM 模型中，一般还存在一个 “接受状态”，并且 FSM 可以从“接受状态” 转换到另一个状态，只有在识别最后一个输入后，才会根据最终状态来决定是否接受所输入的字符。此外，也可以将 “其实状态” 也作为接受状态，因此空的输入序列也是可以接受的。

2、FSM 的设计和实现
------------

### **1) 设计思路**

程序设计思路大致如下：

*   使用状态转换图描述 FSM
*   状态转换图中的结点对应不同的状态对象
*   每个状态对象通过一个输入字符转换到另一个状态上，或者保持原状态不变。

通过输入字符从一个状态切换到另一个状态的过程，我们称之为一个**映射**。在计算机程序设计中，我们可以有两种表示映射的方法：

*   通过算法表示，即 “可执行代码(Executable Code)” 方式
*   通过一张映射表，即 “被动数据(Passive Data)” 方式

### **2) 两种实现方式**

如下详细介绍这两种实现方式：

*   **通过 Executable Code 实现映射的 FSM：(if 或者 switch)**
    

这种方式主要是通过条件分支来处理不同的字符，如 if 或者 switch 语句块，如

```
State* State1::Transition(char c)
{
    switch(c)
    {
    case 'A':
        return &s2;
    case 'B':
        return &s3;
    case 'C':
        return &s4;
    case 'D':
        return &s5;
    case '\0':
        return NULL;
    default:
        return NULL;
    }
}
```

```
// fsm_with_executable_code.h
#ifndef FSM_WITH_EXECUTABLE_CODE_H
#define FSM_WITH_EXECUTABLE_CODE_H
 
#include <string.h>
 
class State
{
public:
    virtual State* Transition(char c) = 0;
};
 
class Fsm
{
public:
    Fsm();
    void Reset();            // move to start state
    void Advance(char c);    // advance one transition
    int EndState();
    int DoomState();
 
private:
    State* p_current;   // &s1, &s2, ..., &s6; NULL ==> doom
};
 
 
class State1 : public State
{
public:
    State* Transition(char c);
};
 
class State2 : public State
{
public:
    State* Transition(char c);
};
 
class State3 : public State
{
public:
    State* Transition(char c);
};
 
class State4 : public State
{
public:
    State* Transition(char c);
};
 
class State5 : public State
{
public:
    State* Transition(char c);
};
 
class State6 : public State
{
public:
    State* Transition(char c);
};
 
#endif // FSM_WITH_EXECUTABLE_CODE_H
 
// fsm_with_executable_code.cc
#include "fsm_with_executable_code.h"
 
State1 s1;
State2 s2;
State3 s3;
State4 s4;
State5 s5;
State6 s6;
 
Fsm::Fsm()
{
    p_current = NULL;
}
 
void Fsm::Reset()
{
    p_current = &s1;
}
 
void Fsm::Advance(char c)
{
    if (p_current != NULL)
        p_current = p_current->Transition(c);
}
 
int Fsm::EndState()
{
    return p_current == &s6;
}
 
int Fsm::DoomState()
{
    return p_current == NULL;
}
State* State1::Transition(char c)
{
    switch(c)
    {
    case 'A':
        return &s2;
    case 'B':
        return &s3;
    case 'C':
        return &s4;
    case 'D':
        return &s5;
    case '\0':
        return NULL;
    default:
        return NULL;
    }
}
 
State* State2::Transition(char c)
{
    switch(c)
    {
    case 'E':
        return &s2;
    case 'I':
        return &s6;
    case '\0':
        return NULL;
    default:
        return NULL;
    }
}
 
State* State3::Transition(char c)
{
    switch(c)
    {
    case 'F':
        return &s3;
    case 'M':
        return &s4;
    case 'J':
        return &s6;
    case '\0':
        return NULL;
    default:
        return NULL;
    }
}
 
State* State4::Transition(char c)
{
    switch(c)
    {
    case 'G':
        return &s4;
    case 'K':
        return &s6;
    case '\0':
        return NULL;
    default:
        return NULL;
    }
}
 
State* State5::Transition(char c)
{
    switch(c)
    {
    case 'O':
        return &s2;
    case 'H':
        return &s5;
    case 'L':
        return &s6;
    case 'N':
        return &s4;
    case '\0':
        return NULL;
    default:
        return NULL;
    }
}
 
State* State6::Transition(char c)
{
    return NULL;
}
 
// test_with_executable_code.cc
#include "fsm_with_executable_code.h"
 
#include "stdio.h"  // printf, scanf
#include "stdlib.h" // system
 
void test_fsm()
{
    char input_string[80];
    printf("Enter input expression: ");
    scanf("%s", input_string);
 
    Fsm fsm;
    fsm.Reset();
    int index = 0;
    fsm.Advance(input_string[index++]);
 
    while (!fsm.EndState() && !fsm.DoomState())
        fsm.Advance(input_string[index++]);
 
    if (fsm.EndState())
        printf("\nValid input expression");
    else
        printf("\nInvalid input expression");
}
 
int main()
{
    test_fsm();
 
    system("pause");
}
```

*    **通过 Passive Data 实现映射的 FSM：(表)**
    

在如上的 switch 分支中，其使用类型大致相同，因此，我们可以考虑将相似的信息保存到一张表中，这样就可以在程序中避免很多函数调用。在每个状态中都使用一张转换表来表示映射关系，转换表的索引使用输入字符来表示。此外，由于通过转换表就可以描述不同状态之间的变化，那么就没有必要将每种状态定义为一个类了，即不需要多余的继承和虚函数了，仅使用一个 State 即可。

```
#include <limits.h>
class State
{
public:
    State();
    State* transition[range];
};
 
对于任意一个状态state和输入字符c，后续状态都可以通过state.transition[c]来确定。
 
类Fsm中的成员state包含6个状态，为了对应方便，我们将结束状态放在state[0]中，每个状态都使用一个三元组 { 当前状态，输入字符，下一个状态 } 来表示：
struct TransGraph   // use triple to describe map
{
    int current_state;
    char input_char;
    int next_state;
};
```

如此，使用了转换表代替了虚函数，简化了程序的设计。

```
// fsm_with_passive_data.h
#ifndef FSM_WITH_PASSIVE_DATA_H
#define FSM_WITH_PASSIVE_DATA_H
 
#include <string.h>
#include <limits.h>     // CHAR_MAX
 
const int range = CHAR_MAX + 1;
 
class State
{
public:
    State();
    State* transition[range];
};
 
struct TransGraph   // use triple to describe map
{
    int current_state;
    char input_char;
    int next_state;
};
 
class Fsm
{
public:
    Fsm();
    void Reset();            // move to start state
    void Advance(char c);    // advance one transition
    int EndState();
    int DoomState();
 
private:
    State* p_current;   // &s1, &s2, ..., &s6; NULL ==> doom
    State state[6];     // 6 states, state[0] is end state
};
 
 
#endif // FSM_WITH_PASSIVE_DATA_H
 
// fsm_with_passive_data.cc
#include "fsm_with_passive_data.h"
 
State::State()
{
    for (int i = 0; i < range; ++i)
        transition[i] = NULL;
}
 
Fsm::Fsm()
{
    static TransGraph graph[] =
    {
        {1, 'A', 2}, {1, 'B', 3}, {1, 'C', 4}, {1, 'D', 5},
        {2, 'E', 2}, {2, 'I', 0},
        {3, 'F', 3}, {3, 'J', 0}, {3, 'M', 4},
        {4, 'G', 4}, {4, 'K', 0},
        {5, 'H', 5}, {5, 'L', 0}, {5, 'O', 2}, {5, 'N', 4},
        {0, 0, 0}
    };
 
    for (TransGraph* p_tg = graph; p_tg->current_state != 0; ++p_tg)
        state[p_tg->current_state].transition[p_tg->input_char] = &state[p_tg->next_state];
 
    p_current = NULL;
}
 
void Fsm::Reset()
{
    p_current = &state[1];
}
 
void Fsm::Advance(char c)
{
    if (p_current != NULL)
        p_current = p_current->transition[c];
}
 
int Fsm::EndState()
{
    return p_current == &state[0];
}
 
int Fsm::DoomState()
{
    return p_current == NULL;
}
 
// test_with_passive_data.cc
#include "fsm_with_passive_data.h"
#include "stdio.h"  // printf, scanf
#include "stdlib.h" // system
 
void test_fsm()
{
    char input_string[80];
    printf("Enter input expression: ");
    scanf("%s", input_string);
 
    Fsm fsm;
    fsm.Reset();
    int index = 0;
    fsm.Advance(input_string[index++]);
 
    while (!fsm.EndState() && !fsm.DoomState())
        fsm.Advance(input_string[index++]);
 
    if (fsm.EndState())
        printf("\nValid input expression");
    else
        printf("\nInvalid input expression");
}
 
 
int main()
{
    test_fsm();
 
    system("pause");
}
```

3、通用 FSM 的设计
------------

如果类 Fsm 可以表示任意类型的 FSM，那么就更符合程序设计的要求了。在构造函数中执行的**具体配置**应该被**泛化为一种机制**，我们通过这种机制来建立任意的 FSM。在 Fsm 的构造函数中，应该将**转换表**作为一个参数传入，**而非**包含**具体的转换表**，如此，则不需要将转换表的大小硬编码到 Fsm 中了。因此，在构造函数中必须动态地创建这个存放转换表的内存空间，在析构函数中记着销毁这块内存。

**具体配置** ------> **泛化 ----->** **一种机制**

**转换表**  ------> **泛化 ----->** **参数传入**

```
class Fsm
{
public:
    Fsm(TransGraph* p_tg);//原来的： Fsm();
    virtual ~Fsm();
    void Reset();
    void Advance(char c);
    int EndState();
    int DoomState();
 
private:
    State* p_current;
    State* p_state;   //原来的： State state[6];     // 6 states, state[0] is end state
};
 
Fsm::Fsm(TransGraph* p_tg)
{
    int max_state = 0;  // size for dynamically allocated graph
    for (TransGraph* p_temp = p_tg; p_temp->current_state != 0; ++p_temp)
    {
        if (p_temp->current_state > max_state)
            max_state = p_temp->current_state;
        if (p_temp->next_state > max_state)
            max_state = p_temp->next_state;
    }
 
    p_state = new State[max_state + 1];
    for (TransGraph* p_temp = p_tg; p_temp->current_state != 0; ++p_temp)
        p_state[p_temp->current_state].transition[p_temp->input_char] = &p_state[p_temp->next_state];
 
    p_current = NULL;
}
 
Fsm::~Fsm()
{
    delete []p_state;
}
```

```
// fsm_with_generalization.h
#ifndef FSM_WITH_GENERALIZATION_H
#define FSM_WITH_GENERALIZATION_H
 
#include <string.h>
#include <limits.h>     // CHAR_MAX
 
const int range = CHAR_MAX + 1;
 
class State
{
public:
    State();
    State* transition[range];
};
 
struct TransGraph
{
    int current_state;
    char input_char;
    int next_state;
};
 
class Fsm
{
public:
    Fsm(TransGraph* p_tg);
    virtual ~Fsm();
    void Reset();
    void Advance(char c);
    int EndState();
    int DoomState();
 
private:
    State* p_current;
    State* p_state;
};
 
 
#endif // FSM_WITH_GENERALIZATION_H
 
// fsm_with_generalization.cc
#include "fsm_with_generalization.h"
 
State::State()
{
    for (int i = 0; i < range; ++i)
        transition[i] = NULL;
}
 
Fsm::Fsm(TransGraph* p_tg)
{
    int max_state = 0;  // size for dynamically allocated graph
    for (TransGraph* p_temp = p_tg; p_temp->current_state != 0; ++p_temp)
    {
        if (p_temp->current_state > max_state)
            max_state = p_temp->current_state;
        if (p_temp->next_state > max_state)
            max_state = p_temp->next_state;
    }
 
    p_state = new State[max_state + 1];
    for (TransGraph* p_temp = p_tg; p_temp->current_state != 0; ++p_temp)
        p_state[p_temp->current_state].transition[p_temp->input_char] = &p_state[p_temp->next_state];
 
    p_current = NULL;
}
 
Fsm::~Fsm()
{
    delete []p_state;
}
 
void Fsm::Reset()
{
    p_current = &p_state[1];
}
 
void Fsm::Advance(char c)
{
    if (p_current != NULL)
        p_current = p_current->transition[c];
}
 
int Fsm::EndState()
{
    return p_current == &p_state[0];
}
 
int Fsm::DoomState()
{
    return p_current == NULL;
}
 
// test_with_generalization.cc
#include "fsm_with_generalization.h"
 
#include "stdio.h"  // printf, scanf
#include "stdlib.h" // system
 
void test_fsm()
{
    char input_string[80];
    printf("Enter input expression: ");
    scanf("%s", input_string);
 
    TransGraph graph[] =
    {
        {1, 'A', 2}, {1, 'B', 3}, {1, 'C', 4}, {1, 'D', 5},
        {2, 'E', 2}, {2, 'I', 0},
        {3, 'F', 3}, {3, 'J', 0}, {3, 'M', 4},
        {4, 'G', 4}, {4, 'K', 0},
        {5, 'H', 5}, {5, 'L', 0}, {5, 'O', 2}, {5, 'N', 4},
        {0, 0, 0}
    };
 
    Fsm fsm(graph);
    fsm.Reset();
    int index = 0;
    fsm.Advance(input_string[index++]);
 
    while (!fsm.EndState() && !fsm.DoomState())
        fsm.Advance(input_string[index++]);
 
    if (fsm.EndState())
        printf("\nValid input expression");
    else
        printf("\nInvalid input expression");
}
 
 
int main()
{
    test_fsm();
 
    system("pause");
}
```

当然也可以将上述程序中的转换表不放在主程序中，**而是由一个派生自 Fsm 的子类 SpecificFsm 提供，在 SpecificFsm 中设置具体的转换表，然后通过 SpecificFsm 的初始化列表传到基类 Fsm 中，这样在主程序中就可以使用 SpecificFsm 来进行操作了。**

原文：[https://www.cnblogs.com/chencheng/archive/2012/06/25/2562660.html](https://www.cnblogs.com/chencheng/archive/2012/06/25/2562660.html)

未消化：

使用 C++ 实现一套简单的状态机模型——实例：[https://fangliang.blog.csdn.net/article/details/44042287](https://fangliang.blog.csdn.net/article/details/44042287)

使用 C++ 实现一套简单的状态机模型——原理解析：[https://fangliang.blog.csdn.net/article/details/44120809](https://fangliang.blog.csdn.net/article/details/44120809)