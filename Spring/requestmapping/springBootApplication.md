# @SpringBootApplication

## 定义

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {
    @AliasFor(
        annotation = EnableAutoConfiguration.class
    )
    Class<?>[] exclude() default {};

    @AliasFor(
        annotation = EnableAutoConfiguration.class
    )
    String[] excludeName() default {};

    @AliasFor(
        annotation = ComponentScan.class,
        attribute = "basePackages"
    )
    String[] scanBasePackages() default {};

    @AliasFor(
        annotation = ComponentScan.class,
        attribute = "basePackageClasses"
    )
    Class<?>[] scanBasePackageClasses() default {};
}
```

## 解析

用于指示一个配置类，该类声明一个或多个 @Bean 方法，并触发自动配置和组件扫描。

这是一个方便的注解，相当于声明`@Configuration`、`@EnableAutoConfiguration` 和 `@ComponentScan`。

```java
 @Configuration
 public class AppConfig {

     @Bean
     public MyBean myBean() {
         // instantiate, configure and return bean ...
     }
 }
```

### 引导 @Configuration 类

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

`@Configuration` 使用 `@Component` 元注解，因此 `@Configuration` 类是组件扫描的候选对象\(通常使用Spring XML的&lt;context:component-scan/&gt;元素\)，因此也可以像任何常规的@Component一样利用@Autowired/@Inject。特别是，如果一个构造函数存在，自动装配语义将透明地应用于该构造函数:



