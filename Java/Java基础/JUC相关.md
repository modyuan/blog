# JUC相关

`JUC = java.util.concurrent`



## 线程Thread对象

我们可以用两种方式创建线程

- 继承Thread类，重写run方法
- 创建Thread对象，使用`Runnable`接口作为构造函数的参数



我们重点关注第二种，Runnable是一个函数式接口，也就是说可以用一个lambda表达式来代替这个接口传入。



## 可运行Runnable接口

函数式接口，具体来说是一个`void run() `方法



## 可调用Callable接口

类似于Runnable，但是有返回值，具体来说是`V call() throws Exception`方法

可以看出，与Runnable不同，这里允许抛出异常。

## Future接口

表达了一个异步计算的结果，通过这个接口我可以判断计算的执行状态，尝试中止、获取计算结果。

ExecutorService接口执行submit函数时，返回的对象便是一个Future接口。

Future接口可以构建异步应用，但依然有其局限性。它很难直接表述多个Future 结果之间的依赖性。所以这种情况下，CompletableFuture更好用。

## FutureTask类

它实现了Future和Runnable接口

这就使得，可以让Thread的执行结果获取出来。

通过创建FutureTask类，然后在构造函数中传入Callable接口，然后把FutureTask类让Thread去执行。

我们便可以通过FutureTask获取执行结果。

当然，这个的构造函数也接受Runnable，但是必须指定返回值。

获取一个结果时方法, 要么通过轮询isDone，确认完成后，调用get()获取值，要么调用get()设置一个超时时间。但是这个get()方法会阻塞住调用线程。



## CompletableFuture类

CompletableFuture类实现了CompletionStage接口和Future接口。比起FutureTask更加强大。

实现类似js那样异步链式执行，还可以整合两个计算结果。

默认使用ForkJoinPool.commonPool 作为线程池。



## ThreadPoolExecutor

实现了了ExecutorService。可以使用submit提交任务，返回Future接口。(这个方法来自ExecutorService接口)



## ForkJoinPool

主要用来将一个任务拆分成多个“小任务”并行计算，再把多个“小任务”的结果合并成总的计算结果。

ForkJoinPool是ExecutorService的实现类，因此是一种特殊的线程池。

submit方法返回ForkJoinTask对象。（ForkJoinTask实现了Future接口）

## Atomic系列

每个基本类型都有个Atomic，除此之外还有通用的AtomicReference。

可以进行原子的获取、设置、增加等操作，此外还可以使用CompareAndSet，CompareAndExchange等原子指令。



## AtomicStampedReference

带版本的原子变量，可以解决ABA问题：

> 线程1准备用CAS将变量的值由A替换为B，在此之前，线程2将变量的值由A替换为C，又由C替换为A，然后线程1执行CAS时发现变量的值仍然为A，所以CAS成功



## LongAdder

LongAdder中会维护一个或多个变量，这些变量共同组成一个long型的“和”。当多个线程同时更新（特指“add”）值时，为了减少竞争，可能会动态地增加这组变量的数量。“sum”方法（等效于longValue方法）返回这组变量的“和”值。
当我们的场景是为了统计技术，而不是为了更细粒度的同步控制时，并且是在多线程更新的场景时，LongAdder类比AtomicLong更好用。 在小并发的环境下，论更新的效率，两者都差不多。但是高并发的场景下，LongAdder有着明显更高的吞吐量，但是有着更高的空间复杂度。

## LongAccumulator类

LongAdder类是LongAccumulator的一个特例，LongAccumulator提供了比LongAdder更强大的功能，如下构造函数，其中accumulatorFunction是一个双目运算器接口，根据输入的两个参数返回一个计算值，identity则是LongAccumulator累加器的初始值。

LongAccumulator相比LongAdder 可以提供累加器初始非0值，后者只能默认为0，另外前者还可以指定累加规则，比如不是累加而相乘，只需要构造LongAccumulator 时候传入自定义双目运算器即可，后者则内置累加规则。





## 锁和条件变量

- ReentrantLock
- Condition
- ReentrantReadWriteLock





## LinkedBlockingDeque

带阻塞控制的双向队列，比起普通的队列（两组操作API，抛异常和返回值）多了两组API，分别是阻塞和带超时控制的等待。



## CountDownLatch

利用它可以实现类似计数器的功能。

countDown()计数减一。

wait()阻塞当前线程直到计数器为零。

内部使用了AbstractQueuedSynchronizer



## CyclicBarrier

字面意思回环栅栏，通过它可以实现让一组线程等待至某个状态之后再全部同时执行。叫做回环是因为当所有等待线程都被释放以后，CyclicBarrier可以被重用。我们暂且把这个状态就叫做barrier，当调用await()方法之后，线程就处于barrier了。

内部使用了条件变量和可重入锁。

构造函数指定线程数。在线程中执行await()会阻塞当前线程，当阻塞的线程数达到指定数量才会开始全部执行。

## Semaphore

Semaphore翻译成字面意思为 **信号量**，Semaphore可以控同时访问的线程个数，通过 acquire() 获取一个许可，如果没有就等待，而 release() 释放一个许可。和锁有点类似，但是多了数量的控制。

内部使用了AbstractQueuedSynchronizer



## AbstractQueuedSynchronizer 抽象类

它提供了一个基于FIFO队列，可以用于构建锁或者其他相关同步装置的基础框架。