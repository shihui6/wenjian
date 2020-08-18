##线程
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


##java集合
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



        

            


    
            


##泛型
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
    
                
            
            


