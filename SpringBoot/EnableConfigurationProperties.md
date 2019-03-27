# @EnableConfigurationProperties

## 定义

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Import(EnableConfigurationPropertiesImportSelector.class)
public @interface EnableConfigurationProperties {

    /**
     * Convenient way to quickly register {@link ConfigurationProperties} annotated beans
     * with Spring. Standard Spring Beans will also be scanned regardless of this value.
     * @return {@link ConfigurationProperties} annotated beans to register
     */
    Class<?>[] value() default {};

}
```

## 解析

启用对 `@ConfigurationProperties` 注解 bean 的支持。`@ConfigurationProperties` bean 可以用标准的方式注册\(例如使用`@Bean` 方法\)，或者，为了方便起见，可以直接在这个注解上指定。

