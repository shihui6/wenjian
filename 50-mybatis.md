*MyBatis
    概念：和数据库进行交互;持久化层框架(SQL映射框架)，半自动化
    优点：
        Mybatis将获取连接，获取PreparedStatement，预编译，执行sql，封装结果都由Mybatis自动执行，把sql语句留给我们自己操作；若项目部署到服务器中出现sql语句问题，可以直接修改sql语句无需将整个服务停掉重新修改再部署编译，热加载即可运行修改后的sql语句
        mybatis将sql和java编码分开，功能边界清晰，一个专注业务，一个专注数据
    特点：
        1.mybatis将重要的步骤抽取出来可以人工定制，其他步骤自动化
        2.重要步骤都是写在配置文件中(好维护)
        3.完全解决数据库的优化问题
        4.mybatis底层就是对原生jdbc的一个简单封装
        5.即将java编码与sql抽取出来，还不会失去自动化功能；即是半自动化的持久层框架
        6.mybatis是一个轻量级的框架

    和原始的jdbc的比较
        原生的jdbc是：dbutils(QueryRunner)---jdbcTemplate，这些称之为工具
        工具只是一些功能的简单封装
        框架：某个领域的整体解决方案;缓存,考虑异常问题，考虑部分字段映射问题。。。
        不用原生jdbc的原因:
            1：麻烦
            2：sql语句是硬编码在程序中的，当项目上线，若sql语句出现问题，需要特地停掉服务修改sql语句，重新包部署到服务器上
    
    另外一种关于数据库的框架：Hibernate-数据库交互的框架(ORM框架)，全自动化
        缺点：
            1.定制sql：Hibernate框架里sql语句已经定制好了，操作sql语句比较麻烦
            2.需要新学习HQL技术结合Hibernate才能操作SQL
            3.全映射框架(解释：一个数据表对应一个javabean，数据表里面有多少字段，javabean里面必须有多少个属性)；但是部分字段映射很难，难做
时间2021、1、26

    使用Mybatis操作数据库
        具体操作：
            1.环境搭建
                1.创建一个java工程(java工程里面的配置导包在下面的步骤里)
                2.创建测试库，测试表，以及封装数据的javabean，和操作数据库的dao接口
                    创建测试库，测试表，通过SQLyong自己创建相应的测试库和测试表
                    创建javabean：Employee(该类封装了表的数据)
                    创建一个Dao接口，用来操作数据库

        步骤：
            1.导包(在工程下创建libs目录与src同级)
                mybatis包      mybatis-3.4.1.jar 
                数据库驱动包    mysql-connector-java-5.1.37-bin.jar
                日志包          log4j-1.2.17.jar    前提：依赖类路径下一个log4j.xml配置文件

            2.写配置
                1.第一个配置文件
                    称为mybatis的全局配置文件，指导mybatis如何正确运行，比如连接向哪个数据库
                    具体操作：在java项目下创建libs目录，目录里面写xml配置文件
                2.第二个配置文件(即接口的实现文件)
                    作用：编写每一个方法都如何向数据库发送sql语句，如何执行。。。相当于接口的实现类
                    注意点：
                        1.将mapper的namespace属性改为接口的全类名
                        2.配置细节

                            ```xml      实现接口文件(EmployeeDao.xml)
                                <?xml version="1.0" encoding="UTF-8" ?>
                                <!DOCTYPE mapper
                                        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
                                        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
                                <!--namespace：是名称空间；要是接口的全类名。作用：告诉Mybatis这个配置文件是实现哪个接口的-->
                                <mapper namespace="com.atguigu.dao.EmployeeDao">
                                    <!--
                                        select：作用：用来定义namespace名称空间的全类名对应类的一个查询操作
                                        id：作用：来指定类EmployeeDao某个方法的实现；这里填方法名
                                        resultType:作用：指定方法运行后的返回值类型(全类名)；(查询操作必须指定)
                                        #{属性名}：概念：将方法参数取出来传递到#{}里面，进行sql操作
                                    -->
                                    <select id="selectBlog" resultType="com.atguigu.bean.Employee">
                                    select * from Blog where id = #{id}
                                </select>
                                </mapper>
                            ```
                        3.我们写的dao接口的实现文件(上面的配置细节)，mybatis默认是不知道的，需要在全局配置文件中注册

                            ```xml      全局配置文件(mybatis-config.xml)
                                <?xml version="1.0" encoding="UTF-8" ?>
                                <!DOCTYPE configuration
                                        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
                                        "http://mybatis.org/dtd/mybatis-3-config.dtd">
                                <configuration>
                                    <environments default="development">
                                        <environment id="development">
                                            <transactionManager type="JDBC"/>
                                            <!--配置连接池-->
                                            <dataSource type="POOLED">
                                                <property name="driver" value="com.mysql.jdbc.Driver"/>
                                                <property name="url" value="jdbc:mysql://localhost:3306/girls"/>
                                                <property name="username" value="root"/>
                                                <property name="password" value="wossh1423875545"/>
                                            </dataSource>
                                        </environment>
                                    </environments>
                                    <!-- dao接口的实现类文件，在全局配置中进行配置的地方 -->
                                    <!--引入我们自己编写的每一个接口的实现文件：即编写的实现接口的文件-->
                                    <mappers>
                                        <!--resource：表示从类路径下找关于接口实现的xml资源-->
                                        <mapper resource="EmployeeDao"/>
                                    </mappers>
                                </configuration>
                            ```

            3.测试
                1.根据全局配置文件先创建一个与数据库的连接
                2.sqlSessionFactory中获取sqlSession对象操作数据库

                ```java
                    public void test() throws IOException {
                            //1.根据全局配置文件创建出一个SqlSessionFactory
                            //SqlSessionFactory:概念：是SqlSession工厂；作用：负责创建SqlSession对象
                            //SqlSession:sql会话(代表和数据库的一次会话)
                            String resource = "mybatis-config.xml";
                            InputStream inputStream = Resources.getResourceAsStream(resource);
                            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

                            //2.获取和数据库的一次会话，相当于获取了和数据库的一次连接
                            SqlSession openSession = sqlSessionFactory.openSession();
                            //3.获取到dao接口的实现，使用SqlSesson操作数据库
                            EmployeeDao mapper = openSession.getMapper(EmployeeDao.class);
                            //4.调用接口实现中的方法，操作sql语句
                            Employee empById = mapper.getEmpById(1);
                            System.out.println(empById);
                        }
                ```
                
            解释说明：
                两个配置文件
                    1.全局配置文件：mybatis.config.xml；指导mybatis正确运行的一些全局设置
                    2.sql映射文件：EmployeeDao.xml;相当于是对Dao接口的一个实现描述
                细节：
                    1.获取到的是接口的代理对象；mybatis自动创建的
                    2.SqlSessionFactory和SqlSession
                        SqlSessionFactory创建SqlSession对象，Factory只new一次就行
                        SqlSession：相当于connection和数据库进行交互，和数据库的一次会话，就应该创建一个新的Sqlsession
            步骤总结：配置数据库连接池的全局配置文件(mybatis-config.xml)->为接口写实现接口文件(EmployeeDao.xml)->把实现接口文件注册到全局配置文件中

            4.更加复杂的测试：

                ```java 类用来描绘表格的结构
                    public class Employee {
                        private String name;
                        private String sex;
                        private String phone;
                        private Integer id;

                        public Employee(String name, String sex, String phone,Integer id) {
                            this.name = name;
                            this.sex = sex;
                            this.phone = phone;
                            this.id = id;
                        }
                    }
                ```
                ```java 实现接口
                    public interface EmployeeDao {
                        //按照员工的id查询员工
                        public Employee getEmpById(Integer id);
                        public void updateEmployee(Employee employee);
                        public int deleteEmployee(Integer id);
                        public Integer insertEmployee(Employee employee);
                    }
                ```
                ```xml  实现接口文件(EmployeeDao.xml)
                    <?xml version="1.0" encoding="UTF-8" ?>
                    <!DOCTYPE mapper
                            PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
                            "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
                    <!--namespace：是名称空间；要是接口的全类名。作用：告诉Mybatis这个配置文件是实现哪个接口的-->
                    <mapper namespace="com.atguigu.dao.EmployeeDao">
                        <!--
                            select：作用：用来定义namespace名称空间的全类名对应类的一个查询操作
                            id：作用：来指定类EmployeeDao某个方法的实现；这里填方法名
                            resultType:作用：指定方法运行后的返回值类型；(查询操作必须指定)
                            #{属性名}：概念：将方法参数取出来传递到#{}里面，进行sql操作
                        -->
                        <select id="getEmpById" resultType="com.atguigu.bean.Employee">
                            select `name`,sex,boyfriend_id from beauty where id = #{id}
                        </select>
                        <select id="updateEmployee" >
                            update beauty set name=#{name},sex=#{sex} where id = #{id}
                        </select>
                        <select id="deleteEmployee" >
                            delete from beauty where id = #{id}
                        </select>
                        <select id="insertEmployee" resultType="Integer">
                            insert into beauty(`name`,sex,phone) values(#{name},#{sex},#{phone})
                        </select>
                    </mapper>
                ```
                ```java 测试用
                    public class MybatisTest {
                        private static SqlSessionFactory sqlSessionFactory = null;
                        static {
                            //1.根据全局配置文件创建出一个SqlSessionFactory
                            //SqlSessionFactory:概念：是SqlSession工厂；作用：负责创建SqlSession对象
                            //SqlSession:sql会话(代表和数据库的一次会话)
                            String resource = "mybatis-config.xml";
                            InputStream inputStream = null;
                            try {
                                inputStream = Resources.getResourceAsStream(resource);
                                sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
                            } catch (IOException e) {
                                e.printStackTrace();
                            }
                        }
                        @Test
                        public void test() throws IOException {
                            //2.获取和数据库的一次会话，相当于获取了和数据库的一次连接
                            SqlSession openSession = sqlSessionFactory.openSession(true);
                            //3.获取到dao接口的实现，使用SqlSesson操作数据库
                            EmployeeDao mapper = openSession.getMapper(EmployeeDao.class);
                            //4.调用接口实现中的方法，操作sql语句，传入的参数根据在实现文件xml中写的顺序一致(new构造方法)
                            Integer integer = mapper.insertEmployee(new Employee("4545645436545", "男", 1));
                            mapper.updateEmployee(new Employee("关之琳","女",33));
                        }
                    }
                ```
时间：2021、1、27
        全局配置文件解析：

            ```xml mybatis-config.xml(全局配置文件)
                <configuration>
                    <!--
                    1.标签properties：作用：引入外部文件，可以将配置连接池的信息放到dbconfig.properties文件中，通过${}引用即可
                        如<properties resource="dbconfig.properties"></properties>

                    2.标签typeAliases：作用：为常用的类型javabean其别名(还是推荐使用全类名)
                        如：
                        <typeAliases>
                            //标签typeAlias：作用：一个typeAlias标签为了给一个javaBean起别名,别名默认为类名(不区分大小写)，在配置文件中直接可以使用类名
                            <typeAlias type="com.atguigu.bean.Employee" alias="指定别名"/>
                            //package标签，作用：为指定包名下的所有的类，减少全类名。默认为类名，可以通过@Alias注解指定别名
                            <package name="写包名，这个包下的所有的类">
                        </typeAliases>

                    3.typeHandlers标签；作用：类似于prepareStatement发送预编译sql，给sql语句设置值的时如setString(1,"天气真好"),typeHandlers在设置setString(),
                    setInteger()等等，设置预编译sql语句时的参数用的

                    4.objectFactory对象工厂：作用：mybatis跟数据库交互之后，通过sql语句查询出来审计局，把这些数据封装在一个对象里面，这个对象就是由objectFactory对象工厂创建的

                    5.插件是mybatis中的强大功能
                    -->
                    <!--
                    6.environments配置环境  属性default="development"指默认使用哪个环境，值对应下面的id
                        6.environment：配置一个具体的环境。都需要一个事务管理器和一个数据源；属性id="development"是当前环境的唯一标识
                            transactionManager：
                        数据源，和事物管理都是Spring来做
                    -->
                    <environments default="development">
                        <environment id="development">
                            <transactionManager type="JDBC"/>
                            <!--配置连接池-->
                            <dataSource type="POOLED">
                                <property name="driver" value="com.mysql.jdbc.Driver"/>
                                <property name="url" value="jdbc:mysql://localhost:3306/girls"/>
                                <property name="username" value="root"/>
                                <property name="password" value="wossh1423875545"/>
                            </dataSource>
                        </environment>
                    </environments>

                    <!--
                    6.databaseIdProvider标签，作用：是mybatis用来考虑数据库移植性的
                    -->
                    <databaseIdProvider type="DB_VENDOR">
                        <!--
                        name：数据库厂商标识，value：给这个标识起一个自己习惯的名字
                        -->
                        <property name="MySQL" value="mysql"></property>
                        <property name="Oracle" value="orcl"></property>
                    </databaseIdProvider>

                    <!--引入我们自己编写的每一个接口的实现文件：即编写的实现接口的文件-->
                    <mappers>
                        <!--
                        mapper属性：url,resource,class
                        url:可以从磁盘或者网络路径引用
                        resource:在类路径下找sql映射
                        class：直接引用接口的全类名
                                要将xml放在和dao接口同目录下，而且文件名和接口名一致，否则会报错
                                class的另外一种用法：在接口里的方法中通过注解写sql语句，不需要在xml配置文件中写了

                        配合使用：重要的dao写在配置文件里
                                简单的dao就直接标注解就可以
                        -->
                        <!--resource：表示从类路径下找关于接口实现的xml资源-->
                        <mapper resource="EmployeeDao"/>
                        <!--
                        批量注册  name=""值是dao所在的包名
                            作用：将包下所有的接口都可以注册到全局配置中来，就可以用该包下所有的接口对应的sql查询语句了
                            注意点：注册接口的时候，需要将每个接口的xml配置文件也同样移植到该包下，或者将包对应的配置文件的包名改成和接口包名一样的名字
                        -->
                        <package name="com.atguigu.dao"></package>
                    </mappers>
                </configuration>
            ```
            ```xml  EmployeeDao.xml 接口配置文件
                <mapper namespace="com.atguigu.dao.EmployeeDao">
                <!--
                    默认这个查询是不区分环境的
                    如果能精确匹配就精确，不能就用模糊
                    select标签属性databaseId可以指定具体在哪个环境下执行sql语句，对应全局全局配置的databaseIdProvider标签里面设置的value值
                -->
                    <select id="getEmpById" resultType="com.atguigu.bean.Employee" databaseId="mysql">
                        select `name`,sex,boyfriend_id from beauty where id = #{id}
                    </select>
                    <update id="updateEmployee" >
                        update beauty set name=#{name},sex=#{sex} where id = #{id}
                    </update>
                    <delete id="deleteEmployee" >
                        delete from beauty where id = #{id}
                    </delete>
                    <insert id="insertEmployee" resultType="Integer">
                        insert into beauty(`name`,sex,phone) values(#{name},#{sex},#{phone})
                    </insert>
                </mapper>
            ```

        映射文件解析

            ```xml      EmployeeDao.xml文件(即接口的映射文件)
                <?xml version="1.0" encoding="UTF-8" ?>
                <!DOCTYPE mapper
                        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
                        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
                <!--namespace：是名称空间；要是接口的全类名。作用：告诉Mybatis这个配置文件是实现哪个接口的-->
                <mapper namespace="com.atguigu.dao.EmployeeDao">
                <!--
                    mapper标签中能写的所有的标签
                        cache:和缓存有关
                        cache-ref:和缓存有关
                        parameterMap:参数map；废弃的。。原来是做复杂参数映射的
                        resultMap:结果映射，自定义结果集的封装规则
                        sql：抽取可重用的sql
                        delete,update,insert,select：增删改查
                -->
                    <select id="getEmpById" resultType="com.atguigu.bean.Employee">
                        select * from boys where id = #{id}
                    </select>
                    <update id="updateEmployee" >
                        update boys set name=#{name}where id = #{id}
                    </update>
                    <delete id="deleteEmployee" >
                        delete from boys where id = #{id}
                    </delete>
                    <insert id="insertEmployee" resultType="Integer">
                        insert into boys(`name`) values(#{name})
                    </insert>
                </mapper>

                    <!-- 
                        delete,update,insert增删改的标签的属性有：
                            id：命名空间的唯一标识符
                            parameterType：将要传入的语句的参数的完全限定类名或别名。这个属性是可选的，因为Mybatis可以通过TypeHandler推断出具体传入语句的参数类型，默认值为unset（解释：id对应方法的参数）

                            flushCache：
                            timeout：这个设置是在抛出异常之前，驱动程序等待数据库返回请求结果的描述，默认值为unset(依赖驱动)
                            statementType：通过使用什么来执行sql语句，这个属性会让Mybatis分别使用Statement,PreparedStatement或CallableStatement，默认值:Prepared;
                            useGeneratedKeys
                            keyProperty
                            keyColumn
                            databaseId
                    -->

                    <!-- 
                        属性的使用：useGeneratedKeys，keyProperty

                            让Mybatis自动的将自增id赋值给传入的employee对象的id属性
                                useGeneratedKeys="true":告诉Mybatis已经自动使用生成的主键，通过原生jdbc获取自增主键
                                keyProperty="id":将刚才自增的id封装给哪个属性,这里即是Employee对象的id属性

                                <insert id="insertEmployee" useGeneratedKeys="true" keyProperty="id">
                                    insert into boys(boyName,userCP) values(#{boyName},#{userCP})
                                </insert>

                                java发送测试代码
                                    Employee employee = new Employee(null,"底特律",600);
                                    Integer empById = mapper.insertEmployee(employee);
                                    System.out.println(empById);
                                    openSession.close();
                                    System.out.println(employee.getId());

                        获取非自增主键的值
                            <insert id="insertEmployee">
                                /*
                                    order="BEFORE"
                                        在核心sql语句之前执行查到id，将查到的id赋值给javabean的id属性，即selectKey标签的keyProperty属性指定的id

                                        执行机制：selectKey查询出来的值赋值给javabean的id属性，在核心sql执行的时候，将id赋值给sql语句
                                */
                                <selectKey order="BEFORE" resultType="integer" keyProperty="id">
                                    select max(id)+1 from boys
                                </selectKey>
                                insert into boys(id,boyName,userCP) values(#{id},#{boyName},#{userCP})
                            </insert>

                            java发送测试代码
                                    Employee employee = new Employee(null,"底特律",600);
                                    Integer empById = mapper.insertEmployee(employee);
                                    System.out.println(empById);
                                    openSession.close();
                                    System.out.println(employee.getId());

                        参数的各种取值
                            1.单个参数
                                基本类型
                                    取值：#{随便写}
                                    <insert id="insertEmployee">
                                        insert into boys(boyName) values(#{boyName})
                                    </insert>
                                传入javabean
                            2.多个参数
                                public Employee getEmpByIdAndEmpName(Integer id,String empName)
                                sql映像文件xml中取值：#{参数名}是无效的
                                可用：0,1(参数的索引)或者param1,param2(第几个参数paramN)
                                原因：只要传入多个参数，Mybatis会自动的将这些参数封装在一个map中，封装时使用key就是参数的索引和参数的第几个标识
                                    #{key}就是从这个map中取值

                                    **.@Param：为参数指定key；告诉mybatis，封装参数map的时使用我们指定的key(推荐这么做)
                                    如：public Employee getEmpByIdAndEmpName(@Param("boyName")Integer boyName,@Param("userCP")String userCP)
                                        在映射文件xml中就可以使用我们自己定义的key值
                                        <insert id="insertEmployee">
                                            insert into boys(boyName,userCP) values(#{boyName},#{userCP})
                                        </insert>

                            3.传入pojo(javaBean)
                                取值：#{pojo的属性名}
                            
                            4.传入map
                                如：public Employee getEmpByIdAndEmpName(Map<String,Object> map)
                                xml:
                                    <select id="getEmpByIdAndEmpName" resultType="com.atguigu.bean.Employee">
                                        select * from boys where id = #{id} and boyName=#{boyName}
                                    </select>
                                java类：
                                    Map<String,Object> map = new HashMap<>()
                                    map.put("id",1);
                                    map.put("boyName","张无忌");
                                    Integer empById = mapper.getEmpByIdAndEmpName(map);
                                    System.out.println(empById);
                                    openSession.close();
                                    System.out.println(employee.getId());


                        两种取值方式
                            #{属性名}：是参数预编译的方式，参数的位置都是用?代替，参数后来都是预编译设置进去的;安全,不会有sql注入问题
                            ${属性名}：不是参数预编译，而是直接和sql语句进行拼串；不安全

                                id=${id} amd empname=#{empName}
                                select * from boys where id=1 and empname=?

                                id=#{id} amd empname=#{empName}
                                select * from boys where id=? and empname=?

                                一般情况下使用#{}的方式，sql语句只有参数位置是支持预编译的
                                使用${}的情况：数据表是动态的，这时使用预编译的方式取值就不行，必须的用${}
                    -->
            ```
        映射文件解析2
            查询返回list列表(多条记录)

                ```xml    
                    <!--    
                        public List<Employee> getAllEmps();
                        resultType="":如果返回的是集合，写的是集合里面的类型
                    -->
                    <select id="getAllEmps" resultType="com.atguigu.bean.Employee">
                        select * from boys
                    </select>
                ```
                ```java
                    public void test() throws IOException {
                            //2.获取和数据库的一次会话，相当于获取了和数据库的一次连接
                            SqlSession openSession = sqlSessionFactory.openSession(true);
                            //3.获取到dao接口的映射，使用SqlSesson操作数据库
                            EmployeeDao mapper = openSession.getMapper(EmployeeDao.class);
                            List<Employee> allEmps = mapper.getAllEmps();
                            for(Employee employee : allEmps){
                                System.out.println(employee);
                            }
                            openSession.close();
                        }
                ```
                
            查询返回map(一条记录)

                ```xml
                    <!--public Map<String,Object> getEmpByIdreturnMap(Integer id);-->
                    <select id="getEmpByIdreturnMap" resultType="map">
                        select * from boys where  id= #{id}
                    </select>
                ```
                ```java 接口
                    public Map<Integer,Employee> getAllreturnMap();
                ```
                ```java 测试调用
                    public void test() throws IOException {
                            //2.获取和数据库的一次会话，相当于获取了和数据库的一次连接
                            SqlSession openSession = sqlSessionFactory.openSession(true);
                            //3.获取到dao接口的映射，使用SqlSesson操作数据库
                            EmployeeDao mapper = openSession.getMapper(EmployeeDao.class);
                            Map<String, Object> empByIdreturnMap = mapper.getEmpByIdreturnMap(1);
                            System.out.println(empByIdreturnMap);
                            openSession.close();
                        }
                ```

            查询返回map(map里有多条记录)

                ```java     接口
                    /**
                    * key是记录的主键，value是这条记录封装好的对象
                    * 把查询的记录的id的值作为key封装这个map
                    */
                    @MapKey("id")
                    public Map<Integer,Employee> getAllreturnMap();
                ```
                ```xml  接口文件
                    <!--public Map<Integer,Employee> getAllreturnMap();-->
                    <!--查询一个map中有多条记录，select标签resultType返回类型写集合里面元素的类型-->
                    <select id="getAllreturnMap" resultType="com.atguigu.bean.Employee">
                        select * from boys
                    </select>
                ```
                ```java java测试调用
                    public void test() throws IOException {
                            //2.获取和数据库的一次会话，相当于获取了和数据库的一次连接
                            SqlSession openSession = sqlSessionFactory.openSession(true);
                            //3.获取到dao接口的映射，使用SqlSesson操作数据库
                            EmployeeDao mapper = openSession.getMapper(EmployeeDao.class);
                            Map<Integer, Employee> allreturnMap = mapper.getAllreturnMap();
                            System.out.println(allreturnMap);
                            Employee employee = allreturnMap.get(1);
                            System.out.println(employee.getBoyName());
                            openSession.close();
                        }
                ```
