

# Spring_study

## 1.1学习资料

[官方学习网站](https://docs.spring.io/spring-framework/docs/current/reference/html/)

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.3.9</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.3.9</version>
</dependency>

```

## 1.2 优点

- Spring 是一个开源的免费的框架
- Spring是一个轻量级的，非入侵式的框架
- 控制反转（IOC），面向切面编程（AOP）
- 支持事务的处理和对框架整合的支持

## 2.IOC理论推导

1.UserDao 接口

2.UserDaoImpl 实现类

3.UserService 业务接口

4.UserService 业务实现类



在之前的业务中，用户的需求可能会影响我们原来的代码，我们需要根据用户的需求去改源代码；如果代码量十分大，修改一次的代价十分昂贵



我们实现一个Set接口

```java
    private UserDao userDao;

    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

```



- 之前，程序是主动创建对象，控制权在程序源
- 现在是被动接受，程序不再具有主动性

这种思想从本质上解决了问题，不用再去管理对象的创建了，使注意力转到实现上。这是IOC的原型！

![image-20210803162508071](.\images\image-20210803162508071.png)

  

## IOC本质

.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">
<!--   使用Spring创建对象-->
    <bean id = "hello" class = "com.xiang.pojo.Hellow">
        <property name = "name" value = "xiangzi"/>
        <!--ref：引用Spring容器中创建好的对象-->
    </bean>

</beans>
```

```java
 ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");

```

### IOC创建对象方式

1.**Constructor argument type matching**

```xml
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg type="int" value="7500000"/>
    <constructor-arg type="java.lang.String" value="42"/>
</bean>
```

2.**Constructor argument index**

```xml
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg index="0" value="7500000"/>
    <constructor-arg index="1" value="42"/>
</bean>
```

3.**Constructor argument name**

```xml
<bean id="exampleBean" class="examples.ExampleBean">
    <constructor-arg name="years" value="7500000"/>
    <constructor-arg name="ultimateAnswer" value="42"/>
</bean>
```

4.可以在bean中加入scope=prototype实现多列

## Spring配置

### 别名 alias

```xml
<!--可以通过别名获得bean-->
<alias name="user" alias="nusdf"/>

```

### Bean配置

```xml


<!-- id 使bean的唯一标识符    class bean对象所对应的全限定名 ： 包名+类型-->
<!-- name 也是别名 也可以取多个别名-->
```

### Import配置

一般用于团队开发使用，可以将多个配置文件，导入合并为一个

```xml
<import source=""/>
```

## 依赖注入

### 构造器注入

### set方式注入【重点】

- 依赖注入：set注入
  - 依赖：bean对象的创建依赖容器
  - 注入：bean对象中的所有属性，由容器来注入

【环境搭建】

1.复杂类型

2.真实测试对象

```java
    private String name;
    private Address address;
    private String[] books;
    private List<String> hobbys;
    private Map<String,String> card;
    private Set<String> game;
```

```xml
    <bean id="student" class="com.xiang.pojo.Student">
<!--        第一种 普通值注入-->
        <property name="name" value="xiangzi"/>
<!--        第二种 Bean注入-->
        <property name="address" ref="address"/>
<!--        数组注入-->
        <property name="books" >
            <array>
                <value>红楼梦</value>
                <value>三国演义</value>
                <value>水浒传</value>
                <value>西游记</value>
            </array>
        </property>
<!--        list注入-->
        <property name="hobbys">
            <list>
                <value>听歌</value>
                <value>敲代码</value>
                <value>看电影</value>
            </list>
        </property>
<!--        Map注入-->
        <property name="card">
            <map>
                <entry key="cc" value="cc"/>
            </map>
        </property>
<!--        Set注入-->
        <property name="game">
            <set>
                <value>LoL</value>
            </set>
        </property>
<!--        Null注入-->
        <property name="wife">
            <null/>
        </property>
<!--        property注入-->
        <property name="info">
            <props>
                <prop key="学号">2020141530122</prop>
                <prop key="sex">男性</prop>
            </props>
        </property>
    </bean>

```

```
/*Student{name='xiangzi', address=Address{address='西安'},
        books=[红楼梦, 三国演义, 水浒传, 西游记],
         hobbys=[听歌, 敲代码, 看电影],
         card={cc=cc},
         game=[LoL],
          wife='null',
          info={学号=2020141530122, sex=男性}}
        */
```



### 拓展方式注入

```xml
    xmlns:p="http://www.springframework.org/schema/p"
    xmlns:c="http://www.springframework.org/schema/c"


    <!--p set注入-->
	<bean name="classic" class="com.example.ExampleBean">
        <property name="email" value="someone@somewhere.com"/>
    </bean>

    <bean name="p-namespace" class="com.example.ExampleBean"
        p:email="someone@somewhere.com"/>



    <!--constructor注入-->
<!-- traditional declaration with optional argument names -->
    <bean id="beanOne" class="x.y.ThingOne">
        <constructor-arg name="thingTwo" ref="beanTwo"/>
        <constructor-arg name="thingThree" ref="beanThree"/>
        <constructor-arg name="email" value="something@somewhere.com"/>
    </bean>

    <!-- c-namespace declaration with argument names -->
    <bean id="beanOne" class="x.y.ThingOne" c:thingTwo-ref="beanTwo"
        c:thingThree-ref="beanThree" c:email="something@somewhere.com"/>
```

### Bean的作用域

- 单例模式（默认）



![image-20210804105626528](.\images\image-20210804105626528.png)

- 原型模式（scope=”prototype“）

![image-20210804105650650](.\images\image-20210804105650650.png)

### Bean自动装配

- 自动装配式Spring满足Bean依赖的一种方式！
- Spring会在上下文中自动寻找，并自动给bean装配属性！



在Spring中有三种自动装配方式

​	1.在xml中显示的配置

​	2.在Java中显示配置

​	3.隐式的自动装配bean【importance】



### ByName自动装配

```xml
<bean id="dog" class="com.xiang.pojo.Dog"/>
    <bean id="cat" class="com.xiang.pojo.Cat"/>
    <bean id="people" class="com.xiang.pojo.People" autowire="byName" >
        <property name="name" value="xiang"/>
    </bean>
```

### ByType自动装配

```xml
<bean id="dog" class="com.xiang.pojo.Dog"/>
    <bean id="cat" class="com.xiang.pojo.Cat"/>
    <bean id="people" class="com.xiang.pojo.People" autowire="byType" >
        <property name="name" value="xiang"/>
    </bean>
```

小结：

- byname的时候，需要保证所有bean的id唯一，并且这个bean需要和自动注入的属性的set方法的值一致
- bytype的时候，需要包装所有的bean的class唯一，并且这个bean需要和自动注入的属性的类型一致

### 注解实现自动装配

使用注解须知

1. 导入约束: context 约束
2. 配置注解的支持：==<context:annotation-config/>==

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

</beans>
```

 **@Autowired**

直接上属性上使用即可，也可以在set方式上使用，默认为By

使用Autowired可以不用编写Set方法，前提是这个自动装配的属性在IOC容器中，且符合名字 ByName；

科普：

```xml
@Nullable 字段标记了这个 说明这个字段可以为null
```

```java
public @interface Autowired {
    boolean required() default true;
}
```

测试代码

```java
public class People {
    //如果显示定义了Autowired的required属性为false,说明这个对象可以为null,否则不允许为空
    @Autowired(required = false)
    private Cat cat;
    @Autowired
    private Dog dog;
    private String name;
}
```



如果@Autowired自动装配的环境比较复杂，自动装配无法通过一个注解完成时，可以使用@Qualifier(value="")去配置@Autowired的使用，指定一个唯一的bean对象注入 ！

```java
public class People {
    //如果显示定义了Autowired的required属性为false,说明这个对象可以为null,否则不允许为空
    @Autowired
    @Qualifier(value="dog222")
    private Cat cat;
    @Autowired
    private Dog dog;
    private String name;
}
```

**@Resource**



小结：

@Resource和@Autowired的区别：

- 都是用来自动装配，都可以放在属性字段上
- @Autowired通过Bytype的方式实现，且必须要求这个对象存在
- @Resource默认通过byname的方式实现，然后再通过type
- 执行顺序不同

### 使用注解开发

在Spring4之后，要使用注解开发，必须保证AOP的包导入了

使用注解需要context的约束，增加注解的支持

1. bean

   

2. 属性如何注入

```java

@Component
public class User{
	public String name;
    
    //相当于<property name="name" value="xiangzi"/>
    @Value("xiangzi")
    public void setName(String name){
		this.name = name;
    }
}
```



1. 衍生的注解

   @Component有几个衍生注解，我们在web开发中 ， 会按照mvc三层架构分层

   - dao [@Repository]

   - service [@Service]

   - controller [@Controller]

     这四个注解功能都是一样的，都代表将投个注册类放在容器中

2. 自动装配置

3. 作用域

   @Scope("singleton")  @Scope("prototype")

4. 小结 

   xml与注解：

   - xml更万能，适用于任何场合！维护简单方便
   - 注解 不是自己类使用不了

   xml与注解的最佳实践：

   - xml来管理bean
   - 注解只负责完成属性的注入
   - 我们在使用过程中，必须让注解生效，就必须开启注解支持

### 使用java的方式配置Spring

实体类

```java
package com.xiang.pojo;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

@Component
public class User {
    
    private String name;

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                '}';
    }

    public String getName() {
        return name;
    }
    @Value("xxxx")
    public void setName(String name) {
        this.name = name;
    }
}

```

配置文件

```java
package com.xiang.config;

import com.xiang.pojo.User;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

@Configuration
@ComponentScan("com.xiang.pojo")
@Import(XiangConfig2.class)
public class XiangConfig {

    @Bean
    public User getUser() {
        return new User();
    }
}
```

测试

```java
import com.xiang.config.XiangConfig;
import com.xiang.pojo.User;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Mytest {
    public static void main(String[] args) {
        ApplicationContext context = new 	AnnotationConfigApplicationContext(XiangConfig.class);
        User user = context.getBean("getUser",User.class);
        System.out.println(user.toString());
    }
}

```

## AOP

### 静态代理

角色分析：

- 抽象角色：一般会使用接口或者抽象类来解决
- 真实角色：被代理的角色
- 代理角色：代理真实角色，代理真实角色后，我们一般会做一些附属操作
- 客户：访问代理对象的人



代码步骤：

1. 接口
2. 真实角色
3. 代理角色
4. 客户端访问代理角色



代理模式的好处：

- 可以使真实角色的操作更加纯粹！不用去关注一些公共的业务
- 公告也就交给了代理角色，实现的业务的分工
- 公告业务发生拓展的时候，方便集中管理！
- 一个动态代理类代理的是一个接口，一般就是对应的一类业务

缺点：

- 一个真实角色就会产生一个代理角色，代码量就会翻倍，开发效率贬低

#### 代理理解

![image-20210805110803013](.\images\image-20210805110803013.png)

### 动态代理

- 动态代理和静态代理角色一样

- 动态代理的代理类是动态生成了，不是我们直接写好的

- 动态代理分为两大类：基于接口的动态代理，基于类的动态代理

  - 基于接口 --JDK 动态代理
  - 基于类：cglib
  - java字节码实现：javsist

  需要了解两个类：Proxy，InvocationHandler

动态代理代码

在运行期动态创建一个`interface`实例的方法如下：

1. 定义一个`InvocationHandler`实例，它负责实现接口的方法调用；

2. 通过

   ```
   Proxy.newProxyInstance()
   ```

   创建

   ```
   interface
   ```

   实例，它需要3个参数：

   1. 使用的`ClassLoader`，通常就是接口类的`ClassLoader`；
   2. 需要实现的接口数组，至少需要传入一个接口进去；
   3. 用来处理接口方法调用的`InvocationHandler`实例。

3. 将返回的`Object`强制转型为接口。

```java
//自动生成代理类
public class ProxyInvocationHandler implements InvocationHandler{
    //被代理的接口
    private Object target;
    
    public void setTarget(Object target){
        this.target = target;
    }
    
    //生成得到代理类
    public Object getProxy(){
		return Proxy.newProxyInstance(this.getClass().getClassLoader(),target.getClass().getInterfaces(),this);
    }
    
    //处理代理实例，并返回结果
    public Object invoke(Object proxy,Method method,Object[] args) throws Throwable {
        //代码
        Object result = method.invoke(target,args);
        //代码
        return result;
    }
}
```



```java
public class Client{
    public static void main(String[] args){
		//真实角色
        UserServiceImpl userService = new UserServiceImpl();
        //代理角色，未写
        ProxyInvocationHandler pih = new ProxyInvocationHandler();
        
        //设置要代理的对象
        pih.setTarget(userService);
        UserService proxy = (UserService)pih.getProxy();
        
        proxy.query();
    }
}
```

### Aop在Spring中的作用

- Aspect：切面，即一个横跨多个核心逻辑的功能，或者称之为系统关注点；
- Joinpoint：连接点，即定义在应用程序流程的何处插入切面的执行；
- Pointcut：切入点，即一组连接点的集合；
- Advice：增强，指特定连接点上执行的动作；
- Introduction：引介，指为一个已有的Java对象动态地增加新的接口；
- Weaving：织入，指将切面整合到程序的执行流程中；
- Interceptor：拦截器，是一种实现增强的方式；
- Target Object：目标对象，即真正执行业务的核心逻辑对象；
- AOP Proxy：AOP代理，是客户端持有的增强后的对象引用。

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-aop -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aop</artifactId>
    <version>5.3.9</version>
</dependency>

```

#### 方式一：Spring的接口

 接口

```java
package com.xiang.service;

public interface UserService {
    public void insert();
    public void delete();
    public void update();
    public void query();

}
```

实体类

```java
package com.xiang.service;

public class UserServiceImpl implements UserService {


    @Override
    public void insert() {
        System.out.println("增加了一个");
    }

    @Override
    public void delete() {
        System.out.println("删除了一个");

    }

    @Override
    public void update() {
        System.out.println("更新了一个");

    }

    @Override
    public void query() {
        System.out.println("查询了一个");

    }
}

```

切入类

```java
package com.xiang.log;

import org.springframework.aop.MethodBeforeAdvice;

import java.lang.reflect.Method;
//注意这里被继承了
public class Log implements MethodBeforeAdvice {
    @Override
    public void before(Method method, Object[] args, Object target) throws Throwable {
        System.out.println(target.getClass().getName() + "的" + method.getName() + "被执行了");
    }
}


package com.xiang.log;

import org.springframework.aop.AfterReturningAdvice;

import java.lang.reflect.Method;

public class AfterLog implements AfterReturningAdvice {
    @Override
    public void afterReturning(Object o, Method method, Object[] objects, Object o1) throws Throwable {
        System.out.println(o1.getClass().getName() + "的" + method.getName() + "被执行了" + "返回的结果为" + o);

    }
}
```

配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">


    <bean id="userService" class="com.xiang.service.UserServiceImpl"/>
    <bean id="log" class="com.xiang.log.Log"/>
    <bean id="afterLog" class="com.xiang.log.AfterLog"/>
<!--    配置AOP 需要导入aop的约束-->
<!--    方式一 使用原生的-->
    <aop:config>
<!--        切入点   expression：表达式 expression（修饰词 返回值 类名 方法名 参数）-->
        <aop:pointcut id="pointcut" expression="execution(* com.xiang.service.UserServiceImpl.*(..))"/>
<!--        执行环绕增强-->
        <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>
        <aop:advisor advice-ref="afterLog" pointcut-ref="pointcut"/>
    </aop:config>
</beans>
```

测试

```java
import com.xiang.service.UserService;
import com.xiang.service.UserServiceImpl;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Mytest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        //注意这儿代理的是接口
        UserService user = (UserService)context.getBean("userService");
        user.insert();
    }
}
```

#### 方法二：自定义实现AOP

切入类

```java
package com.xiang.div;

public class DiyPointCut {
    public void before() {
        System.out.println("kaisssssssssssssssssss");
    }
    public void after() {
        System.out.println("jiesssssssssssssssssss");
    }
}
```

配置

```xml
<aop:config>
<!--        自定义切面 ref 要引用的类-->
        <aop:aspect ref="diy">
<!--            切入点-->
            <aop:pointcut id="pointcut" expression="execution(* com.xiang.service.UserServiceImpl.*(..))"/>
<!--            通知-->
            <aop:before method="before" pointcut-ref="pointcut"/>
            <aop:after method="after" pointcut-ref="pointcut"/>
        </aop:aspect>
    </aop:config>
```

#### 方法三：使用注解的方式

## 整合Mybatis

步骤：

1. 导入相关jar包
   - junit
   - mybatis
   - mysql数据库
   - spring
   - aop织入
   - mybatis-spring【new】
2. 编写配置文件
3. 测试

### 学习mybatis

1. 编写实体类
2. 编写核心配置文件
3. 编写接口
4. 编写Mapper.xml
5. 测试  

### Mybatis-spring

1. 实体类

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

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", account='" + account + '\'' +
                ", password='" + password + '\'' +
                ", name='" + name + '\'' +
                ", info='" + info + '\'' +
                '}';
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

2. 接口

```java
package com.xiang.dao;
import com.xiang.pojo.User;
import org.apache.ibatis.annotations.Param;
import org.apache.ibatis.annotations.Select;

/**
 * @author 86153
 */
public interface UserMapper {
    @Select("SELECT * FROM scu.users WHERE id = #{userId}")
    User getUser(@Param("userId") String userId);
}
```

3. 配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">
<!--    DataSource:使用spring的数据源替换mybatis的配置  -->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/scu?useSSL=false&amp;useUnicode=true&amp;charsetEncoding=UTF-8&amp;serverTimezone=GMT"/>
        <property name="username" value="root"/>
        <property name="password" value="xiangzi033816"/>
    </bean>
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource" />
    </bean>
<!--    sqlSessionFactory -->
</beans>
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">
    <!--    DataSource:使用spring的数据源替换mybatis的配置  -->
    <import resource="spring_dao.xml"/>
    <bean id="userMapper" class="org.mybatis.spring.mapper.MapperFactoryBean">
        <property name="mapperInterface" value="com.xiang.dao.UserMapper" />
        <property name="sqlSessionFactory" ref="sqlSessionFactory" />
    </bean>

    <!--    sqlSessionFactory -->
</beans>
```

4. 测试

```java
import com.xiang.dao.UserMapper;
import com.xiang.pojo.User;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Mytest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserMapper userManager = context.getBean(UserMapper.class);
        User user = userManager.getUser("2");
        System.out.println(user.toString());
    }
}
```

## 声明式事务

事务ACID原则

- 原子性
- 一致性
- 隔离性
- 持久性

### spring中的事务管理

- 声明式事务：AOP 【不容马虎！！】

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"

       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">
<!--    DataSource:  -->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/scu?useSSL=false&amp;useUnicode=true&amp;charsetEncoding=UTF-8&amp;serverTimezone=GMT"/>
        <property name="username" value="root"/>
        <property name="password" value="xiangzi033816"/>
    </bean>
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource" />
    </bean>

<!--    statement for transactions-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <constructor-arg ref="dataSource" />
    </bean>
<!--    implementate the aspect of transactions with AOP-->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
<!--        what methods need to be implemented with transaction-->
        <tx:attributes>
            <tx:method name="add" propagation="REQUIRED"/>
            <tx:method name="query" read-only="true"/>
            <tx:method name="*"/>
        </tx:attributes>
    </tx:advice>
    <aop:config>
        <aop:pointcut id="txPointcut" expression="execution(* com.xiang.dao.*.*(..))"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointcut"/>
    </aop:config>
</beans>
```

开启事务再切入

- 编程式事务：需要在代码中，进行事务管理
