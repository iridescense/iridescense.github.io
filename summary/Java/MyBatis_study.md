# MyBatis

## 1. 简介

### 1.1 什么是MyBatis

> MyBatis 是一款优秀的**持久层**框架，它支持自定义 SQL、存储过程以及高级映射。MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。

```xml
<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.7</version>
</dependency>

<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.25</version>
</dependency>

<!-- https://mvnrepository.com/artifact/junit/junit -->
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
</dependency>

```

### 1.2 持久层

数据持久化

- 持久化就是将程序的在持久状态和瞬时状态转化的过程

- 数据库(jdbc) io文件持久化

### 1.3 持久层

Dao层 service层, Controller层

## 2. 从XML重构建SqlSessionFactory

- 核心配置

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <environments default="development">
    <environment id="development">
      <transactionManager type="JDBC"/>
      <dataSource type="POOLED">
        <property name="driver" value="${driver}"/>
        <property name="url" value="${url}"/>
        <property name="username" value="${username}"/>
        <property name="password" value="${password}"/>
      </dataSource>
    </environment>
  </environments>
  <mappers>
    <mapper resource="org/mybatis/example/BlogMapper.xml"/>
  </mappers>
</configuration>

```

- 编写工具类

```java
package com.xiang.utils;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

//sqlSessionFactory --> sqlSesion
public class MybatisUtils {
    private static SqlSessionFactory sqlSessionFactory;
    static{
        try {
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        }catch (IOException e) {
            e.printStackTrace();
        }
    }
    //既然有了 SqlSessionFactory，顾名思义，我们可以从中获得 SqlSession 的实例。
    //SqlSession 提供了在数据库执行 SQL 命令所需的所有方法
    public static SqlSession getSqlSession(){
        return sqlSessionFactory.openSession();
    }
}
```

### 2.1编写代码

- 实体类

```java
package com.xiang.pojo;

public class User {
    private int id;
    private String account;
    private String password;
    private String name;
    private String info;

    public User(int id, String account, String password, String name, String info) {
        this.id = id;
        this.account = account;
        this.password = password;
        this.name = name;
        this.info = info;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getAccount() {
        return account;
    }

    public void setAccount(String account) {
        this.account = account;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getInfo() {
        return info;
    }

    public void setInfo(String info) {
        this.info = info;
    }
}
```



- Dao接口

```java
package com.xiang.dao;
import com.xiang.pojo.User;
import java.util.List;
public interface UserMapper {
    List<User> getUsers();
}
```

- 接口实现类(userMapper.xml)

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace=绑定一个Dao/Mapper接口-->
<mapper namespace="com.xiang.dao.UserMapper">
<!--    id对应方法名字-->
    <select id="getUsers" resultType="com.xiang.pojo.User">
        select * from scu.users
    </select>
    
    <select id="getUserById" parameterType="int" resultType="com.xiang.pojo.User"> 
        select * from scu.users where id = ${id}
    </select>
    
    <insert id="addUser" parameterType="com.xiang.pojo.User">
        <!--这里的sql数据可以直接从参数类中得出-->
        insert into scu.users valuses(${id},${name},${password});
    </insert>
</mapper>
```

### 2.2测试

注意点：

org.apache.ibatis.binding.BindingException: Type interface com.xiang.dao.UserMapper is not known to the MapperRegistry.

```xml
<mappers>
     <mapper resource="com/xiang/dao/UserMapper.xml"/>
</mappers>
```

Caused by: java.io.IOException: Could not find resource com/xiang/dao/UserMapper.xml

```xml

<!--在build中配置resources，来防止我们资源导出失败的问题-->
    <build>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>false</filtering>
            </resource>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
        </resources>
	</build>
```

**记得删除配置文件中的中文**

junit测试

```java
package com.xiang.dao;

import com.xiang.pojo.User;
import com.xiang.utils.MybatisUtils;
import org.apache.ibatis.session.SqlSession;
import org.junit.Test;
import java.util.List;
public class UserDaoTest {
    @Test
    public void test(){
        //第一步 获得SqlSession对象
        try(SqlSession sqlSession= MybatisUtils.getSqlSession()) {
            //执行
            //方式一：getMapper
            UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
            //增删改需要用 session.commit();来提交事务才能在数据库中生效
            List<User> userlist = userMapper.getUsers();
            for(User user : userlist) {
                System.out.println("user");
                
            }
        }
    }
}

```

