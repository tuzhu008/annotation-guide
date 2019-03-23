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

完全支持常规的Spring生命周期回调。如果一个bean实现了InitializingBean，DisposableBean或Lifecycle接口，它们的相关方法就会被容器调用。

完全支持\*Aware系列的接口，例如：BeanFactoryAware，BeanNameAware，MessageSourceAware，ApplicationContextAware等。

@Bean注解支持任意的初始化和销毁回调方法，这与Spring XML 中bean元素上的init方法和destroy-method属性非常相似：v

