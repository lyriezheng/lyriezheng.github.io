---
layout: post
title:  "mybatis执行器"
date:   2018-02-01 15:48 +0800
categories: mybatis
---

Mybatis 四大对象：**Executor、StatementHandler、ResultHandler、ParameterHandler**。
先让我们看看MyBatis执行器的类图结构。
![executor]({{"/assert/images/Executor.png"}})
默认使用的是`SimpleExecutor`，使用较多的是`SimpleExecutor`、`BatchExecutor`和`ReuseExecutor`。
使用了模板的设计模式，在BaseExecutor搭建了运行框架，每个子类负责实现核心功能。
>数据库的`insert，delete，update`都调用了`Exectutor`的update方法。
BatchExecutor会批量执行`update`操作，缓存update操作，如果与上一个batch的update操作完全相同，则不进行`statementHandler`的`prepare`了，将批量操作的sql加入执行列表中并在在`commit`后一起执行到数据库中。

>`BatchExecutor`的问题是在`Insert`的时候，在事务没有提交前，是没有办法获取到自增id的。

>`ReuseExecutor`可以重用预处理语句，也就是把生成的`Statement`缓存起来，即当获取`Statement`时先去缓存中查找。缓存获取的条件是，sql语句相同且statement已经关闭。

>缓存键名由mapper方法名，Rowbounds的offset，limit，sql语句，sql参数，及Environment id通过计算得hashcode而来。

>Executor的缓存在执行update方法后会清空刷新。






