---
layout: post
title:  "mybatis参数解析逻辑"
date:   2018-01-31 11:21 +0800
categories: jekyll update
---

我们的sql操作往往需要使用参数，那么我们在使用mybatis的时候给方法提供的参数是如何被解析的呢？
* 整个解析过程分为方法形参解析和实参映射两部分。
* 当我们调用一个mapper的方法时，mybatis做的第一步是进行实参转换，生成一个通用的参数对象。
* 解析过程用到了ParamNameResolver对象，发生在ParamNameResolver在预处理Method的时候。
* ParamNameResolver遍历方法参数：
  1. 如果参数是ResultHandler或者RowBonds，跳过。
  2. 如果参数有param注解，取得注解值作为name。
  3. 如果没有param注解，则使用形参名作为name。
  4. 如果没有形参，则使用**有效**参数序号作为name。
  5. 所有的参数存储到一个参数序号->name的 TreeMap中。使用TreeMap的原因是TreeMap可以保证数据按照参数的顺序有序。
* 通过实参生成参数对象时，如果没有实参，返回一个Null，否则取得前面所讲的TreeMap，遍历这个TreeMap，按照name->实参数组[参数序号]的结构把数据放到一个ParamMap中，解析得到的参数对象就是这个ParamMap。
 
 **参数对象的使用我们将在方法调用中介绍**
