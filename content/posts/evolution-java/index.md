---
title: "Java Evolution and Key Features"
date: 2025-06-13T23:00:00+08:00
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

### Overview

|版本|年份|核心特性|
|---|--|----|
|Java 1.0|1996|基础语法、Applet|
|Java 5|2004|泛型、枚举、注解|
|Java 8|2014|Lambda、Stream API|
|Java 11|2018|var、HTTP/2|
|Java 17|2021|密封类、模式匹配|
|Java 21|2023|虚拟线程、结构化并发|
推荐版本：
- 企业级应用：Java 11 / Java 17（LTS）。
- 新项目：Java 21（最新 LTS）。

### Java 1.0 (1996)
- 初始版本：正式发布 Java 语言和基础 API。 核心特性：
- 基本语法（如 Applet 支持）。
- 标准库（java.lang、java.io 等）。

### Java 1.1 (1997)
- 内部类（Inner Classes）。
- 反射 API（java.lang.reflect）。
- JavaBeans（组件模型）。
- JDBC（数据库连接）。
- RMI（远程方法调用）。

### Java 1.2 (1998) - Java 2
- Swing GUI（替代 AWT）。
- 集合框架（java.util 包，如 ArrayList、HashMap）。
- JIT 编译器（提升性能）。
- Java 2 平台划分（J2SE、J2EE、J2ME）。

### Java 1.4 (2002)
- 断言（Assertions）。
- 正则表达式（java.util.regex）。
- NIO（非阻塞 I/O）。
- 日志 API（java.util.logging）。
- XML 解析支持（JAXP）。

### Java 5 (2004) - J2SE 5.0
- 泛型（Generics）（类型安全集合）。
- 增强 for 循环（for-each）。
- 自动装箱/拆箱（基本类型与包装类自动转换）。
- 枚举（Enums）。
- 注解（Annotations）。
- 可变参数（Varargs）。
- 并发包（java.util.concurrent）256。

### Java 8 (2014) - LTS
- Lambda 表达式（函数式编程）: 可以用来替换匿名内部类，简化集合操作，编写流 API 代码。
```java
(x, y) -> x + y
```
- Stream API（集合操作优化）: 可以用来对集合进行操作，简化代码。
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
int sum = numbers.stream().sum();
```
- 默认方法（Default Methods）（接口支持实现）。
- 新的日期时间 API（java.time）。
- Nashorn JavaScript 引擎: 可以用来在 Java 代码中执行 JavaScript 代码。
```java
ScriptEngine engine = new ScriptEngineManager().getEngineByName("nashorn");
engine.eval("print('Hello, world!')");
```
- Optional 类（减少 NullPointerException）: 可以用来处理可能为 null 的值，避免空指针异常。
```java
Optional<String> name = Optional.ofNullable("John Doe");
```
- 方法引用: 可以用来代替 Lambda 表达式，使代码更加简洁易读。

### Java 9 (2017)
- 模块化（JPMS）（提升安全性和可维护性）: 可以用来将 Java 代码组织成模块，提高代码的可维护性和可扩展性。
```java
module com.example.myapp {
    requires java.base;
    exports com.example.myapp.api;
}
```
- JShell（交互式 REPL 工具）。 
```
jshell> var name = "John Doe";
name
$1 ==> "John Doe"
```
- 改进的 Stream API: Reactive Streams API: 可以用来编写异步非阻塞的应用程序。
```java
Publisher<String> publisher = ...;
Subscriber<String> subscriber = ...;
publisher.subscribe(subscriber);
```
- HTTP/2 客户端（孵化）: 可以用来发送 HTTP 请求和接收 HTTP 响应。
```java
HttpClient client = HttpClient.newHttpClient();
HttpRequest request = HttpRequest.newBuilder()
    .uri(URI.create("https://www.example.com/"))
    .build();
HttpResponse response = client.send(request, HttpResponse.BodyHandlers.ofString());
```
- Jigsaw 项目: 可以用来调试和分析 Java 应用程序。
```
jdk.attach("pid");
```

### Java 10
- var 关键字（局部变量类型推断）: 可以用来声明局部变量，无需指定类型。可以简化代码，提高代码的可读性。
```
var name = "John Doe";
var numbers = Arrays.asList(1, 2, 3, 4, 5);
```
- G1 收集器改进: 可以提高 Java 应用程序的性能。
```
-XX:+UseG1GC
```
- Application Class-Data Sharing: 可以提高 Java 应用程序的启动速度。
```
-XX:+UseAppCDS
```

### Java 11 (2018) - LTS
- HTTP/2 客户端标准化。
- 局部变量类型推断增强。
```
var numbers = Arrays.asList(1, 2, 3, 4, 5);
var sum = numbers.stream().sum();
```
- 移除 Java EE 和 CORBA 模块。
- 新的垃圾收集器: Epsilon GC 和 ZGC（低延迟垃圾回收器）。
```
-XX:+UseEpsilonGC
-XX:+UseZGC
```
- Flight Recorder
```
jcmd <pid> FlightRecorder start
jcmd <pid> FlightRecorder stop
jcmd <pid> FlightRecorder dump
```
- Dynamic CDS 
```
-XX:+UseDynamicCDS
```
- GraalVM Native Image

### Java 14
- Records: 可以用来创建不可变的数据类型，提高代码的安全性和可维护性。
```java
record Person(String name, int age) {
    // ...
}
```
- Pattern Matching: 可以用来简化代码，提高代码的可读性。
```
switch (obj) {
    case String s:
        System.out.println("String: " + s);
        break;
    case Integer i:
        System.out.println("Integer: " + i);
        break;
    default:
        System.out.println("Unknown type: " + obj.getClass().getName());
}
```
- 文本块: 第二版: 可以用来编写多行字符串，简化代码。
```
String text = """
    This is a multiline
    string literal.
    """;
```

### Java 17 (2021) - LTS
- 密封类（Sealed Classes）（限制继承）: 可以用来表示有限数量的类型，提高代码的安全性和可维护性。
```
sealed class Animal permits Dog, Cat {
    // ...
}
```
- 模式匹配（Pattern Matching）（instanceof 增强）: 可以用来代替传统。
```
if (obj instanceof String s) {
    System.out.println("String: " + s);
} else if (obj instanceof Integer i) {
    System.out.println("Integer: " + i);
} else {
    System.out.println("Unknown type: " + obj.getClass().getName());
}
```
- 虚拟线程（预览）（Project Loom）。
- ZGC 和 Shenandoah GC 改进。 

### Java 19
- 文本块: 第三版: 可以用来编写多行字符串，并进行格式化。
```java
String text = """
    This is a multiline
    string literal with
    interpolation.
    """;
    System.out.println(text.formatted("%s, %d", "Hello", 123));
```
- Unicode 15.0 支持: 可以使用新的 Unicode 字符。

### Java 21 (2023) - LTS
- 虚拟线程（正式发布）（轻量级并发）。
```
var thread = VirtualThread.of(() -> {
    // ...
});
thread.start();
```
- 结构化并发（Structured Concurrency）。
- 模式匹配（switch 增强）9。

### Java 24（2025 年 3 月发布）：
- 作用域值（Scoped Values）（线程间数据共享优化）。
- 向量 API（第九次孵化）（SIMD 支持）。
- 结构化并发（第四次预览）4。

### Java 25（2025 年 9 月发布，LTS）：
- 模块导入声明（Module Import Declarations）（简化模块管理）。
- 紧凑源文件（Compact Source Files）（初学者友好）7。

##### Reference
- https://www.oracle.com/java/technologies/javase/24all-relnotes.html
