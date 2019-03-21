# @Lazy

## 定义

```java
@Target({ElementType.TYPE, ElementType.METHOD, ElementType.CONSTRUCTOR, ElementType.PARAMETER, ElementType.FIELD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Lazy {

    /**
     * Whether lazy initialization should occur.
     */
    boolean value() default true;

}
```

## 解析

当我们想要惰性地初始化 bean 时，我们使用 `@Lazy`。默认情况下，Spring 在应用程序上下文的启动/引导时贪婪地创建所有**单例 **bean。

但是，在某些情况下，我们需要在请求 bean 时创建它，而不是在应用程序启动时。

这个注解的行为取决于我们将它精确地放在哪里。我们可以这样写:

* `@Bean` 注解的 bean 工厂方法，以延迟方法调用\(创建 bean\)

* 一个 `@Configuration` 类和其包含的所有 `@Bean` 方法都将受到影响

* 一个 不是 `@Configuration` 类的`@Component` 类，这个 bean 将被延迟初始化

* `@Autowired` 构造函数、setter 或字段，以惰性地加载依赖项本身\(通过代理\)

该注解有一个名为 `value` 的参数，默认值为 `true`。这对重写默认行为是有用的。

例如，在全局设置为惰性时标记 bean 要被贪婪地加载，或者在一个标记为 `@Lazy` 的`@Configuration` 类中将特定的 `@Bean` 方法配置为热切加载:

```java
@Configuration
@Lazy
class VehicleFactoryConfig {

    @Bean
    @Lazy(false)
    Engine engine() {
        return new Engine();
    }
}
```

## 详述

默认情况下，Spring 在应用程序上下文的启动/引导时贪婪地创建所有**单例** bean。这背后的原因很简单:避免并立即检测所有可能的错误，而不是在运行时。

但是，在某些情况下，我们需要在请求 bean 时创建 bean，而不是在应用程序上下文启动时。

在这个快速教程中，我们将讨论 Spring 的 `@Lazy` 注解。

### 懒初始化

`@Lazy` 注解自 Spring 3.0 版本以来一直存在。有几种方法可以告诉 IoC 容器惰性地初始化 bean。

### _**@Configuration 类**_

当我们将 `@Lazy` 注解放在 `@Configuration` 类上时，它表明所有带有 `@Bean` 注释的方法都应该延迟加载。

这与基于 XML 的配置的 `default-lazy-init="true"` 属性相同。

让我们来看看这里:

```java
@Lazy
@Configuration
@ComponentScan(basePackages = "com.aho.lazy")
public class AppConfig {

    @Bean
    public Region getRegion(){
        return new Region();
    }

    @Bean
    public Country getCountry(){
        return new Country();
    }
}
```

现在让我们测试功能:

```java
@Test
public void givenLazyAnnotation_whenConfigClass_thenLazyAll() {

    AnnotationConfigApplicationContext ctx
     = new AnnotationConfigApplicationContext();
    ctx.register(AppConfig.class);
    ctx.refresh();
    ctx.getBean(Region.class);
    ctx.getBean(Country.class);
}
```

如我们所见，所有 的bean 只有在我们第一次请求时才会创建:

```
Bean factory for ...AnnotationConfigApplicationContext: 
...DefaultListableBeanFactory: [...];
// application context started
Region bean initialized
Country bean initialized
```

要将此应用于特定的 bean，让我们从类中删除 `@Lazy`。

然后将其添加到所需 bean 的配置中:

```java
@Bean
@Lazy(true)
public Region getRegion(){
    return new Region();
}
```

#### 用于 @Autowired



