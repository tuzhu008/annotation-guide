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



