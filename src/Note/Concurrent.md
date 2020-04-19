

# 并发-多线程**

### 四个并发级别 (*无阻塞*)

| 级别       | 说明                                                         |
| :--------- | ------------------------------------------------------------ |
| **无饥饿** | 没有线程优先级，就不会照成低优先级线程一直抢占不到资源形成饿死现象 |
| **无障碍** | 最弱的非阻塞调度，两个线程如果无障碍的执行，那么不会因为临界区的问题导致一方挂起，一旦检测到冲突，就应该进行回滚，可能导致所有线程都在回滚，而没有一个线程走出临界区，一种可行的无障碍实现可以通过一个“一致性标志”来实现，在线程开始操作前，先读取并保存这个标志，操作完毕后，再次读取，如果不一致，重试操作。对于任何对有资源修改的操作，都需要更新这个一致性标记，表示数据数据不再安全 |
| **无锁**   | 无锁的并行都是无障碍的，与无障碍不同的是，无锁的并发保证必然有一个线程能够在有限步内完成操作离开临界区 |
| **无等待** | 要求所有线程都能够在有限步内完成操作离开临界区，一种典型的无等待就是RCU(Read Copy Update):对数据的读不加控制，但在写的时候先获取原始数据的副本，接着只修改副本数据，修改完成后，在合适的时机在写回数据 |



### 多线程的三个属性：

| 属性       | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| **原子性** |                                                              |
| **可见性** | 一个线程对一个变量的修改，另一个线程并不知道变量大声了修改是为不可见 |
| **有序性** |                                                              |



### 线程的四个属性:

| 属性                 | 说明                                                         |
| -------------------- | ------------------------------------------------------------ |
| 优先级               | setPriority（）调整，优先级再1-10之间，默认为5，MIN_PRIORITY = 1,MAX_PRIORITY = 10;yield(),使调用线程进入让步状态，即将运行权授予相同优先级的其他进程，该方法是一个静态方法。 |
| **守护进程**         | setDaemon(boolean isDaemon)设置是否为守护进程。在调用守护线程的start()前，必须调用.setDaemon(true),当一个java应用程序内制用一个守护线程是，java虚拟机会自动退出 |
| **线程组**           | ThreadGroup:通过调用getThreadGroup()获取线程所在线程组       |
| **未捕获异常处理器** | 处理器必须实现Thread.UncaughtExceptionHandler接口,该接口仅有一个方法uncaughtExecption(Thread t, Throwable e) |



### 线程的状态:

| 状态                   | 说明     | 涉及方法 |
| ---------------------- | -------- | -------- |
| **new**                | 新创建   |          |
| **runnable**           | 可运行   |          |
| **blocked**            | 被阻塞   |          |
| **waiting**            | 等待     |          |
| **timed**  **waiting** | 计时等待 |          |
| **terminated**         | 中止     |          |

#### 查看线程状态：

```java
Thread.currentThread().getState();
```

#### 实现多线程的方法：

```java
1.implement Runnable
```

```java
2.extends Thread
```

```java
3.implements Callable:
Callable<Integer> c = ()->{
	System.out.printfln("xxx");
	return new Integer(1);
}
FutureTask oneTask = new FutureTask(c);
Thread thread = new Thread(oneTask);
thread.start();
```

```java
4:线程池创建
ExecutorService threadPool = Executors.newFixedThreadPool(5);
ExecutorService cachedThreadPool = Executorss.newCachedThreadPool(5);
ExecutorService singlePool = Executors.newSingleThreadExecutor();
ExecutorService scheduledPool = Executors.newScheduledThreadPool(5);
ExecutorService workPool = Executors.newWorkStealingPool();(1.8新增)
```

**ps: **Callable是接口是一个参数化的类型，而Runnable是一个非参数化的，可以经Runnable 想象成一个没有参数和返回值的异步方法，而Callable是有返回值的。

### Thread的start()和run()方法的区别：

1.执行start()会创建一个新县城，并且让这个线程执行run()

2.执行run()是在当前线程内运行，并不会创建一个新县城，而只是作为一个普通的对象方法调用，并且是在当前线程内串行调用run()方法



### 临界区：

可能被多个线程同时执行的代码片段



### 乐观锁：

始终认为在自己去拿数据时被人不会修改，所以不会上锁，但是应该提供一种检测机制用以在更新时判断在此期间别人有没有更新数据。例如版本号机制，乐观锁适用于多读的应用类型，可以提高吞吐量，像数据库提供的write_condition机制。再例如java.util.concurrent.atomic包下的原子变量类(AtomicInteger、AtomicIntegerArray等)

### 悲观锁：

始终任务在自己拿数据时别人会修改，所以在每次拿数据时都会上锁，这样别人在拿数据时必须等待原来锁的持有者释放该锁才可以获得锁。例如 java的  ***synchronized***  关键字，传统型的关系型数据库中的行锁，表锁，读锁，写锁）。
**ps**：区分数据库中的读锁和java中  ***ReentrantReadWriteLock***  中的  ***readLock***  ，java中的读锁也可以同时让别人读

### 多线程环境下，实现同步的四种方式:

1.对整个方法  ***synchronized***  (不推荐)，满足***原子性，有序性***

```java
public synchronized void transfer() {...}
```

2.对方法中部分代码使用  ***synchronized***  修饰，满足***原子性，有序性***

```java
private static Object lock = new Object();
public void transfer() {
	synchronized(lock) {
		A.money +=100;
	}
}
```

3.对可能同时被多个线程修改的变量添加  ***volatile***  关键字修饰，仅满足***可见性***，但是仍然会被编译器优化，发生指令重排，发生并发问题

```

```

4.使用重入锁  ***ReenTrantLock***  ，满足***原子性，有序性***

```java
public static void main(String args...) {
	ReenTrantLock lock = new ReenTrantLock();
	lock.lock();
	try {
	//TODO do somethins
	} catch (Exception e) {
	//TODO do somethins
	} finally {
	lock.unlock();
	}
}
```



### 阻塞队列 -  ***BlockingQueue***:

在队列为空时，有线程从队列中取出元素需要登带队列非空才能取；在队列满时，有线程向队列存放元素需等待队列变为非满状态。

方法:

抛出异常型:

| 方法          | 异常                   | 说明                     | 注释                     |
| ------------- | ---------------------- | ------------------------ | ------------------------ |
| **add(e)**    | IllegalStateException  | 添加                     |                          |
| **element()** | NoSuchElementException | 返回对头元素             | **元素不会从队列中删除** |
| **remove()**  | NoSuchElementException | 返回队头元素，并将其移除 | **元素会从队列中删除**   |

返回特殊值型：(推荐使用接下来这三个方法)

| 方法         | 返回值                  | 说明                     | 注释 |
| ------------ | ----------------------- | ------------------------ | ---- |
| **offer(e)** | 如果队列满，返回false   | 添加                     |      |
| **peek()**   | 如果队列为空，返回nulll | 返回对头元素             |      |
| **poll()**   | 如果队列为空，返回null  | 返回队头元素，并将其移除 |      |

阻塞型：

| 方法       | 阻塞条件             | 说明 | 注释 |
| ---------- | -------------------- | ---- | ---- |
| **put(e)** | 添加元素时，队列已满 | 添加 |      |
| **take()** | 移除元素时，队列已空 | 取出 |      |

超时处理型：

| 方法                          |      | 说明 | 注释 |
| ----------------------------- | ---- | ---- | ---- |
| **offer(e,timeout,timeUnit)** |      |      |      |
| **poll(time,timeUnit)**       |      |      |      |



### 阻塞队列变种：

| 类型                      | 说明                                                         | 注释                                             |
| ------------------------- | ------------------------------------------------------------ | ------------------------------------------------ |
| **LinkedBlockingQueue**   | 容量没有过上边界，但是也可以指定上边界                       | 单向链表                                         |
| **LinkedBlockingDuque**   | 上面一个阻塞队列的双端变种                                   | 双向两百哦                                       |
| **ArrayBlockingQueue**    | 构造时需要制定容量，并且需要一个参数来指定是否公平，如果是公平的，则先到先得 | 同时使用公平性，会降低性能，应该在有需要时才使用 |
| **PriorityBlockingQueue** | 优先级队列，而不是先进先出队列，元素会按照优先级顺序移除，该队列没有容量上限 | 但是取元素操作会阻塞                             |
| **DelayQueue**            | 延时队列，该队列实现了Delayed接口，元素只有在延迟用玩的情况下才能从DelayQueue中移除，同时元素必须实现compareTo()方法，以使队列队元素排序 |                                                  |
| **TransferQueue**         |                                                              |                                                  |
|                           |                                                              |                                                  |

