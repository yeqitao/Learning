# Learning

- [Mybatis](#mybatis)

  - [Mybatis入门-环境搭建](#Mybatis入门-环境搭建)

   - [Mybatis配置文件](#Mybatis配置文件)

     



## Mybatis

​	Mybatis是一个ORM的半自动轻量级持久层框架,全称(Object/Relation Mapping)对象关系映射,Mybatis简化了程序员手动操作jdbc以及封装结果集的繁杂工作.

### 	Mybatis入门-环境搭建

​		maven依赖包

```xml
    <dependencies>
        <!-- Mybatis插件 -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.5</version>
        </dependency>
        <!-- 数据库驱动 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.6</version>
            <scope>runtime</scope>
        </dependency>
        <!-- 单元测试 -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
    </dependencies>
```

​		创建实体类

```java
public class User {
    private int id;
    private String username;
    private String password;
    private int age;
    省略get/set方法
}
```

​		resource路径下

​		创建UserMapper.xml文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!-- namespace 命名空间,IUserDao接口的路径 -->
<mapper namespace="org.example.dao.IUserDao">

    <!--抽取sql内容-->
    <sql id="selectUser">
         select * from user
    </sql>

    <!--查询所有用户 使用以上抽取的selectUser-->
    <!--resultType 返回值类型 , sql查询出来的数据封装到哪里-->
    <select id="findAll" resultType="User">
       <include refid="selectUser"></include>
    </select>

    <!--添加用户-->
    <!--parameterType 请求参数类型 取值使用#{}-->
    <insert id="saveUser" parameterType="user" >
        insert into user values(#{id},#{username},#{password},#{age})
    </insert>
</mapper>
```

​		创建sqlMapConfig.xml文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>

    <!--加载properties文件-->
    <properties resource="jdbc.properties"></properties>

    <!--给实体类的全类名生成别名-->
    <typeAliases>
        <!--给单独一个实体起别名-->
        <!-- <typeAlias type="org.example.pojo.User" alias="user"></typeAlias> -->
        <!--该包下所有实体类启别名,不区分大小写-->
        <package name="org.example.pojo"/>
    </typeAliases>

    <!--development:运行环境-->
    <environments default="development">
        <environment id="development">
            <!--当前事务交由JDBC进行管理-->
            <transactionManager type="JDBC"></transactionManager>
            <!--当前使用mybatis提供的连接池-->
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>

    <!--引入User映射配置文件-->
    <mappers>
        <mapper resource="UserMapper.xml"></mapper>
    </mappers>

</configuration>
```

​		创建dao层接口

```java
public interface IUserDao {

    //插入User信息
    public void saveUser(User user);

    //查询所有用户信息
    public List<User> findAll() throws IOException;

}
```

​		创建测试方法

```java
	@Test
    public void test1() throws IOException {
        //读取配置文件,把配置文件加载为输入流
        InputStream inputStream = Resources.getResourceAsStream("sqlMapConfig.xml");
        //解析输入流并创建sqlsession工厂对象
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        //创建openSession,可带参数autoCommit:true是否自动提交,默认为 false
        SqlSession sqlSession = sqlSessionFactory.openSession();

        IUserDao iUserDao = sqlSession.getMapper(IUserDao.class);
        User user = new User();
        user.setId(1);
        user.setUsername("QT_test");
        user.setPassword("123456");
        user.setAge(25);
        iUserDao.saveUser(user);
        //上面默认不自动提交,所以要手动commit
        sqlSession.commit();
    }
```

​		以上就是一个最基本的mybatis入门程序了,

​		这里只写了插入和查询语句,其实删除和修改都是类似的,就不反复写了



### 	Mybatis配置文件



## Spring

## SpringMVC

## SpringBoot

## JVM

## 设计模式

## MySQL

## Redis

## Zookeeper

## SpringCloud

## Netty

## kafka

## Nginx

## 常用工具

