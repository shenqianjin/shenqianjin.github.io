---
title: "Java Coding Standards"
date: 2025-07-27T23:00:00+08:00
draft: false
author: "shenqianjin"
authorLink: "https://shenqianjin.github.io/"
description: ""
license: ""
images: []
tags: [Java]
categories: [Java]
featuredImage: "cover.png"
featuredImagePreview: ""
---

## 代码为什么会变烂?
### 破窗效应（Broken windows theory）
没有修复的破窗，导致更多的窗户被打破。
### 惯性定律
好的代码会促生好的代码，糟糕的代码也会促生糟糕的代码。   
别低估了惯性的力量，没人想去整理糟糕的代码，同样没人想把完美的代码弄的一团糟。
### 技术债务 (Technical Debt)
开发团队在设计或架构选型时从短期效应的角度选择了一个易于实现的方案，但从长远来看，这种方案会带来更消极的影响，
亦即开发团队所欠的债务。 ———— Ward Cunningham

## 重构的定义
使用一系列的重构准则，对软件内部结构的一种调整，目的是在不改变软件之可察行为的前提下，提高可理解性，降低修改成本。

我写函数时，一开始都冗长而复杂，有太多缩进和嵌套循环，有过长的参数列表。名称是随意取的，也会有重复代码。 ———— Robert Martin

写代码和写别的东西很像，在写论文或文章时，你先想什么就写什么，然后再打磨它，初稿也许粗陋无序，你就斟酌推敲，直至达到你心目中的样子。 —— Robert Martin

我并不是一开始就按照这些规则写函数，我想没人做得到。  —— Robert Martin

任何一个傻瓜都能写出机器能懂的代码，好的程序员应该写出人能懂的代码。 —— Martin Fowler 《重构》

## 经典代码坏味道

- Duplicated Code 重复代码
- Long Method 过长方法
- Large Class 过长类
- Long Parameter List 过长参数列表
- Comments 注释过多
- Temporary Field 临时字段
- Primitive Obsession 基本类型偏执
- Switch Statements switch语句
- Divergent Change 发散式变化
- Shotgun Surgery 霰弹式修改
- Data Clumps 数据泥团
- Data Class 数据类
- Feature Envy 特性依念
- Refused Bequest 拒绝继承
- Message Chains 消息链
- Middle Man 中间人
- inappropriate intimacy 过度亲密
- Lazy Class 冗余类


## 引用
- 重构: https://www.refactoring.com/catalog/
- 代码的坏味道: https://cloud.tencent.com/developer/article/1835315
- https://chenzhigao.github.io/2023/05/code-bad-smell/
- https://gausszhou.github.io/refactoring2-zh/ch3.html#_3-7-%E5%8F%91%E6%95%A3%E5%BC%8F%E5%8F%98%E5%8C%96-divergent-change
- 

