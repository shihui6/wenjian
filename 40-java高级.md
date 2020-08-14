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

                
                        
                
            
            


