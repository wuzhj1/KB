---
type: 学习笔记
主题: synchronized 与 volatile 的区别
来源: 《Java并发编程的艺术》
状态: 学习中
创建日期: 2026-08-10
---

# synchronized 与 volatile 的区别

> volatile 只保证可见性和禁止指令重排,不保证原子性;synchronized 保证原子性、可见性、有序性。

## 核心概念

- **volatile**:修饰共享变量,写操作立即刷入主存,读操作直接从主存读;禁止指令重排
- **synchronized**:内置锁,同一时刻只有一个线程能进入临界区,退出时刷新工作内存
- volatile 不能替代 synchronized 的场景:`i++`(读-改-写)、check-then-act 判断

## 例子

```java
volatile boolean running = true;   // 只读标志位,volatile 够用
public synchronized void inc() {   // 计数必须加锁
    count++;
}
```

## 我的理解

一句话记:volatile 是"弱同步",适合"一个线程写、其他线程只读"的场景;凡涉及复合操作都要上锁。

## 疑问

- volatile 在 DCL 单例里防的是什么重排?(构造器重排)

## 关联

- [[死锁产生的四个必要条件]]

## 下一步

读 JMM 内存模型章节,搞清 happens-before 规则。
