时间2020/12/15
###JDBC
    ***数据的持久化

        1.获取数据库的连接对象的方式
            事例：通过mysql驱动，连接数据库，即通过驱动直接操作，该驱动是对sun公司定义规范的实现
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


                //只要掌握方式五这种方式即可
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
                    >statement:用于执行静态Sql语句并返回它所生成结果的对象（这个类基本不用）
                    >PrepatedStatement:Sql语句被预编译并存储在对象中，可以使用此对象多次高效地执行该语句
                    >CallableStatement：用于执行Sql存储过程

            1.使用PrepatedStatement实现对数据表的添加操作【所有的操作的原理都基于此事例拓展】
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

            3.通用的增删改操作

                    ```java   封装通用的增删改的方法
                        public  void update(String sql,Object ...args) throws Exception {
                                //1.读取配置文件
                                Connection conn = TCommon.setConnect();  //TCommon是自己封装的类
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
                    ```java  对上面封装的通用的增删改操作的使用
                        @Test
                            public void Testupdate() throws Exception {
                                String sql = "delete from customers where id = ?";
                                update(sql,3);
                            }
                    ```

时间2020/12/18

            4.查询操作
                注意点：java与sql对应数据类型转换表
                            java类型                    sql类型
                            boolean                     BIT
                            byte                        TINYINT
                            short                       SMALLINT
                            int                         INTEGER
                            long                        BIGINT
                            String                      char,varchar,longvarchar
                            byte array                  binary,var binary
                            java.sql.Date               Date
                            java.sql.Time               Time
                            java.sql.Timestamp          timestamp


                1.查询操作

                    ```java
                        public class SelectTest {
                            @Test
                            public void Selecttestv() throws Exception {
                                Connection conn = TCommon.setConnect(); //自己疯转的连接方法
                                String sql = "select id,name,email,birth from customers where id = ?";
                                PreparedStatement ps = conn.prepareStatement(sql);
                                //填充占位符
                                ps.setObject(1,1);
                                //执行查询语句，并返回结果集
                                ResultSet resultSet = ps.executeQuery();
                                //处理结果集
                                if(resultSet.next()){  //next():判断结果集中对应的行中的列是否有数据，如果有数据返回true，并指针下移；如果返回false，指针不会下移
                                    //获取当前这条数据的各个字段的值
                                    int id = resultSet.getInt(1);
                                    String name = resultSet.getString(2);
                                    String email = resultSet.getString(3);
                                    Date birth = resultSet.getDate(4);
                                    //用对象显示结果集
                                    Customer customer = new Customer(id, name, email, birth); //用自己写的对象处理结果集
                                    System.out.println(customer);
                                }
                                //关闭资源
                                TCommon.shutDownquan(conn,ps,resultSet);//自己封装的关闭资源的方法
                            }
                        }
                    ```

                2.通用的查询操作

                    ```java   针对同一个表的通用的查询操作,返回表中的一条记录
                        public class CustomerForQuery {
                            /**
                            * 针对customers表的通用的查询操作

                                针对于表的字段名与类的属性名不相同的情况：
                                    1.必须声明sql时，使用类的属性名来命名字段的别名
                                    2.使用ResultSetMetaData时，需要使用getColumnLabel()来替换getColumnName(),来获取列的别名
                                        说明：如果sql中没有给字段取别名，getColumnLabel()获取的就是列名
                            */
                            @Test  对通用表的查询操作的使用
                            public void testSelect() throws Exception {
                                String sql = "select id,name,birth,email from customers where id =?";
                                Customer customer = queryForCustomers(sql, 12);
                                System.out.println(customer);

                            }
                            //封装的通用表的查询操作
                            public Customer queryForCustomers(String sql,Object ...args) throws Exception {
                                Connection conn = TCommon.setConnect();
                                PreparedStatement ps = conn.prepareStatement(sql);
                                //填充占位符
                                for(int i=0;i<args.length;i++){
                                    ps.setObject(i+1,args[i]);
                                }
                                //执行查询语句，并返回结果
                                ResultSet rs = ps.executeQuery();
                                //获取结果集的元数据:ResultSetMetaData(元数据的作用：修饰结果集的数据即元数据)
                                ResultSetMetaData rsmd = rs.getMetaData();
                                //通过ResultSetMetaData获取结果集中的列数
                                int columnCount = rsmd.getColumnCount();

                                if(rs.next()){
                                    Customer cust = new Customer();
                                    //处理结果集一行数据中的每一列
                                    for(int i=0;i<columnCount;i++){
                                        //获取列值
                                        Object columnValue = rs.getObject(i + 1);
                                        //获取每个列的列名
                                        String columnLabel = rsmd.getColumnLabel(i + 1);
                                        //给cust对象指定的columnName属性，赋值为columnValue；通过反射
                                        Field field = Customer.class.getDeclaredField(columnLabel);
                                        field.setAccessible(true);
                                        field.set(cust,columnValue);
                                    }
                                    return cust;
                                }
                                TCommon.shutDownquan(conn,ps,rs);
                                return null;
                            }
                        }
                    ```

                    ```java  针对不同的表(使用存储的类不一样)的通用的查询操作，返回表中的一条记录
                        public class PrepareadStatementQuery {
                            @Test
                            public void testGetInstance() throws Exception {
                                String sql = "select id,name,email from customers where id = ?";
                                Customer customer = getInstance(Customer.class, sql, 12);
                                System.out.println(customer);
                            }

                            public <T>T getInstance(Class<T> clazz,String sql,Object ...args) throws Exception {
                                Connection conn = TCommon.setConnect();
                                PreparedStatement ps = conn.prepareStatement(sql);
                                //填充占位符
                                for(int i=0;i<args.length;i++){
                                    ps.setObject(i+1,args[i]);
                                }
                                //执行查询语句，并返回结果
                                ResultSet rs = ps.executeQuery();
                                //获取结果集的元数据:ResultSetMetaData(元数据的作用：修饰结果集的数据即元数据)
                                ResultSetMetaData rsmd = rs.getMetaData();
                                //通过ResultSetMetaData获取结果集中的列数
                                int columnCount = rsmd.getColumnCount();

                                if(rs.next()){
                                    T t = clazz.newInstance();
                                    //处理结果集一行数据中的每一列
                                    for(int i=0;i<columnCount;i++){
                                        //获取列值
                                        Object columnValue = rs.getObject(i + 1);
                                        //获取每个列的列名
                                        String columnLabel = rsmd.getColumnLabel(i + 1);
                                        //给t对象指定的columnName属性，赋值为columnValue；通过反射
                                        Field field = clazz.getDeclaredField(columnLabel);
                                        field.setAccessible(true);
                                        field.set(t,columnValue);
                                    }
                                    return t;
                                }
                                TCommon.shutDownquan(conn,ps,rs);
                                return null;
                            }
                        }
                    ```
                    ```java 针对不同的表的通用的查询操作，返回表中的多条记录
                        @Test
                            public void getForlist() throws Exception {
                                String sql = "select id,name,email from customers where id < ?";
                                List<Customer> list = getForlist(Customer.class, sql, 12);
                                list.forEach(System.out::println);
                            }

                            public <T> List<T> getForlist(Class<T> clazz, String sql, Object ...args) throws Exception {
                                Connection conn = TCommon.setConnect();
                                PreparedStatement ps = conn.prepareStatement(sql);
                                //填充占位符
                                for(int i=0;i<args.length;i++){
                                    ps.setObject(i+1,args[i]);
                                }
                                //执行查询语句，并返回结果
                                ResultSet rs = ps.executeQuery();
                                //获取结果集的元数据:ResultSetMetaData(元数据的作用：修饰结果集的数据即元数据)
                                ResultSetMetaData rsmd = rs.getMetaData();
                                //通过ResultSetMetaData获取结果集中的列数
                                int columnCount = rsmd.getColumnCount();
                                //创建集合用于保存每行的数据
                                ArrayList<T> list = new ArrayList<>();
                                while (rs.next()){
                                    T t = clazz.newInstance();
                                    //处理结果集一行数据中的每一列,给t对象指定的属性赋值的过程
                                    for(int i=0;i<columnCount;i++){
                                        //获取列值
                                        Object columnValue = rs.getObject(i + 1);
                                        //获取每个列的列名
                                        String columnLabel = rsmd.getColumnLabel(i + 1);
                                        //给t对象指定的columnName属性，赋值为columnValue；通过反射
                                        Field field = clazz.getDeclaredField(columnLabel);
                                        field.setAccessible(true);
                                        field.set(t,columnValue);
                                    }
                                    list.add(t);
                                }
                                TCommon.shutDownquan(conn,ps,rs);
                                return list;
                            }
                    ```
