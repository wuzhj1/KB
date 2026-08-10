---
type: 学习笔记
主题: ThreadPoolExecutor 核心参数
来源: JDK 源码 + 博客
状态: 已掌握
创建日期: 2026-08-10
---

# ThreadPoolExecutor 核心参数

> 线程池七个核心参数各管一件事,组合决定了"队列满之前新任务怎么处理"。

## 核心概念

| 参数 | 作用 |
|------|------|
| corePoolSize | 核心线程数,任务到达时优先复用空闲核心线程 |
| maximumPoolSize | 最大线程数,队列满后才创建到最大 |
| keepAliveTime | 非核心线程空闲存活时间 |
| workQueue | 任务队列,核心线程忙时任务先入队 |
| threadFactory | 线程工厂,自定义线程名/优先级 |
| handler | 拒绝策略,队列满且线程达最大时触发 |

## 例子

```java
new ThreadPoolExecutor(2, 4, 60, TimeUnit.SECONDS,
    new ArrayBlockingQueue<>(100), r -> new Thread(r, "my-pool-" + r.hashCode()),
    new ThreadPoolExecutor.AbortExceptionPolicy());
```

## 我的理解

执行顺序本质是:**核心线程 → 队列 → 非核心线程 → 拒绝策略**。
新手常问"为什么核心线程没满队列先装了"——因为队列在非核心线程之前。

## 疑问

- 无界队列(LinkedBlockingQueue)时 maximumPoolSize 和拒绝策略是否失效?

## 关联

- [[MOC - Java 并发]]

## 下一步

用压测验证:核心线程数该设为 CPU 核数还是 *2,取决于任务类型(CPU 密集/IO 密集)。
