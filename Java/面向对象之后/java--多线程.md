# 多线程：

#### 一、多线程的创建

##### 方式一：

1、创建一个继承于**Thread**类的子类

2、重写Thread类的run() --> 将此线程执行的操作声明在run()中

3、创建Thread类的子类的对象 --> a.启动当前线程 b.调用当前线程的run()

4、通过此对象调用stat()

<!--注意：-->

1、不能通过直接调用run() 的方式启动线程

2、不可以还让已经star()的线程去执行，会报 IllegalThreadStateException异常

```java
//1.创建一个继承于Thread类的子类
class MyThread extends Thread{
    //2.重写Thread类的run()
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            if(i % 2 == 0){
                System.out.println(i);
            }
        }
    }
}

public class ThreadTest {
    public static void main(String[] args) {
        //3.创建Thread类的子类的对象
        MyThread myThread = new MyThread();
        //4.通过此对象调用stat()
        myThread.start();
        //myThread.start();不可以还让已经star()的线程去执行，会报IllegalThreadStateException异常
        
        //仍然在主线程main中执行
        for (int i = 0; i < 100; i++) {
            if(i % 2 == 0){
                System.out.println("main中i = " + i);
            }
        }
    }
}
```



**Thread 类中的常用方法：**

1、star() ：启动当前线程 ，调用当前线程的run()

2、run() ：同化成那个需要重写Thread类中的此方法，将创建的线程要执行的操作声明在此线程中

3、currentThread() : 静态方法，返回执行当前代码的线程

4、getName()：获取线程名称

5、setName()：设置线程名称

6、yield()：释放当前CPU的执行权

7、join()：在线程a中调用线程b的join()方法，此时线程a进入阻塞状态，直到线程b执行完以后，结束阻塞状态

8、stop() ：强制结束线程生命（不推荐使用）

9、sleep()：让当前线程 “睡眠“ 指定时长，在指定的时长内，当前线程是阻塞状态

10、isAlive() ：判断当前线程是否存活



**线程的调度：**

1、线程优先级：

**MIN_PRIORITY = 1**（最低）

**NORM_PRIORITY = 5**（正常）

**MAX_PRIORITY = 10**（最高）

2、获取和设置线程优先级：

getPriority()：获取

setPriority()：设置



##### 方式二：

1、创建一个实现了Runnable接口的类

2、实现类去实现Runable 中的抽象方法：run()

3、创建实现类的对象

4、将此类对象作为参数传递到Thread 类的构造器中，创建Thread类的对象

5、通过Thread类的对象调用star() :a.启动线程 b.调用当前类的run() -->调用了Runnable类型的target

```java
public class ThreadTest {
    public static void main(String[] args) {
        //3、创建实现类的对象
        MyThread myThread = new MyThread();
		//4、将此类对象作为参数传递到Thread 类的构造器中，创建Thread类的对象
        Thread t1 = new Thread(myThread);
        //5、通过Thread类的对象调用star()
        t1.start();

        //在启动一个线程，遍历100以内偶数
        Thread t2 = new Thread(myThread);
        t2.setName("线程二");
        t2.start();

    }
}

//1、创建一个实现了Runnable接口的类
class MyThread implements Runnable{

    //2、实现类去实现Runable 中的抽象方法：run()
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            if(i % 2 == 0){
                System.out.println(i);
            }
        }
    }
}
```



**开发中，优先选择实现Runnable接口的方式：**

​	1、实现的方式没有类的**单继承性的局限性**

​	2、实现的方式更适合处理多个共享数据的情况

联系：Thread类也实现了Runnable接口，两种方式都需要重写run()

<!--目前两种方式要想启动线程都要用star()方法-->



##### 方式三：

**实现Callable接口，jdk5.0新增**

```java
//1.创建一个Callable的实现类
class N implements Callable {
    //2.实现call方法，将此线程需要执行的操作声明在call中
    @Override
    public Object call() throws Exception{
        int sum = 0;
        for (int i = 0; i <= 100; i++) {
            if(i % 2 == 0){
                System.out.println(i);
                sum += i;
            }
        }
        return sum;
    }
}

public class NumThread {
    public static void main(String[] args) {
        //3.创建Callable接口实现类的对象
        N num = new N();
        //4.将此Callable接口实现类的对象作为参数传递到FutureTask构造器中，创建FutureTask的对象
        FutureTask futureTask = new FutureTask(num);
        //5.将FutureTask的对象作为参数传递到Thread类的构造器中，创建Thread对象
        new Thread(futureTask).start();
        try{
            //get()返回值即为futureTask构造器参数Callable实现类重写的call方法的返回值
            Object sum = futureTask.get();
            System.out.println("总和为：" + sum);
        }catch (InterruptedException e){
            e.printStackTrace();
        }catch (ExecutionException e){
            e.printStackTrace();
        }
    }
}
```

**实现Callable接口的方式创建多线程比实现Runnable接口创建多线程方式强大：**

1、call()方法可以有返回值

2、call()方法可以抛出异常，被外面的操作捕获，获取异常信息

3、call()方法是支持泛型的



##### 方式四：

**使用线程池**：

好处：

1、提高响应速度（减少了创建新线程的世界）

2、降低资源消耗（重复利用线程池中线程，不需要每次都创建）

3、便于管理：

​		corePoolSize：核心池的大小

​		maximumPoolSize：最大线程数

​		keepAliveTime：线程没有任务时最多保持多长时间后会终止

```java
//线程1
class NumberThread implements Runnable{
    @Override
    public void run() {
        for (int i = 0; i <= 100; i++) {
            if(i % 2 == 0){
                System.out.println(Thread.currentThread().getName() + ":" + i);
            }
        }
    }
}
//线程2
class NumberThread1 implements Runnable{
    @Override
    public void run() {
        for (int i = 0; i <= 100; i++) {
            if(i % 2 != 0){
                System.out.println(Thread.currentThread().getName() + ":" + i);
            }
        }
    }
}

public class ThreadPool {
    public static void main(String[] args) {
        //1.提供指定数量的线程池
        ExecutorService service = Executors.newFixedThreadPool(10);
        //2.执行指定的线程的操作。需要提供实现Runnable接口或者Callable接口的实现类
        service.execute(new NumberThread());//适合使用于Runnable
        service.execute(new NumberThread1());//适合使用于Runnable
        //service.submit(Callable callable);//适合使用于Callable
        //关闭线程池
        service.shutdown();
    }
}
```



#### 二、线程的生命周期



![image-20220323214532070](C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20220323214532070.png)



#### 三、线程安全问题

**例子：卖票**

1、问题：卖票过程中，出现了重票、错票

2、原因：当某个线程操作车票的过程中，尚未操作完成时，其他线程参与进来，也操作车票

3、解决：当一个线程在操作票的时候，其他线程不能参与进来。直到线程a操作结束的时候，其他线程才可以参与进来操作。这种情况即使线程a出现了阻塞也不能被改变

**4、在java中，我们通过同步机制来解决线程的安全问题。**

​		**方式一：同步代码块**

```java
synchronized(同步监视器，也就是锁){
	//需要被同步的代码
}
```

说明：

（1）操作共享数据的代码即为需要被同步的代码   -->不能包含代码多了，也不能包少了。

（2）共享数据：多个线程共同操作的变量

（3）同步监视器，俗称：**锁**。任何一个类的对象都可以充当锁

​	**<!--要求：多个线程必须要共用同一把锁-->**

补充：在使用实现Runnable接口创建多线程，可以考虑this充当同步监视器

​		**方式二：同步方法**

操作共享数据的代码完整的声明在一个方法中。

1、同步方法仍然设计到同步监视器，只是不需要我们显示的声明

2、非静态的同步方法。同步监视器是：this

​	   静态的同步方法，同步监视器是：当前类本身

```java
public synchronized void show() {
	//............
}
```



```java
//例子：卖票
public static void main(String[] args) {
        Widown widown = new Widown();
        Thread widown1 = new Thread(widown);
        Thread widown2 = new Thread(widown);
        Thread widown3 = new Thread(widown);

        widown1.setName("窗口1");
        widown2.setName("窗口2");
        widown3.setName("窗口3");

        widown1.start();
        widown2.start();
        widown3.start();
    }
}

class Widown implements Runnable{

    private int ticket = 100;
    Object obj = new Object();//锁
/***********************************************/
    @Override
    public void run() {
        while(true){
            synchronized(obj) {//同步代码块
               //..................
            }

        }
    }
/***********************************************/
    public synchronized void show() {//同步方法
       //................
    }

    @Override
    public void run(){
        while(true){
            show();
        }
    }
}
```



**使用同步机制将单例模式中的懒汉式改写为线程安全的：**

```java
class Bank{
    private Bank(){}
    private static Bank instance = null;

    public static Bank getInstance(){
        if(instance == null){
             //当方法被调用进入instance的赋值时线程a有可能阻塞，然后此时线程b也进来，就可能导致instance被两次赋值
            instance = new Bank();
        }
        return instance;
    }
}
```

更改：

```java

class Bank{
    private Bank(){}
    private static Bank instance = null;
	/****************同步方法*******************/
    public static synchronized Bank getInstance(){
        if(instance == null){
            instance = new Bank();
        }
        return instance;
    }
    /****************同步代码块*****************/
    //方式一：效率偏低
    public Bank getInstance(){
        synchronized (Bank.class) {
            //每个线程都会来到这里，但是先来的线程使得instance被new过了，所以后面的线程进来也什么都没有做，就直接拿着new好的instance出去
            if(instance == null){
                instance = new Bank();
            }
            return instance;
        }
    }
    
     //方式二：效率稍高
    public Bank getInstance(){//同步代码块
        //线程来到这里，如果instance由于先进来的线程被new过了，那后面进来的线程就不再进入，直接拿着new好的instance就出去了
        if(instance == null){
            synchronized (Bank.class) {
            if(instance == null){
                instance = new Bank();
            }
        }
        return instance;
    }
}
```



#### 四、死锁

不同的线程分别占用对方需要的同步资源不放弃，都在等待对方放弃自己所需要的同步资源，就形成了线程的死锁。

说明：

1、出现死锁后，不会出现异常，不会出现提示，只是所有的线程都处于阻塞状态，无法继续执行

2、使用同步时，要避免出现死锁

```java
public static void main(String[] args) {
    
        StringBuffer s1 = new StringBuffer();
        StringBuffer s2 = new StringBuffer();
		
    	//线程1
        new Thread(){
            @Override
            public void run(){
                synchronized(s1){

                    s1.append("a");
                    s2.append("1");

                    try {
                        //线程1拿着s1睡眠（阻塞），等着拿s2，此时极有可能执行线程2
                        Thread.sleep(1000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }

                    synchronized (s2){
                        s1.append("b");
                        s2.append("2");
                        System.out.println(s1);
                        System.out.println(s2);
                    }
                }
            }
        }.start();
		
    	//线程2
        new Thread(new Runnable() {
            @Override
            public void run() {

                synchronized(s2){

                    s1.append("c");
                    s2.append("3");

                    try {
                        //线程1睡眠，执行线程2，线程2拿着s2也睡眠，等着拿s1
                        Thread.sleep(1000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }

                    synchronized (s1){
                        s1.append("d");
                        s2.append("4");
                        System.out.println(s1);
                        System.out.println(s2);
                    }
                }
            }
        }).start();
```



##### 解决线程问题方式三：Lock（锁）

与synchronized比较：

1、都可以解决线程安全问题

2、**synchronized机制**在执行完相应的同步代码以后，自动的释放同步监视器。而**Lock**需要手动的启动同步（**lock()**），同时结束同步也需要手动的实现（**unlock()**）

```java
//例子：卖票
public class SynchronizedTest {
    public static void main(String[] args) {
        Window w = new Window();

        Thread t1 = new Thread(w);
        Thread t2 = new Thread(w);
        Thread t3 = new Thread(w);

        t1.setName("窗口1");
        t2.setName("窗口2");
        t3.setName("窗口3");

        t1.start();
        t2.start();
        t3.start();

    }
}

class Window implements Runnable{

    private int ticket = 100;
    //  1.实例化ReentrantLock
    private ReentrantLock lock = new ReentrantLock();

    @Override
    public void run() {
        while(true){
            try{
                //2.调用锁定方法lock
                lock.lock();
            if(ticket > 0){

                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread().getName() + "：售票，票号为：" + ticket);
                ticket--;
            }else{
                break;
            }
            }finally {
                //3.调用解锁方法
                lock.unlock();
            }
        }
    }
}
```



#### 五、线程通信

**wait()方法**：一旦执行此方法，当前线程进入阻塞状态，并释放同步监视器

**notify()方法**：一旦执行此方法，就会唤醒被wait() 的一个线程，如果有多个线程，就唤醒优先级高的

**notifyAll()方法：**一旦执行此方法，所有被wait() 的线程都被唤醒

<!--三个方法必须使用在同步代码块或者同步方法中，而且三个方法的调用者必须是同步代码块或者同步方法中的同步监视器，否则会出现异常-->

```java
public void run() {
        while(true){

            synchronized (this) {
                notify();//唤醒被wait的线程
                if(number <= 100){
                    System.out.println(Thread.currentThread().getName() + "：" + number);
                    number++;

                    try {
                        //使得如下调用wait方法的线程进入阻塞,并且会释放锁
                        wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }

                }else{
                    break;
                }
            }

        }
    }
```






















