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



