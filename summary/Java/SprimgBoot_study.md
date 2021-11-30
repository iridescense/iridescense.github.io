# SpringBoot_Study

## 原理初探

pom.xml

- spring-boot-dependencies: 核心依赖在父工程中

- 我们在写或者引入一些Springboot依赖的时候，不需要指定版本，就因为有版本仓库

启动器

- SpringBoot的启动环境
- SpringBoot会将所有的场景功能变成启动器
- 我们要什么功能，找到对应的启动器就行了

主程序

```java
@SpringBootApplication
public class Springboot01HelloworldApplication {

    public static void main(String[] args) {
        SpringApplication.run(Springboot01HelloworldApplication.class, args);
    }
}
```

- 注解

```java
@SpringBootConfiguration//: springboot的配置
	@Configuration  //spring配置类
	@Component   // 一个spring组件
@EnableAutoConfiguration //自动配置
    @AutoConfigurationPackage 、、自动配置包
        

```

### yaml语法

YAML 是 "YAML Ain't a Markup Language"（YAML 不是一种标记语言）的递归缩写。在开发的这种语言时，YAML 的意思其实是："Yet Another Markup Language"（仍是一种标记语言）。

YAML 的语法和其他高级语言类似，并且可以简单表达清单、散列表，标量等数据形态。它使用空白符号缩进和大量依赖外观的特色，特别适合用来表达或编辑数据结构、各种配置文件、倾印调试内容、文件大纲（例如：许多电子邮件标题格式和YAML非常接近）。

YAML 的配置文件后缀为 **.yml**，如：**runoob.yml** 。

#### 基本语法

- 大小写敏感
- 使用缩进表示层级关系
- 缩进不允许使用tab，只允许空格
- 缩进的空格数不重要，只要相同层级的元素左对齐即可
- '#'表示注释

#### 数据类型

YAML 支持以下几种数据类型：

- 对象：键值对的集合，又称为映射（mapping）/ 哈希（hashes） / 字典（dictionary）
- 数组：一组按次序排列的值，又称为序列（sequence） / 列表（list）
- 纯量（scalars）：单个的、不可再分的值

#### YAML 对象

对象键值对使用冒号结构表示 **key: value**，冒号后面要加一个空格。

也可以使用 **key:{key1: value1, key2: value2, ...}**。

还可以使用缩进表示层级关系；

```yaml
key: 
    child-key: value
    child-key2: value2
```

较为复杂的对象格式，可以使用问号加一个空格代表一个复杂的 key，配合一个冒号加一个空格代表一个 value：

```yaml
?  
    - complexkey1
    - complexkey2
:
    - complexvalue1
    - complexvalue2
```

意思即对象的属性是一个数组 [complexkey1,complexkey2]，对应的值也是一个数组 [complexvalue1,complexvalue2]

#### YAML 数组

以 **-** 开头的行表示构成一个数组：

```yaml
- A
- B
- C
```

YAML 支持多维数组，可以使用行内表示：

```yaml
key: [value1, value2, ...]
```

数据结构的子成员是一个数组，则可以在该项下面缩进一个空格。

```yaml
-
 - A
 - B
 - C
```

一个相对复杂的例子：

```yaml
companies:
    -
        id: 1
        name: company1
        price: 200W
    -
        id: 2
        name: company2
        price: 500W
```

意思是 companies 属性是一个数组，每一个数组元素又是由 id、name、price 三个属性构成。

数组也可以使用流式(flow)的方式表示：

```yaml
companies: [{id: 1,name: company1,price: 200W},{id: 2,name: company2,price: 500W}]
```

#### 复合结构

数组和对象可以构成复合结构，例：

```yaml
languages:
  - Ruby
  - Perl
  - Python 
websites:
  YAML: yaml.org 
  Ruby: ruby-lang.org 
  Python: python.org 
  Perl: use.perl.org
```

转换为 json 为：

```yaml
{ 
  languages: [ 'Ruby', 'Perl', 'Python'],
  websites: {
    YAML: 'yaml.org',
    Ruby: 'ruby-lang.org',
    Python: 'python.org',
    Perl: 'use.perl.org' 
  } 
}
```

#### 纯量

纯量是最基本的，不可再分的值，包括：

- 字符串
- 布尔值
- 整数
- 浮点数
- Null
- 时间
- 日期

使用一个例子来快速了解纯量的基本使用：

```yaml
boolean: 
    - TRUE  #true,True都可以
    - FALSE  #false，False都可以
float:
    - 3.14
    - 6.8523015e+5  #可以使用科学计数法
int:
    - 123
    - 0b1010_0111_0100_1010_1110    #二进制表示
null:
    nodeName: 'node'
    parent: ~  #使用~表示null
string:
    - 哈哈
    - 'Hello world'  #可以使用双引号或者单引号包裹特殊字符
    - newline
      newline2    #字符串可以拆成多行，每一行会被转化成一个空格
date:
    - 2018-02-17    #日期必须使用ISO 8601格式，即yyyy-MM-dd
datetime: 
    -  2018-02-17T15:02:31+08:00    #时间使用ISO 8601格式，时间和日期之间使用T连接，最后使用+代表时区
```

#### 引用

**&** 锚点和 ***** 别名，可以用来引用:

```yaml
defaults: &defaults
  adapter:  postgres
  host:     localhost

development:
  database: myapp_development
  <<: *defaults

test:
  database: myapp_test
  <<: *defaults
```

相当于:

```yaml
defaults:
  adapter:  postgres
  host:     localhost

development:
  database: myapp_development
  adapter:  postgres
  host:     localhost

test:
  database: myapp_test
  adapter:  postgres
  host:     localhost
```

**&** 用来建立锚点（defaults），**<<** 表示合并到当前数据，***** 用来引用锚点。

下面是另一个例子:

```
- &showell Steve 
- Clark 
- Brian 
- Oren 
- *showell 
```

转为 JavaScript 代码如下:

```yaml
[ 'Steve', 'Clark', 'Brian', 'Oren', 'Steve' ]
```

### yaml与类转换

pojo：

```java
package com.xiang.pojo;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;
import org.springframework.validation.annotation.Validated;

import javax.validation.constraints.Email;

@Validated
@Component
@ConfigurationProperties(prefix = "dog")
public class Dog {
    @Email(message = "aaaa")
    private String name;
    private Integer age;

    public Dog() {

    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public Dog(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Dog{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

yaml: 

```yaml
dog:
  name: wang才
  age: 3
```

`@ConfigurationProperties(prefix = "dog")`指定了前缀后自动装配

### 多环境配置及环境位置

见官方文档

### Thymeleaf

- 在html中加命名空间`xmlns:th="http://www.thymeleaf.org/"`

## SpringBoot Web开发

需要解决的问题：

- 导入静态资源
- 首页
- jsp，模板引擎
- 装配扩展SpringMVC
- 增删改查
- 拦截器
- 国际化

### 静态资源

```java
public void addResourceHandlers(ResourceHandlerRegistry registry) {
            if (!this.resourceProperties.isAddMappings()) {
                logger.debug("Default resource handling disabled");
            } else {
                this.addResourceHandler(registry, "/webjars/**", "classpath:/META-INF/reurces/webjars/");
                this.addResourceHandler(registry, this.mvcProperties.getStaticPathPattern(), (registration) -> {
                    registration.addResourceLocations(this.resourceProperties.getStaticLocations());
                    if (this.servletContext != null) {
                        ServletContextResource resource = new ServletContextResource(this.servletContext, "/");
                        registration.addResourceLocations(new Resource[]{resource});
                    }

                });
            }
        }
```

总结：

1. 在springboot中，我们可以使用以下方式处理静态资源
   - webjars		`localhost:8080/webjars/`
   - public，static，/**,resources    `localhost:8080/`

2. 优先级：resource>static(默认)>public

### 首页如何定制

命名为`index.html`再放入resource中，就可以自动访问

### 模板引擎

thymeleaf

```java
private String prefix = "classpath:/templates/";
private String suffix = ".html";
```

### mvc扩展

```java
package com.xiang.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.View;
import org.springframework.web.servlet.ViewResolver;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

import java.util.Locale;

/**
 *如果想diy一些定制化的功能，只要写这个组件，然后将它交给springboot，springboot就是会帮我们子自动配置
 * 扩展 springmvc  dispatchservlet
 **/
@Configuration
public class MyMvcconfig implements WebMvcConfigurer {
    /**
     *     ViewResolver 实现了视图解析器为接口的类，我们就可以把它看成视图解析器
     */
    @Bean
    public ViewResolver myViewResolver() {
        return new MyViewResolver();
    }
    public static class MyViewResolver implements ViewResolver {
        @Override
        public View resolveViewName(String s, Locale locale) throws Exception {
            return null;
        }
    }
}
```



在springboot中，有非常多的 xxxx Configuration帮助我们进行扩展

配置，只要看见了这个东西，我们就要注意了

### 国际化语言翻译

1. 首页配置：

   - 注意点，所有页面的静态资源都需要使用thymeleaf接管；
   - url：@{}

2. 页面国际化：

   - 我们需要配置i18n文件
   - 如果需要在项目中进行按钮自动切换，我们需要自定义一个组件`LocaleResolver`
   - 记得将自己写的组件配置到spring容器中 @Bean
   - #{}

3. 登录+拦截器

   ```java
   //向spring中注册拦截器
   @Override
       public void addInterceptors(InterceptorRegistry registry) {
           registry.addInterceptor(new LoginHandlerIntercepter()).addPathPatterns("/**").excludePathPatterns("/index.html","/","/user/login","/assets/**");
       }
   ```

   ```java
   
   //利用session实现拦截器
   package com.xiang.config;
   import org.springframework.web.servlet.HandlerInterceptor;
   
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   
   public class LoginHandlerIntercepter implements HandlerInterceptor{
       @Override
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           //登录成功 有用户的session
           Object loginUser= request.getSession().getAttribute("loginUser");
           if(loginUser == null){
               request.setAttribute("msg","没有全限，请先登录");
               request.getRequestDispatcher("/index.html").forward(request,response);
           }else {
               return true;
           }
           return false;
       }
   }
   ```

4. 员工列表展示

   - 提取公告页面
     1. `th:fragment="sidebar"`
     2. `th:replace="~{commons/commons::topbar}"`
     3. 如果要传参数，可以直接使用（）传参，接受判断即可
   - 列表循环展示

5. 添加员工

   - 按钮提交
   - 跳转到添加页面
   - 添加员工成功
   - 返回首页

6. CRUD

7. 404

前端：

- 模板：别人写好的，我们拿来改成自己需要的
- 框架：组件：自己手动拼接！ Bootstrap,Layui,semantic-ui
  - 栅格系统
  - 导航栏
  - 侧边栏
  - 表单

### 如何写一个前端

1. 前端搞定：页面长什么样子： 数据
2. 设计数据库（数据库设计难点）
3. 前端让他能自动运行，独立化工程
4. 数据库如何对接：json 对象all in one
5. 前后端联调测试



1. 有一套自己熟悉的后台模板：工作必要！ x-admin
2. 前端页面：至少自己能够通过前端框架，组合出来一个网站页面
   - index
   - about
   - blog
   - post
   - user
3. 让这个网站能独立运行

一个月

### 总结

- springBoot是什么
- 微服务
- HelloWorld
- 探究源码—~自动装配原理
- 配置 yaml
- 多文档环境切换
- 静态资源映射
- Thymeleaf th:xxx
- SpringBoot 如何扩展MVC   javaconfig
- 如何修改SpringBoot的默认配置
- CRUD
- 国际化
- 拦截器
- 定制首页，错误页~

## Data

### 基本的JDBC

- 相关依赖：

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <scope>runtime</scope>
</dependency>
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-jdbc</artifactId>
</dependency>
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
```

- 相关配置

```yaml
spring:
  datasource:
    username: root
    password: xiangzi033816
    url: jdbc:mysql://localhost:3306/scu?useUnicode=true&charset=utf-8
    driver-class-name: com.mysql.cj.jdbc.Driver
```

- 使用(使用template效率更高)

```java
package com.xiang.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;
import java.util.Map;

@RestController
public class JDBCcontroller {
    @Autowired
    JdbcTemplate jdbcTemplate;

    //查询数据库的所有信息
    //没有实体类，数据库中的东西，怎么获取？ map
    @GetMapping("/userList")
    public List<Map<String, Object>> userList(){
        String sql = "select * from users";
        List<Map<String, Object>> maps = jdbcTemplate.queryForList(sql);
        return maps;
    }

    @GetMapping("updateUser/{id}")
    public String updateUser(@PathVariable("id") int id){
        String sql = "update scu.users set name=?,pwd=? where id="+id;
        Object[] objects = new Object[2];
        objects[0] = "小米";
        objects[1] = "zzzzzz";
        jdbcTemplate.update(sql,objects);
        return "update-ok";
    }
}
```

### 加入Druid

- 相关依赖

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.2.6</version>
</dependency>
```

- 相关配置（添加application.yml

```yaml
type: com.alibaba.druid.pool.DruidDataSource
filters: stat,wall
```

- 相关类配置(因为取消了web.xml之类的配置文件)

```java
package com.xiang.config;

import com.alibaba.druid.pool.DruidDataSource;
import com.alibaba.druid.support.http.StatViewServlet;
import com.alibaba.druid.support.http.WebStatFilter;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.boot.web.servlet.FilterRegistrationBean;
import org.springframework.boot.web.servlet.ServletRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.sql.DataSource;
import java.util.HashMap;
import java.util.Map;

@Configuration
public class DruidConfig {
    @ConfigurationProperties(prefix = "spring.datasource")
    @Bean
    public DataSource druidDataSource(){
        return new DruidDataSource();
    }

    //后台监控、
    /**
     *     因为SpringBoot内置了servlet容器 所以没有web。xml 代替方法ServletRegistrationBean
     */
    @Bean
    public ServletRegistrationBean statViewServlet(){
        ServletRegistrationBean<StatViewServlet> bean= new ServletRegistrationBean<>(new StatViewServlet(),"/druid/*");

        //后台需要有人登录，账号密码
        Map<String, String> initParams = new HashMap<>();
        //增加配置 key是固定的
        initParams.put("loginUsername","admin");
        initParams.put("loginPassword","123456");

        //允许谁可以访问
        initParams.put("allow","");

        //禁止谁访问
        //initParams.put("xiangzi","ip");

        /**
         * 初始化参数
         **/
        bean.setInitParameters(initParams);
        return bean;
    }

    @Bean
    public FilterRegistrationBean webStatFilter(){
        FilterRegistrationBean bean = new FilterRegistrationBean();

        bean.setFilter(new WebStatFilter());

        //可以过滤哪些请求呢

        Map<String,String> initParams = new HashMap<String,String>();
        initParams.put("exclusions","*.js,.*css,/druid/*");

        bean.setInitParameters(initParams);
        return  bean;
    }
}
```

### Mybatis

整合包

mybatis-spring-boot-starter

> 在resources>mybatis>mapper进行Mapper配置

1. 导入包

```xml
<!-- https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot-starter -->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.2.0</version>
</dependency>

```



1. 配置文件
2. mybatis配置

```properties
mybatis.type-aliases-package=com.xiang.pojo
mybatis.mapper-locations=classpath:mapper/*.xml
```



1. 编写sql
2. 业务层调用dao
3. controller调用servce层

## SpringSecurity

- 功能权限
- 访问权限
- 菜单权限
- --拦截器，过滤器：大量原生代码，，，冗余

几个重要类

- WebSecurityConfigurerAdapter: 自定义Security策略
- AuthenticationManagerBuilder: 自定义认证策略
- @EnableWebSecurity: 开启WebSecurity模式

Spring Security的两个主要目标是"认证"和"授权"(访问控制)。

"认证"（Authentication）

"授权"（Authorization）

### 代码

```java
package com.xiang.config;


import org.apache.tomcat.util.security.MD5Encoder;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;

//AOP
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    //链式编程
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        //首页所有人可以访问，功能页只有对应有权限的人擦能
        http.authorizeRequests()
                .antMatchers("/").permitAll()
                .antMatchers("/level1/**").hasRole("vip1")
                .antMatchers("/level2/**").hasRole("vip2")
                .antMatchers("/level3/**").hasRole("vip3");

        /**
         * 没有权限默认开启登录页面
         */
        http.formLogin().loginPage("/toLogin").usernameParameter("user").passwordParameter("pwd").loginProcessingUrl("/login");
        /*
        注销，开启了注销功能
         */
        http.rememberMe().rememberMeParameter("remember");
        http.logout().logoutSuccessUrl("/");
    }


    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {

        //这些数据正常在数据库中
        //认证、
        //密码编码 PasswordEncoder
        //在Spring Security 5.0+ 新增了很多的加密方法
        auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
                .withUser("xiangzi").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2")
                .and()
                .withUser("root").password(new BCryptPasswordEncoder().encode("123456")).roles("vip1","vip2","vip3");
    }
}
```

主要步骤是：

- 创建一个Config类
- 开启安全模式`@EnableWebSecurity`
- 继承`WebSecurityConfigurerAdapter`
- 根据不同的功能，覆写不同的configure函数

tips：

1. 如果想用自己的登录页面，可以改变`http.formLogin().loginPage("/toLogin");`
2. 记得在登录提交的url也改变成一样的url

## Shiro

### HelloWorld

- 导入依赖
- 配置文件
- HelloWorld

```java
Subject currentUser = SecurityUtils.getSubject();
Session session = currentUser.getSession();
currentUser.isAuthenticated();
currentUser.getPrincipal();
currentUser.hasRole("schwartz");
currentUser.isPermitted("lightsaber:wield");
currentUser.logout();
```

### SpringBoot中集成

- 依赖

```xml
<!-- https://mvnrepository.com/artifact/org.apache.shiro/shiro-spring-boot-web-starter -->
        <dependency>
            <groupId>org.apache.shiro</groupId>
            <artifactId>shiro-spring-boot-web-starter</artifactId>
            <version>1.7.1</version>
        </dependency>
```

- 创建一个realm对象(UserRealm.java)

```java
package com.xiang.config;

import com.xiang.pojo.UserLogin;
import com.xiang.service.UserLoginServiceImpl;
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.*;
import org.apache.shiro.authz.AuthorizationInfo;
import org.apache.shiro.authz.SimpleAuthorizationInfo;
import org.apache.shiro.realm.AuthorizingRealm;
import org.apache.shiro.subject.PrincipalCollection;
import org.apache.shiro.subject.Subject;
import org.springframework.beans.factory.annotation.Autowired;

public class UserRealm extends AuthorizingRealm {
    @Autowired
    UserLoginServiceImpl userLoginService;
    //授权
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        System.out.println("执行了授权=》》》》》》");
        SimpleAuthorizationInfo info = new SimpleAuthorizationInfo();
        Subject subject = SecurityUtils.getSubject();
        UserLogin  currentUser = (UserLogin) subject.getPrincipal();
        String perms = "";
        if(currentUser.getPerms() != null){
            perms = currentUser.getPerms();
        }
        info.addStringPermission(perms);
        return info;
    }

    //认证
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
        System.out.println("执行了认证=》》》》》》");
        UsernamePasswordToken userToken = (UsernamePasswordToken) authenticationToken;
        String name = userToken.getUsername();
        //和数据库对比	
        UserLogin user = userLoginService.queryUserByName("'"+name+"'"    );
        if(user == null){
            return null;
        }
        String password = user.getPassword();
        return new SimpleAuthenticationInfo(user,password,"");
    }
}

```

- shiro核心配置(ShiroConfig)

```java
    package com.xiang.config;

    import at.pollux.thymeleaf.shiro.dialect.ShiroDialect;
    import org.apache.shiro.spring.web.ShiroFilterFactoryBean;
    import org.apache.shiro.web.mgt.DefaultWebSecurityManager;
    import org.springframework.beans.factory.annotation.Qualifier;
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;

    import java.util.HashMap;
    import java.util.LinkedHashMap;
    import java.util.Map;

    @Configuration
    public class ShiroConfig {

        /**
         * ShiroFilterFactoryBean
         */
        @Bean
        public ShiroFilterFactoryBean shiroFilterFactoryBean(@Qualifier("getDefaultWebSecurityManager") DefaultWebSecurityManager defaultWebSecurityManager){
            ShiroFilterFactoryBean bean = new ShiroFilterFactoryBean();
            /*
            设置安全管理器
             */
            //shiro的内置过滤器
            /*
                anon:无需认证就可以访问
                authc: 必须认证了才能访问
                user : 必须拥有 remember me 功能
                perms: 拥有对某个资源的权限才能访问:
                role: 拥有某个角色权限
             */
            bean.setSecurityManager(defaultWebSecurityManager);
            Map<String, String> filterChainDefinitionMap = new LinkedHashMap<String, String>();
            //授权，正常情况下，没有授权会跳转到未授权页面
            filterChainDefinitionMap.put("/user/add","perms[user:add]");
            filterChainDefinitionMap.put("/user/update","perms[user:update]");
            filterChainDefinitionMap.put("/user/*","authc");
            bean.setFilterChainDefinitionMap(filterChainDefinitionMap);

            //设置登录的请求
            bean.setLoginUrl("/toLogin");
            bean.setUnauthorizedUrl("/noauth");
            return bean;
        }

        /**DefaultWebSecurityManager
         *
         */
        @Bean
        public DefaultWebSecurityManager getDefaultWebSecurityManager(@Qualifier("userRealm") UserRealm userRealm){
            DefaultWebSecurityManager webSecurityManager = new DefaultWebSecurityManager();
            //关联rea了m
            webSecurityManager.setRealm(userRealm);
            return webSecurityManager;
        }

        /**
         * 创建 realm 对象
         */
        @Bean(name = "userRealm")
        public UserRealm userRealm() {
            return new UserRealm();
        }

        /*
         *  整合ShiroDialect: 用来整合shiro thymeleaf
         */
        @Bean
        public ShiroDialect getShiroDialect() {
            return new ShiroDialect();
        }
    }

```

- 响应提交（在Controller中请求，利用Subject进行提交

```java
package com.xiang.controller;

import com.xiang.pojo.UserLogin;
import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.AuthenticationException;
import org.apache.shiro.authc.IncorrectCredentialsException;
import org.apache.shiro.authc.UnknownAccountException;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.subject.Subject;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class MyController {
    @RequestMapping({"/","index"})
    public String toIndex(Model model){
        model.addAttribute("msg","hello,shiro");
        return "index";
    }
    @RequestMapping("user/add")
    public String addUser(){
        return "user/add";
    }
    @RequestMapping("user/update")
    public String updateUser(){
        return "user/update";
    }

    @RequestMapping("/toLogin")
    public String toLogin(){
        return "login";
    }
    @RequestMapping("/login")
    public String login(UserLogin userLogin,Model model){
        System.out.println(userLogin.toString());
        //获得当前用户
        Subject subject = SecurityUtils.getSubject();
        //封装用户的登录数据
        UsernamePasswordToken token = new UsernamePasswordToken(userLogin.getUsername(),userLogin.getPassword());
        try {
            subject.login(token);
            return "index";
        } catch (UnknownAccountException e) {
            model.addAttribute("msg","用户名不对");
            return "login";
        } catch (IncorrectCredentialsException e){
            model.addAttribute("msg","密码错误");
            return "login";
        }

    }
}
```

- Shiro和thymeleaf整合

```xml
<dependency>
            <groupId>com.github.theborakompanioni</groupId>
            <artifactId>thymeleaf-extras-shiro</artifactId>
            <version>2.1.0</version>
        </dependency>
```

```html
xmlns:shiro="http://www.pollix.at/thymeleaf/shiro"
```

## Swagger

### 简介

- 号称世界上最流行的Api框架
- RestFul Api 文档在线自动生成工具=》Api和Api定义同步更新
- 可以在线测试Api接口
- 支持多种语言：java php



在项目使用Swagger需要SpringFox：

- swagger2
- ui



### SpringBoot集成

- 导入依赖

```xml
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-boot-starter -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-boot-starter</artifactId>
    <version>3.0.0</version>
</dependency>
```

- 配置swagger->config	

  - Swagger的Bean实例Docket：

  ```java
  package com.xiang.config;
  
  import org.springframework.context.annotation.Bean;
  import org.springframework.context.annotation.Configuration;
  import springfox.documentation.oas.annotations.EnableOpenApi;
  import springfox.documentation.service.ApiInfo;
  import springfox.documentation.service.Contact;
  import springfox.documentation.spi.DocumentationType;
  import springfox.documentation.spring.web.plugins.Docket;
  
  import java.util.ArrayList;
  
  /**
   *  开启Swagger
   */
  @Configuration
  @EnableSwagger2  //开启Swagger
  public class SwaggerConfig {
      @Bean
      public Docket getDocket() {
          return new Docket(DocumentationType.SWAGGER_2)
                  .apiInfo(getApiInfo());
      }
  
      private ApiInfo getApiInfo() {
          Contact contact = new Contact(
                  "xiang", 
                  "https://iridescense.github.io/", 
                  "2547989204@qq.com");
          ApiInfo apiInfo = new ApiInfo(
                  "香的Swagger文档",
                  "呜呜呜呜", 
                  "1.0",
                  "https://iridescense.github.io/", 
                  contact, "Apache 2.0",
                  "http://www.apache.org/licenses/LICENSE-2.0",
                  new ArrayList());
          return apiInfo;
      }
  }
  ```

  

- Swagger配置扫描接口 Docket.select()

```java

@Bean
    public Docket getDocket() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(getApiInfo())
                .select()
                //RequestHandlerSelectors 配置要扫描接口的方式
                //basePackage 指定要扫描的包
                //any() 全部
                // none()  不扫描
                // withClassAnnotation 扫描类上的注解，参数是一个注解的反射
                // withMethodAnnotation 扫描方法上的注解
                .apis(RequestHandlerSelectors.basePackage("com.xiang.controller"))
                .paths(PathSelectors.ant("/xiang/**"))
                .build();
    }
```

- 配置swagger是否开启`.enable(false)`

- 根据生产环境是否启动swagger

```java

    @Bean
    public Docket getDocket(Environment environment) {
        //设置要显示的Swagger环境
        Profiles profiles = Profiles.of("dev","test");
        //通过environment.acceptsProfiles(profiles); 判断自己是否在设定的环境中
        boolean flag =  environment.acceptsProfiles(profiles);
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(getApiInfo())
                .enable(flag)
                .select()
                //RequestHandlerSelectors 配置要扫描接口的方式
                //basePackage 指定要扫描的包
                //any() 全部
                // none()  不扫描
                // withClassAnnotation 扫描类上的注解，参数是一个注解的反射
                // withMethodAnnotation 扫描方法上的注解
                .apis(RequestHandlerSelectors.basePackage("com.xiang.controller"))
                //.paths(PathSelectors.ant("/xiang/**"))
                .build();
    }
```

- 配置文档的分组

```java
.groupName("xiang")
```

​	如何配置多个分组：多个Docket实例即可

- 配置实体类

```java
package com.xiang.pojo;

import io.swagger.annotations.ApiModel;
import io.swagger.annotations.ApiModelProperty;

@ApiModel("用户实体类")
public class User {
    @ApiModelProperty("用户名")
    private String username;
    @ApiModelProperty("用户密码")
    private String password;

    public User() {
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public User(String username, String password) {
        this.username = username;
        this.password = password;
    }
}

```

总结：

1. 我们可以通过Swagger给一些比较难理解的属性或接口，增加注释信息
2. 接口文档实时更新
3. 可以在线测试

Swagger是一个优秀的工具

注意：在正式发布的时候，关闭Swagger





## 任务

### 异步任务

`@Async`

### 邮件任务

- 引入依赖

```xml
		<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-mail</artifactId>
        </dependency>
```

- 条件配置	

```properties
spring.mail.host=smtp.qq.com
#QQ必须开启加密验证
spring.mail.properties.mail.smtp.ssl.enable=true;
spring.mail.username=2547989204@qq.com
spring.mail.password=iwillback
```

- 添加内容

```java
package com.xiang;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.mail.SimpleMailMessage;
import org.springframework.mail.javamail.JavaMailSender;
import org.springframework.mail.javamail.MimeMessageHelper;

import javax.mail.MessagingException;
import javax.mail.internet.MimeMessage;

import java.io.File;

import static javafx.scene.input.KeyCode.M;

@SpringBootTest
class SpringbootMailApplicationTests {
    @Autowired
    JavaMailSender mailSender;
    @Test  //简单
    void contextLoads() {
        SimpleMailMessage message = new SimpleMailMessage();
        message.setSubject("hhhhhhhh");
        message.setText("xxxxxxxxxx");
        message.setTo("2547989204@qq.com");
        message.setFrom("2547989204@qq.com");
        mailSender.send(message);
    }

    @Test //复杂
    void contextLoads2() throws MessagingException {
        MimeMessage message = mailSender.createMimeMessage();
        //组装
        MimeMessageHelper helper = new MimeMessageHelper(message,true);

        //正文
        helper.setSubject("gggg");
        helper.setText("ssssss");

        //附件
        helper.addAttachment("1.jpg",new File("1.jpg"));

        helper.setTo("xianglcx@qq.com");
        helper.setFrom("xianglcx@qq.com");

        mailSender.send(message);
    }
}
```



### 定时任务

启动类上添加`@EnableScheduling`开启定时功能

在Service的方法上添加`@Scheduled`在一个特定的时间执行  cron表达式

## 分布式Dubbo+Zookeeper+SpringBoot

