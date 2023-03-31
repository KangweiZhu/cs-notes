### Java多线程



#### 1. 什么是线程

线程是操作系统能够进行运算调度的最小单位。它被包含在**进程**之中，是进程中的实际运作单位。

----

360 本身是进程。

其功能就是多个线程。

简单理解就是：应用软件中互相独立，可以同时运行的功能。



提高工作效率。

例子：一个人操作多个流水线。充分利用空闲时间。

```
int a = 10;		
int b = 20;		 
int c = a + b;
System.out.println(c);
```

cpu读每行代码代码。在内存中创建变量。在创建过程中是无法干别的事情的，所以只能等着。 -> 单线程的效率比较低。

多线程特点： 提高程序效率

软件中的耗时操作：

	1. 拷贝、迁移大文件
	1. 加载大量的资源文件。

**用到多线程的地方：聊天软件、后台服务器**

应用场景：**只要想让多个事情同时运行，就需要用到多线程。**

**多线程的作用：充分利用程序当中的等待时间，让cpu在多个程序中进行切换，从而提高了程序的运行效率。**



#### 2. 什么是进程

**一个软件运行之后，他就是一个进程。**



#### 3. 并发

并发： 在同一时刻，有多个指令在单个CPU上**交替**运行。

重点是交替：

手在多个物品间来回交替。手是cpu，物品是线程。 



#### 4. 并行

**并行：在同一个时刻，有多个指令在多个CPU上同时执行。**

重点：两条线程，两个cpu同时进行。



#### 5. 多线程的实现方式

##### 1. 继承Thread类的方式进行实现

Thread就是java中的一个线程。如果你想用到这个线程的话，直接创建一个他的对象,并开启它就可以了。

注意开始是使用start(), 而不是run(). 



##### 2. 实现Runnable接口的方式进行实现



##### 3. 利用Callable接口和Future接口方式实现 

是对前面两种方法的补充。

前面两个方式都没有返回值。

第三种方式，可以获取到多线程运行的结果。



##### 4.  Thread类中常用的成员方法

###### 1.  setName(String name)



###### 2. getName()

就算没有setName过，依旧是有返回内容的。返回的格式是Thread + - + 线程编号。编号从0开始。哪个的对象被先创建，其编号就越前。

```java
public class setThreadDemo extends Thread{
    @Override
    public void run(){
        for (int i = 0; i < 100; i++) {
            System.out.println(this.getName() + "@" + i);
        }
    }

    public static void main(String[] args) {
        setThreadDemo s1 = new setThreadDemo();
        setThreadDemo s2 = new setThreadDemo();
        setThreadDemo s3 = new setThreadDemo();
        System.out.println("thread s1 name is: " + s1.getName());
        System.out.println("thread s2 name is: " + s2.getName());
        System.out.println("thread s3 name is: " + s3.getName());
    }
```

这是因为在thread的构造函数中，调用了nextThreadNum()方法。方法内实现了static变量的自增。



###### 3. static Thread currentThread()

```java
 Thread curThread = Thread.currentThread();
 System.out.println(curThread + " " + curThread.getName());
```

获取执行该方法的线程。

在main方法中， 调用此方法，thread的名字就是main。

```
output: Thread[#1,main,5,main] main
```

原因是：当JVM虚拟机启动之后，会自动的启动多条线程。其中有一条叫做main的线程，作用就是调用main方法，并执行里面的代码。



###### 4. static void sleep(long time)

哪条方法执行到该线程，哪条方法就会在这里停留参数中的时间。



###### 5. setPriority(int newPriority)

java中使用了抢占式调度。抢占式调度的特性就是随机性。

非抢占式调度就是依次执行所有线程.

参数分为10档。从1到10. 参数越高，抢占cpu的概率就越高。

默认优先级是5.



###### 6. void setDaemon(boolean on)

守护线程。将一个线程设为守护线程。当其他的非守护线程执行完毕之后，无论守护进程是否执行完，它都会结束。

应用场景：聊天和文件传输。 当聊天软件被关闭的时候，文件传输也没有必要了。因此就可以把文件传输设置为守护线程。



###### 7. static void yeild()

礼让/出让 线程

让程序运行尽可能均匀。交织运行



###### 8. final void join()

让一个线程插队。



##### 5.线程生命周期

###### 1.同步代码块

```java
synchronized (锁) {
	//操作共享数据的代码
}
```

特点1： 锁默认打开，有一个线程进去了，锁自动关闭

特点2： 里面的代码全部执行完毕，线程出来，锁自动打开。

锁对象，随便是什么对象，但是必须是唯一的，也就是要用static修饰。



###### 2.同步方法

把同步代码块中的内容写到方法里就可以。例如

```java
public synchronized Boolean add(Ppl pp){
	//business logic
}
```



###### 3. lock() 和 unlock()

Lock() 是接口，不能直接创建对象。可以创建它的实现类ReEntrantLock的对象。



##### 6.消费者和生产者 

###### 1.等待唤醒机制

生产者： 生产数据

消费者：消费数据 

交替执行 

###### 1. wait()

###### 2. notify()

###### 3. notifyAll()



###### 2. 阻塞队列
