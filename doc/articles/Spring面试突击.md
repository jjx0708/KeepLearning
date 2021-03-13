## Spring 面试突击

### 1. Spring 是什么？

Spring是一个开源的轻量级、侵入性小、松散耦合的开发框架，能够与多种框架进行集成，并进行不同的操作，可以称之为框架的框架。

具有以下几个模块：

1. Spring 核心容器：
   - Spring Core：核心类库，所有功能都依赖于该类库，提供IOC和DI服务；
   - Spring Beans
   - Spring SpEL（Spring Expression Language）
   - Spring Context：提供框架式的Bean访问方式，以及企业级功能（JNDI、定时任务等）；
2. Data Access / Integration：数据访问框架集成
   - JDBC
   - ORM
   - JMS：消息
   - Transaction：事务
3. Web模块：提供了基本的面向Web的综合特性，提供对常见框架如Struts2的支持，Spring能够管理这些框架，将Spring的资源注入给框架，也能在这些框架的前后插入拦截器；
   - WebSocket
   - Servlet
   - Web
4. AOP：面向切面编程
5. Test：测试框架

![image-20210312135002194](https://gitee.com/kingshion/imgs/raw/master/2021/images/image-20210312135002194.png)



### 2. 对 Spring Ioc 的理解：

1. **Inversion of Control（控制反转）**
2. 是一种设计思想，在Java开发中，将你设计好的对象交给容器控制，而不是显示地用代码进行对象的创建。
3. 把创建和查找依赖对象的控制权交给 IoC 容器，由 IoC 容器进行注入、组合对象。这样对象与对象之间是松耦合、便于测试、功能可复用（减少对象的创建和内存消耗），使得程序的整个体系结构可维护性、灵活性、扩展性变高。

#### 使用 IoC 的好处：

- 资源集中管理，实现资源的可配置和易管理
- 降低了资源的依赖程度，即松耦合
- 便于测试
- 功能可复用（减少对象的创建和内存消耗）
- 使得程序的整个体系结构可维护性、灵活性、扩展性变高

#### DI 是什么？

- （Dependency Injection）依赖注入，是 IoC 容器装配、注入对象的一种方式。通过依赖注入机制，简单的配置即可注入需要的资源，完成自身的业务逻辑，不需要关心资源的出处和具体实现。

**Spring的IoC有三种注入方式 ：构造器注入、setter方法注入、根据注解注入。**

### 3. 对Spring AOP 的理解：

1. **Aspect Oriented Programming，面向切面编程。**
2. 作为面向对象的一种补充，用于将那些与业务无关，但却对多个对象产生影响的公共行为和逻辑，抽取并封装为一个可重用的模块。减少系统中的重复代码，降低了模块间的耦合度，提高系统的可维护性。可用于权限认证、日志、事务处理。
3. AOP代理主要分为静态代理和动态代理，静态代理的代表为AspectJ，动态代理则以Spring AOP为代表。
   1. AspectJ是静态代理，也称为编译时增强，AOP框架会在编译阶段生成AOP代理类，并将AspectJ(切面)织入到Java字节码中，运行的时候就是增强之后的AOP对象。
   2. Spring AOP使用的动态代理，所谓的动态代理就是说AOP框架不会去修改字节码，而是每次运行时在内存中临时为方法生成一个AOP对象，这个AOP对象包含了目标对象的全部方法，并且在特定的切点做了增强处理，并回调原对象的方法。

#### Spring AOP中的动态代理主要有两种方式：

`JDK动态代理`和`CGLIB动态代理`：

1. JDK动态代理只提供接口的代理，不支持类的代理，要求被代理类实现接口。JDK动态代理的核心是`InvocationHandler`接口和Proxy类，在获取代理对象时，使用Proxy类来动态创建目标类的代理类（即最终真正的代理类，这个类继承自Proxy并实现了我们定义的接口），当代理对象调用真实对象的方法时， `InvocationHandler` 通过invoke()方法反射来调用目标类中的代码，动态地将横切逻辑和业务编织在一起；

   > `InvocationHandler` 的 `invoke(Object  proxy,Method  method,Object[] args)`：proxy是最终生成的代理对象;  method 是被代理目标实例的某个具体方法;  args 是被代理目标实例某个方法的具体入参, 在方法反射调用时使用。

2. 如果被代理类没有实现接口，那么Spring AOP会选择使用CGLIB来动态代理目标类。CGLIB（Code Generation Library），是一个代码生成的类库，可以在运行时动态的生成指定类的一个子类对象，并覆盖其中特定方法并添加增强代码，从而实现AOP。CGLIB是通过继承的方式做的动态代理，因此如果某个类被标记为final，那么它是无法使用CGLIB做动态代理的。
3. 静态代理与动态代理区别在于生成AOP代理对象的时机不同，相对来说AspectJ的静态代理方式具有更好的性能，但是AspectJ需要特定的编译器进行处理，而Spring AOP则无需特定的编译器处理。

#### Spring AOP 里面的几个关键名词：

1. 连接点（Join point）：指程序运行过程中所执行的方法。在Spring AOP中，一个连接点总代表一个方法的执行。 

2. 切面（Aspect）：被抽取出来的公共模块，可以用来会横切多个对象。Aspect切面可以看成 Pointcut切点 和 Advice通知 的结合，一个切面可以由多个切点和通知组成。

3. 切点（Pointcut）：切点用于定义 要对哪些Join point进行拦截。

   > 切点分为execution方式和annotation方式。execution方式可以用路径表达式指定对哪些方法拦截，比如指定拦截add*、search*。annotation方式可以指定被哪些注解修饰的代码进行拦截。

4. 通知（Advice）：指要在连接点（Join Point）上执行的动作，即增强的逻辑，比如权限校验和、日志记录等。通知有各种类型，包括Around、Before、After、After returning、After throwing。
5. 目标对象（Target）：包含连接点的对象，也称作被通知（Advice）的对象。 由于Spring AOP是通过动态代理实现的，所以这个对象永远是一个代理对象。
6. 织入（Weaving）：通过动态代理，在目标对象（Target）的方法（即连接点Join point）中执行增强逻辑（Advice）的过程。
7. 引入（Introduction）：添加额外的方法或者字段到被通知的类。Spring允许引入新的接口（以及对应的实现）到任何被代理的对象。例如，你可以使用一个引入来使bean实现 IsModified 接口，以便简化缓存机制。

可以参照下面这张图：

![](https://gitee.com/kingshion/imgs/raw/master/2021/images/20210312085801.png)

#### Spring通知（Advice）有哪些类型？

1. 前置通知（Before Advice）：在连接点（Join point）之前执行的通知。
2. 后置通知（After Advice）：当连接点退出的时候执行的通知（不论是正常返回还是异常退出）。 
3. 环绕通知（Around Advice）：包围一个连接点的通知，这是最强大的一种通知类型。 环绕通知可以在方法调用前后完成自定义的行为。它也可以选择是否继续执行连接点或直接返回它们自己的返回值或抛出异常来结束执行。
4. 返回后通知（AfterReturning Advice）：在连接点正常完成后执行的通知（如果连接点抛出异常，则不执行）
5. 抛出异常后通知（AfterThrowing advice）：在方法抛出异常退出时执行的通知

![image-20210312090630444](https://gitee.com/kingshion/imgs/raw/master/2021/images/image-20210312090630444.png)

**具体实战项目可以参照这个博客：https://www.jianshu.com/p/007bd6e1ba1b**

注意：定义切点时候，还可以通过自定义注解的方式，此时切点会找到使用自定义注解标注的连接点，然后进行切面的织入。

先自定义一个注解，（不会的小伙伴请补充学习元注解等概念），然后在切面的 PointCut 定义时使用 注解方式，而不是使用表达式方式：

```java
// 自定义注解
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface PermissionAnnotation{
}

// 在需要使用 aop 的地方使用自定义注解标注
@RestController
@RequestMapping(value = "/permission")
public class TestController {
    @RequestMapping(value = "/check", method = RequestMethod.POST)
    @PermissionsAnnotation()// 添加这个注解
    public JSONObject getGroupList(@RequestBody JSONObject request) {
        return JSON.parseObject("{\"message\":\"SUCCESS\",\"code\":200}");
    }
}

// 表达式定义
@Pointcut("execution(* com.bytebeats.spring4.aop.annotation.service.BankServiceImpl.*(..))")
public void pointcut1() {
}

// 注解定义（定义一个切点，这个切点通过自定义注解作为标识，能够指定连接点）
@Pointcut("@annotation(com.mu.demo.annotation.PermissionAnnotation)")
private void logAdvicePointcut() {
}
```

分享一个写的非常不错的博客：https://blog.csdn.net/mu_wind/article/details/102758005

### 4. Spring容器的启动流程：

1. 初始化 Spring 容器，注册内置的 `BeanPostProcessor` 的 `BeanDefinition` 到容器中：

   > ① 实例化`BeanFactory`【`DefaultListableBeanFactory`】工厂，用于生成Bean对象
   > ② 实例化`BeanDefinitionReader`注解配置读取器，用于对特定注解（如@Service、@Repository）的类进行读取转化成  `BeanDefinition `对象，（`BeanDefinition` 是 Spring 中极其重要的一个概念，它存储了 bean 对象的所有特征信息，如是否单例，是否懒加载，`factoryBeanName `等）
   > ③ 实例化`ClassPathBeanDefinitionScanner`路径扫描器，用于对指定的包目录进行扫描查找 bean 对象

2. 将配置类的`BeanDefinition`注册到容器中：

3. 调用refresh()方法刷新容器：

   > ①` prepareRefresh()`刷新前的预处理：
   > ② `obtainFreshBeanFactory()`：获取在容器初始化时创建的BeanFactory：
   > ③` prepareBeanFactory(beanFactory)`：BeanFactory的预处理工作，向容器中添加一些组件：
   > ④` postProcessBeanFactory(beanFactory)`：子类重写该方法，可以实现在BeanFactory创建并预处理完成以后做进一步的设置
   > ⑤ `invokeBeanFactoryPostProcessors(beanFactory)`：在BeanFactory标准初始化之后执行BeanFactoryPostProcessor的方法，即BeanFactory的后置处理器：
   > ⑥ `registerBeanPostProcessors(beanFactory)`：向容器中注册Bean的后置处理器BeanPostProcessor，它的主要作用是干预Spring初始化bean的流程，从而完成代理、自动注入、循环依赖等功能
   > ⑦ `initMessageSource()`：初始化MessageSource组件，主要用于做国际化功能，消息绑定与消息解析：
   > ⑧ `initApplicationEventMulticaster()`：初始化事件派发器，在注册监听器时会用到：
   > ⑨` onRefresh()`：留给子容器、子类重写这个方法，在容器刷新的时候可以自定义逻辑
   > ⑩ `registerListeners()`：注册监听器：将容器中所有的ApplicationListener注册到事件派发器中，并派发之前步骤产生的事件：
   > ⑪  `finishBeanFactoryInitialization(beanFactory)`：初始化所有剩下的单实例bean，核心方法是preInstantiateSingletons()，会调用getBean()方法创建对象；
   > ⑫ `finishRefresh()`：发布BeanFactory容器刷新完成事件：

一般面试不会深纠这个问题，只要大概知道。具体详情可以参考敖丙的这篇博客：https://juejin.cn/post/6906637797080170510#comment

### 5. BeanFactory和ApplicationContext有什么区别？

 BeanFactory和ApplicationContext是Spring的两大核心接口，都可以当做Spring的容器。

1. BeanFactory是Spring里面最底层的接口，是IoC的核心，定义了IoC的基本功能，包含了各种Bean的定义、加载、实例化，依赖注入和生命周期管理。ApplicationContext接口作为BeanFactory的子类，除了提供BeanFactory所具有的功能外，还提供了更完整的框架功能：
   1. 继承MessageSource，因此支持国际化。
   2. 资源文件访问，如URL和文件（ResourceLoader）。
   3. 提供在监听器中注册bean的事件。
2. BeanFactroy 采用的是延迟加载形式来注入Bean的，只有在使用到某个Bean时(调用getBean())，才对该Bean进行加载实例化。这样，我们就不能提前发现一些存在的Spring的配置问题。如果Bean的某一个属性没有注入，BeanFacotry加载后，直至第一次使用调用getBean方法才会抛出异常。
3. ApplicationContext 是在容器启动时，一次性创建了所有的Bean。这样，在容器启动时，我们就可以发现Spring中存在的配置错误，这样有利于检查所依赖属性是否注入。 
4. ApplicationContext启动后预载入所有的单实例Bean，所以在运行的时候速度比较快，因为它们已经创建好了。相对于BeanFactory，ApplicationContext 唯一的不足是占用内存空间，当应用程序配置Bean较多时，程序启动较慢。
5. BeanFactory 和 ApplicationContext 都支持BeanPostProcessor、BeanFactoryPostProcessor的使用，但两者之间的区别是：BeanFactory需要手动注册，而ApplicationContext则是自动注册。

### 6. Spring Bean 的生命周期

Spring Bean 的生命周期可以分为四个大阶段：

- 实例化
- 属性赋值
- 初始化
- 销毁

具体步骤如下：

1. 实例化 Bean：

   对于BeanFactory容器，当客户向容器请求一个尚未初始化的bean时，或初始化bean的时候需要注入另一个尚未初始化的依赖时，容器就会调用createBean进行实例化。

   对于ApplicationContext容器，当容器启动结束后，通过获取BeanDefinition对象中的信息，实例化所有的bean。

2. 设置对象属性：实例化后的对象被封装在BeanWrapper对象中，紧接着，Spring根据BeanDefinition中的信息 以及 通过BeanWrapper提供的设置属性的接口完成属性设置与依赖注入。

3. 处理 Aware 接口：Spring会检测该对象是否实现了xxxAware接口，通过Aware类型的接口，可以让我们拿到Spring容器的一些资源：

   ①如果这个Bean实现了BeanNameAware接口，会调用它实现的setBeanName(String beanId)方法，传入Bean的名字；
   ②如果这个Bean实现了BeanClassLoaderAware接口，调用setBeanClassLoader()方法，传入ClassLoader对象的实例。
   ②如果这个Bean实现了BeanFactoryAware接口，会调用它实现的setBeanFactory()方法，传递的是Spring工厂自身。
   ③如果这个Bean实现了ApplicationContextAware接口，会调用setApplicationContext(ApplicationContext)方法，传入Spring上下文；

4. BeanPostProcessor 前置处理：如果想对Bean进行一些自定义的前置处理，那么可以让Bean实现了BeanPostProcessor接口，那将会调用postProcessBeforeInitialization(Object obj, String s)方法。

5. InitializingBean ： 如果Bean实现了InitializingBean接口，执行afeterPropertiesSet()方法。

6. init-method ：如果Bean在Spring配置文件中配置了 init-method 属性，则会自动调用其配置的初始化方法。

7. BeanPostProcessor 后置处理：如果这个Bean实现了BeanPostProcessor接口，将会调用postProcessAfterInitialization(Object obj, String s)方法；由于这个方法是在Bean初始化结束时调用的，所以可以被应用于内存或缓存技术；

**以上几个步骤完成后，Bean就已经被正确创建了，之后就可以使用这个Bean了。**

8. DisposableBean：当Bean不再需要时，会经过清理阶段，如果Bean实现了DisposableBean这个接口，会调用其实现的destroy()方法；
9. destroy-method：如果这个Bean的Spring配置中配置了destroy-method属性，会自动调用其配置的销毁方法。

这里同样贴一篇详细博客：https://blog.csdn.net/a745233700/article/details/113840727

### 7. Spring 中 bean 的作用域：

1. singleton：默认作用域，单例，即每个容器中只会生成一个 bean 的实例；
2. prototype：原型，为每一个bean 的请求都会创建一个新的实例；
3. request：web 模式下的特性，为每一个 request 请求创建一个实例，在请求完成后 ，bean 会失效被GC回收；
4. session：web模式下的特性，同一个 session 会话可以共享一个 bean 实例，不同的 session 会话使用不同的实例；
5. global-session：全局作用域，所有会话共享一个实例。如果想要声明让所有会话共享的存储变量的话，那么这全局变量需要存储在global-session中。比如统计整个站点的在线人数等问题；

### 8. spring中的bean是线程安全的吗？

先说答案：**Spring 不保证 bean 的线程安全，** spring 的bean 线程安全取决于不同的条件。

spring 中的 bean 的默认作用域是 singleton，所以多个线程抢占同一个 bean 的使用权，肯定会出问题的；

把 spring 的 bean 作用域改为 prototype 后，就是单个线程操作一个 bean ，所以又不会出现线程安全问题；

但是有一些 bean 是无状态的，（不同的线程不会对这个bean 进行查询以外的操作，即不会存在说修改属性等）那么这个单例的 bean 就是线程安全的，比如 Conroller 类， Service 和 DAO；

#### 那么如何解决有状态的 bean 的线程安全问题呢？

1. 最浅显的解决办法就是将有状态的bean的作用域由`singleton`改为`prototype`。
2. 采用ThreadLocal解决线程安全问题，为每个线程提供一个独立的变量副本，不同线程只操作自己线程的副本变量。（关于 ThreadLocal 需要补课的小伙伴请自行补课，后期我也会出相关的知识点总结，一起学习，一起进步）。



### 9. Spring基于xml注入bean的几种方式：

1. 构造注入（通过 index，通过类型）
2. set 注入（需要显式定义 setter 方法）
3. 静态工厂注入；
4. 实例工厂；

参考博客：https://blog.csdn.net/a745233700/article/details/89307518

### 10. Spring 自动装配

spring 配置文件中 <bean> 节点的 `autowire` 参数可以控制 bean 自动装配的方式

1. 在 XML 中有5种装配方式：

   1. no(default)：默认方式是不装配，通过人工指定 ref 属性进行装配 bean
   2. byName ： 根据名称进行装配，如果一个bean的 property 与另一bean 的name 相同，就进行自动装配。 
   3. byType - 根据参数的数据类型进行自动装配。
   4. constructor - 根据构造函数进行装配
   5. autodetect：自动探测，如果有构造方法，通过 construct的方式自动装配，否则使用 byType的方式自动装配。

2. 基于注解的自动装配方法：

   使用`@Autowired`、`@Resource`注解来自动装配指定的bean。

   在使用`@Autowired`注解之前需要在Spring配置文件进行配置，`<context:annotation-config />`。

   在启动spring IoC时，容器自动装载了一个AutowiredAnnotationBeanPostProcessor后置处理器，当容器扫描到@Autowied、@Resource或@Inject时，就会在IoC容器自动查找需要的bean，并装配给该对象的属性。在使用@Autowired时，首先在容器中查询对应类型的bean：

   ​	如果查询结果刚好为一个，就将该bean装配给@Autowired指定的数据；

   ​	如果查询的结果不止一个，那么@Autowired会根据名称来查找；

   ​	如果上述查找的结果为空，那么会抛出异常。解决方法时，使用required=false。

> @Autowired可用于：构造函数、成员变量、Setter方法
>
> 注：@Autowired和@Resource之间的区别：
>
> (1) @Autowired默认是按照类型装配注入的，默认情况下它要求依赖对象必须存在（可以设置它required属性为false）。
>
> (2) @Resource默认是按照名称来装配注入的，只有当找不到与名称匹配的bean才会按照类型来装配注入。

### 11. Spring 如何解决循环依赖问题

循环依赖的几种情况如下：

1. 通过构造方法进行依赖注入的时候的循环依赖问题
2. 通过set 注入产生的循环依赖问题（多例模式）
3. 通过set 注入产生的循环以来问题（单例模式）

目前解决在单例模式下的 set 注入的循环依赖问题，主要是通过二级缓存和三级缓存进行处理，在对象实例化之后，依赖注入之前，Spring 先将 Bean实例的引用存储在第三级缓存中。

关于三级缓存可以参考这篇文章：https://blog.csdn.net/a745233700/article/details/110914620

对于构造注入的循环依赖可以使用延迟加载的方案进行解决：

对于类A和类B都是通过构造器注入的情况，可以在A或者B的构造函数的形参上加个@Lazy注解实现延迟加载。当实例化对象时，如果发现参数或者属性有@Lazy注解修饰，那么就不直接创建所依赖的对象了，而是使用动态代理创建一个代理类。在注入依赖时，类A并没有完全的初始化完，实际上注入的是一个类B的代理对象，只有当类A首次被使用的时候才会被完全的初始化。



### 12. Spring 事务

**Spring 事务的本质就是对数据库事务的支持。**

事务具备ACID四种特性，ACID，Atomic（原子性）、Consistency（一致性）、Isolation（隔离性）和Durability（持久性）

#### 事务的传播特性：

事务传播行为就是多个事务方法互相调用时，如果定义不同事务之间的一个传播规则，Spring中定义了7种：

1. propagation_requierd：如果当前没有事务，那么就新建一个事务，如果当前已经存在事务，那么就加入到这个事务中。默认！
2. propagation_supports：支持当前事务，如果没有当前事务，那么就以非事务的方式进行；
3. propagation_mandatory：强制使用当前事务，如果当前没有事务，那么就抛出异常；
4. propagation_required-new：新建事务，如果当前存在事务，那么就把当前事务挂起；
5. propagation_not_supports：不支持事务，以非事务的方式进行，如果当前存在事务，那么就把当前事务挂起；
6. propagation_never：以非事务方式执行，如果当前存在事务则抛出异常；
7. propagation_nested：嵌套事务，如果当前存在事务则在当前事务的内部进行嵌套然后进行，如果当前没有事务则按照默认的 required 方式进行；

#### 事务隔离级别：

一般说到事务隔离级别就会想到数据在没有事务隔离关系的时候会发生什么问题，而我们的事务隔离级别就是针对这些问题提出的解决方案：

- 脏读：指当一个事务正在访问数据，并且对数据做了修改，此时还没有提交到数据库中，被另外的一个事务访问了该条数据然后使用了还没有提交回去的数据。
- 不可重复读：在一个事务内，多次对同一数据进行读取，产生结果不一致的情况；当前事务还没有结束，另一个事务对当前事务操作过的数据进行了修改，然后当前事务再次对数据进行读取的时候出现了前后读取数据不一致的问题。称之为不可重复读。
- 幻影读：一个事务先后读取某个范围内的数据记录，但是两次读取的记录数不一样，称之为幻读（两次执行同一条 select 语句会出现不同的结果，第二次读会增加一数据行，并没有说这两次执行是在同一个事务中）。

那么针对上面出现的几种常见的问题，提出事务隔离级别的的概念：

1. read uncommitted：读未提交，允许另一个事务看到当前事务还没有提交的数据，这样很容易出问题，一般不使用；针对上面的现象啥问题都解决不了。
2. read committed：读已提交，保证当前事务被提交后才能被别的事务读取，另一个事务不能够读取还没有提交的数据；能够解决脏读问题。
3. read repeatable：可重复读，保证了脏读和不可重复读的现象不会出现数据问题，但是可能会出现幻读。
4. serializable：序列化，可以保证上面的所有情况不会发生，但是相对实现比较复杂，这里先不过多阐述，解决方案MVCC，可以参考这篇文章：[MySQL的MVCC及实现原理](https://blog.csdn.net/SnailMann/article/details/94724197?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522160006254319725254053451%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=160006254319725254053451&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v3~pc_rank_v3-1-94724197.pc_ecpm_v3_pc_rank_v3&utm_term=MVCC&spm=1018.2118.3001.4187s)

#### Spring 中的隔离级别：

1. ISOLATION_DEFAULT：这是个 PlatfromTransactionManager 默认的隔离级别，使用数据库默认的事务隔离级别。

2. ISOLATION_READ_UNCOMMITTED：读未提交，允许事务在执行过程中，读取其他事务未提交的数据。

3. ISOLATION_READ_COMMITTED：读已提交，允许事务在执行过程中，读取其他事务已经提交的数据。

4. ISOLATION_REPEATABLE_READ：可重复读，在同一个事务内，任意时刻的查询结果都是一致的。

5.  ISOLATION_SERIALIZABLE：所有事务逐个依次执行。

#### Spring 事务的种类：

声明式事务最大的优点就是不需要在业务逻辑代码中掺杂事务管理的代码，只需在配置文件中做相关的事务规则声明或通过@Transactional注解的方式后者 AOP切面的方式，便可以将事务规则应用到业务逻辑中，减少业务代码的污染。唯一不足地方是，最细粒度只能作用到方法级别，无法做到像编程式事务那样可以作用到代码块级别。
1. 编程式事务管理 `TransactionTemplate`  编程式事务现在基本不怎么用，但是也有存在的意义，这里有一个编程式事务的 [样例](https://blog.csdn.net/meism5/article/details/90446733)

   https://www.jianshu.com/p/bc6396926ed7

2. 基于 `TransactionProxyFactoryBean` 的声明式事务

3. 基于注解 `@Transactional` 的声明式事务

4. 基于AspectJ 的 AOP声明式事务 

具体实战可以参考这篇博客：https://blog.csdn.net/chinacr07/article/details/78817449



### 13. Spring 中常用的设计模式

参考文章：https://juejin.cn/post/6905741110607462413

1. 工厂模式：使用 BeanFactory 创建 bean实例
2. 单例模式：bean模式是单例的
3. 策略模式：例如Resource 的实现类，针对不同的资源文件，实现了不同的资源获取方法
4. 代理模式：动态代理，静态代理，在AOP中使用的是基于接口的实现的JDK动态代理和基于继承实现的CGLIB动态代理，（扯远了，还有静态代理的 AspectJ，性能更优，一般面试会问到一些细节）
5. 模板方法：可以将相同部分的代码放在父类中，而将不同的代码放入不同的子类中，用来解决代码重复的问题。比如 RestTemplate, JmsTemplate, JpaTemplate
6. 适配器模式：Spring AOP的增强或通知（Advice）使用到了适配器模式，Spring MVC中也是用到了适配器模式适配Controller
7. 观察者模式：Spring事件驱动模型就是观察者模式的一个经典应用。
8. 桥接模式：可以根据客户的需求能够动态切换不同的数据源。比如我们的项目需要连接多个数据库，客户在每次访问中根据需要会去访问不同的数据库

