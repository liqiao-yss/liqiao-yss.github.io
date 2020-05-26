---
layout: post
title: synchronized
date: 2020-05-26
Author: mimo
categories: 
tags: [synchronized]
comments: true
typora-root-url: ..
---

### 1.实现原理

**synchronized**可以保证方法或者代码块在运行时，同一时刻只有一个方法可以进入到临界区，同时他还可以保证共享变量的内存可见性。是Java中解决并发问题的一种最常用最简单的方法。他可以确保线程互斥的访问同步代码。

### 2.为什么

在并发编程中存在线程安全问题，主要原因有：

1. 存在共享数据
2. 多线程共同操作共享数据。

关键字**synchronized**可以保证在同一时刻，只有一个线程可以执行某个方法或某个代码块，同时**synchronized**可以保证一个线程的变化可见。

### 3.用法分类

##### 1. 根据修饰对象分类

- 修饰代码块
  - synchronized(this或object) {}
  - synchronized(类.class) {}
- 修饰方法
  - 修饰非静态方法
  - 修饰静态方法

##### 2. 根据获取的锁分类

- 获取对象锁

  > 如果是同一个实例，就会按顺序访问，但是如果是不同实例，就可以同时访问。

  - synchronized(this或object) {}
  - 修饰非静态方法

- 获取类锁

  > 只要采用类锁，就会拦截所有线程，只能让一个线程访问

  - synchronized(类.class) {}
  - 修饰静态方法

### 4.三种应用方式

普通同步方法（实例方法），对象锁，锁是当前实例对象 ，进入同步代码前要获得当前实例的锁

> 当两个或多个同步方法时，其他线程来访问synchronized修饰的其他方法时需要等待线程1先把锁释放
>
> 当一个同步方法，一个非同步方法时，如果某个线程得到了对象锁，但是另一个线程还是可以访问没有进行同步的方法或者代码。
>
> 两个线程（非同一实例创建的）作用于不同的对象，获得的是不同的锁，所以互相并不影响

```
public synchronized void increase(){
        i++;
}
```

静态同步方法，类锁，锁是当前类的class对象 ，进入同步代码前要获得当前类对象的锁

> 两个线程实例化两个不同的对象，但是访问的方法是静态的，两个线程发生了互斥（即一个线程访问，另一个线程只能等着），因为静态方法是依附于类而不是对象的，当synchronized修饰静态方法时，锁是class对象。

```
public static synchronized void increase(){
        i++;
}
...
Thread t1 = new Thread(new synchronizedTest());
```

同步方法块，锁是括号里面的对象，对给定对象加锁，进入同步代码库前要获得给定对象的锁。

```
//对象锁
synchronized(this){	}
//类锁
synchronized(Test.class){ }
```