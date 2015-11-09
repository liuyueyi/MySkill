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