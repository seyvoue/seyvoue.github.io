---
title: '@Java 基础: equals 与 =='
comments: true
categories: 开发
tags: java
abbrlink: 9edb0a2
date: 2018-02-01 18:32:07
updated:
keywords:
description:
---

`==`  是一个二元操作符，用于比较原生类型和对象。

* 就原生类型（如：boolean, int, float等），使用 `==` 比较两者的值是否相等
* 就对象而言，使用 `==` 比较两个对象的引用是否相等，即比较的是对象的地址是否相等

对于对象之间比较是否相等，不能单纯的通过比较其地址相等。最常见的就是判断字符串相等的情形，两个相同的字符串，其对象引用可能是不同的，这时你若使用 `==` 去判断两对象是否相等，就不能达到我们预期的目标，因为，对于内容相同的字符串我们认为它们是相同的字符串，即使它们的地址不等。

Java 中的 `equals` 方法其实是交给开发者去覆写的，让开发者根据具体的业务逻辑去定义满足什么条件的两个对象是相等的。所以不能单纯的说 `equals` 到底比较的是什么，想知道一个类的 `equals` 方法是什么意思就要去看定义。

JAVA 库中的 `Object.equals` 方法实现如下：

```java
public boolean equals(Object obj){
    return (this == obj);
}
```
