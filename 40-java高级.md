##线程(硅谷)
    **概念：
        程序，进程，线程的基本概念
            1：程序：是为完成特定任务，用某种语言编写的一组指令集合。即指一段静态代码，静态对象
            2：进程：是程序的一次执行过程，或是正在运行的一个程序。是一个动态的过程：有它自身的产生，存在和消亡的过程，也叫生命周期
                    如：运行中的qq，运行中的mp3播放器
                    程序时静态的，进行是动态的
                    进程作为资源分配的单位，系统在运行时会为每个进程分配不同的内存区域
            3：线程：进程可进一步细化为线程，是一个程序内部的一条执行路径。
                    若一个进程同一时间并行执行多个线程。就是支持多线程的
                    线程作为调度和执行的单位，每个线程拥有独立的运行栈和程序计数器，线程切换的开销小
                    一个进程中的多个线程共享相同的内存单元/内存地址空间->它们从同一堆中分配对象，可以访问相同的变量和对象。这就使得线程间通信更简便，搞笑。但多个线程操作共享的系统资源可能就会带来安全隐患

        单核CPU和多核CPU的理解
            1：单核CPU，其实是一种假的多线程，因为在一个时间单元内，也只能执行一个线程的任务。平时看到的单核CPU也能有多个程序在运行，是因为CPU的主频比较高，处理程序的比较快，多个应用在短时间内切换着被执行，但是同一个时间段只执行一个程序。外部看着就像执行多程序一样。
            2：如果是多核的话，才能更好的发挥多线程的效率。(现在的服务器都是多核的)
            3：一个java应用程序java.exe，其实至少有三个线程：main()主线程，gc()垃圾回收线程，异常处理线程。当然如果发生异常，会影响主线程
        
        并行和并发概念
            并行：多个CPU同时执行多个任务，比如，多个人同时做不同的事儿。
            并发：一个CPU(采用时间片)通知执行多个任务。比如：秒杀，多个人同时做同一件事儿

        多线程的优点
            提高应用程序的响应。对图形化界面更有意义，可增强用户体验
            提高计算机系统CPU的利用率
            改善程序结构。将即长又复杂的进程分为多个线程，独立运行，利于理解和修改

        何时需要多线程
            程序需要同时执行两个或多个任务
            程序需要实现一些需要等待的任务时，如用户输入，文件读写操作，网络操作，搜索，图片加载等
            需要后台运行的程序时；如垃圾回收机制

        

    **创建线程
        创建线程的方法：
            1：继承Thread类
                Thread中的常用方法：
                    1：start():启动当前线程;调用当前线程的run()
                    2：run()：通常需要重写Thread类中的此方法，将创建的线程要执行的操作声明在此方法中
                    3：currentThread()：静态方法，返回执行当前代码的线程
                    4：getName()：获取当前线程的名字
                    5：setName()：设置当前线程的名字
                    6：yield()：释放当前CPU的执行权；可能下一秒又分到了CPU的执行权
                    7：join():在线程a中调用线程b的join()，此时线程a就进入阻塞状态，直到线程b完全执行完以后，线程a才结束阻塞状态，然后等待CPU给a线程分配资源
                    8：stop：已过时。当执行此方法时，强制结束当前线程
                    9：sleep(Long millitime)：让当前线程"睡眠"指定的millitime毫秒。在指定的millitime毫秒时间内，当前线程是阻塞状态，阻塞完毕之后，等待CPU分配资源
                    10：isAlive()：判断当前线程是否存活

                步骤：
                    1：创建一个集成于Thread类的子类
                    2：重写Thread类的run()方法(run方法中是要执行的操作)
                    3：创建Thread类的子类的对象
                    4：通过此对象调用start()方法

                事例：

                    ```java
                        public class TestTrhead {
                            public static void main(String[] args){
                                //创建Thread类的子类的对象
                                MyThread t1 = new MyThread();
                                t1.start();//通过此对象调用start()：1：启动当前线程，2：调用当前线程的run()
                                            //不可以让已经start()的线程去执行。会报Illegal ThreadStateException

                                // 再启动一个线程
                                MyThread t2 = new MyThread()
                                    t2.setName("线程一") //给线程t2命名
                                t2.start();

                                    // 给主线程命名
                                    Thread.currentThread().setName("主线程")
                                
                                //下面的操作任然是在main线程中执行的
                                System.out.println("hello");
                                for(int i=0;i<100;i++){
                                    if(i%2==0){
                                        System.out.println(i+"*****");
                                    }
                                }
                            }
                        }
                        //常见一个继承与Thread类的子类
                        class MyThread extends Thread {
                            //重写Thread类的run()
                            public void run(){
                                for(int i=0;i<100;i++){
                                    if(i%2==0){
                                        System.out.println(i);
                                    }
                                }
                            }
                        }
                    ```
                事例：创建Thread的匿名子类

                    ```java
                        public static void main(String[] args){
                                //创建Thread类的匿名子类的方式
                                //理解匿名概念：因为这里子类没有名，所以拿Thread这个父类来充当的，后面的大括号里写的是匿名子类的方法体
                                new Thread(){
                                    public void run(){
                                        for(int i=0;i<100;i++){
                                            if(i%2==0){
                                                System.out.println(Thread.currentThread().getName()+":"+i);
                                            }
                                        }
                                    }
                                }.start();
                                new Thread(){
                                    public void run(){
                                        for(int i=0;i<100;i++){
                                            if(i%2==0){
                                                System.out.println(Thread.currentThread().getName()+":"+i);
                                            }
                                        }
                                    }
                                }.start();
                            }
                    ```
                
                线程的调度：
                    概念：高优先级的线程抢占CPU
                    用法：
                        线程的优先级：
                                MAX_PRIORITY:10
                                MIN_PRIORITY:1
                                NORM_PRIORITY:5
                        设置当前线程的优先级：
                            getpriority()：获取线程的优先级
                            setpriority(int p)：设置线程的优先级
                            说明：高优先级的线程要抢占低优先级线程的CPU的执行权。但是只是从概率上讲，高优先级的线程高概率的情况下被执行。并不意味着只有当高优先级的线程执行完以后，低优先级的线程才执行
                        事例：

                            ```java
                                public class TestTrhead {
                                    public static void main(String[] args){
                                        MyThread t2 = new MyThread();
                                        t2.setName("线程一"); //给线程t2命名
                                        //给t2线程设置优先级
                                        //先设置优先级再掉线程
                                        t2.setPriority(Thread.MAX_PRIORITY);
                                        t2.start();
                                        // 给主线程命名
                                        Thread.currentThread().setName("主线程");
                                        //给主线程设置优先级
                                        Thread.currentThread().setPriority(Thread.MIN_PRIORITY);
                                        //下面的操作任然是在main线程中执行的
                                        System.out.println("hello");
                                        for(int i=0;i<100;i++){
                                            if(i%2==0){
                                                System.out.println(Thread.currentThread().getName()+":"+Thread.currentThread().getPriority()+"--"+i);
                                            }
                                        }
                                    }
                                }
                                //常见一个继承与Thread类的子类
                                class MyThread extends Thread {
                                    //重写Thread类的run()
                                    public void run(){
                                        for(int i=0;i<100;i++){
                                            if(i%2==0){
                                                System.out.println(Thread.currentThread().getName()+":"+Thread.currentThread().getPriority()+"--"+i);
                                            }
                                        }
                                    }
                                }
                            ```

            2：实现Runnable接口
                步骤：
                    1：创建一个实现了Runnable接口的类
                    2：实现类去实现Runnable中的抽象方法：run()
                    3：创建实现类的对象
                    4：将此对象作为参数传递到Thread类的构造器中，创建Thread类的对象
                    5：通过Thread类的对象调用start()
                
                特点：共享数据特点
                
                方法：Thread里的方法，在Runnable中也可以用
                事例：

                    ```java
                        public class TestRunnable {
                            public static void main(String[] args){
                                //创建实现类的对象
                                MThread m = new MThread();
                                //将此对象作为参数传递到Thread类的构造器中，创建Thread了类的对象
                                Thread t1 = new Thread(m);
                                //通过Thread类的对象调用start();启动线程，调用当前线程的run()->调用了Runnable类型的target的run方法
                                t1.start();
                                
                                //再启动一个线程
                                Thread t2 = new Thread(m);
                                t2.start();
                            }
                        }
                        //1创建一个实现Runnable接口的类
                        class MThread implements Runnable{
                            @Override
                            //实现类去实现Runnable中的抽象方法:run()
                            public void run() {
                                for(int  i=0;i<100;i++){
                                    if(i%2==0){
                                        System.out.println(Thread.currentThread().getName()+"--"+i);
                                    }
                                }
                            }
                        }
                    ```
                说明：比较创建线程的两种方式;
                                开发中：优先选择，实现Runnable接口的方式
                                原因：1：实现的方式没有类的单继承性的局限性
                                        2：实现的方式更适合来处理多个想成的数据共享的情况
                                
                            联系：public class Thread implements Runnable
                            相同点：两种方式都需要重写run()，将线程要执行的逻辑声明在run()中

            3：实现Callable接口  JDK5.0新增
                如何理解实现callable接口的方式创建多线程比实现Runnable接口创建多线程方式强大？
                    1：call()可以有返回值的
                    2：call()可以抛出异常，被外面的操作捕获，获取异常的信息
                    3：Callable是支持泛型的
                
                事例：

                    ```java
                        public class TestCallab {
                            public static void main(String[] args){
                                //3创建Callable接口实现类的对象
                                NumThread m = new NumThread();
                                //4将此Callable接口实现类的对象作为参数传递到FutureTask构造器中，创建FutureTask的对象
                                FutureTask f = new FutureTask(m);
                                //5将FutureTask的对象作为参数传递到Thread类的构造器中，创建Thread对象，并调用start()
                                new Thread(f).start();
                                
                                //6获取callable中的call方法的返回值;如果对返回值不感兴趣则下面这步可以省略
                                try {
                                    Object sum = f.get();
                                    System.out.println(sum);
                                } catch (InterruptedException e) {
                                    // TODO Auto-generated catch block
                                    e.printStackTrace();
                                } catch (ExecutionException e) {
                                    // TODO Auto-generated catch block
                                    e.printStackTrace();
                                }
                            }
                        }
                        //1创建一个实现Callable的实现类
                        class NumThread implements Callable{
                            //2实现call方法，将此线程需要执行的操作声明在call()中
                            public Object call() throws Exception{
                                int sum = 0;
                                for(int i=0;i<=100;i++){
                                    if(i%2==0){
                                        System.out.println(i);
                                        sum +=i;
                                    }
                                }
                                return sum;
                            }
                        }
                    ```


            4：使用线程池
                什么是线程池？  提前创建好很多线程，放入线程池中，使用时直接获取，使用完放回池中。可以避免频繁创建销毁，实现重复利用。类似生活中的公共交通工具

                好处：提高响应速度(减少创建新线程的时间)
                      降低资源消耗(重复利用线程池中的线程，不需要每次都创建)
                      便于线程管理
                            corePoolSize：核心池的大小
                            maximumPoolSize：最大线程数
                            keepAliveTime：线程没有任务时最多保持多长时间后会终止

                事例：

                    ```java
                        public class ThreadPool {
                            public static void main(String[] args){
                                //1提供指定线程数量的线程池
                                ExecutorService service = Executors.newFixedThreadPool(10);
                                
                                //设置线程池的属性
                                ThreadPoolExecutor service1 = (ThreadPoolExecutor)service;
                        //		service1.setCorePoolSize(10);
                        //		service1.setKeepAliveTime(time, unit);
                                
                                //2执行指定的线程的操作。需要提供实现Runnable接口或Callable接口实现类的对象
                                service.execute(new NumberThread());
                                //关闭线程池
                                service.shutdown();
                            }
                        }

                        class NumberThread implements Runnable {
                            private int count;
                            public void run(){
                                for(int i=0;i<100;i++){
                                    if(i%2==0){
                                        System.out.println(Thread.currentThread().getName()+"此线程"+i);
                                    }
                                }
                            }
                        }
                    ```
            

            

    **线程安全问题
        线程安全问题出现的原因：根本原因多个线程操作共享数据；当某个线程操作车票的过程中，尚未操作完成时，其他线程参与进来，也操作车票

        如何解决：当一个线程a在操作车票的时候，其他线程不能参与进来。知道线程a操作完成车票时，其他线程才可以开始操作车票。这种情况即使线程a出现阻塞，其他线程也不能参与进来

        线程的同步
                为什么要用线程的同步：
                    多个线程执行的不确定性引起执行结果的不稳定
                    多个线程对账本的共享，会造成操作的不完整性，会破话数据
                    通过线程的同步可以解决这个安全问题

        解决方法：java中，通过同步机制，来解决线程的安全问题

            方式一：同步代码块 
                    synchronized(同步监视器){   //这里同步监视器又称锁
                        需要被同步的代码
                    }

                    synchronized执行机制：在执行完需要被同步的代码的时候，会自动的释放同步监视器(也就是锁)，让其他线程来拿锁

                    说明：1：操作共享数据的代码，即为需要被同步的代码  -->不能包含代码多了，也不能包含代码少了
                          2：共享数据：多个线程共同操作的变量。比如多个线程操作火车票票数，这里的票就是共享数据
                          3：同步监视器，俗称：锁。任何一个类的对象，都可以充当锁
                                要求：多个线程必须要共用同一把锁

                            补充，在实现Runnable接口创建多个线程的方式中，我们可以考虑使用this充当同步监视器

                    同步代码块的好处和局限性：
                            好处：解决了线程的安全问题
                            局限性：操作同步代码时，只能有一个线程参与，其他线程在同步代码块外面等着。相当于是一个单线程的过程，效率低


                    事例：同步代码块，解决线程安全问题，以下实现Runnable接口的事例解决了线程的安全问题，ticket没有重复且大于0

                        ```java
                            public class TestRunnable {
                                public static void main(String[] args){
                                    Window m = new Window();
                                    Thread t1 = new Thread(m);
                                    Thread t2 = new Thread(m);
                                    Thread t3 = new Thread(m);
                                    t1.setName("线程一");
                                    t2.setName("线程二");
                                    t3.setName("线程三");
                                    t1.start();
                                    t2.start();
                                    t3.start();
                                }
                            }
                            //1创建一个实现Runnable接口的类
                            class Window implements Runnable{
                                private int ticket  =100;
                                Object obj = new Object();
                                @Override
                                //实现类去实现Runnable中的抽象方法:run()
                                public void run() {
                                    while(true){
                                        //加锁，obj指的是同步监视器，三个线程必须是同一个监视器
                                        //补充，在实现Runnable接口创建多个线程的方式中，我们可以考虑使用this充当同步监视器
                                        synchronized(obj){
                                        //synchronized(this){
                                            if(ticket > 0){
                                                try {
                                                    Thread.sleep(100);//通过sleep阻塞线程
                                                } catch (InterruptedException e) {
                                                    e.printStackTrace();
                                                }
                                                System.out.println(Thread.currentThread().getName()+":买票"+ticket);
                                                ticket--;
                                            }else{
                                                break;
                                            }
                                        }
                                    }
                                }
                            }
                        ```
                    事例：继承Thread类的方式；用同步代码块，解决线程安全问题

                        ```java
                            public class TestTrhead {
                                public static void main(String[] args){
                                    MyThread t1 = new MyThread();
                                    MyThread t2 = new MyThread();
                                    MyThread t3 = new MyThread();
                                    t1.setName("线程一");
                                    t2.setName("线程二");
                                    t3.setName("线程三");
                                    t1.start();
                                    t2.start();
                                    t3.start();
                                }
                            }
                            //常见一个继承与Thread类的子类
                            class MyThread extends Thread {
                                private static int ticket = 100;
                                //定义静态对象
                                private static Object obj = new Object();
                                public void run(){
                                    while(true){
                                        synchronized(obj){
                                            if(ticket > 0){
                                                try {
                                                    Thread.sleep(100);//通过sleep阻塞线程
                                                } catch (InterruptedException e) {
                                                    e.printStackTrace();
                                                }
                                                System.out.println(Thread.currentThread().getName()+":买票"+ticket);
                                                ticket--;
                                            }else{
                                                break;
                                            }
                                        }
                                    }
                                }
                            }
                        ```
                            说明：在继承Thread类创建多线程的方式中，慎用this充当同步监视器，

            方式二：同步方法
                概念：操作共享数据的代码完整的声明在一个方法中，我们不妨将此方法声明同步的synchronized
                同步方法总结：
                    1：同步方法仍然涉及到同步监视器，知识不需要我们现实的声明
                    2：非静态的同步方法，同步监视器是：this
                       静态的同步方法，同步监视器是：当前类本身

                    事例：使用同步方法处理实现接口Runnable的方式中的线程安全问题

                        ```java
                            public class TestRunnable {
                                public static void main(String[] args){
                                    Window m = new Window();
                                    Thread t1 = new Thread(m);
                                    Thread t2 = new Thread(m);
                                    Thread t3 = new Thread(m);
                                    t1.setName("线程一");
                                    t2.setName("线程二");
                                    t3.setName("线程三");
                                    t1.start();
                                    t2.start();
                                    t3.start();
                                }
                            }
                            //1创建一个实现Runnable接口的类
                            class Window implements Runnable{
                                private int ticket  =100;
                                @Override
                                //实现类去实现Runnable中的抽象方法:run()
                                public void run() {
                                    while(true){
                                        show();
                                    }
                                }
                                private synchronized void show(){ //同步监视器：默认会添加this
                                    if(ticket > 0){
                                        try {
                                            Thread.sleep(100);//通过sleep阻塞线程
                                        } catch (InterruptedException e) {
                                            // TODO Auto-generated catch block
                                            e.printStackTrace();
                                        }
                                        System.out.println(Thread.currentThread().getName()+":买票"+ticket);
                                        ticket--;
                                    }
                                }
                            }
                        ```

                    事例：使用同步方法处理继承Thread类的方式中的线程安全问题

                        ```java
                            public class TestTrhead {
                                public static void main(String[] args){
                                    MyThread t1 = new MyThread();
                                    MyThread t2 = new MyThread();
                                    MyThread t3 = new MyThread();
                                    t1.setName("线程一");
                                    t2.setName("线程二");
                                    t3.setName("线程三");
                                    t1.start();
                                    t2.start();
                                    t3.start();
                                }
                            }
                            //常见一个继承与Thread类的子类
                            class MyThread extends Thread {
                                private static int ticket = 100;
                                //定义静态对象
                                private static Object obj = new Object();
                                public void run(){
                                    while(true){
                                        show();
                                    }
                                }
                                private static synchronized void show(){ //在继承中，用同步方法，要定义成静态方法  保证同步监视器是唯一的这里是MyThread.class
                                    //private synchronized void show(){  //同步监视器为t1,t2,t3
                                        if(ticket > 0){
                                            try {
                                                Thread.sleep(100);//通过sleep阻塞线程
                                            } catch (InterruptedException e) {
                                                // TODO Auto-generated catch block
                                                e.printStackTrace();
                                            }
                                            System.out.println(Thread.currentThread().getName()+":买票"+ticket);
                                            ticket--;
                                        }
                                }
                            }
                        ```

                    事例：使用同步机制将单例模式中的懒汉式改写为线程安全的

                        ```java
                            public class BankTest{}
                            class Bank{
                                private Bank(){}
                                private static Bank instance = null;
                                public static Bank getInstance(){
                                    //方式一：效率较差
                                    // synchronized(Bank.class){
                                    //     if(instance == null){
                                    //         instance = new Bank();
                                    //     }
                                    //     return instance;
                                    // }
                                    //方式二：效率较高
                                    if(instance == null) {
                                            synchronized(Bank.class){
                                            if(instance == null){
                                                instance = new Bank();
                                            }
                                        }
                                    }
                                    return instance;
                                }
                            }
                        ```

            方式三：Lock锁 JDK5.0新增
                面试题：1：synchronized与Lock的异同点
                            相同：二者都可以解决线程安全问题
                            不同：synchronized机制在执行完相应的同步代码以后，自动的释放同步监视器
                                Lock需要手动的启动(Lock()),同时结束同步也需要手动的实现(unLock())
                        2：使用解决线程安全问题的优先顺序
                            Lock->同步代码块->同步方法
                
                是什么？ java.util.concurrent.locks.Lock接口是控制多个线程对共享资源进行访问的工具。所提供了对共享资源的独占访问，每次只能有一个线程对Lock对象加锁，线程开始访问共享资源之前应先获得Lock对象

                用法：事例：

                    ```java
                        public class TestLock {
                            public static void main(String[] args){
                                Window m = new Window();
                                Thread t1 = new Thread(m);
                                Thread t2 = new Thread(m);
                                Thread t3 = new Thread(m);
                                t1.setName("线程一");
                                t2.setName("线程二");
                                t3.setName("线程三");
                                t1.start();
                                t2.start();
                                t3.start();
                                
                            }
                        }
                        class Window implements Runnable{
                            private int ticket = 100;
                            private ReentrantLock lock = new ReentrantLock(true); //传值true是公平的意思，意味着线程先进先出
                            @Override
                            public void run() {
                                while(true){
                                    lock.lock();//调用锁定方法lock()
                                    if(ticket > 0){
                                        try{
                                            try {
                                                Thread.sleep(100);//通过sleep阻塞线程
                                            } catch (InterruptedException e) {
                                                // TODO Auto-generated catch block
                                                e.printStackTrace();
                                            }
                                            System.out.println(Thread.currentThread().getName()+":买票"+ticket);
                                            ticket--;
                                        }finally{
                                            lock.unlock();//解除锁定
                                        }
                                    }
                                }
                            }
                        }
                    ```
                


    **线程的死锁
        什么叫死锁：
            不同的线程分别占用对方需要的同步资源不放弃，都在等待对方放弃自己需要的同步资源，就形成了线程的死锁
            出现阻塞后，不会出现异常，不会出现提示，只是所有的线程都处于阻塞状态，无法继续
        
        解决方法
            专门的算法。原则
            尽量减少同步资源的定义
            尽量避免嵌套同步

            事例：死锁的例子

                ```java
                    public class Testsisuo {
                        public static void main(String[] args){
                            StringBuffer s1 = new StringBuffer();
                            StringBuffer s2 = new StringBuffer();
                            new Thread(){   //线程1
                                public void run(){
                                    synchronized(s1){//同步监视器，这个锁是s1
                                        s1.append("a");
                                        s2.append("b");
                                        synchronized(s2){//同步监视器，这个锁是s2
                                            s1.append("A");
                                            s2.append("B");
                                            System.out.println(s1+"和"+s2);
                                        }
                                    }
                                }
                            }.start();
                            new Thread(new Runnable(){ //线程2
                                public void run(){
                                    synchronized(s2){//同步监视器，这个锁是s2
                                        s1.append("CC");
                                        s2.append("DD");
                                        synchronized(s1){//同步监视器，这个锁是s1
                                            s1.append("cc");
                                            s2.append("dd");
                                            System.out.println(s1+"和"+s2);
                                        }
                                    }
                                }
                            }).start();
                        }
                    }
                ```
                事例说明：开始的时候线程1执行拿到锁1，线程2拿到锁2；当线程1要拿锁2的时候，这是锁2被线程2拿着，并没有放。线程2又要拿线程1；所以两个线程就僵持着，形成死锁

    **线程通讯
        事例：线程通讯的例子：使用两个线程打印1-100。线程1，线程2，交替打印
        用到的方法：旦执行此方法，就会唤醒被wait的一个线程。如果有多个线程被wait，就唤醒优先级高的那个
                   notifyAll():一旦wait():一旦执行此方法，当前线程就进入阻塞状态，并且释放锁(同步监视器)
                   notify()：一执行此方法，就会唤醒所有被wait的线程
        说明：
            1：wait()，notify(),notifyAll()。三个方法必须使用在同步代码块或同步方法中
            2：wait()，notify(),notifyAll()，三个方法的调用者必须是同步代码块或同步方法中的同步监视器(监视器或锁调用这三个方法)，否则会出现异常
            3：wait()，notify(),notifyAll()，三个方法是定义在java.Lang.Object类中
        
        事例：

        ```java
            public class TestCONt {
                public static void main(String[] args){
                    Windows m = new Windows();
                    Thread t1 = new Thread(m);
                    Thread t2 = new Thread(m);
                    t1.setName("线程一");
                    t2.setName("线程二");
                    t1.start();
                    t2.start();
                }
            }
            class Windows implements Runnable{
                private int count =1;
                Object obj = new Object();
                @Override
                //实现类去实现Runnable中的抽象方法:run()
                public void run() {
                    while(true){
                        synchronized(obj){
                            obj.notify();//唤醒被wait()的线程
                            if(count <= 100){
                                try {
                                    obj.wait();//是的当前调用线程进入阻塞状态，并且释放锁
                                } catch (InterruptedException e) {
                                    // TODO Auto-generated catch block
                                    e.printStackTrace();
                                }
                                System.out.println(Thread.currentThread().getName()+":累计"+count);
                                count++;
                            }
                        }
                    }
                }
            }
        ```
        面试题：sleep()和wait()的异同
            1：相同点：一旦执行方法，都可以使得当前的线程进入阻塞状态
            2：不同点：
                    两个方法声明的位置不同：Thread类中声明的sleep();object类中声明的wait()
                    调用的要求不同：sleep()可以在任何需要的场景下调用。wait()必须使用在同步代码块中或同步方法中
                    关于是否释放同步监视器：如果两个方法都使用在同步代码块或同步方法中，sleep()不会释放锁，wait()会释放锁
        

##java常用类
    **String字符串
        理解原理：通过内存结构进行理解
        1：如何使用：
            1：使用一对""引起来表示
            2：String声明为final的，不可被继承
            3：String实现了Serializable接口：表示字符串是支持序列化的
                     实现了Comparable接口：表示String可以比较大小
            4：String内部定义了final char[] value用于存储字符串数据
            5：String：代表不可变的字符串序列。简称，不可变性
                    体现：1：当对字符串重新赋值时，需要重写指定内存区域赋值，不能使用原有的value进行赋值
                          2：当对现有的字符串进行连接操作时，也需要重新指定内存区域赋值，不能使用原有的value进行赋值
                          3：当调用String的replace()方法修改指定的字符或字符串时，也需要重新指定内存区域赋值，不能使用原有的value进行赋值
            6：通过字面量的方式(区别于new)给一个字符串赋值，此时的字符串值声明在字符串常量池中
            7：字符串常量池中是不会存储相同内容的字符串的

            上面的事例：

                ```java
                    public static void main(String[] args){
                            String s1 = "abc";//字面量的定义方式
                            String s2 = "abc";
                            System.out.println(s1 == s2);//true,比较s1和s2的地址值
                            
                            String s3 = "abc";
                            s3 += "def";
                            System.out.println(s3);//abcdef  在字符串后面拼接内容，得去新造一个abcdef，在常量池中新造的，因为有变量s3了所以在堆中产生，引用指向常量池
                            
                            String s4 = "abc";
                            String s5 = s4.replace('a', 'm'); //替换字符串，也得去新造一个，原有的字符串不变
                        }
                ```
        
        2：String对象的创建
            String的实例化方式：
                方式一：通过字面量的方式
                        通过字面量定义的方式：此时的s1的数据abc声明在方法区中的字符串常量池中
                        String s1 = "abc";
                方式二：通过new+构造器的方式
                        通过new+ 构造器的方式，此时s2保存的值，是数据在堆空间中开辟空间以后对应的地址值
                        String s2 = new String();
                
                题目：String s = new String("abc");方式创建对象，在内存中创建了几个对象？
                    两个：一个是堆空间中new结构，另一个是char[]对应的常量池中的数据:"abc"

                
                事例：

                ```java
                    String s1 = "hello";
                    String s2 = "world";
                    String s3 = "hello"+"world";
                    String s4 = s1 + "world";
                    String s5 = s1 + s2;
                    String s6 = (s1 + s2).intern();
                    System.out.println(s3==s4);//false
                    System.out.println(s3==s5);//false
                    System.out.println(s4==s5);//false
                    System.out.println(s3==s6);//true
                ```
                    结论：1：常量与常量的的拼接结果是在常量池。且常量池中不会存在相同内容的常量
                        2：只要其中有一个是变量，结果就在堆中
                        3：如果拼接的结果调用intern()方法，返回值就在常量池中；例如，会将s6的引用指向常量池中的"helloworld"地址；不会指向堆中的对象

                
                        
    **String常用方法
        int length():返回字符串的长度：return value.length
        char charAt(int index)：返回某索引处的字符 return value[index]
        boolean isEmpty()：判断是否是空字符串：return value.length == 0
        String toLowerCase()：使用默认语音环境，将String中的所有字符转换为小写
        String toUpperCase()：使用默认语言环境，将String中的所有字符转换为大写
        String trim()：返回字符串的副本，忽略前导空白和尾部空白
        boolean equals(Object obj)：比较字符串的内容是否相同
        booean equalsIgnoreCase(String anotherString)：与equals方法类似，忽略大小写
        String concat(String str)：将指定字符串连接到此字符串的结尾。等价于用"+"
        int compareTO(String anotherString):比较两个字符串的大小
        String substring(int beginIndex)：返回一个新的字符串，他是此字符串的从beginIndex开始截取
        String subString(int beginIndex,int endIndex)：返回一个新字符串，它是此字符串从beginIndex开始，到endInde结束(不包含)

    (下面是尚学堂的)
    **包装类
        概念：java是面向对象的语言，但并不是"纯面向对象"的，因为我们经常用到的基本数据类型就不是对象。但是我们在实际应用中经常需要将基本数据类型转化成对象，以便于操作，所以产生了包装类，包装类的实质还是由基本数据类型组成的。比如：将基本数据类型存储到Object[]数组或集合中的操作等等

        理解包装类：方法中比如say(Object e);这里的e参数就不可以用int类型因为int的祖父类就是他自己，但是可以用Integer类型，Integer最终类指向Object

        包装类均位于java.lang包，八种包装类和基本类型的对应关系如下：
                        基本数据类型        包装类
                            byte            Byte
                            boolean         Boolean
                            short           Short
                            char            Charcter
                            int             Integer
                            long            Long
                            float           Float
                            double          Double

            用法：
                实例：

                ```java
                    public static void main(String[] args){
                            //基本数据类型转成包装类对象
                            Integer a = new Integer(3);
                            Integer b = Integer.valueOf(30);
                            //把包装类对象转成基本数据
                            int c = b.intValue();
                            double d = b.doubleValue();
                            //把字符串转成包装类对象
                            Integer e = new Integer("99999");
                            Integer f = Integer.parseInt("999888");
                            //把包装类对象转成字符串
                            String str = f.toString();
                            //常见的常量
                            System.out.println("int类型最大的整数"+Integer.MAX_VALUE);
                        }
                ```

        2:自动装箱和拆箱
            1：自动装箱
                概念：基本类型的数据处于需要对象的环境中时，会自动转为"对象"
                实例：我们以Integer为例：在JDK1.5以前，这样的代码Integer i=5是错误的，必须要通过Integer i=new Integer(5)这样的语句来实现基本数据类型转换成包装类的过程；而在JDK1.5以后，java提供了自动装箱的功能，因此只需要Integer i=5这样的语句就能实现基本数据类型转换成包装类，这是因为JVM为我们执行了Integer.valueOf(5)这样的操作，这就是java的自动装箱

            2：自动拆箱
                概念：每当需要一个值时，对象会自动转成基本数据类型，没必要再去显示调用intValue()
                实现原理和自动拆箱的一样的，通过编译器进行转换(或者说是JVM执行了转换操作)

        3:包装类自动缓存
            用法
                实例：

                ```java
                    //缓存[-128,127]之间的数字，实际就是系统初始的时候，创建了[-128,127]之间的一个缓存数组
                    Integer in1 = 123;
                    Integer in2 = 123;
                    System.out.println(in1 == in2);		//true 因为123在缓存范围内
                    System.out.println(in1.equals(in2) );
                    Integer in3 = 1234;
                    Integer in4 = 1234;
                    System.out.println(in3 == in4);	//false 因为1234不在缓存范围内
                    System.out.println(in3.equals(in4));
                ```
                实例自动缓存原理：当我么你调用valueOf()的时候，首先检查是否在[-128,127]之间，如果在这个范围内则直接从缓存数组中拿出已经建好的对象如果不在这个范围，则创建新的Integer对象


    **时间相关类
        1：DateFormate类的作用：
            把时间对象转化成指定格式的字符串。反之，把指定格式的字符串转化成时间对象
            DateFormate是一个抽象类，一般使用它的子类SimpleDateFormate类来实现

            用法：
                事例：测试时间对象和字符串之间的互相转换

                ```java
                    public static void main(String[] args) throws ParseException{
                            Date d = new Date();
                            System.out.println(d);
                            System.out.println(d.getTime());	//转成毫秒
                            
                            //遇到日期处理，使用Canlendar类
                            
                            //把时间对象按照指定的格式，转成相应格式的字符串
                            DateFormat df = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");
                            String str = df.format(d.getTime());
                            System.out.println(str);	//2020-08-03 02:22:38
                            
                            //把指定格式的字符串按照"格式字符串指定的格式"转成相应的时间对象
                            DateFormat df2 = new SimpleDateFormat("yyyy年MM月dd日 hh时mm分ss秒");
                            Date date = df2.parse("1983年05月10日 10时45分59秒");
                            System.out.println(date);	//Tue May 10 10:45:59 CST 1983
                            
                            //测试其他的格式字符：比如：利用D，获得本时间对象是所处年份的第几天
                            DateFormat df3 = new SimpleDateFormat("D");
                            String str3 = df3.format(new Date());
                            System.out.println(str3);
                        }
                ```
        
        2：Canlendar日历类
            概念：
                Canlendar类是一个抽象类，为我们提供了关于日期计算的相关功能，比如：年，月，日，时，分，秒的展示和计算
                GregorianCalendar是Calendar的一个具体子类，提供了世界上大多数国家/地区使用的标准日历系统

            用法：
                事例

                ```java
                    public static void main(String[] args){
                            //获取日期的相关元素
                            Calendar calendar = new GregorianCalendar(2020,7,3,22,10,50);
                            int year = calendar.get(Calendar.YEAR);
                            int month = calendar.get(Calendar.MONTH);
                            int day = calendar.get(Calendar.DATE); //几号
                            int weekday = calendar.get(Calendar.DAY_OF_WEEK);//星期几，1：星期日，2：星期一。。。7星期六
                            System.out.println(year);	
                            System.out.println(month);	//0-11表示对应的月份
                            System.out.println(weekday);//表示星期几
                            System.out.println(day);
                            
                            
                            //设置日期的相关元素
                            Calendar c2 = new GregorianCalendar();//不传参数，默认设置当前日期
                            c2.set(Calendar.YEAR, 2090);
                            System.out.println(c2); //输出c2的日历的有关的对象的信息
                            
                            //日期的计算
                            Calendar c3 = new GregorianCalendar();
                            c3.add(Calendar.DATE, 100);	//往后一百天
                            c3.add(Calendar.YEAR, 100); //往后一百年
                            System.out.println(c3);
                            
                            //日期对象和时间对象的转化
                            Date d4 = c3.getTime();
                            Calendar c4 = new GregorianCalendar();
                            c4.setTime(new Date());
                            System.out.println(c4);
                            
                            printCalendar(c4);
                        }
                        
                        //疯转方法转换：将日期转成要想的时间格式
                        public static void printCalendar(Calendar c){
                            //打印：1918年10月10日 11:23:45 周三
                            int year = c.get(Calendar.YEAR);
                            int month = c.get(Calendar.MONTH)+1;
                            int date = c.get(Calendar.DAY_OF_MONTH);    //当月的第几天
                            int dayweek = c.get(Calendar.DAY_OF_WEEK)-1;//当月周几
                            String dayweek2 = dayweek==0?"日":dayweek+"";
                            
                            int hour = c.get(Calendar.HOUR);
                            int minute = c.get(Calendar.MINUTE);
                            int second = c.get(Calendar.SECOND);
                            
                            System.out.println(year+"年"+month+"月"+date+"日");
                        }
                ```

    **String类和常量池 (下面是尚学堂)
        1：概念：
            1：String类又称不可变字符序列
            2：java字符串就是Unicode字符序列，例如字符串"java"就是4个Unicode字符"j","a","v","a"组成的
            3：java没有内置的字符串类型，而是在标准的java类库中提供了一个预定义的类String，每个用双引号括起来的字符串都是String类的一个实例
            4：String属于引用数据类型
            5：声明String类型时，使用一对"",
            6:String可以和8种基本数据类型变量做运算，且运算只能是连接运算：+，运算结果任然为String类型

        2；全局字符串常量池
            全局字符串常量池中存放的内容是类加载完成后存放到String Pool中的，在每个VM中只有一份，存放的是字符串常量的引用值(在堆中生成字符串对象实例)

        3：class文件常量池
            class常量池是在编译的时候每个class都有的，在编译阶段，存放的是常量(文本字符串，final常量等)和符号引用

        4：运行时常量池
            运行时常量池是在类加载完成之后，将每个class常量池中的符号引用值存放到运行时常量池中，也就是说，每个class都有一个运行时常量池，类在解析之后，将符号引用替换成直接引用，与全局常量池中的引用保持一致
        
            实例：

                ```java
                    String str1 = "gaoqi";
                    String str2 = "gaoqi";
                    String str3 = new String("gaoqi");
                    System.out.println(str1 == str3)//返回false，str1引用指向"gaoqi"字符串常量池，str3指向新建的对象，两个地址不一样所以false
                    System.out.println(str1.equals(str3)) //返回true,因为equals直接比较内容
                ```

        5：String的API
                事例：

                    ```java
                        String s2 = " How are you! ";
                        String s3 = "shihui"
                        s = s2.trim();  //trim方法去除字符串守卫的空格。
                        s = s2.toLowerCase(); //toLowerCase转小写
                        s = s2.toUpperCase(); //toUpperCase转大写
                        s = s2.subString(4,7); //subString提取字符串：下标[4,7)不包括7
                        s = s2.startsWith("How"); //startsWith是否以How开头
                        s = s2.endsWith("How"); //endsWith是否以you结尾
                        s = s2.charAt(6); //charAt提取下标为6的字符
                        s = s2.length(); //length字符串长度
                        s = s2.equals(s3); //equals比较两个字符串是否相等
                        s = s2.equalsIgnoreCase(s3); //equalsIgnoreCase比较两个字符串是否相等(忽略大小写)
                        s = s2.indexOf("you"); //indexOf字符串s1中是否包含you
                        s = s2.replace('','&'); //replace将s2中的空格替换成&
                    ```

        6：StringBuilder和StringBuffer
            概念：
                StringBuilder和StringBuffer是可变字符序列(在堆中的原来的对象空间里修改)，而String类是不可变字符序列(堆中原来的对象中修改不了，修改完之后，赋值给新创建的对象)

                StringBuilder线程不安全，效率高(一般使用它)；StringBuffer线程安全，效率低

            用法：
                事例：简单的使用

                ```java
                    public static void main(String[] args){
                            StringBuilder sb = new StringBuilder("abcdefg");
                            System.out.println(Integer.toHexString(sb.hashCode())); //15db9742
                            System.out.println(sb); //abcdefg
                            sb.setCharAt(2,'M');
                            System.out.println(Integer.toHexString(sb.hashCode())); //15db9742
                            System.out.println(sb); //abMdefg
                        }
                ```
                    事例说明：说明了，StringBuilder是可变字符序列，修改的是字符本身

            
            使用不可变字符序列和可变字符序列使用时的性能测试
                事例：

                ```java
                    public static void main(String[] args){
                            //使用String进行字符串拼接
                            String str1 = "";
                            //本质上使用StringBuilder拼接，但是每次循环都会生成一个StringBuilder对象
                            long num1 = Runtime.getRuntime().freeMemory();//获取系统剩余内存空间
                            long time1 = System.currentTimeMillis(); //获取系统的当前时间
                            for(int i=0;i<5000;i++){
                                str1 = str1 + i;
                            }
                            long num2 = Runtime.getRuntime().freeMemory();
                            long time2 = System.currentTimeMillis();
                            System.out.println("String占用内存"+ (num2-num1));      //String占用内存12406960
                            System.out.println("String占用时间"+ (time2-time1));    //String占用时间81毫秒
                            
                            StringBuilder sb1 = new StringBuilder("");
                            long num3 = Runtime.getRuntime().freeMemory();
                            long time3 = System.currentTimeMillis();
                            for(int i=0;i<5000;i++){
                                sb1.append(i);
                            }
                            long num4 = Runtime.getRuntime().freeMemory();
                            long time4 = System.currentTimeMillis();
                            System.out.println("StringBuilder占用内存"+ (num4-num3));      //Stringbuilder占用内存0
                            System.out.println("StringBuilder占用时间"+ (time4-time3));     //StringBUilder占用时间1毫秒
                        }
                ```
                    测试结果：结果为Stringbuilder可变字符序列的性能比不可变字符的性能高很多。

##java集合(硅谷)
    **集合框架的概述
        1：集合，数组都是对多个数据进行存储操作的结构，简称java容器
            说明：此时的存储，主要指的是内存层面的存储，不涉及到持久化的存储(.txt,.jpg,.avi,数据库存储)
        2：数组和集合相比的优缺点
            2.1：数组在存储多个数据方面的特点
                一旦初始化以后，其长度就确定了
                数组一旦定义好，其元素的类型也就确定了。我们也就只能操作指定类型的数据了
            2.2：数组的存储多个数据方面的缺点
                一旦初始化以后，其长度就不可修改
                数组中提供的方法非常有限，对于添加，删除，插入数据等操作，非常不便，同时效率不高。
                获取数组中实际元素的个数的需求，数组没有现成的属性或方法可用
                数组存储数据的特点：有序，可重复。对于无需，不可重复的需求，不能满足

        3：java集合可分为Collection和Map俩种体系
            Collection接口：单列数据，定义了存储一组对象的方法的集合。但是Collection没有具体的实现类，它下面List和Set子接口有具体的实现类
                    List：元素有序，可重复的集合
                    Set：元素无需，不可重复的集合
            Map接口：双列数据，保存具有映射关系(key--value对)的集合。Map接口有它的实现类

    **集合框架
        1：概况：
            Collection接口：单列集合，用来存储一个一个的对象
                List接口:存储有序的，可重复的数据  --->“动态”数组
                        ArrayList，LinkedList，Vector
                Set接口：存储无序的，不可重复的数据
                        HashSet，LinkedHashSet，TreeSet

            Map接口：双列集合，用来存储一对(key-value)的数据
                    HashMap，LinkedHashMap，TreeMap，Hashtable，Properties

        2：Collection接口中的方法的使用
            //size():获取添加元素的个数
            //addAll(Collection coll):将coll1集合中的元素添加到当前的集合中
            //isEmpty():判断当前集合是否为空
            //clear()：清空集合元素
            //contains(Object obj)：判断当前集合中是否包含ogj，底层是通过equals进行对比的
                向Collection接口的实现类的对象中添加数据obj时，要求obj所在类要重写equals()方法
            //containsAll(Collection coll1):判断形参coll1中的所有元素是否都存在于当前集合中的数据，只要有一个不包含就返回false
            //remove(Object obj)：移除操作，移除成功返回true
            //removeAll(Collection coll1)：差集：从当前集合中移除coll1中所有的元素
            //retainAll(Collection coll1)：交集：获取当前集合和coll1的交集，并返回给当前集合
            //equals(Object obj)：要想返回true，需要当前集合和形参集合的元素都相同
            //hashCode():返回当前对象的哈希值
            //toArray()：集合转数组
            //Arrays.asList():数组转换成集合：调用Arrays类的静态方法asList()

            实例：

            ```java
                Collection coll = new ArrayList()
                coll.add("AA");
                coll.add("BB");
                coll.add("CC");
                coll.size();//

                Collection coll1 = new ArrayList()
                coll1.add(123);
                coll1.add("CC");
                coll.addAll(coll1);//将coll1集合中的元素添加到coll集合中

                coll.isEmpty()
                coll.clear()
            ```

##java集合(尚学堂)
    双列集合和单列集合的概念：
        Collection接口：单列集合，用来存储一个一个的对象
            List接口：存储有序的，可重复的数据 -->“动态”数组
                    ArrayList,LinkedList,Vector
            Set接口：存储无序的，不可重复的数据
                    hashSet,LinkedHashSet,TreeSet
            
            Map接口，双列集合，用来存储一对(key,value)的数据
                    HashMap,LinkHashMap,TreeMap,Hashtable,Properties
    
    概念：
        是个对象，用来装其他对象的对象
        在容器中Collection是父接口,list和set是子接口
    数组：
        数组就是一种容器，可以在其中放置对象或基本数据类型
        数组的优势：是一种简单的线性序列，可以快速地访问数组元素，效率高。如果从效率和类型检查的角度讲，数组是最好的
        数组的劣势：不灵活。容量需要事先定义好，不能随着需求的变化而扩容。比如：我们在一个用户管理中，要把今天注册的所有用户取出来，那么这样的用户有多少个？我们在写程序时就无法确定。因此，在这里就不能使用数组

    泛型：
        概念：泛型是JDK1.5以后增加的，它可以帮助我们建立类型安全的集合
        本质：就是"数据类型的参数化"。我们可以把"泛型"理解为数据类型的一个占位符(形式参数)，即告诉编译器，在调用泛型时必须传入实际类型
        用法：我们可以在类的声明处增加泛型列表，如：<T,E,V>，此处，字符可以是任何标识符，一般采用这3个字母

        事例：

            ```java
                public class TESTGeneric {
                    public static void main(String[] args){
                        MyCollection<String> mc = new MyCollection<String>/*new 类名<这里泛型实参可传可不传>*/();//传入的实参
                        mc.set("今天好开心", 0);
                        String b = mc.get(0);
                        System.out.println(b);
                    }
                }
                class MyCollection<E>{  //<E>相当于定了形参
                    Object[] objs = new Object[5];
                    public void set(E obj,int index){
                        objs[index] = obj;
                    }
                    public E get(int index){
                        return (E)objs[index]; //解释：Object的类型强制转成String，因为Object类是所有类型的父类
                    }
                }
            ```

    容器中使用泛型
        操作多个list交集并集
        事例：

        ```java
            public static void test01(){
                    List<String> list01 = new ArrayList<>();
                    list01.add("aa");
                    list01.add("bb");
                    list01.add("cc");
                    List<String> list02 = new ArrayList<>();
                    list02.add("aa");
                    list02.add("dd");
                    list02.add("ee");
                    //list01.addAll(list02);	//将list02里面的元素全部添加到list01里输出list01结果为[aa, bb, cc, aa, dd, ee]
                    //list01.removeAll(list02); //将相同的部分去掉输出list01结果为[bb, cc]
                    //list01.retainAll(list02);  //只输出相同部分，输出list01结果为[aa]
                    //list01.containsAll(list02);//list01里面是否包含list02里面所有的元素，是返回true，否返回false
                    System.out.println(list01);
                }
        ```

    **List:
        概念：
            list是有序，可重复的容器
            有序：List中每个元素都有索引标记。可以根据元素的索引标记(在List中的位置)访问元素，从而精确控制这些元素
            可重复：List允许加入重复的元素。更确切地讲，List通常允许满足e1.equals(e2)的元素重复加入容器

        List接口常用的实现类有3个：ArrayList,LinkedList和Vector

        建议：
            需要线程安全时，用Vector
            不存在线程安全问题时，并且查找较多用ArrayList(一般使用它)
            不存在线程安全问题时，增加或删除元素较多用LinkedList

        事例：

        ```java
            public static void test02(){
                    List<String> list = new ArrayList<>();
                    list.add("A");
                    list.add("B");
                    list.add("C");
                    list.add("D");
                    list.add(2, "石惠");//在2位置插入新的字符串"石惠"，后面的内容会依次往后移动，输出结果[A, B, 石惠, C, D]
                    list.remove(2);//把2位置处对应的内容删掉，输出结果[A, B, C, D]
                    list.set(2, "石老二");//将2位置处的内容替换成"石老二"
                    list.indexOf("B");//返回指定的位置第一次出现的位置，不存在返回-1
                    list.lastIndexOf("B");//返回最后出现的指定元素的位置，否则返回-1
                    System.out.println(list);
                }
        ```

        ArrayList
            特点：
                ArrayList底层是用数组实现的存储。特点：查询效率高，增删效率低，线程不安全
                数组长度是有限的，而ArrayList是可以存放任意数量的对象，长度不受限制
        
        LinkedList
            特点：增加或删除的效率非常高

        Vector
            特点：Vector底层是用数组实现的List，相关的方法都加上了同步检查，因此"线程安全，效率低"。比如，indexOf方法就增加了synchronized同步标记


    **set：
        概念：Set接口继承自Collection，Set接口中没有新增方法，方法和Collection保持完全一致。我们在前面通过List学习的方法，在Set中仍然适用。因此，学习Set的使用将没有任何难度

        特点：无序，不可重复。无序指Set中的元素没有索引，我们只能遍历查找；不可重复指的是不允许加入重复的元素。更确切地讲，新元素如果和Set中某个元素通过equals()方法对比为true，则不能加入；甚至，Set中也只能发乳一个null元素，不能多个

        Set常用的实现类有：HashSet，TreeSet等。我们一般使用HashSet

        HashSet
            事例：

                ```java
                    public class TestHashSet {
                        public static void main(String[] args){
                            Set<String> set1 = new HashSet<>();
                            set1.add("aa");
                            set1.add("bb");
                            set1.add("cc");
                            System.out.println(set1);//[aa, bb, cc]
                            Set<String> set2 = new HashSet<>();
                            set2.add("小时");
                            set2.addAll(set1);
                            System.out.println(set2); //[aa, bb, cc, 小时]
                        }
                    }
                ```

            事例：hashSet源码实现

                ```java
                    public class SxtHashSet {
                        private static final Object PRESENT = new Object();
                        HashMap map;
                        public SxtHashSet() {
                            super();
                            map = new HashMap();
                        }
                        public int size(){
                            return map.size();
                        }
                        public String toString(){
                            StringBuilder s = new StringBuilder();
                            s.append("[");
                            for(Object key:map.keySet()){
                                s.append(key + ",");
                            }
                            s.setCharAt(s.length()-1, ']');
                            return s.toString();
                        }
                        public void add(Object o){
                            map.put(o, PRESENT);
                        }
                        public static void main(String[] args){
                            SxtHashSet  set = new SxtHashSet();
                            set.add("aa");
                            set.add("bb");
                            set.add("cc");
                            System.out.println(set);//[aa,bb,cc]
                        }
                    }
                ```

        
        TreeSet的使用和实现
            概念：TreeSet底层实际是用TreeMap实现的，内部维持了一个简化版的TreeMap 通过key来存储Set的元素。TreeSet内部需要对存储的元素进行排序，因此，我们对应的类需要实现Comparable接口。这样，才能根据compareTo()方法比较对象之间的大小，才能进行内部排序

            原理实现：TreeSet的排序的实现原理和TreeMap的实现原理是一样的，可以参考TreeMap的代码

            事例：TreeSet简单的使用

                ```java
                    public class TestTreeSet {
                        public static void main(String[] args){
                            Set<Integer> set = new TreeSet<>();
                            set.add(300);
                            set.add(200);
                            set.add(600);
                            for(Integer key:set){
                                System.out.println(key);
                                //按照顺序输出200，300，600
                            }
                        }
                    }
                ```

            
    **map
        概念：map就是用来存储"键(key)-值(value)对"的。Map类中存储的"键值对"通过键来标识，所以"键对象"不能重复,如果键重复的话，新的覆盖旧的

        Map接口中实现类有HashMap,treeMap,HashTable,Properties等

        Map接口中常用的方法
            object put(object key,Object value)     存放键值对
            Object get(Object key)                  通过键对象查找得到值对象
            Object remove(Object key)               删除键对象对应的键值对
            boolean containsKey(object key)         Map容器中是否包含键对象对应的键值对
            boolean containsvalue(Object value)     Map容器中是否包含值对象对应的键值对
            int size()                              包含键值对的数量
            boolean isEmpty()                       Map是否为空
            void putAll(Map t)                      将t的所有键值对存放到本map对象
            void clear()                            清空本map对象所有键值对

        事例：简单的例子

            ```java
                public class TestMap {
                    public static void main(String[] args){
                        Map<Integer,String> m1 = new HashMap<>();
                        m1.put(1, "one");
                        m1.put(2, "two");
                        m1.put(3, "three");
                        m1.put(4, "four");
                        System.out.println(m1.get(1)); //one
                        System.out.println(m1.size()); //4
                        System.out.println(m1.isEmpty());//false
                        System.out.println(m1.containsKey(2));//true
                    }
                }
            ```

        事例：键值对：键是Interger类型，值是Object类型

            ```java
                public class TestMap {
                    public static void main(String[] args){
                        Employee e1 = new Employee(1001,"石惠",50000);
                        Employee e2 = new Employee(1002,"侠士",60000);
                        Employee e3 = new Employee(1003,"老式",70000);
                        Map<Integer,Employee> m1 = new HashMap<>();
                        m1.put(1, e1);
                        m1.put(2, e2);
                        m1.put(3, e3);
                        System.out.println(m1);
                        //{1=id:1001name:石惠薪水50000.0, 2=id:1002name:侠士薪水60000.0, 3=id:1003name:老式薪水70000.0}
                    }
                }

                class Employee{
                    int id;
                    String ename;
                    double sarlay;
                    public Employee(int id, String ename, double sarlay) {
                        super();
                        this.id = id;
                        this.ename = ename;
                        this.sarlay = sarlay;
                    }
                    public String toString(){
                        return "id:"+id+"name:"+ename+"薪水"+sarlay;
                    }
                    public int getId() {
                        return id;
                    }
                    public void setId(int id) {
                        this.id = id;
                    }
                    public String getEname() {
                        return ename;
                    }
                    public void setEname(String ename) {
                        this.ename = ename;
                    }
                    public double getSarlay() {
                        return sarlay;
                    }
                    public void setSarlay(double sarlay) {
                        this.sarlay = sarlay;
                    }
                }
            ```

        
        HashMap底层实现采用了哈希表，这是一种非常重要的数据结构，哈希表的基本结构就是"数组+链表"

            知识点介绍：
                数据接口中由数组和链表和哈希表来实现对数据的存储，他们各有特点
                1：数组：占用空间连续。寻址容易，查询速度快。但是，增加和删除效率非常低
                2：链表：占用空间不连续。寻址困难，查询速度慢。但是，增加和删除效率非常高。
                3：哈希表：哈希表结合了数组和链表的有点(即查询快，增删效率也高)，本质就是"数组+链表"
            
            HashMap存储数据put(key,value)的原理：
                1：获得key对象的hashcode，首先调用key对象的hashcode()方法，获得hashcode
                2：根据hashcode计算出hash值(要求在[0,数组长度-1区间)。通过将hash值把数据存储到对应的数组的位置，(数组中的每一项都是链表)

            HashMap取数据get的原理：
                1：获得key的hashcode，通过hash()散列算法得到hash值，进入定位到数组的位置
                2：在链表上挨个比较key对象。调用equals()方法，将key对象和链表上所有节点的key对象进行比较，直到碰到返回true的节点对象为止
                3：返回equals()为true的节点对象的value对象

        
        TreeMap
            概念：
                TreeMap和HashMap实现了同样的接口Map，因此，用法对于调用者来说没有区别。
                HashMap效率高于TreeMap，在需要排序的Map时(key或者value需要排序的时候)才选用TreeMap

            事例：

                ```java
                    public class TestTree {
                        public static void main(String[] args){
                            Map<Integer,String> treemap1 = new TreeMap<>();
                            treemap1.put(20,"aa");
                            treemap1.put(2,"VV");
                            treemap1.put(6,"CC");
                            //按照key递增的方式排序
                            for(Integer key:treemap1.keySet()){
                                System.out.println(key);
                            }
                            Map<Emp,String> treemap2 = new TreeMap<>();
                            treemap2.put(new Emp(100, "张三",50000),"张三是个好小伙子");
                            treemap2.put(new Emp(200, "李四",5000),"张三是个好小伙子");
                            treemap2.put(new Emp(30, "王五",6000),"张三是个好小伙子");
                            treemap2.put(new Emp(3, "金六福",6000),"张三是个好小伙子");
                            for(Emp key:treemap2.keySet()){
                                System.out.println(key);
                                //输出结果
                                // id:200,name:李四,salary:5000.0
                                // id:3,name:金六福,salary:6000.0
                                // id:30,name:王五,salary:6000.0
                                // id:100,name:张三,salary:50000.0
                            }
                        }
                    }
                    class Emp implements Comparable<Emp>{ //实现Comparable接口，方便排序
                        int id;
                        String name;
                        double salary;
                        public Emp(int id, String name, double salary) {
                            super();
                            this.id = id;
                            this.name = name;
                            this.salary = salary;
                        }
                        public String toString(){
                            return "id:"+id+",name:"+name+",salary:"+salary;
                        }
                        @Override
                        public int compareTo(Emp o) {
                            //TreeMap排序实现原理： Emp实例对象通过实现接口Comparable的CompareTo比较方法来进行排序
                            //负数：小于0；等于：整数；正数：大于
                            if(this.salary > o.salary){
                                return 1;
                            }else if(this.salary < o.salary){
                                return -1;
                            }else{
                                if(this.id>o.id){
                                    return 1;
                                }else if(this.id < o.id){
                                    return -1;
                                }else{
                                    return 0;
                                }
                            }
                        }
                    }
                ```

        HashMap和HashTable的区别
            1：HashMap：线程不安全，效率高。允许key或value为null
            2：HashTable：线程安全，效率低。不允许key或value为null

        

        properties
            用法：
                事例：读取配置文件操作

                ```java
                    public class PropertyiesTest {
                        public static void main(String[] args) throws IOException {
                            Properties pros = new Properties();
                            FileInputStream fis = new FileInputStream("jdbc.properties"); //查找项目中以.properties结尾的文件进行加载
                            pros.load(fis);//加载流对应的文件
                            String name = pros.getProperty("name");
                            String password = pros.getProperty("password");
                            System.out.println("name:"+name+"\tpassword:"+password);//输出结果name:tom password:123456
                        }
                    }
                ```

    

    

    
    **Iterator迭代器
        概念：
            1：Iterator对象称为迭代器为我们提供了统一的遍历容器Collection(List/Set>)的方式

            2：迭代器模式定义为：提供一种方法访问一个容器(container)对象中各个元素，而又不需要暴露该对象的内部细节。迭代器模式，就是为容器而生。

            3：Collection接口继承了java.lang.Iterable接口，该接口有一个iterator()方法，那么所有实现了Collection接口的集合类都有一个Iterator()方法，用以返回一个实现了Iterator接口的对象

            4：Iterator仅用于遍历集合，Iterator本身并不提供承装对象的能力。如果需要创建Iterator对象，则必须有一个被迭代的集合

            5：集合对象每次调用iterator()方法都得到一个全新的迭代器对象，默认游标都在集合的第一个元素之前


        迭代器的执行原理：
            例子：Iterator iterator = coll.iterator()
            //hasNext():判断是否还有下一个元素
            while(iterator.hasNext()){
                //next()的作用:1指针下移2：下移以后将集合的位置上的元素返回
                System.out.println(iterator.next());
            }

        迭代器remove()方法的使用
            说明：内部定义了remove()，可以在遍历的时候，删除集合中的元素。此方法不同于集合直接调用remove()
            注意点：
                Iterator可以删除集合中的元素，但是是遍历过程中通过迭代器对象remove方法，不是集合对象的remove方法
                如果还未调用next()或在上一次调用next方法之后已经调用了remove方法，再调用remove都会报illegalStateException

                事例：

                ```java
                    public static void test1(){
                            Collection coll = new ArrayList();
                            coll.add(123);
                            coll.add(456);
                            coll.add("今天天气真好");
                            coll.add(false);
                            //删除集合中"今天天气真好"
                            Iterator itrerator = coll.iterator();
                            while (itrerator.hasNext()){
                                Object obj = itrerator.next();
                                if("今天天气真好".equals(obj)){
                                    itrerator.remove();
                                }
                            }
                            //上面删除之后，再重新遍历集合
                            itrerator = coll.iterator();
                            while (itrerator.hasNext()){
                                System.out.println(itrerator.next());
                            }
                        }
                ```

        forEach循环
            概念：增强for循环；用于遍历集合，数组
            语法：for(集合元素的类型 局部变量 ：集合对象)
            原理：内部任然调用的了迭代器
            Collection coll = new ArrayList();
            for(Object obj:coll){

            }
        
        使用Iterator遍历List
            事例：

            ```java
                public static void testIteratorList(){
                        List<String> list = new ArrayList<>();
                        list.add("aa");
                        list.add("bb");
                        list.add("cc");
                        //使用iterator遍历List
                        for(Iterator<String> iter = list.iterator();iter.hasNext();){
                            String temp = iter.next();//遍历输出当前值，并且将浮标移动到下一位进行遍历
                            System.out.println(temp);
                        }
                    }
            ```

        使用Iterator遍历Set
            事例：

                ```java
                    public static void testIteratorSet(){
                            Set<String> set = new HashSet<>();
                            set.add("aa");
                            set.add("bb");
                            set.add("cc");
                            //使用iterator遍历List
                            for(Iterator<String> iter = set.iterator();iter.hasNext();){
                                String temp = iter.next();//遍历输出当前值，并且将浮标移动到下一位进行遍历
                                System.out.println(temp);
                            }
                        }
                ```

        使用Iterator遍历Map
            事例：

                ```java
                    public static void main(String[] args){
                            Map<Integer,String> map = new HashMap<>();
                            map.put(6,"小时");
                            map.put(3,"小石");
                            map.put(5,"石惠");
                            Set<Integer> ss = map.keySet();
                            for(Iterator<Integer> iterator = ss.iterator();iterator.hasNext();){
                                Integer key = iterator.next();
                                System.out.println(key+"---"+map.get(key));
                            }
                        }
                ```

    理解Set<Integer> ss = map.keySet()，调用方法后返回类名<泛型>的形式
        事例理解：

            ```java
                class my {
                    public static List<Integer> say(){
                        List<Integer> list = new ArrayList<Integer>();
                        list.add(1);
                        return list;
                    }
                } 

                List<Integer> say = my.say(); //调用方法之后返回say，say就是关于List大类，泛型是Integer的类型
            ```


    **Collections工具类
        概念：类java.util.Collections提供了对Set,List,Map进行排序，查找元素的辅助方法
        区别：Collection是接口，Collections是工具类
        常见的方法：
            1：void sort(list) //对List容器内的元素进行排序，排序的规则是按照升序排列
            2：void shuffle(list) //对List容器内的元素进行随机排序
            3：void reverse(list) //对List容器内的元素进行逆续排列
            4：void fill(List,Object) //用一个特定的对象重写整个List容器
            5：int binarySearch(List,Object) //对于顺序的List容器，采用折半查找的方法查找特定的对象

            事例：

            ```java
                public static void main(String[] args){
                        List<String> list = new ArrayList<>();
                        for(int i =0;i<5;i++){
                            list.add("小石"+i);
                        }
                        System.out.println(list);
                        Collections.shuffle(list);//随机排列
                        System.out.println(list);
                        Collections.reverse(list);//逆序
                        System.out.println(list);
                        Collections.sort(list);//正序排序
                        System.out.println(list);
                        System.out.println(Collections.binarySearch(list, "小石0"));//折半法查找元素，list中无查找元素则返回负数，存在返回相应index
                        Collections.fill(list,"hello");//将list中的所有元素换成hello
                        System.out.println(list);
                    }
            ```


    **存储表格数据
        理解：存储表格数据的核心思想是对象关系映射(ORM思想)

        事例：每一行数据使用一个map，整个表格使用一个List(将数据存储在对象中)

            ```java
                public static void main(String[] args){
                        Map<String,Object> m = new HashMap<>();//将每行数据存储在Map对象中
                        m.put("id", 1001);
                        m.put("name", "张三");
                        m.put("薪水", 20000);
                        m.put("入职日期", "2018.5.5");
                        
                        Map<String,Object> m1 = new HashMap<>();
                        m1.put("id", 1002);
                        m1.put("name", "李四");
                        m1.put("薪水", 2000);
                        m1.put("入职日期", "2014.5.4");
                        
                        Map<String,Object> m2 = new HashMap<>();
                        m2.put("id", 1003);
                        m2.put("name", "王五");
                        m2.put("薪水", 5000);
                        m2.put("入职日期", "2019.5.10");
                        
                        List<Map<String,Object>> table1 = new ArrayList<>();//将每一行map对象，存储在List中
                        table1.add(m);
                        table1.add(m1);
                        table1.add(m2);
                        for(Map<String,Object> row:table1){
                            Set<String> keyset = row.keySet();
                            for(String key:keyset){
                                System.out.print(key+"---"+row.get(key)+"\t");
                            }
                            System.out.println();
                        }
                    }       //输出结果：
                            // name---张三	薪水---20000	id---1001	入职日期---2018.5.5	
                            // name---李四	薪水---2000	id---1002	入职日期---2014.5.4	
                            // name---王五	薪水---5000	id---1003	入职日期---2019.5.10
            ```
        
        事例：每一行数据使用javabean对象存储，多行使用放到map或list中
            javaBean的，java类中符合javabean的标准如下：
                类是公共的
                有一个无参的公共构造器
                有属性，且有对应的get，set方法


            ```java
                public class TestStore {
                    public static void main(String[] args){
                        Users user = new Users(1001,"张三",20000,"2018.5.5");
                        Users user1 = new Users(1001,"张三",20000,"2018.5.5");
                        Users user2 = new Users(1001,"张三",20000,"2018.5.5");
                    
                        List<Users> list = new ArrayList<>();
                        list.add(user);
                        list.add(user1);
                        list.add(user2);
                        for(Users m:list){
                            System.out.println(m);
                        }
                    }
                }
                class Users{
                    private int id;
                    private String name;
                    private double salary;
                    private String handate;
                    
                    public Users(int id, String name, double salary, String handate) {
                        super();
                        this.id = id;
                        this.name = name;
                        this.salary = salary;
                        this.handate = handate;
                    }
                    public String toString(){
                        return "id:"+id+"name:"+name+"salary:"+salary+"handate:"+handate;
                    }
                }
            ```



        

            


    
            


##泛型(硅谷)
    为什么要有泛型

        概念：所谓泛型，就是允许在定义类，接口时通过一个标识表示类中某个属性的类型或者是某个方法的返回值及参数类型。这个类型参数将在使用时(例如，继承或实现这个接口，用这个类型声明变量，创建对象时)确定(即传入实际的类型参数，也称为类型实参)

    在集合中使用泛型
        1:集合接口或集合类在jdk5.0时都修改为带泛型的结构
        2：在实例化集合时，可以指明具体的泛型类型
        3：指明完以后，在集合类或接口中凡是定义类或接口时，内部结构(比如：方法，构造器，属性等)使用到类的泛型的位置，都指定为实例化的泛型类型。
            比如：add(E e) --->实例化以后：add(Integer e)
        4：注意点：泛型的类型必须是一个类，不能是基本数据类型，我们需要用到基本数据类型的位置，拿包装类替换
        5：如果实例化时没有指定泛型的类型(没有用泛型)。默认类型为java.lang.Object类型


    自定义泛型结构或泛型类
        说明：泛型类，泛型接口；泛型方法
        事例：

            ```java
                //自定义泛型类或泛型接口
                public class GenericTest1 {
                    //如果定义了泛型类，实例化没有指名类的泛型，则认为此此泛型类型为Object类型
                    //要求：如果大家定义了类的泛型的，建议在实例化时要指名类的泛型
                    //建议：实例化时指名类的泛型
                    Order<String> order = new Order<>();

                    //子类在继承带泛型的父类时，指明了泛型类型。则实例化子类对象时，不再需要指名泛型,子类不是泛型类
                        //实例：
                        class subOrder extends Order<String>{}

                    //子类在继承带泛型的父类时，没有指明泛型类型，则子类也是泛型类
                        //实例：
                        class subOrder1<T> extends Order<T>{}
                }
            ```

        细节点：
            1：泛型类可能有多个参数，此时应将多个参数一起放在尖括号内，比如<T1,T2,T3>
            2:泛型类的构造器如下：public GenericClass(){}
               而下面是错误的：public GenericClass<T>{}
            3：实例化后，操作原来泛型位置的结构必须与指定的泛型类型一致
            4：泛型不同的引用不能相互赋值
                但是尽管在编译时ArrayList<String>和ArrayList<Integer>是两种类型，但是，在运行时只有一个ArrayList被加载带JVM中
            5：泛型如果不指定，将被擦除，泛型对应的类型均按照Object处理，但不等价于Object。泛型要使用一路都用，要不用，一路都不用
            6：如果泛型结构是一个接口或抽象类，则不可创建泛型类的对象
            7：jdk1.7，泛型的简化操作：ArrayList<String> first = new ArrayList<>{}，自定补充
            8：泛型的指定中不能使用基本数据类型，可以使用包装类替换
            9：在类/接口上声明的泛型，在本类或本接口中即代表某种类型，可以作为非静态属性的类型，非静态方法的参数类型，非静态方法的返回值类型。但在静态方法中不能使用类的泛型(从类和实例的生命周期考虑)
            10：异常类不能是泛型
            11：通过泛型实例化时，不能使用new T[]。但是可以：T[] element = (T[])new Object[];
            12：父类有泛型，子类可以选择保留泛型也可以选择指定泛型类型
                有以下这几种情况：

                ```java
                    class Father<T1,T2>{}
                    //子类不保留父类的泛型
                    //1：没有类型  擦除
                    class Son1 extends Father{} //等价于class Son extends Father<Object,Object>{}
                    //2:具体类型
                    class Son2 extends Father<Integer,String>{}

                    //子类保留父类的泛型
                    //1:全部保留
                    class Son3<T1,T2> extends Father<T1,T2>{}
                    //2：部分保留
                    class Son4<T2> extends Father<Integer,T2>{}

                    //子类不保留父类的泛型
                    //1：没有类型 擦除
                    class Son<A,B> extends Father{}//等价于 class Son extends Father<Object,Object>{}
                    //2具体类型
                    class Son<A,B> extends Father<Integer,String>{} //另外的Son额外的还有自己的泛型

                    //子类保留父类的泛型
                    //1：全部保留
                    class Son<T1,T2,A,B> extends Father<T1,T2>{}
                    //2：部分保留
                    class Son<T2,A,B> extends Father<Integer,T2>{}
                ```

    自定义泛型方法
        概念：在方法中出现了泛型的结构，泛型的参数与类的泛型参数没有任何关系
              换句话说，泛型方法所属的类是不是泛型类都没有关系
        
        泛型方法的基本结构： public <T> T see(){}
        
        用法：
            实例：

            ```java
                public <E> List<E> copyFromArrayTolist(E[] arr){
                        ArrayList<E> list = new ArrayList<>();
                        for(E e:arr){
                            list.add(e);
                        }
                        return list;
                    }
            ```
        说明：泛型方法，可以声明为静态的。原因：泛型参数是在调用方法时确定的。并非在实例化类时确定

    泛型在继承方面的体现
        虽然类A是类B的父类，但是G<A>和G<B>二者不具备子类父类关系，二者是并列关系
        补充：类A是类B的父类，A<G>是B<G>的父类

        各种事例说明：

            ```java
                Object obj = null;
                String str = null;
                obj = str;

                Object[] arr1 = null;
                String[] arr2 = null;
                arr1 = arr2;

                List<Object> list1 = null;
                List<String> list2 = null;
                //此时的list1和list2的类型不具备子父类关系
                //list1 = list2;

                List<String> list3 = null;
                ArrayList<String> list4 = null;
                list3 = list4;//这时候是可以的
            ```
        

    通配符的使用
        1：通配符：?
        2：说明：类A和类B的父类，G<A>和G<B>是没有关系的，二者共同的父类是：G<?>
            事例说明：通配符的作用：

            ```java
                List<Object> list1 = null;
                List<String> list2 = null; 
                List<?> list = null;
                list = list1;
                list = list2;
                //说明：list1和list2是并列关系，通配符让list是list1和list2的父类
            ```

            事例：

            ```java
                List<?> list = null;
                List<String> list1 = new ArrayList<>();
                list1.add("AA");
                list1.add("BB");
                list1.add("CC");
                list = list1;
                //添加或者写入：对于List<?>就不能向其内部添加数据。除了添加null之外
                //list.add("DD");
                
                //获取(读取)：允许读取数据，读取数据类型为Object
                Object o = list.get(0);
                System.out.println(o);
            ```
        
        3：有限制条件的通配符的使用
            事例：

            ```java
                List<? extends Person> list1 = null;
                List<? super Person> list2 = null;
                List<Person> list3 = null;
                List<Student> list4 = null;
                List<Object> list5 = null;
                //list1大于等于list3或list4的类，可以将list3或list4赋值给list1，但是大于list1的类不能赋值给list1
                list1 = list3;
                list1 = list4;
                //list1 = list5;不可以，编译不通过

                //list2小于于list3或list5的类，可以将list3或list5赋值给list1，但是小于list1的类不能赋值给list1
                //list2 = list4;不可以，编译不通过
                list2 = list3;
                list2 = list5;
            ```

    泛型应用举例
        DAO类

        ```java
            public class DAO<T> {
                private Map<String,T> map = new HashMap<>();
                //保存T类型的兑现到map成员变量中
                public void save(String id,T entity){
                    map.put(id,entity);
                }
                //从map中获取id对应的对象
                public T get(String id){
                    return map.get(id);
                }
                //替换map中key为id的内容，改为entity对象
                public void update(String id,T entity){
                    if(map.containsKey(id)){
                        map.put(id,entity);
                    }
                }
                //返回map中存放的所有的T对象
                public List<T> list(){
                    Collection<T> values = map.values();
                    List<T> list = new ArrayList<>();
                    for(T e:values){
                        list.add(e);
                    }
                    return  list;
                }
                //删除指定id对象
                public void remove(String id){
                    map.remove(id);
                }
            }
        ```

        User类

        ```java
            public class User {
                private int id;
                private int age;
                private String name;
                public User(int id, int age, String name) {
                    this.id = id;
                    this.age = age;
                    this.name = name;
                }
                @Override
                public String toString() {
                    return "User{" +
                            "id=" + id +
                            ", age=" + age +
                            ", name='" + name + '\'' +
                            '}';
                }
                @Override
                public boolean equals(Object o) {
                    if (this == o) return true;
                    if (o == null || getClass() != o.getClass()) return false;

                    User user = (User) o;

                    if (id != user.id) return false;
                    if (age != user.age) return false;
                    return name != null ? name.equals(user.name) : user.name == null;
                }
            }
        ```

        DAOTest类

        ```java
            public class DAOTest {
                public static void main(String[] args) {
                    DAO<User> dao = new DAO<>();
                    dao.save("1001",new User(1001,34,"周杰伦"));
                    dao.save("1002",new User(1002,20,"昆凌"));
                    dao.save("1003",new User(1003,34,"蔡依林"));
                    List<User> list = dao.list();
                    list.forEach(System.out::println);
                    System.out.println(list);
                }
            }
        ```

##字节，位，字符集
    位：
        概念：位是计算机存储的最bai小单位，简记为b，也称为比特（bit）计算机中用二进制中的0和1来表示数据，一个0或1就代表一位。位数通常指计算机中一次能处理的数据大小

    字节：
        概念：字节，英文Byte，是计算机用于计量存储容量的一种计量单位，通常情况下一字节等于八位，字节同时也在一些计算机编程语言中表示数据类型和语言字符，在现代计算机中，一个字节等于八位

    字符：(char)
        概念：字符是指计算机中使用的文字和符号，比如1、2、3、A、B、C、~！·#￥%……—*（）——+、等等
    
    字节和位之间的关系：1字节=8位
    字节和字符的关系：字节是字符的组成单元

    字节与字符：
        ASCII 码中，一个英文字母（不分大小写）为一个字节，一个中文汉字为两个字节。
        UTF-8 编码中，一个英文字为一个字节，一个中文为三个字节。
        Unicode 编码中，一个英文为一个字节，一个中文为两个字节。
        符号：英文标点为一个字节，中文标点为两个字节。例如：英文句号 . 占1个字节的大小，中文句号 。占2个字节的大小。
        UTF-16 编码中，一个英文字母字符或一个汉字字符存储都需要 2 个字节（Unicode 扩展区的一些汉字存储需要 4 个字节）。
        UTF-32 编码中，世界上任何字符的存储都需要 4 个字节。
    


##IO流
    **File类(尚学堂)
        概念：java.io.File类：代表文件和目录。在开发中，读取文件，生成文件，删除文件，修改文件的属性时经常会用到
        用法：
            事例：

            ```java
                public static void main(String[] args) throws IOException{
                        File f = new File("dd.txt");
                        f.createNewFile();
                        System.out.println(System.getProperty("user.dir"));//当前所在的绝对路径
                        System.out.println("File是否存在"+f.exists());//File是否存在true
                        System.out.println("File是否是目录"+f.isDirectory());//File是否是目录false
                        System.out.println("File是否是文件"+f.isFile());//File是否是文件true
                        System.out.println("File最后修改时间"+new Date(f.lastModified()));//File最后修改时间Mon Aug 03 16:44:09 CST 2020
                        System.out.println("File的大小"+f.length());//File的大小0
                        System.out.println("File的文件名"+f.getName());//File的文件名dd.txt
                        System.out.println("File的目录路径"+f.getAbsolutePath());//File的目录路径C:\javalianxi\Duixiang\dd.txt


                        File f2 = new File("c:/学习/话语/大陆");
                        //boolean flag = f2.mkdir();//目录结构中只要有一个不存在，则不会创建整个目录树
                        boolean flag = f2.mkdirs();//目录结构中有一个不存在也没关系；创建整个目录树
                        System.out.println(flag);
                    }
            ```

            实例：递归遍历目录结构和树状展现

                ```java
                    public static void main(String[] args){
                            //使用递归算法打印目录树
                            File f = new File("C:\\M-mind\\x-mind");
                            printFile(f,0);
                            
                        }
                        static void printFile(File file,int level){
                            for(int i=0;i<level;i++){
                                System.out.print("-");
                            }
                            System.out.println(file.getName());//打印文件名字
                            if(file.isDirectory()){//判断是否是目录
                                File[] files = file.listFiles();//返回目录下面所有的子目录和子文件
                                for(File temp:files){
                                    printFile(temp,level+1);
                                }
                            }
                        }
                ```

    **File类(硅谷)
        1：File类的使用：
            一：
                1：File类是一个对象，代表一个文件或一个文件目录(俗称：文件夹)
                2：File类声明在java.io包下
                3：File类中设计到关于文件或文件目录的创建，删除，重命名，修改时间，文件大小等方法
                    并未涉及到写入或读取文件内容的操作。如果需要读取或写入文件内容，必须使用io流来完成
                4：后续File类的对象常会作为参数传递到流的构造器中，指明读取或写入的"终点"
            二：
                1：相对路径：相较于某个路径下，指明的路径
                2：绝对路径：包含盘符在内的文件或文件目录的路径

            三：路径分隔符
                windows:\\
                unix:/
            
            四：如何创建File类的实例
                File(String filePath)
                File(String parentPath,String childPath)
                File(File parentPath,String childPath)
                用法：
                    实例：

                    ```java
                        //构造器1
                        File file = new File("hello.txt");//相对路径，相对于当前的module
                        File file1 = new File("C:\\java\\myproject\\dayone\\src\\a.txt");
                        //构造器2
                        File file3 = new File("C:\\java\\myproject","dayone\\src");
                        //构造器3
                        File file4 = new File(file3,"a.txt");
                    ```
        
        2：File类的使用：常用方法
            File类的获取功能：

                ```java
                    /*
                    public String getAbsolutePath()：获取绝对路径
                    public String getPath()  获取路径
                    public String getName()  获取名称
                    public String getParent() 获取上一层文件目录路径。如无，返回null
                    public Long Length()  获取文件长度(即：字节数)。不能获取目录的长度
                    public Long LastModified()  获取最后一次的修改时间，毫秒值
                    如下的两个方法适用于文件目录
                    public String[] list()  获取指定目录下的所有文件或者文件目录的名称数组
                    public File[] listFiles() 获取指定目录下的所有文件或者文件目录的File数组
                    */
                    具体举例：
                    public static void Test1(){
                        //构造器1
                        File file = new File("a.txt");//相对路径，相对于当前的module
                        File file1 = new File("C:\\java\\myproject\\dayone\\src\\a.txt");
                        System.out.println(file.getAbsoluteFile());
                        System.out.println(file.getPath());
                        System.out.println(file.getName());
                        System.out.println(file1.getParent());
                        System.out.println();
                    }
                    public static void test2(){
                        File file = new File("C:\\M-md学习\\wenjian");
                        String[] list = file.list();
                        for(String e:list){
                            System.out.println(e);
                        }
                        System.out.println();

                        File[] listfile = file.listFiles();
                        for(File e:listfile){
                            System.out.println(e);
                        }
                    }
                ```

            File类的重命名功能

                ```java
                    /*
                    public boolean renameTo(File dest)：把文件重命名为指定的文件路径
                        比如：file1.renameTo(file2)为例
                            要保证返回true即重命名到指定文件路径成功，需要file1在硬盘中是存在的，且file2不能在硬盘中存在
                    */
                    public  static void test3(){
                        File file1 = new File("a.txt");
                        System.out.println(file1.getAbsoluteFile());
                        File file2 = new File("C:\\io\\hi.txt");
                        boolean b = file1.renameTo(file2);
                        System.out.println(b);
                    }
                ```
            
            File类的判断功能

                ```java
                    /*
                    public boolean isDirectory() 判断是否是文件目录(不存在也会返回false)
                    public boolean isFile()  判断是否是文件(不存在也会返回false)
                    public boolean exists()  判断是否存在
                    public boolean canRead()  判断是否可读
                    public boolean canWrite()  判断是否可写
                    public boolean isHidden()  判断是否隐藏
                    */
                    public static void test4(){
                        File file6 = new File("a.txt");
                        System.out.println(file6.isFile());
                    }
                ```
            
            File类的创建功能

                ```java
                    /*
                    创建硬盘中对应的文件或文件目录
                    public boolean createNewFile()  创建文件。若文件存在，则不创建，返回false
                    public boolean mkdir()  创建文件目录。如果此文件目录存在，就不创建了。如果此文件目录的上层不存在，也不创建
                    public boolean mkdirs() 创建文件目录。如果上层目录不存在，一并创建

                    删除磁盘中的文件或文件目录
                    public boolean delete()  删除文件或者文件夹
                        删除注意事项：java中的删除不走回收站
                    */
                    public static void test6() throws IOException {
                        File file = new File("hello.txt");
                        if(!file.exists()){
                            file.createNewFile();
                            System.out.println("创建成功");
                        }
                    }
                ```

    
    **IO流
        java IO原理
            1：I/O是Input/Output的缩写，I/O技术是非常使用的技术，用于处理设备之间的数据传输。如读/写文件，网络通讯等
            2：java程序中，对于数据的输入/输出操作以"流(stream)"的方式进行
            3：java.io包下提供了各种"流"类和接口，用以获取不同的类的数据，并通过标准的方法输入或输出数据
        
        流的分类
            1：按操作数据单位不同分为：字节流(8 bit)，字符流(16 bit)  (字符流专门处理字符的)(字节流可以处理视频或图片的处理)
            2：按数据流的流向不同分为：输入流和输出流
            3：按流的角色的不同分为：节点流，处理流

        流的体系
            抽象基类            节点流(或文件流)                                    缓冲流(处理流的一种)
            InputStream         FileInputStream (read(byte[] buffer))             BufferedInputStream (read(byte[] buffer))         
            OutputStream        FileOutputStream (write(byte[] buffer,0,len))     BufferedOutputStream(write(byte[] buffer,0,len)/flush())
            Reader              FileReader (read(char[] cbuf))                    BufferedReader (read(char[] cubf) /readline())
            Writer              FileWriter (write(char[] cbuf,0,len))             BufferedWriter (write(char[] cbuf,0,len) / flush())

            字节流或文件流的应用：
                用法：从硬盘中读取数据
                    事例：read一个一个的读的事例

                    ```java
                        //如果将异常throw出去，可能会导致执行中断
                        public static void  test(){
                            FileReader fr = null;
                            try {
                                //1实例化File类的对象，指名要操作的文件
                                File file = new File("dayone\\hello.txt");//若此方法在main中调用，则当前相对路径相对于当前工程
                                //2提供具体的流
                                fr = new FileReader(file);
                                //3数据的读入
                                //read()：返回读入的一个字符。如果达到文本末尾，即没有数据，返回-1
                                int data = fr.read();
                                while (data !=-1){
                                    System.out.print((char)data);
                                    data = fr.read();
                                }
                            }catch (Exception e) {
                                e.printStackTrace();
                            }finally {
                                try {
                                    if(fr !=null)
                                    fr.close();
                                } catch (IOException e) {
                                    e.printStackTrace();
                                }
                            }
                        }
                    ```

                    事例：read按照数组定义的个数一起读

                    ```java
                        public static void test2(){
                            FileReader fr = null;
                            try{
                                //1.File类的实例化
                                File file = new File("dayone\\hello.txt");
                                //2.FileReader流的实例化
                                fr = new FileReader(file);
                                //3.读入的操作
                                //read(char[] cbuf)：返回每次读入cbuf数组中的字符的个数。如果达到文件末尾，返回-1
                                char[] cbuf = new char[5];
                                int len ; //记录每次读入到cbuf数组中的字符的个数
                                while ((len = fr.read(cbuf)) !=-1){ 
                                    for (int i=0;i<len;i++){
                                        System.out.print(cbuf[i]);
                                    }
                                }
                            }catch (Exception e){
                                e.printStackTrace();
                            }finally {
                                //4.资源的关闭
                                try {
                                    fr.close();
                                } catch (IOException e) {
                                    e.printStackTrace();
                                }
                            }
                        }
                    ```

                用法：从内存中写出数据到硬盘的文件里

                    ```java
                        /*
                        说明：
                            1：输出操作，对应的File可以不存在的，并不会报异常
                                File对应的硬盘中的文件如果不存在，在输出的过程中，会自动创建文件
                                File对应的硬盘中的文件如果存在：
                                        如果流使用的构造器：FileWriter(file,false)/FileWriter(file)：对原有文件的覆盖
                                        如果流使用的构造器：FileWriter(file,true)：不会对原有文件覆盖，而是在原有文件基础上追加内容
                        */
                        public static void test3() throws IOException {
                            //1；提供File类的对象，指明写出到的文件
                            File file = new File("dayone\\hello1.txt");
                            //2：提供FileWriter的对象，用于数据的写出
                            FileWriter fw = new FileWriter(file);
                            //3：写出的操作
                            fw.write("我有一个梦想");
                            fw.write("我们都有一个梦想");
                            //4：流资源的关闭
                            fw.close();
                        }
                    ```


                用法：读取硬盘中文件的数据，写入到硬盘中另外一个文件(文件的复制)

                    ```java
                        public static void test6()  {
                            FileReader fr = null;
                            FileWriter fw = null;
                            try {
                                //1创建File类对象，指明读入和写出的文件
                                File srcFile = new File("dayone\\hello1.txt");
                                File destFile = new File("hello2.txt");
                                //2.创建输入流和输出流的对象
                                fr = new FileReader(srcFile);
                                fw = new FileWriter(destFile);
                                //3.数据的读入和写出操作
                                char[] cbuf = new char[5];
                                int len;//记录每次读入到cbuf数组中的字符的个数
                                while ((len = fr.read(cbuf)) != -1){
                                    //每次写入len个字符
                                    fw.write(cbuf,0,len);
                                }
                            }catch (Exception e){
                                e.printStackTrace();

                            }finally {
                                //4.关闭资源流
                                try {
                                    fr.close();
                                    fw.close();
                                } catch (IOException e) {
                                    e.printStackTrace();
                                }
                            }
                        }
                    ```


                用法：实现对图片的复制

                    ```java
                        public static void test8(){
                            FileInputStream fr = null;
                            FileOutputStream fw = null;
                            try {
                                //1创建File类对象，指明读入和写出的文件
                                File srcfile = new File("dayone\\xiaozhang.jpg");
                                File destfile = new File("dayone\\xiaozhang1.jpg");
                                //创建节点流的对象
                                fr = new FileInputStream(srcfile);
                                fw = new FileOutputStream(destfile);

                                int len;
                                byte[] cubf = new byte[5];
                                while ((len = fr.read(cubf))!=-1){
                                    fw.write(cubf,0,len);
                                }
                            }catch (Exception e){
                                e.printStackTrace();

                            }finally {
                                try {
                                    fr.close();
                                    fw.close();
                                } catch (IOException e) {
                                    e.printStackTrace();
                                }
                            }
                    ```

            处理流之一：缓冲流的使用
                1：缓冲流：
                    BufferedInputStream
                    BufferedOutputStream
                    BufferedReader
                    BufferedWriter
                2：作用：提高路的读取，写入的速度
                     提高读写速度的原因是：内部提供了一个缓冲区

                3：处理流，就是"套接"在已有的流的基础上
                    事例：使用BufferedInputStream，BufferedOutputStream实现视频图片的复制

                    ```java
                        public static void BufferedStreamTest(){
                            BufferedInputStream bis = null;
                            BufferedOutputStream bos = null;
                            try {
                                //1：造文件
                                File srcFile = new File("dayone\\xiaozhang.jpg");
                                File desFile = new File("dayone\\xiaozhang2.jpg");
                                //2：造流
                                //  2-1：造节点流
                                FileInputStream fis = new FileInputStream(srcFile);
                                FileOutputStream fos = new FileOutputStream(desFile);

                                //  2-2：造缓冲流
                                bis = new BufferedInputStream(fis);
                                bos = new BufferedOutputStream(fos);

                                //复制的细节：读取，写入
                                byte[] buffer = new byte[10];
                                int len;
                                while ((len = bis.read(buffer))!=-1){
                                    bos.write(buffer,0,len);
                                }
                            }catch (IOException e){
                                e.getMessage();
                            }finally {
                                //资源关闭
                                //要求1：先关闭外层的流，再关闭内层的流
                                //说明：关闭外层流的同时，内层流也会自动的进行关闭。关于内存流的关闭，我们可以省略
                                try {
                                    if(bos != null){
                                        bos.close();
                                    }
                                    if (bis !=null){
                                        bis.close();
                                    }
                                } catch (IOException e) {
                                    e.printStackTrace();
                                }
                            }
                        }
                    ```

                    事例：使用BufferedReader，BufferedWriter实现文本文件的复制

                    ```java
                        public static void Bufferedtest(){
                            BufferedReader br = null;
                            BufferedWriter bw = null;
                            try{
                                br = new BufferedReader(new FileReader(new File("dayone\\hello.txt")));
                                bw = new BufferedWriter(new FileWriter(new File("dayone\\hello5.txt")));
                                    //读写操作
                                    //char[] cbuf = new char[1024];
                                    ////方式一：使用char[]数组
                                    //int len;
                                    //while ((len = br.read(cbuf)) !=-1){
                                    //    bw.write(cbuf,0,len);
                                    //    bw.flush(); //作用：强制刷新缓存区，将读取的数据从内存中直接全部倒出来，不在等待缓存区满了。
                                    //}
                                //方式二：使用String
                                String data;
                                while((data = br.readLine()) !=null){  //readLine();读取方式：一行一行的读取，默认不换行
                                    bw.write(data);
                                    bw.newLine(); //换行
                                }
                            }catch(IOException e){

                            }finally {
                                //关闭资源
                                try {
                                    if(br !=null){
                                        br.close();
                                    }
                                    if(bw !=null){
                                        bw.close();
                                    }
                                } catch (IOException e) {
                                    e.printStackTrace();
                                }
                            }
                        }
                    ```
            

            处理流之二：转换流的使用
                1：转换流：属于字符流
                    InputStreamReader：将一个字节的输入流转换成字符的输入流   
                    OutputStreamWriter：将一个字符的输出流转换为字节的输出流
                2：作用：提供了字节流与字符流之间的转换
                3：过程：解码：字节或字节数组 --->字符数组，字符串
                        编码：字符数组或字符串 --->字节，字节数组

                    事例：在内存中通过转换流输出文本文件内容

                    ```java
                        public static void test7(){
                            InputStreamReader isr = null;
                            try{
                                //造文件，造流
                                // File file1 = new File("dayone\\hello.txt");
                                FileInputStream fis = new FileInputStream("dayone\\hello.txt");
                                isr = new InputStreamReader(fis,"UTF-8");
                                //读写过程
                                char[] cbuf = new char[20];
                                int len;
                                while ((len = isr.read(cbuf)) !=-1){
                                    String str = new String(cbuf,0,len);
                                    //若这里用String的方式操作字节的话，可能会出现乱码；原因，一个中文由很多字节组成
                                    System.out.println(str);
                                }
                            }catch (IOException e){

                            }finally {
                                try {
                                    isr.close();

                                } catch (IOException e) {
                                    e.printStackTrace();
                                }
                            }
                        }
                    ```
                    流的运行机制：isr.read(),在读取数据之前，上面代码初始化new的时候添加到堆中，建立流类似于管道，当调用read方法的时候，才开始有数据传过来，才调用堆中new出来流的方法

                    事例：用转换流进行文件的复制

                    ```java
                        public static void test7(){
                            InputStreamReader isr = null;
                            OutputStreamWriter osw = null;
                            try{
                                //造文件，造流
                                File file1 = new File("dayone\\hello.txt");
                                File file2 = new File("dayone\\hello66.txt");

                                FileInputStream fis = new FileInputStream(file1);
                                FileOutputStream fos = new FileOutputStream(file2);

                                isr = new InputStreamReader(fis,"UTF-8");//使用UTF-8的方式读取
                                osw = new OutputStreamWriter(fos,"UTF-8");//使用UTF-8的方式写入
                                //读写过程
                                char[] cbuf = new char[20];
                                int len;
                                while ((len = isr.read(cbuf)) !=-1){
                                    osw.write(cbuf,0,len);
                                }
                            }catch (IOException e){

                            }finally {
                                try {
                                    isr.close();
                                    osw.close();
                                } catch (IOException e) {
                                    e.printStackTrace();
                                }
                            }
                        }
                    ```


            其他流的使用
                标准的输入输出流
                    事例：从键盘中输入字符串，要求将读取到的整行字符串转成大写输出。然后继续进行输入操作。直至当输入"e"或者"exit"时，退出程序
                        思路：使用键盘事件，System.in实现。System.in--->转换流--->BufferedReader的readLine()

                    ```java
                        /*
                        1：标准的输入流，输出流
                            1.1：
                            System.in：标准的输入流，默认从键盘输入
                            System.out：标准的输出流，默认的输出是控制台
                            1.2：
                            System类的setIn(InputStream is)/setOut(PrintStream ps)方式重新制定输入和输出的流
                        */
                        public static void main(String[] args) {
                            BufferedReader br = null;
                            try{
                                InputStream put = System.in;//输入的时候是字节流，类型是InputStream，是字节流
                                InputStreamReader isr = new InputStreamReader(put);//要使用BufferedReader，就必须通过转换流转成字符流

                                br = new BufferedReader(isr);
                                while (true){
                                    System.out.println("请输入字符串:");
                                    String data = br.readLine();
                                    if("e".equalsIgnoreCase(data) || "exit".equalsIgnoreCase(data)){
                                        System.out.println("程序运行结束");
                                        break;
                                    }
                                    String upperCase = data.toUpperCase();
                                    System.out.println(upperCase);
                                }
                            }catch (IOException e){

                            }finally {
                                try {
                                    br.close();
                                } catch (IOException e) {
                                    e.printStackTrace();
                                }
                            }
                        }
                    ```


            处理流之六：对象流
                对象流：ObjectInputStream，ObjectOutputStream
                作用：用于存储和读取基本数据类型数据或对象的处理流。它的强大之处就是可以把java中的对象写入到数据源中，也能把对象从数据源中还原回来。

                要想一个java对象是可序列化的，需要满足相应的要求。比如Person.java
                    1:需要实现接口：Serializable
                    2：当前类提供一个全局常量：SerialVersionUID
                    3：除了当前Person类需要实现Serializable接口之外，还必须保证其内部所有属性也必须是可序列化的(默认情况下，基本数据类型可序列化)
                    补充：ObjectOutputStream和ObjectInputStream不能序列化static和transient修饰的成员；如用这两个关键字修饰属性或对象，则序列化的时候，不能将传入的值序列化，即为初始值；(用static修饰属性的不属于对象，所以不能被序列化；而自己不想让对象某些属性序列化则可通过transiend修饰成员即可)

                        实例：满足对象序列化的java对象的实例

                        ```java
                            public class Person implements Serializable { //需要实现接口Serializable
                                public final long serivalVersionUID = 12839287345L; //全局常量SerialVersionUID
                                private String name;
                                private static int age;
                                public Person(String name,int age){
                                    this.name = name;
                                    this.age = age;
                                }
                            }
                        ```

                序列化的概念：将数据从内从中保存到数据源中，这个过程我们称之为序列化
                    序列化：用ObjectOutputStream类保存基本数据类型或对象的机制
                    反序列化：用ObjectInputStream类读取基本数据类型或对象的机制

                    注意点：ObjectInputStream,ObjectOutputStream不能序列化Static和transient修饰的成员变量

                对象的序列化机制：
                    允许把内存中的java对象转换成平台无关的二进制流，从而允许把这种二进制流持久地保存在磁盘上，或通过网络将这种二进制流传出到另一个网络节点。当其它程序获取了这种二进制流，就可以恢复成原来的java对象

                    事例：

                    ```java
                        public static void testWrite(){
                            ObjectOutputStream oos = null;
                            try{
                                oos = new ObjectOutputStream(new FileOutputStream("Object.dat"));
                                oos.writeObject("今天天气真好"); //先写入的，先读取，通过flush()来区分先写入后写入的顺序
                                oos.flush();
                                oos.writeObject(new Person("石惠",12));
                                oos.flush();
                                oos.writeObject(new Person("张学良",23));
                                oos.flush();
                            }catch (IOException e){

                            }finally {
                                try {
                                    oos.close();
                                } catch (IOException e) {
                                    e.printStackTrace();
                                }
                            }
                        }
                        public static void testRead(){
                            ObjectInputStream ois = null;
                            try{
                                ois = new ObjectInputStream(new FileInputStream("Object.dat"));
                                String str = (String)ois.readObject();//先写进的限度，字符串先写进，先读取字符串，否则报错
                                Person p = (Person)ois.readObject();//先写进的先读取，后写进的后读取，通过flush()来区分谁先写进的
                                Person p1 = (Person)ois.readObject();
                                System.out.println(str);
                                System.out.println(p);
                                System.out.println(p1);

                            }catch (IOException | ClassNotFoundException e){

                            }finally {
                                try {
                                    ois.close();
                                } catch (IOException e) {
                                    e.printStackTrace();
                                }
                            }
                        }
                    ```


            处理流之七：随机存储文件流
                RandomAccessFile的使用
                    1：RandomAccessFile直接继承与java.lang.Object类，实现了DataInput和DataOutput接口
                    2：RandomAccessFile即可以作为一个输入流，又可以作为一个输出流
                    3：如果RandomAccessFile作为输出流时，写出到的文件如果不存在，则在执行过程中自动创建；如果写出到的文件存在，则会对原有文件内容进行覆盖(默认情况下，从头覆盖，写多少覆盖多少，没覆盖的部分保持原样)

                RandomAccessFile对象包含一个记录指针
                    作用：用以表示当前读写处的位置，从当前位置往后读写。RandomAccessFile类对象可以自由的移动记录指针
                        >long getFilePointer()：获取文件记录指针的当前位置
                        >void seek(long pos)：将文件记录指针定位到pos位置

                RandomAccessFile类
                    创建RandomAccessFile类实例需要制定一个mode参数(构造器 public RandomAccessFile(String name,String mode))，该参数指定RandomAccessFile的访问模式
                        >r：以只读方式打开
                        >rw：打开以便读取和写入
                        >rwd：打开以便读取和写入：同步文件内容的更新
                        >rws：打开以便读取和写入；同步文件内容和元数据的更新
                        说明：
                            如果模式为只读r。则不会创建文件，而是会去读取一个已经存在的文件，如果读取的文件不存在则出现异常。如果模式为rw读写，如果文件不存在则会去创建文件，如果存在则不会创建
                            rw模式，数据不会立即写到硬盘汇总；而rwd，数据会被立即写入硬盘。如果写数据过程发生异常，rwd模式中已被write的数据被保存到硬盘中，而rw则全部丢失

                        事例：使用随机存储文件流复制图片

                        ```java
                            public static void test1() {
                                RandomAccessFile raf1 = null;
                                RandomAccessFile raf2 = null;
                                try {
                                    //1
                                    raf1 = new RandomAccessFile(new File("one6\\xiaozhang.jpg"), "r");
                                    raf2 = new RandomAccessFile(new File("one6\\xiaozhang1.jpg"), "rw");
                                    //2
                                    byte[] buffer = new byte[1024];
                                    int len;
                                    while ((len = raf1.read(buffer)) != -1) {
                                        raf2.write(buffer, 0, len);
                                    }
                                } catch (IOException e) {

                                } finally {
                                    try {
                                        raf1.close();
                                        raf2.close();
                                    } catch (IOException e) {
                                        e.printStackTrace();
                                    }
                                }
                            }
                        ```


##网络编程
    **网络编程概述
        网络基础
            计算机网络：
                概念：把分布在不同地理区域的计算机与专门的外部设备用通讯线路互连成一个规模大，功能强的网络系统，从而使众多的计算机可以方便地互相传递信息，共享硬件，软件，数据信息等资源
            网络编程的目的：
                直接或间接地通过网络协议与其他计算机实现数据交换，进行通讯
            网络编程中有两个主要的问题
                >如果准确地定位网络上一台或多台主机；定位主机上的特定的应用
                >知道主机后如何可靠地高效的进行数据传输
            
    **网络通讯要素概述
        IP和端口号
            IP：
                1:概念：唯一的标识Internet上的计算机(通讯实体)
                2:在java中使用InetAddress类代表ip，类似File代表本地文件一样
                3:IP分类：IPv4和IPv4;万维网和局域网
                4:域名：www.baidu.com  www.mi.com 
                5:如何实例化InetAddress：两个方法：getByName(String host)、getLocalHost()
                                    实例化InetAddress之后的两个常用方法:getHostName() / getHostAddress()
                事例：关于getByName(),getLocalhost()方法的使用，以及实例化之后方法的使用

                ```java
                    public static void main(String[] args) {
                        try {
                            InetAddress inte1 = InetAddress.getByName("www.baidu.com");
                            System.out.println(inte1);
                            InetAddress inte2 = InetAddress.getByName("127.0.0.1");
                            System.out.println(inte2);
                            //获取本地ip
                            InetAddress inte3 = InetAddress.getLocalHost();
                            System.out.println(inte3);
                            //getHostName()获取域名
                            System.out.println(inte3.getHostName());
                            //getHostAddress()获取主机地址
                            System.out.println(inte3.getHostAddress());
                        } catch (UnknownHostException e) {
                            e.printStackTrace();
                        }
                    }
                ```

        网络通讯协议
            计算机网络中实现通讯必须有一些约定，即通讯协议，对速率，传输代码，代码结构，传输控制步骤，出错控制等制定标准。

    **通讯要素1：IP和端口号
    **通讯要素2：网络协议
    **TCP网络编程
    **UDP网络编程
    **URL编程






                

            
        
            
            


