---
title: Spring事务动态代理问题解析
description: 
    1.在项目实现中经常会出现各种层级调用然后事务混乱不知道会不会回滚，会不会影响其他任务处理事务<br>
    2.简单解释项目中使用SpringAOP代理事务的特点
tags:
  - 事务
categories:
  - - 学习
    - SpringAOP事务
date: 2020-01-16 16:48:43
---

### 项目中事务常见问题
在项目实现中经常会出现各种层级调用然后事务混乱不知道会不会回滚，会不会影响其他任务处理事务

### Spring事务基于AOP动态代理
Spring中事务是基于AOP实现的，AOP是基于动态代理，代理即代理模式，是基于类的。
例如我们自己项目中
```
<aop:config proxy-target-class="true">
    <aop:pointcut id="interceptorPointCuts"
        expression="execution(* com.b2bex.*.service.*Manager.*(..)) or execution(* com.autozi.*.*.service.*Manager.*(..))" />
    <aop:advisor advice-ref="txAdvice"
        pointcut-ref="interceptorPointCuts" />
</aop:config>
```
将Manager代码类都做为事务代理，其实就相当与每一个Manager类都是代理
则：有两个manager(A,B)，非代理的facade(C)
>A->B(自己处理异常并未抛给A)：不会回滚

>A->B(发生异常抛给A)：会回滚

>C(非代理)->A(发生异常抛给C)->B(发生异常抛给A)：A,B会回滚，但是C中的其他事务则不会回滚

一个代理类的开始就是一个事务，多层调用是同一个事务，同一个事务传播多少层manager,只要有一个`事务代理`抛出异常，事务管理器就会标记回滚，最终这个事务发现回滚标记就会回滚



