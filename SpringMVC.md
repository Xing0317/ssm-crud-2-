## SpringMVC

### 1.回顾MVC

#### 1.1、什么是MVC

- MVC是模型(Model)、视图(View)、控制器(Controller)的简写，是一种设计规范
- MVC主要作用是降低了视图与业务逻辑键的双向耦合
- MVC不仅仅是一种设计模式，MVC是一种架构模式。当然不同的MVC存在差异

**Model（模型）**：数据模型，提供要展示的数据，因此包含数据和行为，可以认为是领域模型或JavaBean组件（包含数据和行为），不过现在一般都分离开来：Value Object（数据Dao） 和 服务层（行为Service）。也就是模型提供了模型数据查询和模型数据的状态更新等功能，包括数据和业务。

**View（视图）**：负责进行模型的展示，一般就是我们见到的用户界面，客户想看到的东西。

**Controller（控制器）**：接收用户请求，委托给模型进行处理（状态改变），处理完毕后把返回的模型数据返回给视图，由视图负责展示。也就是说控制器做了个调度员的工作。

**最典型的MVC就是JSP + servlet + javabean的模式。**

<img src="C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210824103653605.png" alt="image-20210824103653605" style="zoom:67%;" />

#### 1.2.Spring MVC的特点：

1. 轻量级，简单易学
2. 高效 , 基于请求响应的MVC框架
3. 与Spring兼容性好，无缝结合
4. 约定优于配置
5. 功能强大：RESTful、数据验证、格式化、本地化、主题等
6. 简洁灵活

### 2.执行流程

<img src="C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210828091941760.png" alt="image-20210828091941760" style="zoom:67%;" />

**1.**  DispatcherServlet表示前置控制器，是整个SpringMVC的控制中心。用户发出请求，DispatcherServlet接收请求并拦截请求。

**2.**  HandlerMapping为处理器映射。DispatcherServlet调用HandlerMapping,HandlerMapping根据请求url查找Handler。

**3.**  HandlerExecution表示具体的Handler,其主要作用是根据url查找控制器，如上url被查找控制器为：hello。

**4.**  HandlerExecution将解析后的信息传递给DispatcherServlet,如解析控制器映射等。

**5.**  HandlerAdapter表示处理器适配器，其按照特定的规则去执行Handler。

**6.**  Handler让具体的Controller执行。

**7. ** Controller将具体的执行信息返回给HandlerAdapter,如ModelAndView。

**8.**  HandlerAdapter将视图逻辑名或模型传递给DispatcherServlet。

**9.**  DispatcherServlet调用视图解析器(ViewResolver)来解析HandlerAdapter传递的逻辑视图名。

**10.** 视图解析器将解析的逻辑视图名传给DispatcherServlet。

**11.** DispatcherServlet根据视图解析器解析的视图结果，调用具体的视图。

**12.** 最终视图呈现给用户。

### 3.代码实现（配置版）

1. 在web.xml中注册 DispatcherServlet

~~~xml
<servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--关联一个springmvc的配置文件:【servlet-name】-servlet.xml-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
             <!--关联一个springmvc的配置文件:【servlet-name】-servlet.xml-->
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
        <!--启动级别-1-->
        <load-on-startup>1</load-on-startup>
 </servlet>
	<!--/ 匹配所有的请求；（不包括.jsp）-->
   <!--/* 匹配所有的请求；（包括.jsp）-->
 <servlet-mapping>
       <servlet-name>springmvc</servlet-name>
       <url-pattern>/</url-pattern>
 </servlet-mapping>

~~~

2. 编写SpringMVC 的 配置文件！名称：springmvc-servlet.xml : [servletname]-servlet.xml

说明，这里的名称要求是按照官方来的

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

</beans>
~~~

​	3.添加处理映射器（可省略）

​		添加处理器适配器（可省略）

~~~xml
<bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
<bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
~~~

​	4.添加视图解析器

~~~xml
<!--视图解析器:DispatcherServlet给他的ModelAndView-->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="InternalResourceViewResolver">
   <!--前缀-->
   <property name="prefix" value="/WEB-INF/jsp/"/>
   <!--后缀-->
   <property name="suffix" value=".jsp"/>
</bean>
~~~

​	5.绑定Control

~~~xml
<!--Handler-->
<bean id="/hello" class="com.heng.controller.HelloController"/>
~~~

​	6.编写Control

~~~java
package com.kuang.controller;
import org.springframework.web.servlet.ModelAndView;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.springframework.web.servlet.mvc.Controller;
//注意：这里我们先导入Controller接口
public class HelloController implements Controller {

    public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
        //ModelAndView 模型和视图
        ModelAndView mv = new ModelAndView();

        //封装对象，放在ModelAndView中。Model
        mv.addObject("msg","HelloSpringMVC!");
        //封装要跳转的视图，放在ModelAndView中
        mv.setViewName("hello"); //: /WEB-INF/jsp/hello.jsp
        return mv;
    }
}
~~~

​	7.写要跳转的jsp页面，显示ModelandView存放的数据，以及我们的正常页面

~~~jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
    <head>
        <title>Kuangshen</title>
    </head>
    <body>
        ${msg}
    </body>
</html>
~~~

**说明：**

- 实现接口Controller定义控制器是较老的办法
- 缺点是：一个控制器中只有一个方法，如果要多个方法则需要定义多个Controller；定义的方式比较麻烦；

### 4.代码实现（注解版）

#### 使用注解@Controller

- @Controller注解类型用于声明Spring类的实例是一个控制器（在讲IOC时还提到了另外3个注解）；
- Spring可以使用扫描机制来找到应用程序中所有基于注解的控制器类，为了保证Spring能找到你的控制器，需要在配置文件中声明组件扫描。

~~~xml
 <!-- 自动扫描包，让指定包下的注解生效,由IOC容器统一管理 -->
    <context:component-scan base-package="com.heng.control"/>
    <!-- 让Spring MVC不处理静态资源 -->
    <mvc:default-servlet-handler />
~~~

增加一个HelloController类，使用注解实现

~~~java
Controller
public class HelloControl {
    @RequestMapping("/hello")
    public String hello(Model model){
        
        model.addAttribute("msg","hello SpringMVC");
        //返回结果是视图的名称
        System.out.println(model);
        return "hello";
    }
}
~~~

运行测试

<img src="C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210828092601620.png" alt="image-20210828092601620" style="zoom:67%;" />

**可以发现，我们的两个请求都可以指向一个视图(test)，但是页面结果的结果是不一样的，从这里可以看出视图是被复用的，而控制器与视图之间是弱偶合关系。**

**注解方式是平时使用的最多的方式！**

### 5.RequestMapping

- @RequestMappping注解用于映射url到控制器类或一个特定的处理程序方法
- 可以用在类和方法上，用在类上表示所有的响应请求方法都是以该地址座位父路径

~~~Java
@Controller
@RequestMapping("/test")
public class HelloControl {

    @RequestMapping("/hello")
    public String hello(Model model){

        model.addAttribute("msg","hello SpringMVC");
        //返回结果是视图的名称
        System.out.println(model);
        return "hello";
    }

    @RequestMapping("/hi")
    public String hi(Model model){
        model.addAttribute("msg","hi SpringMVC");
        return "hello";
    }
}
~~~

### 6.RestFul 风格

概念

==Restful就是一个资源定位及资源操作的风格。不是标准也不是协议，只是一种风格。基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存等机制。==

功能

资源：互联网所有的事物都可以被抽象为资源

资源操作：使用POST、DELETE、PUT、GET，使用不同方法对资源进行操作。

分别对应 添加、 删除、修改、查询。

传统方式操作资源 ：通过不同的参数来实现不同的效果！方法单一，post 和 get

http://127.0.0.1/item/queryItem.action?id=1 查询,GET

http://127.0.0.1/item/saveItem.action 新增,POST

http://127.0.0.1/item/updateItem.action 更新,POST

http://127.0.0.1/item/deleteItem.action?id=1 删除,GET或POST

1.在Spring MVC中可以使用 @PathVariable 注解，让方法参数的值对应绑定到一个URI模板变量上。

~~~java
@Controller
public class FirstController {

    @GetMapping("/test/hello/{a}/{b}")
    public String hello(Model model, @PathVariable int a,@PathVariable int b){
        int result = a+b;
        model.addAttribute("msg","结果为"+result);
        return "hello";
    }
}
~~~

2.测试

![image-20210828140103537](C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210828140103537.png)

思考：使用路径变量的好处？

- 使路径变得更加简洁；
- 获得参数更加方便，框架会自动进行类型转换。
- 通过路径变量的类型可以约束访问参数，如果类型不一样，则访问不到对应的请求方法，如这里访问是的路径是/add/1/a，则路径与方法不匹配，而不会是参数转换失败。

**使用method属性指定请求类型**

用于约束请求的类型，可以收窄请求范围。指定请求谓词的类型如GET, POST, HEAD, OPTIONS, PUT, PATCH, DELETE, TRACE等

我们来测试一下：

- 增加一个方法

~~~java
 @RequestMapping(value = "/hi/{a}/{b}",method = {RequestMethod.POST})
    public String hi(@PathVariable String a,@PathVariable String b,Model model){
        String result = a+b;
        model.addAttribute("msg","字符串拼接为"+result);
        return "hello";
    }
~~~

我们使用浏览器地址栏进行访问默认是Get请求，会报错405：

<img src="C:\Users\86150\AppData\Roaming\Typora\typora-user-images\image-20210828141115054.png" alt="image-20210828141115054" style="zoom:67%;" />

​	如果将POST修改为GET则正常了；

**小结：**

Spring MVC 的 @RequestMapping 注解能够处理 HTTP 请求的方法, 比如 GET, PUT, POST, DELETE 以及 PATCH。

**所有的地址栏请求默认都会是 HTTP GET 类型的。**

方法级别的注解变体有如下几个：组合注解

~~~java
@GetMapping
@PostMapping
@PutMapping  更新
@DeleteMapping  删除
@PatchMapping
~~~

### 7.请求转发和重定向

#### 7.1、ModelAndView

设置ModelAndView对象 , 根据view的名称 , 和视图解析器跳到指定的页面 .

页面 : {视图解析器前缀} + viewName +{视图解析器后缀}

#### 7.2、ServletAPI

通过设置ServletAPI , 不需要视图解析器 .

实现了controller 接口后，即可在参数后增加req,rspon方法

1. 通过HttpServletResponse进行输出
2. 通过HttpServletResponse实现重定向
3. 通过HttpServletResponse实现转发

#### 5.3、SpringMVC

**通过SpringMVC来实现转发和重定向 - 无需视图解析器；**

~~~java
@Controller
public class ResultSpringMVC {
    @RequestMapping("/rsm/t1")
    public String test1(){
        //转发
        return "/index.jsp";
    }
    @RequestMapping("/rsm/t2")
    public String test2(){
        //转发二
        return "forward:/index.jsp";
    }
    @RequestMapping("/rsm/t3")
    public String test3(){
        //重定向
        return "redirect:/index.jsp";
    }
}
~~~

**通过SpringMVC来实现转发和重定向 - 有视图解析器；**

重定向 , 不需要视图解析器 , 本质就是重新请求一个新地方嘛 , 所以注意路径问题.

~~~java
@Controller
public class ResultSpringMVC2 {
    @RequestMapping("/rsm2/t1")
    public String test1(){
        //转发
        return "test";
    }
    @RequestMapping("/rsm2/t2")
    public String test2(){
        //重定向
        return "redirect:/index.jsp";
        //return "redirect:hello.do"; //hello.do为另一个请求/
    }
}
~~~

==注意：重定向不能访问到web-inf下的jsp，web-inf下是保护文件，不能被跳转访问==

请求转发上地址栏不变 因此参数不会丢失

重定向 地址栏变换，地址丢失

### 8.数据处理

#### **1、提交的域名称和处理方法的参数名不一致**

需要用注解@RequestParam()来解决这个问题，同时这也是一种==代码规范==

~~~java
//@RequestParam("username") : username提交的域的名称 .
@RequestMapping("/hello")
public String hello(@RequestParam("username") String name){
    System.out.println(name);
    return "hello";
}
~~~

#### **2、提交的是一个对象**

要求提交的表单域和对象的属性名一致 , 参数使用对象即可

~~~Java
public class User {
    private int id;
    private String name;
    private int age;
    //构造
    //get/set
    //tostring()
}
~~~

访问路径：

http://localhost:8080/mvc04/user?name=kuangshen&id=1&age=15

处理方法 :

~~~java
@RequestMapping("/user")
public String user(User user){
    System.out.println(user);
    return "hello";
}
~~~

==说明：如果使用对象的话，前端传递的参数名和对象名必须一致，否则就是null。==

#### 3.数据显示到前端

**第一种 : 通过ModelAndView**

**第二种 : 通过ModelMap**

**第三种 : 通过Model**

对比

~~~tex
Model 只有寥寥几个方法只适合用于储存数据，简化了新手对于Model对象的操作和理解；
ModelMap 继承了 LinkedMap ，除了实现了自身的一些方法，同样的继承 LinkedMap 的方法和特性；
ModelAndView 可以在储存数据的同时，可以进行设置返回的逻辑视图，进行控制展示层的跳转。
~~~

#### 4.乱码问题

在web.xml中增加过滤器

~~~xml
<filter>
    <filter-name>encoding</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>utf-8</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>encoding</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
~~~

终极过滤器

~~~xml
package com.kuang.filter;
import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletRequestWrapper;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.UnsupportedEncodingException;
import java.util.Map;
/**
 * 解决get和post请求 全部乱码的过滤器
 */
public class GenericEncodingFilter implements Filter {
    @Override
    public void destroy() {
    }
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        //处理response的字符编码
        HttpServletResponse myResponse=(HttpServletResponse) response;
        myResponse.setContentType("text/html;charset=UTF-8");
        // 转型为与协议相关对象
        HttpServletRequest httpServletRequest = (HttpServletRequest) request;
        // 对request包装增强
        HttpServletRequest myrequest = new MyRequest(httpServletRequest);
        chain.doFilter(myrequest, response);
    }
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
    }
}
//自定义request对象，HttpServletRequest的包装类
class MyRequest extends HttpServletRequestWrapper {
    private HttpServletRequest request;
    //是否编码的标记
    private boolean hasEncode;
    //定义一个可以传入HttpServletRequest对象的构造函数，以便对其进行装饰
    public MyRequest(HttpServletRequest request) {
        super(request);// super必须写
        this.request = request;
    }
    // 对需要增强方法 进行覆盖
    @Override
    public Map getParameterMap() {
        // 先获得请求方式
        String method = request.getMethod();
        if (method.equalsIgnoreCase("post")) {
            // post请求
            try {
                // 处理post乱码
                request.setCharacterEncoding("utf-8");
                return request.getParameterMap();
            } catch (UnsupportedEncodingException e) {
                e.printStackTrace();
            }
        } else if (method.equalsIgnoreCase("get")) {
            // get请求
            Map<String, String[]> parameterMap = request.getParameterMap();
            if (!hasEncode) { // 确保get手动编码逻辑只运行一次
                for (String parameterName : parameterMap.keySet()) {
                    String[] values = parameterMap.get(parameterName);
                    if (values != null) {
                        for (int i = 0; i < values.length; i++) {
                            try {
                                // 处理get乱码
                                values[i] = new String(values[i]
                                        .getBytes("ISO-8859-1"), "utf-8");
                            } catch (UnsupportedEncodingException e) {
                                e.printStackTrace();
                            }
                        }
                    }
                }
                hasEncode = true;
            }
            return parameterMap;
        }
        return super.getParameterMap();
    }
    //取一个值
    @Override
    public String getParameter(String name) {
        Map<String, String[]> parameterMap = getParameterMap();
        String[] values = parameterMap.get(name);
        if (values == null) {
            return null;
        }
        return values[0]; // 取回参数的第一个值
    }
    //取所有值
    @Override
    public String[] getParameterValues(String name) {
        Map<String, String[]> parameterMap = getParameterMap();
        String[] values = parameterMap.get(name);
        return values;
    }
}
~~~

### 9.1.什么是JSON？

- JSON(JavaScript Object Notation, JS 对象标记) 是一种轻量级的数据交换格式，目前使用特别广泛。
- 采用完全独立于编程语言的**文本格式**来存储和表示数据。
- 简洁和清晰的层次结构使得 JSON 成为理想的数据交换语言。
- 易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率。

在 JavaScript 语言中，一切都是对象。因此，任何JavaScript 支持的类型都可以通过 JSON 来表示，例如字符串、数字、对象、数组等。看看他的要求和语法格式：

- 对象表示为键值对，数据由逗号分隔
- 花括号保存对象
- 方括号保存数组

**JSON 键值对**是用来保存 JavaScript 对象的一种方式，和 JavaScript 对象的写法也大同小异，键/值对组合中的键名写在前面并用双引号 “” 包裹，使用冒号 : 分隔，然后紧接着值：

