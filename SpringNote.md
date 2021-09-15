# 1.spring的学习

## 1.1、简介

- spring 春天 ---->给软件行业带来了春天
- spring的框架是由于软件开发的复杂性创建的 简化服务器的开发 简单 可测试 松耦合

- Rod Johnson Spring Framework创始人，著名作者。 Rod在悉尼大学不仅获得了计算机学位，同时还获得了音乐学位。更令人吃惊的是在回到软件开发领域之前，他还获得了音乐学的博士学位。 有着相当丰富的C/C++技术背景的Rod早在1996年就开始了对Java服务器端技术的研究

- Spring理念：使现有的技术更加容易使用，本身是一个大杂烩，整合了现有的技术框架！

[Spring官方地址](https://spring.io/)

[Spring 5.3.9 学习文档](https://docs.spring.io/spring-framework/docs/current/reference/html/)

## 1.2、优点

- Spring是一个开源的免费的框架（容器）！
- Spring是一个轻量级的，非入侵式的框架
- 控制反转（IOC），面向切面编程（AOP）
- 支持事务的处理，对框架整合的支持！
- Spring 使每个人都可以更快、更轻松、更安全地进行 Java 编程。Spring 对速度、简单性和生产力的关注使其成为世界上最受欢迎的Java框架。

> ```hmtl
> Spring就是一个轻量级的控制反转（IOC）和切面编程（AOP）的框架！
> ```

## 1.3、组成

![image-20210821163512319](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210821163512319.png)

**核心容器（Spring Core）**

　　核心容器提供Spring框架的基本功能。Spring以bean的方式组织和管理Java应用中的各个组件及其关系。Spring使用BeanFactory来产生和管理Bean，它是工厂模式的实现。BeanFactory使用控制反转(IoC)模式将应用的配置和依赖性规范与实际的应用程序代码分开。

**应用上下文（Spring Context）**

　　Spring上下文是一个配置文件，向Spring框架提供上下文信息。Spring上下文包括企业服务，如JNDI、EJB、电子邮件、国际化、校验和调度功能。

**Spring面向切面编程（Spring AOP）**

　　通过配置管理特性，Spring AOP 模块直接将面向方面的编程功能集成到了 Spring框架中。所以，可以很容易地使 Spring框架管理的任何对象支持 AOP。Spring AOP 模块为基于 Spring 的应用程序中的对象提供了事务管理服务。通过使用 Spring AOP，不用依赖 EJB 组件，就可以将声明性事务管理集成到应用程序中。

**JDBC和DAO模块（Spring DAO）**

　　JDBC、DAO的抽象层提供了有意义的异常层次结构，可用该结构来管理异常处理，和不同数据库供应商所抛出的错误信息。异常层次结构简化了错误处理，并且极大的降低了需要编写的代码数量，比如打开和关闭链接。

**对象实体映射（Spring ORM）**

　　Spring框架插入了若干个ORM框架，从而提供了ORM对象的关系工具，其中包括了Hibernate、JDO和 IBatis SQL Map等，所有这些都遵从Spring的通用事物和DAO异常层次结构。

**Web模块（Spring Web）**

　　Web上下文模块建立在应用程序上下文模块之上，为基于web的应用程序提供了上下文。所以Spring框架支持与Struts集成，web模块还简化了处理多部分请求以及将请求参数绑定到域对象的工作。

**MVC模块（Spring Web MVC）**

　　MVC框架是一个全功能的构建Web应用程序的MVC实现。通过策略接口，MVC框架变成为高度可配置的。MVC容纳了大量视图技术，其中包括JSP、POI等，模型来有JavaBean来构成，存放于m当中，而视图是一个街口，负责实现模型，控制器表示逻辑代码，由c的事情。Spring框架的功能可以用在任何J2EE服务器当中，大多数功能也适用于不受管理的环境。Spring的核心要点就是支持不绑定到特定J2EE服务的可重用业务和数据的访问的对象，毫无疑问这样的对象可以在不同的J2EE环境，独立应用程序和测试环境之间重用。

## 1.4 拓展

![image-20210821164211193](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210821164211193.png)

### 1.4.1 Spring Boot

- 一个快速开发的脚手架
- 基于Spring Boot可以快速开发单个微服务
- 约定大于配置（Maven）

### 1.4.2 Spring Cloud

- Spring cloud 是基于Spring Boot实现的

  因为现在公司都在使用Spring Boot进行快速开发，学习Spring Boot的前提 完全掌握Spring和Spring MVC!

### 1.4.3 弊端

​	发展太久之后，违背了原来的理念 配置十分繁琐 人称”**配置地狱**“

# 2.IOC

## 2.1.IOC理论推导

- 原来的实现方式

  1.用户接口

~~~java
public interface UserDao {
    void getUser();
}
~~~

​		2.用户实现类

~~~java
public class UserDaoImpl implements UserDao{
    public void getUser() {
        System.out.println("默认获取用户的数据");
    }
}
~~~

 		3.服务接口

~~~java
public interface UserService {
    void getUser();
}
~~~

​		4.服务实现类

~~~Java
public class UserServiceImpl implements UserService{

    private UserDao userDao = new UserDaoImpl();

    public void getUser() {
        userDao.getUser();
    }
}
~~~

- **弊端**

  如果需求需要改变，则需要每次都去修改 服务实现类中的 UserDao userdao = new XXXImpl();需要我们每次去修改，十分繁琐。

- **Set注入方法**

  ~~~java
  public class UserServiceImpl implements UserService{
  
      private UserDao userDao;
      public void setUserDao(UserDao userDao){
          this.userDao=userDao;
      }
      public void getUser() {
          userDao.getUser();
      }
  }
  ~~~

  - 之前程序通过 new的方式 主动去创建对象，控制权在程序员手里
  - 使用set注入后 程序不再具有主动性质，而是成为了被动的接收对象
  - 这种思想，从本质上解决了问题，不用去关注对象的创建，降低了耦合性

## 2.2.IOC的本质

![image-20210822090553934](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210822090553934.png)

**控制反转IOC（Inversion of Control），是一种设计思想，DI（依赖注入）是实现IOC的一种方法**， 也有人认为DI只是IOC的另一种说法。没有IOC的程序中，我们使用面向对象编程，对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方，个人认为所谓的控制反转就是：获得依赖的方式反转了。

采用XML方式配置Bean的时候，Bean的定义信息是和实现分离的，而采用注解的方式可以把两者合为一体，Bean的定义信息直接以注解的形式定义在实现类中，从而达到了零配置的目的。

**控制反转是一种通过描述（xml或注解）并通过第三方去生产或获取特定对象的方式。在spring中实现控制反转的是IoC容器，其实现方法是依赖注入（Dependency Injection，DI）**

**使用了Spring之后，不用再去程序中改动，要实现不同的操作，只需要再XML配置文件中进行修改，所谓的IOC就是：对象由Spring来创建，管理，装配。**

没有SetXxx方法就没有Spring的创建对象

## 2.3.IOC创建对象的方式

- 无参构造创建

~~~Java
    <bean id="user" class="com.heng.pojo.User">
    </bean>
~~~

- 有参构造创建
  - 索引下标（不推荐使用）
  - 变量类型（不推荐使用）
  - 变量名称（推荐使用）

~~~Java
 	<bean id="user" class="com.yang.entity.User">
        <constructor-arg index="0" value="张三"/>
        <constructor-arg index="1" value="18"/>
    </bean>
        
    <bean id="user1" class="com.yang.entity.User">
        <constructor-arg type="int" value="18"/>
        <constructor-arg type="java.lang.String" value="张三"/>
    </bean>
        
	<bean id="user2" class="com.yang.entity.User">
        <constructor-arg name="name" value="张三"/>
        <constructor-arg name="age" value="18"/>
    </bean>
~~~

## 2.4.注意点

==在获取spring的上下文对象（ new ClassPathXmlApplicationContext(“beans.xml”); ）时，spring容器中的所有的对象(即配置文件种写好的Bean标签)就已经被创建了。==

## 2.5.Spring配置说明

- 别名

  ~~~Java
  	<!--如果添加了别名，通过别名也可以获取对象-->
      <alias name="user" alias="userAlias"/>
  ~~~

- bean的配置

  ~~~java
  <!--
          id: bean的唯一标识符，也就是相当于我们学的对象名
          class： bean对象的全限定名：包名 + 类型
          name： 也是别名 而且name可以同时设置多个别名，可以用逗号 空格 分号隔开
      -->
      <bean id="user1" class="com.yang.entity.User" name="test test1, test2; test3"> 
          <constructor-arg type="int" value="18"/>
          <constructor-arg type="java.lang.String" value="张三"/>
      </bean>
  ~~~

  

- import

~~~java
import，一般用于团队开发使用，他可以将多个配置文件，导入合并为1个
假设，现在项目中又多个人开发，这三个人负责不同的类开发，不同的类需要注册在不同的bean中，我们可以用import将所有人的beans.xml合并为一个总的！
<import resource="applicationContext1.xml"/>
~~~

## 2.6.DI（Dependency Injection）注入

- DI注入常见的属性

  - String

    ~~~xml
     <property name="name" value="邢垆恒"/>
    ~~~

  - Class

    ~~~xml
    <property ref="address" name="address"/>
    //引用了一个类 因此还需要注册一个Bean
     <bean id="address" class="com.heng.pojo.Address">
       <property name="address" value="河北邯郸"/>
     </bean>
    ~~~

  - Map

    ~~~xml
     <property name="card">
       <map>
          <entry key="身份证" value="12456"/>
       </map>
      </property>
    ~~~

  - List

    ~~~xml
    <property name="hobbies">
        <list>
            <value>乒乓球</value>
            <value>编程</value>
        </list>
    </property>
    ~~~

  - Array

    ~~~xml
     <property name="books">
         <array>
             <value>红楼梦</value>
             <value>西游记</value>
         </array>
     </property>
    ~~~

  - Properties

    ~~~xml
    <property name="info">
        <props>
            <prop key="学号">1931030235</prop>
        </props>
    </property>
    ~~~

  - set

  ~~~xml
  <property name="games">
      <set>
          <value>LOL</value>
      </set>
  </property>
  ~~~

  - 空值注入

  ~~~xml
  <property name="wife">
      <null/>
  </property>
  ~~~

  

## 2.7.AtuoWiring

- 自动装配是spring满足bean依赖的一种方式
- Spring会在上下文中自动寻找，并自动给bean装配属性

在Spring中由三种装配方式

1. 在xml中显式配置
2. 在java中显式配置
3. 隐式的自动装配bean

<img src="C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210823193741492.png" alt="image-20210823193741492" style="zoom: 67%;" />

#### 2.7.1.ByName

- byName的时候，需要保证所有bean的id唯一，并且这个bean需要和自动注入的属性的set方法的值一致

  ==byName时候必须使用 id进行注册==

#### 2.7.2.ByType

- byType的时候，需要保证所有bean的class唯一，并且这个bean需要和自动注入的属性的类型一致

  ==byType时候必须用类进行注册，不推荐使用，当类型重复时，就会空指针异常==

#### 2.7.3.使用注解

- **@Autowired**
  直接在属性上使用即可！也可以在set方式上使用
  使用Autowired我们可以不用编写set方法了，前提是你这个自动装配的属性在IOC（Spring）容器中存在，且符合名字byName

  @Nullable 字段标记了这个注解，说明这个字段可以为null；

<img src="C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210823205226167.png" alt="image-20210823205226167" style="zoom:67%;" />

​		如果@Autowired自动装配的环境比较复杂，自动装配无法通过一个注解【@Autowired】完成的时候，我们		可以使用@Qualifier（value = “xxx”）去配置@Autowired的使用，指定一个唯一的bean对象注入！

- **@Resource注解**

  不指定name值，先去判断byName和byType，有一个能注入即成功

<img src="C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210823205519411.png" alt="image-20210823205519411" style="zoom:67%;" />

**小结：@Resource和@Autowired的区别**

- 都是用来自动装配的，都可以放在属性字段上
- @Autowiring通过ByType的方式实现，而且必须要求这个对象存在
- @Resource默认通过ByName的方式实现，如果找不到名字，则通过byType实现，如果两个都不存在则报错

- 执行顺序不同：@Autowired通过byType的方式实现。@Resource默认通过byName的方式实现。

## 2.8.使用注解开发

在Spring4之后，要使用注解开发，必须保证aop的包导入了

**1. bean注入使用@Componet注解**

~~~Java
//相当于之前的 <bean id="user" class="com.heng.pojo.User"/>
@Component
public class User {
    public String name;
}
~~~

**2. 属性注入使用@Value注解**

~~~Java
@Value("邢垆恒")
    public void setName(String name) {
        this.name = name;
    }
~~~

**3.衍生注解**
@Componet有几个衍生注解，我们在web开发中，会按照mvc三层架构分层！

dao层 【@Repository】
service层 【@Service】
controller层 【@Controller】
这四个注解功能都是一样的，都是代表将某个类注册到Spring中，装配Bean

**4.自动装配**

~~~java
@Autowired  自动装配通过类型、名字
			如果Autowired不能唯一自动装配上属性，则需要通过@Qualifier(value="xxx")
@Nullable   字段标记了这个注解，说明这个字段可以为null
@Resource 自动装配通过名字，类型
~~~

**5. 作用域**
@Scope(“singleton”)单例

6.**小结**

XML与注解的

- XML更加万能，使用于所有场合，维护方便
- 注解不是自己的类使用不了，维护比较繁琐，但相对简单

XML与注解的最佳实践

- XML用来管理Bean
- 注解用来属性的注入
- ==我们在使用过程中，只需要注意一个问题：必须让注解生效，就需要开启注解的支持==

~~~Java
    <context:component-scan base-package="com.heng"/>
    <context:annotation-config/>
~~~

# 3.AOP

为什么要学习代理模式：Spring中AOP的底层原理

## 3.1代理模式的分类

### 3.1.1.静态代理

**优点**

1、可以使真实的角色操作更加纯粹！不去关注一些公共的业务

2、公共的也就交了代理角色，实现了业务的分工

3、可以起到保护目标对象的作用。

4、可以对目标对象的功能增强，方便集中管理。

缺点

系统结构复杂，类的增多，因此有了通过反射来动态代理

**角色分析：**

- 抽象角色：一般会使用接口或者抽象类来解决
- 真实角色：被代理的角色
- 代理角色：代理真实角色，代理真实角色后，我们一般会做一些附属操作
- 客户：访问代理对象的人

**代码步骤：**

1. 接口

~~~java
public interface Rent {
    public void rent();
}
~~~

2.真实角色

~~~java
public class Host implements Rent {
    public void rent() {
        System.out.println("我要出租房子");
    }
}
~~~

3.代理角色

~~~java
public class Proxy {
    //中介代理房东 去出租房子
    private Host host;

    public void setHost(Host host) {
        this.host = host;
    }
    
    public Proxy(Host host) {
        this.host = host;
        fare();
        host.rent();
        heTong();
    }

    public void fare(){
        System.out.println("收取中介费");
    }

    public void heTong(){
        System.out.println("签合同");
    }
}
~~~

4.客户端访问代理角色

~~~java
@Test
    public void testProxying(){
        //房东来找中介
        Host host = new Host();
        //中介为房东代理
        Proxy proxy = new Proxy(host);
        //中介出租房子
    }
~~~

### 3.1.2.动态代理

- 动态代理和静态代理角色一样
- 动态代理类使动态生成的，不是外面直接写好的
- 动态代理分为两大类，居于接口的动态代理，基于类的动态代理
  - 基于接口 ：jdk
  - 基于类：cglib

需要了解两个类：Proxy，InvocationHandler 调用处理

## 3.2.什么是AOP

AOP（Aspect Oriented Programming）意为：面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术，AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生泛型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。


![image-20210825202503666](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210825202503666.png)

## 3.3. Aop在Spring中的作用

==提供生命事务：允许用户自定义切面==

- 横切关注点：跨越应用程序多个模块的方法或功能。即与我们的业务逻辑无关的，但是我们需要关注的部分，就是横切关注点。如日志，安全，缓存，事务等等。
- 切面（ASPECT）：横切关注点 被模块化的特殊对象。即 它是一个类
- 通知（Advice）：切面必须要完成的工作，即 他是类中的一个方法
- 目标（target）：被通知的对象
- 代理（Proxy）：向目标对象应用通知之后创建的对象
- 切入点（PointCut）：切面通知 执行的"地点"的定义
- 连接点（jointPoint）：与切入点匹配的执行点

<img src="C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210825202813851.png" alt="image-20210825202813851" style="zoom:67%;" />

SpringAop中，通过Advice定义横切逻辑，Spring中支持的5种类型的Advice

![image-20210825202846232](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210825202846232.png)

## 3.4. 使用Spring实现Aop

【重点】使用AOP织入，需要依赖包

~~~java
<dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.9.4</version>
</dependency>
~~~

**方式一：使用Spring的API接口**
eg:在执行UserService实现类的所有方法时，增加日志功能

UserServer接口

~~~java
package com.yang.service;
public interface UserService {
    public void add();
    public void update();
    public void delete();
    public void select();
}
~~~

UserServer实现类

~~~java
package com.yang.service;
public class UserServiceImpl implements UserService{
    public void add() {
        System.out.println("增加了一个用户");
    }
    public void update() {
        System.out.println("更新了一个用户");
    }
    public void delete() {
        System.out.println("删除了一个用户");
    }
    public void select() {
        System.out.println("检索了一个用户");
    }
}
~~~

Log类

~~~java
import org.springframework.aop.AfterReturningAdvice;
import org.springframework.aop.MethodBeforeAdvice;
import java.lang.reflect.Method;
public class Log implements MethodBeforeAdvice, AfterReturningAdvice {
    //method:要执行的目标对象的方法（method being invoked）
    //object:参数（args: arguments to the method）
    //o:目标对象 （target：target of the method invocation）
    public void before(Method method, Object[] args, Object target) throws Throwable {
        System.out.println(target.getClass().getName() + "的" + method.getName() + "被执行了");
    }
    //returnValue:返回值
    public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
        System.out.println("执行了" + method.getName() + "方法，返回值为" + returnValue);
    }
}
~~~

配置文件

~~~java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--注册bean-->
    <bean id="userService" class="com.yang.service.UserServiceImpl"/>
    <bean id="log" class="com.yang.log.Log"/>
    <!--方式：使用原生Spring Api接口-->
    <!--配置aop-->
    <aop:config>
        <!--切入点：execution:表达式，execution(*(修饰词) *(返回值) *(类名) *(方法名) *(参数))  ..任意参数-->
        <aop:pointcut id="pointcut" expression="execution(* 					   com.yang.service.UserServiceImpl.*(..))"/>
        <!--执行环绕增加-->
        <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>
    </aop:config>
</beans>

~~~

测试类

~~~java
import com.yang.service.UserService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
public class MyTest {
    public static void main(String[] args) {
            ApplicationContext classPathXmlApplicationContext = new 			ClassPathXmlApplicationContext("applicationContext.xml");
            UserService userService = classPathXmlApplicationContext.getBean("userService", UserService.class);
            userService.add();
    }
}
~~~

**方式二：自定义来实现AOP【主要是切面定义】**

第一步 : 写我们自己的一个切入类

~~~Java
public class DiyPointcut {
    public void before(){
        System.out.println("---------方法执行前---------");
    }
    public void after(){
        System.out.println("---------方法执行后---------");
    }
}
~~~



~~~java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--注册bean-->
    <bean id="userService" class="com.yang.service.UserServiceImpl"/>
    <bean id="log" class="com.yang.log.Log"/>
    <bean id="diy" class="com.yang.diy.DiyPointCut"/>
    <!--方式2：自定义类-->
    <aop:config>
    	<!--<aop:aspect ref="diy"> : 标注这个类为切面-->
        <aop:aspect ref="diy">
            <!--切入点-->
            <aop:pointcut id="point" expression="execution(* com.yang.service.UserServiceImpl.*(..))"/>
            <!--通知-->
            <aop:before method="beforeMethod" pointcut-ref="point"/>
        </aop:aspect>
    </aop:config>
</beans>

~~~

#### 第三种方式

**使用注解实现**

第一步：编写一个注解实现的增强类4. Mybatis和Spring的整合

~~~java
package com.kuang.config;
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
@Aspect
public class AnnotationPointcut {
    @Before("execution(* com.kuang.service.UserServiceImpl.*(..))")
    public void before(){
        System.out.println("---------方法执行前---------");
    }
    @After("execution(* com.kuang.service.UserServiceImpl.*(..))")
    public void after(){
        System.out.println("---------方法执行后---------");
    }
    @Around("execution(* com.kuang.service.UserServiceImpl.*(..))")
    public void around(ProceedingJoinPoint jp) throws Throwable {
        System.out.println("环绕前");
        System.out.println("签名:"+jp.getSignature());
        //执行目标方法proceed
        Object proceed = jp.proceed();
        System.out.println("环绕后");
        System.out.println(proceed);
    }
}
~~~

第二步：在Spring配置文件中，注册bean，并增加支持注解的配置

~~~java
<!--第三种方式:注解实现-->
<bean id="annotationPointcut" class="com.kuang.config.AnnotationPointcut"/>
<aop:aspectj-autoproxy/>
~~~



## 4.1整合方式一

1. **导入相关jar包**
   junit
   mybatis
   mysql数据库
   spring相关的
   aop织入
   mybatis-spring

<img src="C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210827170615516.png" alt="image-20210827170615516" style="zoom:67%;" />

​	注意：spring-jdbc 和 spring MVC 的本要一致

**2.编写配置文件**
spring-dao.xml配置数据源DataSource与sqlSession

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">
    
<!--    DataSource 使用Spring的数据源来替换Mybatis的配置 c3p0 dbcp druid-->
<!--    我们这里使用Spring 提供的JDBC org.springframework.jdbc.datasource-->
        <bean id="datasource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
            <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
            <property name="url" value="jdbc:mysql://localhost:3306/mybatis?userSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8&amp;serverTimezone=UTC"/>
            <property name="username" value="root"/>
            <property name="password" value="123456"/>
        </bean>
<!--    sqlSessionFactory-->
        <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
<!--            绑定Mybatis配置文件-->
            <property name="dataSource" ref="datasource"/>
            <property name="configLocation" value="classpath:mybatis-config.xml"/>
            <property name="mapperLocations" value="classpath:com/heng/dao/*.xml"/>
        </bean>
<!--只能用构造器注入参数 因为没有set方法-->
        <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
            <constructor-arg index="0" ref="sqlSessionFactory"/>
        </bean>
</beans>
~~~

**3.mybatis-config.xml配置一些mybatis专属配置**

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--configuration核心配置文件-->
<configuration>
    <typeAliases>
        <package name="com.heng.pojo"/>
    </typeAliases>
</configuration>
~~~

**4.applicationContext.xml整合与注册bean等**

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">
    <import resource="spring-dao.xml"/>
    <bean id="userMapper" class="com.heng.dao.UserMapperImpl">
        <property name="sqlSession" ref="sqlSession"/>
    </bean>
</beans>
~~~

**5.编写实现类**

~~~java
package com.heng.dao;

import com.heng.pojo.User;
import org.mybatis.spring.SqlSessionTemplate;

import java.util.List;

/**
 * @author xingluheng
 * @date 2021/8/27 15:58)
 */
public class UserMapperImpl implements UserMapper{

    //我们所有的操作都用SqlSession来执行  在现在我们使用SqlSessionTemplate
    private SqlSessionTemplate sqlSession;

    public void setSqlSession(SqlSessionTemplate sqlSession) {
        this.sqlSession = sqlSession;
    }

    public List<User> getUserList() {
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        return mapper.getUserList();
    }
}
~~~

**6.mapper映射文件**

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.heng.dao.UserMapper">
    <select id="getUserList" resultMap="users">
        select * from mybatis.user
    </select>

    <resultMap id="users" type="user">
    </resultMap>
</mapper>
~~~

**7.测试类**

~~~java
 @Test
    public void testEnvironment2(){
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserMapper userMapper = context.getBean("userMapper", UserMapper.class);
        for (User user : userMapper.getUserList()) {
            System.out.println(user);
        }
    }
}
~~~

==小结==

1.首先编写了 spring-dao.xml 配置文件 在这里用了Spring的数据源替换了Mybatis的数据源，并配置了连接数据的属性。

2.注册了SqlSessionTemplate的实列，绑定了之前注册的dataSouse，Mybatis的配置文件，mapper的映射路径

3.对SqlSessionTemplate的实列进行构造器注入

4.编写接口的实现类（Spring对此进行操作）

5.编写applicationContext.xml ，引入spring-dao.xml，进行IOC注入sqlSession

6.编写测试类，进行测试

## 4.2.整合方式二

![image-20210827175128804](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210827175128804.png)

==小结==

<img src="C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210827180054862.png" alt="image-20210827180054862" style="zoom:67%;" />

第一种方式需要去给SqlSessionTemplate注入sqlsessionFactory参数,第二种方式则通过继承一个SqlSessionDaoSupport（其父类需要一个SQLSessionFactory来进行构造）

注入区别：第一种方式注入SqlSessionTemplate 第二种方式直接注入 SqlSesionFactory

## 4.3.事务

**1、回顾事务**

- 把一组业务当成一个业务来做，要么都成功，要么都失败。
- 事务在项目的开发中，十分的重要，设计到数据一致性的问题，不能马虎。
- 确保完整性和统一性

事务的ACID原则

- 原子性
- 一致性
- 隔离性
  - 多个业务可能操作同一个资源，防止数据损坏

- 持久性
  - 事务一单提交，无论系统发生什么样的问题，结果都不会再发生影响，被持久化的写到存储器中。

**2.Spring中事务管理**

- 声明式事务：AOP
- 编程式事务 需要卸载代码中，进行事务的管理

**3.为什么要使用事务**

- 如果不配置事务，可能存在数据提交不一致的情况
- 如果不再Spring中去配置声明式事务，就需要我们再代码中手动配置事务。
- 事务在项目的开发中十分重要，设计到数据的一致性和完整性问题，不容马虎。

## 4.4.Spring配置声明事务注入

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/tx
        http://www.springframework.org/schema/tx/spring-tx.xsd">
<!--    DataSource 使用Spring的数据源来替换Mybatis的配置 c3p0 dbcp druid-->
<!--    我们这里使用Spring 提供的JDBC org.springframework.jdbc.datasource-->
        <bean id="datasource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
            <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
            <property name="url" value="jdbc:mysql://localhost:3306/mybatis?userSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8&amp;serverTimezone=UTC"/>
            <property name="username" value="root"/>
            <property name="password" value="123456"/>
        </bean>

<!--    sqlSessionFactory-->
        <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
<!--            绑定Mybatis配置文件-->
            <property name="dataSource" ref="datasource"/>
            <property name="configLocation" value="classpath:mybatis-config.xml"/>
            <property name="mapperLocations" value="classpath:com/heng/dao/*.xml"/>
        </bean>

        <!--只能用构造器注入参数 因为没有set方法-->
        <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
            <constructor-arg index="0" ref="sqlSessionFactory"/>
        </bean>

<!--        配置声明式事务-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <constructor-arg ref="datasource" />
    </bean>

<!--    结合AOP实现事务的织入-->
<!--    配置事务通知-->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
<!--        给哪些方法配置事务-->
        <tx:attributes>
            <!--给哪些方法配置事务-->
        <!--配置事务的传播特性 propagation
                PROPAGATION_REQUIRED:如果当前没有事务，就新建一个事务，如果已存在一个事务中，加入到这个事务中，这是最常见的选择。
                PROPAGATION_SUPPORTS:支持当前事务，如果没有当前事务，就以非事务方法执行。
                PROPAGATION_MANDATORY:使用当前事务，如果没有当前事务，就抛出异常。
                PROPAGATION_REQUIRES_NEW:新建事务，如果当前存在事务，把当前事务挂起。
                PROPAGATION_NOT_SUPPORTED:以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。
                PROPAGATION_NEVER:以非事务方式执行操作，如果当前事务存在则抛出异常。
                PROPAGATION_NESTED:	如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行与PROPAGATION_REQUIRED 类似的操作
        -->
            <tx:method name="add" propagation="REQUIRED"/>
            <tx:method name="update" propagation="REQUIRED"/>
            <tx:method name="delete" propagation="REQUIRED"/>
            <tx:method name="select" read-only="true"/>
<!--            全部方法-->
            <tx:method name="*" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>
<!--    配置事务通知-->
    <aop:config>
<!--        该包下的所有方法-->
        <aop:pointcut id="txPointCut" expression="execution(* com.heng.dao.*.*(..))"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointCut"/>
    </aop:config>
</beans>
~~~

