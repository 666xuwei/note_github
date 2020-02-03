# 2. Java 多线程实现

## 1. 继承 Thread 实现多线程

java.lang.Thread 是一个线程操作的核心类。新建一个线程最简单的方法就是直接继承Thread类，而后覆写该类中
的run()方法(就相当于主类中的main方法)



```
public class Demo01 {
    public static void main(String[] args) {
        MyThread thread1 = new MyThread("thread1");
        MyThread thread2 = new MyThread("thread2");
        thread1.start();
        thread2.start();
    }
}

class MyThread extends Thread {
    private String title;

    public MyThread(String title) {
        this.title = title;
    }

    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            System.out.println(this.title + ", i=" + i);
        }
    }
}
```



##### 为什么不是调用 run() 方法？

run() 方法是在本线程里的，只是线程里的一个函数, 而不是多线程的。如果直接调用run(), 其实就相当于是调用了一个普通函数而已，直接调用 run() 方法必须等待 run() 方法执行完毕才能执行下面的代码，所以执行路径还是只有一条，根本就没有线程的特征，所以在多线程执行时要使用start()方法而不是run()方法。



#### run() 方法源码探究

首先看看run方法是如何做的，点击run方法会跳进Thread.java源码中：



```
@Override
public void run() {
    if (target != null) {
        target.run();
    }
}
```



好理解吧？target不为空，就执行run方法，target是啥？



```
/* What will be run. */
private Runnable target;
```



可以看到target就是我们的Runnable 对象本身，所以能理解吧，没毛病哈？



#### start() 方法源码探究



```
public synchronized void start() {
    //这里private volatile int threadStatus = 0;初始化的时候就是0
    //如果这里不为0的话就抛异常

    if (threadStatus != 0)
        throw new IllegalThreadStateException();

    //把当前线程加入到线程组中
    //private ThreadGroup group;就是这么个东西
    group.add(this);

    //初始化标记位未启动
    boolean started = false;
    try {
        start0();
        //标识为启动状态
        started = true;
    } finally {
        try {
            //如果没开启，标识为启动失败
            if (!started) {
                group.threadStartFailed(this);
            }
        } catch (Throwable ignore) {
        }
    }
}
```



注意这里有个start0()方法，源码是



```
private native void start0();
```



这里用到了native修饰符，表示调用本机的操作系统函数，也可能是 JVM 的，这是因为多线程需要我们机器底层的支持，比如说cpu啥的。


注意：**start（）方法来启动线程，真正实现了多线程运行。这时无需等待 run 方法体代码执行完毕，可以直接继续执行下面的代码；通过调用 Thread 类的 start() 方法来启动一个线程， 这时此线程是处于就绪状态， 并没有运行。 然后通过此 Thread 类调用方法run() 来完成其运行操作的， 这里方法 run() 称为线程体，它包含了要执行的这个线程的内容， Run 方法运行结束， 此线程终止。然后CPU再调度其它线程。**

**
**

**
**

**
**

## 2. Runnable 接口实现多线程

Thread 类的核心功能是进行线程的启动。如果一个类为了实现多线程直接去继承 Thread 类就会有但继承局限。在java 中又提供有另外一种实现模式：Runnable 接口。使用 Runnable 接口时无法直接通过 start() 方法开启多线程，但是 Thread 类提供了构造方法：`public Thread(Runnable target)` 可以接收Runnable 接口对象。



```
public class Demo02 {
    public static void main(String[] args) {
        new Thread(new MyThread2(), "子线程").start();
        for (int i = 0; i < 10; i++) {
            System.out.println(Thread.currentThread().getName() + ":" + i);
        }
    }
}

class MyThread2 implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            System.out.println(Thread.currentThread().getName() + ":" + i);
        }
    }
}
```



注意：此时的 Runnable 接口对象可以采用匿名内部类或者 Lambda 表达式来定义。



#### Thread 和 Runnbale 区别

1. **使用 Runnable 实现多线程可以避免单继承的局限性。**
2. **使用****Runnable****实现的多线程的程序类可以更好的描述出程序共享的概念****(****并不是说****Thread****不能****)**

1. 1. **在共享数据时，Thread 需要用静态，Runnable 不需要静态，只要一个****MyRunnable对象就可以。**
    2. **选择锁对象时，比较方便。Runnble 可以直接用 this 对象，继承Thread的方式不能用 this。**

**
**

**
**

**
**

## 3. 使用 Callable 实现多线程

从 JDK1.5 开始追加了新的开发包：java.uti.concurrent。这个开发包主要是进行**高并发编程**使用的，包含很多在高
并发操作中会使用的类。在这个包里定义有一个新的接口 Callable。



```
@FunctionalInterface
public interface Callable<V> {
    /**
    * Computes a result, or throws an exception if unable to do so.
    * *
    @return computed result
    * @throws Exception if unable to compute a result
    */
    V call() throws Exception;
}
```



使用 Callable 接口的实现类可以带有返回值



```
public class Demo03 {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        FuturueTask<String> task = new FutureTask<String>(new MyCallable());
        new Thread(task, "callable接口子线程").start();
        new Thread(task, "callable接口子线程").start();
        System.out.println(task.get());

    }
}

class MyCallable implements Callable<String> {
    private int ticket = 10;

    @Override
    public String call() throws Exception {
        while (this.ticket > 0) {
            System.out.println("剩余票数：" + this.ticket--);
        }
        return "票卖完了";
    }
}
```



注意：FutureTask 类实现了 RunnableFuture 接口，RunnableFuture 接口 实现了 Runnable，Future 接口，然后 Thread 类构造器需要传入一个 Runnable 对象。



##### Callable 接口与 Runnable 接口的区别

1. Callable 接口的实现有返回值，Runnable 接口实现没有。
2. Callable 接口中的 call() 方法抛出了 Exception 异常。
3. 落地方法不一样，一个是 call() 方法，一个是 run() 方法。



##### 可否直接用 Callable 替换 Runnable？怎么用？

不行，因为 thread 类的构造方法中没有 Callable。

我们可以创建一个 FutureTask 对象，将 Callable 接口对象作为其构造器参数。该 FuturueTask 类顶层实现的是 Runnable 接口，运行成功后可以通过 get() 方法获取返回值。



#### FutureTask 类概述

字母意思：未来的任务，用它就干一件事，异步调用。

main方法就像一个冰糖葫芦，一个个方法由main串起来。但解决不了一个问题：正常调用挂起堵塞问题。



举个例子：

1. 老师上着课，口渴了，直接去买水不合适，讲课继续，可以单起个线程找班长帮忙买水，水买回来了放桌上，需要的时候再去 get。
2. 4个同学，A 算 1+20, B 算 21+30,C 算 31*到 40, D 算 41+50，是不是 C 的计算量有点大啊，FutureTask单起个线程给C计算，我先汇总ABD，最后等C计算完了再汇总C，拿到最终结果。



#### FutureTask 原理

在主线程中需要执行比较耗时的操作时，但又不想阻塞主线程时，可以把这些作业交给 Future 对象在后台完成，当主线程将来需要时，就可以通过 Future 对象获得后台作业的计算结果或者执行状态。



一般 FutureTask 多用于耗时的计算，主线程可以在完成自己的任务后，再去获取结果。



仅在计算完成时才能检索结果；如果计算尚未完成，则阻塞 get 方法。一旦计算完成，就不能再重新开始或取消计算。get方法而获取结果只有在计算完成时获取，否则会一直阻塞直到任务转入完成状态，然后会返回结果或者抛出异常。  

 

只计算一次，get 方法放到最后。



```
public class Demo04 {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        FutureTask<Integer> task1 = new FutureTask<>(() -> {
            System.out.println(Thread.currentThread().getName() + "come in callable");
            TimeUnit.SECONDS.sleep(4);
            return 1024;
        });

        FutureTask task2 = new FutureTask(() -> {
            System.out.println(Thread.currentThread().getName()+"  come in callable");
            TimeUnit.SECONDS.sleep(4);
            return 2048;

        });

        new Thread(task1,"zhang3").start();
        new Thread(task2,"li4").start();
        //一般放在程序后面，直接获取结果，而且只计算一次
        //        System.out.println(task1.get());
        //        System.out.println(task2.get());

        while (!task1.isDone()) {
            TimeUnit.SECONDS.sleep(1);
            System.out.println("wait....");
        }
        System.out.println(task1.get());
        System.out.println(Thread.currentThread().getName()+" come over");
    }
}

class MyThread3 implements Runnable {
    @Override
    public void run() {

    }
}

class MyThread4 implements Callable<Integer> {
    @Override
    public Integer call() throws Exception {
        System.out.println(Thread.currentThread().getName() + "come in callable");
        return 200;
    }
}
```