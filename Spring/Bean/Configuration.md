# @_Configuration_

## 定义

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Configuration {

    /**
     * Explicitly specify the name of the Spring bean definition associated with the
     * {@code @Configuration} class. If left unspecified (the common case), a bean
     * name will be automatically generated.
     * <p>The custom name applies only if the {@code @Configuration} class is picked
     * up via component scanning or supplied directly to an
     * {@link AnnotationConfigApplicationContext}. If the {@code @Configuration} class
     * is registered as a traditional XML bean definition, the name/id of the bean
     * element will take precedence.
     * @return the explicit component name, if any (or empty String otherwise)
     * @see org.springframework.beans.factory.support.DefaultBeanNameGenerator
     */
    @AliasFor(annotation = Component.class)
    String value() default "";

}
```

## 解析

`@Configuration` 是一个类级别的注解，指明此对象是 bean 定义的源。`@Configuration` 类通过 public `@Bean` 注解的方法来声明 bean。在 `@Configuration` 类上对 `@Bean` 方法的调用也可以用于定义 bean 之间的依赖。

`@Configuration` 注解的类说明这个类的主要是作为一个 bean 定义的资源文件。

**@Configuration 类最终只是容器中的一个 bean。**

_Configuration _类可以包含用 @Bean 注解的 bean 工厂方法:

```java
@Configuration
class VehicleFactoryConfig {

    @Bean
    Engine engine() {
        return new Engine();
    }

}
```

上面的 VehicleFactoryConfig 类和 Spring XML 的配置是等价的：

```xml
<beans>
    <bean id="engine" class="com.aho.services.Engine"/>
</beans>
```

### **bean依赖注入**

当 `@Beans` 相互依赖时，表示依赖关系就像一个 bean 方法调用另一个方法一样简单：

```java
@Configuration
public class AppConfig {

    @Bean
    public Foo foo() {
        return new Foo(bar());
    }

    @Bean
    public Bar bar() {
        return new Bar();
    }

}
```

上面的例子，foo 接受一个 bar 的引用来进行构造器注入：

> 这种方法声明的 bean 的依赖关系只有在 `@Configuration` 类的 `@Bean` 方法中有效。你**不能**在 `@Component` 类中来声明 bean 的依赖关系。

### **方法查找注入**

如前所述，方法查找注入是一个你很少用用到的高级特性。在单例的 bean 对原型的 bean 有依赖性的情况下，它非常有用。这种类型的配置使用，Java 提供了实现此模式的自然方法。

```java
public abstract class CommandManager {
    public Object process(Object commandState) {
        // grab a new instance of the appropriate Command interface
        Command command = createCommand();
        // set the state on the (hopefully brand new) Command instance
        command.setState(commandState);
        return command.execute();
    }

    // okay... but where is the implementation of this method?
    protected abstract Command createCommand();
}
```

使用 Java 支持配置，您可以创建一个 CommandManager 的子类，覆盖它抽象的 `createCommand()` 方法，以便它查找一个新的（原型）命令对象：

```java
@Bean
@Scope("prototype")
public AsyncCommand asyncCommand() {
    AsyncCommand command = new AsyncCommand();
    // inject dependencies here as required
    return command;
}

@Bean
public CommandManager commandManager() {
    // return new anonymous implementation of CommandManager with command() overridden
    // to return a new prototype Command object
    return new CommandManager() {
        protected Command createCommand() {
            return asyncCommand();
        }
    }
}
```

### **有关基于Java配置内部如何工作的更多信息**

下面的例子展示了一个 `@Bean` 注解的方法被调用两次：

```java
@Configuration
public class AppConfig {

    @Bean
    public ClientService clientService1() {
        ClientServiceImpl clientService = new ClientServiceImpl();
        clientService.setClientDao(clientDao());
        return clientService;
    }

    @Bean
    public ClientService clientService2() {
        ClientServiceImpl clientService = new ClientServiceImpl();
        clientService.setClientDao(clientDao());
        return clientService;
    }

    @Bean
    public ClientDao clientDao() {
        return new ClientDaoImpl();
    }

}
```

`clientDao()` 方法被 `clientService1()` 和 `clientService2()` 各自调用了一次。因为这个方法创建并返回了一个新的 ClientDaoImpl 实例，你通常期望会有2个实例（每个服务各一个）。这有一个明显的问题：在 Spring 中，bean 实例默认情况下是单例。神奇的地方在于：所有的 `@Configuration` 类在启动时都使用 CGLIB 进行子类实例化。在子类中，子方法在调用父方法创建一个新的实例之前会首先检查任何缓存\(作用域\)的 bean。注意，从 Spring 3.2 开始，不再需要将 CGLIB 添加到类路径中，因为 CGLIB 类已经被打包在 org.springframework.cglib 下，直接包含在 spring-core JAR 中。

> 根据不同的 bean 作用域，它们的行为也是不同的。我们这里讨论的都是单例模式。
>
> 这里有一些限制是由于 CGLIB 在启动时动态添加的特性，特别是配置类都不能是final类型。然而从 4.3 开始，配置类中允许使用任何构造函数，包含 `@Autowired` 使用或单个非默认构造函数声明进行默认注入。如果你希望避免 CGLIB 带来的任何限制，那么可以考虑子在非 `@Configuration` 注解类中声明 `@Bean` 注解方法。例如，使用 `@Component` 注解类。在 `@Bean` 方法之间的交叉调用不会被拦截，所以你需要在构造器或者方法级别上排除依赖注入。

## 详述

用于指示一个配置类，该类声明一个或多个 @Bean 方法，并触发自动配置和组件扫描。

```java
 @Configuration
 public class AppConfig {

     @Bean
     public MyBean myBean() {
         // instantiate, configure and return bean ...
     }
 }
```

### 启动 @Configuration 类

#### 通过 `AnnotationConfigApplicationContext`

`@Configuration` 类通常使用 `AnnotationConfigApplicationContext` 或其支持 web 的变体`AnnotationConfigWebApplicationContext` 引导。关于前者的一个简单例子如下:

```java
 AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext();
 ctx.register(AppConfig.class);
 ctx.refresh();
 MyBean myBean = ctx.getBean(MyBean.class);
 // use myBean ...
```

#### 通过 Spring `<beans>`XML

作为直接针对注释 `AnnotationConfigApplicationContext` 注册 `@Configuration` 类的替代方法，`@Configuration` 类可以在Spring XML 文件中声明为普通的 &lt;bean&gt; 定义:

```java
 <beans>
    <context:annotation-config/>
    <bean class="com.acme.AppConfig"/>
 </beans>
```

在上面的例子中，`<context:annotation-config/>` 是必需的，以便启用 `ConfigurationClassPostProcessor` 和其他与注解相关的 post 处理器，这些处理器可以方便地处理 `@Configuration` 类。

#### 通过组件扫描

`@Configuration` 使用 `@Component` 元注解，因此 `@Configuration` 类是组件扫描的候选对象\(通常使用 Spring XML 的`<context:component-scan/>` 元素\)，因此也可以像任何常规的 `@Component` 一样利用 `@Autowired`/`@Inject`。特别是，如果只有一个构造函数存在，自动装配语义将透明地应用于该构造函数:

```java
 @Configuration
 public class AppConfig {

     private final SomeBean someBean;

     public AppConfig(SomeBean someBean) {
         this.someBean = someBean;
     }

     // @Bean definition using "SomeBean"

 }
```

`@Configuration` 类不仅可以使用组件扫描引导，还可以自己使用 `@ComponentScan` 注解配置组件扫描:

```java
 @Configuration
 @ComponentScan("com.acme.app.services")
 public class AppConfig {
     // various @Bean definitions ...
 }
```

### 使用外部值

#### 使用 `Environment` Api

外部值可以通过将 Spring `Environment`注入到一个 `@Configuration` 类来查找——例如，使用 `@Autowired` 注解:

```java
 @Configuration
 public class AppConfig {

     @Autowired Environment env;

     @Bean
     public MyBean myBean() {
         MyBean myBean = new MyBean();
         myBean.setName(env.getProperty("bean.name"));
         return myBean;
     }
 }
```

通过 `Environment` 解析的属性驻留在一个或多个“属性源”对象中，`@Configuration` 类可以使用 `@PropertySource` 注解将属性源贡献给环境对象:

```java
 @Configuration
 @PropertySource("classpath:/com/acme/app.properties")
 public class AppConfig {

     @Inject Environment env;

     @Bean
     public MyBean myBean() {
         return new MyBean(env.getProperty("bean.name"));
     }
 }
```

#### 使用`@Value` 注解

可以使用 `@Value` 注解将外部值注入 `@Configuration` 类:

```
 @Configuration
 @PropertySource("classpath:/com/acme/app.properties")
 public class AppConfig {

     @Value("${bean.name}") String beanName;

     @Bean
     public MyBean myBean() {
         return new MyBean(beanName);
     }
 }
```

这种方法通常与 Spring 的 `PropertySourcesPlaceholderConfigurer` 一起使用，后者可以通过 `<context:property-placeholder/>`在 XML 配置中自动启用，也可以通过专用的静态 `@Bean` 方法在 `@Configuration` 类中显式启用。但是，请注意，只有在需要自定义配置\(如占位符语法等\)时，才需要通过静态 `@Bean` 方法显式注册 `PropertySourcesPlaceholderConfigurer`。具体地说，如果没有 bean 后处理器\(例如 `PropertySourcesPlaceholderConfigurer`\)为 ApplicationContext 注册了嵌入式值解析器，Spring 将注册一个默认的嵌入式值解析器，它根据环境中注册的属性源解析占位符。

## Composing`@Configuration`classes

#### 使用 @Import 注解

`@Configuration` 类可以使用 `@Import` 注释组成，类似于 `<import>` 在 Spring XML 中工作的方式。因为 `@Configuration` 对象在容器中被管理为 Spring bean，所以导入的配置可能被注入——例如，通过构造函数注入:

```java
@Configuration
 public class DatabaseConfig {

     @Bean
     public DataSource dataSource() {
         // instantiate, configure and return DataSource
     }
 }

 @Configuration
 @Import(DatabaseConfig.class)
 public class AppConfig {

     private final DatabaseConfig dataConfig;

     public AppConfig(DatabaseConfig dataConfig) {
         this.dataConfig = dataConfig;
     }

     @Bean
     public MyBean myBean() {
         // reference the dataSource() bean method
         return new MyBean(dataConfig.dataSource());
     }
 }
```

现在 `AppConfig` 和导入的 `DatabaseConfig` 都可以通过在 Spring 上下文中只注册 AppConfig 来引导:

```java
 new AnnotationConfigApplicationContext(AppConfig.class);
```

#### 使用 @Profile 注释

`@Configuration` 类可能被标记为 `@Profile` 注释，以指示只有在给定的配置文件或配置文件处于活动状态时才应该处理它们:

```java
 @Profile("development")
 @Configuration
 public class EmbeddedDatabaseConfig {

     @Bean
     public DataSource dataSource() {
         // instantiate, configure and return embedded DataSource
     }
 }

 @Profile("production")
 @Configuration
 public class ProductionDatabaseConfig {

     @Bean
     public DataSource dataSource() {
         // instantiate, configure and return production DataSource
     }
 }
```

或者，您也可以在 `@Bean` 方法级别声明配置条件——例如，对于相同配置类中的其他 bean 变量:

```java
 @Configuration
 public class ProfileDatabaseConfig {

     @Bean("dataSource")
     @Profile("development")
     public DataSource embeddedDatabase() { ... }

     @Bean("dataSource")
     @Profile("production")
     public DataSource productionDatabase() { ... }
 }
```

#### 使用 @ImportResource 注解的 Spring XML

如上所述，`@Configuration` 类可以声明为 Spring XML 文件中的常规 Spring &lt;bean&gt; 定义。还可以使用 `@ImportResource` 注解将 Spring XML 配置文件导入 `@Configuration` 类。可以注入从 XML 导入的 Bean 定义——例如，使用 `@Inject` 注释:

```java
 @Configuration
 @ImportResource("classpath:/com/acme/database-config.xml")
 public class AppConfig {

     @Inject DataSource dataSource; // from XML

     @Bean
     public MyBean myBean() {
         // inject the XML-defined dataSource bean
         return new MyBean(this.dataSource);
     }
 }
```

#### 使用嵌套的 @Configuration 类

`@Configuration` 类可以相互嵌套，如下:

```java
 @Configuration
 public class AppConfig {

     @Inject DataSource dataSource;

     @Bean
     public MyBean myBean() {
         return new MyBean(dataSource);
     }

     @Configuration
     static class DatabaseConfig {
         @Bean
         DataSource dataSource() {
             return new EmbeddedDatabaseBuilder().build();
         }
     }
 }
```

在启动这样的配置时，只需要针对应用程序上下文注册 AppConfig。由于是一个嵌套的 `@Configuration` 类，`DatabaseConfig` 将自动注册。这避免了在 AppConfig 和 DatabaseConfig 之间的关系已经隐式清晰时使用 `@Import` 注解。

还请注意，嵌套的 `@Configuration` 类可以与 `@Profile` 注解一起使用，从而为封闭的 `@Configuration` 类提供相同 bean 的两个选项，效果很好。

### 延迟初始化配置

默认情况下，`@Bean`方法将在容器启动时急切地实例化。为了避免这种情况，可以将  `@Configuration` 与 `@Lazy` 注解一起使用，以指示默认情况下，类中声明的所有 `@Bean` 方法都是惰性初始化的。注意 `@Lazy` 也可以用于单个 `@Bean` 方法。

### 测试对 @Configuration 类的支持

Spring-test 模块中提供的 Spring TestContext 框架提供了 `@ContextConfiguration` 注解，该注解可以接受 `@Configuration` 类对象的数组:

```java
@RunWith(SpringRunner.class)
 @ContextConfiguration(classes = {AppConfig.class, DatabaseConfig.class})
 public class MyTests {

     @Autowired MyBean myBean;

     @Autowired DataSource dataSource;

     @Test
     public void test() {
         // assertions against myBean ...
     }
 }
```

### 使用 @Enable 注解启用内置的 Spring 特性

Spring 特性，如异步方法执行、计划任务执行、注解驱动的事务管理，甚至 Spring MVC，都可以使用它们各自的 `@Enable` 注解从`@Configuration` 类启用和配置。

### 编写 @Configuration 类时的约束

* 配置类必须作为类提供\(即不作为从工厂方法返回的实例提供\)，允许通过生成的子类进行运行时增强。

* 配置类必须是非 final 的。

* 配置类必须是非本地的\(即不能在方法中声明\)。

* 任何嵌套的配置类都必须声明为静态的。

* `@Bean` 方法可能不会创建更多的配置类\(任何这样的实例都将被视为普通 bean，它们的配置注解将保持未检测到\)。





