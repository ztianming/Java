# 多线程
## 1. 多线程好处
1. 发挥多核CPU的优势
2. 防止阻塞
3. 便于建模

## 2. 创建线程的⽅式
1. 继承Thread类
2. 实现Runnable接⼝  
**实现接⼝的⽅式⽐继承类的⽅式更灵活，也能减少程序之间的耦合度**

## 3. start()⽅法和run()⽅法的区别
调⽤start()⽅法,不同线程的run()⽅法⾥⾯的代码交替执⾏  
调⽤run()⽅法, 代码同步执⾏,必须等待⼀个线程的run()⽅法⾥⾯的代码全部执⾏完毕之后，另外⼀个线程才可以执⾏

## 4. Runnable接⼝和Callable接⼝的区别
Runnable接⼝中的run()⽅法的返回值是void，它只是纯粹地去执⾏run()⽅法中的代码⽽已  
Callable接⼝中的call()⽅法是有返回值的，是⼀个泛型，和Future、FutureTask配合可以⽤来获取异步执⾏的结果  

## 5. CyclicBarrier和CountDownLatch的区别
1. CyclicBarrier的某个线程运⾏到某个点上之后，该线程即停⽌运⾏，直到所有的线程都到达了这个点，所有线程才重新运
⾏； 
CountDownLatch则不是，某线程运⾏到某个点上之后，只是给某个数值-1⽽已，该线程继续运⾏。
2. CyclicBarrier只能唤起⼀个任务，  
CountDownLatch可以唤起多个任务
3. CyclicBarrier可重⽤，  
CountDownLatch不可重⽤，计数值为0时不可再⽤

## 6. volatile关键字的作⽤
1. volatile关键字修饰的变量，保证了其在多线程之间的可⻅性，即每次读取到volatile变量，⼀定是最新的数据
2. 使⽤volatile则会对禁⽌语义重排序，和CAS结合，保证了原⼦性

## 7. 线程安全
代码在多线程下执⾏和在单线程下执⾏永远都能获得⼀样的结果，那么代码就是线程安全的
### 级别：
1. 不可变  
像String、Integer、Long这些，都是final类型的类，任何⼀个线程都改变不了它们的值
2. 绝对线程安全  
调⽤者都不需要额外的同步措施
3. 相对线程安全

4. 线程⾮安全

## 8. 如何获取到线程dump⽂件
1. 获取到线程的pid，使⽤jps命令，在Linux下还可以使⽤ps -ef | grep java
2. 打印线程堆栈，使⽤jstack pid命令，在Linux下还可以使⽤kill -3 pid
3. Thread类提供了⼀个getStackTrace()⽅法也可以⽤于获取线程堆栈，取获取到的是具体某个线程当前运⾏的堆栈

## 9. 线程如果出现了运⾏时异常会怎么样
如果这个异常没有被捕获的话，这个线程就停⽌执⾏  
如果这个线程持有某个某个对象的监视器，那么这个对象监视器会被⽴即释放

## 10. 线程之间共享数据
1. 共享变量
2. 管道
### 进程通信
1. 共享存储
2. 消息传递
3. 管道
4. 文件映射
5. 套接字

## 11. sleep⽅法和wait⽅法区别
都可以⽤来放弃CPU⼀定的时间  
不同点在于如果线程持有某个对象的监视器，sleep⽅法不会放弃这个对象的监视器，wait⽅法会放弃这个对象的监视器

## 12. ⽣产者消费者模型的作⽤
1. 通过平衡⽣产者的⽣产能⼒和消费者的消费能⼒来**提升整个系统的运⾏效率**
2. 解耦，这是⽣产者消费者模型附带的作⽤

## 13. ThreadLocal
以空间换时间，在每个Thread⾥⾯维护了⼀个以开地址法实现的ThreadLocal.ThreadLocalMap，把数据进⾏隔离，数据不共享，
保证线程安全

## 14. 为什么wait()⽅法和notify()/notifyAll()⽅法要在同步块中被调⽤
JDK强制的，wait()⽅法和notify()/notifyAll()⽅法在调⽤前都必须先获得对象的锁

## 15. wait()⽅法和notify()/notifyAll()⽅法在放弃对象监视器时的区别
wait()⽅法⽴即释放对象监视器，  
notify()/notifyAll()⽅法则会等待线程剩余代码执⾏完毕才会放弃对象监视器。

## 16. 为什么要使⽤线程池
避免频繁地创建和销毁线程，达到线程对象的重⽤  
可以根据项⽬灵活地控制并发的数⽬

## 17. 检测⼀个线程是否持有对象监视器
Thread类提供了⼀个holdsLock(Object obj)⽅法，当且仅当对象obj的监视器被某条线程持有的时候才会返回true  
注意这是⼀个static⽅法，这意味着"某条线程"指的是当前线程

## 18. synchronized和ReentrantLock的区别
相似点：
都是加锁方式同步、阻塞式同步  

synchronized是关键字，ReentrantLock是类，这是⼆者的本质区别。
ReenTrantLock的实现是一种自旋锁，通过循环调用CAS操作来实现加锁。它的性能比较好是因为避免了使线程进入内核态的阻塞状态。
ReentrantLock可以被继承、可以有⽅法、可以有各种各样的类变量
### ReentrantLock扩展性：
1. 可以对获取锁的等待时间进⾏设置，避免死锁
2. 可以获取各种锁的信息
3. 可以灵活地实现多路通知

### ReentrantLock高级功能：
1. 等待可中断
2. 公平锁  
多个线程等待同一个锁时，必须按照申请锁的时间顺序获得锁; 默认非公平锁，可以通过参数true设为公平锁;
Synchronized锁非公平锁
3. 锁绑定多个条件  
一个ReentrantLock对象可以同时绑定对个对象。ReenTrantLock提供了一个Condition（条件）类，用来实现分组唤醒需要唤醒的线程们;  
synchronized要么随机唤醒一个线程要么唤醒全部线程。

## 19. ConcurrentHashMap的并发度
ConcurrentHashMap的并发度就是segment的⼤⼩，默认为16，这意味着最多同时可以有16条线程操作

## 20. ReadWriteLock
ReadWriteLock是⼀个读写锁接⼝，ReentrantReadWriteLock是ReadWriteLock接⼝的⼀个具体实现，  
实现了读写的分离，读锁是共享的，写锁是独占的，提升了读写的性能。

## 21. FutureTask
FutureTask表⽰⼀个异步运算的任务，可以传⼊⼀个Callable的具体实现类，可以对
这个异步运算的任务的结果进⾏等待获取、判断是否已经完成、取消任务等操作。

## 22. Linux环境下如何查找哪个线程使⽤CPU最⻓
1. 获取项⽬的pid，jps或者ps -ef | grep java
2. top -H -p pid，顺序不能改变 ⼗进制的，

## 23. 编程写⼀个会导致死锁的程序
![](https://img-blog.csdnimg.cn/20190221140652104.png)
```javascript 
public class DeadLockDemo {
 
	public static void main(String[] args) {
		Object lockA = new Object();
		Object lockB = new Object();
		new Thread(new Runnable() {
			@Override
			public void run() {
				String name = Thread.currentThread().getName();
				synchronized (lockA) {
					System.out.println(name + " got lockA,  want LockB");
					try {
						Thread.sleep(100);
					} catch (InterruptedException e) {
 
						e.printStackTrace();
					}
					synchronized (lockB) {
						System.out.println(name + " got lockB");
						System.out.println(name + ": say Hello!");
					}
				}
			}
		}, "线程-A").start();
 
		new Thread(new Runnable() {
			@Override
			public void run() {
 
				String name = Thread.currentThread().getName();
				synchronized (lockB) {
					System.out.println(name + " got lockB, want LockA");
					try {
						Thread.sleep(100);
					} catch (InterruptedException e) {
 
						e.printStackTrace();
					}
					synchronized (lockA) {
						System.out.println(name + " got lockA");
						System.out.println(name + ": say Hello!");
					}
				}
 
			}
		}, "线程-B").start();
	}
}
```

## 24. 唤醒⼀个阻塞的线程
如果线程是因为调⽤了wait()、sleep()或者join()⽅法⽽导致的阻塞，可以中断线程，并且通过抛出InterruptedException来唤
醒它；  
如果线程遇到了IO阻塞，⽆能为⼒。

## 25. 不可变对象对多线程的帮助
不可变对象保证了对象的内存可⻅性，对不可变对象的读取不需要进⾏额外的同步⼿段，提升了代码执⾏效率

## 26. 多线程的上下⽂切换
多线程的上下⽂切换是指CPU控制权由⼀个已经正在运⾏的线程切换到另外⼀个就绪并等待获取CPU执⾏权的线程的过程。

## 27. 如果你提交任务时，线程池队列已满，这时会发⽣什么
1. 如果使⽤的是⽆界队列LinkedBlockingQueue，继续添加任务到阻塞队列中等待执⾏
2. 如果使⽤的是有界队列⽐如ArrayBlockingQueue，任务⾸先会被添加到ArrayBlockingQueue中，  
ArrayBlockingQueue满了，会根据maximumPoolSize的值增加线程数量，  
如果增加了线程数量还是处理不过来，ArrayBlockingQueue继续满，那么则会使⽤拒绝策略RejectedExecutionHandler处理满了的任务，默认是AbortPolicy

## 28. 线程调度算法
抢占式。⼀个线程⽤完CPU之后，操作系统会根据线程优先级、线程饥饿情况等数据算出⼀个总的优先级并分配下⼀个时间⽚给
某个线程执⾏。

## 29. Thread.sleep(0)的作⽤
为了让某些优先级⽐较低的线程也能获取到CPU控制权，可以使⽤Thread.sleep(0)⼿动触发⼀次操作系统
分配时间⽚的操作，这也是平衡CPU控制权的⼀种操作

## 30. 什么是⾃旋
synchronized⾥⾯的代码执⾏得⾮常快，不妨让等待锁的线程不要被阻塞，
⽽是在synchronized的边界做忙循环，这就是⾃旋。  
如果做了多次忙循环发现还没有获得锁，再阻塞，这样可能是⼀种更好的策略。

## 31. Java内存模型
1. Java内存模型将内存分为了主内存和⼯作内存
2. 定义了⼏个原⼦操作，⽤于操作主内存和⼯作内存中的变量
3. 定义了volatile变量的使⽤规则
4. happens-before，即先⾏发⽣原则，定义了操作A必然先⾏发⽣于操作B的⼀些规则

## 32. CAS
CAS，全称为Compare and Swap，即⽐较-替换  
3个基本操作数：内存地址V，旧的预期值A，要修改的新值B  
更新一个变量的时候，只有当变量的预期值A和内存地址V当中的实际值相同时，才会将内存地址V对应的值修改为B。  
要volatile变量配合

## 33. 乐观锁和悲观锁
乐观锁:认为竞争不总是会发⽣,不需要持有锁，将⽐较-替换这两个动作作为⼀个原⼦操作尝试去修改内存中的变量，如果失败则表⽰发⽣冲突，那么就应该有相应的
重试逻辑  
适用于写比较少的情况下（多读场景）
悲观锁:认为竞争总是会发⽣，因此每次对某资源进⾏操作时，都会持有⼀个独占的锁  
适用于多写的场景

## 34. AQS
AQS全称为AbstractQueuedSychronizer, 抽象队列同步器
AQS定义了对双向队列所有的操作，⽽只开放了tryLock和tryRelease⽅法给开发者使⽤
提供了一种实现阻塞锁和一系列依赖FIFO等待队列的同步器的框架，ReentrantLock、Semaphore、CountDownLatch、CyclicBarrier等并发类均是基于AQS来实现的

## 35. 单例模式的线程安全性
1. 饿汉式单例模式的写法：线程安全
2. 懒汉式单例模式的写法：⾮线程安全
3. 双检锁单例模式的写法：线程安全

## 36. Semaphore
Semaphore就是⼀个信号量，它的作⽤是限制某段代码块的并发数

## 37. Hashtable的size()只有⼀条语句"return count"，为什么还要做同步？
1. 同⼀时间只能有⼀条线程执⾏固定类的同步⽅法，但是对于类的⾮同步⽅法，可以多条线程同时访问
2. CPU执⾏代码，执⾏的不是Java代码，汇编语句可能有多条

## 38. 线程类的构造⽅法、静态块是被哪个线程调⽤的
线程类的构造⽅法、静态块是被new这个线程类所在的线程所调⽤的，  
⽽run⽅法⾥⾯的代码才是被线程⾃⾝所调⽤的。

## 39. 同步⽅法和同步块，哪个更好
同步块，这意味着同步块之外的代码是异步执⾏的，这⽐同步整个⽅法更提升代码的效率  
原则：同同步的范围越⼩越好
Java虚拟机中还是存在着⼀种叫做锁粗化的优化⽅法，这种⽅法就是把同步范围变⼤  
减少了加锁-->解锁的次数，有效地提升了代码执⾏的效率

## 40. ⾼并发、任务执⾏时间短的业务怎样使⽤线程池？并发不⾼、任务执⾏时间⻓的业务怎样使⽤线程池？并发⾼、业务执⾏
时间⻓的业务怎样使⽤线程池？
1. ⾼并发、任务执⾏时间短的业务，线程池线程数可以设置为CPU核数+1，减少线程上下⽂的切换
2. 并发不⾼、任务执⾏时间⻓的业务要区分开看
    1. IO密集型的任务，可以加⼤线程池中的线程数⽬，让CPU处理更多的业务
    2. 计算密集型任务,线程池中的线程数设置得少⼀些，减少线程上下⽂的切换
3. 并发⾼、业务执⾏时间⻓,关键在于整体架构的设计  
    1. 数据是否能做缓存
    2. 增加服务器
    3. ⽤中间件对任务进⾏拆分和解耦
