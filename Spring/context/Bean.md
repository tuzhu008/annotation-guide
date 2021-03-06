# @Bean

## 定义

```java
@Target({ElementType.METHOD, ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Bean {

    @AliasFor("name")
    String[] value() default {};

    @AliasFor("value")
    String[] name() default {};

    @Deprecated
    Autowire autowire() default Autowire.NO;

    boolean autowireCandidate() default true;

    String initMethod() default "";

    String destroyMethod() default AbstractBeanDefinition.INFER_METHOD;

}
```

## 解析

`@Bean` 是一个方法级注解，声明当前方法的返回值为一个 Bean。

`@Bean` 注解用来说明通过 Spring IoC 容器来管理一个新对象的实例化，配置和初始化的方法。

可以在任何使用 `@Component` 的地方使用 `@Bean`，但是更常用的是在配置 `@Configuration` 的类中使用。

`@Bean` 标记了一个实例化 Spring bean 的**工厂**方法:

```java
@Bean
Engine engine() {
    return new Engine();
}
```

当需要返回类型的新实例时，Spring 调用这些方法。

默认情况下，生成的 bean 具有与工厂方法相同的名称。如果我们想给它起个不同的名字，我们可以用这个注释的 `name` 或者 `value` 参数\(参数 `value` 是参数 `name`的别名\)：

```java
@Bean("engine")
Engine getEngine() {
    return new Engine();
}
```

#### **Bean 依赖**

`@Bean` 注解方法可以具有描述构建该 bean 所需依赖关系的任意数量的参数。例如，如果我们的 Engine 需要一个 Gear，我们可以通过一个方法参数实现该依赖：

```java
@Configuration
public class AppConfig {

    @Bean
    public Engine engine(Gear gear) {
        return new Engine(Gear gear);
    }
}
```

### **生命周期回调**

任何使用了 `@Bean` 定义了的类都支持常规生命周期回调，并且可以使用 JSR-250 中的 `@PostConstruct` 和 `@PreDestroy` 注解，详细信息，参考JSR-250注解。

完全支持常规的 Spring 生命周期回调。如果一个 bean 实现了 `InitializingBean`，`DisposableBean` 或 `Lifecycle` 接口，它们的相关方法就会被容器调用。

完全支持 \*Aware 系列的接口，例如：`BeanFactoryAware`，`BeanNameAware`，`MessageSourceAware`，`ApplicationContextAware` 等。

`@Bean` 注解支持任意的初始化和销毁回调方法，这与 Spring XML 中bean元素上的 init 方法和 destroy-method 属性非常相似：

```java
public class Foo {
    public void init() {
        // initialization logic
    }
}

public class Bar {
    public void cleanup() {
        // destruction logic
    }
}

@Configuration
public class AppConfig {

    @Bean(initMethod = "init")
    public Foo foo() {
        return new Foo();
    }

    @Bean(destroyMethod = "cleanup")
    public Bar bar() {
        return new Bar();
    }

}
```

默认情况下，使用 Java config 定义的具有公开关闭或停止方法的 bean 将自动加入销毁回调。如果你有一个公开的关闭或停止方法，但是你不希望在容器关闭时被调用，只需将 `@Bean(destroyMethod="")` 添加到你的 bean 定义中即可禁用默认（推测）模式。 默认情况下，您可能希望通过 JNDI 获取资源，因为它的生命周期在应用程序之外进行管理。特别地，请确保始终为 DataSource 执行此操作，因为它已知在 Java EE 应用程序服务器上有问题。

```java
@Bean(destroyMethod = "")
public DataSource dataSource() throws NamingException {
    return (DataSource) jndiTemplate.lookup("MyDS");
}
```

另外，通过 `@Bean` 方法，通常会选择使用编程来进行 JNDI 查找：要么使用 Spring 的 JndiTemplate/JndiLocatorDelegate 帮助类，要么直接使用 JNDI InitialContext，但不能使用 JndiObjectFactoryBean 变体来强制将返回类型声明为 FactoryBean 类型以代替目标的实际类型，它将使得在其他 `@Bean` 方法中更难用于交叉引用调用这些在此引用提供资源的方法。

当然上面的 Foo 例子中，在构造期间直接调用 `init()` 方法同样有效：

```java
@Configuration
public class AppConfig {
    @Bean
    public Foo foo() {
        Foo foo = new Foo();
        foo.init();
        return foo;
    }

    // ...

}
```

当您直接在 Java 中工作时，您可以对对象执行任何您喜欢的操作，并不总是需要依赖容器生命周期！

### **指定bean的作用域**

#### **使用@Scope注解**

你可以指定 `@Bean` 注解定义的 bean 应具有的特定作用域。你可以使用 Bean 作用域章节中的任何标准作用域。

默认的作用域是单例，但是你可以用 `@Scope` 注解重写作用域。

```java
@Configuration
public class MyConfiguration {

    @Bean
    @Scope("prototype")
    public Encryptor encryptor() {
        // ...
    }

}
```

**@Scope和 scope 代理**

Spring 提供了一个通过范围代理来处理范围依赖的便捷方法。使用 XML 配置创建此类代理的最简单方法是元素。使用 `@Scope` 注解配置 Java 中的 bean 提供了与 proxyMode 属性相似的支持。默认是没有代理（ScopedProxyMode.NO），但您可以指定ScopedProxyMode.TARGET\_CLASS 或 ScopedProxyMode.INTERFACES。

如果你使用 Java 将 XML 参考文档（请参阅上述链接）到范围的 @Bean 中移植范围限定的代理示例，则它将如下所示  
如果你将 XML 参考文档的 scoped 代理示例转化为 Java @Bean，如下所示：

```java
// an HTTP Session-scoped bean exposed as a proxy
@Bean
@SessionScope
public UserPreferences userPreferences() {
    return new UserPreferences();
}

@Bean
public Service userService() {
    UserService service = new SimpleUserService();
    // a reference to the proxied userPreferences bean
    service.setUserPreferences(userPreferences());
    return service;
```

### **自定义 Bean 命名**

默认情况下，配置类使用 `@Bean` 方法的名称作为生成的 bean 的名称。但是，可以使用 name 属性来重写此功能。

```java
@Configuration
public class AppConfig {

    @Bean(name = "myFoo")
    public Foo foo() {
        return new Foo();
    }

}
```

### **Bean别名**

有时一个单一的 bean 需要给出多个名称，称为 bean 别名。 为了实现这个目标，`@Bean` 注解的 name 属性接受一个 String 数组。

```java
@Configuration
public class AppConfig {

    @Bean(name = { "dataSource", "subsystemA-dataSource", "subsystemB-dataSource" })
    public DataSource dataSource() {
        // instantiate, configure and return DataSource bean...
    }

}
```

### **Bean描述**

有时候需要提供一个详细的 bean 描述文本是非常有用的。当对 bean 暴露（可能通过 JMX）进行监控使，特别有用。

可以使用 `@Description` 注解对 Bean 添加描述：

```java
@Configuration
public class AppConfig {

    @Bean
    @Description("Provides a basic example of a bean")
    public Foo foo() {
        return new Foo();
    }

}
```



