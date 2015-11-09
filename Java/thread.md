# Thread & Process
> Java并发编程

## 1. 线程
> 通过继承Thread, 实现Callable, Runnable来实现多线程任务

### Thread

Thread是一个抽象类，首先需要定义一个类来继承Thread,并重写run方法，其中run方法为线程执行的具体任务，最后通过调用`thread.start()` 来启动

```java
class MyThread extends Thread {
    @Override
    public void run() {
        for(int i = 0; i < 100; i++) {
            System.out.println(currentThread().getId() + " i ");
        }
    }
    
    public static void main(String[] args) {
        MyThread mth = new MyThread();
        // 启动子线程
        mth.start();
        // 下面是获取主线程的名
        System.out.println(currentThread().getName());
    }
}
```

- 从上面的代码也可以看出，通过继承Thread方法，简单粗暴，问题就是Java只能单继承，这种方式就不能再继承其他的类了

### Runnable
是一个接口，其实和上面没什么区别，只是调用方式有点不一样

```java
class MyThread implements Runnable {
    @Override
    public void run() {
        for(int i = 0; i < 100; i++) {
            System.out.println(Thread.currentThread().getName() + " i ");
        }
    }

    public static void main(String[] args) {
        MyThread thread = new MyThread();
        new Thread(thread, "线程1").start();
        new Thread(thread, "线程2").start();
    }
}
```
上面的代码，也很有意思了，调用方式会稍稍麻烦一点，需要new一个Thread对象，将你的Runnable的实现类作为构造参数传入，然后调用Thread对象的start方法来启动线程


### Callable
这个和Runnable其实就很相似了，唯一的区别是它有返回值，即call方法可以返回东西

```java
class MyCallable implements Callable<Integer> {
    @Override
    public Integer call() {
        Integer sum = 0;
        for(int i = 0; i < 100; i++) {
            System.out.println(Thread.currentThread().getName() + " i ");
            sum += i;
        }
        return sum;
    }

    public static void main(String[] args) {
        FutureTask<Integer> task = new FutureTask<Integer>(new MyCallable());
        new Thread(task, "有返回值").start();

        try{
            System.out.println("子线程的返回值为: " + task.get());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

这里是利用Future来获取返回值

## 2. 线程安全
> 当多个线程同时访问公共资源时，可能出现问题

### 场景：

如：ATM取钱的逻辑，刨掉前面的身份验证，流程为: 
- 输入取款金额
- 验证取款金额，是否小于剩余钱数
- 验证通过后，取钱，剩余金额 -= 取款金额

上面的逻辑很清晰，一般的流程也就是这么做的，问题会出现再哪儿呢，出现再判断完能否取钱之后，到取钱之间

假设总钱为1000， A已经到了取钱的阶段，准备get800， 还没有取，即余额没有变； 这时B又来了，他的网速快，也开始取钱800，这是余额还是1000，因此判断可以取，进行到第三部，则会导致AB都可以取钱成功，则余额变成 -600

### 解决？

- 利用同步代码块，or 同步类 `Synchronized`

  将需要同步的代码块，包裹在 Synchronized(对象)之间，再线程访问该区域代码时，其他线程会等待

  ```
Synchronized(Account) {
    // 判断是否可以取钱
    // 取钱
    // 减少余额
}
```

- Lock 锁

  Lock对象，再需要加锁的代码块前调用 lock.lock(); 执行完毕后，调用 lock.unlock() 解锁
    
  `Lock lock = new ReentrantLock();`

  **再加锁、解锁的过程中，需要格外注意，死锁的情况**


## 3. 线程通信
> 这个主要是利用Object对象的 `wait(); notify(); notifyAll()` 三个函数来实现

- 对于同步代码快里面，线程通信的调用方为Sychronized括号中的对象； 而同步方法则是该类

- 对应Lock的地方呢？这就需要Condition来实现，首先是利用 `lock.newCondition();`获取Condition对象，然后调用 `condition.wait(); condition.signal(); condition.signalAll()` 来实现


## 4.线程池
> Executor的类方法来创建线程池，ExecutorServie,然后将线程提交的线程池中，就由线程池来管理线程的执行

```java
class MyCallable implements Callable<Integer> {
    @Override
    public Integer call() {
        Integer sum = 0;
        for(int i = 0; i < 100; i++) {
            System.out.println(Thread.currentThread().getName() + " i ");
            sum += i;
        }
        return sum;
    }

    public static void main(String[] args) {
        FutureTask<Integer> task = new FutureTask<Integer>(new MyCallable());
        new Thread(task, "有返回值").start();

        try{
            System.out.println("子线程的返回值为: " + task.get());
        } catch (Exception e) {
            e.printStackTrace();
        }

        ExecutorService pool = Executors.newFixedThreadPool(8);
        pool.submit(task); // 向线程池中提交线程
        pool.shutdown(); // 关闭线程池
    }
}
```