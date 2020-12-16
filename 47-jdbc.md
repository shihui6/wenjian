时间2020/12/15
###JDBC
    ***数据的持久化
    


        1.获取数据库的连接对象的方式
            事例：通过mysql驱动，连接数据库，即通过驱动直接操作
                注意点：需要导入mysql的驱动


            ```java
                //方式一
                public void testConnection1() throws SQLException {
                    //获取Driver的实现类对象
                    /**
                    * Driver是java sql也就是sun公司定义的接口，然后下面每个公司需要定义驱动实现这个接口
                    * 即new com.mysql.jdbc.Driver()是实现了Driver接口的驱动
                    */
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
                        //Driver driver = (Driver) claszz.newInstance();
                        //注册驱动
                        //DriverManager.registerDriver(driver);
                        //获取连接
                        Connection conn = DriverManager.getConnection(url, user, password);
                        System.out.println(conn);
                    }


                //只要掌握这种方式即可
                //方式五：将数据库连接需要的4个基本信息声明在配置文件中，通过读取配置文件的方式，获取连接
                    /**
                    * 此种方式的好处：
                    *      1.实现了数据与代码的分离，实现了解耦
                    *      2.如果需要修改配置文件信息，可以避免重新打包
                    */
                    @Test
                    public void testConnection5() throws Exception {
                        //1.读取配置文件中4个基本信息
                        InputStream is = ClassLoader.getSystemClassLoader().getResourceAsStream("jdbc.properties");
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

        2-获取对象操作和访问数据库
            概念：数据库连接用于向数据库服务发送命令和Sql语句，并接受数据库服务器返回的结果。其实一个数据库连接就是一个Socket连接
            在java.sql包中有3个接口分别定义了对数据库的调用的不同的方式:
                    >statement:用于执行静态Sql语句并返回它所生成结果的对象
                    >PrepatedStatement:Sql语句被预编译并存储在对象中，可以使用此对象多次高效地执行该语句
                    >CallableStatement：用于执行Sql存储过程

            1.使用PrepatedStatement实现对数据表的添加操作
                事例：

                    ```java
                        public void preparedTest() throws Exception {
                                //1.读取配置文件中4个基本信息
                                InputStream is = ClassLoader.getSystemClassLoader().getResourceAsStream("jdbc.properties");
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
                                //4.预编译sql语句，返回PreparedStatement的实例
                                String sql = "insert into customers(name,email,birth)values(?,?,?)"; //?：占位符
                                PreparedStatement ps = conn.prepareStatement(sql);
                                //5.填充占位符
                                ps.setObject(1,"法海");
                                ps.setObject(2,"123456@qq.com");
                                ps.setObject(3, "2020-09-09");
                                //6.执行操作
                                ps.execute();
                                //7.关闭资源
                                ps.close();
                                conn.close();
                            }
                    ```

            2.使用PrepatedStatement实现对数据表的修改操作
                1.将读取配置文件信息和关掉资源的操作封装成方法

                    ```java
                        public class TCommon {
                                public static Connection setConnect() throws Exception {
                                    //1.读取配置文件中4个基本信息
                                    InputStream is = ClassLoader.getSystemClassLoader().getResourceAsStream("jdbc.properties");
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
                                    return conn;
                                }

                                public static void shutDown(Connection conn, PreparedStatement ps) throws Exception {
                                    conn.close();
                                    ps.close();
                                }
                        }
                    ```

                2.然后在其他方法里面引用

                    ```java
                        public void Common() throws Exception {
                                //1.读取配置文件
                                Connection conn = TCommon.setConnect();
                                //2.预编译
                                String sql = "update customers set name=? where id = ?";
                                PreparedStatement ps = conn.prepareStatement(sql);
                                //3.填充占位符
                                ps.setObject(1,"莫扎特");
                                ps.setObject(2,3);
                                //4.执行操作
                                ps.execute();
                                //5.关闭资源
                                TCommon.shutDown(conn,ps);
                            }
                    ```

            3.写一个通用的增删改操作

                    ```java
                        public  void update(String sql,Object ...args) throws Exception {
                                //1.读取配置文件
                                Connection conn = TCommon.setConnect();
                                //2.预编译
                                PreparedStatement ps = conn.prepareStatement(sql);
                                //3.填充占位符
                                for (int i=0;i<args.length;i++){
                                    ps.setObject(i+1,args[i]);
                                }
                                //4.执行操作
                                ps.execute();
                                //5.关闭资源
                                TCommon.shutDown(conn,ps);
                            }
                    ```


