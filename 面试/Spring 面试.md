#### 1、Spring 是什么？使用 Spring 有什么好处？

 Spring 是一个轻量级的基于 IOC（Inverse of Control：控制反转）和 AOP（Aspect Oriented Programming：面向切面编程）的容器框架，并且它是开源的，为了简化企业级开发而生。通过 Spring，可以使 JavaEE 开发更容易使用，并且通过使基于 POJO 的编程模型，促进良好的编程习惯。 

好处：

1. 轻量：Spring 是轻量的，基本的版本大约 2MB。
2. 控制反转：Spring 通过控制反转实现了松散耦合，对象们给出它们的依赖，而不是创建或查找依赖的对象们。
3. 面向切面的编程(AOP)：Spring 支持面向切面的编程，并且把应用业务逻辑和系统服务分开。
4. 容器：Spring 包含并管理应用中对象的生命周期和配置。
5. MVC 框架：Spring 的 WEB 框架是个精心设计的框架，是 Web 框架的一个很好的替代品。
6. 事务管理：Spring 提供一个持续的事务管理接口，可以扩展到上至本地事务下至全局事务（JTA）。
7. 异 常 处 理 ： Spring 提 供 方 便 的 API 把 具 体 技 术 相 关 的 异 常 （ 比 如 由 JDBC ，Hibernate or JDO 抛出的）转化为一致的 unchecked 异常。

#### 2、Spring 由哪些模块组成?

Spring 框架是一个分层架构，由 7 个定义良好的模块组成。Spring 模块构建在核心容器之上，核心容器定义了创建、配置和管理 bean 的方式。 

1. Spring Core：核心容器， 提供 Spring 框架的基本功能 。其基本组件为 BeanFactory。Spring 框架建立在此模块之上，它使 Spring 成为一
    个容器。
2. Spring Context：Spring 上下文是一个配置文件， 向 Spring 框架提供上下文信息。 
3. Spring AOP： Spring AOP 模块直接将面向切面的编程功能 , 集成到了 Spring 框架中。所以，可以很容易地使 Spring 框架管理任何支持 AOP 的对象 ，Spring AOP 模块为基于 Spring 的应用程序中的对象提供了事务管理服务。通过使用 Spring AOP，不用依赖组件，就可以将声明性事务管理集成到应用程序中。
4. Spring DAO：通过使用 JDBC 抽象和 DAO 模块，保证数据库代码的简洁，并能避免数据库资源错误关闭导致的问题，它在各种不同的数据库的错误信息之上，提供了一个统一的异常访问层。它还利用 Spring 的 AOP 模块给 Spring 应用中的对象提供事务管理服务。
5. Spring ORM：Spring 通过提供 ORM 模块，支持我们在直接 JDBC 之上使用一个对象/关系映射映射(ORM)工具，Spring 支持集成主流的 ORM 框架，如 Hiberate,JDO 和 iBATISSQL Maps。Spring 的事务管理同样支持以上所有 ORM 框架及 JDBC。
6. Spring Web 模块：Spring 的 WEB 模块是构建在 application context 模块基础之上，提供一个适合 web 应用的上下文。这个模块也包括支持多种面向 web 的任务，如透明地处理多个文件上传请求和程序级请求参数的绑定到你的业务对象。它也有对 Jakarta Struts 的支持。
7. Spring MVC 框架：MVC 框架是一个全功能的构建 Web 应用程序的 MVC 实现。通过策略接口，MVC 框架变成为高度可配置的，MVC 容纳了大量视图技术，其中包括 JSP、Velocity、Tiles、iText 和 POI。 

#### 3、BeanFactory 实现举例

Bean 工厂是工厂模式的一个实现，提供了控制反转功能，用来把应用的配置和依赖从应用代码中分离。最常用的 BeanFactory 实现是XmlBeanFactory 类。

最常用的就是 org.springframework.beans.factory.xml.XmlBeanFactory ，它根据 XML 文件中的定义加载 beans。该容器从 XML 文件读取配置元数据并用它去创建一个完全配置的系统或应用。

#### 4、Spring 配置文件

Spring 配置文件是个 XML 文件，这个文件包含了类信息，描述了如何配置它们，以及如何相互调用。

#### 5、什么是 Spring IOC 容器？

Spring IOC 负责创建对象，管理对象（通过依赖注入（DI），装配对象，配置对象，并且管理这些对象的整个生命周期。

#### 6、IOC 的优点是什么？

IOC 或 依赖注入把应用的代码量降到最低。它使应用容易测试，单元测试不再需要单例和 JNDI 查找机制。最小的代价和最小的侵入性使松散耦合得以实现。IOC 容器支持加载服务时的饿汉式初始化和懒加载。

#### 7、ApplicationContext 通常的实现是什么？

- FileSystemXmlApplicationContext ：它是从磁盘路径上加载配置文件，配置文件可以在磁盘的任意位置，且在初始化时就创建单例的bean，也可以通过配置的方式指定创建的Bean是多实例的。
- ClassPathXmlApplicationContext：它是从类的根路径下加载配置文件，推荐使用。
- AnnotationConfigApplicationContext: 当我们使用注解配置容器对象时，需要使用此类来创建 spring 容器。它用来读取注解。

#### 8、Bean 工厂和 Applicationcontexts 有什么区别？

- BeanFactory 是 Spring 容器中最顶层的接口，提供了最简单的容器的功能，只提供了实例化对象和拿对象的功能，在启动的时候不会去实例化 Bean，只有从容器中拿 Bean 的时候才会去实例化。
- ApplicationContext 是 BeanFactory 的子接口，是 Spring 中更高级的容器，功能很多，比如 AOP 等，ApplicationContext 在启动的时候就把所有的 Bean 全部实例化了，它还可以为 Bean 配置 lazy-init=true 来让Bean延迟实例化。

#### 9、一个 Spring 的应用看起来象什么？

- 一个定义了一些功能的接口
- 这实现包括属性，它的 Setter ， getter 方法和函数等
- Spring AOP
- Spring 的 XML 配置文件
- 使用以上功能的客户端程序

#### 10、什么是 Spring 的依赖注入？

依赖注入，是 IOC 的一个方面，是个通常的概念，它有多种解释。这概念是说你不用创建对象，而只需要描述它如何被创建。你不在代码里直接组装你的组件和服务，但是要在配置文件里描述哪些组件需要哪些服务，之后一个容器（IOC 容器）负责把他们组装起来。

#### 11、有哪些不同类型的 IOC（依赖注入）方式？

- 构造器依赖注入：构造器依赖注入通过容器触发一个类的构造器来实现的，该类有一系列参数，每个参数代表一个对其他类的依赖。
- Setter 方法注入：Setter 方法注入是容器通过调用无参构造器或无参 static 工厂 方法实例化 bean 之后，调用该 bean 的 setter 方法，即实现了基于 setter 的依赖注入。
- p 空间注入或 c 空间注入。

#### 12、哪种依赖注入方式你建议使用，构造器注入，还是 Setter 方法注入？

你两种依赖方式都可以使用，构造器注入和 Setter 方法注入。最好的解决方案是用构造器参数实现强制依赖，setter 方法实现可选依赖。

#### 13、什么是 Spring beans？

Spring beans 是那些形成 Spring 应用的主干的 java 对象。它们被 Spring IOC 容器初始化，装配，和管理。这些 beans 通过容器中配置的元数据创建。比如，以 XML 文件中 <bean/> 的形式定义。

Spring 框架定义的 beans 都是单件 beans。在 bean tag 中有个属性”singleton”，如果它被赋为 TRUE，bean 就是单件，否则就是一个 prototype bean。默认是 TRUE，所以所有在 Spring 框架中的 beans 缺省都是单件。

#### 14、一个 Spring Bean 定义 包含什么？

一个 Spring Bean 的定义包含容器必知的所有配置元数据，包括如何创建一个bean，它的生命周期详情及它的依赖。

#### 15、如何给 Spring 容器提供配置元数据？

这里有三种重要的方法给 Spring 容器提供配置元数据。

- XML 配置文件。
- 基于注解的配置。
- 基于 java 的配置。

#### 16、你怎样定义类的作用域？

当定义一个 <bean> 在 Spring 里，我们还能给这个 bean 声明一个作用域。它可以通过 bean 定义中的 scope 属性来定义。如，当 Spring 要在需要的时候每次生产一个新的 bean 实例，bean 的 scope 属性被指定为 prototype。另一方面，一个 bean每 次 使 用 的 时 候 必 须 返 回 同 一 个 实 例 ， 这 个 bean 的 scope 属 性 必 须 设 为 singleton。

Spring 框架支持以下五种 bean 的作用域：

- singleton : bean 在每个 Spring ioc 容器中只有一个实例
-  prototype：一个 bean 的定义可以有多个实例。 
- request：每次 http 请求都会创建一个 bean，该作用域仅在基于 web 的 SpringApplicationContext 情形下有效。 
- session：在一个 HTTP Session 中，一个 bean 定义对应一个实例。该作用域仅在基于 web 的 Spring ApplicationContext 情形下有效。 
- global-session：在一个全局的 HTTP Session 中，一个 bean 定义对应一个实例。该作用域仅在基于 web 的 Spring ApplicationContext 情形下有效。 
- 缺省的 Spring bean 的作用域是 Singleton。

#### 17、Spring 框架中的单例 bean 是线程安全的吗？

不，Spring 框架中的单例 bean 不是线程安全的。

#### 18、解释 Spring 框架中 bean 的生命周期

#### 19、什么是 Spring 的内部 bean？

当一个 bean 仅被用作另一个 bean 的属性时，它能被声明为一个内部 bean，为了定义 inner bean，在 Spring 的基于 XML 的 配置元数据中，可以在 <property/> 或 <constructor-arg/> 元素内使用<bean/> 元素，内部 bean 通常是匿名的，它们的 Scope 一般是 prototype。

#### 20、在 Spring 中如何注入一个 java 集合？

Spring 提供以下几种集合的配置元素：

- <list>类型用于注入一列值，允许有相同的值。
- <set> 类型用于注入一组值，不允许有相同的值。
-  <map> 类型用于注入一组键值对，键和值都可以为任意类

- <props> 类型用于注入一组键值对，键和值都只能为 String 类型。

#### 21、什么是 bean 装配？

装配，或 bean 装配是指在 Spring 容器中把 bean 组装到一起，前提是容器需要知道 bean 的依赖关系，如何通过依赖注入来把它们装配到一起。

#### 22、什么是 bean 的自动装配？

Spring 容 器 能 够 自 动 装 配 相 互 合 作 的 bean ， 这 意 味 着 容 器 不 需 要 <constructor-arg> 和 <property> 配置，能通过 Bean 工厂自动处理 bean 之间的协作。

#### 23、解释 byName 和 byType？

- byName：通过参数名 自动装配，Spring 容器在配置文件中发现 bean 的 autowire属性被设置成 byname，之后容器试图匹配、装配和该 bean 的属性具有相同名字的 bean。

- byType：通过参数类型自动装配，Spring 容器在配置文件中发现 bean 的 autowire 属性被设置成 byType，之后容器试图匹配、装配和该 bean 的属性具有相同类型的 bean。如果有多个 bean 符合条件，则抛出错误

#### 24、你可以在 Spring 中注入一个 null 和一个空字符串吗？

可以。

#### 25、什么是基于 Java 的 Spring 注解配置? 给一些注解的例子

基于 Java 的配置，允许你在少量的 Java 注解的帮助下，进行你的大部分 Spring配置而非通过 XML 文件。以@Configuration 注解为例，它用来标记类可以当做一个 bean 的定义，被 SpringIOC 容器使用。另一个例子是@Bean 注解，它表示此方法将要返回一个对象，作为一个 bean 注册进 Spring 应用上下文。

#### 26、什么是基于注解的容器配置？

开发者通过在相应的类，方法或属性上使用注解的方式，直接组件类中进行配置，而不是使用 xml 表述 bean 的装配关系。

#### 27、怎样开启注解装配？

注解装配在默认情况下是不开启的，为了使用注解装配，我们必须在 Spring 配置文件中配置  <context:annotation-config/>元素。

#### 28、@Autowired 注解 和 @Qualifier 注解

- @Autowired 注解提供了更细粒度的控制，包括在何处以及如何完成自动装配。它的用法和@Required 一样，修饰 setter 方法、构造器、属性或者具有任意名称和/或多个参数的 PN 方法。

- 当 有 多 个 相同 类 型 的 bean 却 只 有 一 个需 要 自 动 装 配 时 ，将 @Qualifier 注 解 和 @Autowire 注解结合使用以消除这种混淆，指定需要装配的确切的 bean。点击这里学习更多常用注解。 

#### 29、