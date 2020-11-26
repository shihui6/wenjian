###Tomcat
    ***介绍
        web资源的分类
            web资源按实现的技术和呈现的效果的不同，又分为静态资源和动态资源
                静态资源：html,css,js,txt,mp4视频，jpg图片
                动态资源：jsp页面，Servlet程序、
        
        常用的web服务器
            Tomcat：由Apache组织提供的一种web服务器，提供对jsp和servlet的支持。它是一种轻量级的javaweb容器(服务器)，也是当前应用最广泛的javaweb服务器(免费)
            Resin：是一个非常流行的服务器，对servlet和jsp提供了良好的支持，性能也比较优良，resin自身采用了java语言开发(收费，应用比较多)


        Tomcat服务器和servlet版本的需要一一对应的关系，如：Tomcat服务器版本高于servlet版本互相不支持
            Servlet程序从2.5版本是现在市面使用最多的版本(xml配置)
            到了servlet3.0之后。就是注解版本的servlet使用

    ***Tomcat的使用
        *安装
            找到你需要的Tomcat版本对应的zip压缩包，解压到需要安装的目录即可
        
        *目录介绍
            bin     专门用来存放Tomcat服务器的可执行程序
            conf    专门用来存放Tomcat服务器的配置文件
            lib     专门用来存放Tomcat服务器的jar包
            logs    专门用来存放Tomcat服务器运行时输出的日记信息
            temp    专门用来存放Tomcat运行时产生的临时数据
            webapps 专门用来存放部署的Web工程
            work    是Tomcat工作时的目录，用来存放Tomcat运行时jsp翻译为Servlet的源码，和session钝化(将对象序列化写到磁盘上)的目录

        
        *启动Tomcat服务器
            方式一：
                找到Tomcat目录下的bin目录下的startup.bat文件，双击，就可以启动Tomcat服务器
                如何测试Tomcat服务器启动是否成功？
                    打开浏览器，在浏览器地址栏中输入以下地址测试：http:localhost:8080
            
                常见的启动失败的情况有，双击startup.bat文件，就会出现一个小黑窗口一闪而过，这个时候，失败的原因基本上都是因为没有配置好JAVA_HOME环境变量(配置的是jdk的环境变量，只需要配置到jdk的安装目录即可。不需要带上bin目录)

            方式二：
                在tomcat的bin目录下命令行输入catalina run即可

        *Tomcat服务器的停止
            方式一：点击tomcat服务窗口关闭
            方式二：命令行ctrl+c
            方式三：找到tomcat的bin目录下的shutdown.bat双击，即可停止tomcat服务

        *修改Tomcat的端口号
            mysql默认的端口号是：3306
            Tomcat默认的端口号是：8080
            修改tomcat的步骤：
                找到Tomcat目录下的conf目录，找到server.xml配置，找到Connector标签，修改port属性值为你需要的端口号

            HTTP协议默认的端口号是80，若写80端口，浏览器会自动抹去，写和不写都一样

        *部署web工程到Tomcat中
            第一种方法：
                只需要把web工程的目录拷贝到tomcat的webapps目录下即可
                如何访问：如，需要访问部署在tomcat上的book工程，则服务地址+tomcat在服务器上的的端口号+book
                            http://localhost:8080/book/index.html(即访问的是tomcat上book工程)
            第二种方法：
                找到tomcat下的conf目录\Catalina\localhost\下，创建如下的配置文件,abc.xml写入下面的内容
                    <Context path="/abc" docBase="E:\book" />
                        Context表示一个工程上下文
                        path表示工程的访问路径:/abc
                        docBase表示你的工程目录在哪里(通过docBase映射到相关目录下)

                    第二种方式的访问路径如：http://localhost:80/abc(表示访问的是磁盘E:/book目录)

            第三种方式
                当我们在浏览器地址栏中输入访问地址如下
                    http://ip:port/ ===>没有工程名的时候，默认访问的是webapps->ROOT工程里面的内容
                当我们在浏览器中输入的访问地址如下
                    httP://ip:port/ ===>没有资源名，默认访问index.html页面


        *IDEA中如何创建动态web工程
            new->Module->Java Enterprise页，选择应用的服务器，一定要勾选Web Application;然后生成的web项目
                生成的web项目的目录介绍
                    src目录存放自己编写的java源代码
                    web目录专门用来存放web工程的资源文件；比如：html页面，css文件，js文件等等
                    web->WEB-INF目录是一个受服务器保护的目录，浏览器无法直接访问到此目录的内容
                    web->WEB-INF->web.xml它是整个动态web工程的配置部署描述文件可以在这里配置很多web工程组件
                            比如：Servlet程序，Filter过滤器，Listener监听器，Session超时......等等
                    web->WEB-INF->lib目录用来存放第三方的jar包(IDEA需要自己配置导入)
                            为什么需要配置导入的jar
                                因为复制粘贴进来的jar包，在项目里不可使用，必须将jar包添加到项目的类库中才可以
                            jar添加到类库的方式
                                方式一
                                    将lib里面的jar包导入到当前类库，点击右键Add as Library就可以将jar包添加到当前类库
                                方式二
                                    打开项目结构菜单操作界面，添加一个自己的类库File->Project Structure->Libraries->java
                                        然后选择项目里类库需要的jar包(前提要将这些需要添加的jar包复制到项目里，才可以选择)，点击确定
                                        然后选择这些jar生成的类库可以使用到的模块
                                        选择Artifacts选项，将类库，添加到打包部署中；操作Artifacts->fix即可

        *在idea中启动部署web模板


###Servlet技术
    ***Servlet
        *什么是Servlet
            1.Servlet是javaEE规范之一。规范就是接口
            2。Servlet是javaweb三大组件之一。三大组件分别是：Servlet程序，Filter过滤器，Listener监听器
            3.Servlet是运行在服务器上的一个java小程序，它可以接受客户端发送过来的请求，并响应数据给客户端


        *手动实现Servlet程序
            1.编写一个类去实现Servlet接口
            2。实现service方法，处理请求，并相应数据
            3.到web.xml中去配置Servlet程序的访问地址
                配置文件

                    ```xml
                        <?xml version="1.0" encoding="UTF-8"?>
                        <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
                                xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                                xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
                                version="4.0">

                            <!--servlet标签给Tomcat配置Servlet程序-->
                            <servlet>
                                <!--servlet-name标签 Servlet程序起一个别名(一般是类名)-->
                                <servlet-name>Helloservlet</servlet-name>
                                <!--servlet-class是Servlet程序的全类名  -->
                                <servlet-class>com.atguigu.servlet.Helloservlet</servlet-class>
                            </servlet>
                            <servlet-mapping>
                                <!--servlet-name标签的作用是告诉服务器，我当前配置的地址给哪个Servlet程序使用-->
                                <servlet-name>Helloservlet</servlet-name>
                                <!--url-pattern标签配置访问地址
                                    /斜杠在服务器解析的时候，表示地址为：http://ip:port/工程路径(就是在tomcat里面的那个访问路径)
                                    /hello 表示地址为：http://ip:port/工程名/hello(该地址访问对应的helloservlet这个类)
                                -->
                                <url-pattern>/hello</url-pattern>
                            </servlet-mapping>
                        </web-app>
                    ```
                说明：为什么输入hello之后就可以访问到配置的Helloservlet的类
                    执行流程：
                        如：
                        当输入http://localhost:8088/06_servlet；访问的是8088端口下的06_servlet这个web项目
                        当输入http://localhost:8088/06_servlet/hello;当地址后添加/hello的时候，程序会首先到配置文件web.xml里面找有没有/hello这个路径，若有就找该路径对应的<serlet-name>标签对应的类名，通过类名和标签<servlet-class>找到对应要访问的类的位置

        
        *Servlet的生命周期
            1.执行Servlet构造方法
            2.执行init初始化方法
                第一，二步，是在第一次访问的时候创建Servlet程序会调用
            3.执行service方法
                第三步，每次访问都会调用
            4.执行destroy销毁方法
                第四步，在web工程停止的时候调用

        *实现Servlet程序
            方式一
                通过继承HttpServlet实现Servlet程序
                    一般在实际项目开发中，都是使用继承HttpServlet类的方式去实现Servlet程序
                        1.编写一个类去继承HttpServlet类
                        2.根据业务需要重写doGet或doPost方法
                        3.到web.xml中配置Servlet程序的访问地址

            方式二
                通过idea快速生成
                    步骤：在package包下，new->Create new Servlet，然后就可以快速生成相应的配置和class类了

        *ServletConfig类
            作用：
                1.可以获取servlet程序的别名servlet-name(配置文件xml的配置信息)的值
                2.获取初始化参数init-param(配置文件xml的配置信息)
                3.获取ServletContext对象

                事例：实现Servlet接口的时候生成的int方法形参类型是ServletConfig,里面写的

                    ```java
                        public void init(ServletConfig servletConfig) throws ServletException {
                        //1.可以获取servlet程序的别名servlet-name(在配置xml文件里)
                            System.out.println("Helloservlet程序别名"+servletConfig.getServletName());
                        //2.获取初始化参数init-param(在配置xml文件里)
                            System.out.println("初始化参数username的值"+servletConfig.getInitParameter("username"));
                        //3.获取ServletContext对象
                            System.out.println(servletConfig.getServletContext());
                        }
                    ```
        

            
            

