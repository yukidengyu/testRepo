> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/FloatDreamed/article/details/84137245?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522164113644316780357283021%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fblog.%2522%257D&request_id=164113644316780357283021&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_ecpm_v1~rank_v31_ecpm-20-84137245.nonecase&utm_term=%E6%9C%89%E9%99%90%E7%8A%B6%E6%80%81%E6%9C%BA)

```
#   _*_ coding:utf-8 _*_
from random import randint
from time import sleep
 
__author__ = 'admin'
 
'''
    23种设计模式之状态模式：有限状态机
    下面介绍了灯的三种状态：
    1.开着
    2.灭着
    3.坏了
    其中"坏了"的状态下，若是修好了，必然进入灭着的状态
'''
 
#   Light类
class Light(object):
    def __init__(self, name):
        self.name = name
        self.state = ""
        self.fsm = None
 
    #   开灯方法
    def lightOn(self):
        #   打印语句代替行为
        print("灯亮着[%s]" % self.name)
 
    #   关灯方法
    def lightOff(self):
        print("灯灭着[%s]" % self.name)
 
    #   坏灯方法
    def lightBroken(self):
        print("灯坏了[%s]" % self.name)
 
    #   绑定状态机
    def bind(self, state, fsm):
        self.state = state
        self.fsm = fsm
 
 
# 定义状态
class State(object):
    def exec(self, obj):
        pass
 
    def exit(self, obj):
        pass
 
 
# 定义开灯状态，继承State(这里就是写进入该状态时该干那些事情，退出该状态时该干那些事情)
class OnState(State):
    #   进入还状态无非就是，将灯做打开操作
    def exec(self, obj):
        obj.lightOn()
 
    #   退出该状态，无非就是，将灯做关闭操作
    def exit(self, obj):
        obj.lightOff()
 
 
# 定义关灯状态，继承State
class OffState(State):
    def exec(self, obj):
        obj.lightOff()
 
    def exit(self, obj):
        obj.lightOn()
 
 
# 定义坏灯状态，继承State
class BrokenState(State):
    def exec(self, obj):
        obj.lightBroken()
 
    def exit(self, obj):
        obj.lightOff()
 
 
#   状态机管理器，这里主要做一些转换状态的操作
class StateMachine(object):
    def __init__(self):
        #   指明信号和各状态的对应关系
        self.states = {0: OnState(), 1: OffState(), 2: BrokenState()}
 
    #   通过给定的state返回其对应的状态对象
    def getFsm(self, state):
        return self.states[state]
 
    #   切换对象的状态
    def changeState(self, NewState, objs):
        for obj in objs:
            #   如果新产生的信号和对象原本的执行状态对应的信号一致的话
            if NewState == obj.state:
                #   保持原状态
                fsm = self.getFsm(obj.state)
                fsm.exec(obj)
            else:
                #   先退出旧状态
                old_fsm = self.getFsm(obj.state)
                old_fsm.exit(obj)
                #   执行新状态
                obj.state = NewState
                new_fsm = self.getFsm(NewState)
                new_fsm.exec(obj)
 
 
def run():
    sm = StateMachine()
    lights = []
    names = ['red', 'green', 'yellow']
    #   对象的制造机
    for i in range(1):
        l = Light(names[i])
        l.bind(0, sm.getFsm(0))
        lights.append(l)
    while True:
        #   生成新的信号
        state = randint(0, 2)
        sm.changeState(state, lights)
        sleep(2)
run()
```