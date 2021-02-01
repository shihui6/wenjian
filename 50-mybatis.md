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

                    2.标签typeAliases：作用：为常用的类型javabean起别名(还是推荐使用全类名)
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
时间2021、1、28
            resultMap自定义结果集的封装规则

                ```xml
                    <mapper namespace="com.atguigu.dao.CatDao">
                        <!--public Cat getCatById(Integer id);-->
                        <!--
                            默认mybatis自动封装结果集
                            1.按照列名和属性名一一对应(不区分大小写)(解释：即查询出来的表格的列名和封装数据的javaBean的属性名)
                            2.如果不一一对应    (解释：即查询出来的表格的列名和封装数据的javaBean的属性名,不一一对应)
                                1.开启驼峰命名法(满足驼峰命名规则 aaa_bbb aaa_Bbb)
                                2.起别名

                            resultType="com.atguigu.bean.Cat"在使用默认规则，即属性名和列名一一对应
                            resultMap="myCat":查出数据封装结果的时候，使用mycat自定义的规则进行封装
                        -->
                        <select id="getCatById" resultMap="myCat">
                            select * from t_cat where id = #{id};
                        </select>

                        <!--
                            自定义结果集(resultMap)：自己定义数据表的每一列数据和javabean的属性的映射规则
                            type="":指定为哪个javabean自定义封装规则；全类名
                            id="":唯一标识
                        -->
                        <resultMap type="com.atguigu.bean.Cat" id="myCat">
                            <!--
                                指定主键列的对应规则
                                column="id":指定哪一列是主键列（数据表中的列）
                                property="":指定car的哪个属性封装id这一列数据
                            -->
                            <id property="id" column="id"></id>
                            <!--普通列-->
                            <result property="name" column="cName"></result>
                            <result property="age" column="cAge"></result>
                            <result property="gender" column="cgender"></result>
                        </resultMap>
                    </mapper>
                ```

            联合查询第一种方式

                ```xml
                    <mapper namespace="com.atguigu.dao.KeyDao">
                        <!--
                            private Integer id;
                            private String keyName;
                            private Lock lock;//当钥匙能开哪个锁
                        -->
                        <select id="getKeyById" resultMap="myKey">
                            select k.id,k.`keyname`,k.`lockid`,
                                l.`id` lid ,l.`lockName` from t_key k
                                left join t_lock l on k.`lockid` = l.`id`
                                where k.`id` = #{id}
                        </select>

                        <!--自定义封装规则：使用级联属性封装联合查询出的结果-->
                        <resultMap type="com.atguigu.bean.Key" id="myKey">
                            <id property="id" column="id"></id>
                            <result property="keyName" column="keyname"></result>
                            <result property="lock.id" column="lid"></result>
                            <result property="lock.lockName" column="lockName"></result>
                        </resultMap>
                    </mapper>

                    //输出的结果：Key{id=1, keyName='1号钥匙', lock=Lock{id=1, lockName='1号锁'}}
                ```
            
            联合查询第二种：使用association标签联合对象

                ```xml
                    <mapper namespace="com.atguigu.dao.KeyDao">
                        <!--
                            private Integer id;
                            private String keyName;
                            private Lock lock;//当钥匙能开哪个锁
                        -->
                        <select id="getKeyById" resultMap="myKey">
                            select k.id,k.`keyname`,k.`lockid`,
                                l.`id` lid ,l.`lockName` from t_key k
                                left join t_lock l on k.`lockid` = l.`id`
                                where k.`id` = #{id}
                        </select>

                        <!--mybatis推荐的<association property=""><association>-->
                        <resultMap type="com.atguigu.bean.Key" id="myKey">
                            <id property="id" column="id"></id>
                            <result property="keyName" column="keyname"></result>
                            <!--接下来的属性是一个对象，自定义这个对象的封装规则，使用association标签：表示联合了一个对象->
                            <!-javaType：指定这个属性的类型-->
                            <association property="lock" javaType="com.atguigu.bean.Lock">
                                <!--定义lock属性对应的这个lock对象如何封装-->
                                <id property="id" column="lid"></id>
                                <result property="lockName" column="lockName"></result>
                            </association>
                        </resultMap>
                    </mapper>

                    //输出的结果：Key{id=1, keyName='1号钥匙', lock=Lock{id=1, lockName='1号锁'}}
                ```
            
            联合查询第二种：使用collection标签封装集合

                ```xml
                    <mapper namespace="com.atguigu.dao.LockDao">
                        <select id="getLockById" resultMap="myLock">
                            select l.*,k.id kid,k.`keyname`,k.`lockid` from t_lock l
                            left join t_key k on l.`id` = k.`lockid`
                            where l.id = #{id}
                        </select>
                        <!--
                            public Class Lock{
                                private Integer id;
                                private String lockName;
                                //查询锁的时候把所有的钥匙也查出来
                                private List<Key> keys;
                            }
                        -->
                        <resultMap type="com.atguigu.bean.Lock" id="myLock">
                            <id property="id" column="id"></id>
                            <result property="lockName" column="lockName"></result>
                            <!--
                                collection:作用：定义集合元素的封装
                                    property="":指定哪个属性是集合属性
                                    javaType:指定对象类型，这个属性在association标签里使用
                                    ofType="":指定集合里面的类型
                            -->
                            <collection property="keys" ofType="com.atguigu.bean.Key">
                                <!--标签体中指定集合中这个元素的封装规则-->
                                <id property="id" column="kid"></id>
                                <result property="keyName" column="keyname"></result>
                            </collection>
                        </resultMap>
                    </mapper>
                ```
                ```java 测试输出结果
                    public void Test(){
                            SqlSession openSession = sqlSessionFactory.openSession(true);
                            LockDao mapper = openSession.getMapper(LockDao.class);
                            Lock lock = mapper.getLockById(3);
                            System.out.println(lock);
                            List<Key> keys = lock.getKeys();
                            for(Key key:keys){
                                System.out.println(key);
                            }
                            openSession.close();
                        }
                        //输出结果;Lock{id=3, lockName='303号房间的钥匙', keys=[Key{id=3, keyName='303钥匙1', lock=null}, Key{id=4, keyName='303钥匙2', lock=null}]}
                ```

            联合查询第三种：分布查询  (用的比较多)
                        步骤：
                            /**
                            * 分布查询：
                            *      思路：查询要是的时候顺便查出锁
                            *          先把key钥匙查出来，在通过钥匙的id查出锁
                            *          1.Key key = keyDao.getKeyById();
                            *          2.Lock lock = lockDao.getLockById(1)
                            */

                                ```xml  LockDao.xml的内容
                                    <mapper namespace="com.atguigu.dao.LockDao">
                                        <!--public Lock getLockByIdSimple(Integer id);-->
                                        <select id="getLockByIdSimple" resultType="com.atguigu.bean.Lock">
                                            select * from t_lock where id=#{id}
                                        </select>
                                    </mapper>
                                ```
                                ```xml  KeyDao.xml的内容
                                    <mapper namespace="com.atguigu.dao.KeyDao">
                                        <!--
                                            public Key getKeyByIdSimple(Integer id);
                                            查询key的时候也可以带上锁的信息
                                            getKeyByIdSimple查询出来的记过： id  keyname  lockid
                                        -->
                                        <select id="getKeyByIdSimple" resultMap="mykey">
                                            select * from t_key where id=#{id}
                                        </select>
                                        <resultMap type="com.atguigu.bean.Key" id="mykey">
                                            <id property="id" column="id"></id>
                                            <result property="keyName" column="keyname"></result>
                                        <!--     
                                            告诉mybatis自己去调用一个查询查锁的
                                            select="":指定一个查询sql的唯一标识，mybatis自动调用指定的sql将查出的lock封装进来
                                            public Lock getLockByIdSimple(Integer id);需要传入锁的id
                                                告诉mybatis把哪一列的值传递过去，column：指定将哪一列的数据传过去
                                                    此处的column值：lockid对应的是上面查出来的值
                                                select="com.atguigu.dao.LockDao.getLockByIdSimple"：指定执行LockDao里面的getLockByIdSimple方法在LockDao.xml里面对应的sql语句
                                        -->
                                            <association property="lock"
                                                select="com.atguigu.dao.LockDao.getLockByIdSimple"
                                                column="lockid">
                                                <!-- 若要传入多个值则：column={key1=列名,key2=列名} -->
                                            </association>
                                        </resultMap>
                                    </mapper>

                                    执行机制：先执行getLockByIdSimple方法执行的sql，返回的结果封装到结果集里，因为KeyDao类里有Lock属性，所以第二步在进行lock的查询操作，将第一步查询出来的lockid作为参数，传递到getLockByIdSimple方法里
                                ```

            动态sql语句
                概念：接口方法可以不传参数，也可传入多个参数；之前我们学的都是传入固定的参数(动态sql也就是对传入参数不确定的处理)

                ```xml
                    <mapper namespace="com.atguigu.dao.TeacherDao">
                    <!--public Teacher getTeacherById(Integer id);-->
                        <select id="getTeacherById" resultMap="teacherMap">
                            select * from t_teacher where id =#{id}
                        </select>
                        <resultMap id="teacherMap" type="com.atguigu.bean.Teacher">
                            <id property="id" column="id"></id>
                            <result property="address" column="address"></result>
                            <result property="birth" column="birth_date"></result>
                            <result property="course" column="class_name"></result>
                            <result property="name" column="teacherName"></result>
                        </resultMap>
                        <!--public List<Teacher> getTeacherByCondition(Teacher teacher);-->
                        <!--
                        if标签：
                            作用：判断接口定义的方法传入的参数，因为方法每次传入的参数的个数和种类可以不固定
                            属性test="":编写判断条件
                            属性id!=null:执行机制：取出传入的javaBean属性中的id值，判断其是否为空;如果满足if条件会把内容拼接sql语句上
                        where标签：（推荐使用这种）
                            作用：可以帮我去掉在编写if判断的时前面的and
                        trim标签：
                            作用：截取字符串
                            属性prefix="":前缀，为我们下面的sql整体添加一个前缀
                            属性prefiexOverrides="":去除整体字符串前面多余的字符
                            属性suffix="":为整体添加一个后缀
                            属性suffixOverrides="":后面那个多了可以去掉
                        -->
                        <select id="getTeacherByCondition" resultMap="teacherMap">
                            select * from t_teacher
                    <!--<where>-->
                    <!--    <if test="id!=null">-->
                    <!--         id>#{id}-->
                    <!--    </if>-->
                    <!--    <if test="name!=null">-->
                    <!--          and teacherName like #{name}-->
                    <!--    </if>-->
                    <!--    <if test="birth!=null">-->
                    <!--          and birth_date &lt; #{birth}-->
                    <!--    </if>-->
                    <!--</where>-->
                            <trim prefix="where" prefixOverrides="and" suffixOverrides="and">
                                <if test="id!=null">
                                    id>#{id} and
                                </if>
                                <if test="name!=null">
                                    teacherName like #{name} and
                                </if>
                                <if test="birth!=null">
                                    birth_date &lt; #{birth} and
                                </if>
                            </trim>
                        </select>
                    </mapper>
                ```
                ```java
                    public void Test(){
                            SqlSession openSession = sqlSessionFactory.openSession(true);
                            TeacherDao mapper = openSession.getMapper(TeacherDao.class);
                            Teacher teacher = new Teacher();
                            //可以不传参数，可以传入多个参数
                            teacher.setName("%a%");
                            teacher.setId(1);
                            List<Teacher> list = mapper.getTeacherByCondition(teacher);
                            System.out.println(list);
                            openSession.close();
                        }
                ```
时间2021、1、29
            动态sql之foreach标签

                ```java     接口文件部分
                    //@Param("ids") List<Integer> ids给参数取别名
                    public List<Teacher> getTeacherByIdIn(@Param("ids") List<Integer> ids);
                ```
                ```xml  配置sql语句
                    <mapper namespace="com.atguigu.dao.TeacherDao">
                        <resultMap id="teacherMap" type="com.atguigu.bean.Teacher">
                            <id property="id" column="id"></id>
                            <result property="address" column="address"></result>
                            <result property="birth" column="birth_date"></result>
                            <result property="course" column="class_name"></result>
                            <result property="name" column="teacherName"></result>
                        </resultMap>
                        <!--
                            foreach标签
                                作用：帮我们遍历集合里的元素
                                属性collection="":指定要遍历的集合的key，这里传入的值为/@Param("ids") List<Integer> ids这里定义的值
                                属性close="":以什么结束
                                属性index="":索引
                                        如果遍历的是一个list
                                            index：指定的变量保存了当前索引
                                            item：保存当前遍历的元素的值
                                        如果遍历的是一个map
                                            index：指定的变量就保存了当前遍历的元素的key
                                            item：就是保存当前遍历的元素的值
                                属性item="变量名":每次遍历出的元素起一个变量名方便引用
                                属性open="":以什么开始
                                属性separator="":每次遍历的元素的分隔符
                        -->
                        <select id="getTeacherByIdIn" resultMap="teacherMap">
                            select * from t_teacher where id in
                            <foreach
                                collection="ids" item="id_item"
                                separator="," open="(" close=")"
                            >
                                #{id_item}
                            </foreach>

                        </select>
                    </mapper>
                ```
                ```java     测试
                    public void Test(){
                        SqlSession openSession = sqlSessionFactory.openSession(true);
                        TeacherDao mapper = openSession.getMapper(TeacherDao.class);
                        List<Teacher> teacherByIdIn = mapper.getTeacherByIdIn(Arrays.asList(1, 2, 3, 4, 5));
                        System.out.println(teacherByIdIn);
                        openSession.close();
                    }
                ```

            动态sql之choose标签的使用

                ```java
                    public List<Teacher> getTeacherByConditionChoose(Teacher teacher);
                ```
                ```xml
                    <mapper namespace="com.atguigu.dao.TeacherDao">
                        <resultMap id="teacherMap" type="com.atguigu.bean.Teacher">
                            <id property="id" column="id"></id>
                            <result property="address" column="address"></result>
                            <result property="birth" column="birth_date"></result>
                            <result property="course" column="class_name"></result>
                            <result property="name" column="teacherName"></result>
                        </resultMap>
                    <!--    public List<Teacher> getTeacherByConditionChoose(Teacher teacher);-->
                        <select id="getTeacherByConditionChoose" resultMap="teacherMap">
                            select * from t_teacher
                            <where>
                                <choose>
                                    <when test="id!=null">
                                        id=#{id}
                                    </when>
                                    <when test="name!=null">
                                        id=#{id}
                                    </when>
                                    <when test="birth !=null">
                                        birth_date=#{birth}
                                    </when>
                                    <otherwise>
                                        1=1
                                    </otherwise>
                                </choose>
                            </where>
                        </select>
                    </mapper>
                ```
                ```java 测试
                    public void Test(){
                            SqlSession openSession = sqlSessionFactory.openSession(true);
                            TeacherDao mapper = openSession.getMapper(TeacherDao.class);
                            Teacher teacher = new Teacher();
                            teacher.setId(1);
                            teacher.setName("%a%");
                            List<Teacher> list = mapper.getTeacherByConditionChoose(teacher);
                            System.out.println(list);
                            openSession.close();
                        }
                ```

            动态sql之：set标签结合if标签更新操作

                ```java 接口
                    public int updateTeacher(Teacher teacher);
                ```
                ```xml
                    <!--public int updateTeacher(Teacher teacher);-->
                    <!--    
                        set标签
                            作用：专门在更新语句中使用，且在if标签拼接的同时能将最后一个逗号去掉
                    -->
                    <update id="updateTeacher">
                        update t_teacher
                        <set>
                            <if test="name!=null">
                                teacherName=#{name},
                            </if>
                            <if test="course!=null">
                                class_name=#{course},
                            </if>
                        </set>
                        <where>
                            id=#{id}
                        </where>
                    </update>
                ```
                ```java 测试
                    public void Test(){
                            SqlSession openSession = sqlSessionFactory.openSession(true);
                            TeacherDao mapper = openSession.getMapper(TeacherDao.class);
                            Teacher teacher = new Teacher();
                            teacher.setId(1);
                            teacher.setName("哈哈哈");
                            int i = mapper.updateTeacher(teacher);
                            System.out.println(i);
                            openSession.close();
                        }
                ```

            动态sql---其他两个参数的使用
                在mybatis中，传入的参数可以用来做判断：
                    _parameter：代表传入来的参数
                        1.传入了单个参数：_parameter就代表这个参数
                        2.传入了多个参数：_parameter就代表多个参数集合起来的map
                    _databaseId：代表当前环境
                        如果配置了databaseIdProvider:_databaseId才有值

            动态sql---抽取可重用的sql，用到include标签结合sql标签

                ```xml
                    <mapper>
                        <!-- 抽取可重用的sql语句 -->
                        <sql id="selectSql">select * from t_teacher</sql>
                        <select id="getTeacherByIdIn" resultMap="teacherMap">
                            <include refid="getTeacherByIdIn"></include> where id in
                            <foreach
                                collection="ids" item="id_item"
                                separator="," open="(" close=")">
                                #{id_item}
                            </foreach>
                        </select>
                    </mapper>
                ```

***mybatis的缓存机制
    缓存：
        概念：暂时的存储一些数据
        作用：加快系统的查询速度
    
    mybatis缓存机制：
        概念：Map，能保存查询出的一些数据
        一级缓存：线程级别的缓存，本地缓存；SqlSession级别的缓存，只有在当前的SqlSession能使用
        二级缓存：全局范围的缓存，除了当前线程，SqlSession能用其他线程也可以使用

            一级缓存：mybatis：SqlSession级别的缓存，默认是存在的
                一级缓存机制：只要之前查询过的数据，mybatis就会保存在一个缓存中(Map),下次获取直接拿
                一级缓存失效的几种情况
                    1.不同的sqlSession，使用不同的一级缓存
                        因为只有在同一个sqlSession期间查询到的数据会保存在这个sqlSession的缓存中，下次使用这个这个sqlSession查询会从缓存中拿
                    2.同一个方法，不同的参数，由于之前没有查询过，所有还会发新的sql
                    3.在这个sqlSession期间执行了任何一次增删改操作，增删改操作会把缓存清空
                    4.在sqlSession期间手动清空缓存
                
                二级缓存
                    概念：全局作用域的缓存，默认不开启，需要手动配置
                    注意点：二级缓存在SqlSession关闭或提交之后才会生效
                    使用步骤：
                        1.全局配置文件中开启二级缓存<setting name="cacheEnabled" value="true" />

                            ```xml
                                <?xml version="1.0" encoding="UTF-8" ?>
                                <!DOCTYPE configuration
                                        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
                                        "http://mybatis.org/dtd/mybatis-3-config.dtd">
                                <configuration>
                                    <settings>
                                        <!--开启全局缓存开关，且告诉映射文件需要使用全局缓存-->
                                        <setting name="cacheEnabled" value="true"></setting>
                                    </settings>
                                </configuration>
                            ```
                        2.需要使用二级缓存的映射文件处使用cache配置缓存<cache />意思是这个映射文件需要使用全局缓存

                            ```xml
                                <?xml version="1.0" encoding="UTF-8" ?>
                                <!DOCTYPE mapper
                                        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
                                        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
                                <mapper namespace="com.atguigu.dao.TeacherDao">
                                    <!--表示该映射文件使用全局缓存(二级缓存)-->
                                    <cache></cache>
                                </mapper>
                            ```
                        3.注意：pojo(javabean)需要实现Seralizable接口

                            ```java
                                public class Teacher implements Serializable {}
                            ```

                    缓存的查询顺序
                        1.任何时候都是先看二级缓存，再看一级缓存，如果大家都没有就去查询数据库
                        2.不会出现一级缓存和二级缓存中有同一个数据
                            二级缓存中是在一级缓存关闭了就有了
                            一级缓存中，二级缓存中没有此数据，就会看一级缓存，一级缓存没有去查数据库，数据库的查询后把结果放在一级缓存中

                    缓存的有关设置
                        1.全局setting的cacheEnable
                            配置二级缓存开关，一级缓存一直是打开的
                        2.select标签的userCache属性
                            配置这个select是否使用二级缓存。一级缓存一直是使用的
                        3.sql标签的flushCache属性
                            增删改默认flushCache=true。sql执行以后，会同时清空一级和二级缓存。查询默认flushCache=false
                        4.sqlSession.clearCache()
                            只是用来清除一级缓存

                        
    整合第三方缓存
        整合ehCache:ehcache非常专业的java进行内的缓存框架
        使用步骤：
            1.导包
                ehcache-core-2.6.8.jar(ehcache核心包)
                mybatis-ehcache-1.0.3.jar(ehcache的整合包)
                slf4j-api-1.7.21.jar(日志包)
                slf4j-log4j12-1.7.21.jar(日志包)
            2.ehcache需要一个配置文件
                ehcache.xml;放在类路径下
            3.在mapper.xml（映射文件中）配置自定义的缓存
                <cache type="org.mybatis.caches.ehcache.EhcacheCache"></cache>
            4.别的映射文件要用这个缓存
                在mapper.xml的<cache type="org.mybatis.caches.ehcache.EhcacheCache"></cache>下面配置一下
                <cache-ref namespace="com.atguigu.dao.TeacherDao">

***SSM  
    概念：Spring+SpringMVC+MyBatis
    
***文件上传和下载(在javaweb里面讲过了)
    概念：浏览器将本地的文件上传到服务器上，交给服务器保存
