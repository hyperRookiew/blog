---
title: Java8 提供的一些屠龙之术实践
categories:
  - Java技术
tags:
  - Java
date: 2022-07-31 17:38:58
---

# Java8 提供的一些屠龙之术实践

>- Java8.0 提供函数式接口、Lambda表达式、方法引用、默认方法、Stream Api 以及Optional等新特性，这几个新的更新还是比较硬的，十分值得在编码中使用。
- 话说，是不是也得开始系统学学Java11了。

## 一、Java8提供的新特性
- 已在xmind，等有空再移植到这

## 二、Stream API实战
### 1. 查找多个数剧中的最大最小值
```java
// 最大的int
Stream.of(1,2,4,5,6,3).max(Comparator.comparing(Integer::valueOf)).get();
// 最长的字符串长度
Stream.of("3333","333","8888").max(Comparator.comparing(String::length)).get();
// 最高分
persons.stream().max(Comparator.comparing(Person::getScores)).get();
```

### 2. 将数组转化为List
- `Arrays.stream(src)`,将数组转化成流
- `boxed()`,将基础数据类型包装成包装类

```java
// 基本类型数组：int [] to List<Integer>， 需要进行boxed处理
Arrays.stream(array).boxed().collect(Collectors.toList());
// String [] to List<String>
Arrays.stream(array).collect(Collectors.toList());
Arrays.asList(array);
```

### 3. 将List转化成数组

```java
// List<Integer> to int []
list.stream().mapToInt(Integer::valueOf).toArray();
// List<Integer> to Integer[]
list.stream().toArray();
list.toArray(new Integer [list.size()];
```

### 4. 将基础类型数组转化成包装类数组

```java
// int [] to Integer []
Arrays.stream(array).boxed().toArray(Integer[]::new);
```

### 5. 将包装类数组转化成基础类型数组
```java
// Integer[] to int []
Arrays.stream(array).mapToInt(Integer::valueOf).toArray();
```
