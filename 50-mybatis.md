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

        
        

