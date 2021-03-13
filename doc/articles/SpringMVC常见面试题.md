## SpringMVC 常见面试题整理

### 1. 什么是 SpringMVC ？说说你对 SpringMVC 的理解

SpringMVC 其实是基于 Spring 框架下的的 webmvc 模块发展而来的，底层的实现是 基于 Servlet API 的。

做到  M（Model）— V（View）— C（Controller）分层。降低各层次之间的耦合性，简化开发。

### 2. SpringMVC 有哪些优点？

1. 与 Spring 集成使用非常方便，生态好。
2. 配置简单，快速上手。
3. 支持 [RESTful](https://baike.baidu.com/item/RestFul/4406165) 风格。
4. 支持各种视图技术，支持各种请求资源映射策略。

### 3. 说一下 SpringMVC 的流程

先上一张整体的流程图：

![SpringMVC流程图](https://gitee.com/kingshion/imgs/raw/master/2021/images/image-20210313114333360.png)

下面详细说下具体流程：

1. 用户发送请求给到前端控制器 `DispatchServlet` ；
2. `DispatchServlet` 收到请求后，调用`HandlerMapping` 处理器映射器查找对应负责处理请求的 Handler；
3. `HandlerMapping` 将找到的具体的处理器 Handler 生成处理器对象以及处理器拦截器（如果有则生成），一起返回给 `DispatchServlet`；
4. `DispatchServlet`调用`HandlerAdapter` 处理器适配器，请求执行具体的 Handler；
5. `HandlerAdapter` 将具体 Handler 执行返回的模型和视图返回给到 `DispatchServlet`；
6. 此时 `DispatchServlet` 已经得到具体的 视图名称了，然后向 `ViewResolver` 发起请求，请求解析视图，返回已经解析好的视图对象；
7. `DispatchServlet` 对视图进行渲染，将 model 与view 进行渲染；
8. `DispatchServlet` 返回响应给到用户浏览器；

> 前端控制器 `DispatcherServlet`：接收请求、响应结果，相当于转发器，有了`DispatcherServlet `能够减少了其它组件之间的耦合度。
> 处理器映射器 `HandlerMapping`：根据请求的URL来查找Handler
> 处理器适配器 `HandlerAdapter`：负责执行Handler
> 处理器 Handler：处理器，需要程序员开发
> 视图解析器 `ViewResolver`：进行视图的解析，根据视图逻辑名将ModelAndView解析成真正的视图（view）
> 视图View：View是一个接口， 它的实现类支持不同的视图类型，如jsp，freemarker，pdf等等



### 4. SpringMVC 的常用注解有哪些？

1. `@RequestMapping` ：用于处理 URL 映射的的注解，既可用于类上，也可用于方法上，当作用于类上时则表示当前类中的所有响应方法都是以这个 url 作为父路径；
2. `@RequestBody` ：请求体注解，能够将前端传到后端的 json 字符串中的数据（请求体中的数据）封装到对应的类型中。（注意：只能解析请求体中的数据！！所以使用 GET 请求方式是拿不到数据的！）这里参考一个写的非常nice 的文章：[@RequestBody的使用](https://blog.csdn.net/justry_deng/article/details/80972817/)
3. `@ResponseBody` ：将 Java 对象转为 json 格式的数据，同样贴一篇详情文章：[@ResponseBody详解](https://blog.csdn.net/originations/article/details/89492884)



### 5. @RestController 和 @Controller

1. `Controller` 返回一个页面：

   单独使用 `Controller` 不加 `@ResponseBody` 使用的话就要经过视图解析流程 （ViewResolve），最后返回给到前端一页面，这种情况属于比较传统的 Spring MVC 应用，对应前后端不分离的情况；

   ![](https://gitee.com/kingshion/imgs/raw/master/2021/images/image-20210313124118788.png)


2. `@RestController` 返回 json 或者 xml 形式的数据：

   `@RestController` 只返回对象，对象以 json 或者 xml 格式写入到 http 响应中，属于 RESTful 风格，也是目前流行的前后端分离方式；

   ![](https://gitee.com/kingshion/imgs/raw/master/2021/images/image-20210313124417282.png)

3.  `@Controller` + `@ResponseBody` 返回 json 或者 xml 形式数据：

   在 Spring 4.x 之前如果想要使用 RESTful 风格的话，是需要使用这两个注解组合进行使用的。但是在之后就可以使用上面的 `@RestController` 一步到位。

   ![image-20210313124652641](https://gitee.com/kingshion/imgs/raw/master/2021/images/image-20210313124652641.png)

### 6. SpringMVC 和 Struts2 的区别

1. Struts2 是类级别的拦截，而 SpringMVC 是方法级别的拦截；SpringMVC 的方法之间基本是独立的，独享 request 和 response 数据，请求数据通过参数获取，处理结果通过 ModelMap 交回给框架，方法之间不共享变量，而 Struts2 虽然方法之间是独立的，但是所有的 Action 变量是共享的。
2. SpringMVC 的入口是 Servlet， 而Struts2 的入口是 Filter。
3. SpringMVC验证支持JSR303，处理起来相对更加灵活方便，而Struts2验证比较繁琐。
4. SpringMVC 开发效率和性能高于 Struts2 。

参考文章：[SpringMVC与Struts2区别与比较总结](https://blog.csdn.net/chenleixing/article/details/44570681)

### 7. 如何解决 POST 请求和 GET 请求中文乱码问题？

1. 解决 POST 请求乱码问题：在 web.xml 中配置一个 CharacterEncodingFilter 过滤器，设置成 utf-8 ；

   ```xml
   <filter>
       <filter-name>CharacterEncodingFilter</filter-name>
       <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
       <init-param>
           <param-name>encoding</param-name>
           <param-value>utf-8</param-value>
       </init-param>
   </filter>
    
   <filter-mapping>
       <filter-name>CharacterEncodingFilter</filter-name>
       <url-pattern>/*</url-pattern>
   </filter-mapping>
   ```

2. GET 请求中出现中文乱码，讲真在url 中传递中文，就不是什么明智之举，最好不要这用做！

   但是事情已经发生了，总得要解决的：

   1. 修改 Tomcat 配置文件；
   2. 在拼接 url 的时候先做一个编码工作，然后将编码后生成的 url 进行拼接，之后再发送请求，然后再服务端拿到数据后做一次反编码的工作；
   3. 最好就是定义一个字典，在发送 url 的get 请求之前先查询一下要拼接的 中文 字段对应的字典值，然后用字典值进行传输，完事了再在服务端做解析；

   [详解get请求和post请求参数中文乱码的解决办法](https://www.cnblogs.com/cdf-opensource-007/p/6337448.html)

### 8. SpringMVC 制器是不是单例模式,如果是,有什么问题,怎么解决？

是单例模式，之前在 Spring 篇章里面有说过控制层和Service 层以及 DAO 层一般是无状态的实例，不涉及数据的增删改等操作，只有简单的查询操作，那么就是线程安全的，所以解决方案就是：在控制器里面不要写可变的状态量，如果需要这些可变的状态量，那么可以通过 `ThreadLocal` 进行解决，为每个线程提供一个独立的变量副本，进行数据操作；

### 9. SpringMVC 怎么样设定重定向和转发的？

1. 转发：在返回值前面加：“forward”–> `"forword:index.do?name=2"`
2. 重定向：在返回值前面加：“redirect” –> `“redirect:http://www.baidu.com”`

关于请求转发和重定向底层原理可以看我以前写的一篇博客：[Servlet重定向和请求转发的区别](https://blog.csdn.net/qq_41377858/article/details/105460863?spm=1001.2014.3001.5502)

### 10. SpringMVC里面拦截器是怎么写?

定义一个实现 `HandlerInterceptor` 接口的拦截器，然后在 web.xml 文件中配置拦截器即可

[详细细节](https://blog.csdn.net/eson_15/article/details/51749880)

### 11. SpringMVC 怎么和AJAX相互调用的？

通过 Jackson 框架可以将 Java 中的对象直接转换为 json 对象：

1. 加入 Jackson.jar 包
2. 在配置文件中配置 json 的映射
3. 在接受 ajax 方法里面可以直接返回对象，但是需要使用 `@ResponseBody` 注解；

参考文章[SpringMVC与Ajax交互的几种形式](https://blog.csdn.net/qq_37230121/article/details/83750888)

### 12. Spring MVC的异常处理

可以将异常抛给Spring框架，由Spring框架来处理；我们只需要配置简单的异常处理器，在异常处理器中添视图页面即可。

- 使用系统定义好的异常处理器 SimpleMappingExceptionResolver
- 使用自定义异常处理器
- 使用异常处理注解

参考文章：[springmvc异常处理](https://www.jianshu.com/p/89273bc5a81c)

### 13. 怎样在方法里面得到Request，或者Session？

直接在方法的形参中声明 request，SpringMVC 就自动把 request 对象传入。

### 14. 如果想在拦截的方法里面得到从前台传入的参数，怎么得到？

直接在形参里面声明这个参数就可以，但必须名字和传过来的参数一样。

### 15. 如果前台有很多个参数传入，并且这些参数都是一个对象的，那么怎么样快速得到这个对象？

直接在方法形参中声明这个对象，SpringMVC 就自动会把属性赋值到这个对象里面。

### 16. SpringMVC 中函数的返回值是什么？

返回值可以有很多类型，有String，ModelAndView。ModelAndView类把视图和数据都合并的一起的，但一般用String比较好。

### 17. SpringMVC 用什么对象从后台向前台传递数据的？

1. 将数据绑定到 request 
2. 返回 ModelAndView;
3. 通过ModelMap对象，可以在这个对象里面调用put方法，把对象加到里面，前端就可以通过el表达式拿到。
4. 绑定数据到 Session中；

下面是以小样例代码，具体可以参考这篇文章：[springMVC向页面传值](https://blog.csdn.net/u011637069/article/details/50792403)

```java
@RequestMapping("/bind")
public String bindData(String str, HttpServletRequest request, ModelMap map, ModelAndView modelAndView, HttpSession session) {
    logger.info("...bind 方法");
    request.setAttribute("str", str);
    map.put("str", str);
    modelAndView.addObject("str", str);
    session.setAttribute("str", str);
    return "success";
}
```


### 18. 怎么样把ModelMap里面的数据放入Session里面？

可以在类上面加上@SessionAttributes注解，里面包含的字符串就是要放入session里面的key。



今天收获一句话：你若对得起时间，时间便对得起你！分享给大家，一起共勉！

☀️ 学而不思则罔，思而不学则殆

👉 我是 江璇 ，一个不断努力的新人程序猿🐵

👊关注我，一起成长！一起进步！