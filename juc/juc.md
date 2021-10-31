线程状态：

```java
public enum State {

    NEW,

    RUNNABLE,

    BLOCKED,

    WAITING,

    TIMED_WAITING,

    TERMINATED;
}
```

wait/sleep的区别

1.wait所有类都有，sleep只来自Thread

2.wait会释放锁，sleep不会释放锁

3.使用范围不同，wait必须在同步代码块中使用，sleep可以随意

4.wait不需要捕获异常，sleep需要捕获异常，因为sleep可能发生超时等待

**lock锁**

ReetrantLock lock=new ReetrantLock ();

lock.lock();

lock.unlock();

> synchronized 和lock的区别

1.synchronized是关键字lock是关键字

2.synchronized无法判断获取锁的状态，lock可以判断是否获取到锁

3.synchronized会自动释放锁，lock必须手动释放

4.synchronized死锁后会一直等待，lock不会一直等待

5.synchronized是可重入锁，不可中断，非公平；lock可重入锁，可判断锁，可自己设置公平锁和非公平锁

6.synchronized适合锁少量的代码同步问题，Lock适合锁大量的同步代码

为了防止虚假唤醒，所以判断的时候需要用while来判断，不能用if进行判断

**condition的优势：**他可以指定condition进行唤醒

```java

public class TheadTest {
    public static void main(String[] args) {
        Customer customer=new Customer();
        new Thread(()->{
            for(int i=0;i<10;i++){
                try {
                    customer.custom();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }).start();
        new Thread(()->{
            for(int i=0;i<10;i++){
                try {
                    customer.produce();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }).start();
    }
}
//传统多线程
class Customer{
    public static int num=0;

    public Customer(){

    }
    public synchronized void custom() throws InterruptedException {
        if(num==0){	//这里使用while可以避免虚假唤醒
            this.wait();
        }
        num--;
        System.out.println(Thread.currentThread().getName()+":"+num);
        this.notifyAll();
    }
    public synchronized void produce() throws InterruptedException {
        if(num==1){
            this.wait();
        }
        num++;
        System.out.println(Thread.currentThread().getName()+":"+num);
        this.notifyAll();
    }
}
//Lock
class NewCustomer{
    public int num=0;
    //默认为公平锁，加了true则为非公平锁
    public Lock lock=new ReentrantLock();
    public Condition condition=lock.newCondition();

    public void custom() throws InterruptedException {
        try{
            lock.lock();
            while(num!=0){
                condition.await();
            }
            num--;
            System.out.println(Thread.currentThread().getName()+":"+num);
            condition.signalAll();
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            lock.unlock();
        }

    }
    public void produce() throws InterruptedException {
        try{
            lock.lock();
            while(num==0){
                condition.await();
            }
            num++;
            System.out.println(Thread.currentThread().getName()+":"+num);
            condition.signalAll();
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            lock.unlock();
        }
    }
}

//Lock 指定唤醒
class NewCustomer{
    public int num=0;
    //默认为公平锁，加了true则为非公平锁
    public Lock lock=new ReentrantLock();
    public Condition condition1=lock.newCondition();
    public Condition condition2=lock.newCondition();
    public Condition condition2=lock.newCondition();
	
    public void printA(){
        print
    }
    public void custom() throws InterruptedException {
        try{
            lock.lock();
            while(num!=0){
                condition.await();
            }
            num--;
            System.out.println(Thread.currentThread().getName()+":"+num);
            condition.signalAll();
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            lock.unlock();
        }

    }
    public void produce() throws InterruptedException {
        try{
            lock.lock();
            while(num==0){
                condition.await();
            }
            num++;
            System.out.println(Thread.currentThread().getName()+":"+num);
            condition.signalAll();
        }catch (Exception e){
            e.printStakTrace();
        }finally {
            lock.unlock();
        }
    }
}

class NewLockPoint{
    public int num=1;
    public Lock lock=new ReentrantLock();
    public Condition condition1=lock.newCondition();
    public Condition condition2=lock.newCondition();
    public Condition condition3=lock.newCondition();

    public void printA(){
        lock.lock();
        try {
            while(num!=1){
                condition1.await();
            }
            System.out.println(Thread.currentThread().getName()+"AAAAAA");
            num=2;
            condition2.signal();
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }

    public void printB(){
        lock.lock();
        try {
            while(num!=2){
                condition2.await();
            }
            System.out.println(Thread.currentThread().getName()+"BBBBBB");
            condition3.signal();
            num=3;
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }

    public void printC(){
        lock.lock();
        try {
            while(num!=3){
                condition3.await();
            }
            System.out.println(Thread.currentThread().getName()+"CCCCCC");
            num=1;
            condition1.signal();

        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
}
```

**8锁现象：**synchronized锁的是调用者，如果是静态方法则锁的是对象的class，每个类只有一个class

线程休眠：TimeUnit.SECONDS.sleep(100)、TimeUnit.DAYS.sleep(100)、TimeUnit.HOURS.sleep(100)

集合多线程不安全问题：

```java
//原生ArrayList多线程不安全
List<String> list=new ArrayList<>();
//解决方案
List<String> list=new Vector<>();	//底层用的是synchronized
//以下两种方法，各种集合在工具类中都提供了
List<String> list=new Collections.synchronizedList(new ArrayList<>());
List<String> list=new CopyOnWriteArrayList<>(); // 底层用的是Lock

```

辅助类：Callable、CountDownLatch 、CyclicBarrier、Semaphore

**Callable**

```java
public class TheadTest {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        MyCallable myCallable=new MyCallable();
        FutureTask futureTask=new FutureTask(myCallable);
        //定义了两个线程，但是第一个线程的结果会被缓存，效率高
        new Thread(futureTask,"A").start();
        new Thread(futureTask,"B").start();
        //get方法可能会发生阻塞
        Integer out=(Integer) futureTask.get();
        System.out.println(out);
        //程序执行结果为
        //call()
        //1024
    }
}
class MyCallable implements Callable<Integer> {

    @Override
    public Integer call() throws Exception {
        System.out.println("call()");
        return 1024;
    }
}
```

**CountDownLatch 等待所有线程结束，作为一个计数作用**

```java
CountDownLatch countDownLatch=new CountDownLatch(6);
for(int i=0;i<6;i++){
    new Thread(()->{
        System.out.println(Thread.currentThread().getName()+":go out");
        countDownLatch.countDown();
    }).start();
}
//等待所有线程结束后，唤醒主线程
countDownLatch.await();
System.out.println("Close down");
```

**CyclicBarrier：**

```java
CyclicBarrier cyclicBarrier=new CyclicBarrier(7,()->{
    System.out.println("条件满足");
});
for(int i=0;i<7;i++){
    int temp=i;
    new Thread(()->{
        System.out.println(Thread.currentThread().getName()+":"+temp);
        try {
            cyclicBarrier.await();
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (BrokenBarrierException e) {
            e.printStackTrace();
        }
    }).start();
}
```

**Semaphore信号量**

```java
Semaphore semaphore=new Semaphore(2);
for(int i=0;i<10;i++){
    new Thread(()->{
        try {
            //如果满了，则会等待其他线程进行release释放
            semaphore.acquire();
            System.out.println(Thread.currentThread().getName()+":进入车位");
            TimeUnit.SECONDS.sleep(2);
            System.out.println(Thread.currentThread().getName()+":离开车位");
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            semaphore.release();
        }
    }).start();
}
```

**读写锁ReentrantReadWriteLock**：读写锁，读只能一个，写可以多个同时

```java

MyReadWriteLock myReadWriteLock=new MyReadWriteLock();
for(int i=0;i<8;i++){
    int finalI = i;
    new Thread(()->{
        myReadWriteLock.write(String.valueOf(finalI),String.valueOf(finalI));
    }).start();
}

for(int i=0;i<8;i++){
    int finalI = i;
    new Thread(()->{
        myReadWriteLock.read(String.valueOf(finalI));
    }).start();
}

class MyReadWriteLock{
    private volatile HashMap<String,String> map=new HashMap<>();
    private ReadWriteLock lock=new ReentrantReadWriteLock();
    public void read(String key){
        lock.readLock().lock();
        System.out.println(Thread.currentThread().getName()+"读取");
        map.get(key);
        System.out.println(Thread.currentThread().getName()+"结束");
        lock.readLock().unlock();
    }
    public void write(String key,String value){
        lock.writeLock().lock();
        System.out.println(Thread.currentThread().getName()+"写入");
        map.put(key,value);
        System.out.println(Thread.currentThread().getName()+"写入结束");
        lock.writeLock().unlock();
    }
}
```























































