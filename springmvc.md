[TOC]

springmvc 是web层mvc框架，何谓mvc？

model 	模型

view		视图

controller	控制器

这是一种设计模式，将责任进行拆分，不同的组件负责不同的事情。

------

## 组件分析

web.xml

注册前控制器，目的在于，我们希望让springmvc去处理所有的请求。

通过

```xml
<!-- servlet映射配置-->
<servlet-mapping>
  <servlet-name>springmvc</servlet-name>
  <!-- 这里统一写/ -->
  <url-pattern>/</url-pattern>
</servlet-mapping>
```

url-pattern 

* /
* /*       （永远不要用）
* *.do      （不建议用）

> /*不能写的原因
>
> 请求/helloCotroller过去的时候，它的视图名称是girl.jsp页面。它将其当作了一个叫girl.jsp的请求，尝试去匹配对应的controller，但是我的容器中根本不存在这样的controller，所以无法匹配，导致404 。

> *.do  
>
> 这种方式，是有的团队习惯将请求的行为加个小尾巴用来区分，.do 	行为	 *.action

## 关于前端控制器的解释

springmvc设计的理念是希望开发者尽量远离原生的serveletAPI，API不是很好用，繁琐点，将操作进一步简化，它将很多东西责任进行了拆分，不希望我们将一些技术点绑定死，可以做到随意切换，本身还是基于Servlet设计的，分发的servlet。

## springmvc配置文件名称问题

默认情况下是用dispatcherServlet的名称当作命名空间

```[servletName]-servlet.xml(WEB_INF)```	之下寻找

```[servletName]-servlet=namespace```

将配置文件移动位置之后，出现下面异常。

```xml
Could not open ServletContext resource [/WEB-INF/springmvc-servlet.xml]
```

如果非要重新使用另外一个名字

```xml
<!-- 在<servlet>中添加 -->
<init-param>
            <param-name>namespace</param-name>
            <param-value>mvc</param-value>
</init-param>
<!-- 且将<WEB-INF>中的[servletName]-servlet.xml改为设置的value值-->
```

默认的规则要求在web-inf下，但是maven项目的标准应在resources下面。

需要重新指定上下文的配置文件的位置。

```xml
<init-param>
  <!--上下文配置文件位置的配置-->
  <param-name>contextConfigLocation</param-name>
  <param-value>classpath:springmvc.xml</param-value>
  <!--指定classpath后名称即可随意更改-->
</init-param>
```

此时是在类路径下寻找springmvc.xml这个配置文件。推荐使用。

## 视图解析器

* jsp
* freemake

内部的资源视图解析器

* 视图前缀
  * /jsp/  它是我们的请求相应的资源的路径的配置，
* 后缀 
  * .jsp   此时我们的前缀+视图名称+后缀=/jsp/girl.jsp



物理视图是由逻辑视图转换而来

物理视图是	webapp/jsp/girl.jsp

逻辑视图

* prefix
* logicViewName
* suffix

p View = prefix + logicViewName + suffix;

## 控制器的解释

是一种比较传统的实现一个接口的方式完成的 Controller。

如果一个接口只有一个方法，这种借口叫做函数式接口。

```java
@Nullable
ModelAndView handleRequest(HttpServletRequest var1, HttpServletResponse var2) throws Exception;
```

类似doGet doPost   返回值void



## 注解开发模式

基本注解

* @Controller

* @ResquestMapping

  注解需要扫描

 ```xml
<beans xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd"/>
<!--    配置一个注解扫描的包-->
<context:component-scan base-package="com.clhz.Controller" />
 ```



>  开发步骤
>
> 1. 记得配置一下基础扫描的包，这样配置的注解才会生效。
> 2. 在指定的类上面添加@Controller注解
> 3. 添加@RequestMapping 类似于前面的controller的那个名字  （不同requesthandler处理HandlerMapping）



当我们写上Controller之后，就标记了它为spring的一个组件，并且是控制器的组件，此时我们的handlermapping会去扫描寻找这个controller是否与之匹配，如果发现匹配就把这里处理的工作交给它。



匹配的规则

具体的匹配就是通过请求的路径进行匹配的

@RequestMapping(URI)

此时就是通过这个URI进行匹配



## 转发与重定向

* 转发到页面  默认到选项
* 重定向到页面   redirect:path
* 转发到另一个控制器   forward:path

## 关于springmvc访问web元素

* request
* session
* application

可以通过模拟对象完成操作，也可以用使用原生的ServletAPI完成，直接在方法当中入参即可。

## 注解详解

### @RequestMapping

* value 写的是路径，是一个数组形式，可以匹配多个路径值。```value = {"v1","v2",...}```
* path 是value的别名，二者选一。
* method  可以指定访问的请求类型   ```method = RequestMethod.POST```
* params 可以去指定参数，还可以限定这个参数的特征，必须等于某个值，不等于某个值```params = {"girl=gname1","boy!=bname1"}```
* headers 能够影响浏览器的行为
* consumers 消费者，媒体类型，可以限定必须为application/json;charset=utf-8
* produces 产生的响应的类型

### @GetMapping    @PostMapping

* GetMapping  限定了只支持get请求
* PostMapping 限定了只支持post请求

### 对于非get post请求的支持

* 过滤器

```xml
<!--注册一个支持所有http请求类型的过滤器-->
<filter>
  <filter-name>hiddenHttpMethodFilter</filter-name>
  <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
</filter>
<filter-mapping>
  <filter-name>hiddenHttpMethodFilter</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>
```



















## 关于请求路径的问题

springmvc 支持ant风格

* ？   任意的字符，/除外
* \*     0到n，任意个字符都可以
* \**   支持任意层路径   /a/**

## 关于静态资源访问的问题

由于我们的servlet设置了URL匹配方式为 /  ，所以它将静态资源也当作一个后台的请求

比如加载html的css文件

它尝试去匹配css的Controller里面的RequestMapping的组合，因为没有，所以404。

解决















































































