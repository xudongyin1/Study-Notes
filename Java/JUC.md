## 1.什么是JUC

![img](https://gitee.com/xudongyin/img/raw/master/img/20200620153317928.png)

java.util工具包

**业务: 普通的线程代码, 之前都是用的thread或者runnable接口**

但是相比于callable来说,thread没有返回值,且效率没有callable高

![img](https://gitee.com/xudongyin/img/raw/master/img/20200620153337216.png)

![img](https://gitee.com/xudongyin/img/raw/master/img/20200620153350685.png)

## 2.线程和进程

> **线程,进程**

**进程** : 一个运行中的程序的集合; 一个进程往往可以包含多个线程,至少包含一个线程

**java默认有几个线程? 两个， main线程 和 gc线程**

**线程** : 线程（thread）是操作系统能够进行运算调度的最小单位。

对于java而言如何创建thread: **继承自thread,实现runnable接口,实现callable接口,线程池创建**

**Java真的可以开启线程吗**? 开不了的,底层是用native关键词修饰.调用本地实现

```java
public synchronized void start() {
    /**
     * This method is not invoked for the main method thread or "system"
     * group threads created/set up by the VM. Any new functionality added
     * to this method in the future may have to also be added to the VM.
     *
     * A zero status value corresponds to state "NEW".
     */
    if (threadStatus != 0)
        throw new IllegalThreadStateException();

    /* Notify the group that this thread is about to be started
     * so that it can be added to the group's list of threads
     * and the group's unstarted count can be decremented. */
    group.add(this);

    boolean started = false;
    try {
        start0();
        started = true;
    } finally {
        try {
            if (!started) {
                group.threadStartFailed(this);
            }
        } catch (Throwable ignore) {
            /* do nothing. If start0 threw a Throwable then
              it will be passed up the call stack */
        }
    }
}
//本地方法,调用底层c++, java无法操作硬件
private native void start0();
```

> **并发,并行**

并发编程: 并发和并行

并发(多线程操作同一个资源,交替执行)

- CPU一核, 模拟出来多条线程,天下武功,唯快不破,快速交替

并行(多个人一起行走, 同时进行)

- CPU多核,多个线程同时进行 ; 使用线程池操作

  ~~~java
  public static void main(String[] args) {
          //获取CPU核数
          //CPU 密集型,IO密集型
          System.out.println(Runtime.getRuntime().availableProcessors());
  }
  ~~~

并发编程的本质: **充分利用CPU的资源**

所有的公司都很看重!

> **线程有几个状态?**

```java
public enum State {
    // 新生
     NEW,
     
    // 运行
    RUNNABLE,

    // 阻塞
    BLOCKED,

    // 等待
    WAITING,

    //超时等待
    TIMED_WAITING,

    //终止
    TERMINATED;
}
```

> **wait/sleep的区别**

1.来自不同的类

	wait来自object类, sleep来自线程类

2.关于锁的释放

	wait会释放锁, sleep不会释放锁

3.使用的范围不同

	wait必须在同步代码块中
	
	sleep可以在任何地方睡

4.是否需要捕获异常

	wait不需要捕获异常
	
	sleep需要捕获异常

## 3.Lock锁(重点)

在java的锁的实现上就有共享锁和独占锁的区别，而这些实现都是基于AbstractQueuedSynchronizer对于共享同步和独占同步的支持。

> **独占模式**

AbstractQueuedSynchronizer（AQS）使用一个volatile类型的int来作为同步变量，任何想要获得锁的线程都需要来竞争该变量，获得锁的线程可以继续业务流程的执行，而没有获得锁的线程会被放到一个FIFO的队列中去，等待再次竞争同步变量来获得锁。

 **锁的独占与共享**

   	java并发包提供的加锁模式分为独占锁和共享锁，独占锁模式下，每次只能有一个线程能持有锁，ReentrantLock就是以独占方式实现的互斥锁。共享锁，则允许多个线程同时获取锁，并发访问 共享资源，如：ReadWriteLock。AQS的内部类Node定义了两个常量SHARED（共享）和EXCLUSIVE（独占），他们分别标识 AQS队列中等待线程的锁获取模式。

   	很显然，独占锁是一种悲观保守的加锁策略，它避免了读/写冲突，如果某个只读线程获取锁，则其他读线程都只能等待，这种情况下就限制了不必要的并发性，因为读操作并不会影响数据的一致性。共享锁则是一种乐观锁，它放宽了加锁策略，允许多个执行读操作的线程同时访问共享资源。 java 的并发包中提供了ReadWriteLock，读-写锁。**它允许一个资源可以被多个读操作访问，或者被一个写操作访问，但两者不能同时进行**。

**锁的公平与非公平**

  	 锁的公平与非公平，是指线程请求获取锁的过程中，是否允许插队。在公平锁上，线程将按他们发出请求的顺序来获得锁；而非公平锁则允许在线程发出请求后立即尝试获取锁，如果可用则可直接获取锁，尝试失败才进行排队等待。ReentrantLock提供了两种锁获取方式，FairSyn和NofairSync。**结论：ReentrantLock是以独占锁的加锁策略实现的互斥锁，同时它提供了公平和非公平两种锁获取方式**。

**AQS提供的模板方法**

  	 AQS提供了独占锁和共享锁必须实现的方法，具有独占锁功能的子类，它必须实现tryAcquire、tryRelease、isHeldExclusively等；共享锁功能的子类，必须实现tryAcquireShared和tryReleaseShared等方法，带有Shared后缀的方法都是支持共享锁加锁的语义。Semaphore是一种共享锁，ReentrantLock是一种独占锁。

  独占锁获取锁时，设置节点模式为Node.EXCLUSIVE

```java
public final void acquire(int arg) {
    if (!tryAcquire(arg) &&
        acquireQueued(addWaiter(Node.EXCLUSIVE), arg))
        selfInterrupt();
}
```

  共享锁获取锁，节点模式则为Node.SHARED

```java
private void doAcquireShared(int arg) {
    final Node node = addWaiter(Node.SHARED);
    boolean failed = true;
    .....
}
```

 **对ConditionObject的认识**

   ReentrantLock是独占锁，**而且AQS的ConditionObject只能与ReentrantLock一起使用，它是为了支持条件队列的锁更方便。ConditionObject的signal和await方法都是基于独占锁的，如果线程非锁的独占线程，则会抛出IllegalMonitorStateException**。例如signalAll源码：

```java
public final void signalAll() {
            if (!isHeldExclusively())
                throw new IllegalMonitorStateException();
            Node first = firstWaiter;
            if (first != null)
                doSignalAll(first);
        }
```

   我在想，既然Condtion是为了支持Lock的，为什么ConditionObject不作为ReentrantLock的内部类呢？对于实现锁功能的子类，直接扩展它就可以实现对条件队列的支持。但是，对于其它非锁语义的实现类如Semaphore、CountDownLatch等类来说，条件队列是无用的，也会给开发者扩展AQS带来困惑。总之，是各有利弊，大师们的思想，还需要仔细揣摩啊！

**共享模式和独占模式的区别在于，独占模式只允许一个线程获得资源，而共享模式允许多个线程获得资源。**

> **传统synchronized**

本质: 队列和锁, synchronized 放在方法上锁的是this,放在代码块中锁的是()里面的对象

~~~java
package com.kuang.demo01; 
// 基本的卖票例子 
import java.time.OffsetDateTime;
/*** 真正的多线程开发，公司中的开发，降低耦合性 
 * 线程就是一个单独的资源类，没有任何附属的操作！ 
 * 1、 属性、方法 */ 
public class SaleTicketDemo01 {
    public static void main(String[] args) { 
        // 并发：多线程操作同一个资源类, 把资源类丢入线程 
        Ticket ticket = new Ticket();
        // @FunctionalInterface 函数式接口，jdk1.8 lambda表达式 (参数)->{ 代码 } 
        new Thread(()->{ for (int i = 1; i < 40 ; i++) { ticket.sale(); } },"A").start(); 
        new Thread(()->{ for (int i = 1; i < 40 ; i++) { ticket.sale(); } },"B").start(); 
        new Thread(()->{ for (int i = 1; i < 40 ; i++) { ticket.sale(); } },"C").start(); 
    } 
}
// 资源类 OOP 
class Ticket { 
    // 属性、方法 
    private int number = 30; 
    // 卖票的方式 
    // synchronized 本质: 队列，锁
    public synchronized void sale(){ 
        if (number>0)
        { System.out.println(Thread.currentThread().getName()+"卖出了"+(number- -)+"票,剩余："+number); }
    }
}
~~~

> **Lock 接口**

![img](https://gitee.com/xudongyin/img/raw/master/img/20200620153408911.png)

**实现类**

![img](https://gitee.com/xudongyin/img/raw/master/img/20200620153421498.png)

**什么是 “可重入”**，可重入就是说某个线程已经获得某个锁，可以再次获取锁而不会出现死锁。

> **ReentrantLock构造器**

~~~java
public ReentrantLock() {
    sync = new NonfairSync(); //无参默认非公平锁
}
public ReentrantLock(boolean fair) {
    sync = fair ? new FairSync() : new NonfairSync();//传参为true为公平锁
}
~~~

**公平锁: 十分公平: 可以先来后到,一定要排队**

**非公平锁: 十分不公平,可以插队(默认)**

```java
public class SaleTicketDemo {

    public static void main(String[] args) {
        // 并发：多线程操作同一个资源类, 把资源类丢入线程
        Ticket ticket = new Ticket();
       // @FunctionalInterface 函数式接口，jdk1.8 lambda表达式 (参数)->{ 代码 }
       new Thread(()->{for(int i = 0; i < 40; i++) {ticket.sale();}}, "a").start();
       new Thread(()->{for(int i = 0; i < 40; i++) ticket.sale();}, "b").start();
       new Thread(()->{for(int i = 0; i < 40; i++) ticket.sale();}, "c").start();

    }
}

class Ticket {
    // Lock三部曲 
    // 1、 new ReentrantLock(); 
    // 2、 lock.lock(); // 加锁 
    // 3、 finally=> lock.unlock(); // 解锁
    private int ticketNum = 30;
    private Lock lock = new ReentrantLock();

    public void sale() {
        lock.lock();
        try {
            if (this.ticketNum > 0) {
                System.out.println(Thread.currentThread().getName() + "购得第" + ticketNum-- + "张票, 剩余" + ticketNum + "张票");
            }
        } finally {
            lock.unlock();
        }
    }
}
```

> **synchronized和lock锁的区别**

- synchronized内置的java关键字,Lock是一个java类

- synchronized无法判断获取锁的状态, Lock可以判断是否获取到了锁

- synchronized会自动释放锁,Lock必须要手动释放锁!如果不是释放锁,会产生死锁，ReentrantLock 和 synchronized 不一样，需要手动释放锁，所以使用 ReentrantLock的时候一定要**手动释放锁**，并且**加锁次数和释放次数要一样**

- 0synchronized 线程1(获得锁,阻塞),线程2(等待); Lock锁就不一定会等待下去

- synchronized 可重入锁,不可以中断的,非公平的; Lock锁,可重入的,可以判断锁,非公平(可自己设置为公平锁);

- synchronized 适合锁少量的代码同步问题,Lock 适合锁大量的同步代码

  

## 4.生产者和消费者问题

面试高频: 单例模式, 八大排序,生产者消费者,死锁

> **Synchronized实现 wait notify**

~~~java
package com.xu.PC;

public class ProducerAndConsumer {
    public static void main(String[] args) {
        Resource resource = new Resource();
        new Thread(()->{
            for (int i = 0; i < 20; i++) {
                try {
                    resource.increment();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"A").start();
        new Thread(()->{
            for (int i = 0; i < 20; i++) {
                try {
                    resource.increment();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"B").start();
        new Thread(()->{
            for (int i = 0; i < 20; i++) {
                try {
                    resource.decrement();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"C").start();
        new Thread(()->{
            for (int i = 0; i < 20; i++) {
                try {
                    resource.decrement();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"D").start();
    }
}

class Resource {
    private int num = 0;

    public synchronized void increment() throws InterruptedException {
        // if (num != 0)
        while (num!=0){//判断是否有值
            this.wait();  //有资源，不生产，进入睡眠
        }
        num++;
        System.out.println(Thread.currentThread().getName()+"->"+num);
        this.notifyAll();//唤醒其他线程,叫其他线程来消费资源
    }

    public synchronized void decrement() throws InterruptedException {
        // if (num == 0)
        while (num == 0) {//判断是否为0
            this.wait();  //无资源，不消费，进入睡眠
        }
        num--;
        System.out.println(Thread.currentThread().getName()+"->"+num);
        this.notifyAll();//唤醒其他线程，叫其他线程来生产资源
    }
}
~~~

**注意：在多个线程通信时，必须使用 while 判断防止虚假唤醒。**

if判断改为while判断

因为if只会执行一次，线程醒来会接着向下执行if（）外边的

而while不会，会重新判断条件是否满足，满足才会向下执行while（）外边的

> **JUC版本生产者和消费者问题**

![img](https://gitee.com/xudongyin/img/raw/master/img/2020062015350558.png)

通过Lock 找到 Condition（监视者）

![img](https://gitee.com/xudongyin/img/raw/master/img/20200620153518552.png)

**任何一个新的技术,绝对不是仅仅覆盖了原来的技术,一定有优势和补充**

```java
package com.xu.PC;

import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class ProducerAndConsumer2 {
    public static void main(String[] args) {
        Resource2 resource = new Resource2();
        new Thread(()->{
            for (int i = 0; i < 20; i++) {

                try {
                    resource.increment();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }

            }
        },"A").start();
        new Thread(()->{
            for (int i = 0; i < 20; i++) {
                try {
                    resource.increment();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"C").start();
        new Thread(()->{
            for (int i = 0; i < 20; i++) {
                try {
                    resource.decrement();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"D").start();
        new Thread(()->{
            for (int i = 0; i < 20; i++) {
                try {
                    resource.decrement();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        },"B").start();
    }
}

class Resource2 {
    private int num = 0;
     Lock lock = new ReentrantLock();
     Condition condition = lock.newCondition();//监视器
    public  void increment() throws InterruptedException {
       lock.lock();

        try {
            while (num!=0){//判断是否有值
               condition.await();
            }
            num++;
            System.out.println(Thread.currentThread().getName()+"->"+num);
            condition.signalAll();//唤醒其他线程
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }

    }

    public  void decrement() throws InterruptedException {
        lock.lock();
        try {
            while (num == 0) {//判断是否为0
               condition.await();
            }
            num--;
            System.out.println(Thread.currentThread().getName()+"->"+num);
            condition.signalAll();//唤醒其他线程
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
}
```

> **condition的精准通知和唤醒线程**

```java
package com.xu.PC;

import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class ProducerAndConsumer3 {
    public static void main(String[] args) {
        Resource3 resource3 = new Resource3();
        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                resource3.A();
            }
        }, "A").start();
        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                resource3.B();
            }
        }, "B").start();
        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                resource3.C();
            }
        }, "C").start();
    }
}

class Resource3 {
    private Lock lock = new ReentrantLock();
    private Condition condition1 = lock.newCondition();
    private Condition condition2 = lock.newCondition();
    private Condition condition3 = lock.newCondition();
    private int number = 1;   
    //A 执行完调用B，B执行完调用C，C执行完调用A    1A 2B 3C
    public void A() {
        lock.lock();
        try {
            while (number != 1) {
                condition1.await();
            }
            number = 2;  //这样就可以执行B线程
            System.out.println(Thread.currentThread().getName() + "->AAAAAA");
            condition2.signal();  //准确唤醒B线程
        } catch (Exception exception) {
            exception.printStackTrace();
        } finally {
            lock.unlock();
        }
    }

    public void B() {
        lock.lock();
        try {
            while (number != 2) {
                condition2.await();
            }
            number = 3;   //这样就可以执行C线程
            System.out.println(Thread.currentThread().getName() + "->BBBBB");
            condition3.signal();  //准确唤醒C线程
        } catch (Exception exception) {
            exception.printStackTrace();
        } finally {
            lock.unlock();
        }
    }

    public void C() {
        lock.lock();
        try {
            while (number != 3) {
                condition3.await();
            }
            number = 1;  //这样就可以执行A线程
            System.out.println(Thread.currentThread().getName() + "->CCCCC");
            condition1.signal();  //准确唤醒A线程
        } catch (Exception exception) {
            exception.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
}
```

## 5. 8锁现象

如何判断锁的是谁！永远的知道什么锁，锁到底锁的是谁！
**深刻理解我们的锁**

```java
package com.xu.LOCK;

import java.util.concurrent.TimeUnit;
/*** 8锁，就是关于锁的8个问题 
   * 1、标准情况下，两个线程先打印 发短信还是 打电话？ 1/发短信 2/打电话 
   * 1、sendSms延迟4秒，两个线程先打印 发短信还是 打电话？ 1/发短信 2/打电话 
   * synchronized 放在方法上锁的是方法调用者，这里只有一个调用者 phone
   */
public class Synchronized {
    public static void main(String[] args) {
        Phone phone = new Phone();
        new Thread(()->{
            try {
                phone.send();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        },"t1").start();
        // 延迟执行下面t2线程，但是结果无影响，因为他们用的是同把锁。
        try {
            TimeUnit.SECONDS.sleep(1); 
        } catch (InterruptedException e) {
                e.printStackTrace(); 
        }
        new Thread(()->{
            phone.call();
        },"t2").start();
    }
    //两个线程持有的是同一个锁，谁先拿到谁先执行完,另一个才执行
}

class Phone {
    public synchronized void send() throws InterruptedException {
        System.out.println(Thread.currentThread().getName() + "sendMessage");
        TimeUnit.SECONDS.sleep(2);
        call();//与send（）是同一把锁 this，锁的是方法调用者，不是两把锁
    }

    public synchronized void call() {
        System.out.println(Thread.currentThread().getName() + "call");
    }
}
```

~~~java
package com.czp.lock;

import java.util.concurrent.TimeUnit;

/**
 * 3、 增加了一个普通方法后！先执行发短信还是Hello？ 普通方法
 * 4.  两个对象，两个同步方法， 发短信还是 打电话？ // 打电话
 */
public class Test2 {
    public static void main(String[] args) {
        Phone1 phone = new Phone1();
        Phone1 phone2 = new Phone1();

        new Thread(() -> {
            phone.sendMessage();
        }, "A").start();

        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        new Thread(() -> {
//            phone.hello();
            phone2.call();
        }, "B").start();
    }
}

class Phone1 {

    public synchronized void sendMessage() {
        try {
            TimeUnit.SECONDS.sleep(4);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(Thread.currentThread().getName() + "sendMessage");
    }

    public synchronized void call() {
        System.out.println(Thread.currentThread().getName() + "call");
    }
    //这里没有锁,不受锁的影响
    public void hello(){
        System.out.println("hello");
    }
}

~~~

~~~java
package com.czp.lock;

import java.util.concurrent.TimeUnit;

/**
 * 5.增加两个静态的同步方法
 * synchronized 锁的是方法调用者
 * static 静态方法类一加载就有了 锁的是CLass
 *
 * 6. 两个对象,两个静态同步方法,先发短信还是先打电话  //发短信
 *
 */
public class Test3 {

    public static void main(String[] args) {
        // 两个对象的Class类模板只有一个，static锁的是Class
        Phone2 phone2 = new Phone2();
        Phone2 phone3 = new Phone2();

        new Thread(() -> {
            phone2.sendMessage();
        }, "A").start();

        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        new Thread(() -> {
            phone3.call();
        }, "B").start();
    }
}
class Phone2 {

    public static synchronized void sendMessage() {
        try {
            TimeUnit.SECONDS.sleep(4);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(Thread.currentThread().getName() + "sendMessage");
    }

    public static synchronized void call() {
        System.out.println(Thread.currentThread().getName() + "call");
    }
}

~~~

~~~java
package com.czp.lock;

import java.util.concurrent.TimeUnit;

/**
 * 7.一个静态同步方法,一个普通同步方法 ,一个对象,先输出哪一个   //打电话
 *
 * 8. 一个静态同步方法,一个普通同步方法, 两个对象,先打印哪一个  //B打电话
 */
public class Test4 {
    public static void main(String[] args) {
        Phone3 phone3 = new Phone3();
        Phone3 phone4 = new Phone3();

        new Thread(() -> {
            phone3.sendMessage();
        }, "A").start();

        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        new Thread(() -> {
            phone4.call();
        }, "B").start();
    }
}

class Phone3 {

    public static synchronized void sendMessage() {
        try {
            TimeUnit.SECONDS.sleep(4);  //延迟
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(Thread.currentThread().getName() + "sendMessage");
    }

    //普通同步方法
    public synchronized void call() {
        System.out.println(Thread.currentThread().getName() + "call");
    }

}
~~~

小结：

- synchronized 方法锁的是调用者   this  具体的一个手机
- static 方法锁的是模板  Class  唯一的一个模板

## 6.集合类不安全

### list 不安全

~~~java
//java.util.ConcurrentModificationException 并发修改异常!
public class ListTest {

    public static void main(String[] args) {
        //并发下 arrayList 是不安全的
        /**
         * 解决方案
         * 1. 使用vector解决
         * 2. List<String> arrayList = Collections.synchronizedList(new ArrayList<>());
         * synchronizedList 方法里面都有 synchronized 同步代码块
         * 3. List<String> arrayList = new CopyOnWriteArrayList<>();
         * CopyOnWriteArrayList 使用 ReentrantLock 同步，同时使用 Arrays 工具类的 copyOf 接口复制数组
         */
        //copyOnWrite 写入时复制  COW 计算机程序设计领域的一种优化策略
        //多个线程调用的时候, list, 读取的时候固定的,写入的时候,可能会覆盖
        //在写入的时候避免覆盖造成数据问题
        //读写分离
        //CopyOnWriteArrayList 比 vector牛逼在哪里

        List<String> arrayList = new CopyOnWriteArrayList<>();
        for (int i = 0; i < 100; i++) {
            new Thread(()->{
                arrayList.add(UUID.randomUUID().toString().substring(0,5));
                System.out.println(arrayList);
            },String.valueOf(i)).start();
        }
    }
}
~~~

### Set 不安全

~~~java
/**
 * 同理可证
 */
public class SetTest {

    public static void main(String[] args) {

//        Set<String> set = new HashSet<>();
        //如何解决hashSet线程安全问题
        //1. Set<String> set = Collections.synchronizedSet(new HashSet<>());
        // 方法里面都有 synchronized 同步代码块
        Set<String> set = new CopyOnWriteArraySet<>();
		// CopyOnWriteArraySet 实际用的是 CopyOnWriteArrayList 来存储数据，只是在添加数据时会使用 CopyOnWriteArrayList 的 addIfAbsent 方法来检查是否已经存在元素
        for (int i = 0; i < 100; i++) {
            new Thread(() -> {
                set.add(UUID.randomUUID().toString().substring(0, 5));
                System.out.println(set);
            }, String.valueOf(i)).start();
        }
    }
}
~~~

> **hashSet底层是什么? hashMap**

~~~java
public HashSet() {
    map = new HashMap<>();
}

// add 的本质就是 map 的 key key是无法重复的
public boolean add(E e) {
    return map.put(e, PRESENT)==null;
}
private static final Object PRESENT = new Object();//这是一个不变的值

~~~

### HashMap 不安全

回顾Map基本操作

![image-20200703104018606](https://gitee.com/xudongyin/img/raw/master/img/image-20200703104018606.png)

~~~java
package com.kuang.unsafe;

import java.util.Collections; import java.util.HashMap; import java.util.Map;
import java.util.UUID;
import java.util.concurrent.ConcurrentHashMap;

// ConcurrentModificationException
public class MapTest {
    public static void main(String[] args) {
        // map 是这样用的吗？ 不是，工作中不用 HashMap
        // 默认等价于什么？	new HashMap<>(16,0.75);
        // Map<String, String> map = new HashMap<>();
        //  唯一的一个家庭作业：研究ConcurrentHashMap的原理
        Map<String, String> map = new ConcurrentHashMap<>();

        for (int i = 1; i <=30; i++) { 
            new Thread(()->{
                map.put(Thread.currentThread().getName(),UUID.randomUUID().toString().substring( 0,5));
                System.out.println(map);
            },String.valueOf(i)).start();
        }
    }
}
~~~

## 7. Callable()

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200620153618662.png)

1. 可以有返回值
2. 可以抛出异常
3. 方法不同, run() => call()

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/2020062015363235.png)

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200620153643848.png)

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200620153655733.png)

~~~java
package com.kuang.callable;

import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException; 
import java.util.concurrent.FutureTask;
import java.util.concurrent.locks.ReentrantLock;

/**
*1、探究原理
*2、觉自己会用
*/
public class CallableTest {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        // new Thread(new Runnable()).start();
        // new Thread(new FutureTask<V>()).start();
        // new Thread(new FutureTask<V>( Callable )).start(); 
        // new Thread().start(); // 怎么启动Callable

        MyThread thread = new MyThread();
        FutureTask futureTask = new FutureTask(thread); // 适配类

        new Thread(futureTask,"A").start();
        new Thread(futureTask,"B").start(); // 只输出Acall()，结果会被缓存，效率高

        Integer o = (Integer) futureTask.get(); //这个get 方法可能会产生阻塞！把他放到最后
            // 或者使用异步通信来处理！
            System.out.println(o);

    }
}

class MyThread implements Callable<Integer> {

    @Override
    public Integer call() {
        System.out.println("call()"); // 会打印几个call
        // 耗时的操作
        return 1024;
    }
}
~~~

**细节：**

1、有缓存
2、结果可能需要等待，会阻塞！

## 8. 常用的辅助类

### CountDownLatch

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200620153710758.png)

~~~java
//计数器
public class CountDownLatchDemo {

    public static void main(String[] args) throws InterruptedException {
        // 倒计时总数是6, 必须要执行任务的时候,再使用!
        CountDownLatch countDownLatch = new CountDownLatch(6);
        for (int i = 0; i < 6; i++) {
            new Thread(()->{
                System.out.println(Thread.currentThread().getName() + " GO out");
                countDownLatch.countDown();     //数量减1
            },String.valueOf(i)).start();
        }
        countDownLatch.await();// 等待计数器归零,唤醒该线程，然后再向下执行
        System.out.println("close Door");
    }
}
~~~

原理:

countDownLatch.countDown(); //数量减1

countDownLatch.await();// 等待计数器归零,然后再向下执行

每次有线程调用countDown()数量-1,计数器变为0, countDownLatch.await();就会被唤醒,继续执行

### CyclicBarrier

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200620153726567.png)

```java
package com.xu.Others;

import java.util.concurrent.BrokenBarrierException;
import java.util.concurrent.CyclicBarrier;

public class TestCyclicBarrier {
    public static void main(String[] args) {
        CyclicBarrier cyclicBarrier = new CyclicBarrier(7, () -> {
            System.out.println("集齐七颗龙珠召唤神龙");
        });
//        CyclicBarrier cyclicBarrier = new CyclicBarrier(7);
        for (int i = 1; i <= 7; i++) {
            final int temp = i;//因为该变量会在循环时或者循环完，会消失，
         //线程延迟还没输出数据，保存在线程里，所以每个线程拥有不同的数据必须final修饰
            new Thread(()->{
                System.out.println(Thread.currentThread().getName() + "集齐了第" + temp + "个龙珠");
                try {
                    cyclicBarrier.await();//没有达到7之前会被wait，不会往下执行
                    //到达7， 才会执行cyclicBarrier的任务
                    System.out.println("ok");
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } catch (BrokenBarrierException e) {
                    e.printStackTrace();
                }
            }).start();
        }

    }
}
```

![image-20200703110723582](https://gitee.com/xudongyin/img/raw/master/img/image-20200703110723582.png)

### Semaphore

Semaphore：信号量

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200620153740695.png)

```java
package com.xu.Others;

import java.util.concurrent.Semaphore;
import java.util.concurrent.TimeUnit;

public class TestSemaphore {
    public static void main(String[] args) {
        //6车---3个停车位置
        Semaphore semaphore = new Semaphore(3);
        for (int i = 1; i <= 6; i++) {
            new Thread(() -> {
                try {
                    System.out.println("ok");
                    semaphore.acquire();//获得资源，如果资源已经被占满就等待资源释放
                    System.out.println(Thread.currentThread().getName() + "抢到了车位");
                    TimeUnit.SECONDS.sleep(2);
                    System.out.println(Thread.currentThread().getName() + "离开了车位");
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } finally {
                    semaphore.release();//释放资源，唤醒等待线程
                }

            },String.valueOf(i)).start();
        }
    }
}
```

**原理:**

semaphore.acquire(); //获取信号量,假设如果已经满了,等待信号量可用时被唤醒

semaphore.release(); //释放信号量

作用: 多个共享资源互斥的使用!并发限流,控制最大的线程数

## 9.读写锁

ReadWriteLock

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200620153755687.png)

```java
package com.xu.WriteAndRead;

import java.util.HashMap;
import java.util.Map;
import java.util.concurrent.locks.ReadWriteLock;
import java.util.concurrent.locks.ReentrantReadWriteLock;

public class Test {
    /**
 * 独占锁(写锁) 一次只能由一个线程占有
 * 共享锁(读锁) 一次可以有多个线程占有
 * readWriteLock
 * 读-读 可以共存
 * 读-写  不能共存
 * 写-写 不能共存
 */
    public static void main(String[] args) {
        Mycache2 mycache = new Mycache2();
        //创建写线程
        for (int i = 1; i <= 10; i++) {
            final int temp = i;
            new Thread(() -> {
                mycache.write(temp + "", temp + "");
            }, String.valueOf(i)).start();
        }
        //创建读线程
        for (int i = 1; i <= 10; i++) {
            final int temp = i;
            new Thread(() -> {
                mycache.read(temp + "");
            }, String.valueOf(i)).start();
        }
    }
}

//加读写锁
class Mycache2 {
    private volatile Map<String, String> map = new HashMap<>();
    private ReadWriteLock readWriteLock = new ReentrantReadWriteLock();

    //写,只有一个人能写
    public void write(String key, String value) {
        readWriteLock.writeLock().lock();

        try {
            System.out.println(Thread.currentThread().getName() + "写");
            map.put(key, value);
            System.out.println(Thread.currentThread().getName() + "写OK");
        } finally {
            readWriteLock.writeLock().unlock();
        }

    }

    //读，可以多个人读
    public void read(String key) {
        readWriteLock.readLock().lock();
        try {
            System.out.println(Thread.currentThread().getName() + "读");
            map.get(key);
            System.out.println(Thread.currentThread().getName() + "读OK");
        } finally {
         readWriteLock.readLock().unlock();
        }

    }
}

//没有任何锁
class Mycache {
    private volatile Map<String, String> map = new HashMap<>();

    //写
    public void write(String key, String value) {
        System.out.println(Thread.currentThread().getName() + "写");
        map.put(key, value);
        System.out.println(Thread.currentThread().getName() + "写OK");
    }

    //读
    public void read(String key) {
        System.out.println(Thread.currentThread().getName() + "读");
        map.get(key);
        System.out.println(Thread.currentThread().getName() + "读OK");
    }
}
```

## 10.阻塞队列

![img](https://gitee.com/xudongyin/img/raw/master/img/20200620153809279.png)

阻塞队列

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200620153819459.png)

**Blockqueue**

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200620153831272.png)

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/2020062015384260.png)

什么情况下我们会使用阻塞队列?   多线程并发处理,线程池!

**学会使用队列**

添加,移除

**四组API**

| 方式         | 抛出异常  | 不会抛出异常,有返回值 | 阻塞等待 | 超时等待                       |
| ------------ | --------- | --------------------- | -------- | ------------------------------ |
| 添加操作     | add()     | offer() 供应          | put()    | offer(obj,int,timeunit.status) |
| 移除操作     | remove()  | poll() 获得           | take()   | poll(int,timeunit.status)      |
| 判断队列首部 | element() | peek() 偷看,偷窥      |          |                                |

```java
package com.xu.Queue;

import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.TimeUnit;

public class testBlockingQueue {
    public static void main(String[] args) throws InterruptedException {
        test4();
    }
    /**
     * 等待，阻塞（等待超时）
     */
    public static void test4() throws InterruptedException {
        ArrayBlockingQueue<Object> arrayBlockingQueue4 = new ArrayBlockingQueue<>(3);
        System.out.println(arrayBlockingQueue4.offer("a"));
        System.out.println(arrayBlockingQueue4.offer("b"));
        System.out.println(arrayBlockingQueue4.offer("c"));
        System.out.println(arrayBlockingQueue4.offer("d",2, TimeUnit.SECONDS));//超时等待，false,不会阻塞
        System.out.println("_____________");
        System.out.println(arrayBlockingQueue4.poll());
        System.out.println(arrayBlockingQueue4.poll());
        System.out.println(arrayBlockingQueue4.poll());
        System.out.println(arrayBlockingQueue4.poll(2,TimeUnit.SECONDS));//超时等待，null，不会抛出异常
    }
    /**
     * 等待，阻塞（一直阻塞）
     */
    public static void test3() throws InterruptedException {
        ArrayBlockingQueue<Object> arrayBlockingQueue3 = new ArrayBlockingQueue<>(3);
       arrayBlockingQueue3.put("a");
       arrayBlockingQueue3.put("b");
       arrayBlockingQueue3.put("c");
//       arrayBlockingQueue3.put("d");// 队列没有位置了，一直阻塞
        System.out.println("_____________");
        System.out.println(arrayBlockingQueue3.take());
        System.out.println(arrayBlockingQueue3.take());
        System.out.println(arrayBlockingQueue3.take());
//        System.out.println(arrayBlockingQueue3.take());// 没有这个元素，一直阻塞
    }
    /**
     * 有返回值，不会抛出异常
     */
    public static void test2() {
        ArrayBlockingQueue<Object> arrayBlockingQueue2 = new ArrayBlockingQueue<>(3);
        System.out.println(arrayBlockingQueue2.offer("a"));
        System.out.println(arrayBlockingQueue2.offer("b"));
        System.out.println(arrayBlockingQueue2.offer("c"));
        System.out.println(arrayBlockingQueue2.offer("d"));//false,不会阻塞
        System.out.println("_____________");
        System.out.println(arrayBlockingQueue2.poll());
        System.out.println(arrayBlockingQueue2.poll());
        System.out.println(arrayBlockingQueue2.poll());
        System.out.println(arrayBlockingQueue2.poll());//null，不会抛出异常
    }
    /**
     * 抛出异常
     */
    public static void test() {

        // 队列的大小
        ArrayBlockingQueue<Object> arrayBlockingQueue = new ArrayBlockingQueue<>(3);
        System.out.println(arrayBlockingQueue.add("a"));
        System.out.println(arrayBlockingQueue.add("b"));
        System.out.println(arrayBlockingQueue.add("c"));
//        System.out.println(arrayBlockingQueue.add("d")); //IllegalStateException: Queue full
        System.out.println("_____________");
        System.out.println(arrayBlockingQueue.remove("b"));
        System.out.println(arrayBlockingQueue.remove());
        System.out.println(arrayBlockingQueue.remove());
//        System.out.println(arrayBlockingQueue.remove());//NoSuchElementException
    }
}
```

> **SynchronizedQueue 同步队列**

没有容量,

进去一个元素,必须等待取出来之后,才能再往里面放一个元素

put take

~~~java

/**
 * 同步队列
 * 和其他的lockQueue 不一样， SynchronousQueue 不存储元素
 * put了一个元素，必须从里面先take取出来，否则不能在put进去值！
 */
public class SyncQueue {

    public static void main(String[] args) {
        SynchronousQueue<String> synchronousQueue = new SynchronousQueue<>(); 
        //同步队列

        new Thread(()->{

            try {
                System.out.println(Thread.currentThread().getName() + "put 1");
                synchronousQueue.put("1");//没有被take，会阻塞
                System.out.println(Thread.currentThread().getName() + "put 2");
                synchronousQueue.put("2");
                System.out.println(Thread.currentThread().getName() + "put 3");
                synchronousQueue.put("3");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        },"T1").start();

        new Thread(()->{
            try {
                TimeUnit.SECONDS.sleep(3);
                System.out.println(Thread.currentThread().getName() + "=>" + synchronousQueue.take()); //没有put，会阻塞
                TimeUnit.SECONDS.sleep(3);
                System.out.println(Thread.currentThread().getName() + "=>" + synchronousQueue.take());
                TimeUnit.SECONDS.sleep(3);
                System.out.println(Thread.currentThread().getName() + "=>" + synchronousQueue.take());
            } catch (InterruptedException e) {
                e.printStackTrace();
            } finally {
            }
        },"T2").start();
    }
}
~~~

## 11.线程池

线程池: 三大方法,七大参数,4种拒绝策略

> **池化技术**

程序的运行，本质：占用系统的资源！ 优化资源的使用！=>池化技术

线程池、连接池、内存池、对象池///.	创建、销毁。十分浪费资源

池化技术：事先准备好一些资源，有人要用，就来我这里拿，用完之后还给我。

**线程池的好处:**

1、降低资源的消耗

2、提高响应的速度

3、方便管理。

**线程复用、可以控制最大并发数、管理线程**

### 线程池: 三大方法

![img](https://gitee.com/xudongyin/img/raw/master/img/20200620154935306.png)

~~~java
//Executors 工具类  3大方法
//使用了线程池之后要使用线程池创建线程
public class Demo01 {

    public static void main(String[] args) {
//        ExecutorService service = Executors.newSingleThreadExecutor();//单个线程
//        ExecutorService service = Executors.newFixedThreadPool(5);//创建一个固定的线程池的大小
        ExecutorService service = Executors.newCachedThreadPool();//可伸缩的，
        try {
            for (int i = 0; i < 10; i++) {
                service.execute(() -> {
                    System.out.println(Thread.currentThread().getName() + "ok");
                });
            }
            //线程池用完要关闭线程池
        } finally {
            service.shutdown();
        }
    }
}
~~~

### 7大参数

newSingleThreadExecutor构造器

~~~java
public static ExecutorService newSingleThreadExecutor() {
        return new FinalizableDelegatedExecutorService
            (new ThreadPoolExecutor(1, 1,
                                    0L, TimeUnit.MILLISECONDS,
                                    new LinkedBlockingQueue<Runnable>()));
    }
~~~

newFixedThreadPool构造器

~~~java
 public static ExecutorService newFixedThreadPool(int nThreads) {
        return new ThreadPoolExecutor(nThreads, nThreads,
                                      0L, TimeUnit.MILLISECONDS,
                                      new LinkedBlockingQueue<Runnable>());
    }
~~~

newCachedThreadPool构造器

~~~java
public static ExecutorService newCachedThreadPool() {
        return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                      60L, TimeUnit.SECONDS,
                                      new SynchronousQueue<Runnable>());
    }
~~~


**本质: 所有线程池最终都调用的ThreadPoolExecutor**

**ThreadPoolExecutor底层构造器**

~~~java
public ThreadPoolExecutor(int corePoolSize, //核心线程池大小
                              int maximumPoolSize,	//最大的线程池大小
                              long keepAliveTime,	// 超时了没有人调用就会释放
                              TimeUnit unit,	//时间单位
                              BlockingQueue<Runnable> workQueue,  //阻塞队列
                              ThreadFactory threadFactory,//线程工厂,创建线程的,一般不动
                              RejectedExecutionHandler handler) {//拒绝策略
        if (corePoolSize < 0 ||
            maximumPoolSize <= 0 ||
            maximumPoolSize < corePoolSize ||
            keepAliveTime < 0)
            throw new IllegalArgumentException();
        if (workQueue == null || threadFactory == null || handler == null)
            throw new NullPointerException();
        this.acc = System.getSecurityManager() == null ? 
        null :
        AccessController.getContext(); 
        this.corePoolSize = corePoolSize;
        this.maximumPoolSize = maximumPoolSize;
        this.workQueue = workQueue;
        this.keepAliveTime = unit.toNanos(keepAliveTime);
        this.threadFactory = threadFactory;
        this.handler = handler;
    }
~~~

![](https://gitee.com/xudongyin/img/raw/master/img/image-20200703120220440.png)

> **手动创建线程池**

```java
package com.xu.Pool;

import java.util.concurrent.*;

public class testThreadPoolExecutor {
    public static void main(String[] args) {
        ExecutorService threadPoolExecutor = new ThreadPoolExecutor(
                2,//核心线程数
                4,//最大线程数
                3,//多久没调用就释放池
                TimeUnit.SECONDS,//超时单位
                new ArrayBlockingQueue<>(6),//阻塞队列大小
                Executors.defaultThreadFactory(),  // 线程工厂，创建线程的，默认不用改
                //队列满了，线程池满了，还有其他任务过来时拒绝策略
//                new ThreadPoolExecutor.AbortPolicy()//满了，不处理，抛出异常
//                new ThreadPoolExecutor.CallerRunsPolicy()//由调用线程（提交任务的线程）处理该任务
//              new ThreadPoolExecutor.DiscardPolicy()//满了，丢掉任务，不抛出异常
                new ThreadPoolExecutor.DiscardOldestPolicy()//抛弃队列中等待最久的任务，然后把当前任务加入队列中尝试再次提交当前任务
        );

        try {
            // 最大承载：Deque + max  等待队列容量和最大线程数之和
            // 超过 RejectedExecutionException  抛不抛出异常看拒绝策略
            for (int i = 0; i <12; i++) {
                // 使用了线程池之后，使用线程池来创建线程
                threadPoolExecutor.execute(() -> {
                    System.out.println(Thread.currentThread().getName() + "run");
                });
            }
        } catch (Exception exception) {
            exception.printStackTrace();
        } finally {
            // 线程池用完，程序结束，关闭线程池
            threadPoolExecutor.shutdown();
        }

    }
}
```

### 四种拒绝策略

~~~java
/**
 * new ThreadPoolExecutor.AbortPolicy() 超出最大处理线程抛出异常
 * new ThreadPoolExecutor.CallerRunsPolicy() 哪个线程创建就由那个线程执行
 * new ThreadPoolExecutor.DiscardPolicy() 队列满了，丢弃任务，不会抛出异常
 * new ThreadPoolExecutor.DiscardOldestPolicy() 抛弃队列中等待最久的任务，然后把当前任务加入队列中尝试再次提交当前任务
 */
~~~

> **小结和扩展**

了解:最大线程到底应该如何定义

1. CPU密集型 几核,就是几,可以保证CPU的效率最高
2. IO 密集型 判断程序中十分耗I/O的线程, 大于两倍

~~~java
//获取电脑处理器数
System.out.println(Runtime.getRuntime().availableProcessors());
~~~

## 12.四大函数式接口

新时代的程序员：lambda表达式、链式编程、函数式接口、Stream流式计算

> **函数式接口：只有一个方法的接口**

~~~java
@FunctionalInterface 
public interface Runnable {
	public abstract void run();
}
// 泛型、枚举、反射
// lambda表达式、链式编程、函数式接口、Stream流式计算
// 超级多FunctionalInterface
// 简化编程模型，在新版本的框架底层大量应用！
// foreach(消费者类的函数式接口)
~~~

![image-20200703122022024](https://gitee.com/xudongyin/img/raw/master/img/image-20200703122022024.png)



### Function函数式接口

![image-20200703122144881](https://gitee.com/xudongyin/img/raw/master/img/image-20200703122144881.png)

~~~java
package com.kuang.function;

import java.util.function.Function;

/**
*Function 函数型接口, 有一个输入参数，有一个输出
*只要是 函数型接口 可以 用 lambda表达式简化
*/
public class Demo01 {
    public static void main(String[] args) {
        //
        //	Function<String,String> function = new Function<String,String>() {
        //	@Override
        //	public String apply(String str) {
        //	return str;
        //	}
        //	};

        Function<String,String> function = (str)->{return str;};

        System.out.println(function.apply("asd"));
    }
}
~~~

### Predicate 断定型接口

![image-20200703122607500](https://gitee.com/xudongyin/img/raw/master/img/image-20200703122607500.png)

~~~java
package com.kuang.function;

import java.util.function.Predicate;

/**
* 断定型接口：有一个输入参数，返回值只能是 布尔值！
*/
public class Demo02 {
    public static void main(String[] args) {
        // 判断字符串是否为空
        //	Predicate<String> predicate = new Predicate<String>(){
        ////	@Override
        ////	public boolean test(String str) {
        ////	return str.isEmpty();
        ////	}
        ////	};

        Predicate<String> predicate = (str)->{return str.isEmpty(); };   
        System.out.println(predicate.test(""));

    }
}
~~~

### Consumer 消费型接口

![image-20200703122935590](https://gitee.com/xudongyin/img/raw/master/img/image-20200703122935590.png)

~~~java
package com.kuang.function;

import java.util.function.Consumer;

/**
* Consumer 消费型接口: 只有输入，没有返回值
*/
public class Demo03 {
    public static void main(String[] args) {
        //	Consumer<String> consumer = new Consumer<String>() {
        //	@Override
        //	public void accept(String str) {
        //	System.out.println(str);
        //	}
        //	};
        Consumer<String> consumer = (str)->{System.out.println(str);};
        consumer.accept("sdadasd");

    }
}
~~~

### Supplier 供给型接口

![image-20200703123928174](https://gitee.com/xudongyin/img/raw/master/img/image-20200703123928174.png)

~~~java
package com.kuang.function;

import java.util.function.Supplier;

/**
* Supplier 供给型接口 没有参数，只有返回值
*/
public class Demo04 {
    public static void main(String[] args) {
        //	Supplier supplier = new Supplier<Integer>() {
        //	@Override
        //	public Integer get() {
        //	System.out.println("get()");
        //	return 1024;
        //	}
        //	};

        Supplier supplier = ()->{ return 1024; }; 
        System.out.println(supplier.get());
    }
}
~~~

## 13.Stream 流式计算

> 什么是流式计算

**大数据：存储 + 计算**

集合、MySQL 本质就是存储东西的； 计算都应该交给流来操作！

![image-20200703134801158](https://gitee.com/xudongyin/img/raw/master/img/image-20200703134801158.png)

~~~java
package com.kuang.stream;

import java.util.Arrays; 
import java.util.List;

/**
*题目要求：一分钟内完成此题，只能用一行代码实现！
*现在有5个用户！筛选：
*1、ID 必须是偶数
*2、年龄必须大于23岁
*3、用户名转为大写字母
*4、用户名字母倒着排序
*5、只输出一个用户！
*/
public class Test {
    public static void main(String[] args) { 
        User u1 = new User(1,"a",21);
        User u2 = new User(2,"b",22); 
        User u3 = new User(3,"c",23); 
        User u4 = new User(4,"d",24);
        User u5 = new User(6,"e",25);
        // 集合就是存储
        List<User> list = Arrays.asList(u1, u2, u3, u4, u5);

        // 计算交给Stream流
        // lambda表达式、链式编程、函数式接口、Stream流式计算
        list.stream()
        .filter(u->{return u.getId()%2==0;})
            .filter(u->{return u.getAge()>23;})
            .map(u->{return u.getName().toUpperCase();})
            .sorted((uu1,uu2)->{return uu2.compareTo(uu1);})
            .limit(1) //取前几个元素，数目可以大于元素总个数
            .forEach(System.out::println);
    }
}
~~~

## 14. ForkJoin

> 什么是ForkJoin

ForkJoin在JDK1.7,并行执行任务,大数据量!

大数据: Map Reduce( 把大任务拆分成小任务)

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200620153942645.png)

> ForkJoin特点: 工作窃取

这个里面维护的是一个双端队列

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200620153954104.png)

> ForkJoin

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200620154007467.png)

~~~java
package com.kuang.forkjoin;

import java.util.concurrent.RecursiveTask;

/**
*求和计算的任务！
*3000	6000（ForkJoin）	9000（Stream并行流）
* 如何使用 forkjoin
    * 1、forkjoinPool 通过它来执行
    * 2、计算任务 forkjoinPool.execute(ForkJoinTask task)
    * 3. 计算类要继承 ForkJoinTask
    */
public class ForkJoinDemo extends RecursiveTask<Long> {

    private Long start;	// 1
    private Long end;	// 1990900000

    // 临界值
    private Long temp = 10000L;

    public ForkJoinDemo(Long start, Long end) { 
        this.start = start;
        this.end = end;
    }

    // 计算方法
    @Override
    protected Long compute() { 
        if ((end-start)<temp){
            Long sum = 0L;
            for (Long i = start; i <= end; i++) { 
                sum += i;
            }
            return sum;
        }else { // forkjoin 递归
            long middle = (start + end) / 2; // 中间值
            ForkJoinDemo task1 = new ForkJoinDemo(start, middle); 
            task1.fork(); // 拆分任务，把任务压入线程队列
            ForkJoinDemo task2 = new ForkJoinDemo(middle+1, end); 
            task2.fork(); // 拆分任务，把任务压入线程队列
            return task1.join() + task2.join();
        }
    }
}
~~~

测试

~~~java
package com.xu.ForkJoin;

import java.util.concurrent.ExecutionException;
import java.util.concurrent.ForkJoinPool;
import java.util.concurrent.ForkJoinTask;
import java.util.stream.LongStream;

public class test {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
//       test1();//611
//        test2();//617
        test3();//618
    }

    public static void test1() {
        long start = System.currentTimeMillis();
        long sum = 0l;
        for (long i = 0l; i <=10_0000_0000l ; i++) {
            sum+=i;
        }
        long end = System.currentTimeMillis();
        System.out.println("sum="+sum+"运行时间；"+(end-start));
    }
    public static void test2() throws ExecutionException, InterruptedException {
        long start = System.currentTimeMillis();
        Long sum;
        ForkJoinPool forkJoinPool = new ForkJoinPool();
        //forkjoinDemo 是自己创建的计算类，参照上面创建
        forkjoinDemo forkjoinDemo = new forkjoinDemo(0l, 10_0000_0000l);
        ForkJoinTask<Long> task = forkJoinPool.submit(forkjoinDemo);// 提交任务
        sum = task.get();
        long end = System.currentTimeMillis();
        System.out.println("sum="+sum+"运行时间；"+(end-start));
    }
    public static void test3() {
        long start = System.currentTimeMillis();
        // Stream并行流  range() 开区间  ()  ，rangeClosed() 闭区间左开右闭  (]
        long sum = LongStream.rangeClosed(0l, 10_0000_0000l).parallel().reduce(1, Long::sum);
        long end = System.currentTimeMillis();
        System.out.println("sum="+sum+"运行时间；"+(end-start));
    }
}
~~~

## 15. 异步回调

> Future设计的初衷: 对将来的某个事件的结果进行建模

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200620154022618.png)

```java
package com.xu.CompletableFutrue;

import java.util.concurrent.CompletableFuture;
import java.util.concurrent.ExecutionException;

/**
*异步调用： CompletableFuture
*  异步执行
*  成功回调
*  失败回调
*/
public class test {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
       //没有返回值的 runAsync 异步回调
//        CompletableFuture<Void> completableFuture=CompletableFuture.runAsync(()->{
//            System.out.println(Thread.currentThread().getName() + "run");
//            try {
//                TimeUnit.SECONDS.sleep(2);
//            } catch (InterruptedException e) {
//                e.printStackTrace();
//            }
//        });
//        completableFuture.get();//阻塞，获取执行结果
//        System.out.println("1111");
        //有返回值的 supplyAsync 异步回调
        // ajax，成功和失败的回调
        // 返回的是错误信息；
        CompletableFuture<Integer> completableFuture = CompletableFuture.supplyAsync(()->{
            System.out.println(Thread.currentThread().getName() + "run");
            int i  =10/0;
            return 1024;
        });
        System.out.println(completableFuture.whenComplete((u, t) -> {
  //接收什么信息跟参数位置有关，第一个参数接收正确运行结果，第二个接收错误运行的结果
            System.out.println("u=" + u);
            System.out.println("t=" + t);
        }).exceptionally((e) -> {
            System.out.println(e.getMessage());
            return 111;  // 可以获取到错误的返回结果
        }).get());
    }
}
/*  运行结果
ForkJoinPool.commonPool-worker-1run
u=null
t=java.util.concurrent.CompletionException: java.lang.ArithmeticException: / by zero
java.lang.ArithmeticException: / by zero
111
*/
```

## 16.JMM

> 请你谈谈你对Volate的理解

Volate是java虚拟机提供**轻量级的同步机制**

- 保证可见性
- **不保证原子性**
- 禁止指令重排

> JMM是什么

JMM: java内存模型,不存在的东西,概念!约定!

关于JMM的一些同步的约定：

- 线程解锁前,必须把共享变量**立刻**刷回主存
- 线程加锁前,必须读取主存中的最新值到工作内存中!
- 加锁和解锁必须是同一把锁

线程 工作内存 主内存

8种操作：（这里的 write 和 store 交换位置）

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200620154037142.png)

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200620154050225.png)

| 操作              | 说明                                                         |
| :---------------- | ------------------------------------------------------------ |
| lock   （锁定）   | 作用于主内存的变量，把一个变量标识为线程独占状态             |
| unlock （解锁）   | 作用于主内存的变量，它把一个处于锁定状态的变量释放出来，释放后的变量才可以被其他线程锁定 |
| read   （读取）   | 作用于主内存变量，它把一个变量的值从主内存传输到线程的工作内存中，以便随后的load动作使用 |
| load   （载入）   | 作用于工作内存的变量，它把read操作从主存中变量放入工作内存中 |
| use    （使用）   | 作用于工作内存中的变量，它把工作内存中的变量传输给执行引擎，每当虚拟机遇到一个需要使用到变量的值，就会使用到这个指令 |
| assign   （赋值） | 作用于工作内存中的变量，它把一个从执行引擎中接受到的值放入工作内存的变量副本中 |
| store  （存储）   | 作用于主内存中的变量，它把一个从工作内存中一个变量的值传送到主内存中，以便后续的write使用 |
| write	（写入） | 作用于主内存中的变量，它把store操作从工作内存中得到的变量的值放入主内存的变量中 |

**八种规则**:

- 不允许read和load、store和write操作之一单独出现。即使用了read必须load，使用了store必须write

- 不允许线程丢弃他最近的assign操作，即工作变量的数据改变了之后，必须告知主存

- 不允许一个线程将没有assign的数据从工作内存同步回主内存

- 一个新的变量必须在主内存中诞生，不允许工作内存直接使用一个未被初始化的变量。就是对变量实施use、store操作之前，必须经过assign和load操作

- 一个变量同一时间只有一个线程能对其进行lock。多次lock后，必须执行相同次数的unlock才能解锁

- 如果对一个变量进行lock操作，会清空所有工作内存中此变量的值，在执行引擎使用这个变量前，必须重新load或assign操作初始化变量的值

- 如果一个变量没有被lock，就不能对其进行unlock操作。也不能unlock一个被其他线程锁住的变量

- 对一个变量进行unlock操作之前，必须把此变量同步回主内存

  问题: 程序不知道主内存中的值已经被修改过了

  <img src="https://gitee.com/xudongyin/img/raw/master/img/image-20200703145946201.png" alt="image-20200703145946201" style="zoom:150%;" />

  

## 17. Volatile

### 保证可见性

~~~java
public class JMMDemo {
    //不加volatile 程序就会死循环
    //加上volatile 可以保证可见性
    private volatile static int number = 0;

    public static void main(String[] args) {


        new Thread(()->{  // 线程 1 对主内存的变化不知道的
            while(number == 0){

            }
        }).start();

        try {
            TimeUnit.SECONDS.sleep(2);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
        }

        number = 1;

        System.out.println(number);
    }
}
~~~

### 不保证原子性

原子性: 不可分割

线程A在执行任务的时候.不能被打扰,也不能被分割,要么同时成功,要么同时失败

~~~java
//测试不保证原子性
public class VDemo {
	// volatile 不保证原子性
    private volatile static int num = 0;
    
    public static void add(){
        num++;
    }

    public static void main(String[] args) {
       //理论上num结果应该为 2 万
        for (int i = 0; i < 20; i++) {
            new Thread(()->{
                for (int j = 0; j < 1000; j++) {
                    add();
                }
            }).start();
        }

        while (Thread.activeCount() > 2){  // main线程	gc垃圾回收线程 
            Thread.yield();
        }
        System.out.println(num);

    }
}
~~~

**如果不加lock和synchronized,如何保证原子性**

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/202006201541096.png)

使用原子类,解决原子性问题

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200620154119551.png)

~~~java
//测试不保证原子性
public class VDemo {

    //原子类的int
    private volatile static AtomicInteger num = new AtomicInteger(0);
    // AtomicInteger 调用的是底层的 CAS
    
    public static void add(){
        // num++; // 不是一个原子性操作
        num.getAndIncrement(); // AtomicInteger + 1 方法， CAS
    }

    public static void main(String[] args) {

        for (int i = 0; i < 20; i++) {

            new Thread(()->{
                for (int j = 0; j < 1000; j++) {
                    add();
                }
            }).start();
        }

        while (Thread.activeCount() > 2){
            Thread.yield();
        }
        System.out.println(num);

    }
  
}
~~~

这些类的底层都直接和操作系统挂钩 ! 在内存中修改值! UnSafe类是一个很特殊的存在

> 指令重排

什么是指令重排: **你写的程序,计算机并不是按照指定的的步骤执行**

源代码—>编译器优化源代码–>指令并行也可能会重排—>内存系统也会重排执行

**处理器在进行指令重排的时候，考虑：数据之间的依赖性！**

~~~java
int x = 1; // 1
int y = 2; // 2 
x = x + 5; // 3 
y = x * x; // 4

我们所期望的：1234	但是可能执行的时候回变成 2134	1324
可不可能是	4123！
~~~

可能造成影响的结果： a b x y 这四个值默认都是 0；

| 线程A | 线程B |
| ----- | ----- |
| x=a   | y=b   |
| b=1   | a=2   |

正常的结果： x = 0；y = 0；但是可能由于指令重排

| 线程A | 线程B |
| ----- | ----- |
| b=1   | a=2   |
| x=a   | y=b   |

 指令重排导致的诡异结果： x = 2；y = 1；

**volatile 可以避免指令重排**：

内存屏障。CPU指令。作用：

1、保证特定的操作的执行顺序！

2、可以保证某些变量的内存可见性 （利用这些特性volatile实现了可见性）

![img](https://gitee.com/xudongyin/img/raw/master/img/wps2.jpg)

**Volatile 是可以保证可见性, 不能保证原子性,由于内存屏障可以避免指令重排的现象产生 !**

## 18.单例模式

### 饿汉模式

~~~java
package com.kuang.single;

// 饿汉式单例
public class Hungry {

    // 可能会浪费空间
    private Hungry(){

    }

    private final static Hungry HUNGRY = new Hungry();

    public static Hungry getInstance(){ 
        return HUNGRY;
    }

}
~~~

### 懒汉模式

~~~java
package com.czp.single;

//单线程安全
public class LazyMan {

    private static LazyMan lazyMan = null;

    private LazyMan(){
    }

    public static LazyMan getInstance(){
        if(lazyMan == null){
            lazyMan = new LazyMan();
        }
        return lazyMan;
    }
}
~~~

### DCL 懒汉式

~~~java
package com.czp.single;

import java.lang.reflect.Constructor;

public class LazyManThread {

    private static volatile LazyManThread lazyManThread = null;

    private static boolean isExist = false;
      //执行一次构造函数就把标志位反置为true，第二次执行构造函数就抛出异常
    private LazyManThread() {
        synchronized (LazyManThread.class) {
            if (!isExist) {
                isExist = true;
            } else {
                throw new RuntimeException("禁止使用反射创建该对象");
            }
        }

    }

    // 双重检测锁模式的 懒汉式单例	DCL懒汉式
    public static LazyManThread getInstance() {
        //if只会判断一次,当两个线程同时判断时一个线程就会在同步代码块中等待
        if (lazyManThread == null) {
            //不直接使用同步的原因,提高执行效率
            synchronized (LazyManThread.class) {
                if (lazyManThread == null) {
                    lazyManThread = new LazyManThread();  // 不是一个原子性操作
                }
            }
        }

        /**
         * 由于对象创建不是原子性操作
         * 1. 分配内存空间
         * 2. 使用构造器创建对象
         * 3. 将对象指向内存空间
         */
        /**
         * 可能会发生指令重排
         * 123
         * 132
         * 这是就需使用volatile关键字来防止指令重排
         */
        return lazyManThread;
    }

 	// 反射！
    public static void main(String[] args) throws Exception {
//        LazyManThread instance = LazyManThread.getInstance();

        Constructor<LazyManThread> declaredConstructor = LazyManThread.class.getDeclaredConstructor();
        declaredConstructor.setAccessible(true);
        LazyManThread lazyManThread = declaredConstructor.newInstance();
        LazyManThread instance = declaredConstructor.newInstance();
        System.out.println(instance);
        System.out.println(lazyManThread);
    }
}
~~~

### 静态内部类

~~~java

public class LazyMan1 {


    private LazyMan1() {}

    public static final LazyMan1 getInstance(){
        return innerClass.LAZY_MAN_1;
    }

    public static class innerClass {
            private static final LazyMan1 LAZY_MAN_1 = new LazyMan1();
    }
}
~~~

### 单例不安全,反射

~~~java
package com.xu.SingleInstance;

import java.lang.reflect.Constructor;
import java.lang.reflect.InvocationTargetException;

public class LazyMan {
    private volatile static LazyMan lazyMan;
   
  //volatile 保证不会指令重排
    private LazyMan() {
        System.out.println("new lazyman()");
    }

    public static LazyMan getLazyMan() {
        if (lazyMan == null) {
            synchronized (LazyMan.class) {
                if (lazyMan == null) {
                    lazyMan = new LazyMan();//不是原子性操作
                    /**
                     * 1，分配内存空间
                     * 2，执行构造函数，初始化对象
                     * 3，把这个对象指向这个空间
                     *
                     * 正常情况是123
                     * 但虚拟机会优化编译，指令重排132
                     */
                }
            }
        }
        return lazyMan;
    }

    public static void main(String[] args) throws NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException {
//        LazyMan lazyMan = getLazyMan();
        Constructor<LazyMan> declaredConstructor = LazyMan.class.getDeclaredConstructor(null);
       declaredConstructor.setAccessible(true);
        LazyMan lazyMan1 = declaredConstructor.newInstance();
        LazyMan lazyMan2 = declaredConstructor.newInstance();
        System.out.println(lazyMan2);
        System.out.println(lazyMan1);
    }
}
/*
没有做特殊处理的单例在反射下可以随便创建实例
输出结果：
new lazyman()
new lazyman()
com.xu.SingleInstance.LazyMan@14ae5a5
com.xu.SingleInstance.LazyMan@7f31245a
*/
~~~

### 枚举

```java
package com.xu.SingleInstance;

import java.lang.reflect.Constructor;
import java.lang.reflect.InvocationTargetException;

public class Enum {
    public static void main(String[] args) throws InterruptedException, NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException {
        //无参构造函数，枚举默认
        Constructor<instance> constructor = Instance.class.getDeclaredConstructor(String.class,int.class);
        //有参构造函数，最后一个String.class 为参数类型
        Constructor<instance> constructor = Instance.class.getDeclaredConstructor(String.class,int.class,String.class);
        constructor.setAccessible(true);
        Instance instance = constructor.newInstance();
         //Exception in thread "main" java.lang.IllegalArgumentException: Cannot reflectively create enum objects   不能通过反射创建实例
    }
}
// enum 是一个什么？ 本身也是一个Class类
public enum Instance {
    ;//注意这里有分号
    private  String name;

    Instance(){
        
    }
    Instance(String name){
        this.name = name;
    }
}
```
下面是探究枚举类构造函数，以及反编译枚举类的源码

~~~java
package com.kuang.single;

import java.lang.reflect.Constructor;
import java.lang.reflect.InvocationTargetException;

// enum 是一个什么？ 本身也是一个Class类
public enum EnumSingle {

    INSTANCE;

    public EnumSingle getInstance(){ 
        return INSTANCE;
    }
}

class Test{

    public static void main(String[] args) throws NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException {
        EnumSingle instance1 = EnumSingle.INSTANCE; 
        Constructor<EnumSingle> declaredConstructor =
            EnumSingle.class.getDeclaredConstructor(String.class,int.class); 
        declaredConstructor.setAccessible(true);
        EnumSingle instance2 = declaredConstructor.newInstance();

        // NoSuchMethodException: com.kuang.single.EnumSingle.<init>() System.out.println(instance1);
        System.out.println(instance2);
    }
~~~

<img src="https://gitee.com/xudongyin/img/raw/master/img/image-20200708101923143.png" alt="image-20200708101923143" style="zoom:150%;" />

枚举类型的最终反编译源码：

~~~java
// Decompiled by Jad v1.5.8g. Copyright 2001 Pavel Kouznetsov.
// Jad home page: http://www.kpdus.com/jad.html
// Decompiler options: packimports(3)
// Source File Name:	EnumSingle.java

package com.kuang.single;

public final class EnumSingle extends Enum
{

    public static EnumSingle[] values()
    {
        return (EnumSingle[])$VALUES.clone();
    }

    public static EnumSingle valueOf(String name)
    {
        return (EnumSingle)Enum.valueOf(com/kuang/single/EnumSingle, name);
    }

    private EnumSingle(String s, int i)
    {
        super(s, i);
    }

    public EnumSingle getInstance()
    {
        return INSTANCE;
    }

    public static final EnumSingle INSTANCE; 
    private static final EnumSingle $VALUES[];

    static
    {
        INSTANCE = new EnumSingle("INSTANCE", 0);
        $VALUES = (new EnumSingle[] { INSTANCE });
    }
}
~~~



## 19.深入理解CAS

~~~java
package com.kuang.cas;

import java.util.concurrent.atomic.AtomicInteger;

public class CASDemo {

    // CAS	compareAndSwarp : 比较并交换！
    public static void main(String[] args) {
         // 期望、更新
        // public final boolean compareAndSet(int expect, int update)
        // 如果我期望的值达到了，那么就更新，否则，就不更新, CAS 是CPU的并发原语！
        AtomicInteger atomicInteger = new AtomicInteger(1);
        System.out.println(atomicInteger.compareAndSet(1, 2));//true
        System.out.println(atomicInteger.get()); //2
       System.out.println(atomicInteger.getAndIncrement());//其实是3,返回旧值，输出2
        System.out.println(atomicInteger.get());//3
        System.out.println(atomicInteger.compareAndSet(1, 2));//false
        System.out.println(atomicInteger.get());//3

    }
}
~~~

### Unsafe 类

<img src="https://gitee.com/xudongyin/img/raw/master/img/image-20200703162742401.png" alt="image-20200703162742401" style="zoom:150%;" />

<img src="https://gitee.com/xudongyin/img/raw/master/img/image-20200703162826398.png" alt="image-20200703162826398" style="zoom:150%;" />

<img src="https://gitee.com/xudongyin/img/raw/master/img/image-20200703163314070.png" alt="image-20200703163314070" style="zoom:150%;" />

**CAS ：  比较当前工作内存中的值和主内存中的值，如果这个值是期望的，那么则执行操作！如果不是就一直循环！**

**缺点：**

1、 循环会耗时
2、一次性只能保证一个共享变量的原子性
3、ABA问题

### CAS：ABA问题（狸猫换太子）

![image-20200703164013726](https://gitee.com/xudongyin/img/raw/master/img/image-20200703164013726.png)

~~~java
package com.kuang.cas;

import java.util.concurrent.atomic.AtomicInteger;

public class CASDemo {

    // CAS	compareAndSwarp : 比较并交换！
    public static void main(String[] args) {
        AtomicInteger atomicInteger = new AtomicInteger(2020);
        // 期望、更新
        // public final boolean compareAndSet(int expect, int update)
        // 如果我期望的值达到了，那么就更新，否则，就不更新, CAS 是CPU的并发原语！
        // ============== 捣乱的线程 ==================
        System.out.println(atomicInteger.compareAndSet(2020, 2021));
        System.out.println(atomicInteger.get());

        System.out.println(atomicInteger.compareAndSet(2021, 2020));
        System.out.println(atomicInteger.get());

        // ============== 期望的线程 ==================
        System.out.println(atomicInteger.compareAndSet(2020, 6666));
        System.out.println(atomicInteger.get());
    }
}
~~~

## 20 . 原子引用

> 解决ABA问题, 引入原子引用 ! 对应的思想: 乐观锁

带版本号的原子操作 !

~~~java
package com.xu.CASdemo;

import java.util.concurrent.TimeUnit;
import java.util.concurrent.atomic.AtomicStampedReference;

public class AtomicStampedReferenceDemo {
    public static void main(String[] args) {
        //AtomicStampedReference 注意，如果泛型是一个包装类，注意对象的引用问题
        AtomicStampedReference<Long> atomicStampedReference = new AtomicStampedReference(1L, 1); //这里1是版本号
        new Thread(()->{//模拟狸猫换太子
            // 获得版本号
            System.out.println("a1->"+atomicStampedReference.getStamp());
            try {
                TimeUnit.SECONDS.sleep(1);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(atomicStampedReference.compareAndSet(1L,
                    2L, atomicStampedReference.getStamp(),
                    atomicStampedReference.getStamp() + 1));
            System.out.println("a2->" + atomicStampedReference.getStamp());
            System.out.println(atomicStampedReference.compareAndSet(2L,
                    1L, atomicStampedReference.getStamp(),
                    atomicStampedReference.getStamp() + 1));
            System.out.println("a3->" + atomicStampedReference.getStamp());

        },"a").start();
        
        // 乐观锁的原理相同
        new Thread(()->{//狸猫换太子后，看能不能进行交换操作
            int stamp = atomicStampedReference.getStamp();// 获得版本号
            System.out.println("b1->" +stamp); //此时版本还是1
            try {
                TimeUnit.SECONDS.sleep(2); //让前面的线程换完太子
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(atomicStampedReference.compareAndSet(1L,
                    6L,stamp,
                    stamp+ 1));//此时版本号已经改变不符，所以交换不成功
            //经历过狸猫换太子后版本号变为了3
            System.out.println("b2->"+atomicStampedReference.getStamp());
        },"b").start();
    }
}
~~~

**注意：Integer 使用了对象缓存机制，默认范围是-128~127，推荐使用静态工厂的方法valueOf 获取对象实例，而不是new，因为valueOf 使用缓存，而new一定会创建新的对象分配新的内存空间；**

<img src="https://gitee.com/xudongyin/img/raw/master/img/image-20200703165612156.png" alt="image-20200703165612156" style="zoom:150%;" />

## 21.各种锁的理解

### 公平锁, 非公平锁

公平锁: 非常公平,先来后到,不允许插队

非公平锁: 非常不公平, 允许插队

~~~java
public ReentrantLock() {
        sync = new NonfairSync(); //无参默认非公平锁
    }
 	public ReentrantLock(boolean fair) {
        sync = fair ? new FairSync() : new NonfairSync();//传参为true为公平锁
    }
~~~

### 可重入锁 ( 递归锁 )

释义: 可重入锁指的是可重复可递归调用的锁，在外层使用锁之后，在内层仍然可以使用，并且不发生死锁（前提得是同一个对象或者class），这样的锁就叫做可重入锁

![image-20200703170451498](https://gitee.com/xudongyin/img/raw/master/img/image-20200703170451498.png)

> **synchronized版本的可重入锁**

~~~java

public class TestLock {

    public static void main(String[] args) {
        TestPhone phone = new TestPhone();
        new Thread(()->{
	    //在调用sendMessage的方法时已经为phone加上了一把锁
        //而call方法由为其加上了一把锁
            phone.sendMessage();
        }).start();
    }
}

class TestPhone {
	
    public synchronized void sendMessage() {
        
        System.out.println(Thread.currentThread().getName() + "sendMessage");
        call();
    }

    public synchronized void call() {

        System.out.println(Thread.currentThread().getName() + "call");
    }
}
~~~

> **Lock版本的可重入锁**

~~~java
package com.kuang.lock;

import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Demo02 {
    public static void main(String[] args) { 
        Phone2 phone = new Phone2();

        new Thread(()->{
            phone.sms();
        },"A").start();

        new Thread(()->{
            phone.sms();
        },"B").start();
    }
}

class Phone2{
    Lock lock = new ReentrantLock();

    public void sms(){
        lock.lock(); // 细节问题：lock.lock(); lock.unlock(); 
        // lock 锁必须配对，否则就会死在里面
        lock.lock(); 
        try {
            System.out.println(Thread.currentThread().getName() + "sms"); 
            call(); // 这里也有锁
        } catch (Exception e) { 
            e.printStackTrace();
        } finally {
            lock.unlock(); 
            lock.unlock();
        }
    }

    public void call(){

        lock.lock();
        try {
            System.out.println(Thread.currentThread().getName() + "call");
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
}

~~~

### 自旋锁

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200620154350376.png)

~~~java
package com.xu.LOCK;

import java.util.concurrent.TimeUnit;
import java.util.concurrent.atomic.AtomicReference;

public class Spinlock {
    public static void main(String[] args) throws InterruptedException {
        Mylock mylock = new Mylock();
        new Thread(()->{
            mylock.lock();
            try {
                System.out.println("a");
                TimeUnit.SECONDS.sleep(5);
            } catch (Exception e) {
                e.printStackTrace();
            }
            finally {
                mylock.unlock();
            }
        },"A").start();
        TimeUnit.SECONDS.sleep(1);
        new Thread(()->{
            mylock.lock();
            try {
                System.out.println("b");
               TimeUnit.SECONDS.sleep(6);
            } catch (Exception e) {
                e.printStackTrace();
            }
            finally {
                mylock.unlock();
            }
        },"B").start();
    }
}

class Mylock {
    AtomicReference<Thread> atomicReference = new AtomicReference<>();
    //对象为null，不带版本号的，AtomicStampedReference才是带版本号的类

    public void lock() {
        Thread thread =Thread.currentThread();//对应的线程
        while (!atomicReference.compareAndSet(null, thread)) {
             //B的线程执行到这里就会自旋，直到等到A线程执行unlock（）解锁
            //这个自旋主要是对线程B起作用，对线程A不起作用，因为
            // atomicReference.compareAndSet(null, thread)对线程A为真
            //因为atomicReference刚开始初始值为null
            System.out.println(thread.getName()+"->b");
        }
        System.out.println(thread.getName()+"->lock");
    }

    public void unlock() {
        Thread thread = Thread.currentThread();
        System.out.println(thread.getName() + "->unlock");
        atomicReference.compareAndSet(thread, null);
    }
}
/*
程序执行过程：先输出A->lock，然后再输出a，此时A就睡5秒，B进来因为 while (!atomicReference.compareAndSet(null, thread)) 一直在自旋，疯狂输出B->b，过了几秒A醒来，进行A->unlock，B就因为 while (!atomicReference.compareAndSet(null, thread)) 不满足所以跳出了自旋，输出B->lock，接着输出b，然后睡6秒醒来后输出B->unlock，解锁
*/
~~~

### 死锁

产生死锁的四大条件：互斥、占有等待、循环等待、不可抢占（破坏其中一个就不会产生死锁）

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200620154404558.png)

~~~java
package com.czp.lock;

import java.util.concurrent.TimeUnit;

public class KillLock implements Runnable {
    private String stringA;
    private String stringB;

    public KillLock(String stringA, String stringB) {
        this.stringA = stringA;
        this.stringB = stringB;
    }

    @Override
    public void run() {
        synchronized (stringA) {

            System.out.println(Thread.currentThread().getName() + "lock" + stringA + "try to lock stringB");
            try {
                TimeUnit.SECONDS.sleep(2);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            synchronized (stringB) {
                System.out.println(Thread.currentThread().getName() + "lock" + stringB + "try to lock stringA");
            }
        }
    }

    public static void main(String[] args) {
        String a = "a";
        String b = "b";

        new Thread(new KillLock(a, b)).start();
        new Thread(new KillLock(b, a)).start();
    }
}
~~~

如何排查死锁

1. 先使用jps -l定位进程号
   ![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200620154417232.png)
2. 再使用 jstack 查看进程信息找到死锁问题
   <img src="https://gitee.com/xudongyin/img/raw/master/img/image-20200703174159947.png" alt="image-20200703174159947" style="zoom:150%;" />

面试，工作中！ 排查问题：

1、日志 9

2、堆栈 1



