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

### 代码的坏味道 - 《重构2》
| Bad Smells in Code | 表现 | 重构方法 | 使用模式重构 |
|---|---|---|---|
| Mysterious Name / 神秘命名 | 变量、函数或类名称模糊，无法清晰表达意图。 | 使用重命名（Rename）重构，确保名称自解释。 |       |
| Duplicated　Code / 重复代码 | 相同或相似代码出现在多个地方。 | 提炼函数（Extract Method）、提炼超类（Extract Superclass）、方法上移、替换算法、链构造方法 | 构造Template Method、以Composite取代一/多之分、引入Null Object、用Adapter统一接口、用FactoryMethod引入多态创建 |
| Long　Function / 过长函数   | 函数代码过长，难以理解逻辑。 | 分解为小函数、提取方法、组合方法、以查询取代临时变量、引入参数对象、保持对象完整 | 转移聚集操作到Vistor、以Strategy取代条件逻辑、以Command取代条件调度程序、转移聚集操作到CllectingParameter |
| Long　Parameter List / 过长参数列表 | 函数参数过多，调用和维护困难。 | 以方法取代参数、引入参数对象、保持对象完整 |       |
| Global　Data / 全局数据     | 全局变量、类变量或单例被随意修改，引发副作用；从代码库的任何一个角落都可以修改它，而且没有任何机制可以探测出到底哪段代码做出了修改。 | 封装变量（Encapsulate Variable），限制作用域 |       |
| Mutable　Data / 可变数据    | 数据被多处修改，导致不可预测的行为；对数据的修改经常导致出乎意料的结果和难以发现的 bug | 封装变量、拆分变量、限制可变性（例如使用不可变对象）、移动语句、提炼函数、将查询函数和修改函数分离、尽早使用移除设值函数、以查询取代派生变量、用函数组合成类、函数组合成变换、将引用对象改为值对象 |       |
| Divergent　Change / 发散式变化 | 一个类因不同原因频繁修改；类经常因为不同的原因在不同方向上发生变化，显然违反了单一职责原则 | 拆分阶段（Split Phase），分离不同职责，提取类 |       |
| Shotgun　Surgery / 霰弹式修改 | 修改一处代码需联动修改多个地方。 | 搬移函数（Move Method）、搬移字段、或内联函数（Inline Function） | 将创建知识搬移到Factory |
| Feature　Envy / 依恋情结    | 函数过度访问另一个类的数据。方法对于某个类的兴趣高过对自己所处的宿主类 | 转移方法、或提炼类（Extract Class）、提取方法 | 引入Strategy、引入Visitor |
| Data　Clumps / 数据泥团     | 多个数据项总是一起出现（如参数、类字段）。 | 提取类、引入参数对象、保持对象完整 |       |
| Primitive　Obsession / 基本类型偏执 | 过度使用基本类型（如字符串、数字）代替对象。 | 以对象取代数据值、以类型取代类型代码、以子类取代类型代码、提取类、引入参数对象、以对象取代数组 | 以State取代状态改变条件语句、以Strategy取代条件逻辑、以Composite取代隐含树、以Interpreter取代隐式语句、转移装饰功能到Decorator、用Builder封装Composite |
| Repeated　Switches / 重复的switch |相同switch逻辑分散在多处。 | 以多态取代条件表达式（Replace Conditional with Polymorphism）。 |       |
| Loops　/ 循环语句           | 复杂循环逻辑难以理解。 | 以管道取代循环（Replace Loop with Pipeline，使用map、filter等）。 |       |
| Lazy　Element / 冗赘的元素   | 类职责过少，存在意义不大。 | 内联类（Inline Class）或折叠继承体系（Collapse Hierarchy） |       |
| Speculative　Generality / 夸夸其谈通用性 | 过度设计“未来可能用到的”抽象。 | 折叠继承关系、内联类、移除参数、移除方法；移除不必要的抽象，遵循YAGNI原则 |       |
| Temporary　Field / 临时字段 | 类中某些字段仅在特定场景使用。 | 提炼类（Extract Class）或引入Null对象（Introduce Null Object） | 引入NullObject |
| Message　Chains / 过长的消息链 | 不断的向一个对象所求另一个对象，连续调用多个对象的方法（如a.getB().getC()）| 隐藏委托、提取方法、转移方法 | 使用抽象引入Chain Of Responsibility |
| Middle　Man / 中间人       | 类接口中有很多方法都委托给其他类 | 移除中间转手人、内联方法、以继承取代委托 |       |
| Insider　Trading / 内幕交易 | 类之间过度耦合，私下交换数据。 | 搬移函数或搬移字段，划清职责边界。 |       |
| Large　Class / 过大的类     | 类承担过多职责，代码臃肿。 | 提取类、提取子类、提取接口、复制被监视数据 | 以Command取代条件调度程序、以State取代状态改变条件语句、以Interpreter取代隐式语言 |
| Alternative　Classes with Different Interfaces / 异曲同工的类 | 功能相似的类接口不一致。 | 重命名方法、转移方法、提取超类 | 用Adapter 统一接口 |
| Data　Class / 纯数据类      | 只拥有字段的数据类 | 封装字段、封装集合、移除设置方法、转移方法、隐藏方法 |       |
| Refused　Bequest / 被拒绝的遗赠 | 继承父类时，子类想要选择继承的成员。子类仅继承父类的部分方法或属性。 | 以委托取代继承（Replace Inheritance with Delegation） |       |
| Comments　/ 注释过多 | 需大量注释解释代码意图。 | 使用一切重构的方法，是方法本身达到自说明的效果，让注释显得多余 |       |

### 代码的坏味道 - 其他
| Bad Smells in Code | 表现                                                     | 重构方法                                                   | 使用模式重构                                                             |
|---|--------------------------------------------------------|--------------------------------------------------------|--------------------------------------------------------------------|
| Switch Statements switch语句 |                                                        |                                                        |                                                                    |
| inappropriate intimacy 过度亲密 |                                                        |                                                        |                                                                    |
| 条件逻辑过度复杂 | | 分解条件式、合并条件式、合并重复的条件片段、移除控制标记、以卫语句取代嵌套条件式、以多态取代条件式、引入断言 | 以Strategy取代条件逻辑、转移装饰功能到Decorator、以State取代状态改变条件语句、引入Null Object    |
| 分支语句 |                                                        | 提取方法、转移方法、以子类取代类型代码、以多态取代条件式、以明确方法取代参数 | 以State/Strategy取代类型代码、引入NullObject、以Command替换条件调度程序、转移聚集操作到Visitor |
| 组合爆炸 |                                                        |                                                        |                                                                    |
| inappropriate intimacy 过度亲密 | 许多段代码使用不同种类或数量的数据或对象做同样的事情(例如使用特定条件和数据库查询) |   ～                    | 以Interpreter取代隐式语言                                                 |
|不恰当的暴露|在客户代码中国呢不应该看到类的字段和方法，却是公开可见的|封装字段、封装群集、移除设置方法、隐藏方法| 用Factory封装类                                                        |
|平行继承体系|当为一个类增加一个子类时，也必须在另一个类中增加一个相应的子类|转移方法、转移字段| ～                                                                  |
|狎昵关系|类之间彼此依赖于其private成员|转移方法、将双向关联改为单向、提取类、隐藏委托、以继承取代委托| ~                                                                  |
|不完善的程序类库|～|引入外加方法、引入本地扩展|用Adapter统一接口、用Facade封装类|
|怪异解决方案|在同一系统中使用不同的方式解决同一问题|替换算法|用Adapter统一接口|


## 四、重构的实践框架

测试驱动开发（TDD）
- 重构需依赖自动化测试保障安全性。作者建议优先编写测试，覆盖关键逻辑和边界条件。

小步修改
- 每次重构仅进行微小调整，例如重命名变量或拆分函数，确保快速验证结果。

代码即沟通
- 优秀的代码应“不言自明”，通过清晰的命名和结构减少注释需求。


## 引用
- 重构: https://www.refactoring.com/catalog/
- 代码的坏味道: https://cloud.tencent.com/developer/article/1835315
- https://chenzhigao.github.io/2023/05/code-bad-smell/
- https://gausszhou.github.io/refactoring2-zh/ch3.html#_3-7-%E5%8F%91%E6%95%A3%E5%BC%8F%E5%8F%98%E5%8C%96-divergent-change
- https://baike.baidu.com/item/%E9%87%8D%E6%9E%84%EF%BC%9A%E6%94%B9%E5%96%84%E6%97%A2%E6%9C%89%E4%BB%A3%E7%A0%81%E7%9A%84%E8%AE%BE%E8%AE%A1%EF%BC%88%E7%AC%AC2%E7%89%88%EF%BC%89%E8%8B%B1%E6%96%87%E7%89%88/50002502

