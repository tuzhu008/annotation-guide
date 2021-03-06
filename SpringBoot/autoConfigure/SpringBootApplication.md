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
    /**
     * 排除特定的自动配置类，这样它们就永远不会被应用。
     * @return 要排除的类
     */
    @AliasFor(
        annotation = EnableAutoConfiguration.class
    )
    Class<?>[] exclude() default {};

    /**
     * 排除特定的自动配置类名，这样它们就永远不会被应用
     * @return 要排除的类
     * @since 1.3.0
     */
    @AliasFor(
        annotation = EnableAutoConfiguration.class
    )
    String[] excludeName() default {};


    /**
     * 指定用于扫描的带注解的组件的基本包。
     * 使用 {@link #scanBasePackageClasses} 为基于字符串的包名提供一种类型安全的替代方案。
     * @return 要扫描的基本包
     * @since 1.3.0
     */
    @AliasFor(
        annotation = ComponentScan.class,
        attribute = "basePackages"
    )
    String[] scanBasePackages() default {};

    /**
     * 类型安全的 {@link #scanBasePackages} 替代方案，用于指定用于扫描的带组件的包。
     * 将扫描指定的每个类的包。
     * 考虑在每个包中创建一个特殊的 no-op 标记类或接口，这个类或接口除了被这个属性引用之外没有其他用途。
     * @return 要扫描的基本包
     * @since 1.3.0
     */
    @AliasFor(
        annotation = ComponentScan.class,
        attribute = "basePackageClasses"
    )
    Class<?>[] scanBasePackageClasses() default {};
}
```

## 解析

我们使用这个注释来标记 Spring Boot 应用程序的主类:

```java
@SpringBootApplication
class VehicleFactoryApplication {

    public static void main(String[] args) {
        SpringApplication.run(VehicleFactoryApplication.class, args);
    }
}
```

这是一个方便的注解，相当于声明`@Configuration`、`@EnableAutoConfiguration` 和 `@ComponentScan`。

