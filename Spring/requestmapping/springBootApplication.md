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
	 * Base packages to scan for annotated components. Use {@link #scanBasePackageClasses}
	 * for a type-safe alternative to String-based package names.
	 * @return base packages to scan
	 * @since 1.3.0
	 */
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

