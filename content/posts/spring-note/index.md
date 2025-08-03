---
title: "Spring Notes"
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

# Spring 循环依赖的6种解法

Spring 为什么禁止循环依赖?
https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.6-Release-Notes#circular-references-prohibited-by-default

方案一: 添加允许循环引用配置
spring.main.allow-circular-references=true

方案二: 添加@Lazy注解
构造方法注入或者Set注入的将其中一个添加@Lazy注解，这样有一个延迟注入，从而打破了循环引用的环。

方案三: 添加@PostConstruct 注解延迟注入
初始化完成后注入，更方便两个类互相依赖的处理，A和B互相依赖。A延迟到 PostConstruct 注入。

```java
@Service
public class ServiceA {
    private ServiceB serviceB;
    
    public void setServiceB(ServiceB serviceB) {
        tthis.serviceB = serviceB;
    }
}

@Service
public class ServiceB {
    @Resource
    private ServiceA serviceA;
    
    @PostConstruct
    public void init() {
        serviceA.setServiceB(this);
    }
}
```

方案四: afterPropertiesSet 之后注入

这个方法需要继承 ApplicationContextAware 和 InitializingBean 接口。属于另一种延后注入的方法。 实现类似方案三。

方案五: 使用时从 Spring 上下文获取

这个方法使用了 ApplicationContext, 实际调用方法时 直接获取该 service (也就是说我们不在进行注入了)。

```java
@Service
public class ServiceB {
    @Resource
    private ApplicationContext applicationContext;
    
    public void hello() {
        ServiceA bean = applicationContext.getBean(ServiceA.class);
        bean.sayHello();
    }
}
```

方案六: 使用消息机制解偶引用关系

相互依赖的原因大部分时需要 相互调用方法，那么可以使用消息的解偶特性，打破相互调用。
```Java
// 定义Event事件
public class HiEvent {
}

// ServiceA监听Event事件
@Service
public class ServiceA {
    
    @EventListener
    public void sayHello(HiEvent event) {
        System.out.println("hi");
    }
}

// ServiceB 发送Event事件
@Service
public class ServiceB {

    @Resource
    private ApplicationContext applicationContext;
    
    public void hello() {
        applicationContext.publishEvent(new HiEvent());
    }
}
```

