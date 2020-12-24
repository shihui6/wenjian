时间2020/12/15
###JDBC
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

###使用PreparedStatement实现CRUD操作

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
                                //ps和conn需要关闭的原因是：因为ps和conn都是以对象的形式在内存中出现的，手动关闭，释放内存
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
                                Connection conn = TCommon.setConnect(); //自己封装的连接方法
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
                                //获取结果集的元数据:ResultSetMetaData(元数据的作用：修饰结果集的数据)
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
                                //获取结果集的元数据:ResultSetMetaData(元数据的作用：修饰结果集的数据的作用)
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



###操作BLOB数据字段
    概念：
        mysql中，BLOB是一个二进制大型对象，是一个可以存储大量数据的容器，它能容纳不同大小的数据
        插入BLOB类型的数据必须使用PreparedStatement，因为BLOB类型的数据无法使用字符串拼接写的
        MYSQL的四种BLOB类型(除了在存储的最大信息量上不同外，他们是等同的)
                                TinyBlob            最大255字节
                                Blob                最大65k字节
                                MediumBlob          最大16M字节
                                LongBlob            最大4G字节
        实际使用中根据需要存入的数据大小定义不同的BLOB类型
        需要注意的是：如果存储的文件过大，数据库的性能会下降
        如果在指定了相关的Blob类型以后，还报错：xxx too large，那么在mysql的安装目录下，找my.ini问价加上如下的配置参数：max_allowed_packet=16M。同时注意：修改了my.ini文件之后，需要重启mysql服务

    使用：测试使用PreparedStatement操作Blob类型的数据
        事例：向数据表customers中插入Blob类型的字段

            ```java
                public void testInsert() throws Exception {
                        Connection conn = TCommon.setConnect();
                        String sql = "insert into customers(name,email,birth,photo)values(?,?,?,?)";
                        PreparedStatement ps = conn.prepareStatement(sql);
                        ps.setObject(1,"石惠");
                        ps.setObject(2,"666666@11.com");
                        ps.setObject(3,"1993-01-02");
                        FileInputStream is = new FileInputStream(new File("kola.jpg"));
                        ps.setBlob(4,is);
                        ps.execute();
                        TCommon.shutDown(conn,ps);
                    }
            ```
            ```java 从数据表中读取Blob类型的数据
                public void testQuery() throws Exception {
                        Connection conn = TCommon.setConnect();
                        String sql = "select id,name,email,birth,photo from customers where id = ?";
                        PreparedStatement ps = conn.prepareStatement(sql);
                        ps.setInt(1,24);
                        ResultSet rs = ps.executeQuery();
                        if(rs.next()){
                            int id = rs.getInt("id");
                            String name = rs.getString("name");
                            String email = rs.getString("email");
                            Date birth = rs.getDate("birth");

                            Customer cust = new Customer(id, name, email, birth);
                            System.out.println(cust);

                            //将Blob类型的字段下载下来，以文件的方式保存在本地
                            Blob photo = rs.getBlob("photo");
                            InputStream is = photo.getBinaryStream();
                            FileOutputStream fos = new FileOutputStream("shihui.jpg");
                            byte[] buffer = new byte[1024];
                            int length;
                            while ((length = is.read(buffer)) != -1){
                                fos.write(buffer,0,length);
                            }
                            is.close();
                            fos.close();
                        }
                        TCommon.shutDown(conn,ps);
                    }
                //注意点：
                /*
                    当添加的文件大小提示 xxx too large的时候，需要在mysql的安装目录下，找my.ini问价加上如下的配置参数：max_allowed_packet=16M。同时注意：修改了my.ini文件之后，需要重启mysql服务
                */
            ```

    使用：使用PreparedStatement实现批量数据的操作
        概念：update,delete本身就具有批量操作的效果

        事例：

            ```java
                //批量插入的方式一：使用PreparedStatement
                    /**
                    * *方式一：
                    *     特点：每次填充完占位符之后，就会execute和数据库服务器交互了一次，一共交互了1000次，所以有过多的交互效率不会太高
                    *     实际上还是一个一个输入插入的
                    *
                    *     方式一批量出入使用的时间48164毫秒
                    */
                    @Test
                    public void testInsert() throws Exception {
                        long start = System.currentTimeMillis();
                        Connection con = TCommon.setConnect();
                        String sql = "insert into goods(name)values(?)";
                        PreparedStatement ps = con.prepareStatement(sql);
                        for(int i=0;i<1000;i++){
                            //填充一次占位符，就执行一次execute
                            ps.setObject(1,"name"+i);
                            ps.execute();
                        }
                        long end = System.currentTimeMillis();
                        System.out.println(end-start);
                        TCommon.shutDown(con,ps);
                    }


                    /**
                    *
                    * 插入方式二
                    * 用到的知识点：1.addBatch() executeBatch() clearBatch()
                    * 注意点：mysql服务器默认是关闭批处理的，我们需要通过一个参数让mysql开启批处理的支持
                    *          ?rewriteBatchedStatements=true 写在配置文件的url后面
                    *       还需要换mysql驱动：换成mysql-connector-java-5.1.37-bin.jar
                    *
                    *方式二处理批量的速度627毫秒
                    */
                    @Test
                    public void testInsert1() throws Exception {
                        long start = System.currentTimeMillis();
                        Connection con = TCommon.setConnect();
                        String sql = "insert into goods(name)values(?)";
                        PreparedStatement ps = con.prepareStatement(sql);
                        for(int i=0;i<1000;i++){
                            ps.setObject(1,"name"+i);
                            //1.积攒sql
                            ps.addBatch();
                            if(i%500 == 0){  //积攒500次sql一次性处理，类似于io流中的字节仓库存储，减少对跟服务器的交互
                                //2.执行batch
                                ps.executeBatch();
                                //3.清空batch
                                ps.clearBatch();
                            }
                        }
                        long end = System.currentTimeMillis();
                        System.out.println(end-start);
                        TCommon.shutDown(con,ps);
                    }

                    /**
                    * 插入方式3
                    *      方法：设置连接不允许自动提交数据
                            sql语句的特点：
                                    执行一条sql语句，会自动提交数据
                    *      方式3的处理时间是437毫秒
                    */
                    @Test
                    public void testInsert2() throws Exception {
                        long start = System.currentTimeMillis();
                        Connection con = TCommon.setConnect();
                        //设置不允许自动提交数据
                        con.setAutoCommit(false);
                        String sql = "insert into goods(name)values(?)";
                        PreparedStatement ps = con.prepareStatement(sql);
                        for(int i=0;i<1000;i++){
                            ps.setObject(1,"name"+i);
                            //1.积攒sql
                            ps.addBatch();
                            if(i%500 == 0){
                                //2.执行batch
                                ps.executeBatch();
                                //3.清空batch
                                ps.clearBatch();
                            }
                        }
                        con.commit();
                        long end = System.currentTimeMillis();
                        System.out.println(end-start);
                        TCommon.shutDown(con,ps);
                    }
            ```

###数据库事务
    1.什么叫事务   
        事务：是一组逻辑操作单元，使数据从一种状态变换到另一种状态
            >一组逻辑操作单元：一个或多个DML操作

    2.事务处理的原则：保证所有事务都作为一个工作单元来执行，即使出现了故障，都不能改变这种执行方式。当在一个事务中执行多个操作时，要么所有的事务都被提交(commit),那么这些修改就永久地保存下来；要么数据库管理系统将放弃所作的所有修改，整个事务回滚(rollback)到最初状态

    3.数据一旦提交就不能修改

    4.哪些操作会导致数据的自动提交
        >DDL操作一旦执行，都会自动提交
        >DML默认情况下，一旦执行，就会自动提交
            >我们可以通过set autocommit = false的方式取消DML操作的自动提交
        >默认在关闭连接时，会自动的提交数据


        使用：
            事例：java代码实现事务的操作

                ```java
                    @Test
                        public void testupdatewith() {
                            Connection con = null;
                            try {
                                con = TCommon.setConnect();
                                //1.取消自动提交
                                con.setAutoCommit(false);
                                String sql1 = "update user_table set balance = balance -100 where user = ?";
                                update(con,sql1,"AA");

                                String sql2 = "update user_table set balance = balance +100 where user = ?";
                                update(con,sql2,"BB");
                                //2.提交数据
                                con.commit();
                            } catch (Exception e) {
                                e.printStackTrace();
                                try {
                                    con.rollback();
                                } catch (SQLException e1) {
                                    e1.printStackTrace();
                                }
                            } finally {
                                try {
                                    TCommon.shutDown(con,null);
                                } catch (Exception e2) {
                                    e2.printStackTrace();
                                }
                            }
                        }


                        public int update(Connection conn,String sql,Object ...args){
                            PreparedStatement ps = null;
                            try {
                                //1.预编译sql语句，返回PreparedStatement的实例
                                ps = conn.prepareStatement(sql);
                                //2.填充占位符
                                for(int i=0;i<args.length;i++){
                                    ps.setObject(i+1,args[i]);
                                }
                                //3.执行
                                return ps.executeUpdate();
                            } catch (Exception e) {
                                e.printStackTrace();
                            } finally {
                                //4.资源关闭
                                try {
                                    TCommon.shutDown(null,ps);
                                } catch (Exception e) {
                                    e.printStackTrace();
                                }
                            }
                            return 0;
                        }
                ```

                ```java  java代码演示数据库事务的隔离级别
                    @Test  
                    public void testTransactionSelect() throws Exception {
                        Connection conn = TCommon.setConnect();
                        String sql = "select user,password,balance from user_table where user = ?";
                        User user = getInstance(conn, User.class, sql, "CC");
                        System.out.println(user);
                    }

                    @Test
                    public void testTransaction() throws Exception {
                        Connection conn = TCommon.setConnect();
                        //获取当前连接的隔离级别
                        System.out.println(conn.getTransactionIsolation());
                        //设置数据库的隔离级别
                        conn.setTransactionIsolation(Connection.TRANSACTION_READ_COMMITTED);
                        //取消自动提交数据
                        conn.setAutoCommit(false);

                        String sql = "select user,password,balance from user_table where user = ?";

                        User user = getInstance(conn, User.class, sql, "CC");
                        System.out.println(user);
                    }



                    //通用的查询操作可用于返回数据表中的一条记录
                    public <T> T getInstance(Connection conn,Class<T> clazz,String sql,Object ...args) throws Exception {
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
                        TCommon.shutDownquan(null,ps,rs);
                        return null;
                    }

                ```
时间2020/12/23

###数据库连接池
    概念：为了解决传统开发中的数据库连接问题，可以采用数据库连接池技术
    数据库连接池的基本思想：
            就是为数据库连接建立一个"缓冲池"。预先在缓冲池中放入一定数量的连接，当需要建立数据库连接时，只要从"缓冲池"中取一个，使用完毕后再放回去
    数据库连接池的作用：
            负责分配，管理和释放数据库连接，它允许引用程序重复使用一个现有的数据库连接，而不是重新建立一个
    
    数据库连接池的基本思路解析：
            数据库连接池在初始化时将创建一定数量的数据库连接放到连接池中，这些数据库连接的数量是由最下数据库连接数来设定的。无论这些数据库连接是否被使用，连接池都将一直保证至少拥有这么多的连接数量。连接池的最大数据库连接数量限定了这个连接池能占有最大的连接数，当应用程序向连接数超过最大连接数量时，这些请求将被加入到等待队列中。


    ***多种开源的数据库连接池
        概念：JDBC的数据库连接池使用javax.sql.DataSource来表示，DataSource只是一个接口，该接口通常由服务器(Weblogic,WebSphere,Tomcat)提供实现，也有一些开源组织提供的实现

        具体的数据库连接吃技术：
            DBCP：是Apache提供的数据库连接池。tomcat服务器自带dbcp数据库连接池。速度相对c3p0较快，但因自身存在BUG,Hibernate3已不再提供支持
            C3P0：是一个开源组织提供的一个数据库连接池，速度相对较慢，稳定性还可以。hibernate官方推荐使用
            Proxool：是sourceforge下的一个开源项目数据库连接池，有监控连接池状态的功能，稳定性较c3p0差一点
            BoneCP：是一个开源阻止提供的数据库连接池，速度快
            Bruid：是阿里提供的数据库连接池，速度快且稳定

        使用：
            事例：c3p0连接池的使用
                步骤一：在src目录下新建xml配置文件，用于读取文件中的配置信息

                ```xml
                    <?xml version="1.0" encoding="UTF-8" ?>
                    <c3p0-config>
                        <!-- This app is massive! -->
                        <named-config name="helloc3p0">
                            <!--        提供获取连接的4个基本信息-->
                            <property name="driverClass">com.mysql.jdbc.driver</property>
                            <property name="jdbcUrl">jdbc:mysql://localhost:3306/test</property>
                            <!--        端口号默认的3306可以省略掉-->
                            <property name="user">root</property>
                            <property name="password">wossh1423875545</property>
                            <!--        进行数据库连接池管理的基本信息-->
                            <!-- 当数据库连接池中的连接数不够时，c3p0一次性向数据库服务器申请的连接数 -->
                            <property name="acquireIncrement">5</property>
                            <!-- c3p0数据库连接池中初始化时的连接数 -->
                            <property name="initialPoolSize">100</property>
                            <!-- c3p0数据库连接池维护的最少连接数 -->
                            <property name="minPoolSize">50</property>
                            <!-- c3p0数据库连接池维护的最多的连接数 -->
                            <property name="maxPoolSize">100</property>
                            <!-- c3p0数据库连接池最多维护的Statement的个数 -->
                            <property name="maxStatements">50</property>
                            <!-- 每个连接中可以最多使用Statement的个数 -->
                            <property name="maxStatementsPerConnection">5</property>
                        </named-config>
                    </c3p0-config>
                ```
                步骤二：读取配置文件信息，创建c3p0连接池,创建连接

                ```java
                    private  static ComboPooledDataSource cpds = new ComboPooledDataSource("helloc3p0");
                    public static Connection testGetConnection1() throws Exception {
                        Connection conn = cpds.getConnection();
                        return conn;
                    }
                ```



            事例：使用DBCP连接池技术

                ```java
                    @Test
                        public void testGetConnection() throws Exception {
                            //创建了DBCP的数据库连接池,最好是放在方法外，添加静态的连接池
                            BasicDataSource source = new BasicDataSource();
                            //设置基本信息
                            source.setDriverClassName("com.mysql.jdbc.Driver");
                            source.setUrl("jdbc:mysql:///test");
                            source.setUsername("root");
                            source.setPassword("wossh1423875545");

                            //还可以设置其他涉及数据库连接池管理的相关属性
                            source.setInitialSize(10);
                            source.setMaxActive(10);

                            Connection conn = source.getConnection();
                            System.out.println(conn);
                        }
                ```

            事例：使用Druid连接池技术

                ```java
                        private static DataSource source1; //数据库连接池
                        static {
                            try {
                                Properties pros = new Properties();
                                InputStream is = ClassLoader.getSystemClassLoader().getResourceAsStream("druid.properties");
                                pros.load(is);
                                source1 = DruidDataSourceFactory.createDataSource(pros);
                            } catch (Exception e) {
                                e.printStackTrace();
                            }
                        }
                        public static Connection getConnection3() throws Exception {
                            Connection conn = source1.getConnection();
                            return conn;
                        }

                        //使用QueryRunner对数据表进行操作
                            //QueryRunner的作用：QueryRunner是ommons-dbutils是Apache组织提供的一个开源的JDBC工具类库

                        /**
                        * 先导入commons-dbutils-1.3.jar包
                        * commons-dbutils是Apache组织提供的一个开源的JDBC工具类库，封装了针对于数据库的增删改查操作
                        */
                        //测试插入
                        public void testInsert() throws Exception {
                            //runner核心原理和之前的PrepareStatement预编译源码是一样的
                            QueryRunner runner = new QueryRunner();
                            Connection conn = TCommon.getConnection3();//调用上面Druid连接技术
                            String sql = "insert into customers(name,email,birth)values(?,?,?)";
                            runner.update(conn,sql,"蔡徐坤","caixukun@qq.com","1997-11-11");
                            conn.close();
                        }

                         /**查询单个记录  QueryRunner是ommons-dbutils是Apache组织提供的一个开源的JDBC工具类库
                        *BeanHander:是ResultSetHandler接口的实现类，用于封装表中的一条记录
                        */
                        @Test
                        public void testSearch() throws Exception {
                            QueryRunner runner = new QueryRunner();
                            Connection conn = TCommon.getConnection3();
                            String sql = "select id,name,email,birth from customers where id =?";
                            BeanHandler<Customer> handler = new BeanHandler<>(Customer.class);
                            Customer queryResult = runner.query(conn, sql, handler, 24);
                            System.out.println(queryResult);
                            conn.close();
                        }

                        /**查询多条记录  QueryRunner是ommons-dbutils是Apache组织提供的一个开源的JDBC工具类库
                        *BeanListHander:是ResultSetHandler接口的实现类，用于封装表中的多条记录
                        */
                        @Test
                        public void testSearch2() throws Exception {
                            QueryRunner runner = new QueryRunner();
                            Connection conn = TCommon.getConnection3();
                            String sql = "select id,name,email,birth from customers ";
                            BeanListHandler<Customer> handler = new BeanListHandler<>(Customer.class);
                            List<Customer> queryResult = runner.query(conn, sql, handler);
                            queryResult.forEach(System.out::println);
                            conn.close();
                        }


                        /**查询单个记录   QueryRunner是ommons-dbutils是Apache组织提供的一个开源的JDBC工具类库
                        *MapHander:是ResultSetHandler接口的实现类，对应封装表中的一条记录
                        * 将字段及相应字段的值作为map中的key和value
                        */
                        @Test
                        public void testSearch3() throws Exception {
                            QueryRunner runner = new QueryRunner();
                            Connection conn = TCommon.getConnection3();
                            String sql = "select id,name,email,birth from customers where id =?";
                            MapHandler mapHandler = new MapHandler();
                            Map<String, Object> queryResult = runner.query(conn, sql, mapHandler, 24);
                            System.out.println(queryResult);
                            conn.close();
                        }

                        /**查询单个记录    QueryRunner是ommons-dbutils是Apache组织提供的一个开源的JDBC工具类库
                        *MapListHander:是ResultSetHandler接口的实现类，对应封装表中的多条记录
                        * 将字段及相应字段的值作为map中的key和value，将这些map添加到list中
                        */
                        @Test
                        public void testSearch4() throws Exception {
                            QueryRunner runner = new QueryRunner();
                            Connection conn = TCommon.getConnection3();
                            String sql = "select id,name,email,birth from customers";
                            MapListHandler mapHandler = new MapListHandler();
                            List<Map<String, Object>> queryResult = runner.query(conn, sql, mapHandler);
                            System.out.println(queryResult);
                            conn.close();
                        }

                        /**
                        *ScalarHandler:用于查询特殊值
                        */
                        @Test
                        public void testSearch5() throws Exception {
                            QueryRunner runner = new QueryRunner();
                            Connection conn = TCommon.getConnection3();
                            String sql = "select count(*) from customers";
                            ScalarHandler handler = new ScalarHandler();
                            Long queryResult = (Long) runner.query(conn, sql, handler);
                            System.out.println(queryResult);
                            conn.close();
                        }


                        /**
                            DbUtils也提供了关闭操作
                            DbUtils.closeQuietly(conn);
                            DbUtils.closeQuietly(ps);
                            DbUtils.closeQuietly(rs);
                        */
                ```


