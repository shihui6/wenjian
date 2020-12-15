时间2020/12/15
###JDBC
    ***数据的持久化
    


    1.获取数据库的连接方式一
        事例：通过mysql驱动，连接数据库，即通过驱动直接操作
            注意点：需要导入mysql的驱动


        ```java
            public void testConnection1() throws SQLException {
                    Driver driver = new com.mysql.jdbc.Driver();
                    //jdbc:mysql：协议
                    //localhost:ip地址
                    //3306；默认mysql的端口号
                    //test:test数据库
                    String url = "jdbc:mysql://localhost:3306/test";
                    //将用户名和密码封装在Properties中
                    Properties info = new Properties();
                    info.setProperty("user","root");
                    info.setProperty("password","wossh1423875545");

                    Connection con = driver.connect(url,info);
                    System.out.println(con);
                }
        ```

    2.获取数据库的连接方式二
        事例：

        ```java
            //方式二：是对方式一的迭代：在如下的程序中不出现第三方的api，使得程序具有更好的可移植性
                @Test
                public void testConnection2() throws Exception {
                    //1.获取Driver实现类对象，使用反射
                    Class claszz = Class.forName("com.mysql.jdbc.Driver");
                    Driver driver = (Driver) claszz.newInstance();
                    //2。提供要连接的数据库
                    String url = "jdbc:mysql://localhost:3306/test";
                    //提供连接需要的用户名和密码
                    Properties info = new Properties();
                    info.setProperty("user","root");
                    info.setProperty("password","wossh1423875545");
                    //获取连接
                    Connection con = driver.connect(url,info);
                    System.out.println(con);
                }

                //方式三：使用DriverManager替换Driver
                @Test
                public void testConnection() throws Exception{
                    //1.获取Driver实现类对象
                    Class claszz = Class.forName("com.mysql.jdbc.Driver");
                    Driver driver = (Driver) claszz.newInstance();
                    //2.提供另外三个连接的基本信息
                    String url = "jdbc:mysql://localhost:3306/test";
                    String user = "root";
                    String password = "wossh1423875545";
                    //注册驱动
                    DriverManager.registerDriver(driver);
                    //获取连接
                    Connection conn = DriverManager.getConnection(url, user, password);
                    System.out.println(conn);
                }

                //方式四：可以只是加载驱动，不用显示的注册驱动
                @Test
                public void testConnection4() throws Exception{
                    //1.提供另外三个连接的基本信息
                    String url = "jdbc:mysql://localhost:3306/test";
                    String user = "root";
                    String password = "wossh1423875545";
                    //2.获取Driver实现类对象
                    Class.forName("com.mysql.jdbc.Driver");
                    //相较于方式三，可以省略如下的操作，因为下面的操作，Driver内部已经做好了
            //        Driver driver = (Driver) claszz.newInstance();
            //        //注册驱动
            //        DriverManager.registerDriver(driver);
                    //获取连接
                    Connection conn = DriverManager.getConnection(url, user, password);
                    System.out.println(conn);
                }

                //方式五：将数据库连接需要的4个基本信息声明在配置文件中，通过读取配置文件的方式，获取连接

                /**
                * 此种方式的好处：
                *      1.实现了数据与代码的分离，实现了解耦
                *      2.如果需要修改配置文件信息，可以避免重新打包
                */
                @Test
                public void testConnection5() throws Exception {
                    //1.读取配置文件中4个基本信息
                    InputStream is = ConnectionTest.class.getClassLoader().getResourceAsStream("jdbc.properties");
                    Properties pros = new Properties();
                    pros.load(is);
                    String user = pros.getProperty("user");
                    String password = pros.getProperty("password");
                    String url = pros.getProperty("url");
                    String driverClass = pros.getProperty("driverClass");
                    //2.加载驱动
                    Class.forName(driverClass);
                    //3.获取连接
                    Connection conn = DriverManager.getConnection(url, user, password);
                    System.out.println(conn);
                }
        ```