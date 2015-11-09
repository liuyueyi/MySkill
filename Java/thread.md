# Thread & Process
> Java并发编程

## 1. 线程
> 通过继承Thread, 实现Callable, Runnable来实现多线程任务

### Thread

Thread是一个抽象类，首先需要定义一个类来继承Thread,并重写run方法，其中run方法为线程执行的具体任务，最后通过调用`thread.start()` 来启动

```java
public MyThread extends Thread {
    @Override
    public void run() {
        for(int i = 0; i < 100; i++){
            System.out.println(getCurrentThread
        }
    }
}
```