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

通过 `Environment` 解析的属性驻留在一个或多个“属性源”对象中，`@Configuration` 类可以使用@PropertySource注释将属性源贡献给环境对象:

